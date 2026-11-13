# Policy-Based Routing (PBR)

Policy-Based Routing (PBR) is a routing technique that allows network administrators to make forwarding decisions based on policies rather than relying exclusively on the destination IP address.

Traditional routing uses the destination IP address to select the best matching route from the routing table.

PBR extends this behavior by allowing traffic to be routed according to additional criteria, such as:

* Source IP address.
* Destination IP address.
* Incoming interface.
* Protocol.
* Source or destination port.
* Packet marks.
* Other platform-supported traffic attributes.

PBR is useful in multi-WAN environments, ISP networks, traffic engineering, network segmentation, and environments where different traffic types must use different network paths.

# 🗁 Traditional Routing vs Policy-Based Routing

### 🖆 Traditional Routing

Traditional IP routing primarily selects a route based on the destination IP address.

The router performs a Longest Prefix Match lookup in its routing table and forwards the packet using the selected route.

Example:

```text
Source:      192.168.1.10
Destination: 8.8.8.8

Routing Table:
0.0.0.0/0 via 203.0.113.1

Selected Path:
203.0.113.1
```

All traffic matching the default route normally uses the same next hop unless another more specific route or routing policy changes the decision.

### 🖆 Policy-Based Routing

PBR allows an administrator to apply additional forwarding rules.

For example:

* Traffic from `192.168.1.10` uses WAN 1.
* Traffic from `192.168.2.10` uses WAN 2.
* Traffic from a VoIP subnet uses a dedicated connection.
* Traffic to a specific destination uses a particular gateway.

```text
                    Router
                       |
              +--------+--------+
              |                 |
          WAN 1              WAN 2
        203.0.113.1        198.51.100.1
              |                 |
              +--------+--------+
                       |
                  LAN Network
                       |
                +------+------+
                |             |
             Client A      Client B
             192.168.1.10   192.168.2.10
                |             |
                v             v
             Policy 1       Policy 2
                |             |
                v             v
              WAN 1         WAN 2
```

PBR determines which routing policy applies to matching traffic.

# 🗁 How Policy-Based Routing Works

A typical PBR implementation follows this process:

```text
             Incoming Packet
                    |
                    v
             Inspect Packet
                    |
                    v
           Match Routing Policy
                    |
             +------+------+
             |             |
          Match          No Match
             |             |
             v             v
       Apply Policy    Normal Routing
             |             |
             v             v
       Select Routing   Select Route
           Table          Normally
             |             |
             +------+------+
                    |
                    v
              Forward Packet
```

The exact processing order depends on the operating system and router platform.

Some implementations evaluate routing policies before normal route lookup. Others use routing marks, routing tables, route maps, or policy databases.

### 🖆 Important Concepts

* A routing policy defines which traffic should receive special treatment.
* A matching rule identifies the traffic to which the policy applies.
* A routing table contains routes used to forward matching traffic.
* A policy action determines the selected table or next hop.
* If no policy matches, the platform generally follows its normal routing behavior.

PBR is not necessarily a replacement for destination-based routing. It is an additional mechanism for controlling route selection.

# 🗁 PBR Matching Criteria

PBR implementations can classify traffic using different packet attributes.

| Matching Criterion | Description | Example |
| --- | --- | --- |
| Source IP | Match the originating IP address | `192.168.1.10` |
| Destination IP | Match the destination IP address | `8.8.8.8` |
| Source subnet | Match a network of clients | `192.168.10.0/24` |
| Incoming interface | Match traffic entering an interface | `eth0` |
| Protocol | Match a network or transport protocol | TCP, UDP, ICMP |
| Source port | Match the application source port | `50000` |
| Destination port | Match an application service | TCP `443` |
| Firewall mark | Match a previously classified packet | Mark `100` |

Not every platform supports every matching criterion.

### 🖆 Example Policy Requirements

| Traffic | Policy |
| --- | --- |
| Finance subnet | WAN 1 |
| Development subnet | WAN 2 |
| VoIP traffic | Dedicated low-latency path |
| Guest network | Restricted internet gateway |
| Specific destination network | VPN tunnel |
| Server backup traffic | Backup WAN |

# 🗁 Common PBR Use Cases

### 🖆 Multi-WAN Traffic Distribution

An organization has two internet connections.

PBR can direct different client subnets through different WAN connections.

```text
LAN 1: 192.168.10.0/24
        |
        +------> WAN 1

LAN 2: 192.168.20.0/24
        |
        +------> WAN 2
```

This provides policy-based traffic distribution.

### 🖆 Application-Based Routing

An organization may want certain applications to use a dedicated connection.

Examples:

* VoIP traffic uses a low-latency WAN.
* Video conferencing uses a reliable WAN.
* Bulk downloads use a lower-cost connection.
* Backup traffic uses a secondary WAN.

Application identification may require firewall rules, connection tracking, or other traffic-classification mechanisms.

### 🖆 VPN Traffic Routing

PBR can direct traffic from selected subnets or destinations through a VPN tunnel.

Example:

```text
Office Network
192.168.10.0/24
       |
       v
    Router
       |
       +------> VPN Tunnel ------> Remote Network
       |
       +------> Normal WAN ------> Internet
```

Only traffic matching the VPN policy uses the tunnel.

### 🖆 ISP Traffic Engineering

An ISP may use PBR to direct traffic based on customer networks, traffic classes, or service requirements.

For example, different customer groups may use different upstream connections.

PBR should be designed carefully to avoid asymmetric paths and unintended routing loops.

# 🗁 Policy-Based Routing on Linux

Linux provides policy routing through the Routing Policy Database (RPDB).

The main tools are:

* `ip rule` — manage routing policy rules.
* `ip route` — manage routes and routing tables.
* `ip route get` — inspect route selection for a destination.
* `iptables` or `nftables` — classify packets and assign firewall marks.

Linux supports multiple routing tables. Policy rules determine which tables are consulted and in what order.

### 🖆 View Routing Rules

```bash
ip rule show
```

Example output:

```text
0:      from all lookup local
32766:  from all lookup main
32767:  from all lookup default
```

These are common default rules.

### 🖆 View Routing Tables

```bash
ip route show table main
ip route show table local
ip route show table default
```

### 🖆 Routing Table Selection

Linux routing tables can be referenced by numeric IDs or names.

The main built-in table identifiers are:

| Table | ID | Purpose |
| --- | ---: | --- |
| local | 255 | Local and broadcast routes |
| main | 254 | Normal routing table |
| default | 253 | Default routing table |
| unspec | 0 | Special unspecified table |

Custom routing tables can be assigned IDs and names.

# 🗁 Linux Routing Tables

Suppose a Linux router has two WAN interfaces:

| Interface | IP Address | Gateway |
| --- | --- | --- |
| `eth0` | `203.0.113.2/24` | `203.0.113.1` |
| `eth1` | `198.51.100.2/24` | `198.51.100.1` |

The main routing table contains the normal routes.

We can create separate routing tables for each WAN.

### 🖆 Define Custom Routing Tables

Edit:

```bash
sudo nano /etc/iproute2/rt_tables
```

Add:

```text
100 wan1
200 wan2
```

The names `wan1` and `wan2` are labels for routing table IDs.

### 🖆 Add Routes to WAN 1 Table

```bash
sudo ip route add 203.0.113.0/24 dev eth0 src 203.0.113.2 table wan1
sudo ip route add default via 203.0.113.1 dev eth0 table wan1
```

### 🖆 Add Routes to WAN 2 Table

```bash
sudo ip route add 198.51.100.0/24 dev eth1 src 198.51.100.2 table wan2
sudo ip route add default via 198.51.100.1 dev eth1 table wan2
```

Each table now has a default route through its respective WAN.

### 🖆 Inspect Custom Tables

```bash
ip route show table wan1
ip route show table wan2
```

The connected network routes are important because the gateway must be reachable through the appropriate interface.

The exact route configuration may also need connected LAN routes, VPN routes, or other routes required by the network design.

# 🗁 Linux Routing Policy Database

Linux uses the Routing Policy Database to decide which routing table should be consulted.

Rules are evaluated according to priority, with lower numerical priority values evaluated first.

### 🖆 View Rules

```bash
ip rule show
```

### 🖆 Add a Source-Based Rule

Suppose traffic from `192.168.10.0/24` should use WAN 1.

```bash
sudo ip rule add from 192.168.10.0/24 table wan1 priority 100
```

Suppose traffic from `192.168.20.0/24` should use WAN 2.

```bash
sudo ip rule add from 192.168.20.0/24 table wan2 priority 200
```

The resulting policy is:

| Source Network | Routing Table | WAN |
| --- | --- | --- |
| `192.168.10.0/24` | wan1 | `eth0` |
| `192.168.20.0/24` | wan2 | `eth1` |

### 🖆 Test Route Selection

```bash
ip route get 1.1.1.1 from 192.168.10.10
```

```bash
ip route get 1.1.1.1 from 192.168.20.10
```

These commands test the routing decision using the specified source addresses.

### 🖆 Remove a Rule

```bash
sudo ip rule del from 192.168.10.0/24 table wan1
```

### 🖆 Remove a Custom Route

```bash
sudo ip route flush table wan1
```

These `ip` commands modify the running configuration. For persistent configuration, use the network manager or network configuration system responsible for the interfaces.

# 🗁 Linux PBR Configuration

A complete source-based routing configuration must account for more than the policy rules themselves.

Consider this topology:

```text
                   Linux Router
                  /             \
              eth0               eth1
             WAN 1              WAN 2
          203.0.113.1        198.51.100.1
                  \             /
                   \           /
                    LAN Bridge
                         |
                 +-------+-------+
                 |               |
             Client A         Client B
          192.168.10.10     192.168.20.10
```

Assume:

* WAN 1 gateway: `203.0.113.1`
* WAN 2 gateway: `198.51.100.1`
* WAN 1 interface address: `203.0.113.2`
* WAN 2 interface address: `198.51.100.2`
* LAN interface: `br0`
* LAN address: `192.168.10.1/24` in this simplified example

For a real deployment, the LAN addressing must include both client networks, using either separate interfaces/VLANs or an appropriately configured bridge and subnet design.

### 🖆 Enable IPv4 Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

To persist the setting, create or edit a file under `/etc/sysctl.d/`:

```text
net.ipv4.ip_forward = 1
```

Apply the configuration:

```bash
sudo sysctl --system
```

### 🖆 Add Routing Rules

```bash
sudo ip rule add from 192.168.10.0/24 table wan1 priority 100
sudo ip rule add from 192.168.20.0/24 table wan2 priority 200
```

### 🖆 Add WAN Routing Tables

```bash
sudo ip route add 203.0.113.0/24 dev eth0 src 203.0.113.2 table wan1
sudo ip route add default via 203.0.113.1 dev eth0 table wan1

sudo ip route add 198.51.100.0/24 dev eth1 src 198.51.100.2 table wan2
sudo ip route add default via 198.51.100.1 dev eth1 table wan2
```

### 🖆 Configure Source NAT

If the WAN connections use private or provider-assigned addresses requiring source NAT, configure NAT using the appropriate firewall framework.

Example using nftables:

```bash
sudo nft add table ip nat

sudo nft 'add chain ip nat postrouting { type nat hook postrouting priority srcnat; policy accept; }'

sudo nft add rule ip nat postrouting oifname "eth0" masquerade
sudo nft add rule ip nat postrouting oifname "eth1" masquerade
```

This example enables masquerading for traffic leaving either WAN interface. It does not configure the complete firewall or forwarding policy.

### 🖆 Verify the Configuration

```bash
ip rule show
ip route show table wan1
ip route show table wan2
```

Test source-specific routing:

```bash
ip route get 8.8.8.8 from 192.168.10.10
ip route get 8.8.8.8 from 192.168.20.10
```

For actual client traffic, also verify forwarding, firewall rules, NAT, return-path behavior, and gateway reachability.

# 🗁 Policy-Based Routing with Firewall Marks

Linux can classify packets using firewall marks and use those marks to select routing tables.

A firewall mark is metadata associated with a packet inside the Linux networking stack. It is not transmitted as part of the IP packet.

Packet marks are commonly assigned using nftables or iptables.

### 🖆 Example Using nftables

Create a table and a prerouting chain:

```bash
sudo nft add table inet pbr
```

```bash
sudo nft 'add chain inet pbr prerouting { type filter hook prerouting priority mangle; policy accept; }'
```

Mark traffic from a selected source network:

```bash
sudo nft add rule inet pbr prerouting ip saddr 192.168.10.0/24 meta mark set 0x1
```

Mark another source network:

```bash
sudo nft add rule inet pbr prerouting ip saddr 192.168.20.0/24 meta mark set 0x2
```

### 🖆 Add Routing Rules for Marks

```bash
sudo ip rule add fwmark 0x1 table wan1 priority 100
sudo ip rule add fwmark 0x2 table wan2 priority 200
```

### 🖆 Verify Marks and Rules

```bash
sudo nft list ruleset
ip rule show
```

### 🖆 Important Considerations

* Packet marks are different from connection marks.
* A packet mark applies to an individual packet.
* Connection tracking can preserve classification across packets in a connection.
* Return traffic may need separate handling.
* Marks must be assigned before the relevant routing lookup.
* Locally generated traffic may require an output-chain marking strategy rather than only prerouting classification.

For complex multi-WAN designs, connection marks and packet marks should be planned carefully to maintain consistent routing decisions.

# 🗁 Policy-Based Routing on MikroTik RouterOS

MikroTik RouterOS v7 supports policy-based routing through routing tables, routing rules, and firewall routing marks.

The main mechanisms are:

* Custom routing tables.
* Routing rules.
* Firewall mangle rules.
* Routing marks.
* Route selection within the selected table.

RouterOS v7 uses the `/routing/table`, `/routing/rule`, and `/routing/route` configuration menus, along with firewall mangle rules.

### 🖆 Example Network

| Network | Purpose | Preferred WAN |
|---|---|---|
| `192.168.10.0/24` | Office | WAN 1 |
| `192.168.20.0/24` | Guest | WAN 2 |

Assume:

| WAN | Gateway |
| --- | --- |
| WAN 1 | `203.0.113.1` |
| WAN 2 | `198.51.100.1` |

### 🖆 Create Routing Tables

```routeros
/routing table
add name=to-wan1 fib
add name=to-wan2 fib
```

The `fib` property enables the table to participate in forwarding lookups.

### 🖆 Add Routes to the Custom Tables

```routeros
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1@main routing-table=to-wan1
add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to-wan2
```

The `@main` notation specifies that gateway resolution is performed using the main routing table.

This assumes the WAN gateway addresses are reachable through routes in the main table.

### 🖆 Configure Routing Rules

For simple source-based policies:

```routeros
/routing rule
add src-address=192.168.10.0/24 action=lookup-only-in-table table=to-wan1
add src-address=192.168.20.0/24 action=lookup-only-in-table table=to-wan2
```

`lookup-only-in-table` restricts lookup to the selected table. If no suitable route exists there, the lookup does not fall back to another table.

The `lookup` action can be used when fallback to subsequent routing rules or the main table is desired.

### 🖆 Verify Routing Rules

```routeros
/routing rule print
```

### 🖆 Verify Routing Tables

```routeros
/ip route print detail
```

### 🖆 Firewall Mangle-Based Routing

For more advanced classification, RouterOS can use firewall mangle rules to assign routing marks.

Example:

```routeros
/ip firewall mangle
add chain=prerouting src-address=192.168.10.0/24 action=mark-routing new-routing-mark=to-wan1 passthrough=no
add chain=prerouting src-address=192.168.20.0/24 action=mark-routing new-routing-mark=to-wan2 passthrough=no
```

The marked routes must exist in the corresponding routing tables.

### 🖆 Important RouterOS Considerations

* Create custom tables before referencing them in routing marks.
* Ensure each selected table contains the necessary routes.
* Use gateway resolution that matches the actual WAN topology.
* Exclude router-local traffic where appropriate.
* Be careful with FastTrack because FastTrack can bypass processing needed by some firewall and mangle-based policies.
* Account for connection tracking and return-path symmetry.
* Test routing rules and mangle policies independently.

RouterOS v6 uses different routing-mark configuration patterns. The examples in this document use RouterOS v7 syntax.

# 🗁 Policy-Based Routing on Cisco IOS

Cisco IOS supports PBR using route maps and interface-level policy application.

A route map defines matching conditions and the forwarding action.

### 🖆 Example Topology

```text
LAN Interface
192.168.10.1
      |
      v
   Router
      |
      +------ WAN 1
      |
      +------ WAN 2
```

Suppose traffic from `192.168.10.0/24` should use next hop `203.0.113.1`.

### 🖆 Create an Access Control List

```cisco
access-list 10 permit 192.168.10.0 0.0.0.255
```

### 🖆 Create a Route Map

```cisco
route-map PBR-WAN1 permit 10
 match ip address 10
 set ip next-hop 203.0.113.1
```

### 🖆 Apply the Policy to an Interface

```cisco
interface GigabitEthernet0/0
 ip policy route-map PBR-WAN1
```

The policy is applied to packets entering the interface.

### 🖆 Verify the Configuration

```cisco
show route-map
show ip policy
show ip access-lists
show ip route
```

Cisco IOS PBR supports additional match and set operations depending on the platform and software version.

# 🗁 PBR in Multi-WAN Networks

Multi-WAN networks commonly use PBR to distribute traffic across multiple internet connections.

Example:

```text
                   Internet
                      |
             +--------+--------+
             |                 |
           WAN 1             WAN 2
             |                 |
             +--------+--------+
                      |
                    Router
                      |
                +-----+-----+
                |           |
             Office       Guest
          192.168.10.0/24 192.168.20.0/24
                |           |
                v           v
              WAN 1       WAN 2
```

### 🖆 Example Policy

| Source Network | Preferred WAN | Backup WAN |
| --- | --- | --- |
| Office | WAN 1 | WAN 2 |
| Guest | WAN 2 | WAN 1 |
| Servers | WAN 1 | WAN 2 |

PBR can direct each network through its preferred connection.

### 🖆 Failover Considerations

A PBR rule that selects a routing table does not automatically guarantee failover.

A robust design must account for:

* WAN link failure.
* Gateway reachability.
* Upstream connectivity.
* Route withdrawal or tracking.
* Backup route availability.
* NAT configuration on each WAN.
* Return traffic and asymmetric routing.

For example, if WAN 1's Ethernet interface remains operational but its upstream internet connection fails, a simple interface-state check may not detect the problem.

Use appropriate health checks or route-tracking mechanisms for the platform.

# 🗁 PBR and NAT

Policy-based routing and Network Address Translation often work together in multi-WAN environments.

PBR selects the outgoing path, while NAT translates source addresses when required.

Example:

```text
Client
192.168.10.10
      |
      v
   Router
      |
      +------ PBR selects WAN 1
      |
      v
  Source NAT
      |
      v
WAN 1 Address
203.0.113.2
      |
      v
   Internet
```

If traffic exits WAN 1, source NAT commonly translates the private client address to the WAN 1 address.

If traffic exits WAN 2, it may require translation to the WAN 2 address.

### 🖆 Why NAT Must Match the Outgoing WAN

Suppose a client sends traffic through WAN 1 but receives a source address associated with WAN 2.

The upstream network may reject the packet because its source address is invalid for that path.

Therefore, NAT rules must align with the selected outgoing interface and the addressing model of the WAN connection.

### 🖆 Return Traffic

The return path should normally reach the router and connection-tracking state that handled the original connection.

Asymmetric routing can cause problems with:

* Stateful firewalls.
* NAT.
* Connection tracking.
* Applications sensitive to source-address changes.

For multi-WAN networks, design both forward and return paths deliberately.

# 🗁 Troubleshooting Policy-Based Routing

PBR troubleshooting should verify classification, policy evaluation, route availability, forwarding, and return traffic.

### 🖆 Inspect Linux Routing Rules

```bash
ip rule show
```

### 🖆 Inspect Routing Tables

```bash
ip route show table main
ip route show table wan1
ip route show table wan2
```

### 🖆 Test Source-Based Routing

```bash
ip route get 8.8.8.8 from 192.168.10.10
```

### 🖆 Inspect Firewall Marks

```bash
sudo nft list ruleset
```

For connection tracking:

```bash
sudo conntrack -L
```

The `conntrack` command may require installing the appropriate package.

### 🖆 Capture Packets

```bash
sudo tcpdump -ni eth0
```

```bash
sudo tcpdump -ni eth1
```

Compare the packet flow on both WAN interfaces.

### 🖆 Common Troubleshooting Checklist

| Symptom | Possible Cause |
| --- | --- |
| Traffic uses the wrong WAN | Incorrect policy match or rule priority |
| Traffic cannot reach the internet | Missing default route in the selected table |
| Gateway is unreachable | Missing connected route or incorrect gateway |
| Some applications fail | NAT, MTU, firewall, or return-path issue |
| Only some packets follow the policy | Inconsistent marking or policy scope |
| Router-originated traffic uses the wrong WAN | Local output traffic is not covered by the policy |
| Connections fail after WAN switching | Existing connection tracking or NAT state |
| FastTrack bypasses expected processing | Firewall acceleration conflicts with the policy design |

### 🖆 Useful Linux Commands

```bash
ip rule show
ip route show table all
ip route get 1.1.1.1 from 192.168.10.10
ip neigh show
ip -s link
ss -tn
sudo nft list ruleset
sudo tcpdump -ni any
```

### 🖆 Useful MikroTik Commands

```routeros
/routing rule print detail
/routing table print
/ip route print detail
/ip firewall mangle print detail
/ip firewall connection print
/interface print stats
```

# 🗁 Best Practices

* Design routing policies around clearly defined traffic requirements.
* Use separate routing tables when different traffic groups require different gateways.
* Verify route availability in every custom routing table.
* Consider router-originated traffic separately from forwarded client traffic.
* Ensure NAT matches the selected outgoing WAN.
* Maintain consistent return paths for stateful connections.
* Avoid unnecessary overlapping policy rules.
* Use clear names for routing tables, firewall marks, and route maps.
* Test WAN failure scenarios, not just normal operation.
* Document policy priority and fallback behavior.
* Monitor WAN health and route availability.
* Account for platform-specific acceleration features such as FastTrack.

# 🗁 Common Mistakes

| Mistake | Correct Understanding |
| --- | --- |
| PBR replaces normal routing. | PBR modifies route selection for matching traffic; normal routing remains important. |
| Creating a routing table is sufficient. | The table must contain usable routes and the policy must select it. |
| Every PBR rule applies to router-originated traffic. | Some policies apply only to forwarded or incoming traffic. |
| PBR automatically provides WAN failover. | Health checks and fallback routes must be configured. |
| NAT is unrelated to PBR. | Multi-WAN PBR commonly requires matching NAT behavior. |
| A firewall mark is transmitted to the destination. | A Linux firewall mark is internal packet metadata. |
| All platforms implement PBR identically. | Policy matching, processing order, and fallback behavior vary. |
| FastTrack never affects routing policies. | FastTrack can bypass processing required by some mangle-based designs. |
| A selected gateway guarantees connectivity. | The gateway and upstream path must also be reachable. |

# 🗁 Interview Questions

### 🖆 What is Policy-Based Routing?

PBR is a routing technique that selects forwarding paths based on configured policies and traffic attributes rather than relying exclusively on the destination IP address.

### 🖆 How is PBR different from traditional routing?

Traditional routing primarily uses destination-based route lookup. PBR can additionally consider source addresses, interfaces, protocols, ports, packet marks, and other attributes.

### 🖆 What are common PBR use cases?

Common use cases include multi-WAN routing, application-based routing, VPN traffic selection, ISP traffic engineering, and network segmentation.

### 🖆 What is the Linux Routing Policy Database?

The Linux Routing Policy Database is the mechanism that evaluates routing rules and determines which routing table should be consulted.

### 🖆 What is the purpose of `ip rule`?

The `ip rule` command adds, removes, and displays Linux routing policy rules.

### 🖆 What is the purpose of a custom routing table?

A custom routing table provides an independent set of routes that can be selected by a policy rule.

### 🖆 What is a firewall mark?

A firewall mark is metadata assigned to a packet inside the Linux networking stack. It can be used to select a routing table or apply other packet-processing policies.

### 🖆 Why is NAT important in multi-WAN PBR?

NAT must translate client source addresses appropriately for the selected outgoing WAN connection. Incorrect NAT can cause upstream filtering or return-path failures.

### 🖆 Does PBR automatically provide failover?

No. PBR needs appropriate route availability, health checks, or tracking mechanisms to handle failures.

### 🖆 What is asymmetric routing?

Asymmetric routing occurs when traffic travels through different paths in the forward and return directions. It can cause problems with stateful firewalls, NAT, and connection tracking.

### 🖆 How do you test Linux source-based routing?

Use:

```bash
ip route get <destination-ip> from <source-ip>
```

### 🖆 What is the difference between `lookup` and `lookup-only-in-table` in RouterOS v7?

`lookup` allows subsequent routing-rule processing if the selected table cannot resolve the destination. `lookup-only-in-table` restricts lookup to the specified table and does not fall back to another table.

# 🗁 Summary

Policy-Based Routing provides control over how selected traffic is forwarded through a network.

Key concepts:

* Traditional routing primarily uses destination-based lookup.
* PBR applies additional policies to matching traffic.
* Policies can match source addresses, destinations, interfaces, protocols, ports, and marks.
* Linux implements policy routing through the Routing Policy Database and multiple routing tables.
* MikroTik RouterOS v7 supports routing rules, custom tables, and firewall routing marks.
* Cisco IOS supports PBR through route maps.
* Multi-WAN environments commonly use PBR to distribute traffic across different internet connections.
* NAT, forwarding, gateway reachability, and return-path behavior must be considered.
* PBR does not automatically provide reliable failover; health monitoring and backup routes are also required.

PBR is an important technique for advanced routing designs, especially in multi-WAN environments, ISP networks, and networks with traffic-specific forwarding requirements.

# 🗁 References

* [Linux IP Routing — ip-rule Manual](https://man7.org/linux/man-pages/man8/ip-rule.8.html)
* [Linux IP Routing — ip-route Manual](https://man7.org/linux/man-pages/man8/ip-route.8.html)
* [Linux Advanced Routing and Traffic Control](https://lartc.org/)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [Cisco — Policy-Based Routing Configuration Guide](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-121-mainline/47121-pbr-cfg.html)
* [nftables Documentation](https://wiki.nftables.org/)
