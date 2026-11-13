# Destination NAT

Destination Network Address Translation (Destination NAT or DNAT) modifies the destination IP address or destination port of packets as they pass through a router.

In MikroTik RouterOS, Destination NAT is commonly used to publish internal services to external networks, redirect traffic, and forward incoming connections to servers inside a private LAN.

A typical example is allowing internet users to access an internal web server through the router's public IP address.

# 🗁 What Is Destination NAT?

Destination NAT changes the destination address of a packet.

Consider a router with a public IP address and an internal web server.

**Before Destination NAT:**

```text
Source IP:      198.51.100.25
Destination IP: 203.0.113.10
Destination Port: 80
```

The packet arrives at the router's public IP address.

**After Destination NAT:**

```text
Source IP:      198.51.100.25
Destination IP: 192.168.10.10
Destination Port: 80
```

The router changes the destination address to the internal server's IP address and forwards the packet.

Connection tracking maintains the translation so that response packets can return through the router with the appropriate address translation.

# 🗁 How Destination NAT Works

Consider the following network:

```text
                 Internet
                     |
                     |
              Public IP Address
               203.0.113.10
                     |
              MikroTik Router
                     |
              LAN: 192.168.10.0/24
                     |
                 Web Server
                192.168.10.10
                   Port 80
```

When an external client accesses `203.0.113.10:80`:

* The client sends a connection request to the router's public IP.
* The router receives the packet on its WAN interface.
* The Destination NAT rule matches the incoming connection.
* RouterOS changes the destination IP to `192.168.10.10`.
* The router forwards the packet to the internal web server.
* The server processes the request and sends a response.
* Connection tracking ensures the response follows the reverse translation.

# 🗁 Destination NAT vs Source NAT

| Feature | Destination NAT | Source NAT |
| --- | --- | --- |
| Main purpose | Changes destination address | Changes source address |
| Common direction | Incoming connections | Outgoing connections |
| Common use | Publishing internal services | Providing internet access |
| RouterOS chain | `dstnat` | `srcnat` |
| Common actions | `dst-nat`, `redirect` | `src-nat`, `masquerade` |

Destination NAT is commonly used for port forwarding, while Source NAT is commonly used for outbound internet connectivity.

# 🗁 Configuring Destination NAT

Assume the following configuration:

| Component | Configuration |
| --- | --- |
| WAN interface | `ether1` |
| LAN interface | `ether2` |
| Router public IP | `203.0.113.10` |
| Web server IP | `192.168.10.10` |
| Web server port | `80` |
| Protocol | TCP |

Create a Destination NAT rule:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Forward HTTP to Web Server"
```

Explanation:

* `chain=dstnat`: Processes packets in the Destination NAT chain.
* `in-interface=ether1`: Matches packets arriving through the WAN interface.
* `protocol=tcp`: Matches TCP traffic.
* `dst-port=80`: Matches packets addressed to destination port 80.
* `action=dst-nat`: Changes the destination address.
* `to-addresses=192.168.10.10`: Specifies the internal server address.
* `to-ports=80`: Specifies the internal destination port.

External clients can now access the web server through the router's public IP address, provided that upstream routing and firewall policies allow the connection.

# 🗁 Forwarding Multiple Ports to One Server

A server may provide multiple services using different ports.

For example:

| Service | External Port | Internal Port |
| --- | ---: | ---: |
| HTTP | 80 | 80 |
| HTTPS | 443 | 443 |
| SSH | 2222 | 22 |

Create separate rules for each service:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="HTTP"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=443 action=dst-nat to-addresses=192.168.10.10 to-ports=443 comment="HTTPS"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=2222 action=dst-nat to-addresses=192.168.10.10 to-ports=22 comment="SSH"
```

The external SSH port is `2222`, while the internal server listens on port `22`.

# 🗁 Forwarding to a Different Internal Port

Destination NAT can translate both the destination IP address and destination port.

For example, forward external TCP port `8080` to internal TCP port `80`:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="External 8080 to Internal 80"
```

The connection flow becomes:

```text
External Client
      |
      | 203.0.113.10:8080
      |
      v
MikroTik Router
      |
      | Destination NAT
      | 192.168.10.10:80
      |
      v
Internal Web Server
```

This is useful when the internal service listens on a different port from the one exposed externally.

# 🗁 Forwarding to Multiple Internal Servers

A router can publish services hosted on different internal servers.

Example:

| External Port | Internal Server | Internal Port |
| --- | --- | ---: |
| 80 | 192.168.10.10 | 80 |
| 443 | 192.168.10.10 | 443 |
| 8080 | 192.168.10.20 | 80 |
| 8443 | 192.168.10.20 | 443 |

Configuration:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Web Server 1 HTTP"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=443 action=dst-nat to-addresses=192.168.10.10 to-ports=443 comment="Web Server 1 HTTPS"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.20 to-ports=80 comment="Web Server 2 HTTP"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8443 action=dst-nat to-addresses=192.168.10.20 to-ports=443 comment="Web Server 2 HTTPS"
```

Different external ports allow multiple internal servers to be accessed through one public IP address.

# 🗁 Using a Specific Public IP Address

If the router has multiple public IP addresses, you can match a specific destination address.

Example:

```routeros
/ip/firewall/nat
add chain=dstnat dst-address=203.0.113.10 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Web Server Public IP 1"
```

This rule matches connections addressed specifically to `203.0.113.10`.

The public address must be assigned or routed to the router so that incoming packets can reach it.

# 🗁 Using the Redirect Action

The `redirect` action is used to redirect traffic to a service running on the MikroTik router itself.

For example, redirect TCP port `8080` to the router's local TCP port `80`:

```routeros
/ip/firewall/nat
add chain=dstnat protocol=tcp dst-port=8080 action=redirect to-ports=80 comment="Redirect to Router Web Service"
```

Unlike `dst-nat`, which normally forwards traffic to another IP address, `redirect` changes the destination to the router itself.

This action is useful for redirecting traffic to local router services or implementing certain traffic interception policies.

# 🗁 Destination NAT and Firewall Filter Rules

A Destination NAT rule does not automatically guarantee that forwarded traffic will be allowed by the firewall.

RouterOS processes forwarded packets through the firewall filter's `forward` chain after destination translation.

A typical firewall rule allowing the published web server is:

```routeros
/ip/firewall/filter
add chain=forward in-interface=ether1 dst-address=192.168.10.10 protocol=tcp dst-port=80 connection-nat-state=dstnat action=accept comment="Allow Forwarded HTTP"
```

The `connection-nat-state=dstnat` matcher identifies connections that have been processed by Destination NAT.

A typical firewall policy also includes rules to accept established and related connections:

```routeros
/ip/firewall/filter
add chain=forward connection-state=established,related action=accept comment="Allow Established and Related"

add chain=forward connection-state=invalid action=drop comment="Drop Invalid Connections"
```

Place appropriate allow rules before general drop rules.

The complete firewall policy must account for the router's existing rules and intended network security design.

# 🗁 Accessing an Internal Server from the LAN

Suppose an internal client accesses the router's public IP to reach an internal server:

```text
LAN Client:       192.168.10.20
Router Public IP: 203.0.113.10
Web Server:       192.168.10.10
```

This is commonly called hairpin NAT or NAT loopback.

A Destination NAT rule that matches only `in-interface=ether1` will not match this LAN-originated request because the packet arrives through the LAN interface.

A hairpin NAT rule can match the internal request:

```routeros
/ip/firewall/nat
add chain=dstnat src-address=192.168.10.0/24 dst-address=203.0.113.10 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Hairpin Destination NAT"
```

A corresponding Source NAT rule can ensure that the internal server sends its response back through the router:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=192.168.10.0/24 dst-address=192.168.10.10 protocol=tcp dst-port=80 action=masquerade comment="Hairpin Source NAT"
```

The resulting connection flow is:

```text
LAN Client
192.168.10.20
      |
      | Requests 203.0.113.10:80
      |
      v
MikroTik Router
      |
      | Destination NAT
      | 203.0.113.10:80 -> 192.168.10.10:80
      |
      | Source NAT
      | 192.168.10.20 -> Router LAN IP
      |
      v
Web Server
192.168.10.10:80
```

Hairpin NAT is useful when internal and external clients need to access the same service using the same public hostname or IP address.

# 🗁 NAT Rule Order and Connection Tracking

RouterOS processes NAT rules in order when a new connection is established.

The first matching NAT rule determines the translation for that connection.

Example:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Specific Web Server"

add chain=dstnat in-interface=ether1 protocol=tcp action=dst-nat to-addresses=192.168.10.20 comment="General TCP Forwarding"
```

The specific HTTP rule should appear before the broader rule.

NAT decisions are associated with connection-tracking entries. Existing connections continue using their established translation even after NAT rules are modified.

When testing a changed NAT rule, create a new connection or remove the relevant connection-tracking entry.

# 🗁 Viewing Destination NAT Rules

Display all NAT rules:

```routeros
/ip/firewall/nat/print
```

Display detailed NAT configuration:

```routeros
/ip/firewall/nat/print detail
```

Display NAT rule counters:

```routeros
/ip/firewall/nat/print stats
```

Counters help determine whether incoming traffic is matching the expected rule.

# 🗁 Monitoring Connection Tracking

Display active tracked connections:

```routeros
/ip/firewall/connection/print
```

Filter connections by destination address:

```routeros
/ip/firewall/connection/print where dst-address~"192.168.10.10"
```

Inspect connection information to verify whether traffic is reaching the expected internal server.

# 🗁 Troubleshooting Destination NAT

If an external client cannot access a forwarded service, check the following areas.

**Verify the public IP address**

```routeros
/ip/address/print
```

Make sure the public IP is correctly configured or routed to the router.

If the router is behind another NAT device, that upstream device must also forward the required port to the MikroTik router.

**Verify the Destination NAT rule**

```routeros
/ip/firewall/nat/print detail
```

Check:

* Correct incoming interface.
* Correct destination IP address.
* Correct protocol.
* Correct external destination port.
* Correct internal server address.
* Correct internal destination port.

**Check NAT counters**

```routeros
/ip/firewall/nat/print stats
```

If the packet counter does not increase, the rule may not be matching incoming traffic.

**Verify the firewall forward chain**

```routeros
/ip/firewall/filter/print
```

Check whether a firewall rule is dropping the forwarded connection.

**Verify the internal server**

Make sure the server is online, has the correct IP address, and is listening on the expected port.

**Check the server's default gateway**

The internal server should normally use the MikroTik router's LAN IP as its default gateway, or have another valid return route through the router.

**Check upstream filtering**

An ISP or upstream firewall may block incoming connections to certain ports.

# 🗁 Important Considerations

* Destination NAT is commonly used to publish internal services.
* Port forwarding is a practical application of Destination NAT.
* The internal server must have a valid return path through the router.
* A Destination NAT rule does not replace firewall filter rules.
* Use `in-interface` or `in-interface-list` to restrict which incoming traffic can match a forwarding rule.
* Use specific destination addresses and ports instead of unnecessarily broad NAT rules.
* Hairpin NAT may be required when LAN clients access internal services through the router's public address.
* Connection tracking maintains NAT translations for active connections.
* Avoid exposing administrative services directly to the public internet unless there is a clear operational requirement and appropriate access controls.

# 🗁 Practical Example

Configure a MikroTik router to publish an internal web server to the internet.

**Network topology:**

```text
                 Internet
                     |
                     |
              Public IP Address
                203.0.113.10
                     |
                  ether1
                     |
              MikroTik Router
                     |
                  ether2
                     |
              LAN: 192.168.10.0/24
                     |
                 Web Server
                192.168.10.10
                   Port 80
```

**Configure the Destination NAT rule:**

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Publish Web Server"
```

**Allow the forwarded connection in the firewall:**

```routeros
/ip/firewall/filter
add chain=forward in-interface=ether1 dst-address=192.168.10.10 protocol=tcp dst-port=80 connection-nat-state=dstnat action=accept comment="Allow Web Server Access"
```

Ensure this allow rule is placed before any general rule that drops the forwarded connection.

**Verify the NAT rule:**

```routeros
/ip/firewall/nat/print stats
```

**Verify connection tracking:**

```routeros
/ip/firewall/connection/print
```

External clients can access the web server using:

```text
http://203.0.113.10
```

The example assumes that the public IP is reachable, the server is running, and the upstream network permits incoming TCP port 80 traffic.

# 🗁 Summary

Destination NAT changes the destination IP address or port of packets passing through a router.

In MikroTik RouterOS, the `dst-nat` action is commonly used to forward incoming connections to internal servers, while `redirect` sends matching traffic to a service running on the router itself.

Understanding Destination NAT is essential for configuring port forwarding, publishing internal services, hairpin NAT, and other network access scenarios.
