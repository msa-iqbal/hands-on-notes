# IXP and Transit

An Internet Exchange Point (IXP) is a physical infrastructure where Internet Service Providers (ISPs), content delivery networks (CDNs), cloud providers, and other autonomous systems interconnect to exchange Internet traffic.

IP transit is a service through which one network provides another network with access to the broader Internet.

Both IXP connectivity and IP transit are important components of ISP network architecture. They help improve connectivity, reduce latency, optimize bandwidth costs, and provide access to networks across the Internet.

For an ISP, understanding the difference between peering and transit is essential when designing upstream connectivity and Internet routing strategies.

# 🗁 Internet Exchange Point Fundamentals

An Internet Exchange Point is a networking facility that allows multiple independent networks to exchange IP traffic directly.

Instead of sending traffic through an upstream transit provider, two participating networks can exchange traffic through an IXP switching fabric.

For example, suppose ISP A and ISP B operate in the same country.

Without direct peering:

```text
ISP A
  |
Transit Provider
  |
ISP B
```

With IXP peering:

```text
ISP A --------+
              |
           IXP Fabric
              |
ISP B --------+
```

The IXP provides the shared infrastructure, while participating networks establish their own routing relationships.

An IXP generally does not become the transit provider for its participants merely by operating the exchange fabric.

### 🖆 Why ISPs Connect to IXPs

ISPs connect to IXPs for several reasons:

* Exchange traffic directly with other networks.
* Reduce dependence on upstream transit.
* Improve latency for locally reachable destinations.
* Reduce international bandwidth consumption.
* Improve routing efficiency.
* Establish relationships with CDNs and content providers.
* Improve network resilience through additional interconnections.
* Access regional and local Internet ecosystems.

An IXP is especially valuable when significant traffic exists between networks participating in the same exchange.

# 🗁 How an IXP Works

An IXP typically operates a Layer 2 Ethernet switching fabric.

Participants connect their routers to the exchange infrastructure and establish BGP sessions with other participants or an IXP route server.

Example:

```text
                 Internet Exchange Point
                 +----------------------+
                 |    Ethernet Fabric   |
                 +----------------------+
                    |       |       |
                    |       |       |
                 ISP A    ISP B    CDN A
                 AS64510  AS64520  AS64530
                    |       |       |
                    +-------+-------+
                         BGP
```

The exchange fabric allows participants to communicate at Layer 2.

BGP operates between the participating autonomous systems and controls which IP prefixes are exchanged.

### 🖆 Traffic Exchange Process

A simplified traffic exchange process:

* ISP A connects its router to the IXP switching fabric.
* ISP B connects its router to the same fabric.
* Both networks establish a BGP peering relationship.
* ISP B advertises its reachable prefixes to ISP A.
* ISP A installs eligible routes in its routing table.
* Traffic destined for ISP B is forwarded through the IXP.

The IXP provides the interconnection infrastructure, but each participant controls its own routing policies.

# 🗁 IXP Infrastructure

An IXP requires reliable physical and logical infrastructure.

Typical components include:

| Component | Purpose |
| --- | --- |
| Ethernet switches | Provide the shared Layer 2 exchange fabric |
| Participant ports | Connect member networks |
| Route servers | Distribute BGP routes between participants |
| Looking glass | Allow participants to inspect routing information |
| Management network | Support administration and monitoring |
| Optical infrastructure | Provide fiber connectivity |
| Redundant power | Improve infrastructure availability |
| Monitoring systems | Monitor ports, traffic, and network health |

### 🖆 Switching Fabric

The switching fabric connects participant routers at Layer 2.

Common technologies include:

* Ethernet switching.
* VLAN-based separation.
* 10 Gigabit Ethernet.
* 25 Gigabit Ethernet.
* 40 Gigabit Ethernet.
* 100 Gigabit Ethernet.
* Higher-capacity Ethernet interfaces.

The actual port speeds depend on the IXP's infrastructure and service offerings.

### 🖆 Layer 2 Peering LAN

An IXP commonly provides a shared peering LAN.

Each participant receives an IP address on that LAN and can establish BGP sessions with other participants.

Example documentation network:

```text
IXP Peering LAN: 192.0.2.0/24

ISP A: 192.0.2.10
ISP B: 192.0.2.20
ISP C: 192.0.2.30
```

These addresses belong to a documentation-only range. An actual IXP uses its own designated peering LAN addressing.

### 🖆 Redundancy

Large IXPs may deploy:

* Multiple switching devices.
* Redundant power supplies.
* Diverse fiber paths.
* Multiple data center locations.
* Redundant management systems.
* High-availability monitoring infrastructure.

Participants should also consider redundant connections when the IXP offers suitable facilities.

# 🗁 IXP Participants

An IXP can connect different types of networks.

| Participant | Purpose |
| --- | --- |
| Internet Service Provider | Provides Internet access to subscribers |
| Content Delivery Network | Delivers content closer to end users |
| Cloud Provider | Provides cloud infrastructure and services |
| Hosting Provider | Hosts websites, applications, and servers |
| Educational Network | Connects academic and research networks |
| Government Network | Connects government infrastructure |
| Enterprise Network | Connects large organizational networks |
| International Carrier | Provides regional and global connectivity |

The more useful networks available through an exchange, the greater its potential value to participants.

However, the number of participants alone does not determine the value of an IXP. Traffic demand, network reachability, service quality, and operational reliability also matter.

# 🗁 Peering at an IXP

Peering is an arrangement where two autonomous systems exchange traffic directly.

An IXP makes it easier for networks to establish these relationships through a shared switching infrastructure.

### 🖆 Bilateral Peering

Bilateral peering means two networks establish a direct BGP session with each other.

Example:

```text
ISP A                         ISP B
AS64510                       AS64520
  |                              |
  +---------- IXP Fabric --------+
              BGP Session
```

Each network independently manages its BGP session and routing policies.

Advantages:

* Direct control over the peering relationship.
* Independent import and export policies.
* Flexible routing configuration.
* No dependency on an IXP route server for that session.

Disadvantages:

* More BGP sessions to configure.
* More operational work as the number of peers increases.
* Each participant relationship must be managed separately.

### 🖆 Multilateral Peering

Multilateral peering allows a participant to exchange routes with multiple networks through an IXP route server.

Instead of establishing a separate BGP session with every participant, the network establishes a session with the route server.

The route server distributes eligible routes according to its policies.

Example:

```text
              IXP Route Server
                /    |    \
               /     |     \
           ISP A   ISP B   ISP C
```

Advantages:

* Fewer BGP sessions.
* Easier access to multiple participants.
* Simplified route distribution.
* Reduced configuration overhead.

Disadvantages:

* Dependence on route server availability for route distribution.
* Less direct control over individual BGP sessions.
* Route server policies and participant policies must be understood.

A route server is not normally in the data path. Actual participant-to-participant traffic travels through the IXP switching fabric.

# 🗁 Route Servers

An IXP route server is a system that distributes BGP routing information between participating networks.

It simplifies multilateral peering by allowing participants to establish BGP sessions with the route server rather than maintaining a full mesh of bilateral sessions.

### 🖆 How a Route Server Works

Suppose three ISPs connect to an IXP.

```text
             IXP Route Server
              /      |      \
             /       |       \
          ISP A    ISP B    ISP C
          AS64510  AS64520  AS64530
```

The process works as follows:

* ISP A advertises its prefixes to the route server.
* The route server evaluates the advertisements according to its policies.
* Eligible routes are distributed to ISP B and ISP C.
* ISP B and ISP C install the routes if their own policies permit them.
* Traffic travels directly between the participants through the IXP fabric.

The route server distributes routing information but does not normally forward participant traffic.

### 🖆 Route Server vs Router

| Feature | Route Server | Conventional Router |
| --- | --- | --- |
| Main purpose | Distribute routing information | Exchange routes and forward packets |
| Data forwarding | Normally not in the participant traffic path | Forwards traffic |
| BGP sessions | Connects participants for route distribution | Establishes routing sessions |
| Traffic path | Through the IXP switching fabric | Through the router's forwarding plane |

### 🖆 Route Server Considerations

Participants should understand:

* Import and export filtering.
* BGP community support.
* Prefix limits.
* Maximum prefix configuration.
* RPKI route validation policies.
* AS_PATH handling.
* Route server redundancy.
* Looking glass availability.

Route server policies are generally documented by the IXP operator.

# 🗁 Internet Transit Fundamentals

IP transit is a commercial service through which an upstream network provides another network with access to the global Internet.

A transit provider advertises Internet routes to its customer and accepts the customer's authorized route announcements.

The customer pays according to the commercial agreement, which may consider bandwidth capacity, traffic volume, committed rate, or other service terms.

Example:

```text
              Global Internet
                    |
              Transit Provider
                 AS64501
                    |
                 IP Transit
                    |
                 ISP Router
                 AS64510
                    |
               ISP Network
                    |
                Subscribers
```

### 🖆 Why ISPs Purchase Transit

An ISP generally cannot reach every network on the Internet through local peering alone.

IP transit provides access to destinations beyond the ISP's directly connected peers.

Transit can provide:

* Global Internet reachability.
* Access to networks not available through local peering.
* International connectivity.
* A default route or broader routing information.
* Connectivity through an upstream provider's network.

### 🖆 Full Transit and Partial Transit

Transit services can provide different levels of routing information.

| Service | Description |
| --- | --- |
| Full Internet transit | Provides broad global Internet routing information |
| Default-only transit | Provides a default route for destinations not otherwise known |
| Partial transit | Provides selected Internet routes according to the provider's policy |

The exact service depends on the provider agreement and BGP configuration.

# 🗁 How IP Transit Works

IP transit is generally delivered through a physical or logical connection between the customer and provider networks.

A simplified process:

* The ISP orders transit service from an upstream provider.
* Both parties establish physical connectivity.
* IP addressing and BGP parameters are agreed upon.
* BGP sessions are established.
* The upstream advertises Internet routes to the ISP.
* The ISP advertises authorized public prefixes to the upstream.
* Traffic is exchanged according to the routing policies.

Example:

```text
ISP ASN:       AS64510
Upstream ASN:  AS64501

ISP Router:       192.0.2.2
Upstream Router:  192.0.2.1

BGP Session:      eBGP
```

The addresses and ASNs are documentation examples.

### 🖆 Transit Handoff

An upstream provider may deliver transit through:

* Dedicated fiber.
* Ethernet handoff.
* Optical transport.
* Data center cross-connect.
* Layer 2 transport service.
* A regional interconnection facility.

The handoff speed may be 1G, 10G, 25G, 40G, 100G, or another supported capacity.

### 🖆 Transit Service Parameters

Before activating transit, an ISP should agree on:

| Parameter | Description |
| --- | --- |
| Provider ASN | Upstream autonomous system number |
| Customer ASN | ISP autonomous system number |
| Handoff interface | Physical or logical connection |
| Interface capacity | Available port speed |
| Commit rate | Contracted bandwidth commitment |
| Burst capacity | Additional permitted traffic, if applicable |
| BGP configuration | Peer addresses and routing policy |
| Prefix authorization | Permitted customer announcements |
| Service availability | Availability and support commitments |
| DDoS protection | Available mitigation services |
| SLA | Service-level agreement |

# 🗁 Transit Provider Selection

Selecting an upstream provider is an important business and technical decision for an ISP.

A provider should be evaluated based on network reachability, performance, reliability, cost, and operational support.

### 🖆 Important Selection Factors

| Factor | What to Evaluate |
| --- | --- |
| Global reachability | Access to international networks |
| Network capacity | Available bandwidth and upgrade options |
| Latency | Delay to important destinations |
| Packet loss | Reliability of traffic delivery |
| Network diversity | Geographic and physical path diversity |
| Peering relationships | Direct connectivity with major networks |
| DDoS protection | Availability of mitigation services |
| Pricing | Cost per Mbps, Gbps, or committed capacity |
| SLA | Availability and fault-resolution commitments |
| Technical support | NOC availability and escalation process |
| BGP capabilities | Routing policy and community support |

### 🖆 Geographic Diversity

Two upstream providers do not necessarily provide meaningful redundancy if they use the same physical fiber route or depend on the same infrastructure.

An ISP should evaluate:

* Fiber path diversity.
* Data center diversity.
* International gateway diversity.
* Power infrastructure.
* Upstream network dependencies.
* Shared carrier and transport risks.

True redundancy requires independent failure paths, not merely different provider names.

# 🗁 Peering vs Transit

Peering and transit serve different connectivity purposes.

| Feature | Peering | IP Transit |
| --- | --- | --- |
| Main purpose | Exchange traffic between networks | Provide broader Internet access |
| Reachability | Participating network and eligible routes | Global Internet destinations |
| Commercial arrangement | Settlement-free or paid | Generally paid |
| Routing relationship | Usually selective | Usually broader route coverage |
| Typical connection | IXP or direct interconnection | Upstream provider |
| Traffic scope | Defined by peering policy | Defined by transit service |

An ISP generally benefits from combining peering and transit rather than relying exclusively on one.

For example:

* Use local peering for traffic to local ISPs.
* Use CDN peering for frequently accessed content.
* Use regional peering for nearby networks.
* Use IP transit for destinations not covered by peering.

# 🗁 IXP and Transit Architecture for an ISP

A well-designed ISP network can use multiple IXPs and upstream transit providers.

Example:

```text
                       Global Internet
                              |
                 +------------+------------+
                 |                         |
             Transit A                 Transit B
              AS64501                   AS64502
                 |                         |
                 +------------+------------+
                              |
                         ISP Edge
                          AS64510
                              |
                    +---------+---------+
                    |                   |
                IXP Connection      ISP Core
                    |                   |
                IXP Fabric          Aggregation
                    |                   |
              Local Networks          Access
                                        |
                                   Subscribers
```

In a production network, the IXP and transit connections may terminate on separate edge routers or redundant routing platforms.

### 🖆 Recommended Network Separation

An ISP should organize its network into logical layers:

| Layer | Responsibility |
| --- | --- |
| Edge | Upstream and IXP connectivity |
| Core | High-capacity internal routing |
| Aggregation | Collect traffic from access networks |
| Access | Connect subscriber equipment |
| Services | DNS, RADIUS, monitoring, and other services |

This separation improves operational clarity and allows different routing policies to be applied at appropriate boundaries.

### 🖆 Example Connectivity Strategy

| Destination | Preferred Connectivity |
| --- | --- |
| Local ISP networks | Local IXP peering |
| Major local CDNs | Direct or IXP peering |
| Regional networks | Regional peering or transit |
| International destinations | IP transit |
| Unavailable peer destinations | Transit fallback |

Actual route selection depends on BGP policies, prefix availability, and commercial agreements.

# 🗁 Traffic Exchange and Optimization

Traffic optimization is one of the main reasons ISPs invest in peering and multiple transit connections.

The goal is to deliver traffic through efficient paths while maintaining reliability and controlling costs.

### 🖆 Local Traffic Optimization

Suppose an ISP has subscribers accessing a service hosted by another local ISP.

Without local peering, traffic may travel through an international transit provider and return to the same country.

With local peering, traffic can remain within the local network ecosystem.

Potential benefits:

* Lower latency.
* Reduced international bandwidth consumption.
* Better routing efficiency.
* Lower transit costs.
* Improved user experience.

### 🖆 CDN Connectivity

Content Delivery Networks distribute cached content through geographically distributed servers.

Examples include:

* Video streaming content.
* Software downloads.
* Website assets.
* Application updates.
* Frequently accessed media.

An ISP may connect to CDN networks through an IXP or establish direct interconnection arrangements.

Local content delivery can reduce the amount of traffic that must travel through international transit.

### 🖆 Traffic Engineering

BGP policies can influence which paths are used for specific destinations.

Common techniques include:

* LOCAL_PREF for outbound path preference.
* AS_PATH prepending for inbound path influence.
* BGP communities supported by upstream providers.
* Selective prefix advertisement.
* More-specific announcements where justified and accepted.
* Policy-based routing for selected traffic classes, where appropriate.

Traffic engineering should be based on measurements and tested policies rather than assumptions about route length alone.

# 🗁 IXP and Transit Capacity Planning

Capacity planning ensures that network interconnections can handle current traffic and expected growth.

An ISP should monitor both average and peak traffic levels.

### 🖆 Important Capacity Metrics

| Metric | Description |
| --- | --- |
| Bandwidth | Maximum capacity of a link |
| Throughput | Actual data transfer rate |
| Utilization | Percentage of available capacity in use |
| Peak traffic | Highest observed traffic level |
| Average traffic | Average traffic over a defined period |
| Packet loss | Percentage of packets that fail to reach their destination |
| Latency | Time required for packets to travel |
| Jitter | Variation in packet delay |
| Interface errors | Physical or logical transmission problems |

### 🖆 Capacity Example

Suppose an ISP has two upstream connections:

| Upstream | Capacity | Peak Utilization |
| --- | ---: | ---: |
| Transit A | 10 Gbps | 7 Gbps |
| Transit B | 10 Gbps | 5 Gbps |

Combined peak traffic is not necessarily equal to the sum of the individual peaks because the measurements may occur at different times.

Capacity planning should consider:

* Peak traffic on each individual link.
* Traffic distribution.
* Failure scenarios.
* Available backup capacity.
* Growth forecasts.
* Interface and router forwarding capacity.

### 🖆 Redundancy Capacity

If an ISP expects one upstream to carry traffic when another fails, the remaining connection must have sufficient capacity for the expected failover load.

For example, two 10 Gbps transit links operating near 8 Gbps each cannot necessarily provide full redundancy if one link fails.

Capacity planning should model the failure of each important interconnection.

# 🗁 IXP and Transit Monitoring

Continuous monitoring helps an ISP identify connectivity problems, capacity limitations, and routing issues.

Important monitoring targets include:

| Monitoring Target | What to Monitor |
| --- | --- |
| Transit interfaces | Utilization, errors, discards |
| IXP interfaces | Traffic volume and port health |
| BGP sessions | Session state and uptime |
| BGP routes | Prefix count and unexpected changes |
| Latency | Response time to important destinations |
| Packet loss | End-to-end connectivity quality |
| Route changes | Unexpected announcements and withdrawals |
| Optical interfaces | Signal levels and link status |

### 🖆 Useful Monitoring Tools

* SNMP for interface and device monitoring.
* Prometheus for metrics collection.
* Grafana for visualization.
* LibreNMS for network monitoring.
* Zabbix for infrastructure monitoring.
* Smokeping for latency and packet-loss visualization.
* BGP monitoring platforms for routing visibility.
* Looking glass services for route inspection.

### 🖆 Traffic Graphs

An ISP should maintain separate traffic graphs for:

* Each upstream provider.
* Each IXP connection.
* Major peering partners.
* Core interconnections.
* Important regional links.

Separate graphs make it easier to identify traffic shifts and understand how routing changes affect bandwidth utilization.

# 🗁 Common Operational Challenges

### 🖆 High Latency Through an Upstream Provider

Possible causes:

* Congested transit links.
* Inefficient routing.
* Long physical paths.
* Upstream network problems.
* Congestion at an international gateway.

Investigation:

* Compare latency across multiple destinations.
* Inspect traceroute results.
* Compare upstream paths.
* Check interface utilization.
* Contact the provider when the problem is outside the ISP network.

### 🖆 IXP Peering Session Down

Possible causes:

* Physical link failure.
* Incorrect VLAN configuration.
* BGP neighbor configuration errors.
* Incorrect peering IP addresses.
* Authentication mismatch.
* IXP maintenance.
* Remote participant maintenance.

Investigation:

* Check physical interface status.
* Verify peering VLAN membership.
* Test Layer 3 connectivity.
* Inspect BGP session state.
* Check IXP operational announcements.

### 🖆 Unexpected Traffic Through Transit

Possible causes:

* IXP BGP session failure.
* Missing peering routes.
* Import policy changes.
* Incorrect LOCAL_PREF.
* Route withdrawal.
* More-specific route selection.

Investigation:

* Inspect BGP routes.
* Compare received prefixes.
* Verify route filters.
* Check route-selection attributes.
* Confirm the intended routing policy.

### 🖆 IXP Link Congestion

Possible causes:

* Insufficient port capacity.
* Increased traffic demand.
* Uneven traffic distribution.
* Unexpected traffic patterns.
* Interface or switching limitations.

Possible solutions:

* Upgrade the IXP port.
* Add an additional connection where supported.
* Review traffic distribution.
* Optimize peering relationships.
* Adjust traffic engineering policies.

# 🗁 Practical MikroTik RouterOS v7 Example

This example demonstrates a basic BGP peering connection to an IXP route server.

Topology:

```text
ISP Router:
ASN: 64510
Peering IP: 192.0.2.10

IXP Route Server:
ASN: 64500
Peering IP: 192.0.2.1

Peering Network:
192.0.2.0/24
```

All IP addresses and ASNs are documentation examples.

### 🖆 Configure the Peering Interface

Assume the IXP VLAN is already configured and connected through an interface named `ixp-vlan`.

Assign the peering address:

```routeros
/ip address
add address=192.0.2.10/24 interface=ixp-vlan
```

### 🖆 Configure the BGP Connection

```routeros
/routing bgp connection
add name=ixp-route-server \
    remote.address=192.0.2.1 \
    remote.as=64500 \
    local.role=ebgp \
    local.as=64510 \
    routing-table=main
```

### 🖆 Verify BGP Session Status

```routeros
/routing bgp session print
```

### 🖆 Inspect BGP Routes

```routeros
/routing route print where protocol=bgp
```

### 🖆 Monitor Interface Traffic

```routeros
/interface monitor-traffic ixp-vlan
```

### 🖆 Verify Connectivity

```routeros
/ping 192.0.2.1
```

This is a basic connectivity example. A production IXP configuration should also include appropriate import and export filters, prefix limits, route validation, and the IXP's documented peering policies.

# 🗁 References

* [RFC 4271 — A Border Gateway Protocol 4 (BGP-4)](https://datatracker.ietf.org/doc/html/rfc4271)
* [RFC 7947 — Internet Exchange BGP Route Server](https://datatracker.ietf.org/doc/html/rfc7947)
* [RFC 7948 — Internet Exchange BGP Route Server Operations](https://datatracker.ietf.org/doc/html/rfc7948)
* [RFC 9234 — Route Leak Prevention and Detection Using Roles](https://datatracker.ietf.org/doc/html/rfc9234)
* [Euro-IX — European Internet Exchange Association](https://www.euro-ix.net/)
* [PeeringDB — Network and Facility Directory](https://www.peeringdb.com/)
* [MANRS — Mutually Agreed Norms for Routing Security](https://manrs.org/)
* [RIPE NCC — Routing Information](https://www.ripe.net/)
* [MikroTik RouterOS — BGP Documentation](https://help.mikrotik.com/docs/)
