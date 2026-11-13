# BGP Configuration

BGP (Border Gateway Protocol) is a dynamic routing protocol used to exchange network reachability information between autonomous systems.

It is the primary routing protocol used to exchange routing information across the Internet.

MikroTik RouterOS v7 supports BGP for:

* ISP and service-provider routing.
* Internet transit connections.
* BGP peering with other networks.
* Multi-homed network designs.
* Route advertisement and filtering.
* Inter-AS routing.
* Enterprise networks with multiple autonomous systems.

Unlike OSPF, which primarily discovers network topology and calculates shortest paths, BGP exchanges reachability information and uses routing attributes and administrative policies to select paths.

This documentation focuses on BGP configuration in MikroTik RouterOS v7.

# 🗁 What Is an Autonomous System?

An Autonomous System (AS) is a collection of IP networks and routers operated under a common routing administration.

Each publicly participating autonomous system is identified by an Autonomous System Number (ASN).

Examples:

| Organization | ASN |
| --- | ---: |
| Google | AS15169 |
| Cloudflare | AS13335 |
| Amazon | AS16509 |
| Microsoft | AS8075 |

ASNs are used by BGP to identify routing domains and represent the path that routing information has traversed.

There are two commonly used ASN ranges:

**Public ASNs**

Used for identifying autonomous systems in the global Internet routing system.

**Private ASNs**

Used for private routing environments where the ASN does not need to be globally unique.

Common private ASN ranges include:

| Range | Type |
| --- | --- |
| 64512–65534 | 16-bit private ASNs |
| 4200000000–4294967294 | 32-bit private ASNs |

# 🗁 BGP Fundamentals

BGP is a path-vector routing protocol.

It exchanges network reachability information between BGP routers and includes attributes describing the path and routing policy.

Important BGP characteristics:

| Feature | Description |
| --- | --- |
| Protocol type | Path-vector |
| Transport protocol | TCP |
| TCP port | 179 |
| Administrative distance | Commonly 20 for eBGP and 200 for iBGP |
| IPv4 multicast | Not used for BGP peer discovery |
| Standard | RFC 4271 |
| Path information | AS_PATH |
| Network information | NLRI |
| Route selection | BGP best-path algorithm |

BGP uses TCP port `179` to establish reliable sessions between neighboring routers.

Unlike OSPF, BGP does not use IP protocol number 89.

# 🗁 eBGP vs. iBGP

BGP connections are commonly divided into two types.

**eBGP (External BGP)**

eBGP establishes routing relationships between routers belonging to different autonomous systems.

Example:

```text
       AS 65001                    AS 65002

   +-------------+             +-------------+
   |   Router A  |-------------|   Router B  |
   +-------------+             +-------------+
       eBGP Session
```

Common uses:

* ISP-to-ISP peering.
* ISP-to-customer connections.
* Internet transit connections.
* Exchange of routes between different organizations.

**iBGP (Internal BGP)**

iBGP establishes routing relationships between BGP routers belonging to the same autonomous system.

Example:

```text
                  AS 65001

       +-------------+       +-------------+
       |   Router A  |-------|   Router B  |
       +-------------+       +-------------+
                 iBGP Session
```

Common uses:

* Distributing external routes inside an organization.
* Connecting edge routers.
* Sharing routing information between internal BGP speakers.
* Building ISP backbone routing infrastructure.

# 🗁 BGP Attributes

BGP uses attributes to describe and evaluate routes.

Important attributes include:

| Attribute | Purpose |
| --- | --- |
| AS_PATH | Lists the autonomous systems traversed by a route |
| NEXT_HOP | Identifies the next-hop address |
| LOCAL_PREF | Indicates the preferred outbound path within an AS |
| MED | Suggests a preferred entry point into an AS |
| ORIGIN | Identifies how the route originated |
| COMMUNITY | Provides a way to group and apply routing policies |
| WEIGHT | RouterOS-specific local path preference |

**AS_PATH**

AS_PATH records the autonomous systems through which a route has passed.

Example:

```text
AS_PATH: 65010 65020 65030
```

This indicates that the route advertisement has traversed those ASNs in that sequence.

BGP commonly prefers a shorter AS_PATH when earlier selection criteria are equal.

**NEXT_HOP**

NEXT_HOP identifies the next-hop address used to reach a destination prefix.

A BGP route with an unreachable next hop cannot be used as a valid forwarding path.

**LOCAL_PREF**

LOCAL_PREF controls outbound path preference within an autonomous system.

A higher LOCAL_PREF is generally preferred.

It is normally exchanged between iBGP peers and is not advertised to eBGP peers.

**MED**

MED (Multi-Exit Discriminator) is used to suggest a preferred entry point into an autonomous system.

A lower MED is generally preferred when the relevant comparison conditions are met.

**COMMUNITY**

BGP communities are tags attached to routes.

They allow network operators to classify routes and apply routing policies without maintaining a separate rule for every prefix.

# 🗁 BGP Message Types

BGP uses four main message types.

| Message | Purpose |
| --- | --- |
| OPEN | Establishes a BGP session and negotiates capabilities |
| UPDATE | Advertises or withdraws routes |
| KEEPALIVE | Maintains the session |
| NOTIFICATION | Reports an error and may terminate the session |

A simplified BGP session process:

```text
TCP Connection
      |
      v
   OPEN Message
      |
      v
 Capability Negotiation
      |
      v
 KEEPALIVE Exchange
      |
      v
 Established Session
      |
      v
 UPDATE Messages
      |
      v
 Route Advertisement
```

Once the session is established, peers exchange routing updates.

# 🗁 BGP Session States

BGP sessions pass through several states during connection establishment.

| State | Description |
| --- | --- |
| Idle | BGP is waiting to start or restart the connection |
| Connect | Waiting for the TCP connection to establish |
| Active | Attempting to establish a TCP connection |
| OpenSent | OPEN message has been sent |
| OpenConfirm | Waiting for a KEEPALIVE or NOTIFICATION |
| Established | Session is established and routes can be exchanged |

The expected operational state is:

`Established`

A session that remains in `Active` or repeatedly returns to `Idle` usually indicates a connectivity, configuration, or compatibility problem.

# 🗁 BGP Configuration Structure in RouterOS v7

RouterOS v7 redesigned its BGP configuration.

The main configuration menus include:

| Menu | Purpose |
| --- | --- |
| `/routing/bgp/instance` | Defines a BGP routing instance and local AS |
| `/routing/bgp/connection` | Configures BGP peer connections |
| `/routing/bgp/template` | Defines reusable BGP protocol settings |
| `/routing/bgp/session` | Displays active and cached BGP session information |

The `instance` menu is explicitly used in RouterOS v7.20 and later.

Older RouterOS v7 releases have differences in how BGP instances are determined and configured. Check the syntax supported by the installed RouterOS version before applying configurations.

# 🗁 Creating a BGP Instance

A BGP instance identifies the local autonomous system and provides the routing context for BGP sessions.

Example:

```routeros
/routing/bgp/instance/add \
    name=AS65001 \
    as=65001
```

This creates a BGP instance using AS65001.

Inspect the instance:

```routeros
/routing/bgp/instance/print detail
```

Use a unique and consistent AS number for the routing domain.

# 🗁 Configuring an eBGP Connection

An eBGP connection is established between routers belonging to different autonomous systems.

Assume:

| Parameter | Router 1 | Router 2 |
| --- | --- | --- |
| Router IP | `10.0.12.1` | `10.0.12.2` |
| Local AS | 65001 | 65002 |
| Remote AS | 65002 | 65001 |
| Interface | ether1 | ether1 |

The routers are directly connected through the `10.0.12.0/30` network.

**Router 1 Configuration**

Create the BGP instance:

```routeros
/routing/bgp/instance/add \
    name=AS65001 \
    as=65001
```

Create the eBGP connection:

```routeros
/routing/bgp/connection/add \
    name=to-AS65002 \
    instance=AS65001 \
    local.address=10.0.12.1 \
    local.role=ebgp \
    remote.address=10.0.12.2 \
    remote.as=65002
```

**Router 2 Configuration**

Create the BGP instance:

```routeros
/routing/bgp/instance/add \
    name=AS65002 \
    as=65002
```

Create the eBGP connection:

```routeros
/routing/bgp/connection/add \
    name=to-AS65001 \
    instance=AS65002 \
    local.address=10.0.12.2 \
    local.role=ebgp \
    remote.address=10.0.12.1 \
    remote.as=65001
```

The `local.role=ebgp` property identifies the connection as an external BGP session.

The `remote.as` property specifies the expected remote autonomous system.

# 🗁 Verifying BGP Sessions

Display configured BGP connections:

```routeros
/routing/bgp/connection/print detail
```

Display active BGP sessions:

```routeros
/routing/bgp/session/print
```

Display detailed session information:

```routeros
/routing/bgp/session/print detail
```

Look for the `E` flag, which indicates an established session.

Useful information includes:

* Local and remote addresses.
* Local and remote AS numbers.
* Session state.
* Negotiated capabilities.
* Session uptime.
* Messages sent and received.
* Prefix count.

# 🗁 Advertising Networks Through BGP

BGP advertises network reachability information to its peers.

RouterOS v7 uses an address list and BGP output configuration to identify networks for advertisement.

A matching route must exist in the routing table for the network to be advertised.

Assume Router 1 owns:

```text
192.168.10.0/24
```

First, create an address list:

```routeros
/ip/firewall/address-list/add \
    list=bgp-networks \
    address=192.168.10.0/24
```

Ensure that the corresponding route exists in the routing table.

For example, if the network is configured on a LAN interface, RouterOS normally creates a connected route automatically.

Configure the BGP connection to advertise the address list:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    output.network=bgp-networks
```

The network is eligible for advertisement when the corresponding route exists and the output policy permits it.

**Important:** Do not advertise prefixes that you do not own or are not authorized to announce.

# 🗁 BGP Output Filtering

BGP output filters control which routes are advertised to a peer.

They are essential for preventing accidental route advertisements.

Create an output filter chain:

```routeros
/routing/filter/rule/add \
    chain=bgp-out \
    rule="if (dst == 192.168.10.0/24) { accept }"
```

This rule accepts the specified prefix.

Configure the BGP connection to use the filter:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    output.filter-chain=bgp-out
```

When an output filter chain is configured, routes not accepted by its rules are rejected by the default filter action.

For a controlled advertisement policy, explicitly permit only the intended prefixes.

# 🗁 BGP Input Filtering

Input filters control which routes are accepted from a BGP peer.

They help protect the routing table from unwanted or unexpected advertisements.

Create an input filter:

```routeros
/routing/filter/rule/add \
    chain=bgp-in \
    rule="if (dst == 203.0.113.0/24) { accept }"
```

Configure the connection:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    input.filter=bgp-in
```

This example accepts only the specified prefix through the configured input filter.

For real deployments, define an explicit inbound prefix policy based on the routes expected from that neighbor.

Input filters and early input acceptance controls serve different purposes. A regular routing filter can reject routes after their NLRI has been processed, while `input.accept-nlri` can restrict which prefixes are accepted before full route processing.

# 🗁 BGP Best-Path Selection

A router may receive multiple paths to the same destination from different BGP peers.

BGP uses a best-path selection process to determine which path should be preferred.

Important selection criteria include:

| Attribute | General Preference |
| --- | --- |
| WEIGHT | Higher |
| LOCAL_PREF | Higher |
| AS_PATH | Shorter |
| ORIGIN | IGP preferred over EGP, then INCOMPLETE |
| MED | Lower, when applicable |
| Peer type | eBGP preferred over iBGP |
| IGP metric to next hop | Lower |
| Router ID | Lower, as a later tie-breaker |

The exact sequence and comparison behavior can depend on RouterOS implementation and configuration.

A route must also be valid. For example, its next hop must be reachable and it must not be rejected by routing filters.

Route distance is used when comparing routes from different routing sources or instances at the general routing-table level. It should not be confused with BGP's internal best-path selection among paths in the same BGP instance.

# 🗁 LOCAL_PREF Configuration

LOCAL_PREF is used to influence outbound path selection within an autonomous system.

A higher LOCAL_PREF makes a path more preferred.

For example, suppose an ISP has two upstream providers:

| Provider | LOCAL_PREF |
| --- | ---: |
| Provider A | 200 |
| Provider B | 100 |

The router prefers Provider A for outbound traffic when the compared routes are otherwise eligible.

A RouterOS v7 input filter can set LOCAL_PREF:

```routeros
/routing/filter/rule/add \
    chain=provider-a-in \
    rule="set bgp-local-pref 200; accept"
```

Configure the connection:

```routeros
/routing/bgp/connection/set \
    [find where name="to-provider-a"] \
    input.filter=provider-a-in
```

For Provider B:

```routeros
/routing/filter/rule/add \
    chain=provider-b-in \
    rule="set bgp-local-pref 100; accept"
```

This is a simplified policy example. Production filters should also restrict which prefixes are accepted.

# 🗁 AS_PATH Prepending

AS_PATH prepending adds additional occurrences of the local AS number to an advertised route.

It can make a route appear less attractive to external networks that use AS_PATH length in their path selection.

For example:

```text
Normal advertisement:
65001

After prepending:
65001 65001 65001
```

Configure an output filter:

```routeros
/routing/filter/rule/add \
    chain=bgp-prepend \
    rule="set bgp-path-prepend 2; accept"
```

Apply it to the BGP connection:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    output.filter-chain=bgp-prepend
```

The prepend setting adds additional local-AS occurrences to the advertised path according to the configured value.

AS_PATH prepending is commonly used to influence inbound traffic selection.

However, it does not guarantee that remote networks will select another path. Their local policies and other BGP attributes may take precedence.

# 🗁 iBGP Configuration

iBGP connects routers within the same autonomous system.

Assume:

| Parameter | Router 1 | Router 2 |
| --- | --- | --- |
| Local AS | 65001 | 65001 |
| Router IP | `10.0.12.1` | `10.0.12.2` |
| Remote IP | `10.0.12.2` | `10.0.12.1` |

Both routers belong to AS65001.

**Router 1**

```routeros
/routing/bgp/instance/add \
    name=AS65001 \
    as=65001

/routing/bgp/connection/add \
    name=to-Router2 \
    instance=AS65001 \
    local.address=10.0.12.1 \
    local.role=ibgp \
    remote.address=10.0.12.2 \
    remote.as=65001
```

**Router 2**

```routeros
/routing/bgp/instance/add \
    name=AS65001 \
    as=65001

/routing/bgp/connection/add \
    name=to-Router1 \
    instance=AS65001 \
    local.address=10.0.12.2 \
    local.role=ibgp \
    remote.address=10.0.12.1 \
    remote.as=65001
```

The routers must have IP connectivity to each other.

For iBGP sessions using loopback addresses, the network must provide reachability to those loopbacks, and the BGP connection must be configured with appropriate local and remote addresses.

# 🗁 iBGP Full Mesh

In a traditional iBGP design, all BGP routers within an autonomous system must establish iBGP sessions with one another or use route reflection.

This is known as the iBGP full-mesh requirement.

For example, three routers require these sessions:

```text
             Router A
             /      \
            /        \
           /          \
      Router B ------ Router C

              AS65001
```

Each router has an iBGP session with the other two routers.

For a small network, full mesh is manageable.

For larger networks, the number of required sessions grows quickly.

# 🗁 Route Reflectors

A route reflector reduces the need for a full iBGP mesh.

It receives routes from route-reflector clients and reflects eligible routes to other clients and peers according to BGP route-reflection rules.

Example:

```text
               Route Reflector
                 AS65001
                /    |    \
               /     |     \
              /      |      \
         Client A  Client B  Client C
```

Route reflection is commonly used in larger ISP and enterprise networks.

It simplifies iBGP session management and improves scalability.

# 🗁 BGP Multihop

By default, eBGP commonly operates between directly connected neighbors.

When the remote peer is reachable through one or more intermediate routers, a multihop configuration may be required.

For example:

```text
Router A -------- Router X -------- Router B
AS65001                             AS65002
```

A multihop connection can be configured using the `multihop` property.

Example:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    multihop=yes
```

The local and remote peer addresses must still be reachable through the routing table.

Use appropriate firewall restrictions and peer authentication for multihop BGP sessions.

# 🗁 BGP Authentication

BGP sessions can use TCP MD5 authentication to help protect the TCP connection between peers.

Configure the same authentication key on both routers.

Example:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    tcp-md5-key="Replace-With-A-Strong-Key"
```

Apply the matching key on the remote peer.

Authentication does not replace firewall restrictions or routing policy.

# 🗁 BGP Route Monitoring

RouterOS provides several commands for monitoring BGP operation.

Display BGP instances:

```routeros
/routing/bgp/instance/print detail
```

Display BGP connections:

```routeros
/routing/bgp/connection/print detail
```

Display active sessions:

```routeros
/routing/bgp/session/print
```

Display detailed session information:

```routeros
/routing/bgp/session/print detail
```

Display BGP routes:

```routeros
/routing/route/print where protocol=bgp
```

Display detailed BGP routes:

```routeros
/routing/route/print detail where protocol=bgp
```

Monitor routing statistics:

```routeros
/routing/stats/print
```

These commands help identify session state, received prefixes, route attributes, and routing-process activity.

# 🗁 Common BGP Problems

| Problem | Possible Cause |
| --- | --- |
| Session remains in Active | TCP connectivity or firewall problem |
| Session repeatedly disconnects | Hold-time expiration, unstable link, or peer resets |
| Authentication failure | Incorrect TCP MD5 key |
| No routes received | Remote advertisement policy or input filtering |
| Routes received but not active | Invalid next hop or route-selection issue |
| Expected route not advertised | Missing source route or output filter rejection |
| Wrong path selected | BGP attributes or routing policy |
| iBGP routes have unreachable next hops | Missing IGP reachability or next-hop handling |
| Duplicate Router ID | Conflicting BGP router identifiers |

# 🗁 BGP Troubleshooting

**Check session status**

```routeros
/routing/bgp/session/print detail
```

Look for:

* Established state.
* Correct local and remote AS numbers.
* Correct local and remote addresses.
* Session uptime.
* Prefix count.
* Last notification information.

**Check peer connectivity**

```routeros
/ping 10.0.12.2
```

Verify that the remote peer is reachable.

**Check TCP port 179**

Ensure that firewall policies permit the BGP TCP connection between the configured peer addresses.

**Check route filters**

```routeros
/routing/filter/rule/print detail
```

Verify that input and output filters permit the intended prefixes.

**Check learned routes**

```routeros
/routing/route/print where protocol=bgp
```

Verify that expected prefixes are present and that their next hops are reachable.

**Check connection configuration**

```routeros
/routing/bgp/connection/print detail
```

Verify:

* Local AS.
* Remote AS.
* Local address.
* Remote address.
* Local role.
* Routing instance.
* Authentication.
* Multihop settings.

# 🗁 Practical Lab: eBGP Between Two MikroTik Routers

**Objective:** Establish an eBGP session between two MikroTik routers and exchange LAN routes.

**Topology:**

```text
       LAN-A                                      LAN-B
   192.168.10.0/24                            192.168.20.0/24
         |                                           |
   +-----------+       10.0.12.0/30            +-----------+
   |  Router 1 |-------------------------------|  Router 2 |
   +-----------+                               +-----------+
         |                                           |
     AS65001                                     AS65002
```

**IP addressing plan:**

| Device | Interface | IP Address |
| --- | --- | --- |
| Router 1 | ether1 | `10.0.12.1/30` |
| Router 1 | bridge-LAN | `192.168.10.1/24` |
| Router 2 | ether1 | `10.0.12.2/30` |
| Router 2 | bridge-LAN | `192.168.20.1/24` |

Assume both LAN networks and the transit IP addresses are already configured.

**Step 1: Configure Router 1**

Create its BGP instance:

```routeros
/routing/bgp/instance/add \
    name=AS65001 \
    as=65001
```

Configure the BGP connection:

```routeros
/routing/bgp/connection/add \
    name=to-AS65002 \
    instance=AS65001 \
    local.address=10.0.12.1 \
    local.role=ebgp \
    remote.address=10.0.12.2 \
    remote.as=65002
```

Create an address list for Router 1's LAN:

```routeros
/ip/firewall/address-list/add \
    list=bgp-networks \
    address=192.168.10.0/24
```

Configure the connection to advertise the LAN:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65002"] \
    output.network=bgp-networks
```

**Step 2: Configure Router 2**

Create its BGP instance:

```routeros
/routing/bgp/instance/add \
    name=AS65002 \
    as=65002
```

Configure the BGP connection:

```routeros
/routing/bgp/connection/add \
    name=to-AS65001 \
    instance=AS65002 \
    local.address=10.0.12.2 \
    local.role=ebgp \
    remote.address=10.0.12.1 \
    remote.as=65001
```

Create an address list for Router 2's LAN:

```routeros
/ip/firewall/address-list/add \
    list=bgp-networks \
    address=192.168.20.0/24
```

Configure the connection to advertise the LAN:

```routeros
/routing/bgp/connection/set \
    [find where name="to-AS65001"] \
    output.network=bgp-networks
```

**Step 3: Verify BGP sessions**

On Router 1:

```routeros
/routing/bgp/session/print detail
```

On Router 2:

```routeros
/routing/bgp/session/print detail
```

Both routers should establish an eBGP session.

**Step 4: Verify advertised routes**

On Router 1:

```routeros
/routing/route/print detail where protocol=bgp
```

Router 1 should learn:

```text
192.168.20.0/24
```

On Router 2:

```routeros
/routing/route/print detail where protocol=bgp
```

Router 2 should learn:

```text
192.168.10.0/24
```

**Step 5: Test connectivity**

From Router 1:

```routeros
/ping 192.168.20.1
```

From Router 2:

```routeros
/ping 192.168.10.1
```

You can also test communication between clients in both LAN networks.

**Step 6: Inspect BGP attributes**

```routeros
/routing/route/print detail where protocol=bgp
```

Inspect:

* AS_PATH.
* NEXT_HOP.
* BGP route state.
* Origin information.
* Route selection.

**Firewall considerations**

BGP uses TCP port `179`.

If the input firewall restricts access to the router, allow BGP connections only from the trusted peer.

On Router 1:

```routeros
/ip/firewall/filter/add \
    chain=input \
    action=accept \
    protocol=tcp \
    src-address=10.0.12.2 \
    dst-port=179 \
    in-interface=ether1 \
    comment="Allow BGP from Router 2"
```

On Router 2:

```routeros
/ip/firewall/filter/add \
    chain=input \
    action=accept \
    protocol=tcp \
    src-address=10.0.12.1 \
    dst-port=179 \
    in-interface=ether1 \
    comment="Allow BGP from Router 1"
```

Place these rules before any input-chain rule that would otherwise drop the BGP traffic.

Also ensure that the forward-chain policy permits the intended traffic between the LAN networks.

**Lab observations:**

* Did both routers establish an eBGP session?
* Which AS number was received from the remote peer?
* Did Router 1 learn Router 2's LAN prefix?
* Did Router 2 learn Router 1's LAN prefix?
* What AS_PATH information appeared in the received routes?
* What happened when the BGP connection was disabled?

# 🗁 Best Practices

* Use the correct ASN for each autonomous system.
* Assign stable and unique BGP Router IDs.
* Restrict BGP sessions to trusted peer addresses.
* Use TCP MD5 authentication where appropriate.
* Apply explicit inbound and outbound routing filters.
* Advertise only authorized prefixes.
* Validate next-hop reachability.
* Understand LOCAL_PREF, MED, and AS_PATH before changing routing policy.
* Use route reflectors for scalable iBGP designs.
* Monitor BGP sessions and prefix counts.
* Avoid accepting unrestricted full routing tables unless the router has sufficient resources and the design requires them.
* Keep backups before changing production BGP policies.
* Test route advertisements and withdrawals in a controlled lab.

# 🗁 Summary

BGP is a path-vector routing protocol used to exchange network reachability information between autonomous systems.

Key points:

* eBGP connects different autonomous systems.
* iBGP connects routers within the same autonomous system.
* BGP uses TCP port `179`.
* AS_PATH identifies the autonomous systems traversed by a route.
* LOCAL_PREF influences outbound path selection within an AS.
* MED can influence inbound path selection.
* RouterOS v7 uses BGP instances, connections, templates, and sessions.
* Address lists and output configuration are used to advertise networks.
* Routing filters control inbound and outbound route policies.
* Route reflectors improve iBGP scalability.

BGP is essential for ISP networking, Internet peering, multi-homed networks, and large-scale routing infrastructure.
