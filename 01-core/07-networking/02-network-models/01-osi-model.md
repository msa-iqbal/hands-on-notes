# 🗁 OSI Model

The **Open Systems Interconnection (OSI) Model** is a conceptual reference model that explains how data travels between devices over a network.

Developed by the International Organization for Standardization (ISO), the OSI model divides network communication into seven logical layers. Each layer has specific responsibilities and interacts with the layers above and below it.

The model provides a standardized way to understand networking technologies, communication protocols, and troubleshooting processes.

The seven layers are:

* Application
* Presentation
* Session
* Transport
* Network
* Data Link
* Physical

Understanding the OSI model is essential for learning network protocols, IP addressing, Ethernet, switching, routing, firewalls, and network troubleshooting.

# 🗁 OSI Model Fundamentals

The OSI model describes network communication by dividing it into seven layers.

Each layer performs a particular function and provides services to the layer above it.

This separation makes networking easier to understand because engineers can study individual responsibilities without needing to understand the entire communication process at once.

### 🖆 Why Was the OSI Model Developed?

Before standardized networking architectures became widely established, different vendors developed networking systems with their own communication methods.

These systems did not always communicate with one another.

The OSI model introduced a common conceptual framework that helped standardize networking terminology, responsibilities, and interoperability concepts.

Its primary goals include:

* Providing a common networking reference model.
* Supporting communication between different systems.
* Separating networking responsibilities into layers.
* Simplifying protocol design and implementation.
* Making network architecture easier to understand.
* Providing a structured approach to troubleshooting.

### 🖆 Seven-Layer Overview

| Layer | Name | Main Responsibility | PDU |
| --- | --- | --- | --- |
| 7 | Application | Network services for applications | Data |
| 6 | Presentation | Data representation and transformation | Data |
| 5 | Session | Session establishment and management | Data |
| 4 | Transport | End-to-end transport | Segment / Datagram |
| 3 | Network | Logical addressing and routing | Packet |
| 2 | Data Link | Framing and local delivery | Frame |
| 1 | Physical | Physical bit transmission | Bits |

### 🖆 OSI Model Diagram

```text
+--------------------------------------+
| 7. APPLICATION                       |
|    Application-level network services|
+--------------------------------------+
| 6. PRESENTATION                      |
|    Data representation and encoding  |
+--------------------------------------+
| 5. SESSION                           |
|    Session establishment and control |
+--------------------------------------+
| 4. TRANSPORT                         |
|    End-to-end communication          |
+--------------------------------------+
| 3. NETWORK                           |
|    Logical addressing and routing    |
+--------------------------------------+
| 2. DATA LINK                         |
|    Framing and MAC addressing        |
+--------------------------------------+
| 1. PHYSICAL                          |
|    Transmission of bits              |
+--------------------------------------+
```

### 🖆 OSI Layer Mnemonics

A commonly used mnemonic for remembering the layers from Layer 7 to Layer 1 is:

**All People Seem To Need Data Processing**

| Word | OSI Layer |
| --- | --- |
| All | Application |
| People | Presentation |
| Seem | Session |
| To | Transport |
| Need | Network |
| Data | Data Link |
| Processing | Physical |

To remember the layers in reverse order, from Layer 1 to Layer 7:

**Please Do Not Throw Sausage Pizza Away**

| Word | OSI Layer |
| --- | --- |
| Please | Physical |
| Do | Data Link |
| Not | Network |
| Throw | Transport |
| Sausage | Session |
| Pizza | Presentation |
| Away | Application |

# 🗁 Seven Layers of the OSI Model

Each OSI layer has a specific role in network communication.

The layers cooperate to deliver data from a source application to a destination application.

# 🗁 Application Layer

<text>

</text>

The Application layer is Layer 7, the topmost layer of the OSI model.

It provides network communication services to application software.

It acts as the interface between network services and applications that need to exchange information.

**Main responsibilities:**

* Provides application-level network services.
* Supports communication between application processes.
* Defines application request and response formats.
* Supports file transfer and resource sharing.
* Provides services for web browsing, email, and name resolution.
* Defines application-level communication behavior.

### 🖆 Common Application-Layer Protocols

| Protocol | Purpose | Common Port |
| --- | --- | --- |
| HTTP | Web communication | TCP 80 |
| HTTPS | Secure web communication | TCP 443 |
| DNS | Domain name resolution | UDP/TCP 53 |
| DHCP | Automatic network configuration | UDP 67/68 |
| FTP | File transfer | TCP 20/21 |
| SMTP | Sending email | TCP 25 |
| POP3 | Receiving email | TCP 110 |
| IMAP | Email access and synchronization | TCP 143 |
| SSH | Secure remote access | TCP 22 |
| Telnet | Remote terminal access | TCP 23 |
| SNMP | Network management | UDP 161/162 |

Port numbers are common defaults. Actual deployments can use different ports or transport protocols.

### 🖆 Practical Example

Suppose you open a website:

```text
https://example.com
```

Your browser uses an application protocol such as HTTP over a secure transport to communicate with the web server.

The Application layer is responsible for the application-level request and response semantics.

### 🖆 Important Concepts

* Application protocols define how applications exchange information.
* DNS translates domain names into IP addresses.
* HTTP defines web request and response messages.
* SMTP handles email submission and transfer.
* SSH provides secure remote access.

**Remember:** The Application layer does not mean the browser or application itself is entirely part of Layer 7. It refers to the networking functions and protocols used by applications.

# 🗁 Presentation Layer

The Presentation layer is Layer 6 of the OSI model.

It handles data representation and transformation so that communicating systems can interpret exchanged information correctly.

It focuses on how data is represented, encoded, compressed, or encrypted.

**Main responsibilities:**

* Data format conversion.
* Character encoding.
* Data serialization.
* Data compression and decompression.
* Data encryption and decryption concepts.
* Data representation compatibility.

### 🖆 Common Technologies and Formats

| Technology / Format | Purpose |
| --- | --- |
| UTF-8 | Character encoding |
| ASCII | Character encoding |
| JSON | Structured data representation |
| XML | Structured data representation |
| JPEG | Image encoding |
| PNG | Image encoding |
| gzip | Data compression |
| ASN.1 | Data representation and encoding rules |

### 🖆 Practical Example

Suppose an application sends JSON data:

```json
{
  "username": "admin",
  "status": "active"
}
```

The data must be represented in an agreed format and character encoding so the receiving application can interpret it correctly.

UTF-8 is commonly used to encode the text.

### 🖆 Encryption and Compression

The Presentation layer is often associated with encryption and compression.

For example:

* Compression reduces the size of transmitted data.
* Character encoding represents text as bytes.
* Encryption transforms readable information into ciphertext.

However, real-world implementations do not always have a separate Presentation layer.

TLS, for example, is commonly implemented as a protocol between application protocols and transport protocols in the TCP/IP architecture.

**Remember:** Layer 6 is a conceptual way to understand data representation and transformation, not a requirement that every network stack must implement a separate Presentation component.

# 🗁 Session Layer

The Session layer is Layer 5 of the OSI model.

It manages logical communication sessions between applications.

A session represents an organized interaction between communicating application endpoints.

**Main responsibilities:**

* Session establishment.
* Session maintenance.
* Session termination.
* Dialog control.
* Synchronization.
* Recovery and checkpoint management.

### 🖆 Session Establishment

Before two applications exchange information, they may establish a logical session.

A simplified process looks like this:

```text
Client Application       Server Application
       |                         |
       |------ Start Session --->|
       |                         |
       |<----- Session Ready ----|
       |                         |
       |------ Exchange Data --->|
       |                         |
       |<----- Response ---------|
       |                         |
       |------ End Session ----->|
       |                         |
```

### 🖆 Practical Example

Consider a user logging into a web application.

The application may:

* Authenticate the user.
* Create a session.
* Associate subsequent requests with that session.
* Maintain the session state.
* Terminate the session when the user logs out or the session expires.

Session cookies and server-side session storage are application-level mechanisms commonly used for this purpose.

### 🖆 Session-Related Technologies

Examples of technologies and concepts associated with session management include:

* Remote Procedure Call (RPC).
* Session management mechanisms.
* Dialog control.
* Synchronization and checkpoints.

**Important:** TCP connections and application sessions are not identical.

A TCP connection provides a transport mechanism. An application session represents application-level interaction and can sometimes span multiple transport connections.

# 🗁 Transport Layer

The Transport layer is Layer 4 of the OSI model.

It provides communication between application processes running on different devices.

It uses port numbers to identify application endpoints.

**Main responsibilities:**

* Process-to-process communication.
* Segmentation and reassembly.
* Port addressing.
* Connection management.
* Reliable delivery when supported.
* Flow control when supported.
* Congestion control when supported.
* Error detection and recovery mechanisms.

### 🖆 Common Transport Protocols

| Protocol | Description |
| --- | --- |
| TCP | Connection-oriented and reliable byte-stream transport |
| UDP | Connectionless and message-oriented transport |
| SCTP | Reliable message-oriented transport |

### 🖆 TCP

Transmission Control Protocol provides reliable, connection-oriented communication.

Its features include:

* Connection establishment.
* Ordered byte-stream delivery.
* Acknowledgments.
* Retransmission.
* Flow control.
* Congestion control.

Example:

```text
Client                         Server
  |                               |
  |---------- SYN --------------->|
  |                               |
  |<--------- SYN-ACK ------------|
  |                               |
  |---------- ACK --------------->|
  |                               |
  |       Connection Ready        |
```

This is the TCP three-way handshake.

### 🖆 UDP

User Datagram Protocol provides a connectionless, message-oriented transport service.

Its characteristics include:

* No connection establishment handshake.
* No built-in delivery acknowledgment.
* No built-in retransmission.
* No built-in ordering guarantee.
* Low protocol overhead.

UDP is commonly used by DNS, real-time communication, streaming, and QUIC.

### 🖆 Port Numbers

Port numbers identify application endpoints at the transport layer.

Examples:

| Application | Common Port | Transport |
| --- | ---: | --- |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| DNS | 53 | UDP/TCP |
| SSH | 22 | TCP |
| SMTP | 25 | TCP |
| DHCP Server | 67 | UDP |
| DHCP Client | 68 | UDP |

### 🖆 Transport Layer PDU

| Protocol | PDU |
| --- | --- |
| TCP | Segment |
| UDP | Datagram |
| SCTP | Packet / Chunk-based message structure |

**Remember:** The Transport layer identifies application endpoints using port numbers, while the Network layer identifies network interfaces using IP addresses.

# 🗁 Network Layer

The Network layer is Layer 3 of the OSI model.

It provides logical addressing and packet delivery between different networks.

It allows devices to communicate even when they are not connected to the same local network.

**Main responsibilities:**

* Logical addressing.
* Routing.
* Packet forwarding.
* Path selection.
* Packet fragmentation where supported.
* Network-level error reporting and diagnostics.

### 🖆 Common Network-Layer Protocols

| Protocol | Purpose |
| --- | --- |
| IPv4 | Internet Protocol version 4 |
| IPv6 | Internet Protocol version 6 |
| ICMP | Network diagnostics and error reporting |
| ICMPv6 | IPv6 diagnostics and control messaging |
| IPsec | Network-layer security architecture |

### 🖆 IP Addressing

IP addresses identify network-layer interfaces and support packet delivery across interconnected networks.

Example IPv4 address:

```text
192.168.1.10
```

Example IPv6 address:

```text
2001:db8::10
```

A router uses destination IP addresses and its routing table to determine where packets should be forwarded.

### 🖆 Routing

Routing is the process of determining paths through interconnected networks.

Example:

```text
Computer A
192.168.1.10
     |
     |
     v
Router
192.168.1.1
     |
     |
     v
Remote Network
     |
     |
     v
Server B
203.0.113.10
```

The router examines the destination IP address and selects an appropriate forwarding path.

### 🖆 Network-Layer Devices

* Routers.
* Layer 3 switches.
* Multilayer switches.

### 🖆 Network-Layer PDU

The Network layer PDU is called a **Packet**.

**Remember:** IP addresses provide logical addressing, while routers use routing information to forward packets between networks.

# 🗁 Data Link Layer

The Data Link layer is Layer 2 of the OSI model.

It provides communication across a directly connected network link or local network segment.

It organizes data into frames and uses link-layer addressing for local delivery.

**Main responsibilities:**

* Framing.
* MAC addressing.
* Local frame delivery.
* Media access control.
* Error detection.
* VLAN tagging.
* Link-level flow control in technologies that support it.

### 🖆 Two Sublayers

In the IEEE 802 architecture, the Data Link layer is commonly divided into two sublayers.

| Sublayer | Full Name | Responsibility |
| --- | --- | --- |
| LLC | Logical Link Control | Logical link services and protocol multiplexing |
| MAC | Media Access Control | MAC addressing and access to the transmission medium |

### 🖆 MAC Addressing

A MAC address is a link-layer address associated with a network interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

Ethernet switches use source MAC addresses to learn which devices are reachable through particular ports.

They use destination MAC addresses to make frame-forwarding decisions.

### 🖆 Ethernet Framing

An Ethernet frame contains:

* Destination MAC address.
* Source MAC address.
* Optional VLAN tag.
* EtherType or length field.
* Payload.
* Frame Check Sequence (FCS).

Simplified Ethernet frame structure:

```text
+---------------------------+
| Destination MAC           |
+---------------------------+
| Source MAC                |
+---------------------------+
| Optional 802.1Q VLAN Tag  |
+---------------------------+
| EtherType / Length         |
+---------------------------+
| Payload                   |
+---------------------------+
| Frame Check Sequence      |
+---------------------------+
```

The VLAN tag is present when the frame is VLAN-tagged using IEEE 802.1Q.

### 🖆 Common Data Link Technologies

| Technology | Purpose |
| --- | --- |
| Ethernet | Wired LAN communication |
| Wi-Fi (IEEE 802.11) | Wireless LAN communication |
| PPP | Point-to-point communication |
| IEEE 802.1Q | VLAN tagging |
| STP | Prevention of Layer 2 switching loops |
| LACP | Link aggregation control |

### 🖆 Data Link Devices

* Layer 2 switches.
* Network bridges.
* Wireless access points operating in bridging mode.

**Remember:** Layer 2 handles local frame delivery. A switch does not normally route traffic between different IP networks unless it also performs Layer 3 routing.

# 🗁 Physical Layer

The Physical layer is Layer 1, the bottommost layer of the OSI model.

It defines how raw bits are transmitted over a physical communication medium.

It deals with the physical and signaling characteristics of network communication.

**Main responsibilities:**

* Bit transmission.
* Signal encoding and modulation.
* Electrical characteristics.
* Optical transmission characteristics.
* Radio-frequency transmission.
* Physical connectors.
* Cable specifications.
* Signaling and synchronization.

### 🖆 Physical Transmission Media

| Medium | Description |
| --- | --- |
| Twisted-pair copper | Uses pairs of twisted copper wires |
| Coaxial cable | Uses a central conductor surrounded by insulation and shielding |
| Fiber-optic cable | Transmits information using light |
| Radio waves | Supports wireless communication |
| Microwave | Supports directional wireless transmission |
| Infrared | Uses infrared light for communication |

### 🖆 Physical Network Components

* Ethernet cables.
* Fiber-optic cables.
* Network connectors.
* Repeaters.
* Hubs.
* Transceivers.
* Physical network interfaces.

### 🖆 Transmission Concepts

**Bandwidth**

The maximum theoretical or configured data-carrying capacity of a communication link, depending on context.

**Data Rate**

The number of bits transmitted per second.

**Signal Attenuation**

The reduction in signal strength as it travels through a medium.

**Interference**

Unwanted signals that affect communication quality.

**Duplex**

The direction in which communication can occur.

* Half-duplex: Communication can occur in both directions, but not simultaneously.
* Full-duplex: Communication can occur in both directions simultaneously.

### 🖆 Physical Layer PDU

The Physical layer transmits **Bits**.

```text
1011010010110101
```

These bits are represented through electrical, optical, or radio signals depending on the transmission technology.

**Remember:** The Physical layer defines how bits travel. It does not interpret IP addresses, MAC addresses, or application data.

# 🗁 OSI Layer Interaction

The seven OSI layers work together during communication.

Each layer provides services to the layer above it and uses services from the layer below it.

### 🖆 Communication Between Layers

Consider a client sending data to a remote server.

On the sending device, data moves downward through the networking stack.

On the receiving device, data moves upward through the networking stack.

```text
SENDER DEVICE                    RECEIVER DEVICE

Application  ---------------->  Application
     |                               ^
Presentation ---------------->  Presentation
     |                               ^
Session      ---------------->  Session
     |                               ^
Transport    ---------------->  Transport
     |                               ^
Network      ---------------->  Network
     |                               ^
Data Link    ---------------->  Data Link
     |                               ^
Physical     ===== Network ===> Physical
```

The horizontal arrows represent logical peer communication between corresponding layers.

Actual transmission takes place through the lower layers and the communication medium.

### 🖆 Vertical Communication

Vertical communication occurs between adjacent layers within the same device.

For example:

```text
Application
     |
     v
Transport
     |
     v
Network
     |
     v
Data Link
     |
     v
Physical
```

### 🖆 Horizontal Communication

Horizontal communication describes logical communication between corresponding protocol layers on different devices.

For example:

* TCP communicates logically between TCP endpoints.
* IP handles packet delivery between IP endpoints.
* Ethernet frames are exchanged between interfaces on a local link.

# 🗁 Data Encapsulation

Encapsulation is the process of adding protocol-specific control information as data moves down the networking layers.

Each layer prepares its data for the layer below it.

### 🖆 Encapsulation Process

Consider an application sending data using TCP, IPv4, and Ethernet.

```text
Application Layer
+----------------------+
| Application Data     |
+----------------------+
          |
          v
Transport Layer
+----------------------+
| TCP Header           |
| Application Data     |
+----------------------+
          |
          v
Network Layer
+----------------------+
| IPv4 Header          |
| TCP Segment          |
+----------------------+
          |
          v
Data Link Layer
+----------------------+
| Ethernet Header      |
| IPv4 Packet          |
| Ethernet Trailer     |
+----------------------+
          |
          v
Physical Layer
+----------------------+
| Bits                 |
+----------------------+
```

### 🖆 Encapsulation Details

| Layer | Added Information | Resulting PDU |
| --- | --- | --- |
| Application | Application-specific information | Data |
| Transport | TCP or UDP header | Segment / Datagram |
| Network | IP header | Packet |
| Data Link | Frame header and trailer | Frame |
| Physical | Physical signaling representation | Bits |

### 🖆 Decapsulation Process

At the receiving device, the process happens in reverse.

```text
Physical Layer
      Bits
       |
       v
Data Link Layer
     Frame
       |
       v
Network Layer
     Packet
       |
       v
Transport Layer
 Segment / Datagram
       |
       v
Application Layer
      Data
```

Each receiving layer processes the information relevant to its protocol and passes the remaining payload to the next layer.

For example, the IP layer processes the IP header and delivers the encapsulated transport data to the appropriate transport protocol.

# 🗁 OSI Model and Network Devices

Different networking devices primarily operate at different OSI layers.

However, modern devices often implement functionality across multiple layers.

### 🖆 Network Device Mapping

| Device | Primary OSI Layer | Main Function |
| --- | --- | --- |
| Hub | Layer 1 | Repeats physical signals |
| Repeater | Layer 1 | Regenerates signals |
| Layer 2 Switch | Layer 2 | Forwards frames using MAC addresses |
| Network Bridge | Layer 2 | Connects Layer 2 network segments |
| Wireless Access Point | Layer 2, with Layer 1 radio functions | Bridges wireless clients to a network |
| Router | Layer 3 | Forwards packets between networks |
| Layer 3 Switch | Layer 2 and Layer 3 | Switching and IP routing |
| Firewall | Varies | Enforces traffic security policies |
| Load Balancer | Varies | Distributes traffic across backend systems |

### 🖆 Layer 1 Devices

A hub or repeater works with physical signals.

It does not make forwarding decisions based on MAC or IP addresses.

### 🖆 Layer 2 Devices

A traditional Layer 2 switch uses MAC addresses to forward Ethernet frames.

It learns source MAC addresses and builds a MAC address table.

### 🖆 Layer 3 Devices

A router uses IP addressing and routing information to forward packets between networks.

### 🖆 Multi-Layer Devices

Modern firewalls, switches, and load balancers may inspect or process information from multiple networking layers.

For example, a firewall may use:

* Source and destination IP addresses.
* TCP or UDP ports.
* Connection state.
* Application-level information.

Therefore, avoid assuming that every device belongs exclusively to one OSI layer.

# 🗁 OSI Model in Network Troubleshooting

The OSI model provides a structured way to investigate networking problems.

Instead of randomly changing configurations, engineers can examine the communication process layer by layer.

### 🖆 Layer-Based Troubleshooting

| Layer | Common Problems | Diagnostic Methods |
| --- | --- | --- |
| Application | Application errors, DNS-related failures, HTTP errors | Browser tools, application logs, `curl`, `dig` |
| Presentation | Encoding, serialization, or data format problems | Inspect payload and application data |
| Session | Application session expiration or interruption | Session logs and application debugging |
| Transport | Connection failures, blocked ports, retransmissions | `ss`, `nc`, `tcpdump` |
| Network | Incorrect IP configuration, routing failures | `ip addr`, `ip route`, `ping`, `traceroute` |
| Data Link | VLAN mismatch, MAC learning issues, frame problems | `ip link`, `bridge`, `tcpdump` |
| Physical | Cable failure, interface down, signal problems | `ethtool`, interface status, physical inspection |

### 🖆 Example: Cannot Access a Website

Suppose a user reports that a website is not loading.

A systematic investigation might look like this:

**Physical layer**

* Check the network cable or wireless connection.
* Confirm the interface is operational.
* Inspect link status.

**Data Link layer**

* Check VLAN configuration.
* Verify local network connectivity.
* Inspect MAC address learning.

**Network layer**

* Check IP address configuration.
* Verify the default gateway.
* Inspect routing information.
* Test connectivity to a remote IP address.

**Transport layer**

* Check whether the destination TCP port is reachable.
* Inspect connection failures and retransmissions.

**Application layer**

* Check DNS resolution.
* Inspect HTTP responses.
* Review browser and server logs.

### 🖆 Example Troubleshooting Flow

```text
Start
  |
  v
Check Physical Connection
  |
  v
Check IP Configuration
  |
  v
Check Default Gateway
  |
  v
Check Remote IP Connectivity
  |
  v
Check DNS Resolution
  |
  v
Check TCP Port Connectivity
  |
  v
Check Application Response
  |
  v
Identify and Resolve Problem
```

This is a practical workflow, not a strict rule that every failure must be investigated in exactly this order.

# 🗁 Practical Linux Commands

Linux provides several networking utilities that help investigate different OSI-layer functions.

### 🖆 Physical and Data Link Inspection

```bash
# Display network interfaces
ip link show

# Display interface details
ip -details link show

# Display interface addresses
ip addr show

# Display Ethernet hardware information
sudo ethtool eth0

# Display bridge information
bridge link show

# Display forwarding database entries
bridge fdb show
```

### 🖆 Network Layer Inspection

```bash
# Display IP addresses
ip addr show

# Display routing table
ip route show

# Display IPv6 routing table
ip -6 route show

# Test local gateway connectivity
ping -c 4 192.168.1.1

# Trace the route to a destination
traceroute example.com
```

### 🖆 Transport Layer Inspection

```bash
# Display listening TCP and UDP sockets
ss -tuln

# Display active TCP connections
ss -tn

# Check whether a TCP port is reachable
nc -vz example.com 443
```

### 🖆 Application Layer Inspection

```bash
# Resolve a domain name
dig example.com

# Display DNS records
dig example.com A
dig example.com AAAA

# Send an HTTP request
curl -I https://example.com

# Display HTTP response headers
curl -v https://example.com
```

### 🖆 Capture Network Traffic

```bash
# Capture traffic on all interfaces
sudo tcpdump -i any

# Capture traffic on a specific interface
sudo tcpdump -i eth0

# Capture TCP traffic
sudo tcpdump -i any tcp

# Capture DNS traffic
sudo tcpdump -i any port 53

# Capture HTTPS traffic
sudo tcpdump -i any port 443
```

These commands do not map exclusively to individual OSI layers. Each provides visibility into particular networking functions.

# 🗁 Best Practices

* Understand the purpose of all seven layers.
* Learn the difference between logical and physical addressing.
* Understand the relationship between IP addresses, MAC addresses, and port numbers.
* Learn how routers and switches process network traffic.
* Practice packet analysis using Wireshark.
* Use Linux networking tools to investigate real network communication.
* Apply the OSI model as a troubleshooting framework.
* Understand that real-world protocols do not always fit perfectly into one conceptual layer.
* Learn the TCP/IP model alongside the OSI model.
* Practice identifying where protocol headers are added and processed.

# 🗁 Common Mistakes

| Mistake | Correct Understanding |
| --- | --- |
| Thinking the OSI model is a physical network architecture | It is a conceptual reference model. |
| Assuming all protocols belong exclusively to one layer | Some protocols and technologies span multiple layers. |
| Confusing MAC addresses with IP addresses | MAC addresses support link-layer delivery; IP addresses support logical addressing. |
| Assuming TCP and IP perform the same function | TCP provides transport services; IP provides network-layer addressing and packet delivery. |
| Thinking switches and routers are interchangeable | Layer 2 switches forward frames; routers forward packets between networks. |
| Assuming the Session layer is the same as a TCP connection | Application sessions and TCP connections are different concepts. |
| Assuming encryption always happens at Layer 6 | Modern encryption can be implemented at multiple layers. |
| Assuming every network problem belongs to only one layer | Networking failures can involve several layers simultaneously. |
| Assuming the OSI model directly describes every TCP/IP implementation | It is a conceptual model used to understand networking responsibilities. |

# 🗁 Practical Exercises

### 🖆 Exercise: Memorize the Seven Layers

Write the seven OSI layers in both directions.

* Layer 7 to Layer 1.
* Layer 1 to Layer 7.

For each layer, write its primary responsibility and PDU.

### 🖆 Exercise: Identify Protocol Layers

Identify the primary OSI layer associated with each item.

| Protocol / Technology | Expected Layer |
| --- | --- |
| HTTP | Application |
| DNS | Application |
| TLS | Often mapped between Application and Transport in TCP/IP discussions; not a strict OSI layer assignment |
| TCP | Transport |
| UDP | Transport |
| IPv4 | Network |
| IPv6 | Network |
| ICMP | Network |
| Ethernet | Data Link / Physical |
| Wi-Fi | Data Link / Physical |
| MAC address | Data Link |
| Fiber-optic cable | Physical |

### 🖆 Exercise: Analyze Network Communication

Consider the following scenario:

A computer with IP address `192.168.1.10` accesses a remote web server at `203.0.113.10` using HTTPS over TCP.

Identify:

* Which layer handles the application request?
* Which layer uses TCP port `443`?
* Which layer handles the destination IP address?
* Which layer handles the Ethernet destination MAC address?
* Which layer transmits the bits?
* What happens to the Ethernet frame when the packet passes through a router?

### 🖆 Exercise: Troubleshoot a Linux Network

Run these commands on Ubuntu:

```bash
ip link show
ip addr show
ip route show
ping -c 4 192.168.1.1
dig example.com
ss -tuln
curl -I https://example.com
```

For each command, identify the networking function being inspected and the OSI layers that are relevant.

### 🖆 Exercise: Analyze a Packet Capture

Install Wireshark on Ubuntu:

```bash
sudo apt update
sudo apt install wireshark
```

Capture network traffic while opening a website.

Inspect:

* Ethernet source and destination MAC addresses.
* IPv4 or IPv6 source and destination addresses.
* TCP source and destination ports.
* DNS queries and responses.
* TLS handshake information, where applicable.

Observe how Wireshark organizes packet information into protocol layers.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is the OSI model?**

The OSI model is a seven-layer conceptual reference model that describes how network communication functions are organized.

**Who developed the OSI model?**

The International Organization for Standardization (ISO) developed the OSI reference model.

**How many layers are in the OSI model?**

Seven layers.

**What are the seven OSI layers?**

Application, Presentation, Session, Transport, Network, Data Link, and Physical.

**Why is the OSI model important?**

It provides a common framework for understanding network communication, protocol responsibilities, and troubleshooting.

### 🖆 Layer-Specific Questions

**Which OSI layer handles application-level network services?**

Layer 7, Application.

**Which OSI layer handles data representation and encoding?**

Layer 6, Presentation.

**Which OSI layer manages logical application sessions?**

Layer 5, Session.

**Which OSI layer uses port numbers?**

Layer 4, Transport.

**Which OSI layer handles IP addressing and routing?**

Layer 3, Network.

**Which OSI layer handles MAC addressing?**

Layer 2, Data Link.

**Which OSI layer transmits raw bits?**

Layer 1, Physical.

### 🖆 Technical Questions

**What is the difference between a MAC address and an IP address?**

A MAC address is a link-layer address used for communication on a local network link, while an IP address is a logical network-layer address used for packet delivery across networks.

**What is the difference between a switch and a router?**

A Layer 2 switch forwards frames using MAC addresses, while a router forwards packets between networks using IP addresses and routing information.

**What is encapsulation?**

Encapsulation is the process of adding protocol-specific information as data moves down the networking stack.

**What is decapsulation?**

Decapsulation is the process of processing and removing protocol-specific information as data moves up the receiving networking stack.

**What is the PDU of the Transport layer?**

A TCP segment or UDP datagram, depending on the transport protocol.

**What is the PDU of the Network layer?**

A packet.

**What is the PDU of the Data Link layer?**

A frame.

**Can a firewall operate at multiple OSI layers?**

Yes. Modern firewalls can inspect Layer 3 and Layer 4 information and may also perform application-layer inspection.

**Does the OSI model describe the exact implementation of the Internet protocol stack?**

No. It is a conceptual reference model. The Internet primarily uses the TCP/IP architecture.

# 🗁 References

* [ISO/IEC 7498-1 — Information Technology: Open Systems Interconnection Basic Reference Model](https://www.iso.org/standard/20269.html)
* [RFC 1122 — Requirements for Internet Hosts: Communication Layers](https://www.rfc-editor.org/rfc/rfc1122)
* [RFC 1123 — Requirements for Internet Hosts: Application and Support](https://www.rfc-editor.org/rfc/rfc1123)
* [RFC 9293 — Transmission Control Protocol (TCP)](https://www.rfc-editor.org/rfc/rfc9293)
* [RFC 768 — User Datagram Protocol (UDP)](https://www.rfc-editor.org/rfc/rfc768)
* [RFC 791 — Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)
* [IEEE 802 Standards](https://standards.ieee.org/)
* [Wireshark Documentation](https://www.wireshark.org/docs/)
* [Linux `ip` Command Manual](https://man7.org/linux/man-pages/man8/ip.8.html)
* [Linux `ss` Command Manual](https://man7.org/linux/man-pages/man8/ss.8.html)
