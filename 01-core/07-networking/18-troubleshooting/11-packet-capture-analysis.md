# Packet Capture Analysis

Packet capture analysis is the process of recording, inspecting, and interpreting network packets to understand communication between devices.

It helps network engineers identify problems that cannot always be diagnosed using ping, traceroute, interface statistics, or application logs.

Packet captures provide visibility into:

* Source and destination IP addresses.
* Source and destination MAC addresses.
* TCP and UDP ports.
* Protocol information.
* Packet sizes and timing.
* TCP connection establishment and termination.
* DNS queries and responses.
* Retransmissions and packet loss symptoms.
* Network errors and protocol failures.
* Application-layer communication.

Common packet analysis tools include:

* Wireshark.
* TShark.
* tcpdump.
* dumpcap.
* Zeek.

# 🗁 How Packet Capture Works

A network interface receives and transmits frames. A packet capture tool observes traffic available to that interface and records it for analysis.

Basic workflow:

```text
Network Traffic
      |
      v
Network Interface
      |
      v
Packet Capture Tool
      |
      v
Capture File
      |
      v
Packet Analysis
      |
      v
Identify Network Problem
```

A capture on a normal switched Ethernet port generally sees traffic addressed to that device, broadcast traffic, and relevant multicast traffic. It does not automatically see every packet exchanged by other devices on the switch.

To observe traffic between other devices, use an appropriate switch mirror/SPAN port, network TAP, or another capture point.

# 🗁 Packet Capture Tools

| Tool | Purpose |
| --- | --- |
| tcpdump | Command-line packet capture and filtering |
| Wireshark | Graphical packet inspection and analysis |
| TShark | Command-line protocol analysis |
| dumpcap | Packet capture engine used by Wireshark |
| Zeek | Network security monitoring and protocol analysis |

**tcpdump** is useful for quick troubleshooting on Linux servers and routers.

**Wireshark** is useful for detailed protocol inspection, stream analysis, and visualizing conversations.

**TShark** is useful for automated analysis and scripting.

# 🗁 Installing Packet Capture Tools

Ubuntu or Debian:

```bash
sudo apt update
sudo apt install tcpdump wireshark tshark
```

Verify installation:

```bash
tcpdump --version
wireshark --version
tshark --version
```

On systems where packet capture permissions are restricted, run capture commands with appropriate privileges.

# 🗁 Identifying Network Interfaces

Before capturing traffic, identify the available interfaces.

```bash
ip link show
```

Display IP addresses:

```bash
ip address show
```

List interfaces recognized by tcpdump:

```bash
sudo tcpdump -D
```

Example interfaces:

```text
lo
eth0
ens33
wlan0
wg0
```

Choose the interface that carries the traffic being investigated.

The `any` interface can capture traffic across multiple Linux interfaces, but it may produce different link-layer information than capturing a specific Ethernet interface.

# 🗁 Basic tcpdump Commands

**Capture packets on a specific interface**

```bash
sudo tcpdump -i eth0
```

**Capture packets on all supported interfaces**

```bash
sudo tcpdump -i any
```

**Capture a limited number of packets**

```bash
sudo tcpdump -i eth0 -c 100
```

**Display packet contents in ASCII**

```bash
sudo tcpdump -i eth0 -A
```

**Display packet contents in hexadecimal and ASCII**

```bash
sudo tcpdump -i eth0 -X
```

**Disable hostname and service-name resolution**

```bash
sudo tcpdump -nn -i eth0
```

The `-nn` option prevents tcpdump from converting IP addresses and port numbers into names.

**Display timestamps**

```bash
sudo tcpdump -tttt -i eth0
```

# 🗁 Saving Packet Captures

Packet captures can be saved for later inspection.

**Save packets to a PCAP file**

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

**Capture a limited number of packets**

```bash
sudo tcpdump -i eth0 -c 500 -w capture.pcap
```

**Read an existing capture file**

```bash
tcpdump -r capture.pcap
```

**Read packets with detailed output**

```bash
tcpdump -nn -vvv -r capture.pcap
```

**Save only packets matching a filter**

```bash
sudo tcpdump -i eth0 'host 192.168.1.10' -w host-traffic.pcap
```

PCAP files can be opened directly in Wireshark.

# 🗁 Capture Filters

Capture filters determine which packets are recorded.

tcpdump uses Berkeley Packet Filter (BPF) syntax.

**Capture traffic from a specific host**

```bash
sudo tcpdump -i eth0 host 192.168.1.10
```

**Capture traffic to or from a host**

```bash
sudo tcpdump -i eth0 host 192.168.1.10
```

**Capture traffic from a source address**

```bash
sudo tcpdump -i eth0 src host 192.168.1.10
```

**Capture traffic to a destination address**

```bash
sudo tcpdump -i eth0 dst host 192.168.1.20
```

**Capture traffic on a specific network**

```bash
sudo tcpdump -i eth0 net 192.168.1.0/24
```

**Capture TCP traffic**

```bash
sudo tcpdump -i eth0 tcp
```

**Capture UDP traffic**

```bash
sudo tcpdump -i eth0 udp
```

**Capture traffic on a specific port**

```bash
sudo tcpdump -i eth0 port 443
```

**Capture DNS traffic**

```bash
sudo tcpdump -i eth0 port 53
```

**Capture HTTP traffic**

```bash
sudo tcpdump -i eth0 tcp port 80
```

**Capture traffic between two hosts**

```bash
sudo tcpdump -i eth0 'host 192.168.1.10 and host 192.168.1.20'
```

**Capture TCP traffic excluding SSH**

```bash
sudo tcpdump -i eth0 'tcp and not port 22'
```

**Capture traffic using multiple conditions**

```bash
sudo tcpdump -i eth0 '(host 192.168.1.10 or host 192.168.1.20) and tcp'
```

# 🗁 Capture Filter vs Display Filter

Capture filters and display filters serve different purposes.

| Feature | Capture Filter | Display Filter |
| --- | --- | --- |
| Applied | During packet capture | After packets are captured |
| Purpose | Controls which packets are recorded | Controls which captured packets are displayed |
| Syntax | BPF syntax | Wireshark display-filter syntax |
| Used by | tcpdump, dumpcap | Wireshark, TShark |

Example capture filter:

```bash
sudo tcpdump -i eth0 'tcp port 443'
```

Example Wireshark display filter:

```text
tcp.port == 443
```

A capture filter permanently excludes packets from the resulting capture. A display filter only hides packets temporarily from the current view.

# 🗁 Wireshark Interface

<text>Wireshark's interface contains several important areas:</text>

* **Packet List:** Displays captured packets.
* **Packet Details:** Displays decoded protocol fields for the selected packet.
* **Packet Bytes:** Displays raw packet bytes in hexadecimal and ASCII.
* **Display Filter Bar:** Filters packets shown in the packet list.
* **Status Bar:** Displays capture information and packet statistics.

<AsyncImage query="Wireshark network protocol analyzer interface packet list packet details packet bytes screenshot" aspectRatio="16:9" width="100%" />

# 🗁 Wireshark Display Filters

Display filters help isolate specific packets from a large capture.

**Filter by IP address**

```text
ip.addr == 192.168.1.10
```

**Filter by source IP**

```text
ip.src == 192.168.1.10
```

**Filter by destination IP**

```text
ip.dst == 192.168.1.20
```

**Filter by TCP**

```text
tcp
```

**Filter by UDP**

```text
udp
```

**Filter by port**

```text
tcp.port == 443
```

**Filter by DNS**

```text
dns
```

**Filter by HTTP**

```text
http
```

**Filter by ICMP**

```text
icmp
```

**Filter by ARP**

```text
arp
```

**Filter by a TCP conversation**

```text
tcp.stream eq 0
```

**Filter by packet length**

```text
frame.len > 1000
```

**Filter by retransmissions**

```text
tcp.analysis.retransmission
```

**Filter by TCP connection establishment**

```text
tcp.flags.syn == 1
```

**Filter by TCP reset**

```text
tcp.flags.reset == 1
```

# 🗁 Understanding Ethernet Frames

Ethernet frames operate at the Data Link Layer.

An Ethernet II frame contains:

| Field | Description |
| --- | --- |
| Destination MAC | Receiving device's MAC address |
| Source MAC | Sending device's MAC address |
| EtherType | Identifies the encapsulated protocol |
| Payload | Encapsulated network-layer data |
| Frame Check Sequence | Error-detection field |

Common EtherTypes:

| EtherType | Protocol |
| --- | --- |
| 0x0800 | IPv4 |
| 0x0806 | ARP |
| 0x86DD | IPv6 |
| 0x8100 | IEEE 802.1Q VLAN tag |

Example Wireshark filter:

```text
eth.addr == 00:11:22:33:44:55
```

Filter by source MAC:

```text
eth.src == 00:11:22:33:44:55
```

Filter by destination MAC:

```text
eth.dst == 00:11:22:33:44:66
```

# 🗁 Understanding IP Packets

IP packets operate at the Network Layer.

Important IPv4 fields include:

* Source IP address.
* Destination IP address.
* TTL.
* Protocol.
* Header length.
* Total packet length.
* Identification.
* Fragmentation flags and offset.

Example filters:

```text
ip.src == 192.168.1.10
```

```text
ip.dst == 192.168.1.20
```

```text
ip.ttl <= 5
```

```text
ip.flags.mf == 1
```

```text
ip.frag_offset > 0
```

A low TTL may indicate that a packet has passed through multiple routers or that a routing loop exists. It is not sufficient by itself to prove a routing problem.

# 🗁 ARP Packet Analysis

Address Resolution Protocol maps IPv4 addresses to MAC addresses on a local network.

Common ARP operations:

* ARP Request.
* ARP Reply.
* Gratuitous ARP.

Example capture:

```bash
sudo tcpdump -ni eth0 arp
```

Wireshark display filter:

```text
arp
```

Common observations:

**Normal ARP resolution**

```text
Who has 192.168.1.1?
192.168.1.1 is at AA:BB:CC:DD:EE:FF
```

**Repeated unanswered ARP requests**

Possible causes:

* Target device is offline.
* Incorrect IP address.
* VLAN mismatch.
* Layer 2 connectivity problem.
* Switch port or cable problem.
* Incorrect subnet configuration.

Repeated ARP requests do not automatically prove an ARP protocol failure. The target may simply be unavailable.

# 🗁 ICMP Packet Analysis

ICMP provides diagnostic and error-reporting messages.

Common ICMP types:

| ICMP Type | Message |
| --- | --- |
| 0 | Echo Reply |
| 3 | Destination Unreachable |
| 5 | Redirect |
| 8 | Echo Request |
| 11 | Time Exceeded |

Capture ICMP traffic:

```bash
sudo tcpdump -ni eth0 icmp
```

Wireshark filter:

```text
icmp
```

Useful ICMP error messages:

* Destination network unreachable.
* Destination host unreachable.
* Fragmentation needed.
* Time exceeded.

ICMP errors can reveal routing, firewall, and MTU problems.

# 🗁 DNS Packet Analysis

DNS translates domain names into IP addresses.

DNS commonly uses UDP port 53. TCP port 53 is also used for cases such as large responses and zone transfers.

Capture DNS traffic:

```bash
sudo tcpdump -ni eth0 port 53
```

Wireshark display filter:

```text
dns
```

Useful DNS filters:

**DNS queries**

```text
dns.flags.response == 0
```

**DNS responses**

```text
dns.flags.response == 1
```

**DNS errors**

```text
dns.flags.rcode != 0
```

**Queries for a specific domain**

```text
dns.qry.name == "example.com"
```

Important DNS response codes:

| Code | Meaning |
| --- | --- |
| 0 | NOERROR |
| 1 | FORMERR |
| 2 | SERVFAIL |
| 3 | NXDOMAIN |
| 4 | NOTIMP |
| 5 | REFUSED |

Common troubleshooting findings:

* Query leaves but no response returns.
* Response contains NXDOMAIN.
* DNS server responds with SERVFAIL.
* Client queries an unexpected DNS server.
* DNS response is delayed.
* UDP response is truncated and TCP retry is required.

Encrypted DNS protocols such as DNS over HTTPS and DNS over TLS cannot generally be inspected as ordinary plaintext DNS without additional decryption or endpoint visibility.

# 🗁 TCP Packet Analysis

TCP provides reliable, connection-oriented transport.

Important TCP fields:

* Source port.
* Destination port.
* Sequence number.
* Acknowledgment number.
* Window size.
* Flags.
* Checksum.
* Options.

Common TCP flags:

| Flag | Meaning |
| --- | --- |
| SYN | Initiates a connection |
| ACK | Acknowledges received data |
| FIN | Gracefully closes a connection |
| RST | Resets a connection |
| PSH | Requests prompt delivery to the application |
| URG | Indicates urgent data |

# 🗁 TCP Three-Way Handshake Analysis

A normal TCP connection starts with a three-way handshake.

```text
Client                         Server
  |                               |
  | -------- SYN ---------------> |
  |                               |
  | <----- SYN + ACK ------------ |
  |                               |
  | -------- ACK ---------------> |
  |                               |
  |        Connection             |
  |        Established            |
```

Wireshark filter:

```text
tcp.flags.syn == 1
```

A more specific filter for initial SYN packets:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Common problems:

**SYN retransmissions**

The client sends SYN packets but does not receive the expected response.

Possible causes:

* Server unavailable.
* Firewall dropping packets.
* Routing failure.
* Incorrect destination address.
* Return path failure.

**SYN followed by RST**

The destination or an intermediate device actively rejects the connection.

Possible causes:

* No service listening on the port.
* Firewall configured to reject.
* Connection directed to the wrong service.

**Successful handshake but no application response**

Possible causes:

* Application server problem.
* TLS negotiation issue.
* Backend service failure.
* Application-layer timeout.

# 🗁 TCP Retransmission Analysis

TCP retransmissions occur when data is not acknowledged as expected.

Common causes:

* Packet loss.
* Congestion.
* Network interface errors.
* Firewall or middlebox behavior.
* Receiver overload.
* Path instability.

Wireshark filters:

```text
tcp.analysis.retransmission
```

```text
tcp.analysis.fast_retransmission
```

```text
tcp.analysis.duplicate_ack
```

```text
tcp.analysis.lost_segment
```

A retransmission is evidence that TCP did not receive an expected acknowledgment in time. It does not identify the exact cause by itself.

Compare captures at both endpoints to determine whether packets are being lost in transit or delayed at an endpoint.

# 🗁 TCP Connection Termination

TCP normally closes a connection using FIN and ACK exchanges.

A connection can also be terminated using RST.

Wireshark filters:

```text
tcp.flags.fin == 1
```

```text
tcp.flags.reset == 1
```

Common interpretations:

* FIN: graceful connection termination.
* RST: immediate connection reset.
* Repeated FIN retransmissions: acknowledgment or delivery problem.
* Unexpected RST: application or network device may have aborted the connection.

# 🗁 UDP Packet Analysis

UDP is a connectionless transport protocol.

It does not establish a TCP-style connection or guarantee delivery.

Common UDP-based services include:

* DNS.
* DHCP.
* NTP.
* VoIP.
* WireGuard.
* QUIC.
* Some VPN and streaming protocols.

Capture UDP:

```bash
sudo tcpdump -ni eth0 udp
```

Wireshark filter:

```text
udp
```

Filter by destination port:

```text
udp.dstport == 53
```

Common UDP troubleshooting symptoms:

* Requests are sent but responses are absent.
* Responses come from an unexpected source.
* Firewall blocks the destination port.
* NAT mappings expire.
* Application receives incomplete or delayed datagrams.

Because UDP has no built-in retransmission mechanism, applications must implement their own reliability behavior when needed.

# 🗁 HTTP and HTTPS Analysis

HTTP is an application-layer protocol used for web communication.

HTTP commonly uses TCP port 80, while HTTPS commonly uses TCP port 443. HTTP/3 uses QUIC over UDP.

Capture HTTP traffic:

```bash
sudo tcpdump -ni eth0 tcp port 80
```

Capture HTTPS transport traffic:

```bash
sudo tcpdump -ni eth0 tcp port 443
```

Wireshark display filters:

```text
http
```

```text
tls
```

```text
quic
```

Useful HTTP response codes:

| Status Code | Meaning |
| --- | --- |
| 200 | OK |
| 301 | Moved Permanently |
| 302 | Found |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

HTTPS encrypts application data. Without suitable decryption keys or endpoint visibility, a packet capture generally exposes transport metadata and TLS handshake information rather than the HTTP request and response contents.

# 🗁 TLS Handshake Analysis

TLS protects application communication through encryption and authentication.

Common handshake stages include:

* Client Hello.
* Server Hello.
* Certificate exchange, depending on TLS version and authentication mode.
* Key establishment.
* Finished messages.

Wireshark display filter:

```text
tls.handshake
```

Filter Client Hello:

```text
tls.handshake.type == 1
```

Filter Server Hello:

```text
tls.handshake.type == 2
```

Filter certificate messages:

```text
tls.handshake.type == 11
```

Common TLS problems:

* Certificate expired.
* Certificate hostname mismatch.
* Unsupported TLS version.
* Cipher suite incompatibility.
* Incorrect system clock.
* Certificate authority not trusted.
* Server does not send a required certificate chain.
* Middlebox interference.

TLS 1.3 encrypts more of the handshake than earlier versions, so not all handshake details are visible in an ordinary capture.

# 🗁 DHCP Packet Analysis

DHCP dynamically assigns IP configuration to clients.

The normal IPv4 DHCP process is often called DORA:

```text
Client                         DHCP Server
  |                                 |
  | -------- Discover ------------> |
  |                                 |
  | <-------- Offer --------------- |
  |                                 |
  | -------- Request -------------> |
  |                                 |
  | <-------- Acknowledge --------- |
```

Capture DHCP traffic:

```bash
sudo tcpdump -ni eth0 'udp port 67 or udp port 68'
```

Wireshark filter:

```text
dhcp
```

Common DHCP issues:

* Discover messages receive no Offer.
* Multiple DHCP servers respond unexpectedly.
* DHCP server provides incorrect gateway.
* Incorrect DNS server is advertised.
* Address pool is exhausted.
* DHCP relay is missing or misconfigured.
* VLAN prevents communication with the DHCP server.

# 🗁 Packet Timing and Latency Analysis

Packet timestamps help identify delays between communication events.

In Wireshark, useful features include:

* Packet time.
* Delta time.
* Time since previous displayed packet.
* TCP stream timing.
* Request-response timing.

Useful TShark command:

```bash
tshark -r capture.pcap -T fields \
  -e frame.number \
  -e frame.time \
  -e ip.src \
  -e ip.dst \
  -e frame.len
```

Use timing analysis to identify:

* Slow DNS responses.
* Delayed TCP acknowledgments.
* Long application response times.
* TCP retransmission delays.
* Irregular packet intervals.

Packet timestamps are only as reliable as the clock and capture environment. When comparing captures from different devices, synchronize clocks using NTP or another suitable time source.

# 🗁 TCP Stream Analysis

Wireshark can reconstruct a TCP conversation using Follow TCP Stream.

Workflow:

* Select a TCP packet.
* Right-click the packet.
* Choose Follow.
* Select TCP Stream.

This displays the reconstructed conversation for the selected TCP connection.

It is useful for:

* HTTP request and response inspection.
* Application protocol troubleshooting.
* Identifying unexpected connection termination.
* Understanding data exchange order.

Encrypted traffic remains encrypted unless suitable decryption material is available.

# 🗁 Wireshark Statistics

Wireshark provides several statistical views.

<AsyncImageGroup query={["Wireshark statistics conversations window screenshot","Wireshark statistics endpoints window screenshot","Wireshark IO graph network traffic screenshot"]} aspectRatio="5:4" layout="carousel"/>

Important analysis features:

**Protocol Hierarchy**

Shows the distribution of captured traffic by protocol.

**Conversations**

Displays communication between pairs of endpoints.

**Endpoints**

Lists devices and addresses observed in the capture.

**I/O Graphs**

Visualizes packet or byte rates over time.

**Expert Information**

Highlights protocol anomalies and potentially suspicious conditions.

These tools help identify high-volume hosts, unexpected protocols, communication patterns, and possible network problems.

# 🗁 TShark Analysis

TShark is the command-line version of Wireshark's protocol analysis engine.

**Read a capture file**

```bash
tshark -r capture.pcap
```

**Display packets matching an IP address**

```bash
tshark -r capture.pcap -Y 'ip.addr == 192.168.1.10'
```

**Display DNS packets**

```bash
tshark -r capture.pcap -Y dns
```

**Display TCP retransmissions**

```bash
tshark -r capture.pcap -Y 'tcp.analysis.retransmission'
```

**Display selected packet fields**

```bash
tshark -r capture.pcap -T fields \
  -e frame.number \
  -e ip.src \
  -e ip.dst \
  -e tcp.srcport \
  -e tcp.dstport
```

**Capture live traffic**

```bash
sudo tshark -i eth0
```

**Capture packets and save them**

```bash
sudo tshark -i eth0 -w capture.pcap
```

The `-Y` option uses Wireshark display-filter syntax, not BPF capture-filter syntax.

# 🗁 MikroTik Packet Capture

RouterOS provides packet inspection capabilities through its built-in tools.

**Use the packet sniffer**

```routeros
/tool sniffer print
```

**Start packet capture**

```routeros
/tool sniffer start
```

**Stop packet capture**

```routeros
/tool sniffer stop
```

**Inspect captured packets**

```routeros
/tool sniffer packet print
```

**Capture traffic on a specific interface**

```routeros
/tool sniffer set interface=ether1
```

**Capture a specific host**

```routeros
/tool sniffer set filter-ip-address=192.168.1.10/32
```

**Capture a specific IP protocol**

```routeros
/tool sniffer set filter-ip-protocol=tcp
```

RouterOS packet sniffer settings and export options depend on the RouterOS version and configuration. Inspect the available properties using:

```routeros
/tool sniffer set ?
```

For detailed analysis, RouterOS can save captures to a file or stream packets to a remote Wireshark instance, depending on the configured sniffer options.

# 🗁 Capturing Traffic on a Switch

A managed switch usually forwards unicast frames only to the appropriate destination ports.

Therefore, capturing traffic on an unrelated switch port will not normally reveal communication between other devices.

Common methods:

**Port Mirroring / SPAN**

Copies traffic from selected source ports or VLANs to a monitoring port.

**Network TAP**

Provides a physical observation point for network traffic.

**Inline Capture**

Places a capture device directly in the communication path.

When configuring port mirroring, consider:

* Source interface or VLAN.
* Ingress and egress direction.
* Destination monitoring port.
* Destination port capacity.
* VLAN tag preservation.
* Potential packet loss when mirrored traffic exceeds destination capacity.

# 🗁 Troubleshooting with Multiple Capture Points

Capturing traffic at more than one location can identify where packets disappear.

Example:

```text
Client
  |
  | Capture A
  |
Switch
  |
Router
  |
  | Capture B
  |
Internet
  |
Server
  |
  | Capture C
```

Compare the captures:

* Packet appears at A but not B: investigate the path between A and B.
* Packet appears at B but not C: investigate the path between B and C.
* Request reaches C but no response returns: investigate the server and its response path.
* Response reaches B but not A: investigate the return path between B and A.

Account for NAT, encapsulation, offloading, and capture-interface placement when comparing packets. A packet may legitimately appear with different addresses, headers, or sizes at different points.

# 🗁 Common Packet Analysis Findings

| Finding | Possible Explanation |
| --- | --- |
| Repeated ARP requests | Target unreachable at Layer 2 or wrong VLAN |
| TCP SYN retransmissions | No SYN-ACK received |
| TCP RST | Connection actively rejected or reset |
| TCP retransmissions | Loss, delay, or acknowledgment problem |
| DNS NXDOMAIN | Requested name does not exist in the queried DNS view |
| DNS SERVFAIL | Resolver could not complete the query |
| ICMP destination unreachable | Routing or policy failure |
| ICMP fragmentation needed | Packet exceeds path MTU |
| DHCP Discover without Offer | DHCP server or relay problem |
| TLS handshake failure | Certificate or compatibility problem |
| Large packets disappear | Possible MTU or fragmentation issue |

# 🗁 Practical Troubleshooting Scenarios

**Scenario: Client cannot access a web server**

Capture traffic:

```bash
sudo tcpdump -ni eth0 'host 192.168.1.20 and tcp port 443'
```

Investigate:

* Does the client send SYN?
* Does the server respond with SYN-ACK?
* Does the handshake complete?
* Does TLS negotiation begin?
* Does the server send application data?

This helps isolate whether the problem occurs at TCP, TLS, or application level.

**Scenario: DNS queries time out**

Capture:

```bash
sudo tcpdump -ni eth0 port 53
```

Investigate:

* Is the DNS query transmitted?
* Does it reach the DNS server?
* Does a response return?
* Does the response contain an error?
* Is the response routed back to the client?

**Scenario: Network experiences slow performance**

Capture traffic and inspect:

```text
tcp.analysis.retransmission
```

Also investigate:

* Packet loss.
* Duplicate acknowledgments.
* TCP window behavior.
* Round-trip time.
* Interface errors.
* Congestion.

**Scenario: DHCP clients cannot obtain addresses**

Capture:

```bash
sudo tcpdump -ni eth0 'udp port 67 or udp port 68'
```

Check whether the DHCP Discover reaches the server and whether the server sends an Offer.

**Scenario: VPN traffic is not passing**

Capture the outer VPN protocol at the physical interface and inner traffic at the tunnel interface.

For example:

```bash
sudo tcpdump -ni eth0 udp port 51820
```

```bash
sudo tcpdump -ni wg0
```

Compare the captures to determine whether the issue is VPN transport, peer configuration, or routing inside the tunnel.

# 🗁 Packet Capture Best Practices

* Capture on the correct interface.
* Use narrow filters when possible.
* Record the time and location of each capture.
* Capture both directions when investigating asymmetric traffic.
* Synchronize clocks across capture devices.
* Use sufficiently large capture buffers.
* Monitor disk usage during long captures.
* Preserve original PCAP files.
* Analyze copies of important captures.
* Avoid collecting unrelated traffic.
* Restrict access to capture files.
* Be aware that captures may contain credentials, session tokens, personal data, or other sensitive information.
* Document capture filters and investigation findings.

# 🗁 Summary

Packet capture analysis provides detailed visibility into network communication.

Important skills include:

* Capturing traffic using tcpdump.
* Analyzing packets with Wireshark.
* Understanding capture and display filters.
* Inspecting Ethernet, IP, TCP, and UDP headers.
* Troubleshooting DNS, DHCP, ARP, ICMP, and TLS.
* Identifying TCP retransmissions and connection failures.
* Analyzing timing and application conversations.
* Comparing captures from multiple network locations.

Packet captures are most useful when combined with routing tables, firewall rules, interface statistics, and application logs. They help identify not only what failed, but also where communication stopped.
