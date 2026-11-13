# Administrative Distance and Routing Metrics

Administrative Distance (AD) and routing metrics are values used by routers to determine which route should be preferred when multiple routes are available to reach a destination.

Although they are related, they serve different purposes:

* **Administrative Distance (AD):** Represents the preference of a route based on its source, such as static routing, OSPF, or RIP.
* **Routing Metric:** Represents the cost assigned to a route by a routing protocol or routing system.
* **Route Preference:** Describes which route a router selects when multiple candidate routes are available.

Understanding these concepts is essential for designing routing policies, configuring backup routes, troubleshooting route selection, and working with multiple routing protocols.

# 🗁 Routing Preference

A router may learn multiple routes to the same destination from different sources.

For example, a router might learn a route through:

* A directly connected network.
* A manually configured static route.
* OSPF.
* BGP.
* RIP.

The router must determine which route to install in its routing table and use for packet forwarding.

Route selection depends on several factors, including:

* Destination prefix length.
* Route source and administrative distance.
* Protocol-specific metric.
* Routing protocol best-path rules.
* Platform-specific route-selection behavior.

A simplified route-selection process is:

```text
              Candidate Routes
                     |
                     v
             Longest Prefix Match
                     |
                     v
          Compare Route Preference
                     |
                     v
          Compare Protocol Metrics
                     |
                     v
          Install Best Route(s)
                     |
                     v
              Forward Packets
```

This is a conceptual overview. Actual route selection differs between operating systems and routing platforms.

# 🗁 Administrative Distance

Administrative Distance (AD) is a vendor-specific concept used by some routing platforms to express the relative preference of routes learned from different sources.

It helps a router choose between routes to the same destination prefix when those routes originate from different routing sources.

A lower administrative distance generally represents a more preferred route on Cisco IOS and MikroTik RouterOS.

For example:

| Route Source | Administrative Distance |
| --- | ---: |
| Static route | 1 |
| OSPF | 110 |
| RIP | 120 |

These are common Cisco IOS default values, not universal values across all vendors.

If a router learns the same destination prefix through a static route and OSPF, the static route is normally preferred because its administrative distance is lower.

### 🖆 Important Characteristics

* AD represents route-source preference.
* It is not a measurement of bandwidth, latency, or hop count.
* Lower values generally indicate higher preference on platforms that use this model.
* AD is commonly used to compare routes to the same destination prefix.
* Different vendors and platforms may implement route preference differently.

### 🖆 Example

Suppose a router learns `192.168.10.0/24` through two sources:

| Route Source | Destination | AD |
| --- | --- | ---: |
| Static | `192.168.10.0/24` | 1 |
| OSPF | `192.168.10.0/24` | 110 |

The static route is preferred under the usual Cisco-style administrative-distance model.

```text
                  Router
                     |
          +----------+----------+
          |                     |
     Static Route           OSPF Route
       AD = 1                 AD = 110
          |                     |
          +----------+----------+
                     |
                     v
             Preferred Route
               Static Route
```

The OSPF route may remain available as a candidate, but it is not necessarily installed as the active route while the preferred static route is valid.

# 🗁 Routing Metrics

A routing metric is a value used to evaluate the relative cost of routes within a routing system.

Routing protocols use metrics to determine which path is preferable according to their own algorithms.

Different protocols calculate metrics differently.

Common metric factors include:

* Hop count.
* Bandwidth.
* Delay.
* Link cost.
* Reliability.
* Load.
* Policy attributes.

A metric does not have one universal meaning. Its interpretation depends on the routing protocol or routing implementation.

### 🖆 Example

Suppose a router learns two routes to the same destination through OSPF:

| Path | OSPF Cost |
| --- | ---: |
| Path A | 20 |
| Path B | 50 |

OSPF normally prefers Path A because its total cost is lower.

```text
                 Destination
                      |
             +--------+--------+
             |                 |
          Path A             Path B
         Cost 20            Cost 50
             |                 |
             +--------+--------+
                      |
                      v
               Preferred Path
                   Path A
```

The cost is calculated according to the OSPF configuration and the accumulated costs along the path.

# 🗁 Administrative Distance vs Routing Metric

Administrative distance and routing metrics are different concepts.

| Characteristic | Administrative Distance | Routing Metric |
| --- | --- | --- |
| Purpose | Compare route sources | Compare paths within a routing system |
| Scope | Primarily between route sources | Defined by a routing protocol or routing system |
| Meaning | Route-source preference | Path cost or preference |
| Example | Static vs OSPF | OSPF Path A vs Path B |
| Lower value | Usually preferred on Cisco-style platforms | Depends on protocol; often preferred |
| Vendor behavior | Vendor-specific | Protocol-specific and implementation-dependent |

### 🖆 Example

A router learns `10.10.0.0/16` through two routing protocols:

| Route Source | Administrative Distance | Metric |
| --- | ---: | ---: |
| OSPF | 110 | 30 |
| RIP | 120 | 2 |

Under the Cisco IOS default administrative-distance model, OSPF is preferred because its administrative distance is lower.

The RIP metric of 2 does not make it preferable to OSPF's metric of 30. These metrics belong to different routing protocols and are not directly comparable.

The router first compares route-source preference according to its platform's rules.

# 🗁 Longest Prefix Match and Route Selection

Longest Prefix Match (LPM) is a fundamental IP forwarding rule.

When a router receives a packet, it looks for matching destination prefixes in its routing table.

The route with the longest matching prefix is selected because it represents the most specific match.

This happens before administrative distance or metric comparisons between routes with different prefix lengths.

### 🖆 Example

Consider the following routing table:

| Destination Prefix | Next Hop |
| --- | --- |
| `10.0.0.0/8` | `192.168.1.1` |
| `10.10.0.0/16` | `192.168.2.1` |
| `10.10.10.0/24` | `192.168.3.1` |

A packet destined for `10.10.10.50` matches all three prefixes.

The router selects:

```text
10.10.10.0/24
```

because `/24` is more specific than `/16` and `/8`.

### 🖆 Important Rule

A route with a higher administrative distance can still be selected over a route with a lower administrative distance if it has a more specific matching prefix.

For example:

| Prefix | Source | AD |
| --- | --- | ---: |
| `10.0.0.0/8` | Static | 1 |
| `10.10.10.0/24` | OSPF | 110 |

For destination `10.10.10.50`, the `/24` route wins through Longest Prefix Match.

Administrative distance does not override the more specific prefix in this situation.

# 🗁 Common Routing Protocol Metrics

Different routing protocols use different mechanisms to evaluate paths.

### 🖆 RIP Metric

RIP uses hop count as its routing metric.

* Each router crossed adds one hop.
* A lower hop count is preferred.
* The maximum usable hop count is 15.
* A metric of 16 represents an unreachable destination.

Example:

| Path | Hop Count |
| --- | ---: |
| Path A | 2 |
| Path B | 4 |
| Path C | 16 |

RIP prefers Path A.

RIP does not consider bandwidth or latency when calculating its standard hop-count metric.

### 🖆 OSPF Metric

OSPF uses cost as its routing metric.

* Lower cost is preferred.
* Interface costs contribute to the total path cost.
* The total path cost is calculated across the route.
* Cost can be configured manually.

A common Cisco OSPF reference-bandwidth calculation is:

```text
OSPF Cost = Reference Bandwidth / Interface Bandwidth
```

For example, with a reference bandwidth of 100 Mbps:

```text
Reference Bandwidth = 100 Mbps
Interface Bandwidth = 10 Mbps

OSPF Cost = 100 / 10

OSPF Cost = 10
```

The exact cost calculation and default reference bandwidth depend on the platform and configuration. Modern networks should configure an appropriate reference bandwidth consistently across OSPF routers.

### 🖆 EIGRP Metric

EIGRP uses a composite metric.

Its traditional metric calculation can consider:

* Minimum bandwidth along the path.
* Cumulative delay.
* Reliability.
* Load.
* Configured K-values.

By default, bandwidth and delay are the primary metric components.

EIGRP is associated with Cisco and is not an IETF-standardized open routing protocol in the same way as OSPF or RIP.

### 🖆 BGP Path Selection

BGP does not use a simple universal scalar metric like RIP hop count or OSPF cost.

It selects paths using path attributes and implementation-specific best-path rules.

Important BGP attributes and decision factors include:

* Local Preference.
* AS_PATH.
* ORIGIN.
* MED.
* eBGP versus iBGP path type.
* IGP cost to the BGP next hop.
* Vendor-specific tie-breakers.

For example, a common policy is to prefer a path with a higher Local Preference within an autonomous system.

BGP route selection is policy-driven and can be more complex than comparing one numerical metric.

# 🗁 Floating Static Routes

A floating static route is a backup static route configured with a higher administrative distance than the primary route.

It remains inactive while a more preferred route to the same destination is available.

If the primary route disappears, the backup route can become active.

### 🖆 Example

A router has two possible paths to `192.168.50.0/24`:

| Route | Next Hop | Administrative Distance |
| --- | --- | ---: |
| Primary static route | `10.0.0.1` | 1 |
| Backup static route | `10.0.0.2` | 10 |

The primary route is preferred because its administrative distance is lower.

If the primary route is withdrawn, the backup route may become active.

```text
                  Router
                     |
             +-------+-------+
             |               |
          Primary          Backup
          AD = 1           AD = 10
             |               |
             v               v
          10.0.0.1         10.0.0.2
             |               |
             +-------+-------+
                     |
                     v
              Destination Network
               192.168.50.0/24
```

### 🖆 Cisco IOS Example

```cisco
ip route 192.168.50.0 255.255.255.0 10.0.0.1 1
ip route 192.168.50.0 255.255.255.0 10.0.0.2 10
```

The second route has a higher administrative distance and acts as a backup.

### 🖆 Important Consideration

A floating static route does not automatically guarantee end-to-end failover.

The router must be able to detect the relevant failure. If the next-hop interface remains operational while the remote destination becomes unreachable, a simple static route may remain installed.

Tracking mechanisms, recursive next-hop validation, routing protocols, or other failure-detection techniques may be needed.

# 🗁 Equal-Cost Routes

Equal-Cost Multipath (ECMP) allows a router to use multiple paths when they have equivalent route preference and cost according to the platform's route-selection rules.

Example:

| Path | Destination | Cost |
| --- | --- | ---: |
| Path A | `192.168.100.0/24` | 20 |
| Path B | `192.168.100.0/24` | 20 |

Both paths may be installed in the routing table.

```text
                 Router
                    |
          +---------+---------+
          |                   |
       Path A               Path B
      Cost 20              Cost 20
          |                   |
          +---------+---------+
                    |
                    v
             Destination Network
             192.168.100.0/24
```

ECMP can provide:

* Load distribution.
* Increased aggregate path capacity.
* Redundancy.
* Improved utilization of multiple links.

Traffic distribution commonly uses hashing based on packet or flow fields.

A single flow generally remains on one selected path to avoid packet reordering. Therefore, ECMP does not necessarily combine the bandwidth of all paths for one individual connection.

ECMP behavior depends on the routing protocol and platform.

# 🗁 Administrative Distance in Cisco IOS

Cisco IOS uses administrative distance to compare routes learned from different sources.

The following table shows common Cisco IOS default administrative distances:

| Route Source | Administrative Distance |
| --- | ---: |
| Directly connected | 0 |
| Static route | 1 |
| EIGRP summary route | 5 |
| eBGP | 20 |
| EIGRP internal | 90 |
| IGRP | 100 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP external | 170 |
| iBGP | 200 |
| Unknown or unusable | 255 |

These values are Cisco IOS defaults. Other platforms can use different values and route-selection rules.

### 🖆 View the Routing Table

```cisco
show ip route
```

### 🖆 View a Specific Destination

```cisco
show ip route 192.168.10.0
```

### 🖆 Configure a Static Route with a Custom Distance

```cisco
ip route 192.168.10.0 255.255.255.0 192.168.1.1 10
```

The final value `10` sets the administrative distance to 10.

### 🖆 Configure a Floating Static Route

```cisco
ip route 192.168.10.0 255.255.255.0 192.168.1.1 1
ip route 192.168.10.0 255.255.255.0 192.168.2.1 10
```

The second route acts as a less-preferred backup route.

# 🗁 Route Distance in MikroTik RouterOS

MikroTik RouterOS uses the `distance` property to represent route preference.

A lower distance is generally preferred when comparing routes to the same destination prefix.

RouterOS v7 supports route distance configuration for static routes and routing protocols.

### 🖆 View the Routing Table

```routeros
/ip route print
```

### 🖆 Configure a Static Route

```routeros
/ip route add dst-address=192.168.10.0/24 gateway=192.168.1.1 distance=1
```

### 🖆 Configure a Backup Static Route

```routeros
/ip route add dst-address=192.168.10.0/24 gateway=192.168.2.1 distance=10
```

The route with distance `1` is preferred over the route with distance `10`, assuming the routes are otherwise comparable and eligible.

### 🖆 View Detailed Route Information

```routeros
/ip route print detail
```

### 🖆 Understand Route Flags

RouterOS route flags provide information about route state and origin.

Common flags include:

| Flag | Meaning |
| --- | --- |
| D | Dynamic |
| A | Active |
| c | Connected |
| s | Static |
| o | OSPF |
| b | BGP |

The exact flags displayed depend on the route type and RouterOS version.

### 🖆 Important Considerations

* Route distance is not the same as an OSPF cost or BGP path attribute.
* A lower distance does not override Longest Prefix Match.
* Multiple equal-preference routes may be installed for ECMP.
* A route must be valid and eligible before it can become active.
* Gateway reachability and recursive next-hop resolution affect route usability.

# 🗁 Route Metrics in Linux

Linux uses route metrics, also called route priorities, to influence route selection among otherwise comparable routes.

Linux does not implement Cisco-style administrative distance as a universal routing-table field.

The `metric` value in the Linux routing table is a route priority used by the kernel's routing implementation.

A lower metric is generally preferred among routes with the same destination prefix and comparable route characteristics.

### 🖆 View the Routing Table

```bash
ip route show
```

Example:

```text
default via 192.168.1.1 dev eth0 metric 100
default via 192.168.2.1 dev eth1 metric 200
```

The default route through `eth0` has a lower metric and is normally preferred.

### 🖆 Add a Route with a Metric

```bash
sudo ip route add 192.168.50.0/24 via 192.168.1.1 dev eth0 metric 100
```

### 🖆 Add an Alternative Route

```bash
sudo ip route add 192.168.50.0/24 via 192.168.2.1 dev eth1 metric 200
```

### 🖆 Inspect Route Selection

```bash
ip route get 192.168.50.10
```

This command displays the route the Linux kernel would use to reach the specified destination, subject to the current routing rules and policy-routing configuration.

### 🖆 Display Routes with Metrics

```bash
ip -details route show
```

### 🖆 Important Considerations

* Linux route metrics are not Cisco administrative distances.
* Lower metrics generally indicate higher priority for otherwise comparable routes.
* Policy routing rules can select different routing tables before route lookup.
* Multiple nexthops can be configured for multipath routing.
* Network managers such as NetworkManager and systemd-networkd may manage persistent route configuration separately from temporary `ip route` changes.

# 🗁 Practical Examples

### 🖆 Comparing OSPF and RIP

A router learns the same destination prefix through OSPF and RIP.

| Protocol | Cisco IOS Default AD | Protocol Metric |
| --- | ---: | ---: |
| OSPF | 110 | Cost 30 |
| RIP | 120 | 2 hops |

OSPF is preferred under the Cisco IOS default administrative-distance model.

The metric values are not directly comparable because they represent different calculations.

### 🖆 Configuring a Backup Default Route

A router uses one internet connection as its primary path and another as a backup.

Cisco IOS example:

```cisco
ip route 0.0.0.0 0.0.0.0 203.0.113.1 1
ip route 0.0.0.0 0.0.0.0 198.51.100.1 10
```

The second route has a higher administrative distance.

The primary route is preferred while it remains valid. A separate tracking or failure-detection mechanism may be needed to detect failures beyond the directly connected next hop.

### 🖆 Comparing Linux Default Routes

Suppose Linux has two default routes:

```text
default via 192.168.1.1 dev eth0 metric 100
default via 192.168.2.1 dev eth1 metric 200
```

The route through `eth0` is normally preferred because it has a lower metric.

Inspect the selected route:

```bash
ip route get 1.1.1.1
```

### 🖆 MikroTik Backup Default Route

RouterOS v7 example:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1
/ip route add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=10
```

The first route has higher preference.

For reliable internet failover, configure an appropriate gateway reachability or recursive-routing strategy rather than relying only on the immediate gateway interface state.

# 🗁 Best Practices

* Understand the route-selection rules of the platform you are configuring.
* Distinguish administrative distance from protocol-specific metrics.
* Remember that Longest Prefix Match takes precedence over route-source preference.
* Use floating static routes when a less-preferred backup path is appropriate.
* Configure failure detection when backup routes must respond to remote path failures.
* Use consistent OSPF reference bandwidth across routers in the same routing domain.
* Avoid comparing metrics from different routing protocols as if they shared the same scale.
* Use ECMP when multiple equal-cost paths are intended to carry traffic.
* Verify route installation and next-hop resolution after changing route preferences.
* Document custom route distances and metrics to make troubleshooting easier.

# 🗁 Common Mistakes

| Mistake | Correct Understanding |
| --- | --- |
| AD and metric are identical. | AD compares route sources; metrics evaluate paths according to a routing system. |
| The lowest metric always wins. | The metric is evaluated within the applicable route-selection rules. |
| AD always overrides prefix length. | Longest Prefix Match takes precedence across different prefix lengths. |
| All vendors use the same AD values. | Administrative-distance defaults are vendor-specific. |
| OSPF cost and RIP hop count can be compared directly. | Each protocol uses its own metric calculation. |
| A floating static route guarantees failover. | Failure detection and route validity must be considered. |
| ECMP combines all link bandwidth for one connection. | Traffic is commonly distributed across flows using hashing. |
| Linux metric is the same as Cisco AD. | Linux route metrics are route priorities, not Cisco-style administrative distances. |
| A route with a lower distance is always active. | Route validity, prefix specificity, policy rules, and platform behavior also matter. |

# 🗁 Interview Questions

### 🖆 What is administrative distance?

Administrative distance is a vendor-specific route-source preference value used by some routing platforms to choose between routes to the same destination prefix.

### 🖆 What is a routing metric?

A routing metric is a value used by a routing protocol or routing system to evaluate the relative cost or preference of available paths.

### 🖆 What is the difference between administrative distance and metric?

Administrative distance compares route sources, while a routing metric evaluates paths according to a routing protocol's calculation.

### 🖆 Which route is preferred: a static route with AD 1 or an OSPF route with AD 110?

On Cisco IOS, the static route is normally preferred when both routes have the same destination prefix and are eligible.

### 🖆 Does a lower administrative distance always win?

No. Longest Prefix Match takes precedence when matching routes have different prefix lengths. Administrative distance is considered according to the platform's route-selection rules.

### 🖆 What is a floating static route?

A floating static route is a backup static route configured with a higher administrative distance or route distance than the preferred route.

### 🖆 What metric does RIP use?

RIP uses hop count. A maximum of 15 hops is usable; 16 represents an unreachable destination.

### 🖆 What metric does OSPF use?

OSPF uses cost. The path with the lowest total cost is normally preferred.

### 🖆 Does BGP use a simple numerical metric?

No. BGP uses path attributes and a best-path decision process, including Local Preference, AS_PATH, ORIGIN, MED, and other factors.

### 🖆 What is ECMP?

Equal-Cost Multipath allows multiple equivalent paths to be installed and used for traffic forwarding.

### 🖆 What does the Linux route metric represent?

It represents route priority used by the Linux routing implementation when comparing otherwise comparable routes.

### 🖆 How can you inspect the selected route in Linux?

Use:

```bash
ip route get <destination-ip>
```

# 🗁 Summary

Administrative distance and routing metrics help routers select preferred paths when multiple routes are available.

Key concepts:

* Administrative distance compares route sources on platforms that implement it.
* Routing metrics evaluate path cost according to a routing protocol or routing system.
* Lower administrative distance is generally preferred on Cisco IOS and MikroTik RouterOS.
* Lower metrics are preferred by protocols such as RIP and OSPF, according to their respective calculations.
* Longest Prefix Match takes precedence over administrative distance and metrics when comparing different prefix lengths.
* Floating static routes provide a less-preferred backup route.
* ECMP allows multiple equal-cost paths to be used.
* Cisco IOS, MikroTik RouterOS, and Linux have different route-preference implementations.

Understanding these differences makes it easier to configure routing policies, implement failover, and troubleshoot complex routing environments.

# 🗁 References

* [Cisco — IP Routing: Administrative Distance](https://www.cisco.com/c/en/us/support/docs/ip/enhanced-interior-gateway-routing-protocol-eigrp/8651-21.html)
* [RFC 2328 — OSPF Version 2](https://www.rfc-editor.org/rfc/rfc2328)
* [RFC 2453 — RIP Version 2](https://www.rfc-editor.org/rfc/rfc2453)
* [RFC 4271 — Border Gateway Protocol 4](https://www.rfc-editor.org/rfc/rfc4271)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [Linux ip-route Manual](https://man7.org/linux/man-pages/man8/ip-route.8.html)
* [Linux ip-rule Manual](https://man7.org/linux/man-pages/man8/ip-rule.8.html)
