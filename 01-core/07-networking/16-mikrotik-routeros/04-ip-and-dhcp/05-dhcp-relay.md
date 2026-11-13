# DHCP Relay in MikroTik RouterOS

A DHCP relay forwards DHCP messages between clients and a DHCP server located on a different network.

Normally, DHCP clients use broadcast messages to discover DHCP servers. Routers do not forward these broadcasts between different IP networks by default.

A DHCP relay solves this problem by receiving DHCP requests from clients and forwarding them to a remote DHCP server.

In MikroTik RouterOS, DHCP relay is useful when multiple VLANs, branch offices, or remote networks share a centralized DHCP server.

# 🗁 Understanding DHCP Relay

Consider a network with two different IP networks:

```text
                 Central DHCP Server
                    10.0.0.10
                         |
                         |
                  Core Network
                         |
                  MikroTik Router
                    DHCP Relay
                    /        \
                   /          \
             VLAN 10          VLAN 20
          192.168.10.0/24  192.168.20.0/24
                |                |
             Clients           Clients
```

The DHCP server is located in the central network, while clients are connected to separate VLANs.

Without DHCP relay, DHCP broadcast requests from VLAN 10 and VLAN 20 cannot normally reach the central DHCP server.

With DHCP relay:

* The relay receives DHCP broadcast requests from clients.
* It forwards the requests to the configured DHCP server.
* The DHCP server selects an address pool based on the client's network information.
* The server sends its response back to the relay.
* The relay forwards the response to the appropriate client.

# 🗁 DHCP Relay vs DHCP Server

Both DHCP relay and DHCP server help clients obtain network configuration, but their responsibilities are different.

| Feature | DHCP Server | DHCP Relay |
| --- | --- | --- |
| Main purpose | Assigns IP configuration | Forwards DHCP messages |
| Maintains address pools | Yes | No |
| Manages leases | Yes | No |
| Forwards DHCP requests | Not its primary role | Yes |
| Requires a remote DHCP server | No | Yes |
| Typical use | Local address allocation | Centralized address allocation |

A DHCP relay does not replace a DHCP server. It connects DHCP clients to an existing DHCP server across routed networks.

# 🗁 DHCP Relay Communication Process

DHCP relay commonly works with the DHCP DORA process:

* Discover
* Offer
* Request
* Acknowledgment

The relay forwards client messages between the local subnet and the remote DHCP server.

```text
       DHCP Client          DHCP Relay          DHCP Server
            |                    |                    |
            |---- Discover ----->|                    |
            |                    |---- Discover ----->|
            |                    |                    |
            |                    |<------ Offer ------|
            |<------ Offer ------|                    |
            |                    |                    |
            |---- Request ------>|                    |
            |                    |---- Request ------>|
            |                    |                    |
            |                    |<------ ACK --------|
            |<------- ACK -------|                    |
            |                    |                    |
            |       DHCP Lease Assigned               |
```

The relay forwards the DHCP messages using unicast communication toward the configured DHCP server.

# 🗁 Understanding the Relay Agent Address

The DHCP relay must identify the network from which a client request originated.

DHCP uses the `giaddr` field, known as the gateway IP address field, in the BOOTP message format.

The relay sets this field to identify the client-facing subnet.

The DHCP server uses this information to select the appropriate address pool and network configuration.

Example:

| Network | Relay Interface Address |
|---|---|
| VLAN 10 | 192.168.10.1 |
| VLAN 20 | 192.168.20.1 |

The central DHCP server can use these relay addresses to determine which subnet should receive an IP address.

The relay interface address must be reachable by the DHCP server, and the server must have a matching scope or subnet configuration.

# 🗁 Network Topology for DHCP Relay

Consider the following topology:

```text
                    Central Network
                    10.0.0.0/24
                         |
                  DHCP Server
                   10.0.0.10
                         |
                      ether1
                         |
                  MikroTik Router
                    DHCP Relay
                    /        \
                   /          \
              VLAN 10        VLAN 20
           192.168.10.1    192.168.20.1
                |               |
             Clients          Clients
```

Network configuration:

| Device | Interface | IP Address |
| --- | --- | --- |
| DHCP Server | Ethernet | 10.0.0.10/24 |
| MikroTik | ether1 | 10.0.0.1/24 |
| MikroTik | vlan10 | 192.168.10.1/24 |
| MikroTik | vlan20 | 192.168.20.1/24 |

The DHCP server will provide addresses for both VLAN networks.

Example address pools on the DHCP server:

| Network | DHCP Pool | Gateway |
| --- | --- | --- |
| VLAN 10 | 192.168.10.10-192.168.10.200 | 192.168.10.1 |
| VLAN 20 | 192.168.20.10-192.168.20.200 | 192.168.20.1 |

# 🗁 Prepare the Router Interfaces

Create the VLAN interfaces if they do not already exist.

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10

add name=vlan20 interface=bridge-LAN vlan-id=20
```

Assign IP addresses:

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10

add address=192.168.20.1/24 interface=vlan20
```

Configure the central network interface:

```routeros
/ip address
add address=10.0.0.1/24 interface=ether1
```

The bridge VLAN configuration and physical port membership must also be correct for the VLAN interfaces to receive client traffic.

# 🗁 Configure DHCP Relay

RouterOS manages DHCP relay through:

```routeros
/ip dhcp-relay
```

Create a DHCP relay for VLAN 10:

```routeros
/ip dhcp-relay
add name=relay-VLAN10 interface=vlan10 dhcp-server=10.0.0.10 disabled=no
```

Create another relay for VLAN 20:

```routeros
/ip dhcp-relay
add name=relay-VLAN20 interface=vlan20 dhcp-server=10.0.0.10 disabled=no
```

Explanation:

| Property | Description |
| --- | --- |
| `name` | DHCP relay instance name |
| `interface` | Client-facing interface receiving DHCP requests |
| `dhcp-server` | IP address of the remote DHCP server |
| `disabled=no` | Enables the relay |

Each client-facing subnet requires an appropriate relay configuration.

# 🗁 Configure Local Address

The `local-address` property specifies the source IP address used by the relay when communicating with the remote DHCP server.

It is useful when the router has multiple addresses or interfaces and the relay should use a specific source address.

Example:

```routeros
/ip dhcp-relay
add name=relay-VLAN10 interface=vlan10 dhcp-server=10.0.0.10 local-address=10.0.0.1 disabled=no
```

In this example:

* `interface=vlan10` identifies the client-facing interface.
* `dhcp-server=10.0.0.10` identifies the remote DHCP server.
* `local-address=10.0.0.1` specifies the relay's source address toward the server.

The local address must be valid for the router and suitable for communication with the DHCP server.

The relay still needs to identify the original client subnet correctly so that the DHCP server selects the appropriate address scope.

# 🗁 Configure Multiple DHCP Servers

RouterOS DHCP relay can forward requests to multiple DHCP server addresses.

Example:

```routeros
/ip dhcp-relay
add name=relay-VLAN10 interface=vlan10 dhcp-server=10.0.0.10,10.0.0.20 disabled=no
```

This configuration provides multiple DHCP server destinations.

The relay forwards DHCP requests to the configured servers. It does not itself provide DHCP server redundancy, lease synchronization, or address-pool coordination.

If multiple servers can independently assign addresses to the same subnet, their scopes and lease management must be designed to prevent conflicting assignments.

# 🗁 Verify DHCP Relay Configuration

Display configured DHCP relays:

```routeros
/ip dhcp-relay print
```

Display detailed information:

```routeros
/ip dhcp-relay print detail
```

Verify:

* The relay is enabled.
* The client-facing interface is correct.
* The DHCP server address is correct.
* The local address is appropriate, if configured.

Also inspect the router's IP addresses and routing table:

```routeros
/ip address print
/ip route print detail
```

# 🗁 Configure the Central DHCP Server

The central DHCP server must have a separate scope for every client subnet served through the relay.

For example, the DHCP server needs these network definitions:

| DHCP Network | Gateway | Address Pool |
| --- | --- | --- |
| 192.168.10.0/24 | 192.168.10.1 | 192.168.10.10-192.168.10.200 |
| 192.168.20.0/24 | 192.168.20.1 | 192.168.20.10-192.168.20.200 |

The server must recognize the relay information and select the correct scope.

If the DHCP server does not have a matching subnet configuration, it may not provide a valid lease.

The DHCP server also needs a return route to the relay and client networks where required by the network topology.

# 🗁 DHCP Relay and Routing

DHCP relay communication depends on correct Layer 3 connectivity.

The relay must be able to reach the DHCP server, and the DHCP server must be able to return traffic to the relay.

Example routing requirements:

```text
MikroTik Router:
10.0.0.0/24       Connected
192.168.10.0/24   Connected
192.168.20.0/24   Connected

Central DHCP Server:
192.168.10.0/24   Via 10.0.0.1
192.168.20.0/24   Via 10.0.0.1
```

If the DHCP server uses the MikroTik router as its default gateway, separate routes may not be necessary.

Otherwise, configure appropriate routes on the DHCP server's network.

# 🗁 DHCP Relay and Firewall

DHCP relay communication uses UDP.

Important ports:

| Port | Direction | Purpose |
| --- | --- | --- |
| UDP 67 | Client/server side | DHCP server communication |
| UDP 68 | Client side | DHCP client communication |

Firewall rules must allow DHCP traffic between the client-facing interfaces, relay, and remote DHCP server.

The exact rules depend on the firewall design and packet flow.

For a basic routed design, verify that:

* Client DHCP broadcasts can reach the relay interface.
* Relay traffic can reach the DHCP server.
* DHCP server responses can return to the relay.
* The relay can forward responses to clients.

Do not expose the router's management services merely to make DHCP relay work.

# 🗁 DHCP Relay vs DHCP Server on the Same Interface

A DHCP server and DHCP relay perform different functions on a network interface.

A DHCP server directly assigns addresses to clients.

A DHCP relay forwards client requests to a remote DHCP server.

Avoid configuring both services on the same client-facing network without a deliberate design. Multiple DHCP responders can produce unpredictable address assignments and configuration conflicts.

For a centralized DHCP design, use the relay on the client-facing subnet and maintain the address pools on the central DHCP server.

# 🗁 DHCP Relay Troubleshooting

**Clients do not receive IP addresses**

Possible causes:

* DHCP relay is disabled.
* Incorrect relay interface.
* Incorrect DHCP server IP address.
* Remote DHCP server is unreachable.
* Missing DHCP scope.
* Firewall blocks DHCP traffic.
* Incorrect VLAN configuration.

Check:

```routeros
/ip dhcp-relay print detail
/ip address print
/ip route print detail
/ping 10.0.0.10
```

**DHCP server receives requests but does not assign addresses**

Possible causes:

* Missing subnet scope.
* Incorrect relay address information.
* Exhausted DHCP pool.
* DHCP server policy restrictions.
* Incorrect gateway or subnet configuration.

Inspect the DHCP server logs and lease database.

**DHCP relay cannot reach the remote server**

Possible causes:

* Missing route.
* Incorrect local address.
* Firewall restrictions.
* Remote server unavailable.
* Incorrect IP addressing.

Check:

```routeros
/ip route print detail
/ip address print
/ping 10.0.0.10
```

**Clients receive addresses from the wrong subnet**

Possible causes:

* Incorrect relay interface.
* Incorrect VLAN assignment.
* Incorrect subnet scope on the DHCP server.
* Multiple DHCP servers responding.
* Incorrect relay agent subnet information.

Verify the relay configuration and the DHCP server's subnet selection behavior.

**Only one VLAN receives DHCP addresses**

Possible causes:

* Missing relay instance for another VLAN.
* Incorrect VLAN interface configuration.
* Missing DHCP scope for the second VLAN.
* Firewall restrictions affecting one subnet.

Check:

```routeros
/ip dhcp-relay print detail
/interface vlan print detail
/ip address print
```

# 🗁 DHCP Relay Packet Inspection

Packet inspection can help identify where DHCP communication fails.

RouterOS provides packet sniffing tools.

Example:

```routeros
/tool sniffer quick interface=vlan10 ip-protocol=udp port=67,68
```

Inspect DHCP traffic on the server-facing interface:

```routeros
/tool sniffer quick interface=ether1 ip-protocol=udp port=67,68
```

Look for DHCP Discover, Offer, Request, and Acknowledgment messages.

A packet capture can help determine whether:

* Client requests reach the relay.
* Relay messages leave toward the server.
* Server responses return.
* Responses reach the client-facing network.

# 🗁 Practical DHCP Relay Lab

**Lab topology**

```text
                  Central DHCP Server
                    10.0.0.10/24
                         |
                      ether1
                         |
                  MikroTik Router
                    DHCP Relay
                    /        \
                   /          \
              VLAN 10        VLAN 20
           192.168.10.1    192.168.20.1
                |               |
              Client          Client
```

**Network requirements**

| Parameter | Value |
| --- | --- |
| DHCP server | 10.0.0.10 |
| MikroTik server-side IP | 10.0.0.1/24 |
| VLAN 10 gateway | 192.168.10.1 |
| VLAN 20 gateway | 192.168.20.1 |
| VLAN 10 network | 192.168.10.0/24 |
| VLAN 20 network | 192.168.20.0/24 |

**Configure the server-facing interface**

```routeros
/ip address
add address=10.0.0.1/24 interface=ether1
```

**Create VLAN interfaces**

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10

add name=vlan20 interface=bridge-LAN vlan-id=20
```

**Assign VLAN gateway addresses**

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10

add address=192.168.20.1/24 interface=vlan20
```

**Configure DHCP relay**

```routeros
/ip dhcp-relay
add name=relay-VLAN10 interface=vlan10 dhcp-server=10.0.0.10 disabled=no

add name=relay-VLAN20 interface=vlan20 dhcp-server=10.0.0.10 disabled=no
```

**Configure the central DHCP server**

Create two DHCP scopes:

```text
Scope 1:
Network: 192.168.10.0/24
Gateway: 192.168.10.1
Pool:    192.168.10.10-192.168.10.200

Scope 2:
Network: 192.168.20.0/24
Gateway: 192.168.20.1
Pool:    192.168.20.10-192.168.20.200
```

Configure the server's return routing and firewall permissions.

**Verify relay configuration**

```routeros
/ip dhcp-relay print detail
```

**Test server connectivity**

```routeros
/ping 10.0.0.10
```

**Connect DHCP clients**

Connect one client to VLAN 10 and another to VLAN 20.

Configure both clients to obtain IP addresses automatically.

Expected results:

| Client | Expected Address Range | Default Gateway |
| --- | --- | --- |
| VLAN 10 client | 192.168.10.10-192.168.10.200 | 192.168.10.1 |
| VLAN 20 client | 192.168.20.10-192.168.20.200 | 192.168.20.1 |

The central DHCP server should record leases for both networks.

# 🗁 Best Practices

* Use centralized DHCP when multiple networks need consistent address management.
* Configure one relay instance for each client-facing subnet.
* Ensure every client subnet has a matching DHCP scope.
* Verify routing between the relay and DHCP server.
* Configure firewall rules for required DHCP traffic.
* Avoid overlapping DHCP address pools.
* Monitor DHCP server availability and lease utilization.
* Document relay interfaces, server addresses, and subnet mappings.
* Use packet inspection when troubleshooting DHCP communication.
* Avoid running competing DHCP services on the same client-facing network.

# 🗁 Official Documentation

* MikroTik DHCP: <https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP>
* MikroTik IP Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing>
