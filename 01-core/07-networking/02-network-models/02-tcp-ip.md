# 🗁 TCP/IP Model

The **TCP/IP Model** is a networking architecture that describes how devices communicate across interconnected networks, including the Internet.

TCP/IP stands for **Transmission Control Protocol / Internet Protocol**. It refers to a collection of communication protocols that work together to provide end-to-end network communication.

Unlike the seven-layer OSI model, the commonly used TCP/IP model consists of four layers:

* Application
* Transport
* Internet
* Network Access

Each layer has specific responsibilities and uses the services provided by the layer below it.

The TCP/IP model is the foundation of modern Internet communication. Understanding it helps explain how applications, transport protocols, IP addressing, routing, and network interfaces work together.

# 🗁 TCP/IP Fundamentals

TCP/IP is a protocol suite that enables communication between devices across different networks.

It defines how data is formatted, addressed, transmitted, routed, and received.

For example, when a computer accesses a website hosted on a remote server, multiple TCP/IP protocols cooperate:

* DNS resolves the website's domain name.
* TCP or UDP provides transport between application endpoints.
* IP provides logical addressing and packet delivery.
* Ethernet or Wi-Fi carries data across the local network.

These protocols work together to make communication possible.

### 🖆 What Is a Protocol Suite?

A protocol suite is a collection of networking protocols designed to work together.

The TCP/IP protocol suite includes protocols such as:

| Protocol | Main Purpose |
| --- | --- |
| HTTP | Web communication |
| HTTPS | Secure web communication |
| DNS | Domain name resolution |
| DHCP | Automatic IP configuration |
| TCP | Reliable, connection-oriented transport |
| UDP | Connectionless, message-oriented transport |
| IPv4 | Internet Protocol version 4 |
| IPv6 | Internet Protocol version 6 |
| ICMP | Network diagnostics and error reporting |
| ARP | IPv4 address-to-link-layer address resolution on applicable networks |
| NDP | IPv6 neighbor discovery |
| Ethernet | Wired local network communication |
| Wi-Fi | Wireless local network communication |

### 🖆 Why TCP/IP Is Important

TCP/IP provides the foundation for modern network communication.

Its importance includes:

* Enables communication across interconnected networks.
* Supports the global Internet.
* Provides logical addressing through IP.
* Supports different transport mechanisms through TCP and UDP.
* Allows applications to communicate across different operating systems and hardware platforms.
* Supports scalable routing across large networks.
* Enables communication between different network technologies.

# 🗁 TCP/IP Architecture

The commonly used TCP/IP model contains four layers.

Each layer performs specific networking functions and communicates with the layers above and below it.

### 🖆 Four-Layer Architecture

| Layer | Main Responsibility | Example Protocols |
| --- | --- | --- |
| Application | Application-level communication | HTTP, HTTPS, DNS, DHCP, SSH, SMTP |
| Transport | Process-to-process communication | TCP, UDP, SCTP |
| Internet | Logical addressing and routing | IPv4, IPv6, ICMP |
| Network Access | Local delivery and physical transmission | Ethernet, Wi-Fi, PPP |

### 🖆 TCP/IP Model Diagram

```text
+------------------------------------------+
| APPLICATION                              |
| HTTP | HTTPS | DNS | DHCP | SSH | SMTP   |
+------------------------------------------+
                    |
                    v
+------------------------------------------+
| TRANSPORT                                |
| TCP | UDP | SCTP                         |
+------------------------------------------+
                    |
                    v
+------------------------------------------+
| INTERNET                                 |
| IPv4 | IPv6 | ICMP                       |
+------------------------------------------+
                    |
                    v
+------------------------------------------+
| NETWORK ACCESS                           |
| Ethernet | Wi-Fi | PPP                   |
+------------------------------------------+
```

### 🖆 Layer Interaction

The TCP/IP layers cooperate during communication.

On the sending device:

* The Application layer creates application data.
* The Transport layer prepares data for process-to-process delivery.
* The Internet layer adds IP addressing information.
* The Network Access layer prepares the data for transmission over a network link.

On the receiving device, the process happens in reverse.

# 🗁 Application Layer

The Application layer is the topmost layer of the TCP/IP model.

It provides network communication services to application software.

It combines responsibilities associated with the OSI Application, Presentation, and Session layers.

### 🖆 Main Responsibilities

* Provides application-level network services.
* Defines application request and response formats.
* Supports communication between application processes.
* Handles application-specific data representation.
* Supports application session management.
* Provides services such as web browsing, email, file transfer, and remote access.

### 🖆 Common Application Protocols

| Protocol | Full Name / Description | Common Port |
| --- | --- | --- |
| HTTP | Hypertext Transfer Protocol | TCP 80 |
| HTTPS | HTTP over TLS | TCP 443 |
| DNS | Domain Name System | UDP/TCP 53 |
| DHCP | Dynamic Host Configuration Protocol | UDP 67/68 |
| FTP | File Transfer Protocol | TCP 20/21 |
| SFTP | SSH File Transfer Protocol | TCP 22 |
| SSH | Secure Shell | TCP 22 |
| Telnet | Remote terminal protocol | TCP 23 |
| SMTP | Simple Mail Transfer Protocol | TCP 25 |
| POP3 | Post Office Protocol version 3 | TCP 110 |
| IMAP | Internet Message Access Protocol | TCP 143 |
| NTP | Network Time Protocol | UDP 123 |
| SNMP | Simple Network Management Protocol | UDP 161/162 |

These are common default ports. Protocols can use other ports depending on their configuration and deployment.

### 🖆 HTTP Example

When a browser accesses a website, it sends an HTTP request to the web server.

Example:

```http
GET / HTTP/1.1
Host: example.com
Accept: text/html
```

The server processes the request and returns an HTTP response.

The Application layer defines the structure and meaning of this exchange.

### 🖆 DNS Example

When a user enters:

```text
example.com
```

The operating system or application may perform a DNS lookup to obtain an IP address.

Example command:

```bash
dig example.com
```

DNS is an Application-layer protocol in the TCP/IP model, even though it helps applications obtain information needed for lower-layer communication.

# 🗁 Transport Layer

The Transport layer provides communication between application processes running on different devices.

It uses port numbers to identify application endpoints.

The two most widely used transport protocols are TCP and UDP.

### 🖆 Main Responsibilities

* Process-to-process communication.
* Port addressing.
* Segmentation and reassembly.
* Connection management.
* Reliable delivery when supported.
* Flow control when supported.
* Congestion control when supported.
* Multiplexing and demultiplexing application traffic.

### 🖆 TCP

**Transmission Control Protocol (TCP)** is a connection-oriented transport protocol.

It provides a reliable, ordered byte stream between communicating endpoints.

Important features include:

* Connection establishment.
* Sequence numbers.
* Acknowledgments.
* Retransmission.
* Ordered delivery.
* Flow control.
* Congestion control.
* Connection termination.

A TCP connection is established using a three-way handshake.

```text
Client                         Server
  |                               |
  |----------- SYN -------------->|
  |                               |
  |<---------- SYN-ACK ------------|
  |                               |
  |----------- ACK -------------->|
  |                               |
  |       Connection Established   |
```

TCP is commonly used by:

* HTTP/1.1.
* HTTP/2.
* SSH.
* SMTP.
* FTP.
* Many database connections.

### 🖆 UDP

**User Datagram Protocol (UDP)** is a connectionless, message-oriented transport protocol.

It provides a lightweight way to send datagrams between application endpoints.

Important characteristics include:

* No connection establishment handshake.
* No built-in delivery acknowledgments.
* No built-in retransmission.
* No built-in ordering guarantee.
* Low protocol overhead.
* Message boundaries are preserved.

UDP is commonly used by:

* DNS.
* DHCP.
* NTP.
* Real-time communication.
* Streaming applications.
* QUIC.

### 🖆 TCP vs UDP

| Feature | TCP | UDP |
| --- | --- | --- |
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable byte-stream delivery | No built-in delivery guarantee |
| Ordering | Ordered byte stream | No ordering guarantee |
| Acknowledgments | Supported | Not provided by UDP itself |
| Retransmission | Supported | Not provided by UDP itself |
| Flow control | Supported | Not provided by UDP itself |
| Congestion control | Built in | Not provided by UDP itself |
| Data format | Byte stream | Datagrams |
| Header size | Minimum 20 bytes | 8 bytes |
| Common uses | Web, SSH, email | DNS, DHCP, real-time traffic |

UDP applications can implement their own reliability, ordering, and congestion-control mechanisms when required.

### 🖆 Port Numbers

A port number identifies a transport-layer endpoint.

TCP and UDP each have their own port-number spaces.

Port numbers range from `0` to `65535`.

| Port Range | Category |
| --- | --- |
| 0–1023 | System / Well-known ports |
| 1024–49151 | User / Registered ports |
| 49152–65535 | Dynamic / Private ports |

Examples:

```text
TCP 80    - HTTP
TCP 443   - HTTPS
TCP 22    - SSH
UDP 53    - DNS
UDP 67    - DHCP Server
UDP 68    - DHCP Client
```

### 🖆 Transport-Layer PDU

| Protocol | PDU |
| --- | --- |
| TCP | Segment |
| UDP | Datagram |
| SCTP | SCTP packet containing chunks |

# 🗁 Internet Layer

The Internet layer is responsible for logical addressing and packet delivery across interconnected networks.

It allows devices to communicate even when they are located on different IP networks.

### 🖆 Main Responsibilities

* Logical addressing.
* Packet forwarding.
* Routing between networks.
* Path selection.
* Packet fragmentation where supported.
* Network-level error reporting and diagnostics.

### 🖆 Internet-Layer Protocols

| Protocol | Purpose |
| --- | --- |
| IPv4 | 32-bit Internet Protocol addressing |
| IPv6 | 128-bit Internet Protocol addressing |
| ICMP | IPv4-related diagnostics and error reporting |
| ICMPv6 | IPv6 diagnostics and control messaging |
| IPsec | Security architecture for IP communication |

### 🖆 IPv4

IPv4 uses 32-bit addresses.

Example:

```text
192.168.1.10
```

An IPv4 address is commonly represented using four decimal octets.

IPv4 provides addressing and packet delivery across IP networks.

### 🖆 IPv6

IPv6 uses 128-bit addresses.

Example:

```text
2001:db8::10
```

IPv6 provides a much larger address space and introduces improvements to IP addressing and packet handling.

### 🖆 Routing

Routers use destination IP addresses and routing tables to determine where packets should be forwarded.

Example:

```text
Client
192.168.1.10
     |
     |
     v
Default Gateway
192.168.1.1
     |
     |
     v
Router Network
     |
     |
     v
Remote Server
203.0.113.10
```

When the destination is outside the client's local subnet, the client normally sends the packet to its configured default gateway.

The router examines the destination IP address and selects an appropriate forwarding path.

### 🖆 IP Packet

An IP packet contains a header and payload.

The IPv4 header includes information such as:

* Source IP address.
* Destination IP address.
* Time To Live (TTL).
* Protocol identifier.
* Total length.
* Header checksum.

The IPv6 header includes:

* Source IP address.
* Destination IP address.
* Hop Limit.
* Next Header.
* Payload Length.
* Traffic Class and Flow Label.

The Internet layer PDU is commonly called an IP packet or IP datagram.

# 🗁 Network Access Layer

The Network Access layer handles communication between devices over a local network link.

It combines the responsibilities commonly associated with the OSI Data Link and Physical layers.

### 🖆 Main Responsibilities

* Local network delivery.
* Frame creation and processing.
* Link-layer addressing.
* Media access control.
* Error detection.
* Physical signal transmission.
* Communication over wired and wireless media.

### 🖆 Common Technologies

| Technology | Purpose |
| --- | --- |
| Ethernet | Wired local area networking |
| Wi-Fi | Wireless local area networking |
| PPP | Point-to-point communication |
| Fiber Ethernet | Ethernet communication over optical fiber |
| VLAN | Logical Layer 2 network segmentation |
| IEEE 802.1Q | VLAN tagging in Ethernet frames |

### 🖆 Ethernet

Ethernet is widely used for wired local area networks.

It defines frame formats and link-layer communication behavior, along with physical-layer specifications in the IEEE 802.3 family.

An Ethernet frame includes:

* Destination MAC address.
* Source MAC address.
* EtherType or length field.
* Payload.
* Frame Check Sequence.

An optional VLAN tag may also be present.

### 🖆 Wi-Fi

Wi-Fi provides wireless local area networking based on the IEEE 802.11 standards.

Wireless devices communicate with access points or other wireless stations using radio signals.

Wi-Fi handles link-layer communication and physical transmission.

### 🖆 MAC Addressing

A MAC address is a link-layer address associated with a network interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

Ethernet switches use MAC addresses to make local frame-forwarding decisions.

### 🖆 Network Access PDU

The Data Link portion of the Network Access layer uses frames.

The Physical portion transmits bits.

# 🗁 TCP/IP Encapsulation

Encapsulation is the process of adding protocol-specific information as data moves down the TCP/IP layers.

Each layer prepares the data for delivery through the next lower layer.

### 🖆 Encapsulation Process

Consider an application sending data using HTTP over TCP over IPv4 and Ethernet.

```text
Application Layer
+--------------------------+
| HTTP Data                |
+--------------------------+
             |
             v
Transport Layer
+--------------------------+
| TCP Header               |
| HTTP Data                |
+--------------------------+
             |
             v
Internet Layer
+--------------------------+
| IPv4 Header              |
| TCP Segment              |
+--------------------------+
             |
             v
Network Access Layer
+--------------------------+
| Ethernet Header          |
| IPv4 Packet              |
| Ethernet Trailer         |
+--------------------------+
             |
             v
Physical Transmission
+--------------------------+
| Bits                     |
+--------------------------+
```

### 🖆 Encapsulation Summary

| TCP/IP Layer | Added Information | Result |
| --- | --- | --- |
| Application | Application-specific data | Data |
| Transport | TCP or UDP header | Segment / Datagram |
| Internet | IP header | IP Packet |
| Network Access | Link-layer header and trailer | Frame |
| Physical transmission | Signal representation | Bits |

### 🖆 Decapsulation

At the receiving device, the process occurs in reverse.

The receiving system:

* Receives physical signals and reconstructs bits.
* Processes the link-layer frame.
* Processes the IP packet.
* Delivers the payload to the appropriate transport protocol.
* Delivers application data to the correct application endpoint.

```text
Received Bits
     |
     v
Ethernet Frame
     |
     v
IP Packet
     |
     v
TCP Segment / UDP Datagram
     |
     v
Application Data
```

### 🖆 Encapsulation Example

Suppose a client sends an HTTPS request using TCP over IPv4.

The encapsulated data contains:

| Layer | Information |
| --- | --- |
| Application | HTTP request protected by TLS |
| Transport | TCP source and destination ports |
| Internet | Source and destination IP addresses |
| Network Access | Source and destination MAC addresses on the local link |
| Physical | Bits represented through the transmission medium |

The actual headers and payload depend on the protocols and configuration being used.

# 🗁 TCP/IP Communication Process

TCP/IP communication involves several cooperating protocols.

Consider a client accessing a remote website.

### 🖆 Example Network

```text
Client
IP: 192.168.1.10
     |
     |
     v
Router
LAN: 192.168.1.1
WAN: 198.51.100.2
     |
     |
     v
Internet
     |
     |
     v
Web Server
IP: 203.0.113.10
```

The addresses in this example use documentation ranges and are not intended to represent real public endpoints.

### 🖆 Communication Steps

**Application request**

The user enters a website address into the browser.

**DNS resolution**

The client resolves the domain name to an IP address if it does not already have a usable cached result.

**Destination determination**

The operating system determines whether the destination IP address is on the local subnet.

**Default gateway selection**

If the destination is remote, the client selects its default gateway.

**Link-layer resolution**

For IPv4 Ethernet communication, the client may use ARP to discover the gateway's MAC address if it is not already known.

For IPv6, Neighbor Discovery performs the corresponding link-layer address resolution.

**Transport communication**

The application communicates using its selected transport protocol.

For HTTPS over HTTP/1.1 or HTTP/2, this commonly involves TCP. HTTP/3 uses QUIC over UDP.

**IP packet forwarding**

Routers forward IP packets toward their destinations using routing information.

**Server processing**

The destination server receives the data and delivers it to the appropriate application.

**Response delivery**

The server sends a response through the network, and the client processes it through its networking stack.

### 🖆 Important Addressing Behavior

During ordinary routing without NAT:

* Source and destination IP addresses normally remain the same across routed links.
* Source and destination MAC addresses are replaced for each new Ethernet link.
* The IP TTL or IPv6 Hop Limit decreases at each forwarding router.

When NAT is used, IP addresses and sometimes transport-layer port numbers can also be modified.

# 🗁 TCP/IP Addressing

TCP/IP communication uses multiple types of addressing.

Each type serves a different purpose.

### 🖆 IP Address

An IP address identifies a network-layer interface.

Examples:

```text
IPv4: 192.168.1.10
IPv6: 2001:db8::10
```

IP addresses support communication across different networks.

### 🖆 MAC Address

A MAC address is a link-layer address used for communication over a local network technology.

Example:

```text
00:1A:2B:3C:4D:5E
```

Ethernet uses MAC addresses for local frame delivery.

### 🖆 Port Number

A port number identifies a transport-layer endpoint.

Examples:

```text
TCP 443 - HTTPS
TCP 22  - SSH
UDP 53  - DNS
```

Ports help the operating system deliver incoming transport data to the appropriate application process.

### 🖆 Addressing Comparison

| Address Type | Layer | Purpose |
| --- | --- | --- |
| IP Address | Internet | Identifies a network-layer endpoint |
| MAC Address | Network Access | Supports local link-layer delivery |
| Port Number | Transport | Identifies an application endpoint |

### 🖆 How the Addresses Work Together

When a computer accesses a remote server:

* The destination IP address identifies the remote network-layer destination.
* The destination MAC address identifies the next-hop device on the local link.
* The destination port identifies the application service on the remote host.

These addressing mechanisms cooperate to deliver data to the correct destination.

# 🗁 TCP/IP Protocol Suite

The TCP/IP protocol suite contains protocols that operate at different layers.

### 🖆 Application Protocols

| Protocol | Purpose |
| --- | --- |
| HTTP | Web communication |
| HTTPS | Secure web communication |
| DNS | Domain name resolution |
| DHCP | Automatic IP configuration |
| FTP | File transfer |
| SSH | Secure remote access |
| SMTP | Email transfer |
| IMAP | Email access |
| POP3 | Email retrieval |
| NTP | Time synchronization |
| SNMP | Network monitoring and management |
| Syslog | Log message transport and collection |

### 🖆 Transport Protocols

| Protocol | Purpose |
| --- | --- |
| TCP | Reliable byte-stream transport |
| UDP | Connectionless datagram transport |
| SCTP | Reliable message-oriented transport |
| QUIC | Secure multiplexed transport built over UDP |

QUIC is implemented above UDP and provides features such as connection establishment, encryption integration, stream multiplexing, and loss recovery.

### 🖆 Internet Protocols

| Protocol | Purpose |
| --- | --- |
| IPv4 | IPv4 addressing and packet delivery |
| IPv6 | IPv6 addressing and packet delivery |
| ICMP | IPv4 diagnostics and error reporting |
| ICMPv6 | IPv6 diagnostics and control messaging |
| IPsec | IP communication security |

### 🖆 Network Access Protocols and Technologies

| Technology | Purpose |
| --- | --- |
| Ethernet | Wired LAN communication |
| Wi-Fi | Wireless LAN communication |
| PPP | Point-to-point communication |
| ARP | IPv4-to-link-layer address resolution on applicable networks |
| NDP | IPv6 neighbor discovery |
| IEEE 802.1Q | VLAN tagging |

ARP and NDP are closely associated with local link operation and IP networking. Their exact layer classification can vary between networking references.

# 🗁 TCP/IP vs OSI Model

The OSI and TCP/IP models both describe network communication through layers.

However, they have different structures and purposes.

### 🖆 Layer Mapping

| OSI Model | TCP/IP Model |
| --- | --- |
| Application | Application |
| Presentation | Application |
| Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link | Network Access |
| Physical | Network Access |

### 🖆 Key Differences

| Feature | OSI Model | TCP/IP Model |
| --- | --- | --- |
| Number of layers | Seven | Four |
| Origin | ISO | DARPA research and Internet protocol development |
| Main purpose | Conceptual reference model | Internet protocol architecture |
| Layer separation | More detailed | More consolidated |
| Protocol dependency | Protocol-independent model | Based on the TCP/IP protocol suite |
| Practical role | Learning, design, and troubleshooting | Real-world Internet communication |
| Network layer | Network | Internet |
| Lower layers | Separate Data Link and Physical | Combined Network Access |

### 🖆 When to Use Each Model

Use the OSI model to:

* Learn networking fundamentals.
* Understand networking responsibilities.
* Classify protocols and technologies.
* Troubleshoot network problems.
* Communicate networking concepts.

Use the TCP/IP model to:

* Understand Internet architecture.
* Study IP-based communication.
* Work with TCP, UDP, IPv4, and IPv6.
* Understand application communication.
* Build and troubleshoot real networks.

# 🗁 Practical Linux Commands

Linux provides networking utilities for inspecting and troubleshooting TCP/IP communication.

### 🖆 Inspect Network Interfaces

```bash
# Display network interfaces
ip link show

# Display interface addresses
ip addr show

# Display detailed interface information
ip -details link show
```

### 🖆 Inspect IP Configuration and Routing

```bash
# Display IPv4 routing table
ip route show

# Display IPv6 routing table
ip -6 route show

# Display the default gateway
ip route show default

# Display the selected route to a destination
ip route get 8.8.8.8
```

### 🖆 Test Network Connectivity

```bash
# Test connectivity to a remote IP address
ping -c 4 8.8.8.8

# Test connectivity using a domain name
ping -c 4 example.com

# Trace the network path
traceroute example.com
```

### 🖆 Inspect Transport Connections

```bash
# Display listening TCP and UDP sockets
ss -tuln

# Display established TCP connections
ss -tn state established

# Display all TCP sockets
ss -ta

# Display UDP sockets
ss -ua
```

### 🖆 Test Application Services

```bash
# Query DNS
dig example.com

# Display HTTP response headers
curl -I https://example.com

# Test TCP port connectivity
nc -vz example.com 443
```

### 🖆 Capture Network Traffic

```bash
# Capture packets on all interfaces
sudo tcpdump -i any

# Capture packets on a specific interface
sudo tcpdump -i eth0

# Capture TCP traffic
sudo tcpdump -i any tcp

# Capture UDP traffic
sudo tcpdump -i any udp

# Capture DNS traffic
sudo tcpdump -i any port 53

# Capture HTTPS traffic
sudo tcpdump -i any port 443
```

These commands help inspect different aspects of TCP/IP communication. They do not each belong exclusively to a single TCP/IP layer.

# 🗁 Common Mistakes

| Mistake | Correct Understanding |
| --- | --- |
| Thinking TCP/IP means only TCP and IP | It is a complete protocol suite containing many protocols. |
| Assuming TCP is always used for web communication | HTTP/3 uses QUIC over UDP. |
| Assuming UDP guarantees delivery | UDP itself does not provide delivery acknowledgments or retransmission. |
| Confusing IP addresses with MAC addresses | IP addresses support logical network addressing; MAC addresses support link-layer delivery. |
| Confusing port numbers with IP addresses | Ports identify transport endpoints, not network interfaces. |
| Assuming routers forward Ethernet frames unchanged | Routers process incoming frames and create new link-layer frames for outgoing links. |
| Assuming the TCP/IP model has exactly the same layers as OSI | The commonly used TCP/IP model has four layers. |
| Assuming every protocol fits into only one conceptual layer | Some protocols and technologies span multiple layers. |
| Assuming a successful ping proves an application is working | ICMP reachability does not guarantee that an application service is available. |
| Assuming all Internet communication uses IPv4 | Modern networks use both IPv4 and IPv6. |

# 🗁 Best Practices

* Understand the responsibilities of all four TCP/IP layers.
* Learn the difference between TCP and UDP.
* Understand IP addressing and subnetting.
* Learn how routers forward IP packets.
* Understand the role of MAC addresses in local delivery.
* Practice inspecting network interfaces and routing tables.
* Learn how DNS resolution works.
* Use `ss`, `ip`, `ping`, `dig`, and `tcpdump` for troubleshooting.
* Practice packet analysis with Wireshark.
* Understand the relationship between TCP/IP and the OSI model.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify TCP/IP Layers

Identify the primary TCP/IP layer associated with each protocol or technology.

| Protocol / Technology | Expected Layer |
| --- | --- |
| HTTP | Application |
| DNS | Application |
| DHCP | Application |
| SSH | Application |
| TCP | Transport |
| UDP | Transport |
| SCTP | Transport |
| IPv4 | Internet |
| IPv6 | Internet |
| ICMP | Internet |
| Ethernet | Network Access |
| Wi-Fi | Network Access |

### 🖆 Exercise: Analyze Addressing

Consider this network:

```text
Client
IP: 192.168.1.10
MAC: 00:11:22:33:44:55
     |
     |
     v
Router
IP: 192.168.1.1
MAC: AA:BB:CC:DD:EE:FF
     |
     |
     v
Remote Server
IP: 203.0.113.10
Port: TCP 443
```

Answer these questions:

* Which address identifies the remote server at the Internet layer?
* Which MAC address is used as the Ethernet destination on the client's first hop?
* Which transport-layer port identifies the HTTPS service?
* Which device forwards packets between the networks?
* What happens to the Ethernet source and destination addresses at the router?

### 🖆 Exercise: Inspect TCP/IP Configuration

Run the following commands on Ubuntu:

```bash
ip link show
ip addr show
ip route show
ip route get 8.8.8.8
ss -tuln
```

Identify:

* Your network interface names.
* Your configured IP addresses.
* Your default gateway.
* The outgoing interface selected for a remote destination.
* The listening TCP and UDP ports.

### 🖆 Exercise: Analyze DNS and HTTP

Run:

```bash
dig example.com
curl -I https://example.com
```

Observe:

* How DNS resolves a domain name.
* Which IP address is returned.
* How the HTTP server responds.
* Which application-level information is visible.

### 🖆 Exercise: Capture TCP/IP Traffic

Use `tcpdump`:

```bash
sudo tcpdump -i any -nn -c 30
```

Generate traffic by opening a website or running:

```bash
curl https://example.com
```

Inspect:

* Source and destination IP addresses.
* TCP or UDP source and destination ports.
* DNS traffic.
* TCP connection establishment.
* Packet direction.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is the TCP/IP model?**

The TCP/IP model is a four-layer networking architecture that describes how devices communicate using the Internet protocol suite.

**What does TCP/IP stand for?**

Transmission Control Protocol / Internet Protocol.

**How many layers are in the commonly used TCP/IP model?**

Four layers: Application, Transport, Internet, and Network Access.

**Why is TCP/IP important?**

It provides the protocol architecture used for communication across modern IP networks and the Internet.

### 🖆 Layer-Specific Questions

**Which TCP/IP layer handles application protocols?**

The Application layer.

**Which TCP/IP layer uses port numbers?**

The Transport layer.

**Which TCP/IP layer handles IP addressing and routing?**

The Internet layer.

**Which TCP/IP layer handles local network delivery?**

The Network Access layer.

**Which TCP/IP layer contains TCP and UDP?**

The Transport layer.

**Which TCP/IP layer contains IPv4 and IPv6?**

The Internet layer.

### 🖆 Technical Questions

**What is the difference between TCP and UDP?**

TCP provides reliable, ordered byte-stream transport, while UDP provides connectionless, message-oriented datagram transport without built-in delivery guarantees.

**What is the difference between an IP address and a MAC address?**

An IP address provides logical network-layer addressing, while a MAC address supports link-layer delivery over a local network technology.

**What is a port number?**

A port number identifies a transport-layer endpoint associated with an application process.

**What is encapsulation?**

Encapsulation is the process of adding protocol-specific information as data moves down the networking stack.

**What is the role of a router in TCP/IP communication?**

A router forwards IP packets between networks using its routing information.

**Why does a router change Ethernet MAC addresses?**

Each routed link uses its own link-layer framing. The router removes the incoming frame and creates a new frame for the outgoing link.

**Does UDP guarantee packet delivery?**

No. UDP does not provide built-in acknowledgments, retransmission, or ordering guarantees.

**Can TCP/IP work over Ethernet and Wi-Fi?**

Yes. IP communication can operate over different network access technologies, including Ethernet and Wi-Fi.

**What is the relationship between TCP/IP and the OSI model?**

Both describe networking functions through layers. The TCP/IP model has four commonly used layers, while the OSI model has seven conceptual layers.

# 🗁 References

* [RFC 1122 — Requirements for Internet Hosts: Communication Layers](https://www.rfc-editor.org/rfc/rfc1122)
* [RFC 1123 — Requirements for Internet Hosts: Application and Support](https://www.rfc-editor.org/rfc/rfc1123)
* [RFC 9293 — Transmission Control Protocol (TCP)](https://www.rfc-editor.org/rfc/rfc9293)
* [RFC 768 — User Datagram Protocol (UDP)](https://www.rfc-editor.org/rfc/rfc768)
* [RFC 791 — Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)
* [RFC 8200 — Internet Protocol, Version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 9000 — QUIC: A UDP-Based Multiplexed and Secure Transport](https://www.rfc-editor.org/rfc/rfc9000)
* [RFC 1034 — Domain Names: Concepts and Facilities](https://www.rfc-editor.org/rfc/rfc1034)
* [RFC 1035 — Domain Names: Implementation and Specification](https://www.rfc-editor.org/rfc/rfc1035)
* [Wireshark Documentation](https://www.wireshark.org/docs/)
* [Linux `ip` Command Manual](https://man7.org/linux/man-pages/man8/ip.8.html)
* [Linux `ss` Command Manual](https://man7.org/linux/man-pages/man8/ss.8.html)
