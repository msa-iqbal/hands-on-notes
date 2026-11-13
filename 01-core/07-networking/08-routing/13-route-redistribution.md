# Route Redistribution

Route redistribution is the process of exchanging routing information between different routing protocols or routing sources.

It allows a router to take routes learned from one routing protocol and advertise them into another routing protocol.

For example, a router running both OSPF and RIP can redistribute OSPF routes into RIP so that routers in the RIP domain can learn how to reach networks in the OSPF domain.

Route redistribution is commonly used when connecting different routing domains, migrating between routing protocols, integrating enterprise networks, and connecting service-provider or customer routing environments.

However, redistribution requires careful planning because different routing protocols use different metrics, route-selection rules, and administrative distances.

# 🗁 Introduction to Route Redistribution

A routing protocol normally exchanges routing information with other routers running the same protocol.

For example:

* OSPF routers exchange OSPF routing information.
* RIP routers exchange RIP routing information.
* BGP routers exchange BGP routing information.

These protocols maintain their own routing information and use different mechanisms to calculate route preferences and metrics.

When two routing domains use different routing protocols, their routers do not automatically exchange all routing information.

Route redistribution solves this problem by allowing a router to inject routes learned from one routing source into another routing protocol.

**Example:**

```text
          OSPF Domain
       +---------------+
       |               |
     R1 ---- R2 ---- R3
                     |
                     |
                Redistribution
                   Router
                     |
                     |
                     R4
                     |
                RIP Domain
              +------------+
              |            |
             R5 ---------- R6
```

The redistribution router participates in both routing domains.

It can learn routes from OSPF and advertise selected routes into RIP.

As a result, routers in the RIP domain can learn how to reach networks inside the OSPF domain.

# 🗁 Why Route Redistribution Is Needed

Route redistribution is useful in several network scenarios.

**Connecting Different Routing Protocols**

An organization may use OSPF internally while a separate network uses RIP.

Redistribution allows both routing domains to exchange selected routes.

**Network Migration**

An organization may migrate from RIP to OSPF gradually.

During the migration, redistribution allows routers running both protocols to exchange routes.

**Connecting Enterprise Networks**

Different departments, subsidiaries, or acquired organizations may use different routing protocols.

Redistribution allows controlled connectivity between these routing domains.

**Connecting Internal and External Routing**

A router may exchange routes between an internal routing protocol and BGP.

For example, an enterprise edge router may advertise selected internal prefixes to an external BGP network.

**Integrating Multiple Routing Domains**

Large networks sometimes use separate routing domains for operational or administrative reasons.

Redistribution can provide controlled route exchange between them.

# 🗁 How Route Redistribution Works

Redistribution takes routes from a source routing table or protocol and introduces eligible routes into a destination routing protocol.

The general process is:

* A router learns routes from one routing protocol.
* The routes become available in its routing information base.
* A redistribution policy selects eligible routes.
* The destination routing protocol imports those routes.
* The destination protocol assigns or derives the required metric and attributes.
* The router advertises the redistributed routes to its neighbors.

**Example: OSPF into RIP**

```text
          OSPF Domain
               |
               |
          OSPF Router
               |
               |
        Redistribution Router
               |
               |
           RIP Router
               |
               |
           RIP Domain
```

Suppose the OSPF domain contains:

```text
192.168.10.0/24
192.168.20.0/24
192.168.30.0/24
```

The redistribution router imports these routes into RIP.

RIP routers can then learn those networks through their normal RIP update process.

The destination protocol does not necessarily preserve the original routing protocol's metric. It applies its own metric rules.

# 🗁 Redistribution Between Routing Protocols

Redistribution can occur between several routing sources.

| Source Protocol | Destination Protocol | Example |
| --- | --- | --- |
| OSPF | RIP | Advertise OSPF routes into RIP |
| RIP | OSPF | Advertise RIP routes into OSPF |
| OSPF | BGP | Advertise selected internal routes into BGP |
| BGP | OSPF | Import selected BGP routes into OSPF |
| Static | OSPF | Advertise manually configured routes into OSPF |
| Connected | OSPF | Advertise directly connected networks into OSPF |
| Static | BGP | Advertise selected static prefixes into BGP |

Not every routing source or protocol combination behaves identically.

The destination protocol must support the relevant redistribution operation, and the platform must have a valid route available for redistribution.

# 🗁 Types of Route Redistribution

There are two common redistribution designs.

# 🗁 One-Way Redistribution

One-way redistribution transfers routes in a single direction.

For example:

```text
        OSPF Domain
             |
             |
       Redistribution
          Router
             |
             |
         RIP Domain

OSPF -----------> RIP
```

OSPF routes are imported into RIP.

RIP routes are not imported into OSPF.

**Advantages:**

* Easier to control.
* Lower risk of routing loops.
* Simpler routing policies.
* Easier troubleshooting.

One-way redistribution is generally preferable when only one routing domain needs to learn routes from another.

# 🗁 Mutual Redistribution

Mutual redistribution transfers routes in both directions.

For example:

```text
        OSPF Domain
             |
             |
       Redistribution
          Router
             |
             |
         RIP Domain

OSPF -----------> RIP
OSPF <----------- RIP
```

The router imports OSPF routes into RIP and RIP routes into OSPF.

**Advantages:**

* Both routing domains can learn routes from each other.
* Useful during migrations and complex network integrations.

**Challenges:**

* Greater risk of routing loops.
* More complicated metric handling.
* Potential route feedback.
* More complex troubleshooting.
* Requires careful route filtering and tagging.

Mutual redistribution should be implemented only when the network design requires it.

# 🗁 Route Metrics During Redistribution

Different routing protocols use different metrics.

When a route is redistributed, the destination protocol needs a metric that it understands.

Consider these examples:

| Routing Protocol | Metric |
| --- | --- |
| RIP | Hop count |
| OSPF | Cost |
| IS-IS | Path metric |
| EIGRP | Composite metric |
| BGP | Path attributes and best-path rules |

An OSPF route uses OSPF cost, while RIP uses hop count.

These metrics are not directly equivalent.

For example:

```text
OSPF Route:
192.168.10.0/24
Cost: 20

Redistributed into RIP:

192.168.10.0/24
Metric: 2
```

The RIP metric is assigned according to the redistribution configuration and platform behavior. It is not automatically a conversion of OSPF cost 20 into RIP hop count 2.

**Important:** Always configure or verify redistribution metrics explicitly when required. Some platforms require a seed metric or default metric for routes imported into a protocol.

# 🗁 Administrative Distance and Redistribution

Administrative distance (AD) is a route-preference mechanism used by some routing platforms to compare routes learned from different sources.

For example, Cisco IOS traditionally uses these default administrative distances:

| Route Source | Cisco IOS Default AD |
| --- | ---: |
| Connected | 0 |
| Static | 1 |
| eBGP | 20 |
| EIGRP internal | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP external | 170 |
| iBGP | 200 |

These are Cisco IOS defaults, not universal values.

Redistribution can introduce routes that compete with routes learned through another routing protocol.

For example:

```text
Destination: 192.168.50.0/24

OSPF Route:
Administrative Distance = 110

RIP Route:
Administrative Distance = 120
```

On a Cisco IOS router, the OSPF route is normally preferred when the destination prefix and other relevant route-selection conditions are comparable.

However, route selection is not based on administrative distance alone.

**Important routing principles:**

* Longest Prefix Match determines which destination prefix is most specific.
* Administrative distance compares competing route sources for the same prefix on platforms that use AD.
* Metrics compare routes according to the relevant protocol's selection rules.
* Redistribution policies determine which routes are imported and with what attributes.

Do not assume that changing a redistribution metric automatically changes the administrative distance of the resulting route.

# 🗁 Redistribution and Routing Loops

Routing loops are one of the most important risks associated with mutual redistribution.

A routing loop can occur when a route learned from one protocol is redistributed into another protocol and later returns to its original protocol.

Consider:

```text
          OSPF Domain
               |
               |
          Router A
               |
               |
          Router B
               |
               |
           RIP Domain
```

Suppose:

* Router A redistributes OSPF routes into RIP.
* Router B redistributes RIP routes into OSPF.
* An OSPF route is advertised into RIP.
* The route is then redistributed back into OSPF.

The route may be reintroduced as external routing information.

Depending on the topology, route-selection rules, and redistribution policies, this can create feedback, incorrect route preferences, or persistent routing loops.

**Example route feedback:**

```text
OSPF Route
    |
    v
Redistributed into RIP
    |
    v
Learned by another router
    |
    v
Redistributed back into OSPF
    |
    v
Route feedback
```

The solution is to control which routes can enter each routing domain.

Common prevention mechanisms include:

* Route tagging.
* Prefix lists.
* Route maps.
* Route filters.
* Redistribution policies.
* Clear routing-domain boundaries.

# 🗁 Route Tags

Route tags are values attached to routing information to identify its origin or redistribution history.

They are useful when routes move between multiple routing protocols.

For example:

```text
OSPF Routes
    |
    | Tag = 100
    v
RIP Domain
    |
    | Reject routes tagged 100
    v
OSPF Redistribution
```

A route originally learned from OSPF receives a tag when redistributed into RIP.

When that route is later considered for redistribution back into OSPF, the router checks the tag and rejects it.

This prevents the route from being redistributed back into its originating domain.

**Example route-tag policy:**

```text
Routes learned from OSPF:
Tag = 100

Routes learned from RIP:
Tag = 200
```

A redistribution policy can then reject routes carrying the tag that identifies the destination protocol's own previously exported routes.

Route-tag support and behavior vary by routing protocol and platform.

# 🗁 Route Filtering and Redistribution Policies

Route filtering controls which routes are allowed to enter or leave a routing protocol.

It is important because unrestricted redistribution can introduce unwanted routes into a routing domain.

Common filtering mechanisms include:

| Mechanism | Purpose |
| --- | --- |
| Prefix list | Matches routes by network prefix and prefix length |
| Route map | Applies conditions and actions to routes |
| Access control list | Matches route addresses in supported redistribution contexts |
| Route tag | Identifies routes by origin or redistribution history |
| Distribution list | Controls advertised or accepted routes where supported |
| Policy statement | Defines routing import and export behavior on supported platforms |

**Example policy:**

Suppose only these networks should be redistributed:

```text
192.168.10.0/24
192.168.20.0/24
```

The following network should not be redistributed:

```text
192.168.99.0/24
```

A prefix-based policy can explicitly permit the required networks and reject everything else.

**Best practice:** Use an explicit allowlist of approved prefixes rather than redistributing every available route without filtering.

# 🗁 Default Route Redistribution

A default route can also be introduced into a routing protocol.

For IPv4, the default route is:

```text
0.0.0.0/0
```

For IPv6, the default route is:

```text
::/0
```

A router may redistribute or originate a default route so that other routers can forward unknown destinations toward it.

Example:

```text
              Internet
                 |
              Router A
                 |
          Redistribution
              Router
                 |
              Router B
                 |
              Internal LAN
```

Router A has internet connectivity.

The redistribution router advertises a default route into the internal routing domain.

Router B can then use that route to forward traffic toward Router A.

The exact mechanism differs by protocol:

* OSPF supports explicit default-information origination.
* BGP can advertise a default route when its origination conditions are satisfied.
* RIP can advertise a default route through supported implementation-specific configuration.

Default route advertisement should be conditional on actual upstream reachability where appropriate.

# 🗁 Redistribution Design Considerations

Before implementing redistribution, determine:

**Which routes should be exchanged?**

Identify the exact prefixes that must be available in each routing domain.

**Which direction is required?**

Use one-way redistribution when mutual redistribution is unnecessary.

**What metrics should be assigned?**

Determine the destination protocol's required metric and appropriate value.

**How will routing loops be prevented?**

Use route tags, prefix filtering, route maps, and explicit routing policies.

**How will external routes be identified?**

Understand how the destination protocol represents redistributed routes.

For example, OSPF distinguishes external route types such as E1 and E2.

**How will default routes be handled?**

Decide whether default routes should be advertised, conditionally originated, or excluded.

**What happens during failure?**

Test route withdrawal, protocol convergence, and upstream reachability.

**How will route growth be controlled?**

Avoid importing unnecessary routes that increase routing-table size and operational complexity.

# 🗁 Redistribution on Linux Using FRRouting

FRRouting (FRR) supports route redistribution between routing protocols.

The following example demonstrates redistributing selected routes between OSPF and RIP.

**Example topology**

```text
          OSPF Router
               |
          10.0.12.0/30
               |
         Redistribution
             Router
               |
          10.0.23.0/30
               |
           RIP Router
```

Assume the redistribution router has:

```text
OSPF-facing interface:
10.0.12.1/30

RIP-facing interface:
10.0.23.1/30

OSPF internal network:
192.168.10.0/24

RIP internal network:
192.168.20.0/24
```

Install FRRouting on Ubuntu:

```bash
sudo apt update
sudo apt install frr frr-pythontools
```

Enable the required daemons in:

```bash
sudo nano /etc/frr/daemons
```

Set:

```ini
ospfd=yes
ripd=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

Enter the FRR CLI:

```bash
sudo vtysh
```

**Configure OSPF**

```text
configure terminal

router ospf
 network 10.0.12.0/30 area 0
 network 192.168.10.0/24 area 0

end
```

**Configure RIP**

```text
configure terminal

router rip
 version 2
 network 10.0.23.0/30
 network 192.168.20.0/24
 no auto-summary

end
```

**Redistribute OSPF routes into RIP**

```text
configure terminal

router rip
 redistribute ospf metric 2

end
```

This imports eligible OSPF routes into RIP with a configured RIP metric of 2.

**Redistribute RIP routes into OSPF**

```text
configure terminal

router ospf
 redistribute rip metric-type 2 metric 20

end
```

This example imports eligible RIP routes into OSPF as external Type 2 routes with an external metric of 20.

The exact accepted syntax and behavior should be checked against the installed FRR version.

**Verify redistribution**

```text
show ip route
show ip route rip
show ip route ospf
show ip ospf
show ip rip
```

Inspect the Linux routing table:

```bash
ip route show
```

**Important:** These commands illustrate the redistribution mechanism. A production configuration should include route filtering, route tagging where supported, and explicit control over which prefixes are redistributed in each direction.

# 🗁 Redistribution on MikroTik RouterOS

MikroTik RouterOS v7 supports routing filters and route redistribution through routing protocol configuration.

The exact configuration depends on the routing protocols involved and the RouterOS version.

A typical design uses routing filters to control which routes are accepted or advertised.

**Inspect routing configuration**

```routeros
/routing ospf instance print
/routing ospf area print
/routing rip instance print
/routing filter rule print
/routing route print detail
```

**Example routing filter concept**

A filter can identify routes by prefix and determine whether they should be accepted or rejected.

For example, a policy may allow:

```text
192.168.10.0/24
192.168.20.0/24
```

while rejecting other prefixes.

A conceptual RouterOS v7 filter rule can look like:

```routeros
/routing filter rule
add chain=redistribution-in rule="if (dst in 192.168.10.0/24) { accept }"
```

The chain must be attached to the relevant routing protocol's supported input or output policy.

**Important:** A routing filter rule does not automatically redistribute routes. The relevant protocol must also be configured to import or export the intended route source. RouterOS v7 uses routing filters and protocol-specific settings rather than the older RouterOS v6 routing configuration model.

For production configurations, verify the available redistribution properties using the installed RouterOS CLI:

```routeros
/routing ospf ?
/routing rip ?
/routing filter rule ?
```

Then inspect the resulting routes:

```routeros
/routing route print detail
```

# 🗁 Redistribution on Cisco IOS

Cisco IOS supports route redistribution through routing protocol configuration commands.

The following example demonstrates redistribution between OSPF and RIP.

**Example topology**

```text
          OSPF Domain
               |
               |
          Router A
               |
               |
          Router B
               |
               |
           RIP Domain
```

Assume Router B participates in both routing domains.

**Configure OSPF**

```cisco
configure terminal

router ospf 1
 network 10.0.12.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0

end
```

**Configure RIP**

```cisco
configure terminal

router rip
 version 2
 no auto-summary
 network 10.0.0.0
 network 192.168.20.0

end
```

**Redistribute OSPF into RIP**

```cisco
configure terminal

router rip
 redistribute ospf 1 metric 2

end
```

This imports eligible OSPF routes into RIP with a metric of 2.

**Redistribute RIP into OSPF**

```cisco
configure terminal

router ospf 1
 redistribute rip subnets metric 20 metric-type 2

end
```

This imports eligible RIP routes into OSPF as external Type 2 routes with an external metric of 20.

The `subnets` keyword allows eligible subnet routes to be redistributed rather than restricting redistribution to classful network boundaries.

**Verify redistribution**

```cisco
show ip route
show ip protocols
show ip ospf
show ip rip database
```

Inspect OSPF external routes:

```cisco
show ip route ospf
```

**Important:** These examples demonstrate the basic commands, not a complete production policy. Mutual redistribution should use route maps and prefix filtering to prevent route feedback.

# 🗁 Redistribution Troubleshooting

Redistribution problems commonly occur because of missing metrics, incorrect route filters, protocol configuration errors, or route-selection conflicts.

**Check the routing table**

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
/routing route print detail
```

**Check routing protocols**

FRRouting:

```text
show ip protocols
show ip ospf
show ip rip
```

Cisco IOS:

```cisco
show ip protocols
show ip ospf
show ip rip database
```

MikroTik:

```routeros
/routing ospf instance print
/routing rip instance print
```

**Check route filters**

Cisco IOS:

```cisco
show route-map
show ip prefix-list
```

MikroTik:

```routeros
/routing filter rule print
```

**Check route metrics**

Confirm that the destination protocol has received a valid metric for redistributed routes.

**Check route tags**

Verify that routes are not unintentionally rejected or reintroduced because of incorrect route-tag policies.

**Check administrative distance**

A redistributed route may exist in the routing protocol database but may not become the active route because another route source is preferred.

**Common redistribution problems**

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| Routes are not redistributed | Redistribution is not configured | Check protocol configuration |
| Routes are missing | Route filters reject prefixes | Inspect filtering policies |
| Invalid metric | Missing or incorrect seed metric | Configure a valid destination metric |
| Route feedback | Mutual redistribution without loop prevention | Apply tags and route filters |
| Route not active | Competing route has better preference | Inspect route-selection rules |
| Unexpected prefixes | Redistribution policy is too broad | Use explicit prefix allowlists |
| Default route missing | Default origination not configured | Configure the appropriate default-route mechanism |
| Routing instability | Route flapping or feedback | Inspect protocol updates and redistribution boundaries |

# 🗁 Best Practices

* Redistribute only the routes that are actually required.
* Prefer one-way redistribution when possible.
* Use route maps, prefix lists, and routing filters.
* Apply route tags to identify redistributed routes.
* Prevent routes from returning to their original routing domain.
* Configure appropriate destination-protocol metrics.
* Understand administrative distance and protocol-specific route selection.
* Avoid unrestricted mutual redistribution.
* Advertise default routes only when the upstream path is valid.
* Monitor routing tables and protocol databases.
* Document redistribution boundaries and policies.
* Test route withdrawal and recovery before production deployment.

# 🗁 Practical Labs

**Lab: Redistribute OSPF Routes into RIP**

Objective: Allow RIP routers to learn networks from an OSPF domain.

Tasks:

* Build an OSPF domain.
* Build a separate RIP domain.
* Configure a router to participate in both.
* Redistribute OSPF routes into RIP.
* Verify that RIP routers learn OSPF networks.
* Test end-to-end connectivity.

Expected result:

RIP routers should learn selected OSPF networks through redistribution.

**Lab: Configure Mutual Redistribution**

Objective: Exchange routes between OSPF and RIP.

Tasks:

* Configure OSPF and RIP on a redistribution router.
* Redistribute OSPF into RIP.
* Redistribute RIP into OSPF.
* Inspect the routing tables.
* Observe how external routes appear.
* Test connectivity between both routing domains.

**Lab: Prevent Route Feedback**

Objective: Use route tags and filtering to prevent routes from returning to their original protocol.

Tasks:

* Configure mutual redistribution.
* Identify routes that can be reintroduced into their original domain.
* Apply route tags or equivalent policy mechanisms.
* Reject previously redistributed routes when they return.
* Verify that route feedback is prevented.

**Lab: Redistribute Static Routes into OSPF**

Objective: Advertise selected manually configured routes through OSPF.

Tasks:

* Configure a static route.
* Configure OSPF.
* Create a redistribution policy.
* Redistribute the selected static route.
* Verify the resulting external OSPF route.
* Remove the static route and observe route withdrawal.

# 🗁 Interview Questions

**What is route redistribution?**

Route redistribution is the process of importing routes from one routing protocol or routing source into another routing protocol.

**Why is redistribution required?**

It allows different routing domains using different protocols to exchange selected routing information.

**What is one-way redistribution?**

It transfers routes in one direction, such as OSPF into RIP.

**What is mutual redistribution?**

It transfers routes in both directions between routing domains.

**What is a redistribution metric?**

It is the metric assigned or derived when a route is imported into the destination routing protocol.

**Why is metric conversion important?**

Different protocols use different metric systems. A destination protocol requires a metric it can evaluate according to its own rules.

**What is route feedback?**

Route feedback occurs when a route redistributed into another protocol is later redistributed back into its original routing domain.

**How can route loops be prevented during redistribution?**

Route tags, prefix lists, route maps, and explicit redistribution policies can prevent unwanted route feedback.

**What is a route tag?**

A route tag is metadata attached to routing information to identify its origin or redistribution history.

**Does redistribution change administrative distance?**

Not necessarily. Redistribution imports routes into another protocol, while administrative distance is a separate route-preference mechanism. Exact behavior depends on the routing platform.

**What is an OSPF external route?**

An OSPF external route is a route introduced into an OSPF domain from an external routing source through redistribution or another external route-origination mechanism.

**What is the difference between OSPF E1 and E2?**

E1 includes the internal OSPF cost to reach the ASBR in addition to the external metric. E2 primarily compares the external metric, using internal cost as a tie-breaker under standard OSPF route selection.

**Why is unrestricted mutual redistribution dangerous?**

It can cause route feedback, routing loops, suboptimal path selection, and unexpected route advertisements.

# 🗁 Summary

Route redistribution allows different routing protocols and routing sources to exchange routing information.

It is essential when connecting separate routing domains, migrating between protocols, or integrating networks with different routing architectures.

Important concepts include:

* One-way and mutual redistribution.
* Destination protocol metrics.
* Administrative distance.
* Route feedback and routing loops.
* Route tags and filtering.
* Default route redistribution.
* Redistribution policies and troubleshooting.

Redistribution provides flexibility, but it must be carefully controlled. A well-designed redistribution policy should explicitly define which routes are imported, how they are identified, and how route feedback is prevented.

# 🗁 References

* [RFC 2328 – OSPF Version 2](https://datatracker.ietf.org/doc/html/rfc2328)
* [RFC 4271 – A Border Gateway Protocol 4 (BGP-4)](https://datatracker.ietf.org/doc/html/rfc4271)
* [RFC 2453 – RIP Version 2](https://datatracker.ietf.org/doc/html/rfc2453)
* [FRRouting Documentation](https://docs.frrouting.org/)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [Cisco IP Routing Documentation](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-software/products-installation-and-configuration-guides-list.html)
