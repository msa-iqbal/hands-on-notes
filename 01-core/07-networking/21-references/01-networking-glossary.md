# Networking Glossary

A practical reference of common networking terms, concepts, technologies, protocols, and abbreviations. This glossary is designed to help you understand networking documentation, configure network devices, troubleshoot connectivity, and communicate effectively as a network engineer.

# 🗁 A

* **AAA (Authentication, Authorization, and Accounting):** A framework for controlling network access. Authentication verifies identity, authorization determines permissions, and accounting records user activity.
* **ACL (Access Control List):** A set of rules that permits or denies network traffic based on criteria such as source IP, destination IP, protocol, or port.
* **Access Point (AP):** A wireless networking device that allows Wi-Fi clients to connect to a wired network.
* **Access Port:** A switch port assigned to one VLAN. It normally carries untagged Ethernet frames for endpoint devices.
* **ACK (Acknowledgment):** A TCP response confirming that data has been received.
* **Address Resolution Protocol (ARP):** An IPv4 protocol used to discover the MAC address associated with an IPv4 address on a local network.
* **AES (Advanced Encryption Standard):** A symmetric encryption algorithm used in technologies such as WPA2 and WPA3.
* **Anycast:** An addressing and routing method in which multiple devices advertise the same IP address, with traffic generally delivered to the nearest available destination according to routing decisions.
* **APIPA (Automatic Private IP Addressing):** An IPv4 link-local addressing mechanism. On many systems, an address in `169.254.0.0/16` is assigned when normal address configuration fails.
* **ARP Table:** A cache containing IPv4-to-MAC address mappings.
* **Autonomous System (AS):** A group of IP networks operated under a common routing policy and identified by an Autonomous System Number (ASN).

# 🗁 B

* **Backbone Network:** The high-capacity central part of a network that interconnects smaller networks.
* **Bandwidth:** The theoretical or configured capacity of a communication link, commonly measured in bits per second.
* **BGP (Border Gateway Protocol):** The path-vector routing protocol used to exchange routing information between autonomous systems.
* **BGP Peering:** A routing relationship between two BGP-speaking routers.
* **Broadcast:** Communication in which a frame or packet is sent to all applicable devices within a broadcast domain.
* **Broadcast Domain:** A logical network area in which Layer 2 broadcast frames are forwarded.
* **Bridge:** A Layer 2 device or software function that forwards Ethernet frames based on MAC addresses.
* **Bridge Domain:** A Layer 2 forwarding domain that provides bridging and broadcast behavior.
* **Broadband:** High-speed internet access delivered through technologies such as fiber, cable, DSL, or fixed wireless.
* **Buffer:** Temporary memory used to hold packets or frames while they wait to be processed or transmitted.
* **Bufferbloat:** Excessive latency caused by large queues buffering packets for too long under congestion.

# 🗁 C

* **Carrier-Grade NAT (CGNAT):** A NAT implementation used by service providers to share public IPv4 addresses among multiple customers. The shared address space `100.64.0.0/10` is reserved for this purpose.
* **CIDR (Classless Inter-Domain Routing):** An IP addressing and routing method that uses prefix lengths, such as `/24`, instead of traditional address classes.
* **Client:** A device or application that requests a service from another system.
* **Collision Domain:** A network segment in which simultaneous Ethernet transmissions could collide. Modern switched full-duplex Ethernet largely eliminates collisions.
* **Congestion:** A condition in which network traffic exceeds available capacity, potentially causing queueing, delay, and packet loss.
* **Connection Tracking (Conntrack):** A firewall mechanism that tracks the state of network connections and related packets.
* **Core Network:** The central high-capacity portion of a network that connects distribution or access networks.
* **CPE (Customer Premises Equipment):** Networking equipment installed at a customer's location, such as an ONT, modem, router, or wireless gateway.
* **Cross-Connect:** A physical or logical connection between two network circuits or devices.
* **CSMA/CD (Carrier Sense Multiple Access with Collision Detection):** The traditional Ethernet access method used by shared, half-duplex Ethernet networks.

# 🗁 D

* **Data Plane:** The part of a network device responsible for forwarding packets or frames.
* **Default Gateway:** The router address a host uses to send traffic to destinations outside its local IP subnet.
* **Default Route:** A route used when no more specific matching route exists. For IPv4, it is `0.0.0.0/0`; for IPv6, it is `::/0`.
* **DHCP (Dynamic Host Configuration Protocol):** A protocol that automatically provides IP configuration, including addresses, subnet masks, gateways, and DNS servers.
* **DHCP Lease:** A time-limited assignment of an IP address and related configuration to a DHCP client.
* **DHCP Relay:** A function that forwards DHCP messages between clients and a DHCP server located on another subnet.
* **DHCP Reservation:** A DHCP configuration that consistently assigns a particular address to a client, usually based on its MAC address or client identifier.
* **DNS (Domain Name System):** A distributed naming system that translates domain names into IP addresses and provides other resource records.
* **DNS Resolver:** A service that processes DNS queries and obtains the requested records, either from its cache or by querying other DNS servers.
* **DNS Zone:** An administrative portion of the DNS namespace managed by authoritative DNS servers.
* **DoS (Denial of Service):** An attack or event that makes a service unavailable by exhausting or disrupting its resources.
* **DDoS (Distributed Denial of Service):** A denial-of-service attack generated from multiple sources.
* **DSCP (Differentiated Services Code Point):** A field in the IP header used to classify packets for differentiated traffic handling and QoS.

# 🗁 E

* **Egress Traffic:** Traffic leaving a network device, interface, or defined network boundary.
* **Encapsulation:** The process of adding protocol headers, and sometimes trailers, as data moves through network layers.
* **Ethernet:** A family of wired networking technologies commonly used in local area networks.
* **EtherType:** An Ethernet frame field identifying the protocol carried in the payload, such as IPv4 or IPv6.
* **ECMP (Equal-Cost Multi-Path):** A routing technique that distributes traffic across multiple paths with equal routing cost.
* **Edge Router:** A router positioned at the boundary between a network and an external network, such as an ISP.
* **EoIP (Ethernet over IP):** A MikroTik tunneling technology that transports Ethernet frames over IP.

# 🗁 F

* **Failover:** Automatic switching to a backup link, device, or service when the primary one becomes unavailable.
* **Firewall:** A security system that controls network traffic according to defined security rules.
* **Firewall Rule:** A condition and associated action used to accept, drop, reject, or otherwise process traffic.
* **Firmware:** Software embedded in a networking device that controls its hardware and core functions.
* **Flow:** A group of packets identified by common attributes, often source and destination addresses, ports, and protocol.
* **Flow Control:** A mechanism that prevents a sender from overwhelming a receiver.
* **Forwarding:** The process of moving packets or frames toward their next destination.
* **Forwarding Table:** A data structure used by a network device to determine where traffic should be sent.
* **Full Duplex:** A communication mode that allows simultaneous transmission and reception.
* **FQDN (Fully Qualified Domain Name):** The complete DNS name of a host, including its domain hierarchy, such as `server.example.com`.

# 🗁 G

* **Gateway:** A device or system that connects different networks or provides access to another network.
* **GRE (Generic Routing Encapsulation):** A tunneling protocol that encapsulates network-layer packets inside IP packets.
* **Guest Network:** A logically separated network intended for visitors or untrusted devices.
* **Gigabit Ethernet:** Ethernet technology supporting a nominal data rate of 1 Gbps.
* **GPON (Gigabit Passive Optical Network):** A passive optical access technology commonly used by fiber internet providers.
* **Graphical User Interface (GUI):** A visual interface for managing a system, such as MikroTik WinBox or WebFig.

# 🗁 H

* **Half Duplex:** A communication mode in which a device can transmit or receive, but not both simultaneously.
* **Handshake:** A sequence of messages used to establish communication parameters or a connection. TCP uses a three-way handshake.
* **Header:** Protocol metadata added to data to support addressing, delivery, control, or interpretation.
* **Hop:** One intermediate Layer 3 forwarding device traversed by a packet.
* **Hop Count:** The number of routing hops to a destination, used as a metric by some routing protocols.
* **Host:** A network-connected device with an IP address, such as a server, workstation, or printer.
* **Hotspot:** A service that provides network access through a captive portal or user authentication mechanism.
* **HTTP (Hypertext Transfer Protocol):** An application-layer protocol used for transferring web resources.
* **HTTPS:** HTTP protected by TLS, providing encryption and authentication for web communication.
* **Hub:** A basic Layer 1 device that repeats incoming signals to all other ports.

# 🗁 I

* **ICMP (Internet Control Message Protocol):** A network-layer support protocol used for error reporting and diagnostics, including ping.
* **ICMPv6:** The IPv6 control-message protocol, supporting diagnostics and essential functions such as Neighbor Discovery.
* **IDS (Intrusion Detection System):** A system that monitors activity and identifies potentially malicious behavior.
* **IEEE (Institute of Electrical and Electronics Engineers):** An organization that develops standards, including Ethernet and Wi-Fi specifications.
* **IGMP (Internet Group Management Protocol):** An IPv4 protocol used by hosts and routers to manage multicast group membership.
* **Ingress Traffic:** Traffic entering a network device, interface, or defined network boundary.
* **Interface:** A physical or logical connection through which a device sends or receives network traffic.
* **IP (Internet Protocol):** A network-layer protocol responsible for logical addressing and packet delivery across interconnected networks.
* **IP Address:** A logical address assigned to a network interface for IP communication.
* **IPsec:** A suite of protocols that provides authentication, integrity, and encryption for IP communication.
* **IPv4:** The 32-bit version of IP, commonly represented in dotted-decimal notation.
* **IPv6:** The 128-bit version of IP, designed to provide a vastly larger address space and additional protocol capabilities.
* **ISP (Internet Service Provider):** An organization that provides internet connectivity and related network services.
* **IXP (Internet Exchange Point):** A facility or platform where multiple networks exchange traffic directly.

# 🗁 J

* **Jitter:** Variation in packet delay over time. High jitter can negatively affect voice and video communication.
* **Jumbo Frame:** An Ethernet frame larger than the traditional 1500-byte IP MTU, commonly configured around a 9000-byte MTU where supported.
* **Jitter Buffer:** A buffer used by real-time applications to compensate for variations in packet arrival times.

# 🗁 K

* **Keepalive:** A periodic message or mechanism used to verify that a connection or peer remains reachable.
* **Kernel:** The core component of an operating system that manages hardware, memory, processes, and networking functions.
* **Key Exchange:** A process through which communicating parties establish cryptographic keys.

# 🗁 L

* **LAN (Local Area Network):** A network covering a limited area, such as a home, office, or building.
* **Layer 2:** The data-link layer of the OSI model, responsible for local frame delivery and MAC addressing.
* **Layer 3:** The network layer of the OSI model, responsible for logical addressing and packet routing.
* **Latency:** The time required for data to travel from one point to another.
* **Link Aggregation:** Combining multiple physical network links into one logical link for increased capacity or redundancy.
* **Link-Local Address:** An address used for communication within a local network segment. IPv4 link-local addresses use `169.254.0.0/16`; IPv6 link-local addresses use `fe80::/10`.
* **Load Balancing:** Distributing traffic across multiple links, servers, or resources.
* **Loopback Address:** An address used for a device to communicate with itself. Common examples are `127.0.0.1` in IPv4 and `::1` in IPv6.
* **Loop Prevention:** Techniques that prevent frames or packets from circulating indefinitely through a network.
* **LLDP (Link Layer Discovery Protocol):** A vendor-neutral protocol that allows directly connected network devices to advertise identity and capabilities.
* **LACP (Link Aggregation Control Protocol):** A protocol standardized by IEEE 802.1AX for dynamically negotiating link aggregation.

# 🗁 M

* **MAC Address (Media Access Control Address):** A link-layer address used to identify a network interface within a local Layer 2 network.
* **MAC Address Table:** A switch or bridge table that maps learned MAC addresses to interfaces.
* **Management Plane:** The part of a network device used for configuration, administration, and monitoring.
* **Metric:** A numerical value used by a routing protocol or routing process to compare possible paths.
* **MIB (Management Information Base):** A structured collection of managed objects that can be accessed through SNMP.
* **MPLS (Multiprotocol Label Switching):** A forwarding technology that uses labels to direct traffic through a network.
* **MTU (Maximum Transmission Unit):** The maximum size of a network-layer packet that can be carried in a link-layer frame without fragmentation at that layer.
* **Multicast:** Communication in which traffic is delivered to members of a specific multicast group.
* **Masquerade:** A form of source NAT commonly used when a device's outgoing interface address may change.
* **MSS (Maximum Segment Size):** The maximum TCP payload size that a host is willing to receive in one TCP segment.
* **Multihoming:** Connecting a network or system to multiple upstream networks or providers for redundancy or routing flexibility.

# 🗁 N

* **NAC (Network Access Control):** A security approach that controls network access based on identity, device posture, or policy.
* **NAT (Network Address Translation):** A technique that modifies IP address information in packet headers as traffic passes through a router or firewall.
* **NAT Traversal (NAT-T):** Techniques that allow protocols such as IPsec to operate through NAT devices.
* **NDP (Neighbor Discovery Protocol):** An IPv6 protocol suite used for neighbor discovery, router discovery, address resolution, and related functions.
* **Netmask:** Another term for subnet mask, which identifies the network and host portions of an IPv4 address.
* **NetFlow:** A flow-export technology used to collect information about network traffic.
* **Network Address:** The address identifying an IP subnet. For example, `192.168.10.0/24`.
* **Network Interface Card (NIC):** A hardware or virtual component that provides network connectivity.
* **Network Segmentation:** Dividing a network into smaller logical or physical sections to improve security, performance, and manageability.
* **Next Hop:** The next router or forwarding destination toward a packet's final destination.
* **NTP (Network Time Protocol):** A protocol used to synchronize system clocks over a network.
* **NAT Loopback:** A technique that allows internal clients to access an internal service using its public or externally mapped address.

# 🗁 O

* **ONT (Optical Network Terminal):** Customer-side equipment that terminates a fiber-optic access connection and converts optical signals into interfaces such as Ethernet.
* **ONU (Optical Network Unit):** An optical network endpoint used in fiber access networks. The terms ONU and ONT are sometimes used differently by vendors and operators.
* **OSPF (Open Shortest Path First):** A link-state interior gateway routing protocol that calculates routes using a shortest-path algorithm.
* **OSI Model:** A seven-layer conceptual model used to understand network communication.
* **Outbound Traffic:** Traffic sent from a device or network toward another destination.
* **Overlay Network:** A logical network built on top of an existing physical or underlay network.
* **Overhead:** Additional protocol data or processing required beyond the application payload.

# 🗁 P

* **Packet:** A unit of data at the network layer, such as an IPv4 or IPv6 datagram.
* **Packet Capture:** The process of recording network packets for analysis.
* **Packet Loss:** The failure of packets to reach their intended destination.
* **Packet Switching:** A communication method in which data is divided into packets that are forwarded independently through a network.
* **PAT (Port Address Translation):** A form of NAT that maps multiple internal connections to an external IP address using different transport-layer ports.
* **PCC (Per Connection Classifier):** A MikroTik RouterOS method for classifying connections, commonly used in multi-WAN load-balancing configurations.
* **Ping:** A diagnostic utility that commonly uses ICMP Echo Request and Echo Reply messages to test reachability and measure round-trip time.
* **PoE (Power over Ethernet):** A technology that delivers electrical power and data over Ethernet cabling.
* **Port:** A logical endpoint used by transport-layer protocols such as TCP and UDP.
* **Port Forwarding:** A configuration that directs traffic arriving at a particular address and port to an internal destination.
* **PPPoE (Point-to-Point Protocol over Ethernet):** A protocol used to establish PPP sessions over Ethernet, commonly used by broadband ISPs.
* **PPTP (Point-to-Point Tunneling Protocol):** An older VPN tunneling protocol with known security weaknesses.
* **Prefix Length:** The number of leading bits used to identify the network portion of an IP address, such as `/24`.
* **Proxy Server:** An intermediary that handles requests between clients and destination servers.
* **Public IP Address:** An IP address that is globally routable on the public internet, subject to routing and filtering policies.
* **Private IP Address:** An IPv4 address reserved for private networks and not globally routed on the public internet.
* **PBR (Policy-Based Routing):** Routing traffic according to policies such as source address, destination, protocol, or other packet attributes rather than only the destination route.

# 🗁 Q

* **QoS (Quality of Service):** Techniques for classifying, prioritizing, shaping, or policing traffic to manage network performance.
* **Queue:** A buffer that holds packets while they wait to be transmitted or processed.
* **Queue Management:** Techniques for controlling packet queues to manage congestion and latency.
* **Query:** A request sent to a network service, such as a DNS query asking for a domain's IP address.

# 🗁 R

* **RADIUS (Remote Authentication Dial-In User Service):** A client-server protocol used for centralized authentication, authorization, and accounting.
* **RDP (Remote Desktop Protocol):** A Microsoft protocol used for remote graphical desktop access.
* **Redundancy:** The use of additional devices, links, or systems to maintain service when a component fails.
* **Remote Access VPN:** A VPN connection that allows an individual user or device to access a private network remotely.
* **Repeater:** A Layer 1 device that regenerates or retransmits signals to extend a physical network segment.
* **RIP (Routing Information Protocol):** A distance-vector routing protocol that uses hop count as its primary metric.
* **RMON (Remote Network Monitoring):** A monitoring standard that provides network traffic and performance information.
* **Router:** A Layer 3 device that forwards packets between IP networks.
* **Routing:** The process of selecting paths through which packets travel to reach their destinations.
* **Routing Protocol:** A protocol used by routers to exchange network reachability information.
* **Routing Table:** A collection of routes used to determine where packets should be forwarded.
* **Route Redistribution:** The process of sharing routes learned from one routing source or protocol with another.
* **RTT (Round-Trip Time):** The time required for a packet or request to travel to a destination and for a response to return.
* **Reverse Proxy:** A proxy that receives requests on behalf of backend servers and forwards them to the appropriate service.

# 🗁 S

* **SFP (Small Form-factor Pluggable):** A removable transceiver module used to connect network equipment to fiber or copper links.
* **SFP+ (Enhanced Small Form-factor Pluggable):** A transceiver form factor commonly used for 10 Gigabit Ethernet connections.
* **Shaping:** A traffic-management technique that delays packets to keep transmission within a configured rate.
* **SNAT (Source Network Address Translation):** NAT that changes the source address of packets.
* **SNMP (Simple Network Management Protocol):** A protocol used to monitor and manage network devices.
* **SNMP Agent:** Software running on a managed device that exposes management information to an SNMP manager.
* **SNMP Manager:** A system that queries SNMP agents and collects monitoring information.
* **SNMP Trap:** An asynchronous notification sent by an SNMP agent to a manager.
* **SNMPv3:** An SNMP version that supports authentication, integrity, and privacy through security mechanisms.
* **Socket:** A communication endpoint typically identified by an IP address, transport protocol, and port.
* **Spanning Tree Protocol (STP):** A Layer 2 protocol that prevents switching loops by creating a loop-free logical topology.
* **SSH (Secure Shell):** An encrypted protocol used for secure remote administration and command execution.
* **SSID (Service Set Identifier):** The name identifying a Wi-Fi network.
* **Stateful Firewall:** A firewall that tracks connection states and uses that information when making filtering decisions.
* **Stateless Firewall:** A firewall that evaluates packets independently without maintaining connection state.
* **Static IP Address:** An address configured manually or assigned through a fixed reservation.
* **Static Route:** A route manually configured by a network administrator.
* **Subnet:** A logical division of an IP network.
* **Subnet Mask:** A 32-bit IPv4 value identifying the network and host portions of an address.
* **Supernetting:** Combining multiple contiguous IP networks into a larger address block using route aggregation.
* **Syslog:** A standard mechanism for sending and collecting system and network-device log messages.
* **SYN:** A TCP control flag used to initiate a connection.
* **SYN-ACK:** A TCP response that acknowledges a received SYN and signals readiness to establish a connection.
* **SFTP (SSH File Transfer Protocol):** A secure file-transfer protocol that operates over SSH.
* **Split Horizon:** A routing technique that prevents a router from advertising a route back through the interface from which it was learned, in protocols that support this behavior.

# 🗁 T

* **TCP (Transmission Control Protocol):** A connection-oriented transport protocol that provides reliable, ordered byte-stream delivery.
* **TCP Handshake:** The three-way process involving SYN, SYN-ACK, and ACK used to establish a TCP connection.
* **TCP Window:** A mechanism used for TCP flow control to advertise how much data a receiver can accept.
* **Telnet:** An older remote-terminal protocol that transmits data without encryption.
* **Throughput:** The actual rate of successful data transfer over a network.
* **Time to Live (TTL):** An IPv4 header field decremented by routers to limit packet lifetime. IPv6 uses the Hop Limit field for the equivalent purpose.
* **Traceroute:** A diagnostic utility that identifies intermediate Layer 3 hops along a path toward a destination.
* **Traffic Policing:** A traffic-control technique that enforces a rate limit, commonly by dropping or remarking traffic that exceeds the configured rate.
* **Traffic Shaping:** A technique that buffers and delays packets to smooth traffic and enforce a rate.
* **Trunk Port:** A switch port that carries traffic for multiple VLANs, commonly using IEEE 802.1Q VLAN tags.
* **Tunnel:** A logical connection that encapsulates one protocol inside another for transport across a network.
* **TLS (Transport Layer Security):** A cryptographic protocol that provides confidentiality, integrity, and authentication for network communication.
* **TFTP (Trivial File Transfer Protocol):** A simple file-transfer protocol commonly used in controlled environments such as network booting.
* **Tunneling Protocol:** A protocol that encapsulates traffic to transport it across another network.

# 🗁 U

* **UDP (User Datagram Protocol):** A connectionless transport protocol that provides datagram delivery without TCP-style reliability or ordering guarantees.
* **Unicast:** Communication from one sender to one destination.
* **Uplink:** A connection from an access or lower-level network device toward an upstream device or network.
* **URL (Uniform Resource Locator):** An address identifying a resource and how it can be accessed.
* **UTP (Unshielded Twisted Pair):** A copper Ethernet cable type that uses twisted wire pairs without additional shielding.
* **UTM (Unified Threat Management):** A security platform combining multiple functions, such as firewalling, intrusion prevention, and web filtering.

# 🗁 V

* **VLAN (Virtual Local Area Network):** A logical Layer 2 network that separates broadcast domains on shared switching infrastructure.
* **VLAN ID:** A numerical identifier assigned to a VLAN. IEEE 802.1Q uses a 12-bit VLAN identifier field.
* **VLAN Tag:** Information inserted into an Ethernet frame to identify its VLAN.
* **VPN (Virtual Private Network):** A technology that creates a protected logical connection across an underlying network.
* **VPLS (Virtual Private LAN Service):** A provider-network technology that offers Layer 2 multipoint connectivity between customer sites.
* **VRF (Virtual Routing and Forwarding):** A technology that maintains separate routing tables on one router.
* **VXLAN (Virtual Extensible LAN):** An overlay tunneling technology that carries Layer 2 Ethernet frames over a Layer 3 network.
* **VoIP (Voice over Internet Protocol):** Technology that carries voice communication over IP networks.
* **Virtual Interface:** A logical interface that exists in software rather than as a separate physical port.

# 🗁 W

* **WAN (Wide Area Network):** A network that connects geographically separated locations.
* **WAF (Web Application Firewall):** A security system that filters and monitors HTTP and HTTPS traffic to protect web applications.
* **WEP (Wired Equivalent Privacy):** An obsolete Wi-Fi security protocol with serious security weaknesses.
* **Wi-Fi:** A family of wireless LAN technologies based on IEEE 802.11 standards.
* **WLAN (Wireless Local Area Network):** A local area network that uses wireless communication.
* **WPA2:** A Wi-Fi security standard commonly using AES-based CCMP encryption.
* **WPA3:** A newer Wi-Fi security generation with improved authentication and security mechanisms.
* **WPS (Wi-Fi Protected Setup):** A mechanism designed to simplify Wi-Fi device enrollment.
* **WDS (Wireless Distribution System):** A mechanism for connecting wireless access points, commonly for bridging or extending wireless coverage.
* **WireGuard:** A modern VPN protocol designed to provide secure, efficient, and relatively simple encrypted tunnels.
* **WAN Failover:** Automatic movement of traffic to a backup WAN connection when the primary connection becomes unavailable.

# 🗁 X

* **X.509:** A standard defining digital certificates commonly used in TLS, VPNs, and public key infrastructure.
* **XDP (eXpress Data Path):** A Linux kernel technology that enables high-performance packet processing at an early point in the networking stack.
* **XML (Extensible Markup Language):** A structured data format sometimes used in network configuration and management interfaces.

# 🗁 Y

* **YANG:** A data modeling language used to describe configuration and operational state for network devices.
* **YANG Model:** A structured definition of data objects, configuration parameters, and operational information used by management systems.

# 🗁 Z

* **Zero Trust:** A security model that requires explicit verification and least-privilege access rather than automatically trusting devices based on network location.
* **Zone-Based Firewall:** A firewall architecture that groups interfaces into security zones and applies policies between those zones.
* **Zero-Touch Provisioning (ZTP):** Automated device deployment in which a new device obtains its configuration with minimal manual intervention.

# 🗁 Important Networking Abbreviations

| Abbreviation | Full Form |
| --- | --- |
| AAA | Authentication, Authorization, and Accounting |
| ACL | Access Control List |
| AP | Access Point |
| APIPA | Automatic Private IP Addressing |
| ARP | Address Resolution Protocol |
| AS | Autonomous System |
| ASN | Autonomous System Number |
| BGP | Border Gateway Protocol |
| CAPWAP | Control and Provisioning of Wireless Access Points |
| CGNAT | Carrier-Grade Network Address Translation |
| CIDR | Classless Inter-Domain Routing |
| CPE | Customer Premises Equipment |
| DHCP | Dynamic Host Configuration Protocol |
| DNS | Domain Name System |
| DoS | Denial of Service |
| DDoS | Distributed Denial of Service |
| DSCP | Differentiated Services Code Point |
| ECMP | Equal-Cost Multi-Path |
| FTP | File Transfer Protocol |
| FQDN | Fully Qualified Domain Name |
| GRE | Generic Routing Encapsulation |
| HTTP | Hypertext Transfer Protocol |
| HTTPS | Hypertext Transfer Protocol Secure |
| ICMP | Internet Control Message Protocol |
| IDS | Intrusion Detection System |
| IP | Internet Protocol |
| IPsec | Internet Protocol Security |
| ISP | Internet Service Provider |
| IXP | Internet Exchange Point |
| LAN | Local Area Network |
| LACP | Link Aggregation Control Protocol |
| MAC | Media Access Control |
| MIB | Management Information Base |
| MPLS | Multiprotocol Label Switching |
| MSS | Maximum Segment Size |
| MTU | Maximum Transmission Unit |
| NAT | Network Address Translation |
| NDP | Neighbor Discovery Protocol |
| NIC | Network Interface Card |
| NTP | Network Time Protocol |
| OSPF | Open Shortest Path First |
| OSI | Open Systems Interconnection |
| ONU | Optical Network Unit |
| ONT | Optical Network Terminal |
| PAT | Port Address Translation |
| PCC | Per Connection Classifier |
| PoE | Power over Ethernet |
| PPPoE | Point-to-Point Protocol over Ethernet |
| QoS | Quality of Service |
| RADIUS | Remote Authentication Dial-In User Service |
| RIP | Routing Information Protocol |
| RTT | Round-Trip Time |
| SFP | Small Form-factor Pluggable |
| SNAT | Source Network Address Translation |
| SNMP | Simple Network Management Protocol |
| SSH | Secure Shell |
| SSID | Service Set Identifier |
| STP | Spanning Tree Protocol |
| SFTP | SSH File Transfer Protocol |
| Syslog | System Logging Protocol |
| TCP | Transmission Control Protocol |
| TFTP | Trivial File Transfer Protocol |
| TLS | Transport Layer Security |
| TTL | Time to Live |
| UDP | User Datagram Protocol |
| VLAN | Virtual Local Area Network |
| VoIP | Voice over Internet Protocol |
| VPN | Virtual Private Network |
| VRF | Virtual Routing and Forwarding |
| VXLAN | Virtual Extensible LAN |
| WAN | Wide Area Network |
| WAF | Web Application Firewall |
| WLAN | Wireless Local Area Network |
| WEP | Wired Equivalent Privacy |
| WPA | Wi-Fi Protected Access |
| WDS | Wireless Distribution System |
| XDP | eXpress Data Path |
| ZTP | Zero-Touch Provisioning |

# 🗁 Important Notes

* **Bandwidth and throughput are different:** Bandwidth describes capacity; throughput describes the actual successful data-transfer rate.
* **Latency and jitter are different:** Latency measures delay; jitter measures variation in delay.
* **A port is not an interface:** A TCP or UDP port identifies an application endpoint, while a network interface connects a device to a network.
* **A switch and a router operate differently:** A traditional Layer 2 switch forwards frames using MAC addresses; a router forwards packets using IP routing information.
* **A public IP is not necessarily reachable:** Firewalls, routing policies, and upstream filtering can prevent incoming connections.
* **NAT is not a complete security solution:** Firewall policies and proper access controls are still required.
* **A VLAN is not automatically a security boundary:** Inter-VLAN routing and firewall policies determine how VLANs can communicate.
* **A successful ping does not prove that an application works:** ICMP reachability does not guarantee that DNS, TCP ports, or application services are functioning.
* **A failed ping does not always mean a host is offline:** ICMP may be filtered or deprioritized.
* **A VPN does not automatically provide anonymity:** It protects traffic between defined tunnel endpoints, but other systems and services may still identify users.

# 🗁 How to Use This Glossary

Use this document as a quick reference while studying networking concepts, configuring MikroTik RouterOS, troubleshooting connectivity, reading vendor documentation, and preparing for networking certifications.

For deeper understanding, follow the relevant topic documents in this knowledge base. The glossary provides concise definitions; the individual topic documents explain how each technology works, how to configure it, and how to troubleshoot it.
