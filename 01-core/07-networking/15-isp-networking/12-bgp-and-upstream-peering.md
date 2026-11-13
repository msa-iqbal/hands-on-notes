# BGP and Upstream Peering

Border Gateway Protocol (BGP) is the routing protocol used to exchange routing information between independent autonomous systems (ASes) across the Internet.

For an Internet Service Provider (ISP), BGP is essential for connecting to upstream providers, exchanging routes with other networks, announcing public IP prefixes, implementing traffic engineering, and maintaining Internet connectivity.

BGP enables an ISP to control how traffic enters and leaves its network through routing policies.

# 🗁 BGP Fundamentals

BGP is a path-vector routing protocol designed to exchange routing information between autonomous systems.

Unlike interior gateway protocols such as OSPF, which primarily operate within an autonomous system, BGP is designed to exchange routes between separate autonomous systems and can also distribute routes internally.

BGP is commonly used for:

* Connecting an ISP to upstream Internet providers.
* Establishing peering sessions with other networks.
* Announcing public IPv4 and IPv6 prefixes.
* Receiving Internet routing tables.
* Implementing multi-homing.
* Controlling inbound and outbound traffic.
* Providing redundant Internet connectivity.
* Applying routing policies based on prefixes and attributes.

BGP uses TCP port `179` for establishing peer sessions.

The current standard specification for BGP-4 is RFC 4271.

### 🖆 Why ISPs Need BGP

Consider an ISP connected to two upstream providers.

```text
                 Internet
                    |
          +---------+---------+
          |                   |
      Upstream A           Upstream B
        AS 64501             AS 64502
          |                   |
          +---------+---------+
                    |
                 ISP Router
                   AS 64510
                    |
               ISP Network
                    |
                Subscribers
```

The ISP can establish BGP sessions with both upstream providers.

It can then:

* Receive routes from both providers.
* Advertise its own public prefixes.
* Select preferred outgoing paths.
* Maintain connectivity when one upstream fails.
* Influence how other networks reach its prefixes.

Without BGP, an ISP could use static default routes, but it would have significantly less control over external routing and multi-homed connectivity.

# 🗁 Autonomous Systems

An Autonomous System (AS) is a collection of IP networks and routers operated under a common administrative routing policy.

Each publicly participating autonomous system is identified by an Autonomous System Number (ASN).

Example:

```text
ISP A: AS 64510
ISP B: AS 64520
ISP C: AS 64530
```

ASNs are used in BGP to identify autonomous systems and represent routing paths.

### 🖆 Public and Private ASNs

ASNs are allocated by Regional Internet Registries (RIRs) or through their associated processes.

Two ASN ranges are commonly relevant:

| ASN Range | Purpose |
| --- | --- |
| 1 - 4,207,853,311 | Public ASN space, excluding reserved ranges |
| 4,200,000,000 - 4,294,967,294 | Private 32-bit ASN range |

The private ASN ranges are:

```text
16-bit private ASN:
64512 - 65534

32-bit private ASN:
4200000000 - 4294967294
```

Private ASNs are used in private routing environments and certain provider configurations. They should not be treated as globally unique public ASNs.

An ISP connecting to the public Internet generally needs an appropriate public ASN, depending on its routing and provider arrangements.

# 🗁 BGP Operating Principles

BGP exchanges network reachability information between BGP peers.

A BGP peer advertises reachable IP prefixes along with path attributes that describe the route.

Example:

```text
ISP AS 64510 advertises:

203.0.113.0/24
```

An upstream provider receives the announcement and can propagate it to other networks according to its routing policies.

The address above is reserved for documentation and is used only as an example. A production ISP must announce an authorized public prefix.

### 🖆 BGP Session Establishment

BGP peers establish sessions over TCP.

The BGP finite-state machine uses these states:

| State | Description |
| --- | --- |
| Idle | BGP process is waiting to start a connection |
| Connect | BGP is attempting to establish TCP connectivity |
| Active | BGP is retrying connection establishment |
| OpenSent | An OPEN message has been sent |
| OpenConfirm | OPEN message has been received and confirmed |
| Established | BGP session is operational and can exchange routes |

An Established session indicates that the BGP neighbors have successfully completed the session negotiation.

It does not necessarily mean that useful routes are being received or advertised. Routing policies and address-family configuration still determine route exchange.

### 🖆 BGP Message Types

BGP uses several message types to maintain peer relationships and exchange routing information.

| Message | Purpose |
| --- | --- |
| OPEN | Establishes a BGP session and negotiates parameters |
| UPDATE | Advertises or withdraws routes |
| KEEPALIVE | Maintains the peer relationship |
| NOTIFICATION | Reports an error and terminates the session |
| ROUTE-REFRESH | Requests route re-advertisement without resetting the session |

# 🗁 eBGP and iBGP

BGP sessions are commonly divided into two categories.

### 🖆 External BGP (eBGP)

eBGP exchanges routes between different autonomous systems.

Example:

```text
ISP AS 64510
      |
      | eBGP
      |
Upstream AS 64501
```

Typical eBGP use cases include:

* ISP-to-upstream connectivity.
* ISP-to-IXP peering.
* Direct peering with another network.
* Customer-to-provider routing.

### 🖆 Internal BGP (iBGP)

iBGP distributes BGP routing information between routers belonging to the same autonomous system.

Example:

```text
             ISP AS 64510
          +----------------+
          |                |
       Core R1           Core R2
          |                |
          +------ iBGP ----+
```

iBGP is commonly used to distribute external routes across an ISP's internal routing infrastructure.

### 🖆 eBGP vs iBGP

| Feature | eBGP | iBGP |
| --- | --- | --- |
| AS relationship | Different ASNs | Same ASN |
| Typical purpose | External route exchange | Internal BGP distribution |
| AS_PATH behavior | Adds the advertising AS | Normally does not add the local AS |
| NEXT_HOP behavior | Commonly updated | Normally preserved unless configured otherwise |
| TTL behavior | Usually 1 by default for directly connected peers | Usually 255 by default |
| Route propagation | Subject to eBGP export policy | iBGP-learned routes are not normally advertised to another iBGP peer |

### 🖆 iBGP Full Mesh

Traditional iBGP requires each router to establish an iBGP session with every other router in the same AS to ensure complete route distribution.

For a network containing many routers, a full mesh becomes difficult to manage.

The number of sessions is:

```text
n(n - 1) / 2
```

For example, with 10 routers:

```text
10 × 9 / 2 = 45 sessions
```

Route reflectors reduce the number of required iBGP sessions by allowing selected routers to distribute routes to other iBGP peers.

# 🗁 Upstream Connectivity

An upstream provider supplies Internet connectivity to another network.

An ISP commonly purchases IP transit from one or more upstream providers.

The upstream provider advertises Internet routes to the ISP, while the ISP advertises its authorized prefixes to the upstream.

### 🖆 Single Upstream

A single-homed ISP connects to one upstream provider.

```text
             Internet
                |
          Upstream AS
                |
             ISP AS
                |
           Subscribers
```

Advantages:

* Simple architecture.
* Lower operational complexity.
* Easier routing policy management.

Disadvantages:

* Dependence on one upstream provider.
* Limited path diversity.
* Upstream failure can interrupt Internet connectivity.

### 🖆 Multiple Upstreams

A multi-homed ISP connects to two or more upstream providers.

```text
              Internet
             /        \
            /          \
       Upstream A    Upstream B
         AS 64501      AS 64502
            \          /
             \        /
              ISP AS
                |
           Subscribers
```

Advantages:

* Better upstream redundancy.
* More routing choices.
* Greater control over traffic engineering.
* Reduced dependency on a single provider.

Disadvantages:

* More complex routing policies.
* Larger routing tables.
* Additional operational and commercial costs.
* More demanding troubleshooting.

# 🗁 Internet Peering

Peering is a network interconnection arrangement in which two autonomous systems exchange traffic directly.

Peering can improve routing efficiency by avoiding unnecessary transit providers.

For example:

```text
          ISP A                 ISP B
         AS 64510              AS 64520
             \                  /
              \                /
               Internet Exchange
                    (IXP)
```

An ISP can establish a BGP session with another participant through an Internet Exchange Point.

### 🖆 Public Peering

Public peering takes place through a shared switching fabric at an IXP.

Multiple networks connect to the same exchange infrastructure and establish individual BGP sessions with selected participants.

Advantages:

* Access to multiple networks through one exchange.
* Reduced dependency on transit for reachable peers.
* Potentially lower latency.
* More efficient traffic exchange.

### 🖆 Private Peering

Private peering uses a dedicated physical or logical interconnection between two networks.

It is commonly used when traffic volume or operational requirements justify a direct connection.

Advantages:

* Dedicated interconnection capacity.
* Greater control over the physical or logical path.
* Potentially better performance for high-volume traffic.

# 🗁 Transit vs Peering

Transit and peering are different interconnection arrangements.

| Feature | IP Transit | Peering |
| --- | --- | --- |
| Purpose | Access to the broader Internet | Exchange traffic between networks |
| Route coverage | Usually provides broad Internet reachability | Usually limited to participating networks and their customers |
| Commercial model | Commonly paid | Settlement-free or paid |
| Typical provider | Upstream transit provider | IXP participant or direct peer |
| Internet reachability | Provides routes toward the global Internet | Does not automatically provide full Internet reachability |

An ISP may use both transit and peering.

A common architecture is to purchase transit from upstream providers while using an IXP to exchange traffic with local and regional networks.

# 🗁 BGP Attributes

BGP attributes provide information used to describe and evaluate routes.

They help routers determine which path should be preferred and how routes should be advertised.

Important BGP attributes include:

| Attribute | Description |
| --- | --- |
| AS_PATH | Lists autonomous systems through which a route has passed |
| NEXT_HOP | Identifies the next-hop address used to reach a destination |
| LOCAL_PREF | Expresses preference for routes within an AS |
| MED | Suggests a preferred entry point into an AS |
| ORIGIN | Indicates how a route originated in BGP |
| COMMUNITY | Tags routes for policy-based processing |
| ATOMIC_AGGREGATE | Indicates that route aggregation has occurred with information loss |

### 🖆 AS_PATH

AS_PATH contains the sequence of autonomous systems through which a route has passed.

Example:

```text
64501 64520 64530
```

A shorter AS_PATH is often preferred when other selection criteria are equal, but it is not universally the deciding factor.

AS_PATH also helps detect routing loops. A router normally rejects a route containing its own ASN in the AS_PATH.

### 🖆 NEXT_HOP

NEXT_HOP identifies the next-hop IP address used to reach a destination.

A route may be rejected or unusable if its next hop cannot be resolved.

For iBGP, the next-hop address is normally preserved when a route is advertised unless a configuration such as next-hop-self changes it.

### 🖆 LOCAL_PREF

LOCAL_PREF is used within an autonomous system to express the preferred outbound path.

A higher LOCAL_PREF value is generally preferred.

Example:

```text
Upstream A: LOCAL_PREF 200
Upstream B: LOCAL_PREF 100
```

The ISP prefers routes learned from Upstream A when comparing otherwise eligible routes.

LOCAL_PREF is distributed within the AS through iBGP and is not normally advertised to external peers.

### 🖆 MED

Multi-Exit Discriminator (MED) is an optional non-transitive attribute used to suggest a preferred entry point into an AS.

A lower MED is generally preferred when the routes are compared under the applicable BGP selection rules.

MED is not a universal instruction that all neighboring networks must follow.

### 🖆 BGP Communities

BGP communities are tags attached to routes to simplify routing policy.

An ISP may use communities to identify:

* Customer routes.
* Peer routes.
* Upstream routes.
* Preferred or backup paths.
* Routes that should be filtered or modified by policy.

Well-known communities include:

| Community | Purpose |
| --- | --- |
| NO_EXPORT | Prevents advertisement outside the local AS or confederation boundary |
| NO_ADVERTISE | Prevents advertisement to any BGP peer |
| NO_EXPORT_SUBCONFED | Restricts advertisement outside the local AS confederation |

Providers may also define their own community values and policies.

# 🗁 BGP Route Selection

When multiple routes exist for the same destination prefix, BGP applies a route-selection process to determine the preferred path.

The exact decision process can vary between implementations, but common criteria include:

* Route eligibility and next-hop reachability.
* Highest LOCAL_PREF.
* Locally originated route preference.
* Shortest AS_PATH, under applicable conditions.
* Lowest ORIGIN type.
* Lowest MED, when routes are comparable under the implementation's rules.
* eBGP preference over iBGP in common implementations.
* Lowest IGP cost to the BGP next hop.
* Additional vendor-specific tie-breakers.

The complete process is more detailed than simply choosing the shortest AS_PATH.

Example:

```text
Destination: 198.51.100.0/24

Path A:
LOCAL_PREF: 200
AS_PATH: 64501 64520

Path B:
LOCAL_PREF: 100
AS_PATH: 64502

Preferred Path: Path A
```

Path A is preferred because its LOCAL_PREF is higher, even though its AS_PATH is longer.

The example prefix is reserved for documentation.

# 🗁 Public Prefix Announcement

A public IP prefix announcement makes a network reachable through BGP.

An ISP must have the authority to originate and advertise the prefix.

Before announcing a prefix, verify:

* The address block is allocated or legitimately assigned.
* The ASN is authorized for the intended origin.
* The prefix length is accepted by upstream providers.
* The prefix is not already being originated incorrectly elsewhere.
* Routing filters permit the announcement.
* IRR route objects or equivalent routing records are maintained where required.
* RPKI Route Origin Authorization (ROA) records are correctly configured where applicable.

### 🖆 Example Prefix Announcement

Suppose an ISP has an authorized public prefix:

```text
203.0.113.0/24
```

It can originate this prefix toward an upstream provider.

This is a documentation-only example. An actual deployment must use its own allocated public prefix.

### 🖆 Default Route vs Full Routing Table

An ISP can receive different levels of routing information from an upstream provider.

| Routing Option | Description |
| --- | --- |
| Default route | Receives a route representing destinations not otherwise known |
| Partial routes | Receives selected routes from the upstream |
| Full routing table | Receives a large set of Internet routes |

A small ISP may use a default route when its routing requirements are simple.

A multi-homed ISP may need partial or full routing information to make more informed path-selection decisions.

Full Internet routing tables require sufficient router memory, CPU, and appropriate software and hardware capacity.

# 🗁 BGP Routing Policies

BGP policies control which routes an ISP accepts, prefers, and advertises.

Policies are implemented through prefix lists, route filters, AS_PATH filters, communities, and routing policy rules.

### 🖆 Import Policy

Import policy controls routes received from a BGP neighbor.

Typical requirements include:

* Accept only expected routes.
* Reject unauthorized or invalid prefixes.
* Set LOCAL_PREF according to upstream preference.
* Apply community-based classification.
* Prevent accidental acceptance of inappropriate routes.

### 🖆 Export Policy

Export policy controls routes advertised to a BGP neighbor.

An ISP should carefully define which prefixes each neighbor is allowed to receive.

For example:

| Neighbor | Advertised Routes |
| --- | --- |
| Upstream A | Authorized ISP prefixes |
| Upstream B | Authorized ISP prefixes |
| IXP Peer | ISP prefixes and eligible customer routes |
| Customer | Routes permitted by the customer agreement |

An ISP should avoid unintentionally advertising routes learned from one provider to another unless its transit or peering agreement and routing policy explicitly permit it.

### 🖆 Prefix Filtering

Prefix filtering limits accepted or advertised routes to approved address ranges and prefix lengths.

Example policy:

```text
Accept:
203.0.113.0/24

Reject:
Unexpected prefixes
Default route, if not required
Overly specific routes
```

This is a conceptual example using documentation space, not a production filter.

# 🗁 Multi-Homing and Redundancy

Multi-homing connects an ISP to multiple upstream providers.

It improves resilience by providing alternative paths when a provider or interconnection fails.

Example:

```text
             Upstream A
                 |
              eBGP
                 |
            +---------+
            | ISP Edge|
            | Router  |
            +---------+
                 |
             ISP Core
                 |
            +---------+
            | ISP Edge|
            | Router  |
            +---------+
                 |
              eBGP
                 |
             Upstream B
```

A production design may use separate edge routers, redundant physical paths, and independent power and transport facilities.

### 🖆 Active-Active Connectivity

Both upstream connections can carry traffic simultaneously.

Traffic distribution depends on routing policy, BGP path selection, and traffic engineering.

### 🖆 Active-Backup Connectivity

One upstream is preferred while another provides backup connectivity.

When the preferred path becomes unavailable, the backup path can become active.

### 🖆 Failover Considerations

A robust multi-homed design should consider:

* BGP session monitoring.
* Physical interface failures.
* Upstream reachability.
* Route withdrawal behavior.
* Redundant edge routers.
* Independent power and transport paths.
* Convergence time.
* Asymmetric traffic paths.
* Session continuity for active customer connections.

BGP convergence does not guarantee that existing application sessions will survive a path change.

# 🗁 Traffic Engineering

Traffic engineering is the process of influencing traffic paths to improve performance, capacity utilization, and network resilience.

An ISP can use BGP attributes and provider-supported policies to influence traffic direction.

### 🖆 Outbound Traffic Engineering

Outbound traffic engineering controls how the ISP sends traffic toward external networks.

Common techniques include:

* LOCAL_PREF.
* Routing policy.
* AS_PATH length considerations.
* Next-hop selection.
* IGP cost to BGP next hops.

Example:

```text
Upstream A: LOCAL_PREF 200
Upstream B: LOCAL_PREF 100
```

The ISP prefers Upstream A for eligible destinations.

### 🖆 Inbound Traffic Engineering

Inbound traffic engineering influences how external networks reach the ISP.

Common techniques include:

* AS_PATH prepending.
* MED, where applicable and honored.
* Provider-specific BGP communities.
* More-specific prefix announcements, when justified and accepted.

Example AS_PATH prepending:

```text
Normal announcement:
64510

Prepended announcement:
64510 64510 64510
```

Prepending may make a path appear longer to remote networks, but it does not guarantee that they will choose another path.

Inbound traffic engineering is less predictable because the final path-selection decision belongs to external networks.

# 🗁 BGP Security

BGP security is critical because incorrect route advertisements can affect connectivity far beyond an individual ISP.

Common risks include:

* Route leaks.
* Prefix hijacking.
* Unauthorized route announcements.
* Incorrect route filtering.
* Session spoofing or disruption.
* Accidental advertisement of private or internal prefixes.

### 🖆 Prefix Filtering

Only accept prefixes that are expected from a neighbor.

Likewise, advertise only authorized prefixes.

### 🖆 RPKI and Route Origin Validation

Resource Public Key Infrastructure (RPKI) helps validate whether an autonomous system is authorized to originate a prefix.

A Route Origin Authorization (ROA) specifies:

* The authorized prefix.
* The authorized origin ASN.
* The maximum permitted prefix length.

Route Origin Validation (ROV) classifies routes as:

| State | Meaning |
| --- | --- |
| Valid | A matching ROA authorizes the origin |
| Invalid | A matching ROA exists, but the origin ASN or prefix length is not authorized |
| NotFound | No covering ROA exists |

An ISP can apply routing policies to reject RPKI-invalid routes.

RPKI origin validation does not validate the entire AS_PATH and does not prevent every form of route leak.

### 🖆 Session Protection

Recommended controls include:

* Use TCP MD5 or TCP Authentication Option (TCP-AO) where supported and appropriate.
* Restrict TCP port 179 access to authorized peers.
* Use infrastructure ACLs.
* Apply appropriate TTL security mechanisms where supported.
* Monitor BGP session changes.
* Protect router management access.
* Keep routing software updated.

# 🗁 MikroTik RouterOS v7 BGP Example

MikroTik RouterOS v7 supports BGP configuration through routing connection, template, and filter objects.

The following is an illustrative eBGP configuration.

Topology:

```text
ISP Router:
Address: 192.0.2.2
ASN:     64510

Upstream Router:
Address: 192.0.2.1
ASN:     64501

ISP Public Prefix:
203.0.113.0/24
```

All addresses are documentation examples. Replace them with authorized production values.

### 🖆 Configure the BGP Connection

```routeros
/routing bgp connection
add name=upstream-a \
    remote.address=192.0.2.1 \
    remote.as=64501 \
    local.role=ebgp \
    local.as=64510 \
    routing-table=main
```

### 🖆 Create a Prefix List

```routeros
/ip firewall address-list
add list=ISP-PUBLIC-PREFIXES \
    address=203.0.113.0/24
```

### 🖆 Advertise the Public Prefix

The prefix must exist in the routing table before BGP can advertise it.

For a controlled example, create a blackhole route:

```routeros
/ip route
add dst-address=203.0.113.0/24 \
    blackhole
```

This blackhole route is only an origination example. A production network must use an appropriate route design and ensure that the announced prefix is genuinely reachable through the ISP network.

### 🖆 Configure an Output Filter

```routeros
/routing filter rule
add chain=ISP-OUT \
    rule="if (dst == 203.0.113.0/24) { accept }"

add chain=ISP-OUT \
    rule="reject"
```

Apply the output filter to the BGP connection:

```routeros
/routing bgp connection
set [find name=upstream-a] \
    output.filter-chain=ISP-OUT
```

### 🖆 Verify the BGP Session

```routeros
/routing bgp session print
```

Inspect received routes:

```routeros
/ip route print
```

Inspect BGP-related routing information:

```routeros
/routing route print where protocol=bgp
```

RouterOS v7 filter syntax and connection properties should be checked against the installed release and the intended routing policy before deployment.

# 🗁 BGP Troubleshooting

BGP problems can result from connectivity failures, incorrect ASNs, authentication mismatches, routing filters, or next-hop resolution issues.

### 🖆 BGP Session Stuck in Active

Possible causes:

* TCP port 179 is blocked.
* Incorrect peer IP address.
* Remote BGP service is unavailable.
* Incorrect ASN configuration.
* No IP connectivity between peers.
* Authentication mismatch.

Useful checks:

```bash
ping 192.0.2.1
```

Check TCP connectivity:

```bash
nc -vz 192.0.2.1 179
```

Inspect routes to the peer:

```bash
ip route get 192.0.2.1
```

### 🖆 BGP Session Established but No Routes Received

Possible causes:

* The neighbor is not advertising any prefixes.
* Import filters reject the routes.
* Address families are not properly configured.
* The upstream provider has not activated the required routing policy.
* The routes are not eligible for installation.

Check the received routes and import policies on both routers.

### 🖆 Prefix Not Being Advertised

Possible causes:

* The prefix does not exist in the routing table.
* The export filter rejects the prefix.
* The prefix is not authorized or permitted by the upstream.
* The route is not eligible for advertisement.
* The wrong address family is enabled.

Verify the local route and outbound routing policy.

### 🖆 Incorrect Outbound Path

Possible causes:

* Unexpected LOCAL_PREF values.
* Incorrect route filtering.
* Unreachable next hop.
* Unexpected AS_PATH.
* Incorrect routing policy.

Inspect the selected route and its attributes.

### 🖆 Useful Troubleshooting Checklist

* [ ] Verify physical and IP connectivity.
* [ ] Confirm peer IP addresses.
* [ ] Confirm local and remote ASNs.
* [ ] Check TCP port 179 connectivity.
* [ ] Verify authentication settings.
* [ ] Check BGP session state.
* [ ] Inspect received and advertised prefixes.
* [ ] Review import and export filters.
* [ ] Verify next-hop reachability.
* [ ] Check RPKI validation results.
* [ ] Confirm upstream routing policies.
* [ ] Review logs for BGP notifications and session resets.

# 🗁 References

* [RFC 4271 — A Border Gateway Protocol 4 (BGP-4)](https://datatracker.ietf.org/doc/html/rfc4271)
* [RFC 1997 — BGP Communities Attribute](https://datatracker.ietf.org/doc/html/rfc1997)
* [RFC 4360 — BGP Extended Communities Attribute](https://datatracker.ietf.org/doc/html/rfc4360)
* [RFC 6793 — BGP Support for Four-Octet Autonomous System Numbers](https://datatracker.ietf.org/doc/html/rfc6793)
* [RFC 6811 — BGP Prefix Origin Validation](https://datatracker.ietf.org/doc/html/rfc6811)
* [RFC 8212 — Default External BGP Route Propagation Behavior](https://datatracker.ietf.org/doc/html/rfc8212)
* [RFC 9234 — Route Leak Prevention and Detection Using Roles](https://datatracker.ietf.org/doc/html/rfc9234)
* [RIPE NCC — BGP and Routing](https://www.ripe.net/)
* [MikroTik RouterOS — BGP](https://help.mikrotik.com/docs/)
