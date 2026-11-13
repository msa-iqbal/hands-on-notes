# Public IP and Private IP Planning

IP address planning is the process of designing, allocating, documenting, and managing IP address spaces across a network.

For an Internet Service Provider (ISP), proper IP address planning is essential for subscriber connectivity, network infrastructure, routing, security, scalability, and efficient address utilization.

A well-designed IP addressing plan helps prevent overlapping subnets, simplifies route aggregation, improves troubleshooting, and supports future network expansion.

# 🗁 IP Addressing Fundamentals

An IP address is a logical identifier assigned to a network interface. It enables devices to communicate across IP networks.

IPv4 addresses are 32 bits long and are commonly represented in dotted-decimal notation.

Example:

```text
192.168.10.25
```

An IPv4 address consists of two logical parts:

* **Network portion:** Identifies the network.
* **Host portion:** Identifies an interface within that network.

The subnet mask or CIDR prefix determines how many bits belong to the network portion.

Example:

```text
IP Address:  192.168.10.25
Subnet Mask: 255.255.255.0
CIDR:        /24
Network:     192.168.10.0
Broadcast:   192.168.10.255
Usable Hosts: 192.168.10.1 - 192.168.10.254
```

For a conventional IPv4 subnet, the number of usable host addresses is:

```text
Usable Hosts = 2^(32 - Prefix Length) - 2
```

The formula excludes the network and broadcast addresses. Special cases include `/31` point-to-point links and `/32` host routes.

# 🗁 Public and Private IPv4 Addresses

IPv4 addresses are commonly categorized as public or private based on their intended use and global routability.

### 🖆 Public IPv4 Addresses

A public IPv4 address is globally unique and can be routed across the public Internet, subject to routing and filtering policies.

ISPs obtain public IPv4 address allocations through the appropriate Regional Internet Registry (RIR) or upstream provider.

Public addresses are commonly used for:

* ISP-facing network infrastructure.
* Public DNS servers.
* Web and application servers.
* Customer connections requiring directly assigned public addresses.
* BGP peering and other externally reachable services.

Example documentation address:

```text
203.0.113.10
```

This address belongs to a documentation-only range and must not be used as a real public allocation.

An ISP must never arbitrarily assign an address from an unrelated public range. It must use address space allocated or legitimately assigned to it.

### 🖆 Private IPv4 Addresses

Private IPv4 addresses are intended for internal networks and are not globally routed across the public Internet.

RFC 1918 defines three private IPv4 ranges.

| Private Range | CIDR | Address Count |
| --- | --- | ---: |
| 10.0.0.0 - 10.255.255.255 | 10.0.0.0/8 | 16,777,216 |
| 172.16.0.0 - 172.31.255.255 | 172.16.0.0/12 | 1,048,576 |
| 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16 | 65,536 |

Private addresses are commonly used for:

* Router and switch management.
* OLT and access network management.
* Internal infrastructure.
* Subscriber connections behind NAT.
* Internal DNS, RADIUS, monitoring, and NTP services.
* Customer LAN networks.

Private addresses require NAT or another suitable translation mechanism when communicating with the public IPv4 Internet.

### 🖆 Public vs Private IPv4

| Feature | Public IPv4 | Private IPv4 |
| --- | --- | --- |
| Global Internet routing | Yes, subject to routing policy | No |
| Global uniqueness | Must be globally unique | Can overlap across separate networks |
| Common usage | Internet-facing services | Internal networks |
| NAT requirement | Not inherently required | Usually required for public Internet access |
| Allocation | RIR or upstream provider | Network administrator |

# 🗁 Special-Purpose IPv4 Address Ranges

Some IPv4 ranges have special purposes and should not be treated as ordinary public or private address space.

| Address Range | Purpose |
| --- | --- |
| 127.0.0.0/8 | Loopback |
| 169.254.0.0/16 | IPv4 link-local |
| 100.64.0.0/10 | Shared address space for carrier-grade NAT |
| 192.0.2.0/24 | Documentation and examples |
| 198.51.100.0/24 | Documentation and examples |
| 203.0.113.0/24 | Documentation and examples |
| 224.0.0.0/4 | IPv4 multicast |

The shared address range `100.64.0.0/10` is particularly important for ISPs implementing CGNAT.

It is defined by RFC 6598 and is not part of the RFC 1918 private address ranges.

Documentation ranges should be used in examples and training materials, not as production allocations.

# 🗁 CIDR and Subnet Planning

CIDR (Classless Inter-Domain Routing) allows networks to be represented using a prefix length rather than traditional class-based addressing.

Example:

```text
192.168.10.0/24
```

The `/24` indicates that the first 24 bits identify the network, leaving 8 bits for host addresses.

Common IPv4 subnet sizes:

| CIDR | Total Addresses | Conventional Usable Hosts |
| --- | ---: | ---: |
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /29 | 8 | 6 |
| /30 | 4 | 2 |
| /31 | 2 | 2 on point-to-point links |
| /32 | 1 | 1 host route |

### 🖆 Variable Length Subnet Masking (VLSM)

VLSM allows different subnet sizes to be allocated according to actual requirements.

For example, an ISP may need:

* A `/24` for a management network.
* A `/26` for a server network.
* A `/27` for a monitoring network.
* A `/31` for a point-to-point router link.

Using VLSM avoids wasting address space and makes allocation more efficient.

### 🖆 Avoiding Overlapping Subnets

Every network segment must have a unique and non-overlapping address range within the routing domain where those networks must communicate.

Incorrect example:

```text
Network A: 10.10.0.0/24
Network B: 10.10.0.128/25
```

Network B overlaps with Network A.

Correct example:

```text
Network A: 10.10.0.0/24
Network B: 10.10.1.0/24
```

Overlapping subnets can cause routing ambiguity, connectivity failures, and operational problems.

# 🗁 ISP IP Addressing Requirements

An ISP needs a structured addressing plan covering its entire infrastructure and customer base.

Typical address categories include:

| Category | Purpose |
| --- | --- |
| Management | Router, switch, OLT, and device management |
| Infrastructure | Internal network services and infrastructure |
| Loopback | Stable router identification and routing endpoints |
| Point-to-point | Links between routers and network devices |
| Subscriber pools | PPPoE and IPoE customer addressing |
| CGNAT | Shared IPv4 addresses for subscriber translation |
| Public NAT pools | Public addresses used by CGNAT |
| Public services | DNS, web, VPN, and other Internet-facing services |
| Customer LAN | Private networks inside customer premises |
| Monitoring | Monitoring, logging, and telemetry systems |

A good plan separates these categories to simplify administration, troubleshooting, and access control.

# 🗁 Designing an ISP IPv4 Addressing Plan

A hierarchical addressing plan assigns address blocks according to network function and location.

The following is an illustrative private IPv4 allocation plan.

| Network Category | Example Allocation | Intended Use |
| --- | --- | --- |
| Management | 10.0.0.0/16 | Device management |
| Infrastructure | 10.1.0.0/16 | Internal services |
| Loopbacks | 10.2.0.0/16 | Router loopback addresses |
| Point-to-point | 10.3.0.0/16 | Router interconnections |
| Subscriber private | 10.16.0.0/12 | PPPoE/IPoE subscribers |
| Shared CGNAT | 100.64.0.0/10 | CGNAT subscriber addresses |
| Public NAT pool | Allocated public prefix | Public translation addresses |

These are documentation examples, not actual public allocations or mandatory ISP standards. The exact ranges and subnet sizes should be selected according to network scale, existing allocations, and growth requirements.

### 🖆 Management Network Example

Management networks should be separated from subscriber traffic.

Example:

```text
Management Block: 10.0.0.0/16

Core Routers:     10.0.1.0/24
Aggregation:      10.0.2.0/24
Access Switches:  10.0.3.0/24
OLT Management:   10.0.4.0/24
```

This structure makes it easier to identify device locations and apply management access policies.

### 🖆 Loopback Address Planning

Loopback addresses are logical interface addresses that remain available independently of a particular physical interface's operational state.

They are commonly used for:

* Router identification.
* OSPF router IDs.
* BGP update sources.
* Network management.
* Monitoring and diagnostics.

Example:

```text
Core Router 1: 10.2.0.1/32
Core Router 2: 10.2.0.2/32
Edge Router 1: 10.2.0.3/32
```

A `/32` is commonly used for an individual IPv4 loopback address.

### 🖆 Point-to-Point Address Planning

Point-to-point links connect two network devices.

A `/31` can be used for IPv4 point-to-point links as defined in RFC 3021. Both addresses are usable as endpoint addresses on the link.

Example:

```text
Link Network: 10.3.0.0/31

Router A: 10.3.0.0/31
Router B: 10.3.0.1/31
```

A `/30` may also be used where required by the equipment or network design.

# 🗁 Subscriber IP Address Allocation

ISPs commonly allocate subscriber addresses through PPPoE or IPoE.

### 🖆 PPPoE Address Allocation

In a PPPoE network, the subscriber establishes a PPP session with the ISP's access concentrator or Broadband Network Gateway (BNG).

The ISP can allocate an address dynamically or statically.

Example:

```text
Subscriber 1: 10.16.0.10
Subscriber 2: 10.16.0.11
Subscriber 3: 10.16.0.12
```

Address allocation can be integrated with RADIUS for centralized subscriber authentication and policy management.

### 🖆 IPoE Address Allocation

IPoE subscribers typically receive IP configuration through DHCP or another access-network provisioning mechanism.

A DHCP server can allocate addresses based on:

* Subscriber identity.
* Access VLAN.
* Circuit or remote identifier.
* MAC address.
* DHCP options.
* Service profile.

The exact implementation depends on the ISP's access architecture.

### 🖆 Static and Dynamic Subscriber Addresses

| Address Type | Description |
| --- | --- |
| Dynamic | Assigned automatically from a configured pool |
| Static | Reserved for a specific subscriber |
| Public | Globally routable address assigned to a customer |
| Private | Internal address that usually requires NAT for Internet access |

Subscriber pools should be sized according to the number of active sessions, address allocation policy, expected growth, and whether customers receive one or multiple addresses.

# 🗁 CGNAT Address Planning

Carrier-Grade NAT allows an ISP to share a limited public IPv4 address pool among multiple subscribers.

RFC 6598 reserves the following shared address space:

```text
100.64.0.0/10
```

This range contains addresses from:

```text
100.64.0.0 - 100.127.255.255
```

An ISP can allocate shared addresses to subscribers and translate their traffic to public IPv4 addresses through a CGNAT system.

Example:

```text
Subscriber 1: 100.64.1.10
Subscriber 2: 100.64.1.11
Subscriber 3: 100.64.1.12

Public NAT Pool: 203.0.113.0/24
```

`203.0.113.0/24` is a documentation prefix in this example, not a production NAT allocation.

Important CGNAT planning considerations:

* Allocate sufficient shared address space for subscriber growth.
* Plan public NAT address capacity.
* Define port allocation policies.
* Monitor concurrent sessions and port utilization.
* Maintain accurate translation logs.
* Synchronize system clocks for reliable logging.
* Consider subscriber fairness and port exhaustion.
* Avoid overlapping shared address space with other internal networks where routing or VPN connectivity requires distinct addressing.

CGNAT architecture and implementation details are covered in the CGNAT documentation.

# 🗁 Hierarchical Addressing and Route Summarization

Hierarchical IP addressing organizes address space into logical blocks.

An ISP can allocate address ranges based on:

* Region.
* Point of Presence (POP).
* Core and aggregation layers.
* Access network.
* Service category.
* Subscriber group.

Example:

```text
ISP Internal Address Space
|
+-- Region A
|   |
|   +-- POP A1
|   +-- POP A2
|
+-- Region B
    |
    +-- POP B1
    +-- POP B2
```

Each region or POP can receive a dedicated address block.

### 🖆 Route Summarization

Route summarization combines multiple contiguous networks into a larger prefix.

For example:

```text
10.10.0.0/24
10.10.1.0/24
10.10.2.0/24
10.10.3.0/24
```

These four networks can be summarized as:

```text
10.10.0.0/22
```

Route summarization reduces routing table size and simplifies routing information exchange.

For effective summarization:

* Allocate contiguous address blocks.
* Maintain consistent subnet boundaries.
* Avoid unnecessary fragmentation of address space.
* Design regional and POP allocations carefully.
* Ensure summarized routes do not accidentally include unrelated or unreachable networks.

# 🗁 IPv6 Address Planning

IPv6 provides a 128-bit address space and is designed to support large-scale network growth.

An ISP should develop an IPv6 addressing strategy alongside its IPv4 plan.

Common IPv6 address categories include:

| Address Type | Prefix | Purpose |
| --- | --- | --- |
| Global Unicast | 2000::/3 | Globally routable IPv6 addresses |
| Unique Local | fc00::/7 | Private internal IPv6 addressing |
| Link-Local | fe80::/10 | Communication on a local link |
| Multicast | ff00::/8 | One-to-many communication |
| Loopback | ::1/128 | Local host loopback |
| Unspecified | ::/128 | Unspecified address |

### 🖆 IPv6 Allocation Structure

An ISP may receive a large IPv6 allocation from its RIR or upstream provider.

It can then divide the allocation among regions, POPs, infrastructure, and subscribers.

An illustrative hierarchy:

```text
ISP IPv6 Allocation
|
+-- Infrastructure
|   |
|   +-- Core Network
|   +-- Management
|   +-- Loopbacks
|
+-- Region A
|   |
|   +-- POP A1
|   +-- POP A2
|
+-- Region B
    |
    +-- POP B1
    +-- POP B2
```

### 🖆 Common IPv6 Prefix Sizes

| Prefix | Common Use |
| --- | --- |
| /32 | Example of a large provider allocation |
| /48 | Common site allocation |
| /56 | Common residential customer delegation |
| /64 | Typical IPv6 LAN subnet |
| /128 | Individual host address |

These are common planning conventions, not universal requirements. Actual allocations should follow the applicable RIR policies and network design.

### 🖆 IPv6 Prefix Delegation

An ISP can use DHCPv6 Prefix Delegation (DHCPv6-PD) to assign an IPv6 prefix to a customer router.

For example:

```text
Customer Delegated Prefix: 2001:db8:1000:1200::/56
```

The customer router can divide that delegated prefix into multiple `/64` LAN subnets.

The `2001:db8::/32` prefix is reserved for documentation and examples. It is not a production allocation.

# 🗁 IP Address Management

IP Address Management (IPAM) is the process of tracking, organizing, allocating, and documenting IP address space.

An ISP should maintain a centralized record of all allocated networks and addresses.

An IPAM system should track:

* Network address and prefix length.
* Subnet mask.
* VLAN and interface association.
* Device or subscriber assignment.
* Region and POP.
* Allocation status.
* Address utilization.
* DHCP and PPPoE pools.
* Public IPv4 assignments.
* CGNAT address pools.
* IPv6 prefix delegations.
* Allocation history.

### 🖆 IPAM Integration

IPAM becomes more useful when integrated with other ISP systems.

| System | Integration Purpose |
| --- | --- |
| DHCP | Track dynamic address assignments |
| PPPoE/BNG | Track subscriber sessions and address pools |
| RADIUS | Associate subscriber identity with assigned addresses |
| DNS | Maintain forward and reverse DNS records |
| Monitoring | Identify devices and network segments |
| Network automation | Provision and validate addressing configurations |

A centralized source of truth reduces manual errors and makes network operations more reliable.

# 🗁 IP Address Planning Best Practices

Follow these practices when designing an ISP addressing architecture:

* Separate management, infrastructure, subscriber, and service networks.
* Use RFC 1918 ranges for internal IPv4 networks.
* Use `100.64.0.0/10` for CGNAT shared address space where appropriate.
* Use only legitimately allocated public IPv4 addresses.
* Design address blocks hierarchically.
* Reserve space for future expansion.
* Avoid overlapping subnets.
* Use VLSM to allocate addresses efficiently.
* Plan loopback and point-to-point addressing consistently.
* Use route summarization where the topology supports it.
* Maintain accurate IPAM documentation.
* Restrict management network access.
* Apply anti-spoofing controls and appropriate ingress filtering.
* Consider Unicast Reverse Path Forwarding (uRPF) where suitable for the network topology.
* Plan IPv6 addressing from the beginning rather than treating it as an afterthought.
* Regularly review address utilization and allocation policies.

# 🗁 Practical Examples and Tools

Several tools can help network engineers calculate, validate, and manage IP address allocations.

### 🖆 Using ipcalc

`ipcalc` is a Linux command-line utility for calculating IPv4 network information.

Install it on Ubuntu:

```bash
sudo apt update
sudo apt install ipcalc
```

Calculate a subnet:

```bash
ipcalc 10.10.10.0/24
```

Example output fields include:

```text
Network
Netmask
Broadcast
HostMin
HostMax
Hosts
```

Calculate another subnet:

```bash
ipcalc 10.10.20.0/26
```

### 🖆 Using Python ipaddress

Python includes the built-in `ipaddress` module for IP address and network calculations.

Example:

```python
import ipaddress

network = ipaddress.ip_network("10.10.10.0/24")

print("Network:", network.network_address)
print("Netmask:", network.netmask)
print("Broadcast:", network.broadcast_address)
print("Total Addresses:", network.num_addresses)
print("Usable Hosts:", network.num_addresses - 2)

for address in network.hosts():
    print(address)
```

### 🖆 Checking Whether Two Networks Overlap

```python
import ipaddress

network_a = ipaddress.ip_network("10.10.0.0/24")
network_b = ipaddress.ip_network("10.10.1.0/24")

if network_a.overlaps(network_b):
    print("Networks overlap")
else:
    print("Networks do not overlap")
```

### 🖆 Calculating a Subnet for a Point-to-Point Link

```python
import ipaddress

network = ipaddress.ip_network("10.3.0.0/31")

print("Network:", network)
print("Endpoint Addresses:")

for address in network:
    print(address)
```

A `/31` is suitable for point-to-point IPv4 links when both endpoints support RFC 3021.

### 🖆 ISP Address Planning Checklist

Before deploying an addressing plan, verify the following:

* [ ] Management and infrastructure ranges are defined.
* [ ] Loopback addresses are assigned consistently.
* [ ] Point-to-point links have appropriate prefixes.
* [ ] Subscriber pools are sized for expected demand.
* [ ] CGNAT shared and public address pools are defined.
* [ ] Public address allocations are verified.
* [ ] IPv6 allocation and delegation policies are documented.
* [ ] No unintended subnet overlaps exist.
* [ ] Route summarization boundaries are identified.
* [ ] IPAM records are updated.
* [ ] Security and access-control policies are applied.
* [ ] Future growth capacity is reserved.

# 🗁 References

* [RFC 1918 — Address Allocation for Private Internets](https://datatracker.ietf.org/doc/html/rfc1918)
* [RFC 4632 — Classless Inter-domain Routing (CIDR)](https://datatracker.ietf.org/doc/html/rfc4632)
* [RFC 3021 — Using 31-Bit Prefixes on IPv4 Point-to-Point Links](https://datatracker.ietf.org/doc/html/rfc3021)
* [RFC 6598 — IANA-Reserved IPv4 Prefix for Shared Address Space](https://datatracker.ietf.org/doc/html/rfc6598)
* [RFC 4291 — IP Version 6 Addressing Architecture](https://datatracker.ietf.org/doc/html/rfc4291)
* [RFC 6177 — IPv6 Address Assignment to End Sites](https://datatracker.ietf.org/doc/html/rfc6177)
* [IANA IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)
* [RIPE NCC — Internet Number Resources](https://www.ripe.net/)
