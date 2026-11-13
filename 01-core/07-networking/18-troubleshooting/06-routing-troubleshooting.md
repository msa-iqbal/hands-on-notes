# Routing Troubleshooting

Routing troubleshooting is the process of identifying and resolving problems that prevent packets from reaching networks outside their local subnet.

A routing problem can occur when:

* A router does not have a route to the destination network.
* The selected route points to an unreachable next hop.
* A routing protocol fails to exchange or install routes.
* An incorrect route takes precedence over the intended route.
* Routing policies send traffic through an unexpected path.
* The return path is missing or different from the forward path.
* The routing table is correct, but packet forwarding is blocked elsewhere.

The objective is to verify the complete forwarding path, from the source host to the destination and back.

# 🗁 Routing Fundamentals

A router examines the destination IP address of an incoming packet and selects an appropriate route.

A routing table entry generally contains:

| Field | Description |
| --- | --- |
| Destination prefix | Network or host address the route matches |
| Next hop | Router to which the packet should be forwarded |
| Outgoing interface | Interface used to forward the packet |
| Metric | Cost used to compare routes within a routing protocol |
| Administrative distance | Route-source preference used by some vendors |
| Route source | Connected, static, OSPF, BGP, or another source |

Example routing table:

```text
Destination       Next Hop        Interface
192.168.10.0/24   Directly connected  eth0
192.168.20.0/24   10.0.0.2           eth1
10.0.0.0/24       Directly connected  eth1
0.0.0.0/0         10.0.0.1           eth1
```

The default route `0.0.0.0/0` matches destinations that do not have a more specific matching route.

# 🗁 Longest Prefix Match

Routers generally select the matching route with the longest prefix.

A longer prefix represents a more specific destination network.

Consider these routes:

```text
10.0.0.0/8       via 192.168.1.1
10.10.0.0/16     via 192.168.1.2
10.10.20.0/24   via 192.168.1.3
```

For a packet destined for `10.10.20.50`, the router selects:

```text
10.10.20.0/24 via 192.168.1.3
```

The `/24` route wins because it is the most specific match.

A route with a less preferred administrative distance can still be selected if it has a longer matching prefix. Longest-prefix matching is evaluated before comparing competing routes for the same destination prefix.

# 🗁 Administrative Distance vs Metric

Administrative distance and metric are different concepts.

**Administrative distance (AD)** indicates how trustworthy a route source is from a router's perspective. It is commonly used to choose between routes learned from different sources.

**Metric** represents the cost of a route according to a routing protocol or route-selection mechanism.

For example:

* OSPF uses cost.
* RIP uses hop count.
* EIGRP uses a composite metric.
* BGP uses multiple path attributes, including AS path and local preference.

Administrative distance values and route-selection behavior vary between vendors.

Do not compare metrics from different routing protocols as though they share a common scale.

# 🗁 Common Routing Problems

| Symptom | Possible cause |
| --- | --- |
| Local network works, remote network fails | Missing route or incorrect gateway |
| One subnet is unreachable | Missing or incorrect specific route |
| Traffic reaches the destination but replies fail | Missing return route or asymmetric routing |
| Internet access fails for all clients | Missing default route or upstream failure |
| Connectivity works intermittently | Route instability or ECMP behavior |
| Traffic takes an unexpected path | More-specific route or policy routing |
| Routing protocol does not install routes | Adjacency, policy, or route-selection issue |
| Traceroute stops at an intermediate hop | Filtering, rate limiting, or forwarding failure |
| Traffic loops between routers | Incorrect routing information |
| A route exists but traffic is dropped | Data-plane, firewall, ACL, or next-hop problem |

# 🗁 Troubleshooting Workflow

Follow a structured process instead of changing routes immediately.

**Verify the source device**

Check the source IP address, subnet mask, default gateway, and interface status.

```bash
ip address show
ip link show
ip route
```

Confirm that the destination is not simply unreachable because of an incorrect local network configuration.

**Check the source routing decision**

Determine which route the operating system will use for the destination.

```bash
ip route get 8.8.8.8
```

Example output:

```text
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.100
```

This indicates that the packet will use:

* Next hop: `192.168.1.1`
* Interface: `eth0`
* Source address: `192.168.1.100`

**Verify next-hop reachability**

Check whether the next-hop router can be reached.

```bash
ping -c 4 192.168.1.1
```

Inspect the neighbor table:

```bash
ip neigh show
```

An unresolved or failed neighbor entry may indicate a Layer 2 connectivity or ARP problem.

**Inspect the routing table**

```bash
ip route show
ip route show table all
ip -6 route show
```

Look for:

* Missing destination prefixes.
* Incorrect next hops.
* Unexpected default routes.
* Incorrect interface selection.
* More-specific routes overriding expected routes.
* Routes marked as unreachable, prohibit, or blackhole.

**Trace the packet path**

```bash
traceroute 8.8.8.8
```

Alternative commands:

```bash
tracepath 8.8.8.8
mtr -rw 8.8.8.8
```

A timeout at an intermediate hop does not automatically indicate a forwarding failure. Some routers filter or rate-limit diagnostic responses while continuing to forward traffic.

**Check the destination and return path**

If the packet reaches the destination but communication still fails, inspect the destination's routing table.

The destination must have a valid route back to the source network.

**Inspect routing protocols and policies**

If routes are learned dynamically, verify protocol adjacencies, route advertisements, filtering policies, and route installation.

# 🗁 Linux Routing Diagnostics

Linux provides several commands for inspecting routing decisions.

**Display IPv4 routes**

```bash
ip route
```

**Display IPv6 routes**

```bash
ip -6 route
```

**Display all routing tables**

```bash
ip route show table all
```

**Check the selected route**

```bash
ip route get 192.168.20.50
```

**Inspect routing policy rules**

```bash
ip rule show
```

**Inspect a specific routing table**

```bash
ip route show table 100
```

**Check connectivity**

```bash
ping -c 4 192.168.20.50
```

**Trace the route**

```bash
traceroute 192.168.20.50
```

**Capture routing-related traffic**

```bash
sudo tcpdump -ni any host 192.168.20.50
```

Packet captures help determine whether traffic leaves the source interface, arrives at the router, and receives a response.

# 🗁 Static Route Problems

Static routes are manually configured routes that do not automatically adapt to topology changes unless additional tracking or automation is configured.

Common problems include:

* Incorrect destination prefix.
* Incorrect next-hop address.
* Incorrect outgoing interface.
* Missing return route.
* Next hop is unreachable.
* Static route conflicts with a more-specific route.
* A stale static route remains after a topology change.

Example Linux route inspection:

```bash
ip route show
```

Example static route:

```bash
sudo ip route add 192.168.20.0/24 via 192.168.1.254
```

Verify the result:

```bash
ip route get 192.168.20.10
```

Remove an incorrect route:

```bash
sudo ip route del 192.168.20.0/24
```

These commands modify the live routing table. Persistent configuration must also be updated through the appropriate network configuration system.

# 🗁 Default Route Problems

A default route provides a forwarding path for destinations without a more-specific matching route.

IPv4 default route:

```text
0.0.0.0/0
```

IPv6 default route:

```text
::/0
```

Common issues:

* No default route exists.
* The next-hop router is unreachable.
* The default route points to the wrong interface.
* An incorrect more-specific route overrides it.
* The upstream router does not have a route to the destination.
* The upstream network blocks or drops the traffic.

Check the default route:

```bash
ip route show default
ip -6 route show default
```

Check the selected path:

```bash
ip route get 1.1.1.1
```

A default route being present does not guarantee Internet connectivity. The next hop and upstream forwarding path must also work.

# 🗁 Dynamic Routing Protocol Problems

Dynamic routing protocols exchange routing information between routers.

Common protocols include:

* OSPF
* BGP
* RIP
* EIGRP

A routing protocol problem can prevent routes from being learned, installed, or advertised correctly.

# 🗁 OSPF Troubleshooting

OSPF routers establish neighbor relationships before exchanging link-state information and building their routing databases.

Common causes of OSPF adjacency failure:

* Area ID mismatch.
* Authentication mismatch.
* Hello or dead timer mismatch.
* Interface subnet mismatch.
* OSPF network type mismatch.
* MTU mismatch.
* Incorrect interface configuration.
* Duplicate router ID.
* ACLs blocking OSPF packets.

OSPF uses IP protocol number `89`, not TCP or UDP ports.

Useful Cisco IOS commands:

```cisco
show ip ospf neighbor
show ip ospf interface brief
show ip ospf interface
show ip route ospf
show ip protocols
```

Useful MikroTik RouterOS v7 commands:

```routeros
/routing ospf neighbor print detail
/routing ospf interface print detail
/routing ospf instance print detail
/routing route print detail
```

Check the following:

* Are expected neighbors visible?
* Are neighbors reaching the FULL state on networks where full adjacency is expected?
* Are interfaces assigned to the correct area?
* Are authentication and timers consistent?
* Are expected routes installed?
* Are routing filters preventing route installation or advertisement?

An OSPF neighbor stuck in EXSTART or EXCHANGE can indicate an MTU or database exchange problem, among other causes.

# 🗁 BGP Troubleshooting

BGP exchanges routing information between autonomous systems and is commonly used for Internet routing and inter-AS connectivity.

BGP uses TCP port `179`.

Common BGP problems:

* Incorrect peer IP address.
* Incorrect local or remote AS number.
* TCP connectivity failure.
* Authentication mismatch.
* Incorrect update source.
* Missing route to the peer.
* Incorrect address-family configuration.
* Import or export filtering.
* Prefix limits or route policy restrictions.

Cisco IOS diagnostic commands:

```cisco
show ip bgp summary
show ip bgp
show ip route bgp
show ip protocols
```

MikroTik RouterOS v7 commands:

```routeros
/routing bgp session print detail
/routing bgp connection print detail
/routing route print detail
```

Check:

* Is the BGP session established?
* Can each peer reach the other's transport address?
* Are the AS numbers correct?
* Are expected prefixes received?
* Are received prefixes accepted by routing filters?
* Are local prefixes being advertised?
* Is the selected path consistent with the routing policy?

A BGP session being established does not guarantee that useful routes are being exchanged.

# 🗁 Policy-Based Routing

Policy-based routing (PBR) selects a forwarding path based on criteria beyond the destination prefix alone.

Depending on the platform, policies may use:

* Source IP address.
* Destination IP address.
* Incoming interface.
* Packet marks.
* Protocol or port.
* Routing marks or routing tables.

Linux supports policy routing through multiple routing tables and `ip rule`.

Inspect policy rules:

```bash
ip rule show
```

Inspect all routing tables:

```bash
ip route show table all
```

Example policy-routing configuration:

```bash
sudo ip route add 192.168.20.0/24 via 192.168.1.254 table 100

sudo ip rule add from 192.168.1.100/32 table 100
```

Verify:

```bash
ip rule show
ip route show table 100
ip route get 192.168.20.10 from 192.168.1.100
```

The source address can affect the selected route. Therefore, testing only the main routing table may not reveal the actual forwarding decision.

# 🗁 Asymmetric Routing

Asymmetric routing occurs when packets travel through different paths in each direction.

Example:

```text
Forward path:
Client → Router A → Router B → Server

Return path:
Server → Router C → Router A → Client
```

Asymmetric routing can be valid in some network designs, but it may cause problems when devices expect both directions of a connection to pass through the same path.

Potential symptoms:

* Stateful firewall drops.
* NAT connection-tracking problems.
* Inconsistent packet inspection.
* Session interruptions.
* Different latency in each direction.

Troubleshooting steps:

* Trace the forward path.
* Trace the return path.
* Inspect routing tables on intermediate routers.
* Verify firewall state and NAT behavior.
* Check whether routing policies create different paths.

Do not assume that a successful forward traceroute proves the return path is correct.

# 🗁 ECMP and Multiple Routes

Equal-Cost Multi-Path (ECMP) routing allows a router to use multiple paths with equal routing preference and cost.

Example:

```text
192.168.20.0/24 via 10.0.0.2
192.168.20.0/24 via 10.0.0.3
```

Depending on the implementation, traffic may be distributed using:

* Per-flow hashing.
* Per-packet distribution.
* Other platform-specific forwarding mechanisms.

Potential problems:

* One next hop is unavailable.
* Unequal path quality.
* Different MTU values.
* Stateful firewall or NAT behavior differs between paths.
* Hashing causes some flows to use a problematic path.

Check every installed next hop rather than testing only one.

# 🗁 IPv4 and IPv6 Routing Issues

IPv4 and IPv6 maintain separate routing information and may use different gateways and policies.

IPv4 route inspection:

```bash
ip route show
ip route get 8.8.8.8
```

IPv6 route inspection:

```bash
ip -6 route show
ip -6 route get 2001:4860:4860::8888
```

Common IPv6 routing problems:

* Missing IPv6 default route.
* Incorrect Router Advertisement configuration.
* Missing IPv6 route on an upstream router.
* Incorrect next-hop link-local address.
* IPv6 forwarding disabled.
* Firewall policy blocks IPv6 traffic.

IPv6 routers commonly use link-local addresses as next hops. A link-local address is only meaningful within its associated link and interface context.

# 🗁 MikroTik RouterOS v7 Routing Diagnostics

RouterOS provides commands for examining active routes, routing protocols, and forwarding behavior.

**Display IPv4 routes**

```routeros
/ip route print detail
```

**Display IPv6 routes**

```routeros
/ipv6 route print detail
```

**Display the routing table**

```routeros
/routing route print detail
```

**Inspect routing rules**

```routeros
/routing rule print detail
```

**Test connectivity**

```routeros
/ping 8.8.8.8
```

**Trace the path**

```routeros
/tool traceroute 8.8.8.8
```

**Inspect OSPF neighbors**

```routeros
/routing ospf neighbor print detail
```

**Inspect BGP sessions**

```routeros
/routing bgp session print detail
```

**Inspect interface status**

```routeros
/interface print detail
```

When troubleshooting RouterOS, verify:

* The route is present and active.
* The selected next hop is reachable.
* The correct routing table is being used.
* Routing rules and routing marks are correct.
* Firewall rules are not dropping forwarded traffic.
* NAT rules are appropriate for the selected path.
* Dynamic routing protocols are establishing expected sessions.

A route shown in the routing table does not by itself prove that packets are successfully forwarded through the data plane.

# 🗁 Cisco IOS Routing Diagnostics

Useful commands for Cisco IOS and IOS XE environments include:

**Display the IPv4 routing table**

```cisco
show ip route
```

**Inspect a specific destination**

```cisco
show ip route 192.168.20.0
```

**Display routing protocol information**

```cisco
show ip protocols
```

**Inspect OSPF neighbors**

```cisco
show ip ospf neighbor
```

**Inspect BGP peers**

```cisco
show ip bgp summary
```

**Inspect CEF forwarding information**

```cisco
show ip cef
```

**Trace the destination**

```cisco
traceroute 8.8.8.8
```

Use these commands to compare the control-plane routing table with the forwarding information used by the router.

# 🗁 Control Plane vs Data Plane

A router's control plane handles routing protocols and calculates routing information.

The data plane forwards packets using the installed forwarding information.

These are related but distinct functions.

A route may appear in the routing table while packets still fail because of:

* Incorrect next-hop resolution.
* Interface or link failure.
* Access control lists.
* Firewall rules.
* Hardware forwarding problems.
* Incorrect policy routing.
* Encapsulation or MTU issues.

Troubleshooting should therefore verify both the route selection and actual packet forwarding.

# 🗁 Practical Troubleshooting Scenarios

**Scenario: Local network works but remote subnet fails**

Network:

```text
LAN A: 192.168.10.0/24
Router A: 192.168.10.1
Router B: 192.168.20.1
LAN B: 192.168.20.0/24
```

Troubleshooting:

* Verify the client default gateway.
* Check Router A's route to `192.168.20.0/24`.
* Check Router B's route to `192.168.10.0/24`.
* Verify the inter-router link.
* Test connectivity from each router.
* Inspect firewall and forwarding policies.

Possible cause: Router B has no return route to LAN A.

**Scenario: Internet access fails**

Troubleshooting:

* Check the client IP configuration.
* Verify the default gateway.
* Check the router's default route.
* Test the upstream next hop.
* Test an external IP address.
* Check DNS separately.
* Inspect upstream firewall and NAT configuration.

Possible cause: The router has no valid default route.

**Scenario: OSPF neighbor does not become FULL**

Troubleshooting:

* Check interface status and IP addressing.
* Verify area configuration.
* Compare hello and dead timers.
* Verify authentication settings.
* Inspect network type.
* Check MTU compatibility.
* Inspect OSPF logs and neighbor state.

Possible cause: An MTU mismatch prevents database exchange from completing.

**Scenario: BGP session remains down**

Troubleshooting:

* Verify peer IP reachability.
* Confirm TCP port `179` connectivity.
* Check local and remote AS numbers.
* Verify source interface or update source.
* Inspect authentication settings.
* Check routing to the peer address.

Possible cause: The peer address is unreachable because the required underlay route is missing.

**Scenario: Some connections work while others fail**

Troubleshooting:

* Inspect ECMP routes.
* Identify the next hop selected for each flow.
* Compare path MTUs.
* Check firewall and NAT state.
* Inspect packet captures on each path.

Possible cause: One ECMP next hop has an upstream connectivity problem.

# 🗁 Packet Capture for Routing Analysis

Packet captures help determine whether traffic reaches an interface and whether responses return.

Capture traffic to a destination:

```bash
sudo tcpdump -ni eth0 host 192.168.20.50
```

Capture ICMP traffic:

```bash
sudo tcpdump -ni any icmp
```

Capture traffic on a specific interface:

```bash
sudo tcpdump -ni eth1
```

Compare captures on ingress and egress interfaces.

Interpretation:

* Packet appears on ingress but not egress: investigate route selection, forwarding, filtering, or neighbor resolution.
* Packet appears on egress but no reply returns: investigate the next hop, destination, and return path.
* Reply reaches the router but not the source: investigate the reverse forwarding path and filtering.
* Packet uses an unexpected interface: inspect routing rules, more-specific routes, and ECMP selection.

# 🗁 Routing Troubleshooting Checklist

Use this checklist during real-world investigations.

* [ ] Verify source IP address and subnet mask.
* [ ] Verify the source default gateway.
* [ ] Confirm interface and link status.
* [ ] Inspect the routing table.
* [ ] Check longest-prefix route selection.
* [ ] Verify the selected next hop.
* [ ] Test next-hop reachability.
* [ ] Verify the outgoing interface.
* [ ] Trace the packet path.
* [ ] Check the destination routing table.
* [ ] Verify the return path.
* [ ] Inspect policy routing rules.
* [ ] Check OSPF or BGP neighbor status when applicable.
* [ ] Verify route advertisements and filtering.
* [ ] Inspect firewall and ACL policies.
* [ ] Check ECMP paths if multiple next hops exist.
* [ ] Compare control-plane routes with data-plane forwarding.
* [ ] Capture packets when the failure remains unclear.
* [ ] Document the root cause and corrective action.

# 🗁 Best Practices

* Understand the expected network topology before troubleshooting.
* Keep routing tables and network diagrams documented.
* Use consistent IP addressing and route summarization.
* Avoid unnecessary static routes in dynamic routing environments.
* Monitor routing protocol neighbor state.
* Configure route tracking where supported and appropriate.
* Validate both forward and return paths.
* Use controlled changes and maintain configuration backups.
* Avoid changing multiple routing parameters simultaneously.
* Record the route-selection behavior before and after changes.

# 🗁 Summary

Routing troubleshooting requires examining how packets are forwarded across networks.

The most important areas to investigate are:

* Routing table entries and longest-prefix matching.
* Next-hop reachability and outgoing interfaces.
* Default and static routes.
* Dynamic routing protocol adjacencies.
* Policy-based routing and ECMP.
* Forward and return paths.
* Control-plane and data-plane behavior.

A systematic approach helps isolate routing failures without introducing unnecessary configuration changes.
