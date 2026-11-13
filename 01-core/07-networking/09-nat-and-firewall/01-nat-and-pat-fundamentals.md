# NAT and PAT Fundamentals

Network Address Translation (NAT) is a networking technique that modifies IP addresses in packet headers as packets pass through a router or firewall.

Port Address Translation (PAT) is a form of NAT that allows multiple internal devices or connections to share one or more public IP addresses by translating transport-layer port numbers.

NAT and PAT are widely used in IPv4 networks to provide internet connectivity, conserve public IPv4 addresses, and publish internal services.

They are commonly implemented in:

* Home and office routers.
* Enterprise networks.
* Internet Service Provider (ISP) networks.
* Data centers.
* Cloud infrastructure.
* Carrier-Grade NAT (CGNAT) systems.

# 🗁 What Is NAT?

NAT stands for **Network Address Translation**.

It modifies the source or destination IP address of packets as they pass through a network device.

Depending on the implementation, NAT may also modify TCP or UDP port numbers.

Consider a private network connected to the internet through a router:

```text
              Private Network
               192.168.1.0/24

       ┌──────────┐
       │  Laptop  │
       │192.168.1.10
       └────┬─────┘
            │
       ┌────▼─────┐
       │  Router  │
       │          │
       │ LAN:     │
       │192.168.1.1
       │          │
       │ WAN:     │
       │203.0.113.10
       └────┬─────┘
            │
         Internet
            │
       ┌────▼─────┐
       │ Web      │
       │ Server   │
       │198.51.100.20
       └──────────┘
```

The laptop uses a private IP address:

```text
192.168.1.10
```

When it accesses an internet server, the router translates its source address to the public IP address:

```text
203.0.113.10
```

The external server sees the translated public address instead of the laptop's private address.

When the response returns, the router uses its translation and connection-tracking information to deliver the packet to the correct internal device.

# 🗁 Why NAT Is Used

NAT is primarily used to support communication between networks using different IP address spaces and to conserve public IPv4 addresses.

**Public IPv4 address conservation**

IPv4 has a limited address space. NAT allows many internal devices to share a smaller number of public IPv4 addresses.

**Private address reuse**

Organizations can use private IPv4 addresses internally without requiring a globally unique public address for every device.

**Internal addressing independence**

Organizations can change their internal IP addressing without necessarily changing their public-facing addressing.

**Publishing internal services**

Destination NAT can forward traffic arriving at a public IP address to an internal server.

**Overlapping address networks**

NAT can sometimes help connect networks that use overlapping IP address ranges, although redesigning the addressing plan is generally preferable.

# 🗁 Private and Public IPv4 Addresses

NAT is commonly used between private and public IPv4 networks.

**Private IPv4 address ranges**

RFC 1918 defines three private IPv4 address blocks.

| Range | CIDR | Address Count |
| --- | --- | ---: |
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` | 16,777,216 |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` | 1,048,576 |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` | 65,536 |

These addresses are not globally routable on the public internet.

**Public IPv4 addresses**

Public IPv4 addresses are globally unique addresses allocated for internet routing.

Example:

```text
203.0.113.10
```

This address belongs to a documentation-only range and is used for illustrative examples.

**Carrier-Grade NAT address space**

RFC 6598 reserves:

```text
100.64.0.0/10
```

This shared address space is commonly used between ISP customer networks and CGNAT systems.

It is not an RFC 1918 private address range.

# 🗁 What Is PAT?

PAT stands for **Port Address Translation**.

It is a form of NAT that translates IP addresses and transport-layer port numbers to allow multiple connections to share one public IP address.

PAT is also commonly called NAPT (Network Address and Port Translation).

Consider three internal devices:

| Device | Private IP |
| --- | --- |
| Laptop | `192.168.1.10` |
| Desktop | `192.168.1.11` |
| Phone | `192.168.1.12` |

All three devices access the internet through the same public IP:

```text
203.0.113.10
```

The router distinguishes their connections using translated source ports.

Example:

| Internal Source | Translated Source |
| --- | --- |
| `192.168.1.10:51500` | `203.0.113.10:40001` |
| `192.168.1.11:51501` | `203.0.113.10:40002` |
| `192.168.1.12:51502` | `203.0.113.10:40003` |

Although all devices share one public IP address, their connections can be distinguished using the translated transport information.

PAT is commonly used in home routers, enterprise gateways, and ISP networks.

# 🗁 How NAT Works

Assume the following network:

| Device | IP Address |
| --- | --- |
| Laptop | `192.168.1.10` |
| Router LAN | `192.168.1.1` |
| Router WAN | `203.0.113.10` |
| Web Server | `198.51.100.20` |

The laptop accesses the web server over TCP port `443`.

Before source NAT:

```text
Source IP:        192.168.1.10
Destination IP:   198.51.100.20
```

After source NAT:

```text
Source IP:        203.0.113.10
Destination IP:   198.51.100.20
```

The router changes the source IP address before forwarding the packet to the external network.

When the server responds, the router performs reverse translation so the response reaches the original internal device.

# 🗁 How PAT Works

PAT performs address and port translation.

Assume a laptop creates a TCP connection to a web server.

Original packet:

```text
Protocol:         TCP
Source IP:        192.168.1.10
Source Port:      51500
Destination IP:   198.51.100.20
Destination Port: 443
```

The router translates the source address and port:

```text
Protocol:         TCP
Source IP:        203.0.113.10
Source Port:      40001
Destination IP:   198.51.100.20
Destination Port: 443
```

The router records this mapping in its translation or connection-tracking table.

When the response arrives, the router identifies the matching connection and restores the original destination address and port.

**PAT packet flow**

```text
Internal Device
192.168.1.10:51500
        |
        | Original packet
        ▼
   NAT / PAT Router
        |
        | Address and port translation
        ▼
203.0.113.10:40001
        |
        ▼
 External Web Server
 198.51.100.20:443
        |
        | Response
        ▼
   NAT / PAT Router
        |
        | Reverse translation
        ▼
Internal Device
192.168.1.10:51500
```

The router uses the protocol and connection information to identify the correct internal destination.

TCP and UDP use separate transport protocols and port namespaces. Other protocols, such as ICMP, may use identifiers rather than TCP or UDP ports for relevant translation behavior.

# 🗁 NAT and PAT Comparison

| Feature | NAT | PAT |
| --- | --- | --- |
| Full Name | Network Address Translation | Port Address Translation |
| IP translation | Yes | Yes |
| Port translation | Not necessarily | Yes, when required |
| Address mapping | Can be one-to-one or many-to-one depending on type | Multiple connections share public addresses |
| Public IP usage | May require multiple public addresses | Conserves public addresses |
| Translation information | Address mappings | Address and transport mappings |
| Common use | Static and dynamic address translation | Home and enterprise internet access |

PAT is a specific NAT technique rather than a completely separate networking technology.

# 🗁 Types of NAT

NAT can be classified according to its mapping behavior and the packet fields being modified.

| Type | Description |
| --- | --- |
| Static NAT | Fixed one-to-one address mapping |
| Dynamic NAT | Temporary address allocation from a pool |
| PAT / NAPT | Multiple connections share public addresses using transport ports |
| Source NAT | Modifies the packet's source address |
| Destination NAT | Modifies the packet's destination address |

Static NAT and dynamic NAT describe how mappings are allocated.

PAT describes how multiple connections can share public IP addresses through port translation.

Source NAT and destination NAT describe which address field is modified.

These classifications can be combined in practical network designs.

# 🗁 NAT Terminology

**Inside local address**

The address assigned to an internal device as seen from the internal network.

Example:

```text
192.168.1.10
```

**Inside global address**

The address representing an internal device to external networks.

Example:

```text
203.0.113.10
```

**Outside local address**

The address of an external device as represented inside the internal network. It may be identical to its outside global address when no translation is applied.

**Outside global address**

The actual address assigned to an external device on the external network.

Example:

```text
198.51.100.20
```

These four terms are commonly used in Cisco NAT terminology.

**Original tuple**

The original packet's protocol, source address and port, and destination address and port before translation.

**Translated tuple**

The corresponding addressing and transport information after translation.

**NAT translation table**

A table containing configured or active translation mappings.

**Connection tracking**

A mechanism that tracks network connections and their states, allowing stateful firewall and NAT implementations to associate packets with existing flows.

# 🗁 Source NAT and Destination NAT

NAT operations are commonly classified according to which IP address is modified.

**Source NAT (SNAT)**

SNAT modifies the source IP address.

It is commonly used for outbound internet access.

```text
Before:

192.168.1.10 → 198.51.100.20

After:

203.0.113.10 → 198.51.100.20
```

**Destination NAT (DNAT)**

DNAT modifies the destination IP address.

It is commonly used for publishing internal services and port forwarding.

```text
Before:

198.51.100.50 → 203.0.113.10

After:

198.51.100.50 → 192.168.1.100
```

SNAT and DNAT describe the translated packet field. They do not determine whether the mapping is static or dynamic.

# 🗁 NAT Processing and Packet Flow

NAT processing is integrated with packet forwarding and connection tracking.

On Linux, Netfilter provides packet-processing hooks commonly used by nftables NAT rules.

| Hook | Typical Operation |
| --- | --- |
| PREROUTING | Destination NAT |
| INPUT | Traffic destined for the local system |
| FORWARD | Traffic routed through the system |
| OUTPUT | Locally generated packets |
| POSTROUTING | Source NAT |

**Typical outbound packet flow**

```text
Internal Host
      |
      ▼
PREROUTING
      |
      ▼
Routing Decision
      |
      ▼
FORWARD
      |
      ▼
POSTROUTING
      |
      ▼
Source NAT
      |
      ▼
External Network
```

**Typical inbound destination NAT flow**

```text
External Network
      |
      ▼
PREROUTING
      |
      ▼
Destination NAT
      |
      ▼
Routing Decision
      |
      ▼
FORWARD
      |
      ▼
Internal Server
```

For typical forwarded traffic, source NAT occurs in POSTROUTING after the routing decision, while destination NAT in PREROUTING occurs before the routing decision.

The exact processing behavior depends on the platform.

Connection tracking maintains flow information and supports reverse translation for response packets.

# 🗁 NAT Translation Table

A NAT translation table records mappings between original and translated traffic.

Example:

| Protocol | Original Source | Translated Source | Destination |
| --- | --- | --- | --- |
| TCP | `192.168.1.10:51500` | `203.0.113.10:40001` | `198.51.100.20:443` |
| TCP | `192.168.1.11:51501` | `203.0.113.10:40002` | `198.51.100.20:443` |
| UDP | `192.168.1.12:53000` | `203.0.113.10:40003` | `198.51.100.53:53` |

The router uses these mappings to identify the internal connection associated with returning traffic.

Mappings may be created dynamically and removed when connections close or their timeout expires.

# 🗁 NAT and Routing

Routing and NAT are different networking operations.

**Routing** determines where a packet should be forwarded.

**NAT** modifies packet addressing information.

For example:

```text
Source:      192.168.1.10
Destination: 198.51.100.20
```

The router consults its routing table to determine the next hop and outgoing interface.

If source NAT is configured, the router also changes the packet's source address.

A router can route packets without NAT. It can also perform NAT while forwarding traffic between interfaces.

Successful communication requires appropriate routing, NAT behavior, firewall policy, and return-path handling.

# 🗁 NAT Limitations

NAT introduces several networking challenges.

**End-to-end connectivity**

NAT modifies packet addressing and can make direct communication between devices more difficult.

**Inbound connectivity**

Devices behind source NAT generally cannot accept unsolicited inbound connections without an appropriate translation or forwarding rule.

**Carrier-Grade NAT**

When an ISP uses CGNAT, customers may not control the public-facing translation. Ordinary inbound port forwarding may therefore be unavailable.

**Application protocols**

Some protocols embed IP addresses or port information inside application payloads. NAT may not automatically update those values.

Application Layer Gateways (ALGs) can assist with certain protocols, but they may introduce compatibility and security issues.

**IPsec AH**

IPsec Authentication Header (AH) authenticates portions of the IP header. NAT modifies IP header fields, which conflicts with AH integrity protection.

IPsec deployments commonly use Encapsulating Security Payload (ESP) with NAT Traversal (NAT-T) when NAT is present.

**Peer-to-peer communication**

Some peer-to-peer applications require NAT traversal mechanisms such as STUN, TURN, or ICE.

**Troubleshooting complexity**

NAT changes packet addresses and sometimes ports. Troubleshooting may require examining traffic on both sides of the translating device.

# 🗁 NAT and IPv6

NAT is strongly associated with IPv4 because public IPv4 addresses are limited.

IPv6 provides a significantly larger address space, allowing networks to assign globally unique addresses to many devices.

Conventional NAT is generally unnecessary for IPv6 connectivity.

IPv6 networks commonly rely on:

* Global Unicast Addresses for globally routable communication.
* Unique Local Addresses for internal communication.
* Routing between networks.
* Firewall policies to control access.

IPv6 also supports Network Prefix Translation (NPTv6), defined by RFC 6296.

NPTv6 provides stateless translation between IPv6 prefixes. It is different from conventional stateful IPv4 NAT and does not provide transport-layer port multiplexing.

# 🗁 Inspecting NAT on Linux

Linux commonly uses Netfilter with nftables for NAT and firewall functionality.

**View IP addresses**

```bash
ip addr show
```

**View routing table**

```bash
ip route show
```

**View nftables rules**

```bash
sudo nft list ruleset
```

**Install connection-tracking tools**

```bash
sudo apt update
sudo apt install conntrack
```

**List tracked connections**

```bash
sudo conntrack -L
```

**Monitor connection events**

```bash
sudo conntrack -E
```

**Capture packets**

```bash
sudo tcpdump -ni any
```

These commands help inspect network interfaces, NAT rules, and active connection state.

# 🗁 Inspecting NAT on MikroTik RouterOS

MikroTik RouterOS provides NAT through its IP firewall.

**View NAT rules**

```routeros
/ip firewall nat print
```

**View detailed NAT rules**

```routeros
/ip firewall nat print detail
```

**View active connections**

```routeros
/ip firewall connection print
```

**View connection-tracking settings**

```routeros
/ip firewall connection tracking print
```

The main NAT chains are:

| Chain | Purpose |
| --- | --- |
| `srcnat` | Source address translation |
| `dstnat` | Destination address translation |

A common source NAT configuration for a dynamic WAN address is:

```routeros
/ip firewall nat add \
    chain=srcnat \
    out-interface-list=WAN \
    action=masquerade
```

This is an illustrative rule. The WAN interface list must exist and contain the correct interfaces.

# 🗁 Best Practices

* Use private IPv4 ranges for internal networks.
* Use PAT when multiple devices need to share limited public IPv4 addresses.
* Use static NAT when predictable one-to-one mappings are required.
* Use dynamic NAT when allocating public addresses from a pool.
* Combine NAT with explicit firewall filtering.
* Avoid exposing unnecessary internal services.
* Document public-to-private address mappings.
* Monitor connection-tracking table utilization.
* Consider CGNAT implications when designing ISP networks.
* Use split DNS when it simplifies internal access to public service names.
* Design IPv6 networks around routing and firewall policies rather than assuming NAT is necessary.

# 🗁 Common Misconceptions

| Misconception | Correct Explanation |
| --- | --- |
| NAT and routing are the same. | Routing selects a forwarding path; NAT modifies packet addressing. |
| NAT is a firewall. | NAT and firewall filtering are separate functions. |
| NAT always changes port numbers. | Port translation is characteristic of PAT/NAPT, not every NAT type. |
| PAT gives every device a separate public IP. | PAT allows multiple connections to share public addresses. |
| NAT is required for internet access. | Properly routed networks can access the internet without NAT. |
| `100.64.0.0/10` is an RFC 1918 range. | It is shared address space reserved by RFC 6598. |
| Destination NAT automatically permits forwarding. | Firewall forwarding policy must also allow the traffic. |
| IPv6 requires NAT for security. | IPv6 networks use routing and firewall policies; conventional NAT is generally unnecessary. |

# 🗁 Summary

NAT modifies IP addresses in packet headers as traffic passes through a router or firewall.

PAT extends NAT by translating transport-layer ports, allowing multiple internal connections to share one or more public IPv4 addresses.

Key concepts:

* NAT translates source or destination IP addresses.
* PAT/NAPT translates addresses and transport ports.
* Static NAT creates fixed address mappings.
* Dynamic NAT allocates addresses from a pool.
* SNAT modifies source addresses.
* DNAT modifies destination addresses.
* Connection tracking maintains information about active flows.
* NAT and routing are separate operations.
* NAT does not replace firewall security policies.
* IPv6 generally relies on routing and firewall policies rather than conventional NAT.

These fundamentals provide the foundation for understanding firewall operation, packet filtering, connection tracking, source NAT, destination NAT, and masquerading.

# 🗁 References

* [RFC 1918 – Address Allocation for Private Internets](https://datatracker.ietf.org/doc/html/rfc1918)
* [RFC 3022 – Traditional IP Network Address Translator](https://datatracker.ietf.org/doc/html/rfc3022)
* [RFC 4787 – NAT Behavioral Requirements for Unicast UDP](https://datatracker.ietf.org/doc/html/rfc4787)
* [RFC 5382 – NAT Behavioral Requirements for TCP](https://datatracker.ietf.org/doc/html/rfc5382)
* [RFC 5508 – NAT Behavioral Requirements for ICMP](https://datatracker.ietf.org/doc/html/rfc5508)
* [RFC 6598 – Shared Address Space](https://datatracker.ietf.org/doc/html/rfc6598)
* [RFC 6296 – IPv6-to-IPv6 Network Prefix Translation](https://datatracker.ietf.org/doc/html/rfc6296)
* [Netfilter Documentation](https://www.netfilter.org/documentation/)
* [nftables Documentation](https://wiki.nftables.org/)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
