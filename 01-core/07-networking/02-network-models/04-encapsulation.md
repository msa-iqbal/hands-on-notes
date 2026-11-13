# 🗁 Network Encapsulation and Decapsulation

**Encapsulation** and **decapsulation** are fundamental processes that allow data to travel across networks using multiple communication protocols.

When data is sent from an application, it passes through the networking stack. Each layer adds protocol-specific information, such as headers and sometimes trailers. This information allows receiving devices and intermediate network devices to process and deliver the data correctly.

At the destination, the receiving device performs the reverse operation, removing and processing protocol information layer by layer.

* **Encapsulation:** Adding protocol information as data moves down the networking stack.
* **Decapsulation:** Processing and removing protocol information as data moves up the networking stack.

These processes are essential to understanding Ethernet, IP, TCP, UDP, HTTP, and other networking protocols.

# 🗁 Encapsulation

Encapsulation is the process of wrapping data with protocol-specific information as it moves from higher networking layers to lower networking layers.

Each layer adds information required by the corresponding protocol.

For example, when a browser sends an HTTP request over TCP/IP and Ethernet:

* The Application layer produces application data.
* The Transport layer adds a TCP header.
* The Internet layer adds an IP header.
* The Data Link layer adds an Ethernet header and trailer.
* The Physical layer transmits the resulting frame as bits.

### 🖆 Encapsulation Diagram

```text
             Application Layer
             +----------------+
             | Application    |
             | Data           |
             +----------------+
                      |
                      v
              Transport Layer
             +----------------+
             | TCP Header     |
             | Application    |
             | Data           |
             +----------------+
                      |
                      v
               Internet Layer
             +----------------+
             | IP Header      |
             | TCP Segment    |
             +----------------+
                      |
                      v
             Network Access Layer
             +----------------+
             | Ethernet Header|
             | IP Packet      |
             | Ethernet Trailer
             +----------------+
                      |
                      v
                Physical Layer
             0101010101010101...
```

Each layer treats the data received from the layer above as its payload and adds the information required for its own operation.

### 🖆 Why Encapsulation Is Necessary

Encapsulation allows different networking protocols to work together.

It provides:

* **Addressing:** Identifies source and destination devices or processes.
* **Routing:** Provides information required to forward packets across networks.
* **Delivery:** Helps deliver data to the correct application.
* **Error detection:** Enables detection of certain transmission errors.
* **Reliability:** Supports reliable delivery when provided by protocols such as TCP.
* **Protocol identification:** Helps receiving systems determine how to process incoming data.

# 🗁 Decapsulation

Decapsulation is the reverse of encapsulation.

When data arrives at a destination device, it moves upward through the networking stack. Each layer processes its corresponding protocol information and removes its header or trailer when appropriate.

The remaining payload is passed to the next higher layer.

### 🖆 Decapsulation Diagram

```text
                 Physical Layer
                       |
                       v
               Received Ethernet Frame
                       |
                       v
             Network Access Layer
             +----------------+
             | Ethernet Header|
             | IP Packet      |
             | Ethernet Trailer
             +----------------+
                       |
              Process Ethernet
              Remove Ethernet
              Header and Trailer
                       |
                       v
                 Internet Layer
             +----------------+
             | IP Header      |
             | TCP Segment    |
             +----------------+
                       |
                 Process IP
                 Remove IP Header
                       |
                       v
                Transport Layer
             +----------------+
             | TCP Header     |
             | Application    |
             | Data           |
             +----------------+
                       |
                 Process TCP
                 Remove TCP Header
                       |
                       v
                Application Layer
             +----------------+
             | Application    |
             | Data           |
             +----------------+
```

### 🖆 Why Decapsulation Is Necessary

Decapsulation allows the receiving device to:

* Identify the protocol used by the incoming data.
* Verify addressing information.
* Check protocol-specific fields.
* Identify the correct destination application.
* Reconstruct the original application data.
* Deliver the data to the appropriate process.

# 🗁 Encapsulation Process

Consider a client sending an HTTP request to a remote web server.

The communication uses HTTP, TCP, IP, and Ethernet.

### 🖆 Application Layer

The application generates the HTTP request.

Example:

```http
GET /index.html HTTP/1.1
Host: example.com
Accept: text/html
```

At this stage, the data contains application-level information.

### 🖆 Transport Layer

TCP receives the application data and adds a TCP header.

The header contains information such as:

* Source port.
* Destination port.
* Sequence number.
* Acknowledgment number.
* Control flags.
* Window size.
* Checksum.

The resulting unit is called a **TCP segment**.

```text
+-------------------------+
| TCP Header              |
+-------------------------+
| HTTP Request Data       |
+-------------------------+
```

### 🖆 Internet Layer

IP receives the TCP segment and adds an IP header.

The IP header contains information such as:

* Source IP address.
* Destination IP address.
* Time to Live (TTL) or Hop Limit.
* Protocol or Next Header information.
* Packet length.
* Other control fields.

The resulting unit is an **IP packet** or **IP datagram**.

```text
+-------------------------+
| IP Header               |
+-------------------------+
| TCP Segment             |
|                         |
+-------------------------+
```

### 🖆 Data Link Layer

The Data Link layer receives the IP packet and encapsulates it inside a link-layer frame.

For Ethernet, the frame includes:

* Destination MAC address.
* Source MAC address.
* EtherType or length field.
* Payload.
* Frame Check Sequence (FCS).

The resulting unit is an **Ethernet frame**.

```text
+-------------------------+
| Ethernet Header         |
+-------------------------+
| IP Packet               |
+-------------------------+
| Ethernet Trailer (FCS)  |
+-------------------------+
```

### 🖆 Physical Layer

The Physical layer transmits the frame as a sequence of bits over the selected medium.

Depending on the technology, transmission may use:

* Electrical signals over copper.
* Light pulses over fiber.
* Radio waves over wireless networks.

# 🗁 Protocol Data Units

A **Protocol Data Unit (PDU)** is the unit of data exchanged or processed by a protocol at a particular layer.

Different layers use different PDU names.

| Layer | Protocol Data Unit | Example |
| --- | --- | --- |
| Application | Data / Message | HTTP request |
| Transport | Segment / Datagram | TCP segment / UDP datagram |
| Internet | Packet / IP Datagram | IPv4 packet |
| Data Link | Frame | Ethernet frame |
| Physical | Bits | Binary transmission |

### 🖆 Data

At the Application layer, the information is commonly called data or a message.

Examples include:

* HTTP requests.
* DNS queries.
* SMTP messages.
* SSH communication data.

### 🖆 Segment

At the Transport layer, TCP divides application data into transport segments.

TCP uses sequence numbers and acknowledgment information to support reliable, ordered delivery.

### 🖆 Datagram

UDP uses datagrams as its transport-layer PDU.

UDP provides connectionless communication without TCP's built-in retransmission and ordering mechanisms.

The term *datagram* is also used for IP packets, so its meaning depends on the protocol layer being discussed.

### 🖆 Packet

At the Internet layer, IP encapsulates transport-layer data into IP packets.

Routers examine IP headers to determine where packets should be forwarded.

### 🖆 Frame

At the Data Link layer, packets are encapsulated into frames for transmission across a particular network link.

Ethernet and Wi-Fi use different frame formats.

### 🖆 Bits

At the Physical layer, the frame is transmitted as signals representing binary data.

The actual signaling mechanism depends on the physical technology.

# 🗁 Headers and Trailers

Headers and trailers contain protocol-specific information required for communication.

### 🖆 Headers

A header is control information placed before a protocol's payload.

Examples:

| Header | Important Information |
| --- | --- |
| Ethernet Header | Source MAC, destination MAC, EtherType |
| IPv4 Header | Source IP, destination IP, TTL, protocol |
| TCP Header | Ports, sequence number, flags |
| UDP Header | Source port, destination port, length, checksum |

Headers help network devices and receiving applications process data correctly.

### 🖆 Trailers

A trailer is control information placed after a protocol's payload.

Ethernet uses a Frame Check Sequence (FCS) in its frame trailer.

The FCS uses a Cyclic Redundancy Check (CRC) to detect certain transmission errors.

If a receiving Ethernet interface detects an invalid FCS, it normally discards the frame.

Not every protocol uses a trailer. TCP and UDP, for example, do not use separate transport-layer trailers in the way Ethernet does.

# 🗁 Encapsulation Example

Consider the following hypothetical communication:

```text
Client:
IP Address: 192.168.1.10
MAC Address: AA:AA:AA:AA:AA:AA

Server:
IP Address: 203.0.113.20
MAC Address: BB:BB:BB:BB:BB:BB

Application:
HTTP over TCP

Source Port:
51514

Destination Port:
80
```

The client sends an HTTP request to the server.

### 🖆 Application Data

```http
GET / HTTP/1.1
Host: example.com
```

### 🖆 TCP Segment

TCP adds transport information:

```text
Source Port:      51514
Destination Port: 80
Sequence Number: 1000
Flags:            SYN (for connection establishment)
```

The example SYN segment is part of establishing the TCP connection. The HTTP request is sent after the connection has been established.

### 🖆 IP Packet

IP adds logical addressing:

```text
Source IP:      192.168.1.10
Destination IP: 203.0.113.20
Protocol:       TCP
TTL:            64
```

### 🖆 Ethernet Frame

Ethernet adds local link-layer addressing:

```text
Source MAC:      AA:AA:AA:AA:AA:AA
Destination MAC: Gateway MAC Address
EtherType:       IPv4
```

Notice that the destination MAC address is the gateway's MAC address rather than the remote server's MAC address.

This happens because the server is on another IP network. The client sends the Ethernet frame to its default gateway, which forwards the IP packet toward the server.

The actual MAC addresses and header values depend on the network configuration.

# 🗁 Encapsulation Across Network Devices

Encapsulation information does not remain identical throughout the entire network path.

Different devices process different protocol layers.

### 🖆 Switch Behavior

A traditional Layer 2 Ethernet switch examines Ethernet frame information.

It uses the destination MAC address to determine where to forward the frame.

When forwarding a frame within the same Ethernet network, the switch generally preserves the IP packet and its transport-layer payload.

It may forward the original frame unchanged or discard it depending on VLAN membership, filtering, errors, and forwarding rules.

### 🖆 Router Behavior

A router receives an Ethernet frame and processes its contents.

For a typical IPv4 forwarding operation:

* The router receives the frame.
* It validates and removes the incoming link-layer encapsulation.
* It examines the destination IP address.
* It performs a routing-table lookup.
* It decrements the IPv4 TTL.
* It updates the IPv4 header checksum.
* It encapsulates the IP packet in a new outgoing link-layer frame.
* It forwards the frame through the selected interface.

The source and destination IP addresses normally remain unchanged during ordinary routing, but the link-layer source and destination addresses change at each routed Ethernet hop.

NAT, tunneling, fragmentation, and other mechanisms can modify additional packet information.

### 🖆 Router Forwarding Diagram

```text
       Network A                         Network B

+------------------+              +------------------+
| Client           |              | Router           |
|                  |              |                  |
| IP: 192.168.1.10 |              |                  |
+------------------+              +------------------+
        |                                  |
        | Ethernet Frame                   |
        | Destination MAC: Router          |
        +--------------------------------->|
                                           |
                                    Remove Ethernet
                                    Encapsulation
                                           |
                                    Inspect IP Header
                                           |
                                    Routing Decision
                                           |
                                    Decrement TTL
                                           |
                                    Add New Ethernet
                                    Encapsulation
                                           |
                                           v
                                  +------------------+
                                  | Destination      |
                                  | Network          |
                                  +------------------+
```

### 🖆 Important Difference

At a router:

* The incoming Ethernet frame is removed.
* The IP packet is processed.
* A new outgoing link-layer frame is created.

At a traditional Layer 2 switch:

* The frame is forwarded based on Layer 2 information.
* The IP packet is not normally decapsulated and routed.

# 🗁 MTU and Fragmentation

The **Maximum Transmission Unit (MTU)** defines the maximum size of a network-layer packet that can be carried in a link-layer payload without fragmentation or other special handling.

For standard Ethernet, the default MTU is commonly 1500 bytes.

This means an Ethernet payload can carry an IP packet of up to 1500 bytes under standard Ethernet settings.

The Ethernet header and FCS are additional frame overhead and are not included in the 1500-byte IP MTU.

### 🖆 MTU Example

Assume:

```text
Ethernet MTU:       1500 bytes
IPv4 Header:          20 bytes
TCP Header:           20 bytes
```

The maximum TCP application payload for a basic IPv4/TCP packet without additional options is:

```text
1500 - 20 - 20 = 1460 bytes
```

This value is commonly known as the TCP Maximum Segment Size (MSS) for that configuration.

TCP options, IPv4 options, IPv6 headers, and tunneling overhead can affect the available payload size.

### 🖆 Fragmentation

Fragmentation occurs when an IP packet is too large for a network link and the applicable IP mechanism permits fragmentation.

**IPv4**

IPv4 routers can fragment packets when fragmentation is permitted and necessary. The Don't Fragment (DF) flag can prevent router fragmentation.

**IPv6**

IPv6 routers do not fragment packets. A source host can use the IPv6 Fragment header when fragmentation is necessary, typically after Path MTU Discovery.

### 🖆 Why MTU Matters

Incorrect MTU configuration can cause:

* Packet loss.
* Connectivity problems.
* Slow application performance.
* VPN connectivity issues.
* Path MTU Discovery problems.

# 🗁 Practical Packet Analysis

Packet analysis helps you observe encapsulation and decapsulation in real network traffic.

On Ubuntu, `tcpdump` can capture packets from network interfaces.

### 🖆 Install tcpdump

```bash
sudo apt update
sudo apt install tcpdump
```

### 🖆 List Available Interfaces

```bash
ip link show
```

### 🖆 Capture Network Traffic

```bash
sudo tcpdump -i any -nn
```

Options:

| Option | Meaning |
| --- | --- |
| `-i any` | Capture on available interfaces |
| `-nn` | Disable hostname and port-name resolution |

### 🖆 Capture HTTP Traffic

```bash
sudo tcpdump -i any -nn 'tcp port 80'
```

### 🖆 Capture HTTPS Traffic

```bash
sudo tcpdump -i any -nn 'tcp port 443'
```

### 🖆 Capture DNS Traffic

```bash
sudo tcpdump -i any -nn 'port 53'
```

### 🖆 Capture and Save Packets

```bash
sudo tcpdump -i any -nn -w capture.pcap
```

Read the saved capture:

```bash
tcpdump -nn -r capture.pcap
```

For detailed protocol inspection, open the capture file with Wireshark.

### 🖆 What to Observe

During packet analysis, identify:

* Ethernet source and destination MAC addresses.
* IP source and destination addresses.
* TCP or UDP source and destination ports.
* TCP flags such as SYN, ACK, and FIN.
* IP TTL or IPv6 Hop Limit.
* Ethernet frame boundaries.
* Encapsulation differences between local and routed traffic.

# 🗁 Common Misconceptions

**Encapsulation only adds headers.**

Not always. Some protocols add trailers as well. Ethernet, for example, uses an FCS trailer.

**Every networking layer adds a header.**

Not necessarily. The Physical layer transmits signals and does not add a conventional protocol header in the same sense as higher layers.

**The Ethernet header contains IP addresses.**

Incorrect. Ethernet uses MAC addresses. IP addresses are contained in the IP header.

**MAC addresses remain unchanged from source to destination.**

Incorrect. MAC addresses are link-local. Routers normally replace the incoming link-layer frame with a new outgoing frame.

**IP addresses always change at every router.**

Incorrect. In ordinary routing without NAT, source and destination IP addresses generally remain the same.

**Every packet has TCP information.**

Incorrect. IP can carry different transport or control protocols, including TCP, UDP, and ICMP.

**Decapsulation happens only at the final destination.**

Incorrect. Routers and other intermediate devices also process and remove link-layer encapsulation when forwarding traffic.

# 🗁 Best Practices

* Understand the responsibilities of each networking layer.
* Learn the difference between an Ethernet frame, IP packet, TCP segment, and UDP datagram.
* Remember that MAC addresses are used for local link delivery, while IP addresses support communication across networks.
* Understand how routers replace link-layer encapsulation during forwarding.
* Use packet-capture tools to inspect real protocol headers.
* Consider MTU and encapsulation overhead when troubleshooting VPNs and tunnels.
* Learn how TCP MSS relates to IP MTU.
* Practice identifying protocol headers in Wireshark.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify the PDU

Identify the correct protocol data unit for each layer.

| Layer | PDU |
| --- | --- |
| Application | |
| Transport (TCP) | |
| Transport (UDP) | |
| Internet (IP) | |
| Data Link (Ethernet) | |
| Physical | |

### 🖆 Exercise: Understand Encapsulation

Given the following information:

```text
Application: HTTP
Transport: TCP
Source IP: 192.168.10.20
Destination IP: 8.8.8.8
Source Port: 50000
Destination Port: 80
```

Explain:

* Which layer adds the TCP header?
* Which layer adds the IP header?
* Which layer adds the Ethernet header?
* Which destination MAC address will the client use when sending traffic to a remote network?
* Which fields does a router normally change when forwarding an IPv4 packet?

### 🖆 Exercise: Calculate TCP MSS

Assume:

```text
Ethernet MTU: 1500 bytes
IPv4 Header: 20 bytes
TCP Header: 20 bytes
```

Calculate the maximum TCP payload size without additional IP or TCP options.

### 🖆 Exercise: Capture Packets

Capture DNS and TCP traffic:

```bash
sudo tcpdump -i any -nn 'port 53 or tcp port 443'
```

Visit a website and inspect the captured traffic.

Identify:

* DNS request and response packets.
* TCP source and destination ports.
* TCP connection establishment.
* IP source and destination addresses.

### 🖆 Exercise: Inspect an Ethernet Frame

Use Wireshark to open a packet capture and expand:

```text
Ethernet II
Internet Protocol
Transmission Control Protocol
Application Protocol
```

Identify the header fields belonging to each protocol.

# 🗁 Interview Questions

**What is encapsulation in networking?**

Encapsulation is the process of adding protocol-specific headers and, where applicable, trailers to data as it moves down the networking stack.

**What is decapsulation?**

Decapsulation is the process of receiving, processing, and removing protocol information as data moves up the networking stack.

**What is a PDU?**

A Protocol Data Unit is the unit of data handled by a protocol at a particular networking layer.

**What is the difference between a packet and a frame?**

A packet is a Network or Internet layer PDU, while a frame is a Data Link layer PDU that carries a packet or other payload across a local link.

**What is the difference between a TCP segment and a UDP datagram?**

A TCP segment contains TCP header information and supports reliable, ordered communication. A UDP datagram contains a UDP header and provides connectionless transport without TCP's built-in reliability mechanisms.

**Why does a router create a new Ethernet frame?**

Because Ethernet addressing is specific to a local link. When forwarding a packet through another interface, the router uses the appropriate link-layer addressing for the outgoing network.

**What is the purpose of an Ethernet FCS?**

The Frame Check Sequence enables the receiving interface to detect certain transmission errors using a CRC calculation.

**What is MTU?**

MTU is the maximum size of a network-layer packet that can be carried in a link-layer payload without fragmentation or other special handling.

**What happens to the TTL when an IPv4 packet passes through a router?**

The router decrements the TTL by at least one. If the TTL reaches zero, the packet is discarded and an ICMP Time Exceeded message is normally generated.

**Does a switch perform decapsulation?**

A switch processes the link-layer information needed for forwarding. A traditional Layer 2 switch does not normally remove and route the IP packet, although it may inspect other protocol fields for additional features.

# 🗁 References

* [RFC 1122 — Requirements for Internet Hosts: Communication Layers](https://www.rfc-editor.org/rfc/rfc1122)
* [RFC 791 — Internet Protocol (IPv4)](https://www.rfc-editor.org/rfc/rfc791)
* [RFC 8200 — Internet Protocol, Version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 9293 — Transmission Control Protocol (TCP)](https://www.rfc-editor.org/rfc/rfc9293)
* [RFC 768 — User Datagram Protocol (UDP)](https://www.rfc-editor.org/rfc/rfc768)
* [RFC 894 — Standard for the Transmission of IP Datagrams over Ethernet Networks](https://www.rfc-editor.org/rfc/rfc894)
* [Wireshark Documentation](https://www.wireshark.org/docs/)
* [tcpdump Manual](https://www.tcpdump.org/manpages/tcpdump.1.html)
