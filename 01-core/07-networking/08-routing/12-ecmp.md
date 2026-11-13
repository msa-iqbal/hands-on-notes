# Equal-Cost Multipath Routing (ECMP)

Equal-Cost Multipath (ECMP) is a routing technique that allows a router to install and use multiple forwarding paths to the same destination when those paths have equal routing preference and cost.

Instead of forwarding all traffic through a single next hop, ECMP distributes traffic across multiple available paths.

ECMP improves network utilization, provides path redundancy, and can increase aggregate network throughput when multiple links are available.

It is commonly used in enterprise networks, data centers, service-provider networks, and large IP routing environments.

# 🗁 Introduction to ECMP

Traditional routing commonly forwards traffic through the selected best route.

When multiple paths to a destination have equal routing preference and equal cost, a router can install multiple next hops in its forwarding table.

This is called Equal-Cost Multipath routing.

For example:

```text
                    Router B
                   /        \
                  /          \
              Router A      Router D
                  \          /
                   \        /
                    Router C
                        |
                  Destination
                   Network X
```

Router A has two equal-cost paths to Network X:

* Router A → Router B → Router D → Network X
* Router A → Router C → Router D → Network X

If both paths qualify for ECMP, Router A can use both next hops.

**Key characteristics:**

| Characteristic | Description |
| --- | --- |
| Routing technique | Multiple-path forwarding |
| Path requirement | Equal routing preference and cost |
| Main purpose | Load distribution and redundancy |
| Forwarding behavior | Uses multiple next hops |
| Common distribution | Per-flow hashing |
| Common protocols | OSPF, IS-IS, RIP, BGP and static routing |
| Main benefits | Better path utilization and resilience |

# 🗁 How ECMP Works

ECMP works by identifying multiple valid routes to the same destination and installing them as alternative forwarding next hops.

The general process is:

* A routing protocol or static configuration provides multiple routes.
* The router evaluates route preference and metric.
* Routes that qualify as equal-cost alternatives are selected.
* The forwarding table stores multiple next hops.
* A load-balancing algorithm selects a path for outgoing traffic.
* If a path becomes unavailable, the router removes or stops using that path and forwards traffic through the remaining eligible paths.

**Example routing table:**

```text
Destination: 192.168.50.0/24

Next Hop: 10.0.12.2
Next Hop: 10.0.13.2
```

Both next hops provide an equal-cost route to the destination.

The router can distribute eligible traffic between them.

# 🗁 ECMP Example Topology

Consider a network with two paths between Router A and Router D.

```text
                    10.0.12.0/30
              +----------------------+
              |                      |
          Router A                Router B
              |                      |
              |                      |
              |                  Router D
              |                      |
          Router C                   |
              |                      |
              +----------------------+
                    10.0.13.0/30

                    Destination
                  192.168.50.0/24
```

A simpler logical representation is:

```text
                 +---- Router B ----+
                 |                  |
Source ---- Router A             Router D ---- Destination
                 |                  |
                 +---- Router C ----+
```

Assume both paths have the same routing cost.

Router A installs two next hops:

| Path | Next Hop | Cost |
| --- | --- | ---: |
| Path 1 | Router B | 20 |
| Path 2 | Router C | 20 |

Router A can distribute traffic across both paths.

If Router B becomes unavailable, Router A can continue forwarding through Router C, assuming the remaining path is still valid.

# 🗁 ECMP and Routing Protocols

ECMP can be used with several routing mechanisms.

| Routing Source | ECMP Support | How Equal Cost Is Determined |
| --- | --- | --- |
| Static routing | Platform-dependent | Configured route preference and metric |
| OSPF | Yes | Equal OSPF path cost |
| IS-IS | Yes | Equal IS-IS path metric |
| RIP | Implementation-dependent | Equal hop count |
| BGP | Platform and configuration-dependent | Multiple paths qualify under BGP multipath rules |

The exact number of installed paths and the rules for selecting them depend on the routing platform.

**Important:** ECMP does not mean that every route to a destination is automatically used. Routes must satisfy the platform's route-selection and multipath eligibility rules.

# 🗁 ECMP Load-Balancing Methods

Routers can distribute traffic across multiple paths using different algorithms.

Two common methods are:

* Per-flow load balancing.
* Per-packet load balancing.

# 🗁 Per-Flow Load Balancing

Per-flow load balancing assigns packets belonging to the same flow to the same next hop.

A router commonly uses a hash calculated from packet information such as:

* Source IP address.
* Destination IP address.
* IP protocol.
* Source TCP or UDP port.
* Destination TCP or UDP port.

The selected next hop remains consistent for packets with the same hash inputs, subject to the implementation's hashing and path-set behavior.

**Example:**

```text
Flow A → Router B
Flow B → Router C
Flow C → Router B
Flow D → Router C
```

Advantages:

* Preserves packet ordering within a flow in normal operation.
* Reduces packet reordering.
* Works well for TCP traffic.
* Distributes multiple independent flows across available paths.

Disadvantages:

* A single large flow generally uses only one path.
* Traffic distribution can be uneven.
* A small number of large flows may not utilize all available links effectively.

Per-flow hashing is widely used in routers because it generally avoids packet reordering caused by sending consecutive packets over paths with different delays.

# 🗁 Per-Packet Load Balancing

Per-packet load balancing distributes individual packets across multiple available paths.

For example:

```text
Packet 1 → Router B
Packet 2 → Router C
Packet 3 → Router B
Packet 4 → Router C
```

Advantages:

* Can distribute individual packets across multiple paths.
* May utilize multiple links even when there are few traffic flows.

Disadvantages:

* Different paths may have different latency.
* Packets can arrive out of order.
* TCP performance can be affected by packet reordering.
* Applications may experience unstable packet delivery timing.

For these reasons, per-packet load balancing is less commonly used for general-purpose IP routing.

# 🗁 ECMP Hashing

ECMP commonly uses a hash function to select a forwarding path.

A hash function processes packet fields and produces a value used to select one of the available next hops.

Conceptually:

```text
Packet Information
        |
        v
+----------------------+
| Source IP            |
| Destination IP       |
| Protocol             |
| Source Port          |
| Destination Port     |
+----------------------+
        |
        v
    Hash Function
        |
        v
  Next-Hop Selection
        |
        +------ Router B
        |
        +------ Router C
```

For example, a router may calculate a hash and map the result to one of two paths.

```text
Hash Result 0 → Path 1
Hash Result 1 → Path 2
```

The actual algorithm and packet fields vary by vendor and configuration.

**Hashing considerations:**

* Layer 3 hashing uses IP addresses.
* Layer 4 hashing can also use transport ports.
* Layer 2 hashing can use MAC addresses.
* Some platforms support configurable hash fields.
* Tunnel and encapsulation headers can affect which fields are available.
* A change in the ECMP next-hop set can remap some flows.

# 🗁 ECMP and Unequal-Cost Paths

ECMP specifically refers to equal-cost paths.

When paths have different costs, ordinary ECMP does not automatically distribute traffic across them.

Example:

| Path | Cost |
|---|---:|
| Path A | 10 |
| Path B | 20 |

Path A is preferred under ordinary shortest-path selection.

Some platforms support unequal-cost load balancing through additional mechanisms, but this is not standard ECMP behavior.

Examples include:

* Cisco EIGRP variance-based unequal-cost load balancing.
* Vendor-specific weighted multipath implementations.
* Policy-based forwarding and traffic-engineering mechanisms.

These mechanisms should not be confused with equal-cost multipath routing.

# 🗁 ECMP and Failover

ECMP provides path redundancy because multiple next hops are available.

When one path becomes unavailable, the router can remove that path from the forwarding set and continue using the remaining paths.

Example:

```text
Before Failure:

Source
  |
Router A
  |
  +------ Router B ------+
  |                      |
  +------ Router C ------+
                         |
                    Destination


After Router B Failure:

Source
  |
Router A
  |
  +------ Router C ------+
                         |
                    Destination
```

ECMP failover depends on how the router detects path failure.

Possible mechanisms include:

* Interface link-state detection.
* Routing protocol neighbor failure detection.
* Routing protocol dead timers.
* Bidirectional Forwarding Detection (BFD), where supported.
* Next-hop reachability tracking.

A physical link can remain operational while a remote destination is unreachable. Therefore, link-state detection alone may not detect every forwarding failure.

**Important:** ECMP provides multiple paths, but it does not guarantee hitless failover. Existing flows can be remapped, packets can be lost during convergence, and application sessions may be affected.

# 🗁 ECMP vs Link Aggregation

ECMP and Link Aggregation both distribute traffic across multiple paths or links, but they operate at different network layers and use different mechanisms.

| Feature | ECMP | Link Aggregation |
| --- | --- | --- |
| Primary layer | Layer 3 | Layer 2 |
| Main purpose | Multiple IP forwarding paths | Combine physical links into one logical link |
| Typical technology | Routing protocols and forwarding tables | LACP or static aggregation |
| Path selection | Routing and hashing | Link aggregation hashing |
| Network topology | Can use multiple routers | Usually connects the same pair of logical devices |
| Failure handling | Removes unavailable next hops | Removes unavailable member links |
| Common use | Routed network redundancy | Switch-to-switch or switch-to-server connectivity |

**Example of ECMP:**

```text
Router A
   |
   +---- Router B ----+
   |                  |
   +---- Router C ----+
                      |
                  Destination
```

**Example of Link Aggregation:**

```text
       Switch A
        |    |
        |    |
        |    |
        |    |
        Switch B
```

Two physical Ethernet links are combined into one logical Layer 2 connection.

ECMP is not a replacement for LACP, and LACP is not a replacement for ECMP.

# 🗁 ECMP Configuration on Linux

Linux supports multipath routing through the `iproute2` routing utilities.

A multipath route can contain multiple next hops.

**Example topology**

```text
                  Gateway A
                 10.0.12.2
                     |
                  Interface
                     |
Linux Router --------+
                     |
                  Interface
                     |
                 10.0.13.2
                  Gateway B
```

Assume the Linux router has these addresses:

| Interface | Address |
| --- | --- |
| `eth0` | `10.0.12.1/30` |
| `eth1` | `10.0.13.1/30` |

The destination network is:

```text
192.168.50.0/24
```

**Configure the interfaces**

```bash
sudo ip address add 10.0.12.1/30 dev eth0
sudo ip address add 10.0.13.1/30 dev eth1

sudo ip link set eth0 up
sudo ip link set eth1 up
```

Configure the ECMP route:

```bash
sudo ip route add 192.168.50.0/24 \
    nexthop via 10.0.12.2 dev eth0 weight 1 \
    nexthop via 10.0.13.2 dev eth1 weight 1
```

Both next hops have equal weight.

**Verify the route**

```bash
ip route show
```

Example output:

```text
192.168.50.0/24
    nexthop via 10.0.12.2 dev eth0 weight 1
    nexthop via 10.0.13.2 dev eth1 weight 1
```

**Inspect route selection**

```bash
ip route get 192.168.50.10
```

The command performs a route lookup for the supplied destination. The selected next hop can depend on the flow information supplied to the lookup and the Linux multipath implementation.

For flow-specific lookup:

```bash
ip route get 192.168.50.10 \
    from 192.168.1.10 \
    ipproto tcp \
    sport 40000 \
    dport 443
```

**Delete the ECMP route**

```bash
sudo ip route del 192.168.50.0/24
```

**Enable IPv4 forwarding**

If the Linux machine is acting as a router:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

For persistent configuration, create an appropriate file under `/etc/sysctl.d/`.

Example:

```ini
net.ipv4.ip_forward=1
```

Apply the configuration:

```bash
sudo sysctl --system
```

**Important:** The example assumes both gateways are reachable through their respective interfaces and that the remote network has valid return routes. A multipath route alone does not create end-to-end connectivity.

# 🗁 ECMP Configuration on MikroTik RouterOS

MikroTik RouterOS v7 supports multipath routing through multiple equal-preference routes.

A common approach is to configure multiple routes to the same destination with equal distance and different gateways.

**Example topology**

```text
                 Gateway A
                    |
                 10.0.12.2
                    |
                RouterOS
                    |
                 10.0.13.2
                    |
                 Gateway B

Destination: 192.168.50.0/24
```

Assume the RouterOS router has:

```text
ether1: 10.0.12.1/30
ether2: 10.0.13.1/30
```

Configure the routes:

```routeros
/ip route
add dst-address=192.168.50.0/24 gateway=10.0.12.2 distance=1
add dst-address=192.168.50.0/24 gateway=10.0.13.2 distance=1
```

Both routes have the same destination prefix and distance.

When the routes qualify for multipath installation, RouterOS can use both next hops.

**Verify routes**

```routeros
/ip route print detail
/routing route print detail
```

Inspect the active routing entries and their gateway information.

**Important considerations:**

* Equal distance alone does not guarantee that every route will be installed as an ECMP next hop.
* Next-hop reachability must be valid.
* Routing-table and gateway-resolution behavior must be considered.
* RouterOS v7 route selection and multipath behavior can depend on route source and configuration.
* Verify the installed active route rather than assuming that two configured routes are both forwarding traffic.

For production deployment, consult the RouterOS documentation for the installed release and validate the actual forwarding behavior.

# 🗁 ECMP in OSPF

OSPF supports equal-cost multipath routing.

OSPF calculates shortest paths using its link-state database and the Shortest Path First (SPF) algorithm.

When multiple paths to the same destination have equal OSPF cost, a router can install multiple next hops.

Example:

```text
                 Router B
                /        \
               /          \
          Router A       Router D
               \          /
                \        /
                 Router C
                      |
                 Network X
```

Assume:

```text
Router A → Router B → Router D = Cost 20
Router A → Router C → Router D = Cost 20
```

Both paths have equal cost.

OSPF can install both paths as ECMP next hops, subject to platform limits and configuration.

**Cisco IOS example**

```cisco
configure terminal

router ospf 1
 maximum-paths 4

end
```

The `maximum-paths` command controls the maximum number of equal-cost paths installed by supported Cisco IOS implementations.

Verify:

```cisco
show ip route ospf
show ip ospf
show ip ospf database
```

OSPF cost is the routing metric. Equal OSPF cost does not necessarily mean equal bandwidth or equal latency.

# 🗁 ECMP in BGP

BGP can support multipath routing when multiple paths satisfy the platform's BGP multipath eligibility rules.

Unlike OSPF, BGP does not simply calculate the shortest physical path.

BGP evaluates path attributes and routing policies to select its preferred path.

Some platforms allow multiple eligible BGP paths to be installed in the forwarding table.

This is commonly called BGP multipath or BGP ECMP.

Examples include:

* eBGP multipath.
* iBGP multipath.
* eBGP and iBGP multipath under supported configurations.
* Equal-cost paths from multiple upstream providers.

**Cisco IOS example**

```cisco
configure terminal

router bgp 65001
 maximum-paths 4

end
```

For eligible eBGP paths, this configures a maximum of four paths on supported Cisco IOS implementations.

For iBGP multipath, Cisco IOS platforms may provide a separate `maximum-paths ibgp` configuration.

The exact BGP multipath requirements vary by vendor and platform. Depending on implementation, paths may need matching attributes such as AS_PATH length, origin, MED, or other selected attributes.

Some platforms also support relaxed multipath rules.

**Verify BGP paths**

```cisco
show ip bgp
show ip route bgp
show ip bgp summary
```

**Important:** BGP multipath does not mean that all BGP routes are automatically installed. The paths must meet the implementation's multipath eligibility criteria.

# 🗁 ECMP Troubleshooting

ECMP problems can occur when multiple paths are expected but only one is installed or used.

**Check the routing table**

Linux:

```bash
ip route show
ip route get 192.168.50.10
```

MikroTik:

```routeros
/routing route print detail
```

Cisco IOS:

```cisco
show ip route
show ip cef 192.168.50.10
```

**Check interface status**

Linux:

```bash
ip link show
ip addr show
```

MikroTik:

```routeros
/interface print
/ip address print
```

Cisco IOS:

```cisco
show ip interface brief
```

**Check next-hop reachability**

Linux:

```bash
ip neigh show
ping 10.0.12.2
ping 10.0.13.2
```

MikroTik:

```routeros
/ping 10.0.12.2
/ping 10.0.13.2
```

Cisco IOS:

```cisco
ping 10.0.12.2
ping 10.0.13.2
```

**Check traffic distribution**

Generate multiple independent traffic flows and inspect interface counters.

Linux:

```bash
ip -s link show
```

MikroTik:

```routeros
/interface print stats
```

Cisco IOS:

```cisco
show interfaces counters
```

The exact command for interface counters varies by platform.

**Common ECMP problems**

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| Only one path installed | Routes do not qualify as equal cost | Compare route preference and metrics |
| One next hop is inactive | Gateway is unreachable | Verify interface and next-hop connectivity |
| Traffic uses only one link | Hashing maps flows to one path | Test with multiple independent flows |
| Uneven traffic distribution | Hash collision or unequal flow sizes | Inspect hash behavior and traffic patterns |
| Packet reordering | Per-packet distribution or changing paths | Prefer per-flow forwarding |
| Traffic stops after failure | Slow failure detection or stale next-hop state | Review routing convergence and failure detection |
| One-way connectivity | Missing return route | Verify routing in both directions |
| BGP multipath not installed | BGP path eligibility rules not satisfied | Inspect BGP attributes and multipath configuration |

# 🗁 Best Practices

* Prefer per-flow load balancing for general-purpose IP traffic.
* Verify that multiple next hops are actually installed in the forwarding table.
* Ensure all ECMP next hops are reachable.
* Use routing protocols with appropriate convergence and failure-detection mechanisms.
* Monitor interface utilization and packet loss.
* Understand the hashing algorithm used by the router.
* Do not assume equal traffic distribution merely because paths have equal cost.
* Test path failure and recovery before production deployment.
* Verify return-path routing to avoid asymmetric connectivity problems.
* Consider firewall and connection-tracking behavior when traffic can use different paths.
* Configure sufficient routing and forwarding capacity for the expected number of paths.

# 🗁 Practical Labs

**Lab: Configure Static ECMP on Linux**

Objective: Create a route with two equal-cost next hops.

Tasks:

* Configure two router-facing interfaces.
* Assign IP addresses.
* Add a multipath route using `ip route`.
* Verify the routing table.
* Generate multiple traffic flows.
* Inspect interface counters.

Expected result:

The Linux router should have a multipath route containing both configured next hops.

**Lab: Configure OSPF ECMP**

Objective: Learn how OSPF installs equal-cost paths.

Tasks:

* Build a four-router topology.
* Create two paths between the source and destination.
* Configure OSPF on every router.
* Adjust interface costs to make both paths equal.
* Verify the routing table.
* Change one interface cost.
* Observe how the installed paths change.

**Lab: Test ECMP Failover**

Objective: Observe routing behavior after a path failure.

Tasks:

* Configure two equal-cost paths.
* Generate continuous traffic.
* Disable one path.
* Observe route changes.
* Measure packet loss.
* Restore the failed path.
* Verify whether the router reinstalls both paths.

**Lab: Compare Per-Flow and Per-Packet Distribution**

Objective: Understand traffic distribution and packet ordering.

Tasks:

* Configure a topology with two paths having different delays.
* Generate multiple TCP and UDP flows.
* Inspect the selected next hops.
* Compare per-flow and per-packet forwarding where supported.
* Observe packet ordering and application performance.

# 🗁 Interview Questions

**What is ECMP?**

ECMP is a routing technique that allows a router to use multiple equal-cost paths to reach the same destination.

**Why is ECMP used?**

It improves path utilization, provides redundancy, and can increase aggregate network throughput.

**What is the difference between ECMP and ordinary routing?**

Ordinary best-path routing may select one preferred next hop, while ECMP can install multiple eligible equal-cost next hops.

**What is per-flow load balancing?**

It uses packet or flow information to consistently select a next hop for packets belonging to the same flow.

**What is per-packet load balancing?**

It distributes individual packets across multiple paths.

**Why is per-flow load balancing commonly preferred?**

It generally preserves packet ordering within a flow and reduces packet reordering.

**Does ECMP guarantee equal traffic distribution?**

No. Hashing, flow sizes, and traffic patterns can result in uneven utilization.

**Can OSPF support ECMP?**

Yes. OSPF can install multiple equal-cost shortest paths, subject to platform limits and configuration.

**Can BGP support ECMP?**

Yes. BGP multipath allows multiple eligible BGP paths to be installed in the forwarding table.

**What is the difference between ECMP and LACP?**

ECMP operates at Layer 3 using multiple routed next hops. LACP negotiates Layer 2 link aggregation between connected devices.

**Does ECMP provide automatic failover?**

It can provide path redundancy, but actual failover depends on failure detection, route convergence, and forwarding-table updates.

**Can ECMP use unequal-cost paths?**

Standard ECMP uses equal-cost paths. Unequal-cost load balancing requires additional platform-specific mechanisms.

**What happens when one ECMP path fails?**

The failed next hop is removed from the eligible forwarding set, and traffic can continue through the remaining available paths.

# 🗁 Summary

ECMP allows routers to use multiple equal-cost paths to reach the same destination.

It improves network utilization, increases aggregate forwarding capacity across multiple paths, and provides routing redundancy.

Important concepts include:

* Equal-cost route selection.
* Multiple next-hop forwarding.
* Per-flow and per-packet load balancing.
* Hash-based path selection.
* OSPF and BGP multipath.
* Route failure detection and convergence.
* Differences between ECMP and Layer 2 link aggregation.

ECMP is an important technique in modern IP networks, particularly in data centers, enterprise networks, and service-provider environments where multiple routed paths are available.

# 🗁 References

* [RFC 2991 – Multipath Issues in Unicast and Multicast Next-Hop Selection](https://datatracker.ietf.org/doc/html/rfc2991)
* [RFC 2992 – Analysis of an Equal-Cost Multi-Path Algorithm](https://datatracker.ietf.org/doc/html/rfc2992)
* [RFC 2328 – OSPF Version 2](https://datatracker.ietf.org/doc/html/rfc2328)
* [RFC 4271 – A Border Gateway Protocol 4 (BGP-4)](https://datatracker.ietf.org/doc/html/rfc4271)
* [Linux ip-route Documentation](https://man7.org/linux/man-pages/man8/ip-route.8.html)
* [FRRouting Documentation](https://docs.frrouting.org/)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [Cisco IP Routing Documentation](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-software/products-installation-and-configuration-guides-list.html)
