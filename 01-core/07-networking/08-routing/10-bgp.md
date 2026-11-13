# Border Gateway Protocol (BGP)

Border Gateway Protocol (BGP) is a path-vector routing protocol used to exchange routing information between autonomous systems (ASes).

It is the primary inter-domain routing protocol used by the global internet.

BGP allows independently operated networks, such as Internet Service Providers (ISPs), cloud providers, enterprises, and large organizations, to exchange information about reachable IP prefixes and apply routing policies.

Unlike OSPF, which is commonly used to calculate paths within an autonomous system, BGP is designed to exchange routing information between autonomous systems and make policy-based path-selection decisions.

Key characteristics:

* Path-vector routing protocol.
* Exterior Gateway Protocol (EGP).
* Used for inter-domain routing.
* Operates over TCP port 179.
* Supports IPv4 and IPv6 address families.
* Uses AS_PATH and other path attributes.
* Supports policy-based routing decisions.
* Supports route aggregation.
* Supports route filtering.
* Supports eBGP and iBGP.
* Uses incremental updates after initial route exchange.

# 🗁 What Is BGP?

BGP is a routing protocol that exchanges reachability information between autonomous systems.

An autonomous system is a collection of IP networks operated under a common routing administration and policy, identified by an Autonomous System Number (ASN).

BGP allows networks to announce which IP prefixes they can reach and learn which prefixes are reachable through other networks.

### 🖆 Example Network

```text
             AS 65001
              ISP A
                |
                |
              eBGP
                |
                |
             AS 65002
              ISP B
                |
                |
              eBGP
                |
                |
             AS 65003
             ISP C
```

Each autonomous system can exchange routing information with neighboring autonomous systems using BGP.

BGP does not simply select the path with the fewest routers. It uses path attributes and routing policies to select preferred paths.

# 🗁 Autonomous Systems

An Autonomous System (AS) is a group of IP networks and routers managed under a common routing policy.

Each publicly participating autonomous system is identified by an Autonomous System Number (ASN).

ASNs are assigned by Regional Internet Registries (RIRs), directly or through their allocation processes.

Examples of RIRs include:

* APNIC.
* ARIN.
* RIPE NCC.
* LACNIC.
* AFRINIC.

### 🖆 Autonomous System Number Ranges

ASNs are 32-bit values.

| ASN Range | Purpose |
| --- | --- |
| `0` | Reserved |
| `1–64495` | Public ASNs |
| `64496–64511` | Documentation |
| `64512–65534` | Private-use 16-bit ASN range |
| `65535` | Reserved |
| `65536–65551` | Documentation |
| `65552–4199999999` | Public ASNs |
| `4200000000–4294967294` | Private-use 32-bit ASN range |
| `4294967295` | Reserved |

The documentation ranges should be used in examples and documentation, not as real production assignments.

### 🖆 Public ASN

A public ASN is globally unique and used for routing between independently operated networks.

### 🖆 Private ASN

Private ASNs are used in environments where the ASN does not need to be globally unique.

They are commonly used in:

* Private enterprise networks.
* Internal BGP deployments.
* Lab environments.
* Certain provider customer designs.

Private ASNs must be handled appropriately when routes are advertised to the public internet.

# 🗁 BGP Characteristics

| Characteristic | Description |
| --- | --- |
| Protocol type | Path-vector |
| Routing category | Exterior Gateway Protocol |
| Transport | TCP |
| TCP port | 179 |
| Administrative distance | Cisco IOS defaults: eBGP 20, iBGP 200 |
| Primary metric | No single universal scalar metric |
| Path information | AS_PATH and other attributes |
| Address families | IPv4, IPv6, VPN and other supported families |
| Updates | Incremental after initial synchronization |
| Routing scope | Inter-domain and policy-controlled internal routing |

BGP uses TCP to provide reliable transport for routing messages.

A BGP session normally uses TCP port 179 on the listening peer.

# 🗁 BGP vs IGP

BGP and Interior Gateway Protocols serve different routing purposes.

IGPs are designed to exchange routing information within an autonomous system.

BGP is designed to exchange routing information between autonomous systems and apply routing policies.

| Characteristic | IGP | BGP |
| --- | --- | --- |
| Purpose | Internal routing | Inter-domain and policy-based routing |
| Examples | OSPF, RIP, IS-IS | BGP |
| Routing model | Link-state or distance-vector | Path-vector |
| Typical scope | Within an AS | Between ASes and internally through iBGP |
| Metric | Protocol-specific | Path attributes and policy |
| Transport | Protocol-specific | TCP port 179 |
| Convergence behavior | Usually optimized for internal topology changes | Policy-driven and dependent on path exploration and timers |

BGP can also operate within an autonomous system through iBGP.

# 🗁 eBGP and iBGP

BGP has two major peering types.

### 🖆 External BGP (eBGP)

eBGP is used between BGP routers in different autonomous systems.

Example:

```text
          Router A                 Router B
           AS 65001                 AS 65002
               |                       |
               +--------- eBGP --------+
```

Common characteristics:

* Peers belong to different ASNs.
* Commonly used between ISPs and between an ISP and its customer.
* Advertises routes between autonomous systems.
* Normally uses an external neighbor relationship.

### 🖆 Internal BGP (iBGP)

iBGP is used between BGP routers within the same autonomous system.

Example:

```text
                 AS 65001
          +---------------------+
          |                     |
       Router A ------------ Router B
          |                     |
          +--------- iBGP ------+
```

Common characteristics:

* Both peers belong to the same ASN.
* Distributes BGP routing information within an autonomous system.
* Uses the same BGP protocol.
* Has different route propagation rules from eBGP.

### 🖆 eBGP vs iBGP

| Characteristic | eBGP | iBGP |
| --- | --- | --- |
| ASN relationship | Different ASNs | Same ASN |
| Typical purpose | Inter-AS routing | Internal BGP distribution |
| AS_PATH | Local AS is normally added when advertising externally | Normally unchanged when advertising to another iBGP peer |
| Next-hop behavior | Commonly changes to the advertising router's address | Normally preserved |
| Default Cisco IOS AD | 20 | 200 |
| Peering TTL | Commonly 1 for directly connected peers | Commonly 255 by default on Cisco IOS |

The TTL behavior can be changed using features such as eBGP multihop.

# 🗁 BGP Peering

BGP peering is the relationship established between two BGP routers to exchange routing information.

The routers are called BGP peers or neighbors.

A BGP session requires:

* Reachability between peer addresses.
* Correct local and remote ASNs.
* Compatible BGP configuration.
* TCP connectivity on port 179.
* Compatible address-family configuration.
* Appropriate authentication settings when configured.
* No firewall rule blocking the session.

### 🖆 Direct eBGP Peering

Two routers in different autonomous systems establish a session over a directly connected link.

```text
Router A                         Router B
AS 65001                         AS 65002
10.0.12.1/30                     10.0.12.2/30
     |                                |
     +------------- TCP 179 ----------+
```

### 🖆 Loopback Peering

BGP can use loopback addresses as peer endpoints.

This is common in larger networks, particularly for iBGP.

Loopback peering requires IP reachability between the loopback addresses and appropriate source-address configuration.

# 🗁 BGP Neighbor Establishment

BGP peers establish a session through a series of states.

### 🖆 BGP State Progression

```text
Idle
  |
  v
Connect
  |
  v
Active
  |
  v
OpenSent
  |
  v
OpenConfirm
  |
  v
Established
```

The exact transitions can involve retries and return to earlier states when a connection attempt fails.

### 🖆 BGP States

| State | Description |
| --- | --- |
| Idle | BGP is initializing or waiting for the next connection attempt |
| Connect | Waiting for TCP connection establishment |
| Active | Attempting to establish a TCP connection |
| OpenSent | OPEN message has been sent |
| OpenConfirm | OPEN message has been received and KEEPALIVE exchange is pending |
| Established | Session is operational and routing information can be exchanged |

### 🖆 Established State

Established is the operational state in which BGP peers can exchange routing information.

If a session remains in Active or Connect, common causes include:

* Incorrect peer IP address.
* Incorrect ASN.
* Missing route to the peer.
* Firewall blocking TCP port 179.
* Incorrect source address.
* TTL or multihop configuration problems.
* Authentication mismatch.

# 🗁 BGP Message Types

BGP uses several message types to establish sessions and exchange routing information.

| Type | Message | Purpose |
| --- | --- | --- |
| 1 | OPEN | Establishes a BGP session and exchanges capabilities |
| 2 | UPDATE | Advertises and withdraws routes |
| 3 | NOTIFICATION | Reports errors and terminates a session |
| 4 | KEEPALIVE | Maintains the session |
| 5 | ROUTE-REFRESH | Requests a new advertisement of routes for an address family |

### 🖆 OPEN

The OPEN message contains information such as:

* BGP version.
* Sender's ASN.
* Hold Time.
* BGP Identifier.
* Optional parameters and capabilities.

### 🖆 UPDATE

The UPDATE message advertises new routes, withdraws previously advertised routes, and carries path attributes.

### 🖆 NOTIFICATION

A NOTIFICATION message reports a protocol error and normally causes the BGP session to close.

### 🖆 KEEPALIVE

KEEPALIVE messages maintain the session when there are no UPDATE messages to send.

### 🖆 ROUTE-REFRESH

ROUTE-REFRESH allows a router to request route advertisements again without requiring a full session reset, provided the peer supports the capability.

# 🗁 BGP Path Attributes

BGP path attributes describe routing information and influence path selection.

They are an important part of BGP's policy-based decision process.

### 🖆 Well-Known Mandatory Attributes

These attributes must be recognized and included in applicable UPDATE messages.

| Attribute | Description |
| --- | --- |
| ORIGIN | Identifies how the route originated in BGP |
| AS_PATH | Lists autonomous systems through which the route has passed |
| NEXT_HOP | Identifies the next-hop address used to reach the destination |

### 🖆 Well-Known Discretionary Attributes

These attributes must be recognized but are not required in every UPDATE message.

| Attribute | Description |
| --- | --- |
| LOCAL_PREF | Expresses preference for a route within an autonomous system |
| ATOMIC_AGGREGATE | Indicates that aggregation may have removed path information |

### 🖆 Optional Transitive Attributes

These attributes may be passed between BGP autonomous systems.

| Attribute | Description |
| --- | --- |
| AGGREGATOR | Identifies the router and ASN that performed aggregation |
| COMMUNITY | Labels routes for routing policy |
| EXTENDED COMMUNITY | Provides an extended community attribute format |
| LARGE_COMMUNITY | Supports larger structured community values |

### 🖆 Optional Non-Transitive Attributes

| Attribute | Description |
| --- | --- |
| MED | Suggests a preferred entry point into an autonomous system |
| ORIGINATOR_ID | Identifies the originator of a route in route-reflector environments |
| CLUSTER_LIST | Records route-reflector cluster IDs traversed by a route |

The exact attribute set and usage depend on the address family and BGP implementation.

# 🗁 BGP Path Selection

BGP may learn multiple paths to the same destination prefix.

It must choose the best path according to its path-selection process and configured policies.

Unlike OSPF, BGP does not simply choose the path with the lowest total link cost.

BGP considers path attributes such as:

* Local Preference.
* AS_PATH.
* ORIGIN.
* MED.
* eBGP or iBGP path type.
* IGP cost to the NEXT_HOP.
* Router ID and other tie-breakers.

### 🖆 Common Path-Selection Preferences

The following is a conceptual summary of common Cisco IOS best-path behavior. Exact decision sequences and tie-breakers vary by vendor and implementation.

| Attribute or Factor | Common Preference |
| --- | --- |
| Weight on Cisco IOS | Higher preferred |
| LOCAL_PREF | Higher preferred |
| Locally originated route | Commonly preferred over learned routes |
| AS_PATH | Shorter preferred |
| ORIGIN | IGP preferred over EGP, then INCOMPLETE |
| MED | Lower preferred when comparison rules permit |
| eBGP vs iBGP | eBGP commonly preferred |
| IGP cost to NEXT_HOP | Lower preferred |
| Additional tie-breakers | Implementation-specific |

### 🖆 Weight

Weight is a Cisco-specific attribute.

It is locally significant and is not advertised to other BGP peers.

A higher weight is preferred.

### 🖆 Local Preference

LOCAL_PREF is used within an autonomous system to express the preferred outbound path.

A higher LOCAL_PREF is preferred.

Example:

| Route | LOCAL_PREF |
| --- | ---: |
| Path A | 200 |
| Path B | 100 |

Path A is preferred under the usual BGP decision process.

### 🖆 AS_PATH

AS_PATH records the sequence of ASNs a route has traversed.

A shorter AS_PATH is commonly preferred when earlier selection criteria are equal.

AS_PATH also helps prevent routing loops: a router normally rejects a route containing its own ASN in the AS_PATH.

### 🖆 ORIGIN

ORIGIN indicates how a route was introduced into BGP.

Common values:

| ORIGIN | Meaning |
| --- | --- |
| IGP | Originated through a BGP network statement or equivalent process |
| EGP | Historical EGP origin; rarely used |
| INCOMPLETE | Originated through redistribution or another process |

The ORIGIN attribute is distinct from the routing protocol currently used inside the network.

### 🖆 MED

Multi-Exit Discriminator is used to suggest a preferred entry point into an autonomous system.

A lower MED is generally preferred when the comparison is applicable.

MED comparison rules can differ between implementations and configurations, particularly when paths originate from different neighboring ASes.

### 🖆 NEXT_HOP

NEXT_HOP identifies the next-hop address used to reach a BGP destination.

The next hop must be resolvable through the routing system for the route to be usable.

An unreachable BGP next hop can prevent a route from becoming the selected forwarding route.

# 🗁 BGP Routing Information Base

BGP maintains routing information in several logical data structures.

Common terminology includes:

| Component | Description |
| --- | --- |
| Adj-RIB-In | Routes received from a peer before or during inbound policy processing, depending on implementation |
| Loc-RIB | Local BGP routing information selected by the BGP decision process |
| Adj-RIB-Out | Routes selected for advertisement to a particular peer |

These are logical RIB concepts described by the BGP specification. Actual implementations may optimize or organize storage differently.

### 🖆 BGP Table vs Routing Table

The BGP table contains BGP-learned path information.

The main routing table contains routes selected by the overall routing system.

A BGP route may exist in the BGP table but not be installed in the main routing table because:

* Its next hop is unreachable.
* Another routing source provides a preferred route.
* It is not the selected BGP best path.
* A routing policy rejects it.
* The route is otherwise ineligible.

# 🗁 BGP Route Advertisement

BGP advertises IP prefixes to neighboring routers using UPDATE messages.

A router may advertise a prefix if:

* The prefix exists in an eligible routing source.
* The BGP configuration allows its advertisement.
* Export policies permit the route.
* The address family is activated.
* The route passes relevant validation and policy checks.

### 🖆 Example

AS 65001 announces:

```text
203.0.113.0/24
```

to AS 65002.

```text
AS 65001                         AS 65002
     |                                |
     |------ 203.0.113.0/24 --------->|
     |                                |
     +------------ eBGP --------------+
```

AS 65002 can then use the received route according to its BGP decision process and routing policies.

### 🖆 Route Origination

Routes can enter BGP through several mechanisms:

* Network statements.
* Redistribution from another routing source.
* Aggregate route configuration.
* Static or connected route origination, depending on implementation.

A route should not be advertised merely because an administrator intends to announce it. The router must have a valid route and an appropriate export policy.

# 🗁 BGP Route Filtering

BGP route filtering controls which routes a router accepts, rejects, or advertises.

Filtering is critical for preventing accidental route leaks and protecting routing infrastructure.

Common filtering mechanisms include:

* Prefix lists.
* AS-path filters.
* Community filters.
* Route maps.
* Routing filters.
* Import and export policies.

### 🖆 Prefix Filtering

A prefix filter controls which destination prefixes are accepted or advertised.

Example policy:

```text
Permit: 203.0.113.0/24
Reject: All other prefixes
```

### 🖆 AS_PATH Filtering

AS_PATH filters match routes according to the ASNs in their AS_PATH attribute.

They can be used to reject routes from specific autonomous systems or apply policy to selected paths.

### 🖆 Inbound Filtering

Inbound filtering controls routes received from a BGP neighbor.

### 🖆 Outbound Filtering

Outbound filtering controls routes advertised to a BGP neighbor.

### 🖆 Important Considerations

* Apply explicit import and export policies.
* Advertise only prefixes that the organization is authorized to announce.
* Validate customer route announcements.
* Prevent private or unintended prefixes from reaching the public internet.
* Use maximum-prefix limits where supported.
* Review filtering changes carefully.

# 🗁 BGP Route Aggregation

Route aggregation combines multiple more-specific prefixes into a larger summary prefix.

It reduces routing-table size and can simplify routing information exchange.

### 🖆 Example

Suppose an organization owns these networks:

```text
198.51.100.0/24
198.51.101.0/24
198.51.102.0/24
198.51.103.0/24
```

These four contiguous, correctly aligned networks can be summarized as:

```text
198.51.100.0/22
```

### 🖆 Aggregation Diagram

```text
198.51.100.0/24
198.51.101.0/24
198.51.102.0/24
198.51.103.0/24
        |
        v
198.51.100.0/22
```

### 🖆 Important Considerations

* Prefixes must be contiguous.
* The aggregate must be correctly aligned.
* The summary must not unintentionally include address space outside the intended allocation.
* More-specific route advertisements may still be needed.
* An aggregate route should generally be advertised only when the intended aggregate is valid.

# 🗁 BGP Communities

BGP communities are optional transitive path attributes used to label routes.

They allow networks to apply routing policies based on community values rather than matching every prefix individually.

### 🖆 Standard Communities

A standard BGP community is a 32-bit value, commonly represented as:

```text
65001:100
```

The two-part representation is a conventional administrative format, often using an ASN and a local value.

### 🖆 Common Well-Known Communities

| Community | Purpose |
| --- | --- |
| `NO_EXPORT` | Prevents advertisement outside the local autonomous system or confederation boundary according to its defined scope |
| `NO_ADVERTISE` | Prevents advertisement to any BGP peer |
| `NO_EXPORT_SUBCONFED` | Restricts advertisement outside the local AS confederation |
| `NOPEER` | Restricts advertisement to bilateral or settlement-free peers according to its defined scope |

### 🖆 Large Communities

Large Communities provide a 96-bit structure:

```text
Global Administrator : Local Data 1 : Local Data 2
```

They are useful in networks with 32-bit ASNs and more complex community-based policies.

### 🖆 Common Uses

* Identify customer routes.
* Identify peer-learned routes.
* Apply local preference policies.
* Control route propagation.
* Request provider-specific routing actions.
* Simplify large routing-policy configurations.

Community meanings are often defined by the network operator or ISP. Always consult the provider's published community documentation.

# 🗁 BGP Multihoming

BGP multihoming connects an organization or network to more than one upstream provider.

It improves connectivity options and can provide redundancy and traffic-engineering capabilities.

### 🖆 Example

```text
                  Internet
                 /        \
                /          \
             ISP A        ISP B
                \          /
                 \        /
                Customer AS
                  65010
                     |
                 Customer LAN
```

The customer establishes BGP sessions with both providers.

### 🖆 Benefits

* Upstream redundancy.
* Multiple routing paths.
* Inbound traffic engineering.
* Outbound traffic engineering.
* Improved control over routing policy.

### 🖆 Outbound Traffic Engineering

The customer controls which upstream path it uses to reach destinations.

Common techniques include:

* Local Preference.
* AS_PATH prepending.
* Route filtering.
* Policy-based route selection.

### 🖆 Inbound Traffic Engineering

The customer influences how external networks reach its prefixes.

Common techniques include:

* AS_PATH prepending.
* MED where applicable.
* Provider-specific communities.
* More-specific prefix announcements, subject to provider policy and global routing constraints.

Inbound traffic engineering is not fully controlled by the announcing network because remote networks apply their own routing policies.

# 🗁 BGP Route Reflectors

iBGP has a full-mesh requirement under its basic route propagation rules.

An iBGP router does not normally advertise a route learned from one iBGP peer to another iBGP peer.

This prevents loops but creates a scaling problem as the number of routers increases.

A Route Reflector (RR) reduces the need for a full mesh.

It receives routes from clients and reflects eligible routes to other clients and non-client peers according to BGP rules.

### 🖆 Without Route Reflectors

```text
        Router A
         / | \
        /  |  \
       /   |   \
  Router B | Router C
       \   |   /
        \  |  /
         Router D
```

A full mesh requires each iBGP router to peer with every other iBGP router.

### 🖆 With Route Reflector

```text
          Client A
              |
              |
          Route Reflector
              |
        +-----+-----+
        |           |
     Client B    Client C
```

### 🖆 Benefits

* Reduces iBGP session count.
* Improves scalability.
* Simplifies internal BGP topology.
* Supports hierarchical BGP designs.

Route reflectors introduce additional path-selection and path-visibility considerations. They should be designed carefully to avoid suboptimal routing.

# 🗁 BGP Confederations

BGP confederations divide a large autonomous system into smaller internal sub-autonomous systems.

The internal sub-ASes use eBGP-like sessions between themselves while the overall network presents a single external AS identity.

### 🖆 Example

```text
              Public AS 65000
                     |
          +----------+----------+
          |                     |
       Sub-AS 65001          Sub-AS 65002
          |                     |
       Router A              Router B
```

Confederations can reduce the complexity of large iBGP deployments.

They are an alternative scaling technique to route reflectors, although route reflectors are more commonly used in many modern networks.

# 🗁 BGP Address Families

BGP supports multiple address families through Multiprotocol BGP (MP-BGP).

The original BGP specification primarily described IPv4 unicast routing.

Multiprotocol extensions allow BGP to exchange routing information for additional address families.

Common address families include:

| Address Family | Purpose |
| --- | --- |
| IPv4 Unicast | IPv4 unicast routes |
| IPv6 Unicast | IPv6 unicast routes |
| IPv4 Multicast | IPv4 multicast routing information |
| VPNv4 | VPN-IPv4 routes, commonly used in MPLS L3VPN |
| VPNv6 | VPN-IPv6 routes |
| L2VPN EVPN | Ethernet VPN information |
| IPv4 Flowspec | IPv4 traffic filtering and flow specifications |
| IPv6 Flowspec | IPv6 traffic filtering and flow specifications |

### 🖆 MP-BGP

Multiprotocol BGP extends BGP using capabilities and path attributes that identify the network-layer protocol and address family.

This allows one BGP session to support multiple address families when both peers negotiate the necessary capabilities.

### 🖆 IPv4 and IPv6

A BGP session can exchange IPv4 and IPv6 routing information independently through their respective address families.

The transport connection and advertised address families are related but distinct concepts.

# 🗁 BGP Security

BGP security is essential because incorrect route advertisements can affect large portions of the internet.

Important security concerns include:

* Prefix hijacking.
* Route leaks.
* Incorrect route origination.
* Unauthorized BGP peering.
* Session disruption.
* Misconfigured route filters.

### 🖆 Prefix Hijacking

Prefix hijacking occurs when an unauthorized network announces a prefix belonging to another network.

Other networks may accept the announcement and send traffic toward the unauthorized origin.

### 🖆 Route Leaks

A route leak occurs when routing information is propagated beyond its intended scope or contrary to the agreed routing policy.

### 🖆 Route Origin Validation

Resource Public Key Infrastructure (RPKI) helps networks validate whether an autonomous system is authorized to originate a prefix.

A Route Origin Authorization (ROA) defines:

* An IP prefix.
* An authorized origin ASN.
* An optional maximum prefix length.

A BGP route can be classified as:

| State | Meaning |
| --- | --- |
| Valid | Origin ASN and prefix length are authorized |
| Invalid | Route conflicts with available ROA data |
| NotFound | No covering ROA is available |

RPKI origin validation does not validate the complete AS_PATH and does not prevent every type of route leak or hijack.

### 🖆 Additional Security Practices

* Use strict prefix filters.
* Validate customer announcements.
* Apply maximum-prefix limits.
* Use TCP authentication mechanisms where supported.
* Restrict access to TCP port 179.
* Use TTL security mechanisms where supported.
* Monitor unexpected AS_PATH changes.
* Implement RPKI Route Origin Validation.
* Use appropriate route-leak prevention policies.

# 🗁 BGP Configuration on Linux

Linux can run BGP using routing software such as FRRouting (FRR).

FRR provides BGP, OSPF, OSPFv3, RIP, and other routing protocols.

### 🖆 Install FRRouting on Ubuntu

```bash
sudo apt update
sudo apt install frr frr-pythontools
```

### 🖆 Enable the BGP Daemon

Edit:

```bash
sudo nano /etc/frr/daemons
```

Set:

```text
bgpd=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

### 🖆 Example Topology

```text
Router A                          Router B
AS 65001                          AS 65002
10.0.12.1/30                      10.0.12.2/30
       |                                |
       +------------- eBGP -------------+
```

Assume Router A will advertise `192.0.2.0/24` and Router B will advertise `198.51.100.0/24`.

The prefixes below are documentation examples.

### 🖆 Configure Router A

Open FRR:

```bash
sudo vtysh
```

Configure BGP:

```text
configure terminal

router bgp 65001
 bgp router-id 1.1.1.1
 neighbor 10.0.12.2 remote-as 65002
 address-family ipv4 unicast
  network 192.0.2.0/24
 exit-address-family

end
write memory
```

The `network` statement requires the exact prefix to exist in the relevant routing table before FRR can originate it.

### 🖆 Configure Router B

```text
configure terminal

router bgp 65002
 bgp router-id 2.2.2.2
 neighbor 10.0.12.1 remote-as 65001
 address-family ipv4 unicast
  network 198.51.100.0/24
 exit-address-family

end
write memory
```

Ensure the advertised prefix exists in Router B's routing table.

### 🖆 Verify BGP

```text
show ip bgp summary
show ip bgp
show ip bgp neighbors
show ip route bgp
```

### 🖆 Inspect Linux Routes

```bash
ip route show
```

BGP routes installed by FRR should be visible through the Linux routing integration when they are selected and installed.

# 🗁 BGP Configuration on MikroTik RouterOS

MikroTik RouterOS v7 supports BGP through its routing configuration system.

RouterOS v7 uses BGP connection and template configuration rather than the older RouterOS v6 peer model.

### 🖆 Example Topology

```text
Router A                          Router B
AS 65001                          AS 65002
10.0.12.1/30                      10.0.12.2/30
       |                                |
       +------------- eBGP -------------+
```

Assume:

* Router A ASN: `65001`
* Router B ASN: `65002`
* Router A address: `10.0.12.1`
* Router B address: `10.0.12.2`
* Router A advertises `192.0.2.0/24`
* Router B advertises `198.51.100.0/24`

### 🖆 Configure Router A

Assign the interface address:

```routeros
/ip address
add address=10.0.12.1/30 interface=ether1
```

Create a BGP connection:

```routeros
/routing bgp connection
add name=to-router-b \
    local.role=ebgp \
    local.as=65001 \
    remote.address=10.0.12.2/32 \
    remote.as=65002 \
    output.network=bgp-networks
```

Create a network list:

```routeros
/ip firewall address-list
add list=bgp-networks address=192.0.2.0/24
```

The network must also exist as an eligible route in the routing table for advertisement.

### 🖆 Configure Router B

Assign the interface address:

```routeros
/ip address
add address=10.0.12.2/30 interface=ether1
```

Create a BGP connection:

```routeros
/routing bgp connection
add name=to-router-a \
    local.role=ebgp \
    local.as=65002 \
    remote.address=10.0.12.1/32 \
    remote.as=65001 \
    output.network=bgp-networks
```

Create a network list:

```routeros
/ip firewall address-list
add list=bgp-networks address=198.51.100.0/24
```

Ensure the advertised prefix exists in the routing table.

### 🖆 Verify BGP

```routeros
/routing bgp session print detail
/routing bgp connection print detail
/routing route print where belongs-to~"bgp"
```

The exact route display properties can vary across RouterOS v7 releases.

### 🖆 Important Considerations

* Configure unique Router IDs.
* Verify the local and remote ASNs.
* Ensure TCP port 179 is reachable.
* Advertise only intended prefixes.
* Configure import and output filters.
* Check that advertised prefixes exist in the routing table.
* Use appropriate firewall rules.
* RouterOS v6 and v7 BGP configuration syntax differ significantly.

# 🗁 BGP Configuration on Cisco IOS

Cisco IOS commonly configures BGP using the `router bgp` command.

### 🖆 Example Topology

```text
Router A                          Router B
AS 65001                          AS 65002
10.0.12.1/30                      10.0.12.2/30
       |                                |
       +------------- eBGP -------------+
```

### 🖆 Configure Router A

Configure the interface:

```cisco
interface GigabitEthernet0/0
 ip address 10.0.12.1 255.255.255.252
 no shutdown
```

Configure BGP:

```cisco
router bgp 65001
 bgp router-id 1.1.1.1
 neighbor 10.0.12.2 remote-as 65002
 network 192.0.2.0 mask 255.255.255.0
```

The exact prefix must exist in the routing table for the `network` statement to originate it.

### 🖆 Configure Router B

```cisco
interface GigabitEthernet0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
```

Configure BGP:

```cisco
router bgp 65002
 bgp router-id 2.2.2.2
 neighbor 10.0.12.1 remote-as 65001
 network 198.51.100.0 mask 255.255.255.0
```

### 🖆 Verify BGP

```cisco
show ip bgp summary
show ip bgp
show ip bgp neighbors
show ip route bgp
```

### 🖆 View a Specific Prefix

```cisco
show ip bgp 192.0.2.0
```

# 🗁 BGP Troubleshooting

BGP troubleshooting should begin with session establishment and then move to route exchange, path selection, and forwarding.

### 🖆 Check BGP Session State

Cisco IOS:

```cisco
show ip bgp summary
```

FRR:

```text
show ip bgp summary
```

MikroTik RouterOS:

```routeros
/routing bgp session print detail
```

A session in Established state indicates that the BGP connection is operational.

### 🖆 Check TCP Connectivity

```bash
nc -vz 10.0.12.2 179
```

This checks TCP connectivity to the peer's BGP listening port. It does not independently prove that the BGP session configuration is correct.

### 🖆 Check IP Reachability

```bash
ping 10.0.12.2
```

Check the route to the peer:

```bash
ip route get 10.0.12.2
```

### 🖆 Check BGP Routes

Cisco IOS:

```cisco
show ip bgp
show ip bgp neighbors
show ip route bgp
```

FRR:

```text
show ip bgp
show ip bgp neighbors
show ip route bgp
```

MikroTik:

```routeros
/routing route print
/routing bgp session print detail
```

### 🖆 Common BGP Problems

| Problem | Possible Cause |
| --- | --- |
| Session remains Idle | BGP disabled, incorrect peer configuration, or administrative shutdown |
| Session remains Active | TCP connection failure, firewall, missing route, or incorrect source address |
| Session resets repeatedly | Hold timer expiration, unstable transport, authentication issue, or peer instability |
| Session Established but no routes | Address family not activated, route filters, or no eligible prefixes |
| Prefix received but not installed | Unreachable next hop, competing route, or policy |
| Prefix not advertised | Missing local route, export filter, or incorrect address-family configuration |
| Unexpected route selected | Local Preference, AS_PATH, MED, or other path-selection factors |
| Traffic follows wrong upstream | Incorrect import/export policy or next-hop resolution |

### 🖆 Inspect BGP Packets

```bash
sudo tcpdump -ni eth0 tcp port 179
```

Wireshark display filter:

```text
bgp
```

### 🖆 Troubleshooting Workflow

```text
       Check Peer IP Reachability
                   |
                   v
        Check TCP Port 179
                   |
                   v
       Check Local and Remote ASN
                   |
                   v
        Check BGP Session State
                   |
                   v
       Check Address-Family State
                   |
                   v
       Check Received Prefixes
                   |
                   v
       Check Best-Path Selection
                   |
                   v
       Check Installed Routes
                   |
                   v
       Verify End-to-End Forwarding
```

# 🗁 BGP Best Practices

* Use unique and correctly assigned ASNs.
* Establish explicit import and export policies.
* Advertise only authorized IP prefixes.
* Validate customer route announcements.
* Use RPKI Route Origin Validation where available.
* Configure maximum-prefix limits.
* Protect BGP sessions with supported authentication and security mechanisms.
* Monitor BGP session state and route changes.
* Use communities to simplify routing policy.
* Avoid unnecessary route redistribution.
* Plan multihoming and failover policies carefully.
* Maintain clear documentation of peer addresses, ASNs, and routing policies.
* Use route reflectors for scalable iBGP designs where appropriate.
* Validate inbound and outbound traffic engineering behavior.
* Test policy changes before applying them to production networks.

# 🗁 Common BGP Mistakes

| Mistake | Correct Understanding |
| --- | --- |
| BGP is a link-state protocol. | BGP is a path-vector routing protocol. |
| BGP uses UDP port 179. | BGP uses TCP port 179. |
| BGP always selects the shortest AS_PATH. | Earlier decision criteria, policy, and implementation-specific rules can override AS_PATH length. |
| BGP is only used between ISPs. | Enterprises, cloud providers, data centers, and other networks also use BGP. |
| iBGP and eBGP are identical in behavior. | They have different AS relationships and route propagation rules. |
| An Established session guarantees routes are exchanged. | Address-family activation and routing policies also matter. |
| A BGP route is always installed in the main routing table. | The route must be eligible and selected by the routing system. |
| MED always influences path selection across every neighboring AS. | MED comparison depends on policy and implementation rules. |
| RPKI prevents every BGP hijack. | RPKI origin validation helps validate route origins but does not validate the entire AS_PATH or prevent all route leaks. |
| More-specific prefixes are always accepted. | Providers and networks commonly apply prefix-length filters. |

# 🗁 Interview Questions

### 🖆 What is BGP?

BGP is a path-vector Exterior Gateway Protocol used to exchange routing information between autonomous systems.

### 🖆 What is an autonomous system?

An autonomous system is a collection of IP networks operated under a common routing administration and policy.

### 🖆 What is an ASN?

An Autonomous System Number identifies an autonomous system in inter-domain routing.

### 🖆 What is the difference between eBGP and iBGP?

eBGP exchanges routes between different autonomous systems, while iBGP distributes BGP routes within the same autonomous system.

### 🖆 Which transport protocol does BGP use?

BGP uses TCP port 179.

### 🖆 What are the BGP states?

Idle, Connect, Active, OpenSent, OpenConfirm, and Established.

### 🖆 What is AS_PATH?

AS_PATH is a BGP path attribute containing the sequence of autonomous systems through which a route has passed.

### 🖆 What is Local Preference?

Local Preference is an attribute used within an autonomous system to indicate the preferred outbound path. A higher value is generally preferred.

### 🖆 What is MED?

Multi-Exit Discriminator is an attribute used to suggest a preferred entry point into an autonomous system. A lower value is generally preferred when comparison rules permit.

### 🖆 What is the difference between Weight and Local Preference?

Weight is Cisco-specific and locally significant. Local Preference is a BGP attribute distributed within an autonomous system.

### 🖆 What is a BGP Route Reflector?

A Route Reflector reduces the number of iBGP sessions by reflecting eligible routes between clients and other BGP peers.

### 🖆 What is BGP multihoming?

BGP multihoming connects a network to multiple upstream providers for redundancy and routing-policy control.

### 🖆 What is route aggregation?

Route aggregation combines multiple more-specific prefixes into a larger summary prefix.

### 🖆 What are BGP communities?

BGP communities are attributes used to label routes and simplify routing-policy decisions.

### 🖆 What is RPKI?

Resource Public Key Infrastructure provides cryptographic authorization information that can be used to validate BGP route origins.

### 🖆 What is a route leak?

A route leak occurs when routing information is propagated beyond its intended scope or contrary to the intended routing policy.

### 🖆 Why can a BGP route be received but not installed?

Possible reasons include an unreachable next hop, route-selection preference, filtering, or another route being preferred by the routing system.

### 🖆 What is the difference between the BGP table and the routing table?

The BGP table contains BGP path information. The routing table contains routes selected by the overall routing system for forwarding.

# 🗁 Summary

BGP is the primary inter-domain routing protocol used by the global internet.

Key concepts:

* BGP is a path-vector Exterior Gateway Protocol.
* It exchanges routing information between autonomous systems.
* It uses TCP port 179.
* eBGP connects different autonomous systems.
* iBGP distributes BGP routes within the same autonomous system.
* AS_PATH records the autonomous systems traversed by a route.
* Local Preference influences outbound path selection.
* MED can influence inbound path selection by neighboring networks.
* Route Reflectors improve iBGP scalability.
* Communities support flexible routing policies.
* MP-BGP supports multiple address families.
* RPKI helps validate route origins.
* FRRouting, MikroTik RouterOS v7, and Cisco IOS support BGP configuration.

BGP is essential for ISP networking, internet routing, multihoming, traffic engineering, and large-scale network architecture.

# 🗁 References

* [RFC 4271 — A Border Gateway Protocol 4 (BGP-4)](https://www.rfc-editor.org/rfc/rfc4271)
* [RFC 4760 — Multiprotocol Extensions for BGP-4](https://www.rfc-editor.org/rfc/rfc4760)
* [RFC 1997 — BGP Communities Attribute](https://www.rfc-editor.org/rfc/rfc1997)
* [RFC 4360 — BGP Extended Communities Attribute](https://www.rfc-editor.org/rfc/rfc4360)
* [RFC 8092 — BGP Large Communities Attribute](https://www.rfc-editor.org/rfc/rfc8092)
* [RFC 4456 — BGP Route Reflection](https://www.rfc-editor.org/rfc/rfc4456)
* [RFC 5065 — Autonomous System Confederations for BGP](https://www.rfc-editor.org/rfc/rfc5065)
* [RFC 6811 — BGP Prefix Origin Validation](https://www.rfc-editor.org/rfc/rfc6811)
* [RFC 8212 — Default External BGP Route Propagation Behavior](https://www.rfc-editor.org/rfc/rfc8212)
* [RFC 9234 — Route Leak Prevention and Detection Using Roles](https://www.rfc-editor.org/rfc/rfc9234)
* [FRRouting Documentation](https://docs.frrouting.org/)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [Cisco BGP Configuration Documentation](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/13753-25.html)
