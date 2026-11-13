# Packet Sniffer

Packet Sniffer is a network diagnostic tool in MikroTik RouterOS that captures and analyzes packets passing through network interfaces.

It helps administrators investigate communication problems, inspect packet headers, troubleshoot protocol behavior, verify network connectivity, and diagnose firewall or routing issues.

Unlike Torch, which summarizes live traffic flows, Packet Sniffer captures individual packets and provides more detailed information about network communication.

# 🗁 Understanding Packet Sniffer

Packet Sniffer captures network packets and makes them available for inspection.

Depending on the capture method and configuration, it can provide information such as:

* Source and destination MAC addresses.
* Source and destination IP addresses.
* Source and destination ports.
* IP protocol.
* Packet size.
* VLAN tags.
* TCP flags.
* ICMP messages.
* Packet timestamps.

Packet Sniffer is particularly useful when you need to understand what is happening inside an individual network communication.

# 🗁 Packet Sniffer vs Torch

Both tools are used for network traffic investigation, but they serve different purposes.

| Feature | Torch | Packet Sniffer |
| --- | --- | --- |
| Primary purpose | Real-time traffic-flow monitoring | Packet capture and inspection |
| Source and destination IPs | Yes | Yes |
| Protocol visibility | Yes | Yes |
| Port visibility | Yes | Yes |
| Individual packet inspection | Limited | Yes |
| Packet capture files | No | Yes |
| Detailed packet analysis | Limited | More detailed |
| Historical analysis | No | Possible through saved captures |

Use Torch to identify which devices and applications are consuming bandwidth.

Use Packet Sniffer to investigate how packets are exchanged and identify communication problems.

# 🗁 Inspecting Packet Sniffer Configuration

Display the Packet Sniffer configuration:

```routeros
/tool/sniffer/print
```

The configuration includes settings for:

* Interface selection.
* Capture direction.
* Source address filtering.
* Destination address filtering.
* Port filtering.
* Protocol filtering.
* Packet size limits.
* Memory buffering.
* File-based capture.

# 🗁 Capturing Packets on an Interface

Start capturing packets on a specific interface:

```routeros
/tool/sniffer/set filter-interface=ether1
```

Start the capture:

```routeros
/tool/sniffer/start
```

Stop the capture:

```routeros
/tool/sniffer/stop
```

Inspect captured packets:

```routeros
/tool/sniffer/packet/print
```

Replace `ether1` with the interface you want to investigate.

The packet list contains captured packet information. The amount of detail available depends on the capture settings and RouterOS version.

# 🗁 Capturing Packets Without an Interface Filter

To capture traffic across eligible interfaces, remove the interface restriction:

```routeros
/tool/sniffer/set filter-interface=all
```

Start capturing:

```routeros
/tool/sniffer/start
```

Stop capturing:

```routeros
/tool/sniffer/stop
```

Inspect the results:

```routeros
/tool/sniffer/packet/print
```

Capturing across multiple interfaces can generate a large amount of data. Prefer a specific interface when investigating a known traffic path.

# 🗁 Filtering Packets by Source Address

To capture packets originating from a specific IP address:

```routeros
/tool/sniffer/set filter-src-ip-address=192.168.10.10/32
```

Select the interface:

```routeros
/tool/sniffer/set filter-interface=bridge1
```

Start the capture:

```routeros
/tool/sniffer/start
```

Stop the capture:

```routeros
/tool/sniffer/stop
```

This is useful when investigating communication from a particular LAN device.

# 🗁 Filtering Packets by Destination Address

To capture packets directed toward a specific IP address:

```routeros
/tool/sniffer/set filter-dst-ip-address=8.8.8.8/32
```

Start the capture:

```routeros
/tool/sniffer/start
```

This helps investigate communication toward a particular server or destination.

# 🗁 Filtering Packets by Protocol

Protocol filtering reduces unnecessary captured traffic.

Capture TCP packets:

```routeros
/tool/sniffer/set filter-ip-protocol=tcp
```

Capture UDP packets:

```routeros
/tool/sniffer/set filter-ip-protocol=udp
```

Capture ICMP packets:

```routeros
/tool/sniffer/set filter-ip-protocol=icmp
```

Capture GRE packets:

```routeros
/tool/sniffer/set filter-ip-protocol=gre
```

Protocol filters are useful when investigating a specific network service or communication type.

# 🗁 Filtering Packets by Port

Packet Sniffer supports source and destination port filters.

Capture HTTPS traffic:

```routeros
/tool/sniffer/set filter-ip-protocol=tcp filter-port=443
```

Capture DNS traffic:

```routeros
/tool/sniffer/set filter-ip-protocol=udp filter-port=53
```

Capture SSH traffic:

```routeros
/tool/sniffer/set filter-ip-protocol=tcp filter-port=22
```

The `filter-port` setting matches the configured port filter. For more precise filtering, use source and destination port filters when supported by the installed RouterOS version.

# 🗁 Configuring Capture Direction

Packet Sniffer can filter traffic according to its direction on the selected interface.

The commonly available directions are:

| Direction | Description |
| --- | --- |
| `rx` | Packets received by the router on the interface |
| `tx` | Packets transmitted by the router through the interface |
| `any` | Both receive and transmit directions |

Example:

```routeros
/tool/sniffer/set filter-direction=rx
```

To capture transmitted traffic:

```routeros
/tool/sniffer/set filter-direction=tx
```

To capture both directions:

```routeros
/tool/sniffer/set filter-direction=any
```

Direction is relative to the selected interface, not necessarily the perspective of the end-user device.

# 🗁 Saving Packet Captures to a File

Packet Sniffer can save captured packets to a file for later analysis.

Configure the capture interface and output file:

```routeros
/tool/sniffer/set filter-interface=ether1 file-name=traffic-capture.pcap
```

Start capturing:

```routeros
/tool/sniffer/start
```

Stop capturing:

```routeros
/tool/sniffer/stop
```

List available files:

```routeros
/file/print
```

The resulting PCAP file can be downloaded and opened using Wireshark or another compatible packet analysis tool.

For example, the file can be opened in Wireshark to inspect:

* Ethernet frames.
* IP packets.
* TCP handshakes.
* DNS queries and responses.
* ICMP messages.
* Application-layer protocol metadata.

# 🗁 Configuring Capture Size and Buffering

Packet captures can consume considerable memory and storage.

Use capture limits to control resource consumption.

Configure a maximum captured packet size:

```routeros
/tool/sniffer/set filter-operator-between-entries=and
```

Packet Sniffer also provides packet-size filtering options through `filter-ip-address`, `filter-mac-address`, and other supported filters, depending on the RouterOS release.

For file-based captures, configure an appropriate file limit:

```routeros
/tool/sniffer/set file-limit=10MiB
```

A capture file limit helps prevent a single capture from consuming excessive storage.

The precise buffering and file-rotation options depend on the installed RouterOS version. Inspect available properties using command completion:

```routeros
/tool/sniffer/set ?
```

# 🗁 Analyzing TCP Handshakes

Packet Sniffer can help investigate TCP connection establishment.

A normal TCP three-way handshake consists of:

* SYN: Client requests a connection.
* SYN-ACK: Server acknowledges the request.
* ACK: Client acknowledges the server response.

The sequence is:

```text
Client                  Server
  |                       |
  |-------- SYN --------->|
  |                       |
  |<----- SYN-ACK --------|
  |                       |
  |-------- ACK --------->|
  |                       |
  |    Connection Ready   |
```

If the handshake does not complete, investigate:

* Firewall filtering.
* Incorrect routing.
* Missing return routes.
* Server availability.
* NAT configuration.
* Network congestion.
* Asymmetric routing.

Packet capture can help determine whether SYN packets leave the client network, reach the server network, and receive responses.

# 🗁 Troubleshooting DNS Communication

Packet Sniffer can help investigate DNS resolution problems.

Capture DNS traffic:

```routeros
/tool/sniffer/set filter-interface=bridge1 filter-ip-protocol=udp filter-port=53
```

Start capturing:

```routeros
/tool/sniffer/start
```

Generate a DNS query from a connected client, then stop capturing:

```routeros
/tool/sniffer/stop
```

Inspect the captured packets:

```routeros
/tool/sniffer/packet/print
```

Look for:

* DNS query packets.
* Destination DNS server address.
* DNS response packets.
* Response delays.
* Missing responses.
* Unexpected DNS destinations.

DNS may also use TCP port 53, particularly for larger responses and other DNS operations. Encrypted DNS protocols use different transport arrangements and will not necessarily appear in a port-53 capture.

# 🗁 Troubleshooting ICMP Communication

ICMP is commonly used for network diagnostics.

Capture ICMP traffic:

```routeros
/tool/sniffer/set filter-interface=ether1 filter-ip-protocol=icmp
```

Start the capture:

```routeros
/tool/sniffer/start
```

Generate ping traffic from a connected device.

Stop the capture:

```routeros
/tool/sniffer/stop
```

Inspect the captured packets:

```routeros
/tool/sniffer/packet/print
```

Important ICMP messages include:

| ICMP Type | Description |
| --- | --- |
| Echo Request | Ping request |
| Echo Reply | Ping response |
| Destination Unreachable | Destination or service cannot be reached |
| Time Exceeded | Packet exceeded its permitted lifetime |

ICMP captures can help identify connectivity failures, routing problems, and path-related issues.

# 🗁 Using Packet Sniffer with Wireshark

Wireshark is a packet analysis application that provides detailed protocol dissection and filtering.

A typical workflow is:

**Capture packets on MikroTik**

```routeros
/tool/sniffer/set filter-interface=ether1 file-name=network-capture.pcap
```

**Start the capture**

```routeros
/tool/sniffer/start
```

**Stop the capture**

```routeros
/tool/sniffer/stop
```

**Download the PCAP file**

```routeros
/file/print
```

Use WinBox or another supported file-transfer method to download the capture.

**Open the file in Wireshark**

Wireshark can display packet details, protocol fields, timestamps, TCP streams, and other information.

Useful Wireshark display filters include:

```text
ip.addr == 192.168.10.10
```

```text
tcp.port == 443
```

```text
udp.port == 53
```

```text
icmp
```

```text
tcp.flags.syn == 1
```

These are Wireshark display filters, not RouterOS commands.

# 🗁 Packet Sniffer Limitations

Packet Sniffer is powerful, but its visibility depends on the network architecture and hardware.

Important considerations:

* Hardware-offloaded switching traffic may bypass the CPU and remain invisible to CPU-based packet capture.
* FastTracked traffic can affect visibility in CPU-based inspection.
* Traffic may need to be captured at another interface or network observation point.
* Encrypted traffic generally exposes packet metadata but not the protected application payload.
* Capturing large traffic volumes can consume memory, storage, and CPU resources.
* Interface selection determines which part of the traffic path is observed.
* A packet visible on one interface may not necessarily be visible on another interface.

For hardware-switched networks, consider using a switch mirror port or another suitable network TAP when router-based capture cannot observe the required traffic.

# 🗁 Practical Packet Capture Workflow

Use this workflow when investigating a client that cannot reach an external server.

**Check interface status**

```routeros
/interface/print
```

**Configure the capture interface**

```routeros
/tool/sniffer/set filter-interface=ether1
```

**Apply a destination filter**

```routeros
/tool/sniffer/set filter-dst-ip-address=8.8.8.8/32
```

**Start packet capture**

```routeros
/tool/sniffer/start
```

**Generate test traffic**

From a client device:

```bash
ping 8.8.8.8
```

**Stop packet capture**

```routeros
/tool/sniffer/stop
```

**Inspect captured packets**

```routeros
/tool/sniffer/packet/print
```

Analyze whether requests and responses are visible.

If requests leave the LAN but no corresponding responses return, investigate upstream routing, firewall policies, NAT, and the remote destination.

If requests never appear on the expected interface, investigate the client configuration, bridge, VLAN, and local forwarding path.

# 🗁 Best Practices for Packet Sniffing

* Capture traffic on the interface closest to the problem.
* Apply filters to reduce unnecessary packet capture.
* Use short capture sessions during troubleshooting.
* Save important captures as PCAP files for detailed analysis.
* Use Wireshark for deeper protocol inspection.
* Monitor router CPU and memory during large captures.
* Consider FastTrack and hardware offloading when packets are missing.
* Avoid exposing sensitive packet captures to unauthorized users.
* Remove unnecessary capture files after analysis.
* Document capture conditions and relevant timestamps.

Packet Sniffer is an essential troubleshooting tool for examining individual packets, verifying protocol behavior, and identifying communication failures across MikroTik networks.
