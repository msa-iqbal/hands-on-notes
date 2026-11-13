# IPv6 Routing

IPv6 routing is the process of forwarding IPv6 packets between different IPv6 networks. Routers examine the destination IPv6 address, consult their IPv6 routing tables, select the most appropriate route, and forward packets toward their destinations.

IPv6 routing follows many of the same fundamental principles as IPv4 routing, including longest prefix match, connected routes, static routes, default routes, and dynamic routing protocols.

However, IPv6 introduces important differences, including 128-bit addresses, link-local next hops, Neighbor Discovery Protocol (NDP), Router Advertisements (RA), and the absence of broadcast addressing.

Understanding IPv6 routing is essential for designing, configuring, and troubleshooting modern enterprise, service-provider, and internet networks.

# 🗁 IPv6 Routing Overview

IPv6 routing allows routers to exchange packets between IPv6 networks.

An IPv6 router performs the following operations when forwarding a packet:

* Receives an IPv6 packet on an interface.
* Examines the destination IPv6 address.
* Searches its IPv6 routing table.
* Selects the best matching route.
* Determines the outgoing interface and next hop.
* Resolves the next-hop link-layer address using NDP when necessary.
* Forwards the packet through the selected interface.

Unlike IPv4, IPv6 routers do not fragment packets during forwarding. If a packet exceeds the outgoing link's MTU, the router drops it and sends an ICMPv6 Packet Too Big message when appropriate.

### 🖆 IPv4 vs IPv6 Routing

| Feature | IPv4 | IPv6 |
| --- | --- | --- |
| Address size | 32 bits | 128 bits |
| Address notation | Decimal | Hexadecimal |
| Routing prefix | CIDR | CIDR |
| Broadcast | Supported | Not supported |
| Neighbor resolution | ARP | NDP |
| Router discovery | Commonly configured manually or through DHCP-related mechanisms | Router Advertisements |
| Default route | `0.0.0.0/0` | `::/0` |
| Loopback | `127.0.0.1` | `::1` |
| Link-local range | `169.254.0.0/16` | `fe80::/10` |
| Fragmentation by routers | Possible when permitted | Not permitted |
| Minimum link MTU | Depends on link technology | 1280 bytes |

### 🖆 IPv6 Packet Forwarding Example

Consider the following topology:

```text
                IPv6 Internet
                      |
                      |
                 +---------+
                 | Router A|
                 +---------+
                      |
                2001:db8:1::/64
                      |
                 +---------+
                 | Router B|
                 +---------+
                  /       \
                 /         \
     2001:db8:2::/64     2001:db8:3::/64
             |                 |
        +---------+       +---------+
        | LAN A   |       | LAN B   |
        +---------+       +---------+
```

Router B connects two internal IPv6 networks.

* LAN A uses `2001:db8:2::/64`.
* LAN B uses `2001:db8:3::/64`.
* Router B forwards packets between the two networks.
* Router B also needs an appropriate default route to reach destinations outside its directly connected networks.

---

# 🗁 IPv6 Address Types

IPv6 defines several address types. Understanding them is important because routing behavior depends on the address scope and purpose.

### 🖆 Global Unicast Addresses

Global Unicast Addresses (GUA) are globally routable IPv6 addresses.

The commonly allocated global unicast range is:

```text
2000::/3
```

Example:

```text
2001:db8:100::10
```

Global unicast addresses are used for communication across different networks and potentially across the public internet.

The `2001:db8::/32` prefix is reserved for documentation and should be used in examples rather than real public deployments.

### 🖆 Unique Local Addresses

Unique Local Addresses (ULA) are intended for private IPv6 communication.

Address range:

```text
fc00::/7
```

Locally assigned ULA prefixes commonly use:

```text
fd00::/8
```

Example:

```text
fd12:3456:789a:1::10
```

ULAs are not intended to be globally routed across the public internet.

They are useful for internal networks, infrastructure services, and communication that should remain independent of public IPv6 addressing.

### 🖆 Link-Local Addresses

Link-local addresses are automatically available on IPv6-enabled interfaces.

Address range:

```text
fe80::/10
```

Example:

```text
fe80::1
```

Link-local addresses are used for communication within a single local link.

Common uses include:

* Neighbor Discovery Protocol.
* Router Advertisements.
* Router Solicitation.
* Dynamic routing protocol adjacencies.
* Next-hop addressing between directly connected routers.

Link-local addresses are not forwarded by routers.

The same link-local address can exist on multiple interfaces because its scope is limited to its local link.

### 🖆 Multicast Addresses

IPv6 multicast addresses identify groups of interfaces.

Address range:

```text
ff00::/8
```

Common examples:

| Address | Purpose |
| --- | --- |
| `ff02::1` | All IPv6 nodes on the local link |
| `ff02::2` | All IPv6 routers on the local link |
| `ff02::5` | All OSPFv3 routers on the local link |
| `ff02::6` | All OSPFv3 designated routers on the local link |
| `ff02::9` | All RIPng routers on the local link |

IPv6 uses multicast instead of broadcast.

### 🖆 Loopback Address

The IPv6 loopback address is:

```text
::1/128
```

It refers to the local device itself.

It is commonly used for local testing and software communication.

### 🖆 Unspecified Address

The unspecified IPv6 address is:

```text
::/128
```

It represents the absence of a specified address.

It can be used as a source address during certain address-configuration procedures, such as Duplicate Address Detection.

It must not be assigned as a normal interface address or used as a destination for ordinary communication.

---

# 🗁 IPv6 Prefixes and Subnets

IPv6 uses CIDR prefix notation to identify network portions of addresses.

A prefix length specifies how many leading bits represent the network portion.

Example:

```text
2001:db8:100:1::/64
```

Here:

* `2001:db8:100:1` identifies the network prefix.
* `/64` indicates that the first 64 bits represent the network portion.
* The remaining 64 bits are available for interface identifiers.

### 🖆 Common IPv6 Prefix Lengths

| Prefix | Typical purpose |
| --- | --- |
| `/32` | Common provider allocation |
| `/48` | Common organizational allocation |
| `/56` | Common customer allocation |
| `/64` | Typical LAN subnet |
| `/127` | Point-to-point router links |
| `/128` | Single IPv6 address or host route |

These are common deployment patterns rather than universal requirements.

A `/127` can be appropriate for point-to-point router links under RFC 6164. A `/128` identifies one exact address and is frequently used for loopbacks and host routes.

### 🖆 Example IPv6 Network Allocation

An organization receives:

```text
2001:db8:1000::/48
```

It can divide the allocation into multiple `/64` subnets:

```text
2001:db8:1000:1::/64
2001:db8:1000:2::/64
2001:db8:1000:3::/64
2001:db8:1000:4::/64
```

Each subnet can be assigned to a separate VLAN, department, or network segment.

### 🖆 IPv6 Address Compression

IPv6 addresses can be shortened by removing leading zeros within each hexadecimal group.

Example:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

Compressed:

```text
2001:db8::1
```

The double-colon notation (`::`) can replace one consecutive sequence of zero groups only once in an address.

---

# 🗁 IPv6 Routing Table

An IPv6 routing table stores information about reachable IPv6 destinations.

A route generally contains:

* Destination prefix.
* Next-hop address, if applicable.
* Outgoing interface.
* Route preference or metric.
* Route source or protocol.
* Route status.

### 🖆 Example IPv6 Routing Table

```text
Destination                  Next Hop             Interface

2001:db8:1::/64              Directly connected   eth0
2001:db8:2::/64              Directly connected   eth1
2001:db8:3::/64              fe80::2               eth0
2001:db8:100::/48            fe80::5               eth1
::/0                         fe80::1               eth0
```

Explanation:

| Destination | Meaning |
| --- | --- |
| `2001:db8:1::/64` | Directly connected network |
| `2001:db8:2::/64` | Another directly connected network |
| `2001:db8:3::/64` | Remote network through `fe80::2` |
| `2001:db8:100::/48` | Aggregated remote network |
| `::/0` | Default route |

### 🖆 Route Sources

IPv6 routes can originate from different sources.

| Route source | Description |
| --- | --- |
| Connected | Created from an assigned interface address |
| Static | Manually configured |
| Default | Route used when no more specific route matches |
| OSPFv3 | Learned through OSPFv3 |
| RIPng | Learned through RIPng |
| BGP | Learned through BGP |
| IS-IS | Learned through IS-IS |

The exact route representation and preference rules depend on the operating system and routing platform.

---

# 🗁 Longest Prefix Match

IPv6 routers use Longest Prefix Match (LPM) to select the most specific matching route.

The route with the longest matching prefix is preferred over less-specific matching routes.

### 🖆 Example

Suppose a router has these routes:

```text
2001:db8::/32
2001:db8:100::/48
2001:db8:100:1::/64
```

A packet arrives with destination:

```text
2001:db8:100:1::50
```

All three prefixes match the destination.

However, `/64` is the longest matching prefix.

Therefore, the router selects:

```text
2001:db8:100:1::/64
```

### 🖆 Important Routing Rule

Longest Prefix Match is evaluated before route preference or metric comparisons between routes with different prefix lengths.

Administrative distance, route preference, and metrics are relevant when selecting between competing routes to the same destination prefix, subject to platform-specific rules.

---

# 🗁 Connected IPv6 Routes

A connected IPv6 route is automatically created when an IPv6 address and prefix are configured on an operational network interface.

Example interface configuration:

```text
Interface: eth0
Address:   2001:db8:1::10/64
```

The system installs a route for:

```text
2001:db8:1::/64
```

This route identifies the network directly reachable through `eth0`.

### 🖆 Linux Example

Assign an IPv6 address:

```bash
sudo ip -6 addr add 2001:db8:1::10/64 dev eth0
```

Display IPv6 addresses:

```bash
ip -6 addr show
```

Display IPv6 routes:

```bash
ip -6 route show
```

A connected route may look like:

```text
2001:db8:1::/64 dev eth0 proto kernel metric 256
```

The exact output depends on the system configuration.

### 🖆 Connected Routes and Neighbor Discovery

A connected route does not guarantee that every device in the subnet is reachable.

The router or host still needs to resolve the destination's link-layer address using NDP.

Inspect the IPv6 neighbor cache:

```bash
ip -6 neigh show
```

If NDP resolution fails, packets may not reach their destination even when the connected route exists.

---

# 🗁 Static IPv6 Routing

Static IPv6 routing is the process of manually configuring routes to remote IPv6 networks.

An administrator specifies the destination prefix and the next hop or outgoing interface.

Static routes are useful for:

* Small networks.
* Stub networks.
* Predictable network paths.
* Backup routing.
* Default gateway configuration.
* Specific traffic engineering requirements.

### 🖆 Static Route Example

Consider:

```text
                 Router A
                2001:db8:1::1
                      |
                2001:db8:1::/64
                      |
                 Router B
                2001:db8:1::2
                      |
                2001:db8:2::/64
                      |
                   LAN B
```

Router A needs a route to:

```text
2001:db8:2::/64
```

The next hop is Router B.

### 🖆 Linux Static Route

Configure a static route:

```bash
sudo ip -6 route add 2001:db8:2::/64 via 2001:db8:1::2
```

Display the route:

```bash
ip -6 route show
```

Test the route:

```bash
ip -6 route get 2001:db8:2::10
```

Remove the route:

```bash
sudo ip -6 route del 2001:db8:2::/64
```

These `ip` commands configure runtime state. Persistent configuration should be managed through the system's network manager, such as Netplan or NetworkManager.

### 🖆 Static Route Through a Link-Local Next Hop

IPv6 routers commonly use link-local addresses as next hops.

Example:

```bash
sudo ip -6 route add 2001:db8:2::/64 via fe80::2 dev eth0
```

The interface is required because a link-local address is only meaningful within its local link.

### 🖆 Static Route with a Different Metric

Linux supports route metrics:

```bash
sudo ip -6 route add 2001:db8:2::/64 via fe80::2 dev eth0 metric 100
```

A competing route can be configured with a higher metric:

```bash
sudo ip -6 route add 2001:db8:2::/64 via fe80::3 dev eth1 metric 200
```

The lower metric is generally preferred among otherwise comparable routes in Linux.

Actual route selection can also depend on policy routing, route scope, route type, and other attributes.

---

# 🗁 IPv6 Default Routing

A default route is used when no more specific route matches the destination address.

The IPv6 default route is:

```text
::/0
```

It matches every IPv6 destination address.

### 🖆 Linux Default Route

Configure a default route:

```bash
sudo ip -6 route add default via fe80::1 dev eth0
```

Here:

* `default` represents `::/0`.
* `fe80::1` is the next-hop router.
* `eth0` is the outgoing interface.

Display the default route:

```bash
ip -6 route show default
```

Remove it:

```bash
sudo ip -6 route del default
```

### 🖆 Default Route Example

```text
                IPv6 Internet
                      |
                 +---------+
                 | Router A|
                 +---------+
                      |
                 fe80::1
                      |
                 +---------+
                 | Router B|
                 +---------+
                      |
                    LAN
```

Router B forwards unknown IPv6 destinations toward Router A.

Its default route may be:

```text
::/0 via fe80::1 dev eth0
```

### 🖆 Router Advertisements and Default Routers

IPv6 hosts commonly discover their default router through Router Advertisements.

A router sends an RA containing information such as:

* Router lifetime.
* Prefix information.
* Flags controlling address configuration behavior.
* Other network configuration options.

A host can learn its default router from the RA without manually configuring a static default route.

DHCPv6 can provide addresses and other configuration information, but it does not normally provide the default-router information. Default-router discovery is performed through IPv6 Neighbor Discovery.

---

# 🗁 Link-Local Next-Hop Addresses

Link-local addresses are especially important in IPv6 routing.

Unlike globally unique addresses, link-local addresses are scoped to a particular network interface and link.

A router can use its neighbor's link-local address as the next hop for a route.

### 🖆 Example

```text
Router A                         Router B
---------                         ---------
eth0                              eth0
fe80::1/64                        fe80::2/64
2001:db8:1::1/64                  2001:db8:1::2/64
```

Router A can configure:

```text
2001:db8:2::/64 via fe80::2
```

The route must identify the outgoing interface because `fe80::2` is not globally unique.

### 🖆 Why Link-Local Next Hops Are Common

Link-local next hops offer several benefits:

* They remain valid even if global addressing changes.
* They work independently of global address allocation.
* They are suitable for routing protocol adjacencies.
* They identify a directly connected neighbor.

### 🖆 Interface Scope

On Linux, a link-local next hop must be associated with an interface.

Example:

```bash
sudo ip -6 route add 2001:db8:2::/64 via fe80::2 dev eth0
```

When inspecting routes, always check the outgoing interface associated with a link-local next hop.

---

# 🗁 Neighbor Discovery and Router Advertisements

IPv6 uses Neighbor Discovery Protocol (NDP) instead of ARP.

NDP operates through ICMPv6 and provides important functions for IPv6 communication.

### 🖆 NDP Functions

| Function | Description |
| --- | --- |
| Router Discovery | Hosts discover routers on the local link |
| Prefix Discovery | Hosts learn network prefixes |
| Address Resolution | Resolves IPv6 addresses to link-layer addresses |
| Neighbor Unreachability Detection | Checks whether neighbors remain reachable |
| Duplicate Address Detection | Checks whether an IPv6 address is already in use |
| Redirect | Informs a host about a better first-hop router |

### 🖆 Important NDP Messages

| Message | ICMPv6 Type | Purpose |
| --- | ---: | --- |
| Router Solicitation | 133 | Host requests router information |
| Router Advertisement | 134 | Router advertises network information |
| Neighbor Solicitation | 135 | Neighbor resolution and DAD |
| Neighbor Advertisement | 136 | Responds to neighbor solicitation |
| Redirect | 137 | Indicates a better next hop |

### 🖆 Router Advertisement

A Router Advertisement can provide:

* Default-router information.
* IPv6 network prefixes.
* Prefix lifetimes.
* MTU information.
* Managed address configuration flag.
* Other configuration information flag.

The M and O flags help indicate whether DHCPv6 address configuration or other DHCPv6 information is available.

### 🖆 SLAAC and DHCPv6

IPv6 hosts can obtain addressing information through different mechanisms.

**SLAAC (Stateless Address Autoconfiguration)** allows hosts to generate IPv6 addresses using advertised prefixes and interface configuration mechanisms.

**DHCPv6** provides stateful or stateless configuration services depending on the deployment.

These mechanisms can coexist.

Default-router discovery remains a Neighbor Discovery function, not a standard DHCPv6 default-gateway assignment.

### 🖆 Linux NDP Inspection

Display IPv6 neighbors:

```bash
ip -6 neigh show
```

Display IPv6 addresses:

```bash
ip -6 addr show
```

Capture Neighbor Discovery traffic:

```bash
sudo tcpdump -ni eth0 icmp6
```

---

# 🗁 IPv6 Forwarding

A Linux system can operate as an IPv6 host or an IPv6 router.

By default, IPv6 forwarding may be disabled.

When forwarding is disabled, the system generally does not forward IPv6 packets between interfaces.

### 🖆 Enable IPv6 Forwarding Temporarily

Enable forwarding:

```bash
sudo sysctl -w net.ipv6.conf.all.forwarding=1
```

Verify:

```bash
sysctl net.ipv6.conf.all.forwarding
```

Expected result:

```text
net.ipv6.conf.all.forwarding = 1
```

### 🖆 Enable IPv6 Forwarding Permanently

Create a configuration file:

```bash
sudo nano /etc/sysctl.d/99-ipv6-forwarding.conf
```

Add:

```ini
net.ipv6.conf.all.forwarding = 1
```

Apply the configuration:

```bash
sudo sysctl --system
```

### 🖆 Router Advertisement Considerations

Enabling IPv6 forwarding changes how Linux handles Router Advertisements.

A Linux router generally does not behave like an ordinary host when accepting RAs on its interfaces.

If an upstream router provides the default route through RA, configure the relevant interface behavior appropriately.

For example, Linux can be configured to accept RAs while forwarding is enabled:

```bash
sudo sysctl -w net.ipv6.conf.eth0.accept_ra=2
```

The value `2` permits RA acceptance even when forwarding is enabled.

Use this setting only on interfaces where learning routes from upstream Router Advertisements is intended.

### 🖆 Verify IPv6 Forwarding

Inspect forwarding configuration:

```bash
sysctl net.ipv6.conf.all.forwarding
```

Inspect interface addresses:

```bash
ip -6 addr show
```

Inspect routes:

```bash
ip -6 route show
```

Inspect firewall rules:

```bash
sudo nft list ruleset
```

Forwarding requires both suitable routes and firewall policies that permit the intended traffic.

---

# 🗁 Dynamic IPv6 Routing Protocols

Dynamic routing protocols automatically exchange routing information between routers.

They reduce the need for manually configuring every remote network.

Common IPv6-capable routing protocols include:

| Protocol | Purpose |
| --- | --- |
| OSPFv3 | Link-state interior gateway routing |
| RIPng | Distance-vector IPv6 routing |
| MP-BGP | Exchange of IPv6 routes between autonomous systems and routing domains |
| IS-IS for IPv6 | Link-state routing with IPv6 support |

### 🖆 OSPFv3

OSPFv3 is the IPv6-capable evolution of OSPF.

It provides link-state routing and uses a link-state database to calculate shortest-path routes.

Important characteristics:

* Uses Dijkstra's Shortest Path First algorithm.
* Uses cost as its routing metric.
* Supports areas and hierarchical routing.
* Uses IPv6 link-local addresses for neighbor communication.
* Uses IPv6 as its transport network.
* Uses IP protocol number 89, not a TCP or UDP port.

OSPFv3 is specified in RFC 5340.

Modern OSPFv3 also supports address families through extensions defined in RFC 5838. Therefore, OSPFv3 is not simply OSPFv2 with larger IP addresses.

### 🖆 RIPng

RIPng is the IPv6 version of the Routing Information Protocol.

Important characteristics:

* Uses hop count as its metric.
* Maximum usable distance is 15 hops.
* A metric of 16 represents an unreachable destination.
* Uses UDP port 521.
* Uses multicast address `ff02::9`.
* Supports IPv6 prefixes.

RIPng is suitable for relatively small and simple networks, but its limited hop count and slower convergence make it less suitable for large modern networks.

RIPng is specified in RFC 2080.

### 🖆 MP-BGP for IPv6

Multiprotocol BGP extends BGP to support multiple network-layer protocols and address families.

IPv6 routes are commonly exchanged using:

```text
AFI: 2
SAFI: 1
```

Where:

* AFI 2 represents IPv6.
* SAFI 1 represents unicast routing information.

MP-BGP allows IPv4 and IPv6 routing information to be exchanged through BGP sessions.

Important characteristics:

* Used for inter-AS routing.
* Supports IPv6 unicast route exchange.
* Uses path attributes for route selection.
* Supports IPv6 next-hop information.
* Can support IPv4 NLRI with IPv6 next hops through extensions.

Relevant standards include RFC 4760 and RFC 2545. RFC 8950 specifies extensions for using IPv6 next hops with IPv4 NLRI.

### 🖆 IS-IS for IPv6

IS-IS is a link-state routing protocol that can support IPv6 through protocol extensions.

Important characteristics:

* Uses a link-state database.
* Calculates shortest paths using the SPF algorithm.
* Supports hierarchical routing.
* Can carry both IPv4 and IPv6 routing information.
* Commonly used in large enterprise and service-provider networks.

IPv6 extensions for IS-IS are defined in RFC 5308.

### 🖆 Choosing an IPv6 Routing Protocol

| Environment | Common choice |
| --- | --- |
| Small network | Static routing |
| Enterprise internal network | OSPFv3 or IS-IS |
| Service-provider backbone | IS-IS or OSPFv3 |
| Internet routing | MP-BGP |
| Simple legacy environment | RIPng |

The appropriate protocol depends on network size, convergence requirements, operational complexity, and interoperability.

---

# 🗁 IPv6 Routing on Linux

Linux provides IPv6 routing functionality through the `iproute2` utility.

The `ip -6` command manages IPv6 addresses, routes, neighbors, and related network configuration.

### 🖆 Display IPv6 Addresses

```bash
ip -6 addr show
```

Display addresses on a specific interface:

```bash
ip -6 addr show dev eth0
```

### 🖆 Display IPv6 Routing Table

```bash
ip -6 route show
```

Display the default route:

```bash
ip -6 route show default
```

Display routes from a specific table:

```bash
ip -6 route show table main
```

### 🖆 Add an IPv6 Address

```bash
sudo ip -6 addr add 2001:db8:1::10/64 dev eth0
```

Remove the address:

```bash
sudo ip -6 addr del 2001:db8:1::10/64 dev eth0
```

### 🖆 Add a Static IPv6 Route

```bash
sudo ip -6 route add 2001:db8:2::/64 via fe80::2 dev eth0
```

### 🖆 Add an IPv6 Default Route

```bash
sudo ip -6 route add default via fe80::1 dev eth0
```

### 🖆 Inspect Route Selection

```bash
ip -6 route get 2001:db8:2::10
```

This command displays how Linux would route a packet toward the specified destination.

### 🖆 Inspect Neighbor Information

```bash
ip -6 neigh show
```

### 🖆 Inspect Policy Routing Rules

```bash
ip -6 rule show
```

Linux supports policy-based routing for IPv6. Rules can select routing tables based on attributes such as source prefix, destination prefix, firewall mark, or other supported selectors.

### 🖆 Test IPv6 Connectivity

Ping an IPv6 destination:

```bash
ping -6 2606:4700:4700::1111
```

Trace the IPv6 route:

```bash
traceroute -6 2606:4700:4700::1111
```

Inspect the path MTU:

```bash
tracepath -6 2606:4700:4700::1111
```

Capture ICMPv6 packets:

```bash
sudo tcpdump -ni any icmp6
```

---

# 🗁 IPv6 Routing on MikroTik RouterOS

MikroTik RouterOS v7 supports IPv6 addressing, static routing, dynamic routing, and IPv6 firewall configuration.

IPv6 routing requires the IPv6 package and appropriate configuration.

### 🖆 Enable IPv6 Forwarding

RouterOS v7 normally supports IPv6 routing when IPv6 functionality is available and forwarding is enabled.

Inspect IPv6 settings:

```routeros
/ipv6 settings print
```

Enable forwarding if required:

```routeros
/ipv6 settings set forward=yes
```

### 🖆 Assign IPv6 Addresses

Assign an IPv6 address to an interface:

```routeros
/ipv6 address add address=2001:db8:1::1/64 interface=ether1 advertise=no
```

Assign an address to another interface:

```routeros
/ipv6 address add address=2001:db8:2::1/64 interface=ether2 advertise=no
```

These addresses create connected IPv6 routes when the interfaces and configuration are operational.

### 🖆 Display IPv6 Addresses

```routeros
/ipv6 address print
```

### 🖆 Display IPv6 Routes

```routeros
/ipv6 route print
```

Connected IPv6 routes are generally identified by the connected route flag.

### 🖆 Configure a Static IPv6 Route

Configure a route to a remote IPv6 network:

```routeros
/ipv6 route add dst-address=2001:db8:3::/64 gateway=2001:db8:1::2
```

### 🖆 Configure an IPv6 Default Route

Configure a default route:

```routeros
/ipv6 route add dst-address=::/0 gateway=2001:db8:1::2
```

For a link-local gateway, identify the appropriate interface as part of the gateway configuration where required by the RouterOS version and syntax.

### 🖆 Configure IPv6 Router Advertisements

RouterOS can advertise IPv6 prefixes to hosts through Neighbor Discovery.

Inspect ND configuration:

```routeros
/ipv6 nd print
```

Inspect advertised prefixes:

```routeros
/ipv6 nd prefix print
```

Example prefix configuration:

```routeros
/ipv6 nd prefix add prefix=2001:db8:2::/64 interface=ether2
```

Ensure that the intended interface has appropriate ND settings and that the advertised prefix is correct.

### 🖆 Verify IPv6 Connectivity

Inspect routes:

```routeros
/ipv6 route print detail
```

Inspect neighbors:

```routeros
/ipv6 neighbor print
```

Test an IPv6 destination:

```routeros
/ping 2001:db8:1::2
```

RouterOS v6 and v7 differ in routing implementation and available features. Use RouterOS v7 documentation when configuring modern deployments.

---

# 🗁 IPv6 Routing on Cisco IOS

Cisco IOS supports IPv6 routing through IPv6 interface addressing, static routes, and dynamic routing protocols.

### 🖆 Enable IPv6 Unicast Routing

Enable IPv6 packet forwarding:

```cisco
configure terminal
ipv6 unicast-routing
```

Without IPv6 unicast routing enabled, a Cisco router may not forward IPv6 traffic between interfaces.

### 🖆 Configure IPv6 Addresses

Configure an interface:

```cisco
configure terminal

interface GigabitEthernet0/0
 ipv6 address 2001:DB8:1::1/64
 no shutdown
```

Configure another interface:

```cisco
interface GigabitEthernet0/1
 ipv6 address 2001:DB8:2::1/64
 no shutdown
```

### 🖆 Configure a Static IPv6 Route

```cisco
ipv6 route 2001:DB8:3::/64 2001:DB8:1::2
```

### 🖆 Configure an IPv6 Default Route

```cisco
ipv6 route ::/0 2001:DB8:1::2
```

### 🖆 Display IPv6 Routing Information

Display the IPv6 routing table:

```cisco
show ipv6 route
```

Display IPv6 interface information:

```cisco
show ipv6 interface brief
```

Display IPv6 neighbors:

```cisco
show ipv6 neighbors
```

### 🖆 Configure OSPFv3

A simplified Cisco IOS OSPFv3 example:

```cisco
configure terminal

ipv6 router ospf 10
 router-id 1.1.1.1

interface GigabitEthernet0/0
 ipv6 ospf 10 area 0
```

OSPFv3 configuration syntax varies between Cisco IOS and IOS XE releases. Verify the syntax for the target platform.

---

# 🗁 IPv6 Route Aggregation

Route aggregation combines multiple contiguous networks into a shorter summary prefix.

It reduces the number of routing table entries and simplifies routing information exchange.

### 🖆 Example

Suppose a router has these networks:

```text
2001:db8:1000:0000::/64
2001:db8:1000:0001::/64
2001:db8:1000:0002::/64
2001:db8:1000:0003::/64
```

These four aligned `/64` networks can be summarized as:

```text
2001:db8:1000::/62
```

The `/62` prefix covers the four `/64` subnets.

### 🖆 Benefits of Route Aggregation

* Reduces routing table size.
* Simplifies routing policies.
* Reduces routing update volume.
* Improves scalability.
* Makes hierarchical addressing easier to manage.

### 🖆 Important Aggregation Rules

For exact aggregation:

* Networks must be contiguous.
* Network boundaries must be correctly aligned.
* The summarized prefix must contain all included networks.
* The summary must not unintentionally include unrelated address space.

An incorrect summary can cause traffic to be routed toward a router that does not actually have a path to the destination.

---

# 🗁 IPv6 Routing Security

IPv6 introduces security considerations that must be addressed in network design and operation.

### 🖆 Rogue Router Advertisements

An attacker may send unauthorized Router Advertisements to influence host routing configuration.

Possible consequences include:

* Traffic interception.
* Traffic redirection.
* Incorrect default gateway selection.
* Denial of service.

Mitigations include:

* Router Advertisement Guard (RA Guard).
* Layer 2 access controls.
* Trusted router-port configuration.
* Monitoring unexpected RA messages.

### 🖆 Neighbor Discovery Attacks

NDP can be targeted through forged Neighbor Advertisements or other malicious messages.

Possible attacks include:

* Neighbor spoofing.
* Duplicate Address Detection interference.
* Neighbor cache exhaustion.
* Unauthorized redirects.

Use supported Layer 2 security features, appropriate filtering, and monitoring.

### 🖆 ICMPv6 Filtering

Do not indiscriminately block ICMPv6.

IPv6 depends on ICMPv6 for essential operations, including:

* Neighbor Discovery.
* Router Discovery.
* Packet Too Big messages.
* Destination Unreachable messages.
* Time Exceeded messages.

Blocking necessary ICMPv6 traffic can break address resolution, router discovery, and Path MTU Discovery.

### 🖆 Source Address Validation

Apply source-address validation to reduce spoofed traffic.

Use ingress filtering and appropriate network-edge controls.

BCP 38 provides guidance on network ingress filtering.

### 🖆 Routing Protocol Security

Protect routing infrastructure through:

* Authentication where supported.
* Routing protocol interface restrictions.
* Control-plane filtering.
* Prefix filtering.
* Route-policy validation.
* Secure management access.
* Monitoring routing changes.

---

# 🗁 IPv6 Routing Troubleshooting

IPv6 connectivity problems can originate from incorrect addressing, missing routes, failed neighbor resolution, disabled forwarding, firewall policies, or MTU problems.

Troubleshoot systematically.

### 🖆 Verify IPv6 Address Configuration

```bash
ip -6 addr show
```

Check:

* Correct IPv6 address.
* Correct prefix length.
* Interface operational state.
* Link-local address availability.
* Duplicate address detection status.

### 🖆 Verify the Routing Table

```bash
ip -6 route show
```

Check:

* Connected routes.
* Static routes.
* Default route.
* Next-hop addresses.
* Outgoing interfaces.
* Route metrics.

### 🖆 Verify Route Selection

```bash
ip -6 route get 2001:db8:2::10
```

This helps identify the selected route and outgoing interface.

### 🖆 Verify Neighbor Resolution

```bash
ip -6 neigh show
```

Look for neighbor states such as:

| State | Meaning |
| --- | --- |
| REACHABLE | Neighbor is considered reachable |
| STALE | Previously reachable; confirmation may be needed |
| DELAY | Waiting before probing |
| PROBE | Active reachability probing |
| INCOMPLETE | Address resolution is in progress |
| FAILED | Neighbor resolution failed |

Linux may also display states such as `PERMANENT` and `NOARP`.

### 🖆 Verify IPv6 Forwarding

```bash
sysctl net.ipv6.conf.all.forwarding
```

Expected router configuration:

```text
net.ipv6.conf.all.forwarding = 1
```

If forwarding is disabled, the Linux system may not route packets between interfaces.

### 🖆 Test IPv6 Connectivity

Test the local interface:

```bash
ping -6 ::1
```

Test a local gateway:

```bash
ping -6 fe80::1%eth0
```

Test a remote IPv6 destination:

```bash
ping -6 2606:4700:4700::1111
```

The interface zone identifier is required in tools that need to disambiguate link-local destinations.

### 🖆 Trace the IPv6 Path

```bash
traceroute -6 2606:4700:4700::1111
```

This helps identify where packets stop receiving responses.

An unanswered hop does not necessarily indicate that the router is offline.

### 🖆 Capture ICMPv6 Traffic

```bash
sudo tcpdump -ni any icmp6
```

Inspect Neighbor Discovery:

```bash
sudo tcpdump -ni eth0 'icmp6'
```

Inspect IPv6 traffic:

```bash
sudo tcpdump -ni any 'ip6'
```

### 🖆 Common IPv6 Routing Problems

| Problem | Possible cause | Investigation |
| --- | --- | --- |
| Cannot reach local gateway | NDP failure or incorrect VLAN | Inspect neighbors and interface configuration |
| Cannot reach remote network | Missing route | Inspect IPv6 routing table |
| Internet unavailable | Missing default route | Check `::/0` |
| Router does not forward packets | Forwarding disabled | Inspect forwarding sysctl or RouterOS settings |
| Hosts select incorrect gateway | Rogue or incorrect RA | Inspect Router Advertisements |
| Large transfers fail | MTU or PMTUD problem | Inspect ICMPv6 Packet Too Big |
| Neighbor remains INCOMPLETE | NDP resolution failure | Capture ICMPv6 and check Layer 2 |
| Only some destinations fail | Incorrect prefix or policy routing | Inspect route selection and `ip -6 rule` |
| Routing protocol does not form adjacency | Interface, multicast, scope, or protocol configuration | Inspect protocol neighbors and interface settings |

---

# 🗁 Best Practices

Follow these practices when designing and operating IPv6 networks.

* Use a structured IPv6 addressing plan.
* Allocate `/64` prefixes to ordinary LAN segments unless the design requires an exception.
* Use `/127` where appropriate for point-to-point router links.
* Use `/128` for loopback and host routes when suitable.
* Keep global unicast, ULA, and link-local addressing purposes clear.
* Use link-local next hops carefully and identify the correct interface.
* Prefer dynamic routing for larger networks with multiple routers and changing topology.
* Use static routing for small, predictable, or stub networks.
* Advertise only intended prefixes through Router Advertisements.
* Enable IPv6 forwarding on devices that must act as routers.
* Maintain correct ICMPv6 firewall policies.
* Protect against unauthorized Router Advertisements.
* Monitor routing changes and neighbor state.
* Use route aggregation only when address boundaries are correct.
* Validate IPv6 MTU and Path MTU Discovery across routed and tunneled networks.
* Test IPv6 connectivity independently instead of assuming IPv4 success means IPv6 is working.
* Maintain IPv4 and IPv6 security policies consistently in dual-stack environments.

---

# 🗁 Practical Labs

These exercises help develop practical IPv6 routing skills using Ubuntu/Linux, MikroTik RouterOS, and Cisco IOS.

### 🖆 Lab: Configure IPv6 Addresses on Linux

**Objective:** Assign IPv6 addresses to two Linux interfaces.

Example:

```bash
sudo ip -6 addr add 2001:db8:1::1/64 dev eth0
sudo ip -6 addr add 2001:db8:2::1/64 dev eth1
```

Verify:

```bash
ip -6 addr show
ip -6 route show
```

Expected result:

* Both interfaces have the configured IPv6 addresses.
* Connected IPv6 routes are installed.

### 🖆 Lab: Configure a Static IPv6 Route

**Objective:** Connect two IPv6 networks through a router.

Configure a static route:

```bash
sudo ip -6 route add 2001:db8:3::/64 via fe80::2 dev eth0
```

Verify:

```bash
ip -6 route show
ip -6 route get 2001:db8:3::10
```

Test connectivity to a host in the remote network.

### 🖆 Lab: Configure an IPv6 Default Route

**Objective:** Configure a default route toward an upstream router.

```bash
sudo ip -6 route add default via fe80::1 dev eth0
```

Verify:

```bash
ip -6 route show default
```

Test an external IPv6 destination.

### 🖆 Lab: Enable IPv6 Forwarding

**Objective:** Convert an Ubuntu system into an IPv6 router.

Enable forwarding:

```bash
sudo sysctl -w net.ipv6.conf.all.forwarding=1
```

Verify:

```bash
sysctl net.ipv6.conf.all.forwarding
```

Configure two interfaces with different IPv6 subnets and test forwarding between them.

### 🖆 Lab: Investigate Neighbor Discovery

**Objective:** Observe IPv6 Neighbor Discovery messages.

Run:

```bash
sudo tcpdump -ni eth0 icmp6
```

Generate traffic toward a local IPv6 neighbor:

```bash
ping -6 2001:db8:1::20
```

Inspect the neighbor table:

```bash
ip -6 neigh show
```

Identify Neighbor Solicitation and Neighbor Advertisement messages.

### 🖆 Lab: Configure IPv6 Routing on MikroTik

**Objective:** Configure two IPv6 networks and a static route.

Assign addresses:

```routeros
/ipv6 address add address=2001:db8:1::1/64 interface=ether1
/ipv6 address add address=2001:db8:2::1/64 interface=ether2
```

Inspect routes:

```routeros
/ipv6 route print
```

Configure a route to a remote network:

```routeros
/ipv6 route add dst-address=2001:db8:3::/64 gateway=2001:db8:1::2
```

Verify the route and test connectivity.

### 🖆 Lab: Compare IPv4 and IPv6 Routing

**Objective:** Understand differences between IPv4 and IPv6 routing.

Run:

```bash
ip -4 route show
ip -6 route show
```

Compare:

* Default route formats.
* Connected network prefixes.
* Next-hop addressing.
* Interface selection.
* Neighbor resolution.

Document the differences you observe.

---

# 🗁 Interview Questions

### 🖆 Fundamentals

**What is IPv6 routing?**

IPv6 routing is the process of forwarding IPv6 packets between different IPv6 networks using routing tables and route-selection mechanisms.

**What is the IPv6 default route?**

The IPv6 default route is `::/0`. It is used when no more specific route matches the destination.

**What is the difference between IPv4 and IPv6 routing?**

IPv6 uses 128-bit addresses, NDP instead of ARP, Router Advertisements for default-router discovery, and does not allow routers to fragment forwarded packets.

**What is a link-local IPv6 address?**

A link-local address is an IPv6 address in `fe80::/10` that is valid only on its local link.

**Why do IPv6 routers commonly use link-local next hops?**

Link-local addresses are stable within a link and do not depend on global address allocation. They are also commonly used by IPv6 routing protocols.

### 🖆 Routing Tables and Protocols

**What is Longest Prefix Match?**

It is the route-selection process that chooses the most specific matching destination prefix.

**What is the difference between a connected route and a static route?**

A connected route is automatically created from an interface address and prefix, while a static route is manually configured by an administrator.

**What is OSPFv3?**

OSPFv3 is an OSPF version that supports IPv6 routing and link-state path calculation.

**What is RIPng?**

RIPng is the IPv6 version of RIP. It uses hop count as its metric and treats 16 hops as unreachable.

**What is MP-BGP?**

MP-BGP extends BGP to support multiple address families, including IPv6 unicast routing.

### 🖆 Neighbor Discovery and Security

**What is the difference between ARP and NDP?**

ARP resolves IPv4 addresses to MAC addresses. NDP performs IPv6 neighbor resolution and additional functions through ICMPv6.

**What is Router Advertisement?**

A Router Advertisement is an ICMPv6 message through which routers advertise information such as prefixes and default-router availability.

**Does DHCPv6 provide the default gateway?**

Normally, no. IPv6 default-router discovery is performed through Router Advertisements.

**Why should ICMPv6 not be blocked indiscriminately?**

IPv6 depends on ICMPv6 for Neighbor Discovery, Router Discovery, Packet Too Big messages, and other essential network functions.

**Can IPv6 routers fragment packets?**

No. IPv6 routers do not fragment forwarded packets. The source node handles fragmentation when necessary.

### 🖆 Practical Questions

**How do you display the IPv6 routing table on Linux?**

```bash
ip -6 route show
```

**How do you configure an IPv6 default route on Linux?**

```bash
sudo ip -6 route add default via fe80::1 dev eth0
```

**How do you enable IPv6 forwarding on Ubuntu?**

```bash
sudo sysctl -w net.ipv6.conf.all.forwarding=1
```

**How do you inspect IPv6 neighbors on Linux?**

```bash
ip -6 neigh show
```

**How do you test IPv6 connectivity?**

```bash
ping -6 2606:4700:4700::1111
```

---

# 🗁 Summary

IPv6 routing enables communication between IPv6 networks through routers and routing tables.

Important concepts include:

* IPv6 address types and prefix allocation.
* Connected, static, and default routes.
* Longest Prefix Match.
* Link-local next-hop addressing.
* Neighbor Discovery Protocol.
* Router Advertisements and SLAAC.
* IPv6 forwarding.
* OSPFv3, RIPng, MP-BGP, and IS-IS.
* IPv6 routing configuration on Linux, MikroTik, and Cisco.
* Route aggregation and routing security.
* IPv6 troubleshooting and Path MTU Discovery.

A solid understanding of IPv6 routing is essential for operating modern enterprise networks, ISP infrastructure, cloud environments, and dual-stack networks.

---

# 🗁 References

### 🖆 Official Standards and RFCs

* [RFC 8200 — Internet Protocol, Version 6 (IPv6) Specification](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 4291 — IP Version 6 Addressing Architecture](https://www.rfc-editor.org/rfc/rfc4291)
* [RFC 4861 — Neighbor Discovery for IP Version 6](https://www.rfc-editor.org/rfc/rfc4861)
* [RFC 4862 — IPv6 Stateless Address Autoconfiguration](https://www.rfc-editor.org/rfc/rfc4862)
* [RFC 4193 — Unique Local IPv6 Unicast Addresses](https://www.rfc-editor.org/rfc/rfc4193)
* [RFC 6164 — Using 127-Bit IPv6 Prefixes on Inter-Router Links](https://www.rfc-editor.org/rfc/rfc6164)
* [RFC 8201 — Path MTU Discovery for IP Version 6](https://www.rfc-editor.org/rfc/rfc8201)
* [RFC 5340 — OSPF for IPv6](https://www.rfc-editor.org/rfc/rfc5340)
* [RFC 5838 — Support of Address Families in OSPFv3](https://www.rfc-editor.org/rfc/rfc5838)
* [RFC 2080 — RIPng for IPv6](https://www.rfc-editor.org/rfc/rfc2080)
* [RFC 4760 — Multiprotocol Extensions for BGP-4](https://www.rfc-editor.org/rfc/rfc4760)
* [RFC 2545 — Use of BGP-4 Multiprotocol Extensions for IPv6 Inter-Domain Routing](https://www.rfc-editor.org/rfc/rfc2545)
* [RFC 8950 — Advertising IPv4 Network Layer Reachability Information with an IPv6 Next Hop](https://www.rfc-editor.org/rfc/rfc8950)
* [RFC 5308 — Routing IPv6 with IS-IS](https://www.rfc-editor.org/rfc/rfc5308)

### 🖆 Linux Documentation

* [Linux IP Command Documentation](https://man7.org/linux/man-pages/man8/ip.8.html)
* [Linux IPv6 Documentation](https://docs.kernel.org/networking/ipv6.html)
* [Netplan Documentation](https://netplan.readthedocs.io/)

### 🖆 Vendor Documentation

* [MikroTik RouterOS IPv6 Documentation](https://help.mikrotik.com/docs/)
* [Cisco IPv6 Configuration Documentation](https://www.cisco.com/c/en/us/solutions/ipv6/overview.html)
* [FRRouting Documentation](https://docs.frrouting.org/)
