# Load Balancing and Failover Fundamentals

# 🗁 What Is Load Balancing?

Load balancing is a networking technique that distributes traffic across multiple Internet connections or WAN links.

An ISP, organization, or network administrator can use multiple upstream connections to improve bandwidth utilization, distribute traffic, and reduce dependency on a single Internet connection.

For example, a MikroTik router connected to two ISPs can distribute outgoing traffic across both connections.

```text
                 Internet
                 /      \
                /        \
           ISP-1          ISP-2
          100 Mbps       200 Mbps
                \        /
                 \      /
              MikroTik Router
                     |
                 LAN Network
                     |
                  Clients
```

Instead of sending all traffic through one ISP, the router can distribute selected connections across both WAN links.

# 🗁 What Is Failover?

Failover is a network redundancy mechanism that automatically switches traffic to an alternative WAN connection when the primary connection becomes unavailable.

For example:

```text
                 ISP-1
                Primary
                   |
                   |
              MikroTik
               Router
                   |
                   |
                 ISP-2
                Backup
```

Under normal conditions, traffic uses ISP-1.

If ISP-1 becomes unavailable, the router switches traffic to ISP-2.

When ISP-1 becomes available again, the router can return traffic to the primary connection, depending on the configured routing policy.

# 🗁 Load Balancing vs Failover

| Feature | Load Balancing | Failover |
| --- | --- | --- |
| Main purpose | Distribute traffic | Maintain connectivity |
| WAN usage | Multiple links can be active | Usually one preferred link |
| Backup connection | Can be active simultaneously | Usually used when primary fails |
| Bandwidth utilization | Uses multiple links | Primarily uses preferred link |
| Failure handling | Redistributes new connections | Switches to an alternative route |
| Common implementation | PCC, ECMP, policy routing | Route distance, gateway checks |

Load balancing and failover can also be combined.

For example, an ISP can distribute normal traffic across two WAN links and use a third link as a backup.

# 🗁 Why Use Multiple WAN Connections?

Multiple WAN connections provide several benefits:

* Improved network availability.
* Better utilization of available bandwidth.
* Reduced dependency on one ISP.
* Alternative paths during upstream failures.
* Flexible traffic distribution.
* Support for different traffic policies.
* Better resilience for business and ISP networks.

However, multiple WAN connections introduce additional routing, NAT, monitoring, and troubleshooting requirements.

# 🗁 Understanding WAN Link Types

A MikroTik router can connect to multiple types of upstream networks.

| WAN type | Description |
| --- | --- |
| DHCP Client | Receives addressing and routing information dynamically |
| Static IP | Uses manually configured addressing |
| PPPoE Client | Establishes a PPPoE session with the provider |
| LTE/5G | Uses a mobile broadband connection |
| VLAN WAN | Uses a VLAN interface to reach the upstream network |

Each WAN connection must have appropriate addressing, routing, and upstream connectivity before load balancing or failover can work.

# 🗁 Understanding Routing Distance

RouterOS uses route distance to determine preference among routes with the same destination and routing context.

A route with a lower distance is generally preferred over a route with a higher distance when competing routes are otherwise eligible.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1 distance=1
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2
```

In this example:

* The first route is preferred.
* The second route is an alternative.
* If the preferred route becomes inactive, the alternative can be selected.

The addresses above are documentation-only examples. Replace them with actual WAN gateway addresses.

Route distance alone does not guarantee that an upstream Internet connection is healthy. A gateway may remain reachable while the ISP's wider network is unavailable.

# 🗁 Understanding Gateway Reachability

A gateway is the next-hop router through which traffic leaves the local network.

For example:

```text
MikroTik Router
      |
      | WAN Interface
      |
ISP Gateway
      |
      |
Internet
```

A router may successfully communicate with its ISP gateway even when the ISP has lost upstream connectivity.

This creates an important distinction:

* Gateway reachability: The next-hop router responds.
* Internet reachability: A destination beyond the ISP network can be reached.

Reliable failover design should monitor a suitable upstream destination rather than relying only on the directly connected gateway.

# 🗁 Understanding RouterOS Gateway Checks

RouterOS supports gateway checking through route configuration.

One commonly used option is `check-gateway=ping`.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=192.0.2.1 \
    check-gateway=ping \
    distance=1
```

RouterOS periodically checks the configured gateway using ICMP echo requests.

If the gateway fails the reachability checks, the route can become inactive.

Gateway checking is useful for basic failover, but it does not necessarily detect failures beyond the gateway.

Recursive gateway checks provide another approach for monitoring reachability through an upstream path. They are covered in the next document.

# 🗁 Understanding Connection-Based Load Balancing

Most MikroTik multi-WAN load balancing designs distribute connections rather than individual packets.

This distinction matters because a single application session usually expects its outgoing packets to use a consistent public source address and WAN path.

For example:

```text
Client A
   |
   +---- Connection 1 ---- WAN-1
   |
   +---- Connection 2 ---- WAN-2
   |
   +---- Connection 3 ---- WAN-1
```

Different connections can use different WAN links while each connection remains associated with its selected path.

Connection-based distribution is commonly used for web browsing, downloads, and other ordinary Internet traffic.

# 🗁 Connection Distribution vs Packet Distribution

**Connection-based distribution**

* Assigns a connection to a WAN link.
* Keeps packets belonging to that connection on the selected path.
* Works well with stateful NAT.
* Commonly implemented using PCC or connection marks.

**Per-packet distribution**

* Distributes individual packets across multiple WAN links.
* Packets from one connection may take different paths.
* Can introduce packet reordering and inconsistent latency.
* May cause problems with NAT and stateful applications.

For ordinary Internet access through multiple NATed WAN connections, connection-based load balancing is generally the preferred approach.

# 🗁 Understanding PCC

Per Connection Classifier (PCC) is a RouterOS firewall matcher used to classify connections according to selected address and port fields.

PCC can divide connections into groups so that different connections use different WAN links.

Common PCC classification methods include:

* Source address.
* Destination address.
* Source and destination address.
* Source and destination ports.
* Combinations of address and port fields.

For example, a two-WAN design can divide connections into two groups:

```text
                 New Connections
                       |
                       |
                    PCC Rule
                       |
                +------+------+
                |             |
             Group 0       Group 1
                |             |
              WAN-1         WAN-2
```

PCC is particularly useful when a router needs to distribute new connections across multiple WAN links while maintaining connection consistency.

Detailed PCC configuration is covered in its dedicated document.

# 🗁 Understanding ECMP

Equal-Cost Multi-Path (ECMP) routing allows multiple routes with equal routing preference to be used for forwarding traffic.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1,198.51.100.1
```

The router can use multiple eligible next hops for the same destination.

ECMP is a routing-level mechanism, while PCC is a connection-classification technique implemented through firewall and routing marks.

They serve related but different purposes.

ECMP behavior depends on the routing configuration and RouterOS version. It should not be assumed to provide the same connection distribution or policy control as a carefully designed PCC configuration.

# 🗁 Understanding NAT with Multiple WAN Connections

When multiple WAN links use different public IP addresses, NAT must be designed to match the selected outgoing WAN.

For example:

```text
LAN Network
192.168.10.0/24
       |
       |
 MikroTik Router
       |
       +---------------- WAN-1
       |                  Public IP: ISP-1
       |
       +---------------- WAN-2
                          Public IP: ISP-2
```

A basic NAT configuration can use separate rules:

```routeros
/ip/firewall/nat
add chain=srcnat out-interface=ether1 action=masquerade comment="WAN-1 NAT"
add chain=srcnat out-interface=ether2 action=masquerade comment="WAN-2 NAT"
```

The outgoing interface determines which masquerade rule applies.

For policy-based load balancing, routing and NAT must work together. Connections marked for WAN-1 should leave through WAN-1, and connections marked for WAN-2 should leave through WAN-2.

A NAT rule by itself does not select the routing path.

# 🗁 Understanding Connection Tracking

RouterOS connection tracking maintains information about active network connections.

It is important for:

* Stateful firewall rules.
* NAT translations.
* Connection marking.
* PCC classification.
* Connection persistence.
* Troubleshooting multi-WAN traffic.

Inspect active connections:

```routeros
/ip/firewall/connection/print detail
```

Check the number of tracked connections:

```routeros
/ip/firewall/connection/print count-only
```

When a WAN link fails, existing connections associated with that WAN may stop working. New connections can use another available route, but existing sessions do not automatically migrate their NAT state to a different public IP address.

Applications may need to reconnect.

# 🗁 Understanding Policy-Based Routing

Policy-based routing (PBR) allows traffic to use different routing tables or paths according to defined conditions.

For example:

* One LAN subnet uses WAN-1.
* Another LAN subnet uses WAN-2.
* Selected destinations use a preferred ISP.
* VoIP traffic uses a low-latency connection.
* Backup traffic uses a secondary connection.

RouterOS can implement policy-based routing through routing tables and routing marks.

A basic example of creating a separate routing table:

```routeros
/routing/table
add name=to-wan2 fib
```

A routing rule can direct traffic from a particular source subnet into that table:

```routeros
/routing/rule
add src-address=192.168.20.0/24 \
    action=lookup-only-in-table \
    table=to-wan2
```

The selected routing table must contain a suitable route to the destination. If no usable route exists, traffic handled with `lookup-only-in-table` will not fall back to another routing table.

Production PBR designs also need to consider connection tracking, NAT, local router traffic, and routing symmetry.

# 🗁 Understanding Symmetric Routing

Symmetric routing means traffic travels through the same network path in both directions, or through paths that are compatible with the stateful network devices involved.

Example of a symmetric connection:

```text
Client
  |
  v
MikroTik Router
  |
  v
WAN-1
  |
  v
Internet
  |
  v
WAN-1
  |
  v
MikroTik Router
  |
  v
Client
```

Asymmetric routing can occur when outgoing traffic uses WAN-1 but return traffic arrives through WAN-2.

This can cause problems with:

* NAT translations.
* Stateful firewall connection tracking.
* Upstream filtering.
* Application sessions.

Connection marking and policy routing help maintain consistent paths for multi-WAN connections.

# 🗁 Understanding FastTrack in Multi-WAN Networks

FastTrack accelerates eligible established connections by bypassing parts of the normal packet-processing path.

However, FastTrack can interfere with features that depend on regular packet processing, including:

* Mangle-based connection marking.
* Policy-based routing.
* Queue processing.
* Some traffic accounting and filtering designs.

A multi-WAN configuration using PCC and routing marks must be designed carefully around FastTrack.

Do not assume that the default FastTrack rule is suitable for every multi-WAN configuration. Exclude traffic requiring special routing or processing from FastTrack, or disable FastTrack where appropriate.

# 🗁 Common Multi-WAN Design Patterns

**Primary and backup**

```text
WAN-1: Primary
WAN-2: Backup
```

All normal traffic uses WAN-1. WAN-2 takes over when the primary path becomes unavailable.

**Two-WAN load balancing**

```text
WAN-1: Active
WAN-2: Active
```

New connections are distributed across both WAN links.

**Load balancing with backup**

```text
WAN-1: Active
WAN-2: Active
WAN-3: Backup
```

Normal traffic is distributed across two links. A third link provides additional redundancy.

**Policy-based WAN selection**

```text
LAN-1 -------- WAN-1
LAN-2 -------- WAN-2
VoIP ---------- WAN-1
Backup Traffic - WAN-2
```

Different traffic groups use different WAN links according to network policy.

# 🗁 Important Design Considerations

Before implementing multi-WAN routing, consider:

* WAN bandwidth and capacity.
* ISP gateway addresses.
* Public IP address assignments.
* NAT requirements.
* Routing table design.
* Gateway monitoring.
* Connection tracking.
* Firewall rule order.
* PCC classification strategy.
* Return traffic paths.
* FastTrack compatibility.
* VPN and port-forwarding requirements.
* Router CPU and memory capacity.
* IPv6 routing and failover.

# 🗁 Verification Commands

Check active routes:

```routeros
/ip/route/print detail
```

Check routing tables:

```routeros
/routing/table/print
```

Check routing rules:

```routeros
/routing/rule/print detail
```

Check WAN interface status:

```routeros
/interface/print detail
```

Monitor WAN traffic:

```routeros
/interface/monitor-traffic ether1
/interface/monitor-traffic ether2
```

Inspect NAT rules:

```routeros
/ip/firewall/nat/print stats
```

Inspect connection tracking:

```routeros
/ip/firewall/connection/print count-only
```

Check system resources:

```routeros
/system/resource/print
```

# 🗁 Best Practices

* Plan WAN addressing and routing before configuring load balancing.
* Use connection-based distribution for ordinary multi-WAN Internet access.
* Understand the difference between gateway reachability and Internet reachability.
* Use suitable health checks for failover.
* Keep NAT consistent with the selected outgoing WAN.
* Preserve connection symmetry where required.
* Review FastTrack rules when using PCC or policy routing.
* Test failure scenarios instead of assuming failover works.
* Monitor each WAN independently.
* Document routing tables, connection marks, and NAT policies.
* Keep a backup of the working configuration before changing routing behavior.
