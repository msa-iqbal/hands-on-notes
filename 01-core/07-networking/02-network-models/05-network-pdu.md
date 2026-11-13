# 🗁 Network Protocol Data Unit (PDU)

A **Protocol Data Unit (PDU)** is a unit of data exchanged or processed by a networking protocol at a particular layer of the network architecture.

Each networking layer has its own responsibilities and uses protocol-specific information to communicate with the corresponding layer on another device.

As data moves through the networking stack, its structure changes. Headers and trailers are added, processed, and sometimes removed.

Understanding PDUs helps explain:

* How data travels through network layers.
* How protocols communicate with each other.
* The difference between data, segments, packets, and frames.
* How encapsulation and decapsulation work.
* How to analyze network traffic using packet-capture tools.

# 🗁 What Is a PDU?

A PDU is the formatted unit of information handled by a specific protocol layer.

For example, when a computer sends data using HTTP over TCP/IP and Ethernet, the data passes through multiple layers.

Each layer processes the information and prepares it for the next layer.

The common PDU names are:

* **Data:** Application-layer information.
* **Segment:** TCP transport-layer unit.
* **Datagram:** UDP transport-layer unit.
* **Packet:** IP network-layer unit.
* **Frame:** Data Link layer unit.
* **Bits:** Physical-layer transmission.

### 🖆 Why PDUs Matter

PDUs allow each networking layer to operate independently while supporting communication across the complete network stack.

For example:

* TCP uses sequence numbers and port numbers to manage transport communication.
* IP uses source and destination IP addresses to support communication across networks.
* Ethernet uses MAC addresses to deliver frames across a local link.
* Physical technologies transmit the resulting information as signals.

Each layer understands the information relevant to its protocol rather than needing to understand every detail of the application data.

# 🗁 PDU Names by Network Layer

The PDU name depends on the networking layer and protocol.

### 🖆 OSI Model

| OSI Layer | Layer Name | Common PDU |
| --- | --- | --- |
| 7 | Application | Data / Message |
| 6 | Presentation | Data |
| 5 | Session | Data |
| 4 | Transport | Segment / Datagram |
| 3 | Network | Packet |
| 2 | Data Link | Frame |
| 1 | Physical | Bits |

### 🖆 TCP/IP Model

| TCP/IP Layer | Common PDU | Examples |
| --- | --- | --- |
| Application | Data / Message | HTTP request, DNS query |
| Transport | Segment / Datagram | TCP, UDP |
| Internet | Packet / Datagram | IPv4, IPv6 |
| Network Access | Frame | Ethernet, Wi-Fi |
| Physical functions | Bits | Electrical, optical, radio signals |

The TCP/IP model commonly uses four layers, with physical transmission included in the Network Access layer. The five-layer educational representation separates Data Link and Physical functions.

### 🖆 Important Terminology

The term **datagram** can refer to different protocol data units depending on context:

* A UDP datagram is a Transport layer PDU.
* An IP datagram is an Internet layer PDU.

Always identify the protocol when using this term.

# 🗁 Application Layer PDU

The Application layer provides network services to application processes.

Its PDU is commonly called **data** or a **message**.

Examples include:

* HTTP requests and responses.
* DNS queries and responses.
* SMTP messages.
* SSH application data.
* DHCP messages.

### 🖆 HTTP Message Example

```http
GET /index.html HTTP/1.1
Host: example.com
Accept: text/html
```

This represents an HTTP request message.

The HTTP message becomes payload for a lower-layer protocol, such as TCP.

### 🖆 DNS Message Example

A DNS query contains information such as:

* Transaction ID.
* Query flags.
* Question count.
* Requested domain name.
* Record type.

Example:

```text
Query Name: example.com
Query Type: A
Class: IN
```

The DNS message is commonly transported using UDP, although DNS also uses TCP in several situations.

# 🗁 Transport Layer PDU

The Transport layer provides communication between application processes running on different hosts.

Its PDU depends on the transport protocol.

### 🖆 TCP Segment

TCP uses a PDU called a **segment**.

A TCP segment contains:

* Source port.
* Destination port.
* Sequence number.
* Acknowledgment number.
* Header length.
* Control flags.
* Window size.
* Checksum.
* Optional TCP options.
* Payload.

Simplified structure:

```text
+-----------------------------+
| TCP Header                  |
|                             |
| Source Port                 |
| Destination Port            |
| Sequence Number             |
| Acknowledgment Number       |
| Flags                       |
| Window Size                 |
| Checksum                    |
+-----------------------------+
| Application Data            |
+-----------------------------+
```

TCP uses sequence numbers and acknowledgments to support reliable, ordered byte-stream delivery.

A TCP segment does not necessarily correspond to one complete application message. TCP transports a continuous byte stream and does not preserve application message boundaries.

### 🖆 UDP Datagram

UDP uses a PDU called a **datagram**.

A UDP header is smaller and simpler than a TCP header.

It contains four fields:

| Field | Size |
| --- | ---: |
| Source Port | 16 bits |
| Destination Port | 16 bits |
| Length | 16 bits |
| Checksum | 16 bits |

Simplified structure:

```text
+-----------------------------+
| UDP Header                  |
|                             |
| Source Port                 |
| Destination Port            |
| Length                      |
| Checksum                    |
+-----------------------------+
| Application Data            |
+-----------------------------+
```

UDP provides connectionless transport. It does not provide TCP-style retransmission, ordering, or flow control.

Applications can implement their own reliability mechanisms when required.

### 🖆 TCP Segment vs UDP Datagram

| Feature | TCP Segment | UDP Datagram |
| --- | --- | --- |
| Protocol | TCP | UDP |
| Connection Model | Connection-oriented | Connectionless |
| Header Size | Minimum 20 bytes | 8 bytes |
| Reliability | Built-in | Not built-in |
| Ordering | Built-in | Not built-in |
| Flow Control | Yes | No |
| Congestion Control | Yes | No built-in mechanism |
| Common Uses | HTTP/1.1, SSH, SMTP | DNS queries, DHCP, real-time media |

# 🗁 Internet Layer PDU

The Internet layer of the TCP/IP model is responsible for logical addressing and packet delivery across interconnected networks.

Its PDU is commonly called an **IP packet** or **IP datagram**.

IP packets contain:

* Source IP address.
* Destination IP address.
* Packet length.
* Protocol information.
* TTL or Hop Limit.
* Other protocol-specific fields.

### 🖆 IPv4 Packet Structure

A simplified IPv4 packet:

```text
+-----------------------------+
| IPv4 Header                 |
|                             |
| Version                     |
| Header Length               |
| Total Length                |
| Identification              |
| Flags                       |
| Fragment Offset             |
| TTL                         |
| Protocol                    |
| Header Checksum             |
| Source IP Address           |
| Destination IP Address      |
+-----------------------------+
| Payload                     |
| TCP / UDP / ICMP / Other    |
+-----------------------------+
```

The IPv4 header has a minimum size of 20 bytes and can be larger when options are present.

### 🖆 IPv6 Packet Structure

IPv6 uses a fixed-size base header of 40 bytes.

Simplified structure:

```text
+-----------------------------+
| IPv6 Base Header            |
|                             |
| Version                     |
| Traffic Class               |
| Flow Label                  |
| Payload Length              |
| Next Header                 |
| Hop Limit                   |
| Source Address              |
| Destination Address         |
+-----------------------------+
| Payload                     |
| TCP / UDP / ICMPv6 / Other  |
+-----------------------------+
```

IPv6 uses extension headers for additional functionality rather than adding optional fields directly to the base header.

### 🖆 IP Packet Responsibilities

IP provides:

* Logical addressing.
* Routing and forwarding.
* Packet delivery across networks.
* Best-effort delivery.
* Fragmentation behavior according to the IP version.

IP itself does not guarantee that packets will arrive, arrive only once, or arrive in order.

# 🗁 Data Link Layer PDU

The Data Link layer handles communication over a local network link.

Its PDU is called a **frame**.

A frame commonly contains:

* Destination link-layer address.
* Source link-layer address.
* Protocol identification or length information.
* Payload.
* Error-detection information where provided.

### 🖆 Ethernet Frame Structure

A simplified Ethernet frame:

```text
+-----------------------------+
| Destination MAC Address     |
+-----------------------------+
| Source MAC Address          |
+-----------------------------+
| EtherType / Length          |
+-----------------------------+
| Payload                     |
| Usually an IP Packet        |
+-----------------------------+
| Frame Check Sequence (FCS)  |
+-----------------------------+
```

A standard Ethernet frame has:

| Field | Size |
| --- | ---: |
| Destination MAC | 6 bytes |
| Source MAC | 6 bytes |
| EtherType / Length | 2 bytes |
| Payload | 46–1500 bytes for standard Ethernet |
| FCS | 4 bytes |

These values describe the conventional Ethernet frame format without VLAN tagging. VLAN tags add additional header information.

The Ethernet frame is transmitted over the physical medium.

### 🖆 Why Frames Are Important

Frames provide local network delivery.

For example, when a computer sends an IP packet to a remote network, it creates an Ethernet frame addressed to its default gateway.

The destination MAC address identifies the next-hop device on the local Ethernet network, not necessarily the final destination host.

# 🗁 Physical Layer PDU

The Physical layer transmits information over a physical medium.

Its PDU is commonly called **bits**.

Bits represent binary values:

```text
10101010 11001100 11110000
```

The physical medium represents these bits using signals.

Examples:

| Medium | Transmission Method |
| --- | --- |
| Copper Ethernet | Electrical signals |
| Fiber-optic cable | Light signals |
| Wi-Fi | Radio signals |

The Physical layer deals with characteristics such as:

* Signal encoding.
* Modulation.
* Transmission frequency.
* Voltage levels.
* Optical signaling.
* Symbol timing.
* Physical connectors and interfaces.

The exact physical representation depends on the networking technology.

# 🗁 PDU Encapsulation Process

Encapsulation occurs when data moves from higher layers toward lower layers.

Each layer adds its own protocol information.

Consider an application sending data through TCP/IP over Ethernet.

### 🖆 Step-by-Step Structure

**Application Layer**

The application generates data.

```text
Application Data
```

**Transport Layer**

TCP adds a TCP header.

```text
TCP Header + Application Data
```

The result is a TCP segment.

**Internet Layer**

IP adds an IP header.

```text
IP Header + TCP Segment
```

The result is an IP packet.

**Data Link Layer**

Ethernet adds a frame header and FCS.

```text
Ethernet Header + IP Packet + FCS
```

The result is an Ethernet frame.

**Physical Layer**

The frame is transmitted as bits.

```text
010101010101010101010101...
```

### 🖆 Complete Encapsulation Diagram

```text
+-----------------------------+
| Application Data            |
+-----------------------------+
              |
              v
+-----------------------------+
| TCP Header                  |
| Application Data            |
+-----------------------------+
              |
              v
+-----------------------------+
| IP Header                   |
| TCP Header                  |
| Application Data            |
+-----------------------------+
              |
              v
+-----------------------------+
| Ethernet Header             |
| IP Header                   |
| TCP Header                  |
| Application Data            |
| Ethernet FCS                |
+-----------------------------+
              |
              v
| 010101010101010101010101... |
              Bits
```

# 🗁 PDU Decapsulation Process

Decapsulation occurs when received data moves from lower layers toward higher layers.

Each layer processes the protocol information relevant to it.

### 🖆 Step-by-Step Structure

**Physical Layer**

Receives signals and reconstructs the transmitted bits.

**Data Link Layer**

Processes the Ethernet frame, checks the FCS, and identifies the payload protocol.

**Internet Layer**

Processes the IP header and identifies the next protocol, such as TCP or UDP.

**Transport Layer**

Processes the TCP or UDP header and identifies the destination application using the port information.

**Application Layer**

Receives the application data and processes it according to the application protocol.

### 🖆 Complete Decapsulation Diagram

```text
Received Bits
      |
      v
+-----------------------------+
| Ethernet Frame              |
| Header + IP Packet + FCS    |
+-----------------------------+
      |
      v
Remove Ethernet Encapsulation
      |
      v
+-----------------------------+
| IP Packet                   |
| IP Header + TCP Segment     |
+-----------------------------+
      |
      v
Process and Remove IP Header
      |
      v
+-----------------------------+
| TCP Segment                 |
| TCP Header + Application    |
+-----------------------------+
      |
      v
Process TCP Header
      |
      v
+-----------------------------+
| Application Data            |
+-----------------------------+
```

# 🗁 PDU vs SDU

Another important networking concept is the **Service Data Unit (SDU)**.

A PDU and an SDU describe different perspectives of data at a networking layer.

* **SDU:** Data received from the layer above.
* **PDU:** Data formatted by the current layer, including its protocol control information.

A layer receives an SDU, adds its own protocol information, and produces a PDU.

### 🖆 Example

Suppose the Transport layer receives application data.

```text
Application Data
      |
      v
Transport Layer
      |
      +----------------------+
      | TCP Header           |
      +----------------------+
      | Application Data     |
      +----------------------+
      |
      v
TCP Segment
```

In this example:

| Concept | Meaning |
| --- | --- |
| Transport SDU | Application data received from above |
| Transport PDU | TCP segment |
| Protocol Control Information (PCI) | TCP header |

### 🖆 General Relationship

```text
        Layer N+1
            |
            | SDU
            v
        +----------------+
        |    Layer N     |
        |                |
        | Add PCI        |
        +----------------+
            |
            | PDU
            v
        Layer N-1
```

This distinction is useful when studying protocol architecture and how individual layers provide services to one another.

# 🗁 PDU Size and Network Overhead

Every protocol header consumes space in a packet.

This information is important when calculating MTU, TCP MSS, and total transmission overhead.

### 🖆 Example: Ethernet with IPv4 and TCP

Assume:

```text
Ethernet MTU:       1500 bytes
IPv4 Header:          20 bytes
TCP Header:           20 bytes
```

The maximum TCP payload is:

```text
1500 - 20 - 20 = 1460 bytes
```

The resulting IP packet is:

```text
IPv4 Header:          20 bytes
TCP Header:           20 bytes
TCP Payload:        1460 bytes
-----------------------------
Total IP Packet:    1500 bytes
```

The Ethernet header and FCS are additional frame overhead.

For an untagged Ethernet frame:

```text
Ethernet Header:      14 bytes
IP Packet:          1500 bytes
Ethernet FCS:          4 bytes
-----------------------------
Frame Size:         1518 bytes
```

This calculation excludes the Ethernet preamble, Start Frame Delimiter, and interpacket gap because they are not part of the conventional Ethernet frame-size measurement.

### 🖆 Why Overhead Matters

Protocol overhead affects:

* Network efficiency.
* Maximum payload size.
* Throughput.
* Packet processing.
* VPN and tunnel MTU calculations.
* Fragmentation and Path MTU Discovery.

# 🗁 Practical Packet Analysis

Packet-capture tools allow you to inspect PDUs in real network traffic.

On Ubuntu, you can use `tcpdump` or Wireshark.

### 🖆 Install tcpdump

```bash
sudo apt update
sudo apt install tcpdump
```

### 🖆 Capture Traffic

```bash
sudo tcpdump -i any -nn
```

### 🖆 Capture TCP Traffic

```bash
sudo tcpdump -i any -nn tcp
```

### 🖆 Capture UDP Traffic

```bash
sudo tcpdump -i any -nn udp
```

### 🖆 Capture DNS Traffic

```bash
sudo tcpdump -i any -nn port 53
```

### 🖆 Save a Packet Capture

```bash
sudo tcpdump -i any -nn -w network-capture.pcap
```

Read the capture:

```bash
tcpdump -nn -r network-capture.pcap
```

### 🖆 Inspect with Wireshark

Open the capture file in Wireshark and expand the protocol layers.

For example:

```text
Frame
  Ethernet II
    Internet Protocol Version 4
      Transmission Control Protocol
        Hypertext Transfer Protocol
```

Each section exposes information associated with a particular protocol.

You can identify:

* Frame length.
* MAC addresses.
* IP addresses.
* TCP or UDP ports.
* Protocol-specific headers.
* Application data when it is available for inspection.

Encrypted application payloads, such as HTTPS traffic, are not normally readable as plain HTTP without the required decryption information.

# 🗁 Common Misconceptions

**A PDU is always called a packet.**

Incorrect. The PDU name depends on the protocol layer. TCP uses segments, UDP uses datagrams, Ethernet uses frames, and the Physical layer transmits bits.

**TCP and UDP use the same PDU name.**

Incorrect. TCP uses segments, while UDP uses datagrams.

**A frame and a packet are identical.**

Incorrect. A frame is a Data Link layer PDU. An IP packet is an Internet or Network layer PDU carried inside the frame.

**The Physical layer adds a header to every frame.**

Incorrect. The Physical layer transmits encoded signals representing the frame. It does not add a conventional network-layer header.

**Every PDU contains an Ethernet header.**

Incorrect. Ethernet headers are used when Ethernet is the link-layer technology. Wi-Fi and other technologies use their own framing formats.

**A TCP segment always contains one complete HTTP request.**

Incorrect. TCP provides a byte stream and does not preserve application message boundaries. An HTTP message may be split across multiple TCP segments or multiple messages may be carried through the same connection.

**PDU and SDU mean the same thing.**

Incorrect. An SDU is the data received from the layer above, while a PDU is the unit produced after the current layer adds its protocol control information.

# 🗁 Best Practices

* Memorize the common PDU names and understand what each represents.
* Distinguish TCP segments from UDP datagrams.
* Understand the difference between packets and frames.
* Learn how headers and trailers contribute to PDU structure.
* Understand how MTU affects packet size.
* Use Wireshark to inspect real protocol data units.
* Remember that PDU terminology can vary slightly between protocol specifications and educational materials.
* Identify the actual protocol before assigning a PDU name.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify the PDU

Complete the table.

| Protocol or Layer | PDU |
| --- | --- |
| HTTP | |
| TCP | |
| UDP | |
| IPv4 | |
| IPv6 | |
| Ethernet | |
| Physical transmission | |

### 🖆 Exercise: Understand Encapsulation

Consider this communication:

```text
Application: HTTP
Transport: TCP
Network: IPv4
Data Link: Ethernet
```

Explain:

* What is the Application layer PDU?
* What is the Transport layer PDU?
* What is the Internet layer PDU?
* What is the Data Link layer PDU?
* What is transmitted by the Physical layer?

### 🖆 Exercise: Calculate Frame Size

Assume:

```text
Ethernet Header: 14 bytes
IPv4 Header:      20 bytes
TCP Header:       20 bytes
TCP Payload:    1000 bytes
Ethernet FCS:      4 bytes
```

Calculate:

* TCP segment size.
* IPv4 packet size.
* Ethernet frame size.

### 🖆 Exercise: Inspect PDUs with Wireshark

Capture network traffic and identify:

* Ethernet frame.
* IP packet.
* TCP segment or UDP datagram.
* Application message, when visible.

Explain which header fields belong to each protocol.

### 🖆 Exercise: Compare TCP and UDP

Create a comparison table containing:

* Header size.
* PDU name.
* Reliability.
* Connection behavior.
* Common applications.

# 🗁 Interview Questions

**What is a Protocol Data Unit?**

A PDU is a formatted unit of information handled by a particular networking protocol layer.

**What is the PDU of the Transport layer?**

TCP uses segments, while UDP uses datagrams.

**What is the PDU of the Network layer?**

A packet, commonly called an IP packet or IP datagram.

**What is the PDU of the Data Link layer?**

A frame.

**What is the PDU of the Physical layer?**

Bits.

**What is the difference between a segment and a packet?**

A segment is a Transport layer PDU, while a packet is a Network or Internet layer PDU.

**What is the difference between a packet and a frame?**

A packet contains Network layer information and is carried as payload inside a link-layer frame.

**What is the difference between a PDU and an SDU?**

An SDU is the data received from the layer above. A PDU is the formatted unit produced by the current layer after adding protocol control information.

**Why is Ethernet overhead important?**

Ethernet headers and trailers consume transmission space and must be considered when calculating frame size and network efficiency.

**Can TCP data be divided across multiple segments?**

Yes. TCP transports a byte stream and can divide application data into multiple segments according to transport and network constraints.

# 🗁 References

* [RFC 1122 — Requirements for Internet Hosts: Communication Layers](https://www.rfc-editor.org/rfc/rfc1122)
* [RFC 791 — Internet Protocol (IPv4)](https://www.rfc-editor.org/rfc/rfc791)
* [RFC 8200 — Internet Protocol, Version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 9293 — Transmission Control Protocol (TCP)](https://www.rfc-editor.org/rfc/rfc9293)
* [RFC 768 — User Datagram Protocol (UDP)](https://www.rfc-editor.org/rfc/rfc768)
* [RFC 894 — Standard for the Transmission of IP Datagrams over Ethernet Networks](https://www.rfc-editor.org/rfc/rfc894)
* [Wireshark Documentation](https://www.wireshark.org/docs/)
* [tcpdump Manual](https://www.tcpdump.org/manpages/tcpdump.1.html)
