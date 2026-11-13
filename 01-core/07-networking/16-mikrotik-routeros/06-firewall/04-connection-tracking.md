# Connection Tracking

Connection Tracking (Conntrack) is a RouterOS mechanism that monitors network connections and maintains information about packets belonging to those connections.

Instead of treating every packet as an independent unit, connection tracking identifies relationships between packets and determines their connection state.

This information allows firewall rules to make decisions based on whether traffic belongs to a new, established, related, invalid, or untracked connection.

Connection tracking is essential for many RouterOS features, including:

* Stateful firewall filtering.
* Source NAT and destination NAT.
* Connection-state matching.
* Connection limits and rate-based policies.
* FastTrack.
* Connection marks and advanced traffic management.

Connection tracking is managed under:

```routeros
/ip/firewall/connection
```

# 🗁 What Is a Network Connection?

A network connection represents communication between two endpoints.

For example, when a LAN client opens an HTTPS website:

```text
Client IP:       192.168.10.10
Client Port:     51520

Server IP:       93.184.216.34
Server Port:     443

Protocol:        TCP
```

The client initiates communication with the server. The server sends response packets back to the client.

Connection tracking observes both directions and associates the packets with the same connection.

```text
             LAN Client
          192.168.10.10
                 |
                 | New connection
                 |
                 v
          +--------------+
          |   MikroTik   |
          |              |
          | Connection   |
          |  Tracking    |
          +--------------+
                 |
                 | Forwarded traffic
                 |
                 v
           HTTPS Server
          93.184.216.34
```

The router maintains connection information so that firewall rules can distinguish between the initial connection and its subsequent packets.

# 🗁 Connection Tracking States

RouterOS identifies packets using connection states.

| State | Description |
| --- | --- |
| `new` | A packet that starts a new connection or belongs to a connection that has not yet seen traffic in both directions |
| `established` | A packet belonging to a connection that has seen traffic in both directions |
| `related` | A packet associated with an existing connection but belonging to a separate related connection |
| `invalid` | A packet that cannot be associated with a valid connection |
| `untracked` | A packet that has been deliberately excluded from connection tracking |

These states are commonly used in firewall filter rules.

# 🗁 New Connection State

The `new` state identifies packets that initiate a connection or belong to a connection that has not yet been established in both directions.

For example, a LAN client sends the first TCP SYN packet to an HTTPS server.

```text
Client                  Server
  |                        |
  |------ TCP SYN -------->|
  |                        |
```

The initial packet is classified as `new`.

Example firewall rule:

```routeros
/ip/firewall/filter
add chain=forward connection-state=new action=accept comment="Allow new forwarded connections"
```

This rule permits new forwarded connections. It should be scoped to the intended interfaces, addresses, and services in a real firewall policy.

# 🗁 Established Connection State

The `established` state identifies packets belonging to a connection that has already exchanged traffic in both directions.

For example, after the TCP handshake:

```text
Client                  Server
  |                        |
  |------ TCP SYN -------->|
  |<----- TCP SYN-ACK -----|
  |------ TCP ACK -------->|
  |                        |
  |<---- HTTPS Data ------>|
```

RouterOS identifies subsequent packets as belonging to an established connection.

Example:

```routeros
/ip/firewall/filter
add chain=forward connection-state=established action=accept comment="Allow established connections"
```

Established traffic is commonly permitted early in a stateful firewall policy.

# 🗁 Related Connection State

The `related` state identifies traffic associated with an existing connection but represented by a separate connection.

Examples may include:

* Certain ICMP error messages associated with an existing connection.
* Auxiliary connections created by protocols that negotiate additional data channels.
* Other traffic recognized by connection tracking helpers.

Example:

```routeros
/ip/firewall/filter
add chain=forward connection-state=related action=accept comment="Allow related connections"
```

Related traffic is commonly accepted together with established traffic when the firewall policy requires it.

# 🗁 Invalid Connection State

The `invalid` state identifies packets that connection tracking cannot associate with a valid connection.

Possible causes include:

* Malformed or unexpected packets.
* Packets that do not match the expected connection state.
* Asymmetric routing or missing connection information.
* Inconsistent packet sequences.
* Certain unusual network conditions.

A common firewall rule drops invalid packets:

```routeros
/ip/firewall/filter
add chain=input connection-state=invalid action=drop comment="Drop invalid input"
add chain=forward connection-state=invalid action=drop comment="Drop invalid forwarding"
```

Dropping invalid traffic helps prevent unexpected packets from being accepted by broader firewall rules.

# 🗁 Untracked Connection State

The `untracked` state identifies packets that are intentionally excluded from connection tracking.

This is commonly achieved through the firewall `raw` table using the `notrack` action.

Example:

```routeros
/ip/firewall/raw
add chain=prerouting src-address=192.0.2.0/24 action=notrack comment="Exclude selected traffic from tracking"
```

A corresponding filter rule can match untracked traffic:

```routeros
/ip/firewall/filter
add chain=forward connection-state=untracked action=accept comment="Allow selected untracked traffic"
```

Untracked traffic does not receive normal connection-tracking services such as stateful connection matching and NAT processing. Use `notrack` only when the network design supports it.

# 🗁 Stateful Firewall Concept

A stateful firewall evaluates packets using connection information.

For example, when a LAN client initiates an internet connection:

* The first packet is classified as `new`.
* Once traffic has been observed in both directions, subsequent packets are classified as `established`.
* Related packets are classified as `related` when connection tracking recognizes their association.
* Unexpected packets may be classified as `invalid`.

This makes it possible to allow outgoing connections and their responses without creating separate broad allow rules for every return packet.

A common starting pattern is:

```routeros
/ip/firewall/filter
add chain=input connection-state=established,related action=accept comment="Allow established and related input"
add chain=input connection-state=invalid action=drop comment="Drop invalid input"

add chain=forward connection-state=established,related action=accept comment="Allow established and related forwarding"
add chain=forward connection-state=invalid action=drop comment="Drop invalid forwarding"
```

These rules are not a complete firewall policy. New connections still require explicit permissions based on the network's security requirements.

# 🗁 Connection Tracking Across Firewall Chains

Connection tracking works alongside the input, forward, and output chains.

| Chain | Example connection |
| --- | --- |
| `input` | LAN client connects to router SSH |
| `forward` | LAN client connects to an internet server |
| `output` | Router initiates a DNS query |

The connection state describes the packet's relationship to a tracked connection. The firewall chain describes the packet's path through the router.

For example, a LAN client accessing an external HTTPS server uses the forward chain for both outgoing and returning packets.

# 🗁 Connection Tracking and NAT

Connection tracking is closely integrated with NAT.

RouterOS uses connection information to maintain address and port translations across packets belonging to the same connection.

**Source NAT example**

A LAN client accesses an external server:

```text
Before NAT:

192.168.10.10:51520
        |
        v
93.184.216.34:443
```

The router translates the source address:

```text
After NAT:

203.0.113.10:51520
        |
        v
93.184.216.34:443
```

Connection tracking remembers the translation so that response packets can be translated back to the original LAN client.

**Destination NAT example**

An external client connects to a public address:

```text
Before NAT:

198.51.100.20
        |
        v
203.0.113.10:443
```

The router redirects the connection to an internal server:

```text
After NAT:

198.51.100.20
        |
        v
192.168.10.100:443
```

Connection tracking maintains the destination translation for the connection.

# 🗁 Connection NAT States

RouterOS provides the `connection-nat-state` property for matching connections according to their NAT status.

Common values include:

| Value | Description |
| --- | --- |
| `srcnat` | Connection has source NAT applied |
| `dstnat` | Connection has destination NAT applied |

Example:

```routeros
/ip/firewall/filter
add chain=forward connection-nat-state=dstnat action=accept comment="Allow destination-NAT connections"
```

This can be useful when allowing port-forwarded traffic through the forward chain.

For production policies, scope the rule to the intended interfaces, protocols, destination addresses, and ports rather than allowing every destination-NAT connection.

# 🗁 Connection Tracking Settings

Connection tracking settings are available under:

```routeros
/ip/firewall/connection/tracking
```

Display the current configuration:

```routeros
/ip/firewall/connection/tracking/print
```

Connection tracking can be enabled or disabled:

```routeros
/ip/firewall/connection/tracking
set enabled=yes
```

Display connection tracking statistics and settings:

```routeros
/ip/firewall/connection/tracking/print
```

Disabling connection tracking affects features that depend on it, including NAT and stateful firewall filtering. It should not be disabled on a normal production router without understanding the consequences.

# 🗁 Connection Tracking Table

RouterOS maintains a connection table containing information about tracked connections.

Display all tracked connections:

```routeros
/ip/firewall/connection/print
```

Display detailed connection information:

```routeros
/ip/firewall/connection/print detail
```

Display TCP connections:

```routeros
/ip/firewall/connection/print where protocol=tcp
```

Display UDP connections:

```routeros
/ip/firewall/connection/print where protocol=udp
```

Display connections involving a specific address:

```routeros
/ip/firewall/connection/print where src-address~"192.168.10.10"
```

Useful connection-table fields include:

| Field | Description |
| --- | --- |
| `protocol` | Transport protocol |
| `src-address` | Original source address and port |
| `dst-address` | Original destination address and port |
| `reply-src-address` | Source address and port in the reply direction |
| `reply-dst-address` | Destination address and port in the reply direction |
| `tcp-state` | TCP connection state, when applicable |
| `timeout` | Remaining connection timeout |
| `orig-packets` | Packets observed in the original direction |
| `repl-packets` | Packets observed in the reply direction |
| `orig-bytes` | Bytes observed in the original direction |
| `repl-bytes` | Bytes observed in the reply direction |

The exact information displayed depends on the connection and RouterOS version.

# 🗁 Removing Connection Entries

Remove a specific connection by its internal item identifier:

```routeros
/ip/firewall/connection/print
/ip/firewall/connection/remove numbers=0
```

The item number is an example. Inspect the connection table and identify the correct entry before removing it.

Remove connections associated with a specific source address:

```routeros
/ip/firewall/connection/remove [find where src-address~"192.168.10.10"]
```

Removing a connection entry interrupts the router's tracking of that connection. Existing traffic may be treated as a new connection afterward, and NAT mappings associated with it may be lost.

Avoid clearing the entire connection table on a production router unless there is a specific operational reason.

# 🗁 Connection Tracking and FastTrack

FastTrack is a RouterOS feature that accelerates eligible established connections.

It relies on connection tracking to identify connections that can be FastTracked.

Example:

```routeros
/ip/firewall/filter
add chain=forward connection-state=established,related action=fasttrack-connection hw-offload=yes comment="FastTrack eligible connections"
add chain=forward connection-state=established,related action=accept comment="Allow established and related"
```

FastTrack can reduce CPU usage, but it bypasses parts of the normal packet-processing path.

This may affect:

* Queueing and traffic shaping.
* Firewall processing and accounting.
* Traffic inspection.
* Policy-based routing.
* Some IPsec configurations.

Review feature compatibility before enabling FastTrack on a router that uses advanced traffic processing.

# 🗁 Connection Tracking and Timeouts

Connection tracking maintains timeout values for different connection types and states.

A timeout determines how long a connection entry remains in the connection table without receiving relevant traffic.

Examples include:

* TCP connection timeouts.
* UDP connection timeouts.
* ICMP connection timeouts.
* Other protocol-specific timeouts.

Display connection tracking settings:

```routeros
/ip/firewall/connection/tracking/print
```

Timeout tuning should be based on actual network requirements. Arbitrarily reducing timeouts can interrupt legitimate long-lived or idle connections.

# 🗁 Connection Tracking and Performance

Connection tracking requires memory and processing resources because RouterOS maintains connection information.

Performance considerations include:

* Number of concurrent connections.
* Connection creation rate.
* Firewall rule complexity.
* NAT processing requirements.
* Router CPU and available memory.
* FastTrack eligibility.
* Traffic volume and packet rate.

Monitor router resources:

```routeros
/system/resource/print
```

Inspect connection entries:

```routeros
/ip/firewall/connection/print count-only
```

A high connection count does not automatically indicate a problem. It must be evaluated against the router's hardware capacity and normal traffic patterns.

# 🗁 Troubleshooting Connection Tracking

**NAT is not working**

Check whether connection tracking is enabled:

```routeros
/ip/firewall/connection/tracking/print
```

Then inspect NAT rules and tracked connections:

```routeros
/ip/firewall/nat/print
/ip/firewall/connection/print
```

**A connection is classified as invalid**

Inspect the connection path, routing symmetry, packet sequence, and any upstream network devices that may alter traffic.

**Connections are not appearing in the connection table**

Check whether the traffic is excluded using `notrack` or whether connection tracking is disabled.

```routeros
/ip/firewall/raw/print
/ip/firewall/connection/tracking/print
```

**Existing connections stop working after a firewall change**

Inspect the connection table and filter rules. Changes to NAT or firewall policies may affect how new and existing connections are processed.

**Connection table grows unexpectedly**

Inspect the connection count and identify traffic patterns that may be creating excessive connections.

```routeros
/ip/firewall/connection/print count-only
/ip/firewall/connection/print
```

**FastTrack causes unexpected behavior**

Temporarily test the policy without FastTrack in a controlled environment and verify whether the affected feature requires normal packet processing.

# 🗁 Practical Lab

**Objective:** Observe connection tracking states and understand how RouterOS maintains connection information.

Network assumptions:

| Component | Configuration |
| --- | --- |
| Router LAN interface | `bridge-LAN` |
| Router LAN IP | `192.168.10.1` |
| LAN subnet | `192.168.10.0/24` |
| Test client | `192.168.10.10` |
| External destination | `1.1.1.1` |

**Verify connection tracking**

```routeros
/ip/firewall/connection/tracking/print
```

Ensure connection tracking is enabled.

**Display the connection table**

```routeros
/ip/firewall/connection/print
```

**Generate traffic from a LAN client**

From the client, access an external website or generate a ping to an external destination.

**Inspect tracked connections**

```routeros
/ip/firewall/connection/print
```

Identify the connection involving the test client.

**Inspect connection statistics**

```routeros
/ip/firewall/connection/print detail
```

Observe:

* Source and destination addresses.
* Protocol.
* Original and reply packet counters.
* Connection timeout.
* TCP state, where applicable.
* NAT-related information.

**Experiment with firewall states**

Inspect the firewall counters:

```routeros
/ip/firewall/filter/print stats
```

Generate new traffic and observe which rules receive packets.

The goal is to understand how connection tracking identifies new and established traffic and how firewall rules use that information.

# 🗁 Best Practices

* Keep connection tracking enabled when using NAT and stateful firewall policies.
* Use connection states to build clear firewall policies.
* Drop invalid packets where appropriate.
* Allow established and related traffic according to the security policy.
* Avoid unnecessary connection-table clearing.
* Monitor connection counts on routers handling large numbers of clients.
* Tune timeouts only when justified by network requirements.
* Understand FastTrack compatibility before enabling it.
* Use `notrack` selectively and only when connection tracking is not required for the affected traffic.
* Inspect connection entries when troubleshooting NAT and stateful filtering.

# 🗁 Knowledge Check

* What is connection tracking?
* Why does RouterOS maintain a connection table?
* What is the difference between `new` and `established`?
* What does the `related` state represent?
* What can cause a packet to be classified as `invalid`?
* What does the `untracked` state mean?
* How does connection tracking support NAT?
* What is the purpose of `connection-nat-state`?
* Why does FastTrack depend on connection tracking?
* What can happen when connection tracking is disabled?

# 🗁 References

* MikroTik RouterOS Documentation — Connection Tracking: <https://help.mikrotik.com/docs/spaces/ROS/pages/130220087/Connection+tracking>
* MikroTik RouterOS Documentation — Firewall: <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall>
* MikroTik RouterOS Documentation — Packet Flow: <https://help.mikrotik.com/docs/spaces/ROS/pages/328227/Packet+Flow>
