# Network Terminologies

Computer networking uses a large number of technical terms to describe devices, communication methods, protocols, network performance, addressing, and infrastructure.

Understanding these terminologies is essential for communicating effectively about network architecture, configuring network devices, troubleshooting connectivity, and designing reliable networks.

This document introduces the most important networking terms used throughout the networking knowledge base.

# 🗁 Overview

Networking terminology can be organized into several categories:

* General networking concepts.
* Network architecture and infrastructure.
* Network addressing.
* Network communication.
* Network performance.
* Switching and routing.
* Network services and protocols.
* Network security.
* Troubleshooting terminology.

# 🗁 General Networking Concepts

These terms describe fundamental networking concepts and how devices communicate.

### 🖆 Network

A network is a collection of interconnected devices that communicate and share resources.

Examples include:

* Home networks.
* Office networks.
* Enterprise networks.
* Data center networks.
* Internet service provider networks.

### 🖆 Node

A node is any device or connection point participating in a network.

Examples:

* Computers.
* Servers.
* Routers.
* Switches.
* Printers.
* Network-enabled IoT devices.

### 🖆 Host

A host is a network-connected device that can send or receive network communication.

Examples include computers, servers, smartphones, and virtual machines.

A host is generally an endpoint rather than an intermediate forwarding device.

### 🖆 Client

A client is a device or application that requests a service or resource from another system.

Example:

A web browser requests a webpage from a web server.

### 🖆 Server

A server is a system or application that provides services or resources to clients.

Examples:

* Web server.
* DNS server.
* DHCP server.
* Database server.
* File server.
* Mail server.

### 🖆 Peer

A peer is a device or application that communicates with another device or application at an equivalent level.

Peer-to-peer (P2P) networking allows participants to exchange resources without requiring every communication to pass through a centralized server.

### 🖆 Endpoint

An endpoint is a device or application at the end of a communication connection.

Examples include:

* Desktop computers.
* Mobile phones.
* Servers.
* Network-connected applications.

### 🖆 Link

A link is a communication connection between two network interfaces or network devices.

A link may be:

* Wired.
* Wireless.
* Point-to-point.
* Logical or virtual.

### 🖆 Network Segment

A network segment is a portion of a network separated by a logical or physical boundary.

The exact meaning depends on context. It may refer to an Ethernet segment, IP subnet, VLAN, or another defined network portion.

### 🖆 Protocol

A protocol is a set of rules that defines how devices communicate.

Examples:

| Protocol | Purpose |
| --- | --- |
| HTTP | Web communication |
| HTTPS | Secure web communication |
| TCP | Reliable transport |
| UDP | Connectionless transport |
| IP | Logical addressing and packet delivery |
| DNS | Name resolution |
| DHCP | Automatic IP configuration |
| SSH | Secure remote access |

### 🖆 Standard

A standard defines technical requirements or specifications that allow products and systems from different vendors to work together.

Examples include:

* IEEE 802.3 for Ethernet.
* IEEE 802.11 for wireless LAN.
* IETF RFC specifications for Internet protocols.

### 🖆 Interoperability

Interoperability is the ability of different devices, systems, or software implementations to communicate and work together.

For example, a Linux computer should be able to communicate with a Windows server using standard networking protocols.

### 🖆 Network Topology

Network topology describes how network devices and connections are arranged.

Common examples:

* Bus.
* Star.
* Ring.
* Mesh.
* Tree.
* Hybrid.

### 🖆 Network Architecture

Network architecture describes the overall design of a network, including its components, communication methods, protocols, security boundaries, and operational structure.

# 🗁 Network Architecture and Infrastructure

These terms describe the physical and logical components that form a network.

### 🖆 LAN — Local Area Network

A LAN connects devices within a relatively limited geographical area.

Examples:

* Home network.
* Office network.
* School computer laboratory.

### 🖆 WLAN — Wireless Local Area Network

A WLAN is a LAN that uses wireless communication, commonly Wi-Fi.

### 🖆 WAN — Wide Area Network

A WAN connects networks across large geographical areas.

Examples:

* Corporate branch connectivity.
* ISP backbone networks.
* International private networks.

### 🖆 MAN — Metropolitan Area Network

A MAN connects networks across a city or metropolitan region.

### 🖆 PAN — Personal Area Network

A PAN connects personal devices within a short range.

Example: A smartphone connected to wireless headphones through Bluetooth.

### 🖆 CAN — Campus Area Network

A CAN connects multiple LANs across a campus or organizational property.

Examples:

* University networks.
* Hospital campuses.
* Large corporate campuses.

### 🖆 Internet

The Internet is a global system of interconnected networks that communicate using the Internet Protocol suite.

### 🖆 Intranet

An intranet is a private network or collection of services accessible within an organization.

### 🖆 Extranet

An extranet provides controlled access to selected internal resources for external users or partner organizations.

### 🖆 Backbone Network

A backbone network is a high-capacity network infrastructure that interconnects smaller networks or major network segments.

### 🖆 Access Network

An access network connects end users or customer devices to a larger network.

Examples:

* Fiber-to-the-home.
* DSL access.
* Cable broadband.
* Cellular access.
* Enterprise access LANs.

### 🖆 Distribution Layer

The distribution layer aggregates access-layer connections and applies policies such as routing, filtering, and segmentation.

### 🖆 Core Layer

The core layer provides high-speed transport between major network segments.

In a traditional three-tier campus architecture, the layers are:

```text
        +----------------+
        |   Core Layer   |
        +----------------+
                |
        +----------------+
        | Distribution  |
        +----------------+
                |
        +----------------+
        | Access Layer   |
        +----------------+
                |
          End Devices
```

### 🖆 Edge Network

An edge network is positioned near end users, devices, or external network connections.

The term may refer to an ISP edge, enterprise network edge, or cloud edge depending on context.

### 🖆 Point of Presence (PoP)

A PoP is a physical location where a network provider offers connectivity or access to its network.

ISPs and content delivery networks commonly operate multiple PoPs.

### 🖆 Data Center

A data center is a facility containing computing, storage, networking, power, and cooling infrastructure.

### 🖆 Network Interface Card (NIC)

A NIC is a hardware or virtual component that provides network connectivity to a device.

It may provide Ethernet, Wi-Fi, or another network interface.

### 🖆 Access Point (AP)

An access point provides wireless connectivity to clients and bridges wireless communication into a wired or other network infrastructure.

### 🖆 Modem

A modem converts signals or adapts communication between a customer device or network and an access technology.

Examples include cable modems and DSL modems.

### 🖆 Gateway

A gateway connects different networks or systems and may perform protocol translation or other forwarding functions.

In ordinary IP networking, the term default gateway commonly refers to the router used to reach destinations outside the local subnet.

# 🗁 Network Addressing Terminologies

These terms describe how network devices and interfaces are identified.

### 🖆 IP Address

An IP address is a logical address assigned to a network interface for communication using the Internet Protocol.

There are two main versions:

* IPv4 — 32 bits.
* IPv6 — 128 bits.

### 🖆 IPv4 Address

An IPv4 address is a 32-bit address commonly represented using dotted-decimal notation.

Example:

```text
192.168.1.10
```

### 🖆 IPv6 Address

An IPv6 address is a 128-bit address commonly represented using hexadecimal notation.

Example:

```text
2001:db8::10
```

### 🖆 MAC Address

A MAC address is a link-layer identifier used by network interfaces for communication on a local network.

Example:

```text
00:1A:2B:3C:4D:5E
```

### 🖆 Subnet

A subnet is a logical division of an IP network.

Subnetting divides a larger address block into smaller network blocks.

### 🖆 Subnet Mask

A subnet mask identifies the network and host portions of an IPv4 address.

Example:

```text
255.255.255.0
```

### 🖆 CIDR — Classless Inter-Domain Routing

CIDR is a method of representing IP network prefixes using slash notation.

Example:

```text
192.168.1.0/24
```

The `/24` indicates that the first 24 bits represent the network prefix.

### 🖆 Network Address

A network address identifies an IPv4 subnet.

Example:

```text
192.168.1.0/24
```

### 🖆 Broadcast Address

An IPv4 broadcast address targets all hosts within a subnet.

Example:

```text
192.168.1.255
```

For the `192.168.1.0/24` subnet.

### 🖆 Host Address

A host address identifies an individual interface within an IP network.

Example:

```text
192.168.1.10
```

### 🖆 Public IP Address

A public IP address is globally routable within the public Internet, subject to routing and address allocation policies.

### 🖆 Private IP Address

A private IPv4 address is intended for use within private networks and is not globally routed across the public Internet.

RFC 1918 defines these private IPv4 ranges:

| Network | CIDR |
| --- | --- |
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

### 🖆 Static IP Address

A static IP address is configured manually or assigned through a reservation or other mechanism intended to keep the address stable.

### 🖆 Dynamic IP Address

A dynamic IP address is assigned automatically, commonly through DHCP.

### 🖆 Loopback Address

A loopback address allows a device to communicate with itself through its local networking stack.

Examples:

```text
IPv4: 127.0.0.1
IPv6: ::1
```

### 🖆 Link-Local Address

A link-local address is used for communication within a local network link and is not normally routed beyond that link.

Examples:

```text
IPv4: 169.254.0.0/16
IPv6: FE80::/10
```

### 🖆 Default Gateway

A default gateway is the next-hop router used when a destination does not match a more specific route.

### 🖆 DNS Name

A DNS name is a human-readable name associated with DNS records.

Example:

```text
example.com
```

### 🖆 FQDN — Fully Qualified Domain Name

An FQDN identifies a complete domain name within the DNS hierarchy.

Example:

```text
www.example.com.
```

The trailing dot represents the DNS root.

### 🖆 Port Number

A port number identifies an application endpoint within a transport-layer protocol.

TCP and UDP use 16-bit port numbers.

Examples:

| Service | Common Port |
| --- | ---: |
| HTTP | 80 |
| HTTPS | 443 |
| SSH | 22 |
| DNS | 53 |
| DHCP Server | UDP 67 |
| DHCP Client | UDP 68 |

### 🖆 Socket

A socket is a communication endpoint used by an application.

For TCP and UDP, a socket is commonly associated with an IP address, transport protocol, and port number.

### 🖆 Public IP vs Private IP

| Feature | Public IP | Private IP |
| --- | --- | --- |
| Global Internet routing | Globally routable in normal operation | Not globally routed |
| Typical use | Internet-facing services | Internal networks |
| Address allocation | Public address allocation policies | RFC 1918 private ranges |
| NAT requirement | Not inherently required for Internet reachability | Commonly used for IPv4 Internet access |

# 🗁 Communication Terminologies

These terms describe how data moves between network devices.

### 🖆 Unicast

One sender communicates with one specific receiver.

### 🖆 Broadcast

One sender communicates with all devices in a broadcast domain.

### 🖆 Multicast

One sender communicates with selected members of a multicast group.

### 🖆 Anycast

Multiple network nodes share the same IP address, and routing directs traffic to one suitable destination.

### 🖆 Point-to-Point

A point-to-point connection directly connects two endpoints or network devices.

### 🖆 Half-Duplex

Half-duplex communication allows data transmission in both directions, but not simultaneously.

### 🖆 Full-Duplex

Full-duplex communication allows simultaneous transmission and reception.

### 🖆 Simplex

Simplex communication allows data transmission in only one direction.

### 🖆 Connection-Oriented Communication

Connection-oriented communication establishes a logical communication relationship before data transfer.

TCP is a common example.

### 🖆 Connectionless Communication

Connectionless communication sends data without establishing a transport-layer connection first.

UDP is a common example.

### 🖆 Encapsulation

Encapsulation is the process of wrapping data with protocol-specific headers and, where applicable, trailers as it moves down the network stack.

### 🖆 Decapsulation

Decapsulation is the process of removing protocol headers and trailers as data moves up the network stack at the receiving device.

### 🖆 Packet

A packet is a network-layer protocol data unit.

An IP packet contains an IP header and payload.

### 🖆 Frame

A frame is a Data Link Layer protocol data unit.

An Ethernet frame contains a link-layer header, payload, and frame check sequence.

### 🖆 Segment

A segment commonly refers to a TCP transport-layer protocol data unit.

### 🖆 Datagram

A datagram is a self-contained unit of data transmitted without requiring a connection.

The term is commonly used for UDP and IP protocol data units.

### 🖆 Payload

Payload is the actual data carried inside a protocol unit, excluding the relevant protocol header and trailer.

### 🖆 Header

A header contains control information required by a protocol.

### 🖆 Trailer

A trailer is information appended to the end of a protocol data unit.

Ethernet uses a Frame Check Sequence (FCS) trailer.

### 🖆 MTU — Maximum Transmission Unit

MTU is the maximum size of the network-layer packet that can be carried in a particular link-layer frame without fragmentation or other special handling.

A common Ethernet IP MTU is:

```text
1500 bytes
```

The exact supported value depends on the link technology and configuration.

### 🖆 MSS — Maximum Segment Size

MSS is the maximum TCP payload size that a host is willing to receive in a TCP segment.

For a common IPv4 Ethernet path:

```text
Ethernet MTU: 1500 bytes
IPv4 header:    20 bytes
TCP header:    20 bytes
MSS:          1460 bytes
```

This example assumes no additional IP or TCP header options.

### 🖆 TTL — Time To Live

TTL is an IPv4 header field used to limit how long a packet can remain in the network.

Routers decrement TTL when forwarding an IPv4 packet.

When it reaches zero, the packet is discarded.

### 🖆 Hop Limit

Hop Limit is the IPv6 equivalent of the IPv4 TTL field.

Routers decrement it during forwarding.

# 🗁 Network Performance Terminologies

These terms describe the speed, efficiency, and quality of network communication.

### 🖆 Bandwidth

Bandwidth describes the maximum data-carrying capacity of a communication link or network resource.

It is commonly measured in:

* Kbps.
* Mbps.
* Gbps.
* Tbps.

### 🖆 Throughput

Throughput is the actual amount of data successfully transferred over a network during a period of time.

Example:

A connection with a 100 Mbps capacity may deliver 85 Mbps of application data.

### 🖆 Goodput

Goodput is the amount of useful application-level data delivered successfully per unit of time, excluding protocol overhead and retransmitted data.

### 🖆 Latency

Latency is the time required for data to travel from one point to another.

It is commonly measured in milliseconds.

### 🖆 Round-Trip Time (RTT)

RTT is the total time taken for a packet to travel from a source to a destination and for a response to return.

The `ping` command commonly reports RTT measurements.

### 🖆 Jitter

Jitter is variation in packet delay over time.

High jitter can negatively affect real-time applications such as:

* Voice over IP.
* Video conferencing.
* Online gaming.
* Live media streaming.

### 🖆 Packet Loss

Packet loss occurs when transmitted packets fail to reach their intended destination.

Possible causes include:

* Congestion.
* Faulty cables.
* Wireless interference.
* Hardware problems.
* Incorrect network configuration.
* Overloaded network devices.

### 🖆 Congestion

Congestion occurs when the demand for network resources exceeds available capacity.

It can cause increased latency, packet loss, and reduced throughput.

### 🖆 QoS — Quality of Service

QoS refers to mechanisms used to manage network traffic according to defined policies and performance requirements.

Examples include prioritization, shaping, policing, and traffic classification.

### 🖆 Traffic Shaping

Traffic shaping controls the rate at which traffic is transmitted, usually by buffering traffic to smooth bursts.

### 🖆 Traffic Policing

Traffic policing measures traffic against a configured rate and applies actions such as dropping or remarking packets that exceed policy limits.

### 🖆 Buffer

A buffer is temporary memory used to hold data while it waits to be processed or transmitted.

### 🖆 Bufferbloat

Bufferbloat is excessive latency caused by large or poorly managed packet queues in network devices.

### 🖆 Availability

Availability describes the proportion of time a network service or system is operational and accessible.

### 🖆 Uptime

Uptime is the period during which a device or service remains operational.

### 🖆 Downtime

Downtime is the period during which a device or service is unavailable.

### 🖆 SLA — Service Level Agreement

An SLA is a formal agreement defining expected service performance and responsibilities.

It may specify:

* Availability.
* Latency.
* Packet loss.
* Support response times.
* Service restoration targets.

### 🖆 Oversubscription

Oversubscription occurs when the total potential demand on shared network resources exceeds their available capacity.

For example, an ISP may provide multiple customers with service plans whose combined maximum advertised speeds exceed the capacity of an upstream link.

# 🗁 Switching and Routing Terminologies

These terms describe how network devices forward traffic.

### 🖆 Switching

Switching is the process of forwarding frames between network interfaces within a Layer 2 network.

### 🖆 Routing

Routing is the process of determining paths and forwarding packets between IP networks.

### 🖆 Switch

A switch is a network device that forwards Ethernet frames based on destination MAC addresses.

### 🖆 Router

A router forwards IP packets between different IP networks using a routing table.

### 🖆 Bridge

A bridge connects Layer 2 network segments and forwards frames based on MAC addresses.

### 🖆 VLAN — Virtual Local Area Network

A VLAN logically separates a Layer 2 network into distinct broadcast domains.

### 🖆 Access Port

An access port normally carries traffic for one assigned VLAN without requiring an IEEE 802.1Q VLAN tag on the connected endpoint link.

### 🖆 Trunk Port

A trunk port carries traffic for multiple VLANs, commonly using IEEE 802.1Q tagging.

### 🖆 MAC Address Table

A MAC address table maps learned MAC addresses to switch ports and, in VLAN-aware environments, VLAN identifiers.

### 🖆 ARP — Address Resolution Protocol

ARP resolves an IPv4 address to a link-layer address on a local network.

### 🖆 NDP — Neighbor Discovery Protocol

Neighbor Discovery is an IPv6 protocol suite used for functions including neighbor address resolution and router discovery.

### 🖆 Routing Table

A routing table contains information used to determine where IP packets should be forwarded.

### 🖆 Next Hop

The next hop is the next router or forwarding destination to which a packet is sent along its route.

### 🖆 Static Route

A static route is manually configured by an administrator.

### 🖆 Default Route

A default route is used when no more specific route matches a destination.

IPv4 default route:

```text
0.0.0.0/0
```

IPv6 default route:

```text
::/0
```

### 🖆 Dynamic Routing

Dynamic routing uses routing protocols to exchange network reachability information and calculate routes.

Examples:

* OSPF.
* BGP.
* RIP.
* IS-IS.

### 🖆 Routing Metric

A routing metric is a value used by a routing protocol or routing process to evaluate route preference or path cost.

### 🖆 Administrative Distance

Administrative distance is a vendor-specific routing preference value used by some routing platforms to compare routes learned from different sources.

Lower values generally indicate a more preferred route on platforms using this convention.

### 🖆 Convergence

Convergence is the process through which routers reach a consistent understanding of network reachability after a topology or routing change.

### 🖆 STP — Spanning Tree Protocol

STP prevents Layer 2 switching loops by creating a loop-free logical forwarding topology.

### 🖆 Link Aggregation

Link aggregation combines multiple physical links into one logical connection to provide increased aggregate capacity and redundancy.

### 🖆 LACP — Link Aggregation Control Protocol

LACP dynamically negotiates and manages link aggregation between compatible devices.

# 🗁 Network Services and Protocol Terminologies

These terms describe common services used by networked systems.

### 🖆 DNS — Domain Name System

DNS translates domain names into DNS records, including IP address records.

Example:

```text
example.com → 93.184.216.34
```

The returned address is illustrative; DNS answers can vary.

### 🖆 DHCP — Dynamic Host Configuration Protocol

DHCP automatically provides network configuration to clients.

Typical information includes:

* IP address.
* Subnet mask or prefix information.
* Default gateway.
* DNS server addresses.
* Lease duration.

### 🖆 HTTP — Hypertext Transfer Protocol

HTTP is an application-layer protocol used for web communication.

### 🖆 HTTPS — HTTP Secure

HTTPS is HTTP communication protected using TLS.

### 🖆 TLS — Transport Layer Security

TLS is a cryptographic protocol that provides security properties such as confidentiality, integrity, and authentication.

### 🖆 FTP — File Transfer Protocol

FTP is a protocol used for file transfer.

Traditional FTP does not encrypt its credentials or data by default.

### 🖆 SFTP — SSH File Transfer Protocol

SFTP is a file-transfer protocol that operates over SSH.

It is different from FTP and FTPS.

### 🖆 SSH — Secure Shell

SSH provides secure remote access and encrypted communication between systems.

### 🖆 Telnet

Telnet is a remote terminal protocol that transmits communication without encryption by default.

### 🖆 SMTP — Simple Mail Transfer Protocol

SMTP is used to send and relay email messages.

### 🖆 POP3 — Post Office Protocol Version 3

POP3 is an email retrieval protocol.

### 🖆 IMAP — Internet Message Access Protocol

IMAP allows clients to access and synchronize email stored on a mail server.

### 🖆 NTP — Network Time Protocol

NTP synchronizes system clocks across networks.

### 🖆 SNMP — Simple Network Management Protocol

SNMP is used to monitor and manage network devices.

### 🖆 ICMP — Internet Control Message Protocol

ICMP provides control and error-reporting messages for IP networking.

The `ping` command commonly uses ICMP Echo Request and Echo Reply messages.

### 🖆 Syslog

Syslog is a standard mechanism for transmitting and recording log messages.

### 🖆 RADIUS — Remote Authentication Dial-In User Service

RADIUS is an AAA protocol commonly used for centralized network access authentication, authorization, and accounting.

### 🖆 AAA — Authentication, Authorization, and Accounting

AAA describes three security functions:

* Authentication: Verifying identity.
* Authorization: Determining permitted actions.
* Accounting: Recording usage and activity.

# 🗁 Network Security Terminologies

These terms describe network protection, access control, and security architecture.

### 🖆 Firewall

A firewall controls network traffic according to configured security policies.

It may operate at different layers and use different filtering methods.

### 🖆 ACL — Access Control List

An ACL is a set of rules that permits or denies traffic or access to resources.

### 🖆 NAT — Network Address Translation

NAT modifies IP address information in packet headers as traffic passes through a network device.

### 🖆 PAT — Port Address Translation

PAT allows multiple internal connections to share one external IP address by translating transport-layer port information.

### 🖆 VPN — Virtual Private Network

A VPN creates a protected logical communication path over an underlying network.

Common technologies include:

* IPsec.
* WireGuard.
* OpenVPN.

### 🖆 Encryption

Encryption transforms readable information into an encoded form using cryptographic algorithms and keys.

### 🖆 Authentication

Authentication verifies the identity of a user, device, or service.

### 🖆 Authorization

Authorization determines what an authenticated entity is allowed to access or perform.

### 🖆 Access Control

Access control defines and enforces rules governing who or what can access network resources.

### 🖆 IDS — Intrusion Detection System

An IDS monitors activity to identify suspicious or potentially malicious behavior.

### 🖆 IPS — Intrusion Prevention System

An IPS detects suspicious traffic and can take preventive action, such as blocking or dropping traffic.

### 🖆 DDoS — Distributed Denial of Service

A DDoS attack uses multiple sources to overwhelm a service or network resource, reducing availability for legitimate users.

### 🖆 Zero Trust

Zero Trust is a security approach that avoids automatically trusting users or devices based solely on their network location.

Access decisions are based on identity, device state, context, and policy.

### 🖆 DMZ — Demilitarized Zone

A DMZ is a network segment used to isolate externally accessible services from more sensitive internal networks.

### 🖆 Network Segmentation

Network segmentation divides a network into separate security or operational zones.

VLANs, subnets, firewalls, and access-control policies can be used to implement segmentation.

### 🖆 MAC Filtering

MAC filtering permits or denies network access based on configured MAC addresses.

It should not be treated as strong authentication because MAC addresses can be spoofed.

# 🗁 Network Troubleshooting Terminologies

These terms are commonly used when diagnosing network connectivity problems.

### 🖆 Connectivity

Connectivity describes whether two devices or systems can communicate over a network.

### 🖆 Reachability

Reachability describes whether a destination can be contacted through the network.

### 🖆 Packet Capture

Packet capture records network packets for analysis.

Tools include:

* Wireshark.
* tcpdump.
* tshark.

### 🖆 Packet Sniffing

Packet sniffing is the process of capturing and inspecting network traffic.

### 🖆 Ping

Ping tests IP connectivity using ICMP Echo Request and Echo Reply messages.

Example:

```bash
ping -c 4 8.8.8.8
```

### 🖆 Traceroute

Traceroute identifies intermediate Layer 3 hops along a path toward a destination.

On Linux:

```bash
traceroute example.com
```

An alternative commonly available on Linux is:

```bash
tracepath example.com
```

### 🖆 MTR — My Traceroute

MTR combines continuous probing with traceroute-style path information to help diagnose latency and packet loss along a route.

Example:

```bash
mtr example.com
```

### 🖆 DNS Resolution

DNS resolution is the process of obtaining DNS records for a requested domain name.

Example:

```bash
dig example.com
```

### 🖆 Port Scanning

Port scanning checks which transport-layer ports are reachable or appear to be listening on a target system.

Example:

```bash
nmap -sT 192.168.1.10
```

### 🖆 Interface Status

Interface status indicates whether a network interface is administratively enabled and whether its physical or logical link is operational.

Linux command:

```bash
ip link show
```

### 🖆 Link Down

A link-down condition means the interface does not currently have an operational link.

Possible causes include:

* Disconnected cable.
* Failed transceiver.
* Disabled switch port.
* Wireless disconnection.
* Physical-layer problems.

### 🖆 CRC Error

A CRC error indicates that a frame failed a cyclic redundancy check or related frame integrity validation.

CRC errors may indicate physical-layer problems, interference, damaged cables, or faulty hardware.

### 🖆 Duplex Mismatch

A duplex mismatch occurs when connected interfaces use incompatible duplex settings.

It can cause poor performance, collisions on legacy half-duplex Ethernet links, and transmission errors.

### 🖆 Interface Flapping

Interface flapping occurs when a network interface repeatedly changes between operational and non-operational states.

### 🖆 MAC Address Flapping

MAC address flapping occurs when a switch repeatedly learns the same MAC address on different ports.

Possible causes include:

* Layer 2 loops.
* Duplicate MAC addresses.
* Incorrect link aggregation.
* Network topology problems.

### 🖆 Asymmetric Routing

Asymmetric routing occurs when traffic travels through different paths in opposite directions.

It can be normal in some network designs, but it may cause problems with stateful firewalls and traffic inspection systems.

### 🖆 Black Hole Routing

A black hole route intentionally or unintentionally discards traffic destined for a particular prefix or destination.

### 🖆 Routing Loop

A routing loop occurs when packets circulate between routers instead of reaching their destination.

TTL or Hop Limit prevents packets from circulating indefinitely.

# 🗁 Common Networking Abbreviations

The following table provides a quick reference to frequently used networking abbreviations.

| Abbreviation | Full Form |
| --- | --- |
| AAA | Authentication, Authorization, and Accounting |
| ACL | Access Control List |
| AP | Access Point |
| APIPA | Automatic Private IP Addressing |
| ARP | Address Resolution Protocol |
| BGP | Border Gateway Protocol |
| CAN | Campus Area Network |
| CIDR | Classless Inter-Domain Routing |
| CRC | Cyclic Redundancy Check |
| DHCP | Dynamic Host Configuration Protocol |
| DNS | Domain Name System |
| DDoS | Distributed Denial of Service |
| DMZ | Demilitarized Zone |
| FQDN | Fully Qualified Domain Name |
| FTP | File Transfer Protocol |
| HTTP | Hypertext Transfer Protocol |
| HTTPS | Hypertext Transfer Protocol Secure |
| ICMP | Internet Control Message Protocol |
| IDS | Intrusion Detection System |
| IGMP | Internet Group Management Protocol |
| IP | Internet Protocol |
| IPS | Intrusion Prevention System |
| IPsec | Internet Protocol Security |
| ISP | Internet Service Provider |
| LAN | Local Area Network |
| LACP | Link Aggregation Control Protocol |
| MAC | Media Access Control |
| MAN | Metropolitan Area Network |
| MLD | Multicast Listener Discovery |
| MSS | Maximum Segment Size |
| MTU | Maximum Transmission Unit |
| NAT | Network Address Translation |
| NDP | Neighbor Discovery Protocol |
| NIC | Network Interface Card |
| NTP | Network Time Protocol |
| OSPF | Open Shortest Path First |
| PAT | Port Address Translation |
| PoP | Point of Presence |
| QoS | Quality of Service |
| RADIUS | Remote Authentication Dial-In User Service |
| RTT | Round-Trip Time |
| SFTP | SSH File Transfer Protocol |
| SLA | Service Level Agreement |
| SMTP | Simple Mail Transfer Protocol |
| SNMP | Simple Network Management Protocol |
| SSH | Secure Shell |
| STP | Spanning Tree Protocol |
| TCP | Transmission Control Protocol |
| TLS | Transport Layer Security |
| TTL | Time To Live |
| UDP | User Datagram Protocol |
| VLAN | Virtual Local Area Network |
| VPN | Virtual Private Network |
| WAN | Wide Area Network |
| WLAN | Wireless Local Area Network |

# 🗁 Practical Examples

The following examples demonstrate how several networking terms work together.

### 🖆 Example: Accessing a Website

A computer accesses a website using the following components:

```text
User
 |
 v
Web Browser
 |
 | DNS Query
 v
DNS Resolver
 |
 | Returns an IP address
 v
Web Browser
 |
 | HTTPS Request
 v
Default Gateway
 |
 v
Internet
 |
 v
Web Server
 |
 | HTTPS Response
 v
User
```

Relevant terminology:

* Client.
* Server.
* DNS.
* IP address.
* Default gateway.
* Router.
* TCP.
* TLS.
* HTTPS.
* Packet.
* Latency.
* Throughput.

### 🖆 Example: Connecting a Computer to a LAN

```text
             Internet
                |
             Router
                |
             Switch
          /     |     \
         /      |      \
       PC A    PC B   Printer
```

Relevant terminology:

* LAN.
* Host.
* NIC.
* MAC address.
* IP address.
* Ethernet.
* Switch.
* Default gateway.
* Broadcast domain.
* DHCP.

### 🖆 Example: Enterprise Network Segmentation

```text
                 Internet
                    |
                 Firewall
                    |
                Core Switch
                    |
        +-----------+-----------+
        |           |           |
      VLAN 10     VLAN 20     VLAN 30
       Users      Servers      Guest
        |           |           |
      PCs        Servers      Wi-Fi
```

Relevant terminology:

* VLAN.
* Subnet.
* Firewall.
* ACL.
* Network segmentation.
* Routing.
* Access control.
* Security policy.

# 🗁 Best Practices

* Learn the full forms and meanings of common networking abbreviations.
* Understand the difference between Layer 2 and Layer 3 terminology.
* Distinguish bandwidth, throughput, and goodput.
* Distinguish latency, RTT, and jitter.
* Understand the difference between a switch, router, bridge, and gateway.
* Learn the relationship between MAC addresses, IP addresses, and port numbers.
* Use correct protocol terminology when troubleshooting.
* Understand the difference between a subnet and a VLAN.
* Use packet captures to connect theoretical concepts with real network traffic.
* Refer to official RFC and IEEE documentation when studying protocol specifications.

# 🗁 Interview Questions

### 🖆 What is the difference between a host and a node?

A node is any participating device or connection point in a network. A host is an endpoint capable of sending or receiving network communication.

### 🖆 What is the difference between a client and a server?

A client requests services or resources, while a server provides them.

### 🖆 What is the difference between LAN and WAN?

A LAN connects devices within a limited geographical area, while a WAN interconnects networks over larger geographical distances.

### 🖆 What is the difference between bandwidth and throughput?

Bandwidth describes the available or maximum data-carrying capacity, while throughput measures the actual data transfer rate achieved.

### 🖆 What is the difference between throughput and goodput?

Throughput measures successfully transferred data at a particular measurement point. Goodput measures useful application data delivered, excluding protocol overhead and retransmitted data.

### 🖆 What is the difference between latency and RTT?

Latency measures delay in one direction or between defined measurement points. RTT measures the total time for a request to travel to a destination and for a response to return.

### 🖆 What is jitter?

Jitter is variation in packet delay over time.

### 🖆 What is MTU?

MTU is the maximum network-layer packet size that a link can carry without fragmentation or another special handling mechanism.

### 🖆 What is MSS?

MSS is the maximum TCP payload size a host is willing to receive in a TCP segment.

### 🖆 What is the difference between a switch and a router?

A Layer 2 switch forwards frames using MAC addresses, while a router forwards IP packets between networks using routing information.

### 🖆 What is the difference between a subnet and a VLAN?

A subnet is a logical IP network, while a VLAN is a logical Layer 2 broadcast domain. They are often mapped one-to-one in network designs, but they are not the same thing.

### 🖆 What is a default route?

A default route is used when no more specific routing-table entry matches the destination.

### 🖆 What is a next hop?

A next hop is the next router or forwarding destination to which a packet is sent.

### 🖆 What is network convergence?

Network convergence is the process through which routing devices reach a consistent view of network reachability after a topology change.

### 🖆 What is QoS?

Quality of Service refers to mechanisms used to manage and prioritize network traffic according to defined policies.

### 🖆 What is an SLA?

A Service Level Agreement defines expected service performance and responsibilities between a provider and customer.

### 🖆 What is asymmetric routing?

Asymmetric routing occurs when traffic in opposite directions follows different network paths.

### 🖆 What is a DMZ?

A DMZ is a separate network segment used to host externally accessible services while isolating them from sensitive internal networks.

### 🖆 What is a PoP?

A Point of Presence is a physical location where a network provider provides connectivity or access to its network.

# 🗁 References

* [RFC Editor](https://www.rfc-editor.org/)
* [IANA Protocol Registries](https://www.iana.org/protocols)
* [IEEE Standards Association](https://standards.ieee.org/)
* [IETF - Internet Engineering Task Force](https://www.ietf.org/)
* [Cisco Networking Documentation](https://www.cisco.com/c/en/us/support/index.html)
* [Linux Networking Documentation](https://docs.kernel.org/networking/)
* [Linux iproute2 Documentation](https://man7.org/linux/man-pages/man8/ip.8.html)
