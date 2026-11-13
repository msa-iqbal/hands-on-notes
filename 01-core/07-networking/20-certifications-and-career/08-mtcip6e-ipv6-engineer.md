# MTCIP6E — MikroTik Certified IPv6 Engineer

MTCIP6E stands for **MikroTik Certified IPv6 Engineer**. This learning roadmap focuses on IPv6 networking concepts and their implementation in MikroTik RouterOS.

IPv6 was developed to address the limitations of IPv4, particularly its limited address space. It provides a significantly larger address space, simplified packet headers, stateless address autoconfiguration, and other capabilities for modern networks.

IPv6 is increasingly important for:

* Internet Service Providers.
* Enterprise networks.
* Data centers.
* Cloud infrastructure.
* Internet of Things deployments.
* Mobile networks.
* Large-scale network architectures.

A network engineer should understand IPv6 addressing, subnetting, routing, neighbor discovery, autoconfiguration, firewall security, and transition technologies.

# 🗁 Learning Objectives

After completing this roadmap, you should be able to:

* Understand IPv6 architecture and addressing.
* Read and write IPv6 addresses.
* Calculate IPv6 network prefixes.
* Configure IPv6 addresses in RouterOS.
* Understand Neighbor Discovery Protocol.
* Configure SLAAC and DHCPv6.
* Implement IPv6 routing.
* Configure IPv6 firewall policies.
* Understand IPv4-to-IPv6 transition technologies.
* Configure IPv6 DNS.
* Monitor and troubleshoot IPv6 connectivity.
* Design dual-stack and IPv6-only network deployments.

# 🗁 IPv6 Fundamentals

### 🖆 What Is IPv6?

IPv6 stands for Internet Protocol version 6.

It is the successor to IPv4 and provides a 128-bit addressing system.

IPv4 uses 32-bit addresses, whereas IPv6 uses 128-bit addresses.

| Feature | IPv4 | IPv6 |
| --- | --- | --- |
| Address size | 32 bits | 128 bits |
| Address format | Decimal | Hexadecimal |
| Address example | 192.168.1.1 | 2001:db8:1::1 |
| Address space | Approximately 4.3 billion addresses | \(2^{128}\) addresses |
| Broadcast | Supported | Not supported |
| Multicast | Supported | Supported |
| Address configuration | Manual and DHCP | Manual, SLAAC, and DHCPv6 |
| Header | Variable length | Fixed base header |
| Fragmentation | Routers and hosts | Source host only |

### 🖆 Why IPv6 Is Necessary

IPv4 has a limited public address space.

Although NAT and private addressing extend the usability of IPv4, they introduce operational complexity and do not eliminate the underlying address shortage.

IPv6 provides:

* A vastly larger address space.
* Globally unique addressing capabilities.
* More structured address allocation.
* Stateless Address Autoconfiguration.
* Multicast-based network communication.
* Improved support for modern network architectures.

### 🖆 IPv6 Characteristics

Important IPv6 characteristics include:

* 128-bit addresses.
* Hexadecimal notation.
* Prefix-based addressing.
* No broadcast addressing.
* Multicast and anycast communication.
* Neighbor Discovery Protocol.
* SLAAC.
* DHCPv6.
* Extension headers.
* Support for IPsec at the protocol architecture level.

IPv6 does not automatically provide network security. Firewall policies and secure configurations are still necessary.

# 🗁 IPv6 Address Architecture

### 🖆 IPv6 Address Structure

An IPv6 address contains 128 bits.

It is normally represented as eight groups of four hexadecimal digits separated by colons.

Example:

```text
2001:0db8:0001:0000:0000:0000:0000:0001
```

Each hexadecimal digit represents four bits.

Therefore:

* One hexadecimal group contains 16 bits.
* Eight groups contain 128 bits.

### 🖆 Full IPv6 Address

Example:

```text
2001:0db8:0001:0000:0000:0000:0000:0001
```

### 🖆 Leading Zero Compression

Leading zeros in each group can be removed.

```text
2001:db8:1:0:0:0:0:1
```

### 🖆 Consecutive Zero Compression

One continuous sequence of zero groups can be replaced with `::`.

```text
2001:db8:1::1
```

The `::` notation can be used only once in an IPv6 address because using it more than once would make the number of omitted groups ambiguous.

### 🖆 IPv6 Prefix Notation

IPv6 uses CIDR-style prefix notation.

Example:

```text
2001:db8:1234::/48
```

The `/48` indicates that the first 48 bits represent the network prefix.

The remaining 80 bits are available for subnet and interface addressing within that allocation.

### 🖆 Network Prefix and Interface Identifier

An IPv6 address can be understood as containing:

* Network prefix.
* Interface identifier.

The exact division depends on the prefix length and addressing architecture.

A `/64` network is commonly used for ordinary IPv6 LAN segments.

# 🗁 IPv6 Address Types

### 🖆 Unicast Address

A unicast address identifies a single interface.

Packets sent to a unicast address are intended for that interface.

Common unicast categories include:

* Global Unicast.
* Link-Local.
* Unique Local.
* Loopback.
* Unspecified.

### 🖆 Global Unicast Address

Global Unicast Addresses are globally routable IPv6 addresses.

The commonly used global unicast allocation is within:

```text
2000::/3
```

Example:

```text
2001:db8:1234::1
```

The documentation prefix `2001:db8::/32` is reserved for examples and documentation. It should not be used as a real public Internet allocation.

### 🖆 Link-Local Address

Link-local addresses are used for communication within a single IPv6 link.

Their prefix is:

```text
fe80::/10
```

Examples:

```text
fe80::1
fe80::abcd:1234:5678:9abc
```

Link-local addresses are essential for Neighbor Discovery and many routing protocols.

They are not forwarded by routers.

### 🖆 Unique Local Address

Unique Local Addresses are intended for private internal communication.

Their allocation prefix is:

```text
fc00::/7
```

In typical deployments, locally assigned ULAs use the `fd00::/8` portion.

Example:

```text
fd12:3456:789a::1
```

They are not intended to be globally routed on the public Internet.

### 🖆 Loopback Address

The IPv6 loopback address is:

```text
::1
```

It is equivalent in purpose to IPv4 `127.0.0.1`.

### 🖆 Unspecified Address

The unspecified address is:

```text
::
```

It represents the absence of an assigned IPv6 address.

It is commonly used as a source address during certain initialization procedures.

### 🖆 Multicast Address

IPv6 multicast addresses begin with:

```text
ff00::/8
```

Multicast allows communication with a group of interfaces.

Examples:

| Address | Purpose |
| --- | --- |
| `ff02::1` | All IPv6 nodes on the local link |
| `ff02::2` | All IPv6 routers on the local link |
| `ff02::1:ff00:0/104` | Solicited-node multicast address range |

### 🖆 Anycast Address

An anycast address is assigned to multiple interfaces.

A packet sent to an anycast address is delivered to one of the associated interfaces, generally selected according to routing topology.

Anycast is useful for distributed services such as DNS and content delivery.

# 🗁 IPv6 Address Configuration

### 🖆 Enable IPv6 on RouterOS

RouterOS provides IPv6 functionality through its IPv6 package.

Check installed packages:

```routeros
/system package print
```

Check IPv6 availability:

```routeros
/ipv6 address print
```

### 🖆 Configure a Static IPv6 Address

Example:

```routeros
/ipv6 address
add address=2001:db8:10::1/64 interface=ether2 advertise=no
```

This assigns an IPv6 address to `ether2`.

The documentation prefix is used only as an example.

### 🖆 Configure a Link-Local Address

RouterOS normally creates link-local addresses automatically on applicable interfaces.

Inspect them:

```routeros
/ipv6 address print detail
```

### 🖆 Assign IPv6 Addresses to Multiple Interfaces

Example:

```routeros
/ipv6 address
add address=2001:db8:10::1/64 interface=ether2 advertise=no
add address=2001:db8:20::1/64 interface=ether3 advertise=no
```

Each interface represents a separate IPv6 network.

### 🖆 Verify IPv6 Addresses

```routeros
/ipv6 address print detail
/ipv6 neighbor print
```

Check:

* Address assignment.
* Interface association.
* Prefix length.
* Address scope.
* Address state.

# 🗁 IPv6 Subnetting

### 🖆 IPv6 Subnetting Fundamentals

IPv6 subnetting divides a larger address allocation into smaller network prefixes.

For example:

```text
2001:db8:1000::/48
```

A `/48` allocation contains 16 bits available for creating `/64` subnets.

Therefore, it provides:

\[
2^{64-48}=65,536
\]

possible `/64` subnets.

### 🖆 Common IPv6 Prefix Lengths

| Prefix | Typical Purpose |
| --- | --- |
| `/32` | ISP allocation |
| `/40` | ISP or large organization allocation |
| `/48` | Organization allocation |
| `/56` | Small organization or customer allocation |
| `/60` | Smaller customer allocation |
| `/64` | Standard LAN subnet |
| `/127` | Point-to-point link |
| `/128` | Single interface address or host route |

These are common deployment patterns, not universal mandatory allocations.

### 🖆 Subnetting Example

Given:

```text
2001:db8:1000::/48
```

Create four `/64` networks:

```text
2001:db8:1000:1::/64
2001:db8:1000:2::/64
2001:db8:1000:3::/64
2001:db8:1000:4::/64
```

Each network contains \(2^{64}\) possible interface identifier values.

### 🖆 IPv6 Subnetting Practice

Practice these tasks:

* Convert full IPv6 addresses into compressed notation.
* Expand compressed IPv6 addresses.
* Identify network prefixes.
* Calculate the number of available subnets.
* Divide `/48` allocations into `/64` networks.
* Divide `/56` allocations into `/64` networks.
* Plan addressing for branch offices.
* Allocate prefixes to customer networks.

### 🖆 Point-to-Point Prefixes

A `/127` prefix can be used for point-to-point links between routers.

Example:

```text
2001:db8:100:1::/127
```

Understand the operational considerations and relevant standards before choosing `/127` or `/64` for point-to-point networks.

# 🗁 IPv6 Neighbor Discovery Protocol

### 🖆 What Is NDP?

Neighbor Discovery Protocol is an IPv6 protocol used for communication between nodes on the same link.

NDP replaces several functions that were traditionally provided by IPv4 ARP and other mechanisms.

It uses ICMPv6 messages.

### 🖆 NDP Functions

NDP provides:

* Router discovery.
* Neighbor discovery.
* Address resolution.
* Neighbor reachability detection.
* Duplicate Address Detection.
* Redirect messages.
* Prefix discovery.

### 🖆 Neighbor Solicitation

Neighbor Solicitation messages are used to discover neighboring nodes and perform Duplicate Address Detection.

### 🖆 Neighbor Advertisement

Neighbor Advertisement messages provide information in response to Neighbor Solicitations or announce neighbor information.

### 🖆 Router Solicitation

A host sends a Router Solicitation message to request router information.

### 🖆 Router Advertisement

Routers send Router Advertisement messages containing information such as:

* Network prefixes.
* Default router information.
* Router lifetime.
* Address autoconfiguration flags.
* Other configuration parameters.

### 🖆 Neighbor Cache

RouterOS maintains IPv6 neighbor information.

Inspect it:

```routeros
/ipv6 neighbor print detail
```

Understand:

* Neighbor address.
* Link-layer address.
* Interface.
* Reachability state.
* Neighbor discovery behavior.

# 🗁 IPv6 Router Advertisements

### 🖆 Router Advertisement Fundamentals

Router Advertisements communicate IPv6 network information to hosts.

They can provide:

* Prefix information.
* Default router information.
* Address autoconfiguration settings.
* Other network configuration information.

### 🖆 Configure Router Advertisements

RouterOS uses the IPv6 Neighbor Discovery configuration.

Inspect the configuration:

```routeros
/ipv6 nd print detail
```

Inspect advertised prefixes:

```routeros
/ipv6 nd prefix print detail
```

### 🖆 Enable Prefix Advertisement

A static IPv6 address can be configured to advertise its prefix.

Example:

```routeros
/ipv6 address
add address=2001:db8:10::1/64 interface=ether2 advertise=yes
```

The relevant Neighbor Discovery configuration must also permit Router Advertisements on the interface.

### 🖆 Router Advertisement Parameters

Study:

* Managed Address Configuration flag.
* Other Configuration flag.
* Router lifetime.
* Prefix lifetime.
* Autonomous flag.
* On-link flag.
* Advertisement interval.

Understand how these parameters influence host configuration.

### 🖆 Router Advertisement Security

Rogue Router Advertisements can redirect traffic or provide malicious network configuration.

Use appropriate Layer two protections and network access controls to prevent unauthorized devices from acting as IPv6 routers.

# 🗁 IPv6 SLAAC and DHCPv6

### 🖆 SLAAC Fundamentals

SLAAC stands for Stateless Address Autoconfiguration.

It allows an IPv6 host to configure its own address using network information received through Router Advertisements.

The process generally involves:

* Discovering routers.
* Receiving advertised prefixes.
* Generating an interface identifier.
* Performing Duplicate Address Detection.
* Configuring an IPv6 address.

### 🖆 DHCPv6 Fundamentals

DHCPv6 provides IPv6 configuration services.

It supports:

* Stateful address assignment.
* Stateless configuration information.
* Additional network parameters.

DHCPv6 does not replace Router Advertisements for default-router discovery.

### 🖆 Stateful DHCPv6

In stateful DHCPv6, the DHCPv6 server maintains address allocation information for clients.

Understand:

* DHCPv6 client.
* DHCPv6 server.
* DHCPv6 relay.
* Address pool.
* Lease management.
* Identity Association.

### 🖆 Stateless DHCPv6

Stateless DHCPv6 provides additional configuration information while hosts obtain their addresses through SLAAC.

For example, DNS configuration can be supplied through DHCPv6.

### 🖆 SLAAC and DHCPv6 Comparison

| Feature | SLAAC | DHCPv6 |
| --- | --- | --- |
| Address configuration | Host-generated | Server-provided in stateful mode |
| Router discovery | Router Advertisement | Still requires Router Advertisement |
| Address tracking | Not inherently centralized | Stateful server maintains leases |
| DNS configuration | Can use RA options such as RDNSS | Can provide DNS information |
| Server required | No | Yes |

### 🖆 RouterOS DHCPv6

Study the following RouterOS components:

```routeros
/ipv6 dhcp-server
/ipv6 dhcp-client
/ipv6 pool
```

Understand the difference between receiving a delegated prefix from an ISP and assigning addresses or prefixes to downstream clients.

# 🗁 IPv6 Routing

### 🖆 IPv6 Routing Fundamentals

IPv6 routing forwards packets between different IPv6 networks.

Routers use IPv6 routing tables to select the appropriate next hop.

Inspect IPv6 routes:

```routeros
/ipv6 route print detail
```

### 🖆 Connected Routes

Connected routes are created when IPv6 addresses are assigned to interfaces.

Example:

```routeros
/ipv6 address
add address=2001:db8:10::1/64 interface=ether2 advertise=no
```

The corresponding connected network becomes available in the routing table.

### 🖆 Static IPv6 Routes

Static routes are manually configured.

Example:

```routeros
/ipv6 route
add dst-address=2001:db8:20::/64 gateway=2001:db8:10::2
```

### 🖆 Default IPv6 Route

The IPv6 default route is:

```text
::/0
```

Example:

```routeros
/ipv6 route
add dst-address=::/0 gateway=2001:db8:10::2
```

### 🖆 Dynamic IPv6 Routing

Study dynamic routing protocols that support IPv6:

* OSPFv3.
* BGP for IPv6 unicast.
* RIPng.
* IS-IS for IPv6.

Understand:

* Neighbor relationships.
* Route advertisements.
* Route selection.
* Metrics.
* Administrative preference.
* Route redistribution.

### 🖆 OSPFv3

OSPFv3 provides link-state routing for IPv6 networks.

Study:

* Router ID.
* Areas.
* Neighbor discovery.
* Adjacencies.
* Link-state advertisements.
* SPF calculation.
* Route installation.

### 🖆 BGP IPv6

BGP supports IPv6 routing through the IPv6 unicast address family.

Study:

* IPv6 peer addressing.
* MP-BGP.
* AS numbers.
* Prefix advertisements.
* Route filters.
* Local preference.
* AS path.
* Next-hop handling.

### 🖆 RouterOS IPv6 Routing Practice

Build a topology with multiple routers.

Tasks:

* Assign IPv6 addresses.
* Configure static routes.
* Configure default routes.
* Establish OSPFv3 neighbors.
* Advertise IPv6 prefixes through BGP.
* Verify routing tables.
* Test end-to-end connectivity.

# 🗁 IPv6 Firewall Security

### 🖆 IPv6 Firewall Fundamentals

IPv6 has its own firewall configuration in RouterOS.

Inspect the rules:

```routeros
/ipv6 firewall filter print detail
```

IPv4 firewall rules do not automatically protect IPv6 traffic.

### 🖆 IPv6 Input Chain

The input chain protects the router itself.

A basic policy should:

* Accept established and related traffic.
* Drop invalid traffic.
* Permit necessary ICMPv6.
* Permit required neighbor discovery.
* Permit authorized management access.
* Drop unauthorized access.

Example:

```routeros
/ipv6 firewall filter
add chain=input action=accept connection-state=established,related
add chain=input action=drop connection-state=invalid
add chain=input action=accept protocol=icmpv6
add chain=input action=accept src-address=2001:db8:10::/64
add chain=input action=drop
```

This is an illustrative policy. Production rules must account for required routing protocols, VPN traffic, management access, and other infrastructure services.

### 🖆 IPv6 Forward Chain

The forward chain controls traffic passing through the router.

Example:

```routeros
/ipv6 firewall filter
add chain=forward action=accept connection-state=established,related
add chain=forward action=drop connection-state=invalid
add chain=forward action=drop src-address=2001:db8:30::/64 dst-address=2001:db8:10::/64 comment="Block guest access to management"
```

Build explicit policies for all network segments.

### 🖆 ICMPv6 Security

ICMPv6 is essential to IPv6 operation.

It supports:

* Neighbor Discovery.
* Router Discovery.
* Packet Too Big messages.
* Destination Unreachable messages.
* Time Exceeded messages.
* Echo requests and replies.

Blocking all ICMPv6 traffic can break essential IPv6 functions.

Apply controlled filtering rather than indiscriminately dropping ICMPv6.

### 🖆 IPv6 Security Best Practices

* Restrict administrative access.
* Permit necessary ICMPv6 traffic.
* Filter unauthorized inbound traffic.
* Prevent rogue Router Advertisements.
* Secure neighbor discovery where supported.
* Monitor unusual IPv6 traffic.
* Protect internal network segments.
* Review exposed services.
* Do not assume NAT is required for IPv6 security.

# 🗁 IPv6 Transition Technologies

### 🖆 Dual Stack

Dual stack allows IPv4 and IPv6 to operate simultaneously.

A dual-stack device has both IPv4 and IPv6 connectivity.

Advantages:

* Supports IPv4-only services.
* Supports IPv6-enabled services.
* Allows gradual migration.
* Reduces dependence on translation mechanisms.

### 🖆 Tunneling

IPv6 tunneling carries IPv6 traffic across an IPv4 network or vice versa.

Study:

* 6in4.
* GRE.
* IPIP.
* Other supported tunneling mechanisms.

Understand encapsulation overhead, MTU considerations, and security implications.

### 🖆 NAT64

NAT64 enables communication between IPv6-only clients and IPv4-only servers through address translation.

Study:

* NAT64.
* DNS64.
* IPv6-only client networks.
* IPv4 service access.

### 🖆 464XLAT

464XLAT is a transition technology used to provide IPv4 connectivity to IPv6-only clients through a combination of translation mechanisms.

It is particularly relevant in some mobile and service-provider networks.

### 🖆 Transition Strategy

Understand how to select an appropriate transition approach based on:

* Existing infrastructure.
* ISP capabilities.
* Application compatibility.
* Address availability.
* Operational complexity.
* Monitoring requirements.

# 🗁 IPv6 VPN and Tunneling

### 🖆 IPv6 over IPsec

IPsec can protect IPv6 traffic using authentication and encryption.

Study:

* IKEv2.
* ESP.
* Security Associations.
* Encryption algorithms.
* Authentication algorithms.
* Traffic selectors.
* Tunnel mode.

### 🖆 WireGuard with IPv6

WireGuard can carry IPv6 traffic between peers.

Example peer address:

```text
fd12:3456:789a:1::2/128
```

Example allowed address:

```routeros
/interface wireguard peers
add interface=wg1 public-key="REMOTE_PUBLIC_KEY" allowed-address=fd12:3456:789a:1::2/128
```

Configure the interface IPv6 address and routing as required.

### 🖆 IPv6 Tunnel Troubleshooting

Check:

* Endpoint reachability.
* Peer keys.
* IPv6 routes.
* Firewall policies.
* MTU.
* Prefix assignments.
* Return-path routing.

# 🗁 IPv6 DNS

### 🖆 IPv6 DNS Fundamentals

DNS supports IPv6 through specific record types.

Important record types:

| Record | Purpose |
| --- | --- |
| A | IPv4 address |
| AAAA | IPv6 address |
| PTR | Reverse DNS |
| NS | Name server |
| MX | Mail exchanger |
| CNAME | Canonical name |

### 🖆 AAAA Records

An AAAA record maps a hostname to an IPv6 address.

Example:

```text
www.example.com. IN AAAA 2001:db8:100::10
```

### 🖆 Reverse DNS

IPv6 reverse DNS uses the `ip6.arpa` domain.

IPv6 addresses are represented by reversing their hexadecimal nibbles and separating them with dots.

### 🖆 DNS64

DNS64 synthesizes AAAA records from IPv4 A records when used with NAT64.

Understand why this mechanism is useful for IPv6-only networks that need to reach IPv4-only services.

# 🗁 IPv6 Monitoring and Troubleshooting

### 🖆 Useful RouterOS Commands

Inspect IPv6 addresses:

```routeros
/ipv6 address print detail
```

Inspect routes:

```routeros
/ipv6 route print detail
```

Inspect neighbors:

```routeros
/ipv6 neighbor print detail
```

Inspect firewall rules:

```routeros
/ipv6 firewall filter print detail
```

Inspect DHCPv6 configuration:

```routeros
/ipv6 dhcp-client print detail
/ipv6 dhcp-server print detail
```

Test connectivity:

```routeros
/ping 2001:db8:10::2
```

### 🖆 Linux IPv6 Troubleshooting

Display IPv6 addresses:

```bash
ip -6 address show
```

Display IPv6 routes:

```bash
ip -6 route show
```

Display neighbor entries:

```bash
ip -6 neighbor show
```

Test connectivity:

```bash
ping -6 2001:db8:10::1
```

Trace the route:

```bash
traceroute -6 2001:db8:10::1
```

Query AAAA records:

```bash
dig AAAA example.com
```

### 🖆 Common IPv6 Troubleshooting Problems

Study:

* Incorrect IPv6 prefix length.
* Missing default route.
* Missing Router Advertisement.
* Duplicate IPv6 address.
* Incorrect DHCPv6 configuration.
* Missing DNS AAAA records.
* Firewall blocking ICMPv6.
* Neighbor Discovery failures.
* Incorrect VLAN configuration.
* MTU and Path MTU Discovery problems.
* Incorrect routing advertisements.

### 🖆 Packet Capture

Use packet capture to investigate IPv6 communication.

Important ICMPv6 message types include:

| Type | Message |
| ---: | --- |
| 1 | Destination Unreachable |
| 2 | Packet Too Big |
| 3 | Time Exceeded |
| 128 | Echo Request |
| 129 | Echo Reply |
| 133 | Router Solicitation |
| 134 | Router Advertisement |
| 135 | Neighbor Solicitation |
| 136 | Neighbor Advertisement |

Understanding these messages helps identify IPv6 connectivity and neighbor-discovery problems.

# 🗁 IPv6 Deployment Planning

### 🖆 Address Planning

Design an IPv6 addressing plan before deploying IPv6 across an organization.

Consider:

* ISP-provided prefix.
* Site allocations.
* VLAN allocations.
* Loopback addresses.
* Point-to-point links.
* Infrastructure networks.
* Management networks.
* Customer allocations.

### 🖆 ISP IPv6 Deployment

Study IPv6 deployment in service-provider environments:

* Prefix delegation.
* Customer Prefix Allocation.
* DHCPv6 Prefix Delegation.
* IPv6 routing.
* BGP IPv6.
* IPv6 peering.
* Dual-stack customer services.
* IPv6-only customer services.
* Reverse DNS delegation.

### 🖆 Prefix Delegation

An ISP can delegate an IPv6 prefix to a customer router using DHCPv6 Prefix Delegation.

For example, a customer might receive:

```text
2001:db8:1000:1200::/56
```

The customer can then divide this allocation into multiple `/64` LAN networks.

### 🖆 Dual-Stack Network Design

A dual-stack design should account for:

* IPv4 and IPv6 addressing.
* Separate IPv4 and IPv6 firewall policies.
* Routing for both protocols.
* DNS A and AAAA records.
* Monitoring for both protocols.
* Application compatibility.

### 🖆 IPv6 Security Planning

Include:

* Router Advertisement protection.
* Neighbor Discovery security.
* Firewall policies.
* Management access restrictions.
* IPv6 source-address validation.
* Logging and monitoring.
* Secure prefix allocation.

# 🗁 Practical IPv6 Labs

### 🖆 Basic IPv6 Addressing Lab

**Objective:** Configure IPv6 addressing between two devices.

Tasks:

* Assign IPv6 addresses.
* Configure `/64` prefixes.
* Verify link-local addresses.
* Test connectivity.
* Inspect neighbor tables.

### 🖆 IPv6 Subnetting Lab

**Objective:** Divide a larger allocation into multiple networks.

Given:

```text
2001:db8:5000::/48
```

Tasks:

* Create multiple `/64` subnets.
* Allocate prefixes to different VLANs.
* Document the addressing plan.
* Assign gateway addresses.
* Test communication.

### 🖆 SLAAC Lab

**Objective:** Configure automatic IPv6 addressing.

Tasks:

* Configure an IPv6 prefix on the router.
* Enable appropriate Router Advertisements.
* Connect Linux clients.
* Observe address generation.
* Verify default-router information.
* Inspect neighbor discovery.

### 🖆 DHCPv6 Lab

**Objective:** Configure DHCPv6 services.

Tasks:

* Configure a DHCPv6 server or client.
* Configure address or prefix allocation as appropriate.
* Test client configuration.
* Verify lease information.
* Compare stateful and stateless behavior.

### 🖆 IPv6 Routing Lab

**Objective:** Connect multiple IPv6 networks.

Tasks:

* Configure IPv6 addresses on multiple routers.
* Add static routes.
* Configure a default route.
* Test end-to-end connectivity.
* Configure OSPFv3.
* Verify routing advertisements.

### 🖆 IPv6 Firewall Lab

**Objective:** Secure an IPv6 network.

Tasks:

* Configure input filtering.
* Configure forward filtering.
* Permit required ICMPv6 traffic.
* Restrict management access.
* Block unauthorized inter-VLAN traffic.
* Verify permitted and denied communication.

### 🖆 ISP IPv6 Deployment Lab

**Objective:** Simulate an ISP providing IPv6 services to customers.

Requirements:

* ISP router.
* Customer edge router.
* IPv6 transit network.
* Prefix delegation.
* Customer LAN networks.
* IPv6 routing.
* DNS configuration.
* Firewall protection.
* Monitoring and troubleshooting.

# 🗁 Common IPv6 Mistakes

Avoid these mistakes:

* Treating IPv6 addresses like IPv4 addresses.
* Using incorrect prefix lengths.
* Assuming every IPv6 LAN should use a different prefix size.
* Forgetting link-local addressing.
* Blocking essential ICMPv6 traffic.
* Assuming DHCPv6 provides default-router information.
* Forgetting Router Advertisements.
* Configuring IPv6 addresses without routing.
* Assuming IPv4 firewall rules protect IPv6.
* Treating NAT as a mandatory IPv6 security mechanism.
* Ignoring IPv6 DNS records.
* Failing to monitor IPv6 traffic.
* Deploying IPv6 without an addressing plan.
* Ignoring IPv6 MTU and Path MTU Discovery behavior.

# 🗁 Study and Preparation Strategy

### 🖆 Build Your IPv6 Foundation

Study:

* Binary and hexadecimal conversion.
* IPv6 address notation.
* IPv6 prefix lengths.
* Unicast and multicast.
* Link-local addressing.
* IPv6 subnetting.
* ICMPv6.
* Neighbor Discovery.

### 🖆 Practice RouterOS IPv6

Build practical experience with:

* IPv6 address configuration.
* Static IPv6 routes.
* Router Advertisements.
* SLAAC.
* DHCPv6.
* Prefix delegation.
* OSPFv3.
* BGP IPv6.
* IPv6 firewall.
* IPv6 VPN connectivity.

### 🖆 Build a Dedicated IPv6 Lab

Recommended environment:

* Two or more MikroTik CHR routers.
* Linux client machines.
* A simulated ISP router.
* Multiple VLANs.
* DHCPv6 services.
* A DNS server.
* A packet-capture environment.

### 🖆 Validate Your Knowledge

For each lab, answer:

* How is an IPv6 address structured?
* What does a `/64` prefix represent?
* What is the purpose of a link-local address?
* How does NDP replace ARP functionality?
* How does SLAAC work?
* What is the difference between stateful and stateless DHCPv6?
* Why are Router Advertisements necessary?
* How does IPv6 routing work?
* Why must ICMPv6 be handled carefully?
* How does prefix delegation work in an ISP environment?

### 🖆 Official Certification Preparation

Consult MikroTik's current official training catalog and certification information to verify the availability, course scope, prerequisites, and examination requirements for the IPv6 certification track.

# 🗁 Additional Resources

* MikroTik Official Website: <https://mikrotik.com/>
* MikroTik Training: <https://mikrotik.com/training>
* MikroTik RouterOS Documentation: <https://help.mikrotik.com/docs/>
* MikroTik IPv6 Documentation: <https://help.mikrotik.com/docs/>
* RFC 8200 — Internet Protocol, Version 6 (IPv6) Specification: <https://www.rfc-editor.org/rfc/rfc8200>
* RFC 4291 — IP Version 6 Addressing Architecture: <https://www.rfc-editor.org/rfc/rfc4291>
* RFC 4861 — Neighbor Discovery for IP Version 6: <https://www.rfc-editor.org/rfc/rfc4861>
* RFC 4862 — IPv6 Stateless Address Autoconfiguration: <https://www.rfc-editor.org/rfc/rfc4862>
* RFC 8415 — DHCP for IPv6 (DHCPv6): <https://www.rfc-editor.org/rfc/rfc8415>
* RFC 4193 — Unique Local IPv6 Unicast Addresses: <https://www.rfc-editor.org/rfc/rfc4193>

Always verify RouterOS syntax and feature support against the documentation for your specific RouterOS version and MikroTik device.
