# Network Engineer Interview Questions

Network engineering interviews evaluate more than theoretical knowledge. Interviewers want to understand whether you can design, configure, secure, monitor, and troubleshoot real networks.

This document provides technical interview questions with concise answers and practical scenarios covering:

* Networking fundamentals
* TCP/IP and OSI models
* IPv4, IPv6, and subnetting
* Switching, VLANs, and routing
* DNS, DHCP, NAT, and firewalls
* Wireless networking and VPNs
* MikroTik RouterOS
* Network security and monitoring
* Real-world troubleshooting

Use these questions to develop both conceptual understanding and practical problem-solving skills.

# 🗁 Networking Fundamentals

### 🖆 What is a computer network?

A computer network is a collection of interconnected devices that communicate and share resources using communication protocols.

Examples include:

* Local Area Network (LAN)
* Wide Area Network (WAN)
* Wireless Local Area Network (WLAN)
* Metropolitan Area Network (MAN)

### 🖆 What is the difference between LAN and WAN?

| LAN | WAN |
| --- | --- |
| Covers a limited geographical area | Covers large geographical areas |
| Usually managed by one organization | Often depends on multiple service providers |
| Typically offers high local bandwidth | Performance depends on WAN technology |
| Example: Office network | Example: ISP backbone |

### 🖆 What is the difference between a switch and a router?

A switch primarily forwards Ethernet frames within a Layer 2 network using MAC addresses.

A router forwards IP packets between different IP networks using routing information.

### 🖆 What is a MAC address?

A MAC address is a link-layer identifier associated with a network interface.

A typical Ethernet MAC address is 48 bits long and written in hexadecimal notation.

Example:

```text
00:1A:2B:3C:4D:5E
```

### 🖆 What is the difference between unicast, broadcast, and multicast?

* **Unicast:** One sender communicates with one receiver.
* **Broadcast:** One sender communicates with all devices in a broadcast domain.
* **Multicast:** One sender communicates with a selected group of receivers.

### 🖆 What is a default gateway?

A default gateway is the router or Layer 3 device that forwards traffic destined for networks outside the local subnet.

### 🖆 What is the difference between bandwidth and throughput?

* **Bandwidth:** The theoretical or configured data transmission capacity.
* **Throughput:** The actual amount of successfully delivered data per unit of time.

### 🖆 What are latency, jitter, and packet loss?

* **Latency:** Time required for data to travel from source to destination.
* **Jitter:** Variation in packet delay.
* **Packet loss:** Packets that fail to reach their destination.

These metrics are particularly important for VoIP, video conferencing, gaming, and real-time applications.

# 🗁 OSI and TCP/IP Models

### 🖆 What is the OSI model?

The OSI model is a seven-layer conceptual framework used to understand network communication.

| Layer | Name | Main responsibility |
| --- | --- | --- |
| 7 | Application | Network services for applications |
| 6 | Presentation | Data representation and encryption |
| 5 | Session | Session management |
| 4 | Transport | End-to-end transport |
| 3 | Network | Logical addressing and routing |
| 2 | Data Link | Framing and MAC addressing |
| 1 | Physical | Transmission of raw bits |

### 🖆 What is the TCP/IP model?

The TCP/IP model describes the architecture used by the Internet.

Its commonly used four-layer representation is:

* Application
* Transport
* Internet
* Network Access

### 🖆 What is encapsulation?

Encapsulation is the process of adding protocol-specific headers and, where applicable, trailers as data moves down the networking stack.

For example:

```text
Application Data
       |
       v
TCP Segment
       |
       v
IP Packet
       |
       v
Ethernet Frame
       |
       v
Physical Bits
```

### 🖆 What is the difference between a packet, frame, and segment?

* **Segment:** Transport-layer data unit, commonly associated with TCP.
* **Packet:** Network-layer data unit.
* **Frame:** Data-link-layer data unit.

### 🖆 At which OSI layer does a switch operate?

A traditional Ethernet switch primarily operates at Layer 2.

A multilayer switch can also perform Layer 3 routing.

### 🖆 At which OSI layer does a router operate?

A router primarily operates at Layer 3 because it forwards packets based on IP addresses.

# 🗁 IP Addressing and Subnetting

### 🖆 What is an IP address?

An IP address is a logical address used to identify an interface and support packet delivery across IP networks.

### 🖆 What is the difference between IPv4 and IPv6?

| IPv4 | IPv6 |
| --- | --- |
| 32-bit address | 128-bit address |
| Dotted-decimal notation | Hexadecimal colon notation |
| Example: `192.168.1.10` | Example: `2001:db8::10` |
| Supports broadcast | Does not use broadcast |
| Commonly uses NAT | Designed for a much larger address space |

### 🖆 What is a subnet mask?

A subnet mask identifies which bits of an IPv4 address represent the network portion and which represent the host portion.

Example:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
CIDR: /24
Network: 192.168.1.0
```

### 🖆 What is CIDR?

Classless Inter-Domain Routing (CIDR) represents an IP network using an address followed by a prefix length.

Example:

```text
192.168.1.0/24
```

The `/24` indicates that the first 24 bits represent the network prefix.

### 🖆 How many usable host addresses are available in a /24 IPv4 subnet?

A conventional IPv4 `/24` subnet contains 256 addresses.

For an ordinary subnet with a network address and broadcast address:

```text
Usable Hosts = 2^(32 - 24) - 2
             = 256 - 2
             = 254
```

### 🖆 What is the difference between public and private IP addresses?

* **Public IP:** Globally routable on the public Internet.
* **Private IP:** Reserved for private network communication and not globally routed on the public Internet.

IPv4 private ranges:

| Range | CIDR |
| --- | --- |
| `10.0.0.0 - 10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0 - 172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0 - 192.168.255.255` | `192.168.0.0/16` |

### 🖆 What is APIPA?

Automatic Private IP Addressing allows a device to assign itself an IPv4 link-local address when it cannot obtain an address through DHCP.

The IPv4 APIPA range is:

```text
169.254.0.0/16
```

### 🖆 What is the difference between a static and dynamic IP address?

* **Static IP:** Manually configured or administratively reserved.
* **Dynamic IP:** Assigned automatically, commonly through DHCP.

### 🖆 What is a loopback address?

A loopback address allows a device to communicate with its own networking stack.

Common examples:

```text
IPv4: 127.0.0.1
IPv6: ::1
```

### 🖆 What is subnetting?

Subnetting divides a larger IP network into smaller logical networks.

Benefits include:

* Efficient address allocation
* Network segmentation
* Reduced broadcast domains
* Easier network administration
* Improved security boundaries

# 🗁 Switching and VLANs

### 🖆 What is a VLAN?

A Virtual Local Area Network logically divides a switched network into separate Layer 2 broadcast domains.

### 🖆 Why do we use VLANs?

VLANs provide:

* Network segmentation
* Broadcast-domain separation
* Improved administrative organization
* Security boundaries when combined with appropriate controls
* Separation of departments and services

### 🖆 What is the difference between access and trunk ports?

| Access Port | Trunk Port |
| --- | --- |
| Normally carries one assigned VLAN | Carries multiple VLANs |
| Usually sends and receives untagged frames | Commonly uses IEEE 802.1Q tags |
| Connects endpoint devices | Connects switches, routers, and VLAN-aware devices |

### 🖆 What is IEEE 802.1Q?

IEEE 802.1Q is the standard used to identify VLAN membership by inserting a VLAN tag into Ethernet frames.

### 🖆 What is a native VLAN?

A native VLAN is the VLAN associated with untagged traffic on an 802.1Q trunk.

Both ends of a trunk should have consistent native VLAN configuration.

### 🖆 What is inter-VLAN routing?

Inter-VLAN routing enables communication between devices belonging to different VLANs through a Layer 3 routing function.

Common approaches include:

* Router-on-a-stick
* Layer 3 switch using switched virtual interfaces
* Router with separate VLAN interfaces

### 🖆 What is STP?

Spanning Tree Protocol prevents Layer 2 switching loops by logically blocking redundant paths while maintaining a loop-free topology.

### 🖆 What happens when a switching loop occurs?

A Layer 2 loop can cause:

* Broadcast storms
* MAC address table instability
* Duplicate frames
* High CPU utilization
* Network outages

# 🗁 Routing

### 🖆 What is routing?

Routing is the process of selecting paths and forwarding IP packets between different networks.

### 🖆 What is a routing table?

A routing table contains network destinations and associated forwarding information.

Typical information includes:

* Destination prefix
* Next-hop address
* Outgoing interface
* Route source
* Administrative preference or distance
* Metric

### 🖆 What is the difference between static and dynamic routing?

| Static Routing | Dynamic Routing |
| --- | --- |
| Manually configured | Learned through routing protocols |
| Simple for small networks | Useful for larger or changing networks |
| Does not automatically adapt to topology changes | Can adapt to routing changes |
| Requires manual maintenance | Requires protocol configuration and monitoring |

### 🖆 What is a default route?

A default route is used when no more specific matching route exists.

IPv4:

```text
0.0.0.0/0
```

IPv6:

```text
::/0
```

### 🖆 What is longest-prefix match?

Longest-prefix match means a router selects the matching route with the most specific network prefix.

For example, if a destination matches both `/16` and `/24`, the `/24` route is selected.

### 🖆 What is OSPF?

Open Shortest Path First (OSPF) is a link-state interior gateway routing protocol.

It uses link-state information and the Shortest Path First algorithm to calculate routes.

### 🖆 What is BGP?

Border Gateway Protocol is a path-vector routing protocol used to exchange routing information between autonomous systems.

It is fundamental to Internet inter-domain routing.

### 🖆 What is the difference between OSPF and BGP?

| OSPF | BGP |
| --- | --- |
| Interior Gateway Protocol | Exterior Gateway Protocol |
| Commonly used within an autonomous system | Used between autonomous systems and also within an AS |
| Link-state protocol | Path-vector protocol |
| Uses cost as its primary path metric | Uses path attributes and routing policy |
| Designed for internal route calculation | Designed for scalable inter-domain routing |

### 🖆 What is ECMP?

Equal-Cost Multi-Path routing allows multiple paths with equal routing cost to be used for forwarding traffic.

### 🖆 What is route redistribution?

Route redistribution imports routes from one routing protocol or routing source into another.

It requires careful filtering and policy design to avoid routing loops and suboptimal paths.

# 🗁 Transport and Application Protocols

### 🖆 What is TCP?

Transmission Control Protocol is a connection-oriented transport protocol that provides reliable, ordered byte-stream delivery.

Its mechanisms include:

* Sequence numbers
* Acknowledgments
* Retransmissions
* Flow control
* Congestion control

### 🖆 What is UDP?

User Datagram Protocol is a connectionless transport protocol that provides datagram delivery without TCP-style reliability or ordering guarantees.

### 🖆 What is the difference between TCP and UDP?

| TCP | UDP |
| --- | --- |
| Connection-oriented | Connectionless |
| Reliable ordered byte stream | Datagram-based delivery |
| Uses acknowledgments and retransmissions | No built-in retransmission mechanism |
| Has flow and congestion control | No TCP-style built-in flow control |
| Commonly used by HTTPS and SSH | Commonly used by DNS, VoIP, and streaming applications |

### 🖆 What is the TCP three-way handshake?

TCP establishes a connection using three messages:

```text
Client                    Server
  |                          |
  |-------- SYN ------------>|
  |                          |
  |<----- SYN + ACK ----------|
  |                          |
  |-------- ACK ------------>|
  |                          |
  |     Connection Ready     |
```

### 🖆 What is a port number?

A port number identifies a communication endpoint associated with a transport-layer protocol.

Examples:

| Service | Port | Transport |
| --- | ---: | --- |
| FTP control | 21 | TCP |
| SSH | 22 | TCP |
| DNS | 53 | UDP/TCP |
| DHCP server | 67 | UDP |
| DHCP client | 68 | UDP |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP; QUIC commonly uses UDP 443 |
| SMTP submission | 587 | TCP |
| RDP | 3389 | TCP/UDP |

### 🖆 What is a socket?

A socket is a communication endpoint. A network connection is commonly identified by its protocol and endpoint information, including source and destination IP addresses and port numbers.

### 🖆 What is TLS?

Transport Layer Security is a cryptographic protocol that provides confidentiality, integrity, and authentication for network communications.

HTTPS uses HTTP over TLS, or HTTP over QUIC with TLS 1.3.

# 🗁 DNS and DHCP

### 🖆 What is DNS?

Domain Name System translates domain names into information such as IP addresses and other resource records.

Example:

```text
example.com → 93.184.216.34
```

### 🖆 What are common DNS record types?

| Record | Purpose |
| --- | --- |
| A | Maps a hostname to an IPv4 address |
| AAAA | Maps a hostname to an IPv6 address |
| CNAME | Creates an alias |
| MX | Identifies mail exchangers |
| NS | Identifies authoritative name servers |
| TXT | Stores text-based information |
| PTR | Supports reverse DNS lookup |
| SOA | Contains zone authority information |

### 🖆 What is the difference between recursive and authoritative DNS servers?

A recursive resolver obtains DNS answers on behalf of clients, using its cache and querying other DNS servers when necessary.

An authoritative DNS server provides the official DNS records for the zones it serves.

### 🖆 What is DNS caching?

DNS caching temporarily stores DNS responses to reduce lookup time and repeated queries.

The Time to Live (TTL) determines how long a cached record may be retained.

### 🖆 What is DHCP?

Dynamic Host Configuration Protocol automatically provides network configuration to clients.

It commonly assigns:

* IP address
* Subnet mask or prefix information
* Default gateway
* DNS server addresses
* Lease duration

### 🖆 Explain the DHCP DORA process

DORA represents the four main messages used in a typical DHCPv4 address allocation process.

```text
Client                    DHCP Server
  |                            |
  |-------- DISCOVER --------->|
  |                            |
  |<--------- OFFER -----------|
  |                            |
  |-------- REQUEST ---------->|
  |                            |
  |<---------- ACK ------------|
  |                            |
  |       IP Assigned          |
```

### 🖆 What happens when a DHCP server is unavailable?

A client may fail to obtain its expected DHCP configuration.

Depending on the operating system and configuration, it may use an existing lease, retain a previously configured address, or assign an IPv4 link-local address.

### 🖆 What is a DHCP reservation?

A DHCP reservation associates a specific client identifier, commonly a MAC address, with a predetermined IP address.

# 🗁 NAT and Firewall

### 🖆 What is NAT?

Network Address Translation modifies IP address information as packets pass through a router or firewall.

It is commonly used to connect private IPv4 networks to public networks.

### 🖆 What is PAT?

Port Address Translation allows multiple internal connections to share an external IP address by translating transport-layer port information.

It is commonly called NAT overload.

### 🖆 What is the difference between source NAT and destination NAT?

* **Source NAT (SNAT):** Modifies the source address.
* **Destination NAT (DNAT):** Modifies the destination address.

### 🖆 What is port forwarding?

Port forwarding is a form of destination NAT that directs incoming connections to a specific internal address and port.

### 🖆 What is a firewall?

A firewall enforces network traffic policies by allowing, rejecting, or dropping traffic according to configured rules.

### 🖆 What is the difference between stateful and stateless firewalls?

| Stateful Firewall | Stateless Firewall |
| --- | --- |
| Tracks connection state | Evaluates individual packets |
| Can recognize established connections | Does not maintain connection state |
| Uses connection-tracking information | Uses configured packet-matching rules |
| Commonly simplifies return-traffic handling | Often requires explicit directional rules |

### 🖆 What is connection tracking?

Connection tracking maintains information about network flows so a firewall can identify connection states such as new, established, related, and invalid.

### 🖆 What is a DMZ?

A Demilitarized Zone is a separate network segment used to host services that need controlled access from less-trusted networks, such as the Internet.

# 🗁 Wireless Networking

### 🖆 What is Wi-Fi?

Wi-Fi is a family of wireless networking technologies based on IEEE 802.11 standards.

### 🖆 What is the difference between an access point and a wireless router?

An access point provides wireless connectivity to a network.

A wireless router typically combines routing, firewalling, DHCP, NAT, and wireless access point functionality.

### 🖆 What is an SSID?

A Service Set Identifier is the name used to identify a wireless network.

### 🖆 What is the difference between 2.4 GHz, 5 GHz, and 6 GHz Wi-Fi?

| Band | Characteristics |
| --- | --- |
| 2.4 GHz | Longer range in many environments, more interference, fewer non-overlapping channels |
| 5 GHz | More channel capacity and generally higher throughput, with shorter range through obstacles |
| 6 GHz | Additional spectrum for supported Wi-Fi generations and devices, with regional regulatory restrictions |

### 🖆 What is the difference between WPA2 and WPA3?

WPA2 and WPA3 are Wi-Fi security certification generations.

WPA3 introduces stronger security mechanisms, including SAE for personal networks, while WPA2 commonly uses PSK-based authentication in personal deployments.

### 🖆 What causes poor Wi-Fi performance?

Common causes include:

* Radio interference
* Weak signal strength
* Channel congestion
* Excessive client density
* Poor access point placement
* Incorrect channel width
* Incompatible client capabilities
* Wired uplink bottlenecks

# 🗁 VPN and Tunneling

### 🖆 What is a VPN?

A Virtual Private Network creates a protected logical connection over an underlying network, commonly the public Internet.

### 🖆 What is the difference between site-to-site and remote-access VPN?

* **Site-to-site VPN:** Connects separate networks or locations.
* **Remote-access VPN:** Connects an individual user's device to a private network.

### 🖆 What is IPsec?

IPsec is a collection of protocols and mechanisms that protect IP communications using authentication, integrity protection, and encryption.

### 🖆 What is WireGuard?

WireGuard is a modern VPN protocol designed around a relatively small codebase and modern cryptographic primitives.

### 🖆 What is OpenVPN?

OpenVPN is a VPN solution that uses TLS-based security and supports flexible remote-access and site-to-site deployments.

### 🖆 What is GRE?

Generic Routing Encapsulation tunnels one network protocol inside another. GRE itself does not provide encryption.

### 🖆 What is the difference between GRE and IPsec?

GRE provides encapsulation and supports carrying different network protocols.

IPsec provides security services such as encryption, integrity, and authentication.

They can be combined when both tunneling flexibility and cryptographic protection are required.

# 🗁 Network Security

### 🖆 What is the principle of least privilege?

The principle of least privilege means giving users, devices, and services only the permissions necessary to perform their tasks.

### 🖆 What is AAA?

AAA stands for:

* **Authentication:** Verifies identity.
* **Authorization:** Determines permitted actions.
* **Accounting:** Records activity and resource usage.

### 🖆 What is RADIUS?

Remote Authentication Dial-In User Service is a protocol commonly used for centralized authentication, authorization, and accounting in network access environments.

### 🖆 What is 802.1X?

IEEE 802.1X provides port-based network access control. It commonly uses an authenticator, a supplicant, and an authentication server such as RADIUS.

### 🖆 What is a DDoS attack?

A Distributed Denial of Service attack uses multiple sources to overwhelm a target's network, service, or infrastructure and disrupt availability.

### 🖆 How can you secure network devices?

Recommended controls include:

* Use SSH instead of Telnet.
* Disable unused services and interfaces.
* Use strong authentication and MFA where supported.
* Apply role-based access control.
* Restrict management access to trusted networks.
* Keep firmware and software updated.
* Use secure management protocols.
* Monitor logs and configuration changes.
* Maintain tested configuration backups.

### 🖆 What is network segmentation?

Network segmentation divides a network into smaller security or operational zones using technologies such as VLANs, routing policies, firewalls, and access-control rules.

# 🗁 MikroTik RouterOS

### 🖆 What is MikroTik RouterOS?

MikroTik RouterOS is a network operating system used on MikroTik hardware and virtualized platforms such as Cloud Hosted Router (CHR).

It supports routing, switching, firewalling, wireless networking, VPNs, bandwidth management, and ISP services.

### 🖆 What is WinBox?

WinBox is a graphical management utility used to configure and monitor MikroTik RouterOS devices.

### 🖆 How do you check the RouterOS version?

```routeros
/system resource print
```

### 🖆 How do you view configured IP addresses?

```routeros
/ip address print
```

### 🖆 How do you configure an IP address?

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
```

### 🖆 How do you configure a default route?

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.0.2.1
```

### 🖆 How do you view the routing table?

```routeros
/ip route print detail
```

### 🖆 How do you configure a DHCP server?

A typical configuration sequence is:

```routeros
/ip pool add name=lan-pool ranges=192.168.10.100-192.168.10.200

/ip dhcp-server add \
    name=lan-dhcp \
    interface=ether2 \
    address-pool=lan-pool \
    disabled=no

/ip address add address=192.168.10.1/24 interface=ether2

/ip dhcp-server network add \
    address=192.168.10.0/24 \
    gateway=192.168.10.1 \
    dns-server=192.168.10.1
```

The interface must be configured as an appropriate LAN interface, and the DHCP service must not conflict with another DHCP server on the same broadcast domain.

### 🖆 How do you configure source NAT for Internet access?

A common masquerade rule for a dynamically assigned WAN address is:

```routeros
/ip firewall nat add \
    chain=srcnat \
    out-interface=ether1 \
    action=masquerade
```

The WAN interface and firewall policy must match the actual network design.

### 🖆 What is the difference between masquerade and src-nat?

* **masquerade:** A source NAT action commonly used when the outgoing interface address may change.
* **src-nat:** Uses an explicitly configured translated source address or address range.

### 🖆 What is PCC in MikroTik?

Per Connection Classifier (PCC) is a RouterOS firewall mechanism that classifies connections using selected address and port information.

It is commonly used to distribute connections across multiple WAN links.

PCC generally distributes connections, not individual packets of a single connection.

### 🖆 What is the difference between load balancing and failover?

* **Load balancing:** Distributes traffic across multiple available WAN connections.
* **Failover:** Uses an alternative WAN connection when the preferred path becomes unavailable.

### 🖆 What is Torch?

Torch is a RouterOS real-time traffic monitoring tool that displays traffic information by interface and selected traffic attributes.

```routeros
/tool torch interface=ether1
```

### 🖆 How do you monitor interface traffic?

```routeros
/interface monitor-traffic ether1
```

### 🖆 How do you view firewall rules?

```routeros
/ip firewall filter print
/ip firewall nat print
```

### 🖆 How do you configure an OSPF instance in RouterOS v7?

A basic RouterOS v7 instance can be created with:

```routeros
/routing ospf instance add \
    name=ospf-instance \
    version=2 \
    router-id=1.1.1.1
```

An area and interface template must also be configured for the intended OSPF topology.

### 🖆 What is MikroTik CHR?

Cloud Hosted Router is the RouterOS virtual machine edition designed to run on supported virtualization and cloud platforms.

It is useful for network labs, testing, and virtualized routing environments.

# 🗁 Network Monitoring and Troubleshooting

### 🖆 What is SNMP?

Simple Network Management Protocol is used to monitor and manage network devices.

Common ports:

* UDP 161: SNMP queries and responses.
* UDP 162: SNMP traps and notifications.

SNMPv3 with authentication and privacy is generally preferred for secure management.

### 🖆 What is Syslog?

Syslog is a standard mechanism for transmitting and collecting event messages from network devices and systems.

It supports centralized logging and troubleshooting.

### 🖆 What is NetFlow?

NetFlow provides summarized network traffic-flow information, such as source and destination addresses, ports, protocols, and traffic counters.

IPFIX is a standardized flow-export protocol, while sFlow uses sampling and interface counters.

### 🖆 What is packet capture?

Packet capture records network packets for detailed protocol analysis.

Common tools include:

* Wireshark
* tcpdump
* tshark

### 🖆 What is the difference between ping and traceroute?

* **Ping:** Tests reachability and measures round-trip time using ICMP Echo where supported.
* **Traceroute:** Attempts to identify intermediate network hops using TTL or Hop Limit behavior and response messages.

### 🖆 A user cannot access the Internet. How would you troubleshoot?

Follow a layered approach:

* Check physical connectivity and interface status.
* Verify the client's IP address and subnet mask.
* Verify the default gateway.
* Test connectivity to the gateway.
* Test connectivity to a known external IP.
* Test DNS resolution.
* Check routing and NAT.
* Review firewall policies.
* Inspect packet captures and device logs when necessary.

### 🖆 A user can access websites by IP address but not by domain name. What could be wrong?

Likely DNS-related causes include:

* Incorrect DNS server configuration
* DNS server outage
* Failed recursive resolution
* Firewall blocking DNS traffic
* Incorrect DNS records
* DNS cache problems

### 🖆 A network is experiencing high latency. What would you investigate?

Investigate:

* Interface utilization
* Link errors and drops
* Queue congestion
* WAN performance
* Packet loss
* Routing paths
* CPU and memory utilization
* Wireless interference
* Application and server response times

# 🗁 Practical Scenario-Based Questions

### 🖆 Two computers in the same VLAN cannot communicate. How would you troubleshoot?

Check:

* Physical link and interface status
* VLAN assignment on both switch ports
* IP address and subnet configuration
* Duplicate IP addresses
* Switch MAC address table
* Port security and access-control rules
* Host firewall settings
* ARP resolution

Use packet capture if the problem is not clear.

### 🖆 Two VLANs cannot communicate. What would you check?

Verify:

* VLAN existence on the switches
* Access-port assignments
* Trunk configuration
* Allowed VLAN list
* Native VLAN consistency
* Layer 3 interfaces or router subinterfaces
* IP addressing and gateway configuration
* Inter-VLAN firewall rules
* Return routing

### 🖆 A DHCP client receives an APIPA address. What does it indicate?

The client did not obtain the expected DHCP configuration.

Investigate:

* DHCP server availability
* DHCP scope exhaustion
* VLAN and trunk configuration
* DHCP relay configuration
* DHCP server firewall rules
* DHCP discover and offer packets

### 🖆 An Internet connection works, but users experience slow browsing. What could be wrong?

Possible causes include:

* DNS resolution delays
* High latency
* Packet loss
* Congested WAN links
* MTU or Path MTU Discovery problems
* Proxy or firewall inspection delays
* Slow upstream DNS resolvers
* Application or server-side problems

### 🖆 A company has two ISP connections. How would you design failover?

A typical design includes:

* Primary and secondary WAN interfaces
* Separate routing paths
* Health checks that test meaningful upstream reachability
* Route preference or policy-based routing
* NAT rules for both WAN links
* Connection tracking considerations
* Monitoring and failover alerts

A gateway being locally reachable does not necessarily mean the Internet path is healthy. Use appropriate end-to-end or upstream health checks.

### 🖆 A remote office cannot communicate with the headquarters network. What would you check?

Investigate:

* VPN tunnel status
* Peer reachability
* Authentication and encryption settings
* IKE and IPsec negotiation
* Local and remote traffic selectors
* Routing in both directions
* Firewall policies
* NAT exemption or NAT traversal requirements
* MTU and fragmentation behavior

### 🖆 A switch CPU suddenly reaches 100%. What would you investigate?

Check:

* Broadcast and multicast storms
* Layer 2 loops
* STP topology changes
* Excessive control-plane traffic
* Interface errors
* MAC address flapping
* Management polling frequency
* Device logs and resource utilization

### 🖆 A server is reachable from the local network but not from the Internet. What would you check?

Check:

* Public IP assignment
* Upstream routing
* Destination NAT or port forwarding
* Firewall rules
* Server listening ports
* Server default gateway
* ISP filtering or upstream access restrictions
* Return-path routing

# 🗁 Linux Networking Commands

A network engineer should be comfortable using Linux command-line tools to inspect connectivity, interfaces, routing, sockets, DNS, and packet traffic.

### 🖆 How do you view network interfaces?

```bash
ip link show
ip address show
```

### 🖆 How do you view the routing table?

```bash
ip route show
ip -6 route show
```

### 🖆 How do you test connectivity?

```bash
ping -c 4 8.8.8.8
```

### 🖆 How do you trace a network path?

```bash
traceroute example.com
```

If `traceroute` is unavailable, install it using your distribution's package manager or use an available alternative such as `tracepath`.

### 🖆 How do you test DNS resolution?

```bash
dig example.com
```

Alternative:

```bash
nslookup example.com
```

### 🖆 How do you inspect listening ports and sockets?

```bash
ss -tulpen
```

### 🖆 How do you inspect ARP or neighbor information?

```bash
ip neigh show
```

### 🖆 How do you capture packets?

```bash
sudo tcpdump -i eth0
```

Capture DNS traffic:

```bash
sudo tcpdump -i eth0 port 53
```

Capture traffic for a specific host:

```bash
sudo tcpdump -i eth0 host 192.168.10.50
```

### 🖆 How do you inspect interface statistics?

```bash
ip -s link show
```

### 🖆 How do you test whether a TCP port is reachable?

```bash
nc -vz 192.168.10.10 443
```

These commands are useful for validating connectivity from the Linux host's perspective. They should be combined with device-side checks and packet analysis.

# 🗁 Interview Preparation Strategy

### 🖆 Build strong fundamentals

Be comfortable explaining:

* OSI and TCP/IP models
* IPv4 and IPv6 addressing
* Subnetting and CIDR
* TCP and UDP
* DNS and DHCP
* Switching and routing
* VLANs and trunking
* NAT and firewall policies

### 🖆 Practice configuration

Build practical experience with:

* Linux networking
* MikroTik RouterOS
* VLAN configuration
* Static and dynamic routing
* DHCP and DNS
* Firewall and NAT
* VPN deployment
* Monitoring and troubleshooting

### 🖆 Prepare practical lab scenarios

Use VirtualBox, MikroTik CHR, GNS3, or EVE-NG to build and troubleshoot networks.

Suggested lab projects:

* Small office network
* Multiple VLANs with inter-VLAN routing
* DHCP and DNS deployment
* OSPF multi-router topology
* Dual-WAN failover
* PCC load balancing
* Site-to-site VPN
* Firewall segmentation
* PPPoE ISP simulation
* Centralized network monitoring

### 🖆 Learn to explain your troubleshooting process

During an interview, do not jump directly to a configuration change.

A strong troubleshooting answer follows this sequence:

```text
Understand the Problem
        |
        v
Identify the Scope
        |
        v
Collect Evidence
        |
        v
Form a Hypothesis
        |
        v
Test the Hypothesis
        |
        v
Apply a Controlled Fix
        |
        v
Verify the Result
        |
        v
Document the Resolution
```

### 🖆 Prepare project explanations

For every project you have completed, be ready to explain:

* What problem the network was designed to solve
* The topology and IP addressing plan
* The equipment and technologies used
* The routing and switching design
* Security controls
* Monitoring and troubleshooting methods
* Challenges encountered
* How you verified the final result

### 🖆 Final preparation checklist

* [ ] Explain the OSI and TCP/IP models confidently.
* [ ] Solve IPv4 subnetting problems without relying entirely on a calculator.
* [ ] Understand IPv6 addressing and routing.
* [ ] Configure VLANs and trunk ports.
* [ ] Configure static routes and OSPF.
* [ ] Understand BGP fundamentals.
* [ ] Configure DHCP, DNS, NAT, and firewall rules.
* [ ] Understand VPN technologies and their use cases.
* [ ] Configure and troubleshoot MikroTik RouterOS.
* [ ] Use Linux networking tools.
* [ ] Analyze packet captures with Wireshark.
* [ ] Explain practical troubleshooting scenarios.
* [ ] Present at least two or three completed network projects.

# 🗁 Conclusion

A strong network engineer combines theoretical knowledge, practical configuration skills, systematic troubleshooting, and security awareness.

Interview preparation should not be limited to memorizing answers. Build networks, intentionally introduce faults, investigate them, document the root causes, and explain your solutions clearly.

The ability to demonstrate how a network works—and how to fix it when it fails—is one of the most valuable skills in a network engineering career.
