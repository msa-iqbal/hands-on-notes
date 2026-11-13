# Routing Information Protocol (RIP)

Routing Information Protocol (RIP) is a distance-vector dynamic routing protocol used by routers to exchange routing information and automatically discover network paths.

RIP selects routes based on **hop count**, which represents the number of routers a packet must cross to reach a destination network.

RIP is one of the earliest interior gateway protocols (IGPs). Although it is largely considered a legacy protocol, it remains useful for understanding distance-vector routing, studying routing fundamentals, and maintaining older network environments.

RIP operates within an Autonomous System (AS) and supports automatic route advertisement, route updates, and network convergence.

# 🗁 Introduction to RIP

RIP is a distance-vector routing protocol that allows routers to share information about reachable networks with neighboring routers.

Each router maintains a routing table containing destination networks, next-hop information, outgoing interfaces, and routing metrics.

Instead of maintaining a complete network topology, a RIP router primarily learns routes from its directly connected RIP neighbors.

For example:

```text
Network A
   |
 Router A
   |
 Router B
   |
 Router C
   |
Network B
```

Router A can learn how to reach Network B through Router B and Router C.

The route has a hop count of two because the packet crosses two routers.

**Important characteristics:**

| Characteristic | Description |
| --- | --- |
| Routing type | Dynamic routing |
| Algorithm | Distance-vector |
| Metric | Hop count |
| Maximum usable hop count | 15 |
| Unreachable metric | 16 |
| Administrative domain | Interior Gateway Protocol |
| RIPv1 transport | UDP port 520 |
| RIPv2 transport | UDP port 520 |
| RIPng transport | UDP port 521 |
| RIPv2 multicast | `224.0.0.9` |
| RIPng multicast | `ff02::9` |
| Standard | RFC 2453 for RIPv2 |

# 🗁 How RIP Works

RIP routers periodically exchange their routing information with neighboring routers.

A router advertises destination networks and their associated hop-count metrics.

When a router receives an update, it:

* Examines the advertised destination networks.
* Adds one hop to the received metric.
* Compares the resulting metric with its existing route.
* Installs a better route when appropriate.
* Updates its routing information and advertises changes according to RIP's update mechanisms.

Consider the following topology:

```text
        Network A
            |
         Router A
            |
         Router B
            |
         Router C
            |
        Network B
```

Assume Network B is directly connected to Router C.

| Router | Distance to Network B |
| --- | ---: |
| Router C | 1 |
| Router B | 2 |
| Router A | 3 |

The directly connected network is advertised with metric 1 in conventional RIP routing updates.

Each receiving router increases the advertised metric by one.

**Example route propagation:**

```text
Router C:
  Network B = 1

Router B:
  Network B = 2

Router A:
  Network B = 3
```

RIP routers exchange routes with neighbors, allowing routing information to propagate throughout the RIP domain.

# 🗁 RIP Versions

RIP has three major versions:

* RIPv1
* RIPv2
* RIPng

| Feature | RIPv1 | RIPv2 | RIPng |
| --- | --- | --- | --- |
| IP version | IPv4 | IPv4 | IPv6 |
| Routing type | Distance-vector | Distance-vector | Distance-vector |
| Metric | Hop count | Hop count | Hop count |
| Classless routing | No | Yes | Yes |
| VLSM support | No | Yes | Yes |
| CIDR support | No | Yes | Yes |
| Authentication | No standard authentication mechanism | Yes | Not built into the base protocol |
| Updates | Broadcast | Multicast | Multicast |
| UDP port | 520 | 520 | 521 |
| Multicast address | Not used | `224.0.0.9` | `ff02::9` |

# 🗁 RIPv1

RIPv1 is the original version of RIP.

It is a classful routing protocol, meaning it does not include subnet mask information in its routing updates.

Consequently, RIPv1 does not properly support VLSM or classless routing.

**Characteristics:**

* Supports IPv4.
* Uses hop count as its metric.
* Does not carry subnet mask information.
* Uses broadcast updates.
* Does not support standard RIP authentication.
* Cannot properly advertise networks with different subnet masks within the same classful network boundary.

RIPv1 sends routing updates using the IPv4 limited broadcast address:

```text
255.255.255.255
```

Because of its classful behavior and limitations, RIPv1 is obsolete for most modern network designs.

# 🗁 RIPv2

RIPv2 improves upon RIPv1 by introducing classless routing support and additional routing information.

Unlike RIPv1, RIPv2 includes subnet mask information in its routing updates.

This allows it to support:

* CIDR.
* VLSM.
* Discontiguous subnet designs.
* Route authentication.
* Next-hop information.
* Route tags.
* Multicast routing updates.

RIPv2 sends updates to the multicast address:

```text
224.0.0.9
```

RIPv2 is defined in RFC 2453.

It is generally preferred over RIPv1 when working with IPv4 RIP environments.

# 🗁 RIPng

RIPng stands for RIP next generation.

It extends RIP to support IPv6 routing.

RIPng is defined in RFC 2080.

**Characteristics:**

* Supports IPv6 prefixes.
* Uses hop count as its metric.
* Sends routing updates using UDP port 521.
* Uses IPv6 multicast address `ff02::9`.
* Supports IPv6 next-hop information.
* Uses link-local IPv6 addresses for RIPng neighbor communication.

RIPng does not include the same built-in authentication mechanism as RIPv2. Security must be addressed through appropriate IPv6 security mechanisms and network controls.

# 🗁 RIP Metric

RIP uses hop count as its routing metric.

A hop represents a router crossed when forwarding a packet toward its destination.

The route with the lowest hop count is preferred among otherwise comparable RIP routes.

| Hop Count | Meaning |
| --- | --- |
| 1 | One router away |
| 2 | Two routers away |
| 5 | Five routers away |
| 10 | Ten routers away |
| 15 | Maximum reachable RIP distance |
| 16 | Destination is unreachable |

**Example:**

```text
                 Router B
                /        \
               /          \
          Router A       Router C
               \          /
                \        /
                 Router D
                      |
                  Network X
```

Assume Router A learns two paths to Network X:

| Path | Hop Count |
| --- | ---: |
| Router A → Router B → Router D → Network X | 3 |
| Router A → Router C → Router D → Network X | 3 |

Both paths have the same RIP metric.

Depending on platform capabilities and configuration, the router may install both paths using Equal-Cost Multipath (ECMP).

**Important:** RIP does not consider bandwidth, latency, jitter, or link utilization when calculating its metric.

A high-speed connection and a low-speed connection can have the same RIP metric if they have the same hop count.

# 🗁 Maximum Hop Count

RIP supports a maximum usable hop count of 15.

A metric of 16 represents an unreachable destination.

This limitation prevents RIP from supporting large network topologies.

For example:

```text
Router A → Router B → Router C → Router D
```

If a destination requires more than 15 hops, RIP considers that destination unreachable.

This is commonly known as RIP's count-to-infinity limit.

# 🗁 RIP Timers

RIP uses timers to control route updates, route expiration, and route removal.

Common default timer values for traditional RIPv2 implementations are:

| Timer | Common Default | Purpose |
| --- | ---: | --- |
| Update timer | 30 seconds | Periodic routing updates |
| Timeout / Invalid timer | 180 seconds | Marks a route invalid when updates stop |
| Garbage collection timer | 120 seconds | Removes invalid routes after a waiting period |
| Triggered update | Event-driven | Advertises important route changes without waiting for the next periodic update |

Timer behavior and exact defaults can vary by implementation.

**Update Timer**

RIP routers periodically send their routing information to neighboring routers.

A common default interval is 30 seconds, with implementations typically introducing timing variation to avoid synchronized updates.

**Timeout Timer**

If a router does not receive a valid update for a route within the timeout period, the route becomes invalid.

A common default is 180 seconds.

**Garbage Collection Timer**

After a route becomes invalid, it can remain in routing advertisements with an unreachable metric before being removed from the routing table.

A common default garbage collection period is 120 seconds.

**Triggered Updates**

Triggered updates allow a router to advertise important routing changes without waiting for the next periodic update.

They help improve convergence after network changes.

# 🗁 RIP Update Mechanism

RIP uses two important update mechanisms.

**Periodic Updates**

Routers periodically send routing information to their neighbors.

For example:

```text
Router A ---- Router B ---- Router C

Every update interval:

Router A → Router B
Router B → Router A

Router B → Router C
Router C → Router B
```

**Triggered Updates**

When a significant routing change occurs, a router can immediately advertise the updated route information.

For example:

```text
Before failure:

Router A ---- Router B ---- Network X

After failure:

Router A ---- Router B     Network X
                             (Down)
```

Router B detects that Network X is unreachable and advertises the route as unreachable to its neighbors.

Triggered updates reduce the time required to distribute routing changes, although they do not guarantee instantaneous convergence.

# 🗁 Loop Prevention Mechanisms

Distance-vector protocols can experience routing loops when routers have inconsistent routing information.

RIP uses several mechanisms to reduce the possibility and duration of routing loops.

# 🗁 Split Horizon

Split horizon prevents a router from advertising a route back through the interface from which it originally learned that route.

**Example:**

```text
Router A -------- Router B -------- Router C
                    |
                 Network X
```

If Router B learns about Network X from Router A through a particular interface, it does not advertise that route back through the same interface.

This helps prevent neighboring routers from repeatedly learning incorrect routes from each other.

# 🗁 Route Poisoning

Route poisoning advertises an unavailable network with a metric of 16.

A metric of 16 means the destination is unreachable in RIP.

**Example:**

```text
Normal route:

Network X = 2

After failure:

Network X = 16
```

This informs neighboring routers that the previously reachable destination is no longer available through that route.

# 🗁 Poison Reverse

Poison reverse is a stronger form of split horizon.

Instead of simply omitting a route when advertising it back through the interface from which it was learned, the router advertises that route with an unreachable metric of 16.

**Example:**

```text
Router A -------- Router B

Router A learns Network X from Router B.

Router A advertises Network X back to Router B:

Network X = 16
```

This helps the neighbor understand that Router A must not be used as an alternative path to that destination.

# 🗁 Hold-Down

Some RIP implementations support hold-down behavior.

During a hold-down period, a router temporarily limits acceptance of certain potentially incorrect route updates after detecting a route failure.

This helps reduce instability caused by inconsistent routing information.

Hold-down behavior is implementation-dependent and should not be assumed to be identical across all RIP platforms.

# 🗁 Count-to-Infinity Problem

Count-to-infinity occurs when routers repeatedly increase their advertised distance to an unreachable destination because they incorrectly believe another router still has a valid path.

Example:

```text
Router A -------- Router B
     |                 |
     +---- Network X --+
```

If Network X becomes unavailable, Router A and Router B may temporarily advertise increasingly large hop counts for the failed destination.

RIP limits this process by defining metric 16 as infinity.

Split horizon, route poisoning, poison reverse, and triggered updates help reduce the problem.

# 🗁 RIP Authentication

RIP authentication helps protect routing updates against unauthorized or forged advertisements.

RIPv1 does not provide a standard authentication mechanism.

RIPv2 supports authentication through its authentication entry format.

RIPv2 authentication options include:

* Simple password authentication.
* Cryptographic authentication extensions, including keyed message authentication mechanisms.

Simple password authentication does not provide confidentiality because the password is not encrypted.

Where supported, cryptographic authentication should be preferred.

RIPng does not define an equivalent built-in authentication field in its base protocol.

Security for RIPng should be handled through appropriate IPv6 security mechanisms, interface controls, and network segmentation.

**Best practices:**

* Enable authentication where supported.
* Restrict RIP updates to trusted interfaces and neighbors.
* Use passive interfaces on networks that should not form RIP adjacencies.
* Apply firewall rules to restrict unauthorized RIP traffic.
* Avoid exposing RIP to untrusted networks.

# 🗁 RIP Packet Structure

RIP messages contain a command, version, and routing information entries.

A RIP packet has the following general structure:

```text
+-------------------------------+
| Command (1 byte)              |
+-------------------------------+
| Version (1 byte)              |
+-------------------------------+
| Reserved (2 bytes)            |
+-------------------------------+
| Routing Information Entry    |
+-------------------------------+
| Routing Information Entry    |
+-------------------------------+
| ...                           |
+-------------------------------+
```

A standard RIP message can contain multiple routing entries.

For RIPv2, each routing information entry includes:

| Field | Purpose |
| --- | --- |
| Address Family Identifier | Identifies the address family |
| Route Tag | Carries route information such as redistribution origin |
| IP Address | Destination network address |
| Subnet Mask | Destination network mask |
| Next Hop | Suggested next-hop address |
| Metric | RIP hop-count metric |

RIP supports two primary command types:

| Command | Meaning |
| --- | --- |
| Request | Requests routing information |
| Response | Sends routing information |

A router can request the entire routing table or specific route information.

# 🗁 RIP Ports and Multicast Addresses

| Version | IP Version | Transport | Destination |
| --- | --- | --- | --- |
| RIPv1 | IPv4 | UDP 520 | Broadcast `255.255.255.255` |
| RIPv2 | IPv4 | UDP 520 | Multicast `224.0.0.9` |
| RIPng | IPv6 | UDP 521 | Multicast `ff02::9` |

RIPv1 and RIPv2 use UDP port 520.

RIPng uses UDP port 521.

The RIPv2 multicast address is part of the IPv4 link-local control block. RIPng uses an IPv6 link-local multicast address.

These multicast destinations are intended for routers on the local link rather than for forwarding across ordinary routers.

# 🗁 RIP Network Design

Consider a small network containing three routers.

```text
             LAN A
          192.168.10.0/24
                |
             Router A
          10.0.12.1/30
                |
          10.0.12.2/30
             Router B
          10.0.23.1/30
                |
          10.0.23.2/30
             Router C
                |
             LAN C
          192.168.30.0/24
```

The routers have the following networks:

| Router | Connected Networks |
| --- | --- |
| Router A | `192.168.10.0/24`, `10.0.12.0/30` |
| Router B | `10.0.12.0/30`, `10.0.23.0/30` |
| Router C | `10.0.23.0/30`, `192.168.30.0/24` |

With RIP enabled, Router A can learn about `192.168.30.0/24` through Router B.

Router C can learn about `192.168.10.0/24` through Router B.

The routers exchange routing information dynamically instead of requiring administrators to configure every remote network manually.

# 🗁 Configuration Concepts

Before configuring RIP, understand these common configuration concepts:

| Concept | Description |
| --- | --- |
| Network advertisement | Identifies networks participating in RIP |
| Interface activation | Enables RIP communication on an interface |
| Passive interface | Advertises connected networks without sending routing updates through that interface |
| Authentication | Protects routing updates |
| Default route advertisement | Advertises a default route to RIP neighbors |
| Redistribution | Introduces routes learned from other sources into RIP |
| Route filtering | Controls which routes are accepted or advertised |

Configuration syntax differs between routing platforms.

The following examples use RIPv2 unless stated otherwise.

# 🗁 RIP Configuration on Linux

Linux can run RIP through FRRouting (FRR).

FRR provides routing daemons for protocols such as RIP, OSPF, and BGP.

**Install FRRouting**

On Ubuntu:

```bash
sudo apt update
sudo apt install frr frr-pythontools
```

Enable the RIP daemon.

Edit:

```bash
sudo nano /etc/frr/daemons
```

Set:

```ini
ripd=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

**Example topology**

```text
Router A -------- Router B
10.0.12.1/30     10.0.12.2/30

Router A LAN: 192.168.10.0/24
Router B LAN: 192.168.20.0/24
```

**Configure Router A**

Enter the FRR command-line interface:

```bash
sudo vtysh
```

Configure RIP:

```text
configure terminal

router rip
 version 2
 network 10.0.12.0/30
 network 192.168.10.0/24
 no auto-summary

end
write memory
```

**Configure Router B**

```bash
sudo vtysh
```

```text
configure terminal

router rip
 version 2
 network 10.0.12.0/30
 network 192.168.20.0/24
 no auto-summary

end
write memory
```

The exact interface activation and network-matching behavior depends on the FRR version and configuration model.

**Verify RIP**

Inside `vtysh`:

```text
show ip rip
show ip rip status
show ip route rip
show ip protocols
```

Display the Linux routing table:

```bash
ip route show
```

A successfully learned route may appear similar to:

```text
192.168.20.0/24 via 10.0.12.2 dev eth0 proto rip
```

The exact output depends on the interface names and FRR version.

# 🗁 RIP Configuration on MikroTik RouterOS

MikroTik RouterOS v7 supports RIP through its routing configuration.

The following example configures RIPv2 on a router.

**Example topology**

```text
Router A -------- Router B
10.0.12.1/30     10.0.12.2/30

Router A LAN: 192.168.10.0/24
Router B LAN: 192.168.20.0/24
```

**Assign IP addresses**

Router A:

```routeros
/ip address
add address=10.0.12.1/30 interface=ether1
add address=192.168.10.1/24 interface=ether2
```

Router B:

```routeros
/ip address
add address=10.0.12.2/30 interface=ether1
add address=192.168.20.1/24 interface=ether2
```

**Configure RIP on Router A**

```routeros
/routing rip instance
add name=rip-instance version=2

/routing rip interface-template
add instance=rip-instance interfaces=ether1,ether2
```

**Configure RIP on Router B**

```routeros
/routing rip instance
add name=rip-instance version=2

/routing rip interface-template
add instance=rip-instance interfaces=ether1,ether2
```

The interface-template configuration enables RIP on the specified interfaces.

For production networks, configure interface participation carefully. LAN-facing interfaces that should not send routing updates can be configured as passive where supported by the relevant RouterOS v7 release.

**Verify RIP**

```routeros
/routing rip instance print
/routing rip interface print
/routing rip neighbor print
/routing route print
```

Look for dynamically learned routes.

RouterOS v7 menu details and available properties can vary between releases. Check the installed version's CLI help when adapting the configuration.

# 🗁 RIP Configuration on Cisco IOS

Cisco IOS supports RIP through its routing configuration mode.

**Example topology**

```text
Router A -------- Router B

Router A LAN: 192.168.10.0/24
Router B LAN: 192.168.20.0/24
Transit:      10.0.12.0/30
```

**Configure Router A**

```cisco
enable
configure terminal

router rip
 version 2
 no auto-summary
 network 10.0.0.0
 network 192.168.10.0

end
write memory
```

**Configure Router B**

```cisco
enable
configure terminal

router rip
 version 2
 no auto-summary
 network 10.0.0.0
 network 192.168.20.0

end
write memory
```

Cisco IOS RIP `network` statements identify major classful networks and enable RIP on matching interfaces. They are not CIDR route declarations.

**Verify RIP**

```cisco
show ip protocols
show ip route rip
show ip rip database
show ip interface brief
```

A learned RIP route is commonly marked with `R` in the routing table.

Example:

```text
R    192.168.20.0/24 [120/1] via 10.0.12.2
```

The notation means:

* `R`: Route learned through RIP.
* `120`: Cisco IOS default administrative distance for RIP.
* `1`: RIP metric of one hop.
* `10.0.12.2`: Next-hop address.

The displayed administrative distance is Cisco IOS behavior and should not be assumed to apply to every routing platform.

# 🗁 Default Route Advertisement

A default route allows routers to forward traffic toward a destination that does not have a more specific matching route.

The IPv4 default route is:

```text
0.0.0.0/0
```

A router can advertise a default route through RIP so that neighboring routers can use it as their gateway toward external networks.

For example:

```text
LAN A
  |
Router A
  |
Router B
  |
Internet
```

Router B has internet connectivity.

Router B can advertise a default route to Router A.

Router A can then forward unknown destinations toward Router B.

Default route advertisement and origination syntax vary by implementation. It should be configured deliberately to avoid advertising an invalid or unintended gateway.

# 🗁 Route Redistribution

Route redistribution allows a router to introduce routes learned from one routing source into another routing protocol.

For example:

```text
        OSPF Domain
             |
          Router A
             |
         RIP Domain
```

Router A can redistribute selected OSPF routes into RIP.

Similarly, it can redistribute RIP routes into OSPF.

Redistribution requires careful control because it can introduce:

* Routing loops.
* Duplicate routing information.
* Unexpected route preferences.
* Suboptimal routing.
* Excessive routing updates.

Use route maps, prefix lists, route tags, and filtering mechanisms where supported.

Avoid unrestricted bidirectional redistribution between protocols without a deliberate policy.

# 🗁 RIP Troubleshooting

RIP problems can occur because of incorrect network statements, interface configuration, authentication mismatches, firewall restrictions, or routing policy.

**Check RIP process status**

Linux FRR:

```text
show ip rip status
show ip protocols
```

Cisco IOS:

```cisco
show ip protocols
show ip route rip
```

MikroTik RouterOS:

```routeros
/routing rip instance print
/routing rip neighbor print
```

**Check interface addresses**

Linux:

```bash
ip addr show
ip link show
```

Cisco IOS:

```cisco
show ip interface brief
```

MikroTik:

```routeros
/ip address print
/interface print
```

**Check routing tables**

Linux:

```bash
ip route show
```

Cisco IOS:

```cisco
show ip route
```

MikroTik:

```routeros
/routing route print
```

**Check UDP traffic**

RIPv1 and RIPv2:

```bash
sudo tcpdump -ni any udp port 520
```

RIPng:

```bash
sudo tcpdump -ni any udp port 521
```

**Check firewall rules**

Ubuntu using nftables:

```bash
sudo nft list ruleset
```

Ensure required RIP traffic is permitted on trusted routing interfaces.

**Common RIP problems**

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| No RIP neighbors | RIP not enabled on the interface | Verify interface participation |
| Routes not learned | Incorrect network configuration | Check advertised networks |
| Routes missing | Firewall blocking UDP traffic | Review firewall rules |
| Route marked unreachable | Metric reached 16 | Investigate the path |
| Routing instability | Route flapping or inconsistent updates | Inspect interface stability and updates |
| Authentication failure | Mismatched authentication settings | Verify authentication configuration |
| Incorrect next hop | Bad next-hop information or topology | Inspect route advertisements |
| Unexpected routes | Redistribution or advertisement policy | Review route filtering |

# 🗁 RIP Limitations and Best Practices

RIP is simple to configure, but its design introduces significant limitations.

**Limitations**

* Maximum usable hop count of 15.
* Relatively slow convergence compared with modern IGPs.
* Periodic routing updates consume bandwidth.
* Hop count does not represent bandwidth or latency.
* Limited scalability.
* Susceptibility to routing loops and count-to-infinity behavior.
* Limited support for complex routing policies.
* Not suitable for modern large-scale ISP backbone routing.

**Best Practices**

* Prefer RIPv2 over RIPv1 for IPv4 networks.
* Use RIPng only when an IPv6 RIP environment is specifically required.
* Enable authentication where supported.
* Configure passive interfaces on LANs that should not exchange routing updates.
* Apply route filtering and redistribution policies.
* Advertise only necessary networks.
* Monitor route changes and routing convergence.
* Avoid using RIP for large or complex network architectures.
* Prefer OSPF or IS-IS for scalable internal routing designs.

RIP is best suited to small legacy networks, controlled environments, and educational labs.

# 🗁 Practical Labs

**Lab: Configure RIPv2 Between Two Routers**

Objective: Allow two routers to exchange IPv4 routes dynamically.

Topology:

```text
192.168.10.0/24
       |
    Router A
       |
  10.0.12.0/30
       |
    Router B
       |
192.168.20.0/24
```

Tasks:

* Configure IP addresses on both routers.
* Enable RIPv2.
* Advertise connected networks.
* Verify that each router learns the remote LAN.
* Test connectivity between LAN hosts.
* Inspect routing tables.
* Capture RIP updates using tcpdump or Wireshark.

Expected result:

* Router A learns `192.168.20.0/24`.
* Router B learns `192.168.10.0/24`.
* Hosts in both LANs can communicate when forwarding and firewall policies permit.

**Lab: Observe RIP Route Changes**

Objective: Understand triggered updates and route invalidation.

Tasks:

* Build a three-router RIP topology.
* Verify that all networks are reachable.
* Disable an interface connecting two routers.
* Observe route changes.
* Inspect routing updates.
* Identify when a route becomes unreachable.
* Restore the interface and observe recovery.

**Lab: Compare RIPv1 and RIPv2**

Objective: Understand classful and classless routing behavior.

Tasks:

* Configure a network using different subnet masks within a classful address space.
* Observe RIPv1 behavior.
* Change the configuration to RIPv2.
* Verify subnet mask advertisements.
* Compare routing table entries.

**Lab: Configure RIPng**

Objective: Exchange IPv6 routes using RIPng.

Tasks:

* Configure IPv6 addresses on router interfaces.
* Enable RIPng on the relevant interfaces.
* Verify UDP port 521 traffic.
* Inspect IPv6 routing tables.
* Test IPv6 connectivity between LANs.

# 🗁 Interview Questions

**What is RIP?**

RIP is a distance-vector dynamic routing protocol that uses hop count to select routes.

**What is the maximum hop count in RIP?**

15 hops. A metric of 16 represents an unreachable destination.

**What is the difference between RIPv1 and RIPv2?**

RIPv1 is classful and does not include subnet masks in routing updates. RIPv2 is classless and supports CIDR, VLSM, authentication, and multicast updates.

**What is RIPng?**

RIPng is the IPv6 version of RIP.

**Which transport protocol and ports does RIP use?**

RIPv1 and RIPv2 use UDP port 520. RIPng uses UDP port 521.

**What is the RIPv2 multicast address?**

`224.0.0.9`.

**What is the RIPng multicast address?**

`ff02::9`.

**What is split horizon?**

Split horizon prevents a router from advertising a route back through the interface from which it learned that route.

**What is route poisoning?**

Route poisoning advertises an unreachable route using a metric of 16.

**What is poison reverse?**

Poison reverse advertises a learned route back through the receiving interface with an unreachable metric.

**What is the count-to-infinity problem?**

It is a routing instability in which routers repeatedly increase their advertised distance to an unreachable destination.

**What is a triggered update?**

A triggered update is an event-driven routing advertisement sent when an important route change occurs.

**Does RIP consider bandwidth when selecting routes?**

No. RIP uses hop count as its routing metric.

**Why is RIP unsuitable for large networks?**

Its 15-hop limit, convergence characteristics, and limited routing capabilities make it unsuitable for large and complex network environments.

**What is the administrative distance of RIP?**

It is implementation-dependent. Cisco IOS traditionally uses 120 by default, while other platforms use their own route preference mechanisms.

# 🗁 Summary

RIP is a distance-vector dynamic routing protocol that exchanges routing information between neighboring routers.

Its primary characteristics include:

* Hop-count-based route selection.
* Maximum usable metric of 15.
* Unreachable metric of 16.
* Periodic and triggered routing updates.
* Split horizon and route poisoning mechanisms.
* RIPv1 and RIPv2 for IPv4.
* RIPng for IPv6.

RIPv2 improves upon RIPv1 by supporting classless routing, VLSM, authentication, and multicast updates.

RIP remains useful for learning dynamic routing concepts and supporting legacy networks, but modern networks generally use more scalable routing protocols such as OSPF and IS-IS.

# 🗁 References

* [RFC 1058 – Routing Information Protocol](https://datatracker.ietf.org/doc/html/rfc1058)
* [RFC 2453 – RIP Version 2](https://datatracker.ietf.org/doc/html/rfc2453)
* [RFC 2080 – RIPng for IPv6](https://datatracker.ietf.org/doc/html/rfc2080)
* [RFC 4822 – RIPv2 Cryptographic Authentication](https://datatracker.ietf.org/doc/html/rfc4822)
* [FRRouting Documentation](https://docs.frrouting.org/)
* [MikroTik RouterOS Routing Documentation](https://help.mikrotik.com/docs/)
* [Cisco IOS IP Routing Documentation](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-software/products-installation-and-configuration-guides-list.html)
