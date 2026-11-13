# Torch

Torch is a real-time traffic monitoring tool in MikroTik RouterOS. It allows network administrators to inspect traffic passing through an interface and identify which devices, IP addresses, protocols, and ports are consuming bandwidth.

Unlike basic interface statistics, which show aggregate traffic counters, Torch provides a more detailed view of individual traffic flows.

It is especially useful for identifying bandwidth-heavy devices, investigating unexpected traffic, troubleshooting connectivity, and analyzing network usage.

# 🗁 Understanding Torch

Torch monitors traffic on a selected network interface and displays information about observed traffic flows.

It can show information such as:

* Source IP address.
* Destination IP address.
* Source port.
* Destination port.
* IP protocol.
* MAC protocol.
* VLAN ID, where applicable.
* Transmit traffic rate.
* Receive traffic rate.
* Packet rate.

The available fields depend on the interface type and traffic being monitored.

# 🗁 Opening Torch

Run Torch on a specific interface:

```routeros
/tool/torch interface=ether1
```

Replace `ether1` with the interface you want to monitor.

For example, to monitor a WAN interface:

```routeros
/tool/torch interface=ether1
```

To monitor a LAN interface:

```routeros
/tool/torch interface=ether2
```

Torch displays live traffic information and continuously updates the results.

Press `Q` or `Ctrl+C` to stop the monitoring session, depending on the terminal interface.

# 🗁 Understanding Torch Output

A typical Torch session may display columns such as:

| Field | Description |
| --- | --- |
| `SRC-ADDRESS` | Source IP address |
| `DST-ADDRESS` | Destination IP address |
| `SRC-PORT` | Source transport-layer port |
| `DST-PORT` | Destination transport-layer port |
| `PROTOCOL` | IP protocol |
| `TX` | Transmit traffic rate |
| `RX` | Receive traffic rate |
| `TX-PACKETS` | Transmitted packets per second |
| `RX-PACKETS` | Received packets per second |

The exact column names and available values can vary by RouterOS version and monitoring context.

For example, traffic might include:

```text
SRC-ADDRESS       DST-ADDRESS       PROTOCOL    TX        RX
192.168.10.10     8.8.8.8           udp         2Mbps     1Mbps
192.168.10.20     1.1.1.1           udp         500kbps   300kbps
192.168.10.30     93.184.216.34     tcp         4Mbps     2Mbps
```

This example illustrates how Torch can help identify which source and destination addresses are exchanging traffic.

# 🗁 Monitoring Traffic by Source Address

You can filter Torch by source IP address.

For example, monitor traffic originating from a particular LAN device:

```routeros
/tool/torch interface=bridge1 src-address=192.168.10.10
```

To monitor a source subnet:

```routeros
/tool/torch interface=bridge1 src-address=192.168.10.0/24
```

This is useful when investigating a specific device or subnet that is consuming excessive bandwidth.

# 🗁 Monitoring Traffic by Destination Address

Filter traffic by destination IP address:

```routeros
/tool/torch interface=ether1 dst-address=8.8.8.8
```

Monitor traffic going to a destination subnet:

```routeros
/tool/torch interface=ether1 dst-address=192.168.10.0/24
```

This helps identify traffic directed toward a particular server, service, or network.

# 🗁 Monitoring Traffic by Protocol

Torch can filter traffic using IP protocols.

For example, monitor TCP traffic:

```routeros
/tool/torch interface=ether1 protocol=tcp
```

Monitor UDP traffic:

```routeros
/tool/torch interface=ether1 protocol=udp
```

Monitor ICMP traffic:

```routeros
/tool/torch interface=ether1 protocol=icmp
```

Common IP protocols include:

| Protocol | Description |
| --- | --- |
| TCP | Connection-oriented transport protocol |
| UDP | Connectionless transport protocol |
| ICMP | Network diagnostic and error-reporting protocol |
| GRE | Generic Routing Encapsulation |
| ESP | Encapsulating Security Payload |

Protocol availability depends on the traffic observed and supported filter values.

# 🗁 Monitoring Traffic by Port

Port filtering helps identify traffic associated with particular applications and services.

Monitor HTTP traffic:

```routeros
/tool/torch interface=ether1 protocol=tcp dst-port=80
```

Monitor HTTPS traffic:

```routeros
/tool/torch interface=ether1 protocol=tcp dst-port=443
```

Monitor DNS traffic over UDP:

```routeros
/tool/torch interface=ether1 protocol=udp dst-port=53
```

Monitor SSH traffic:

```routeros
/tool/torch interface=ether1 protocol=tcp dst-port=22
```

Common ports:

| Port | Protocol | Service |
| --- | --- | --- |
| 22 | TCP | SSH |
| 53 | UDP/TCP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 123 | UDP | NTP |
| 161 | UDP | SNMP |
| 3389 | TCP/UDP | RDP |

Port filtering helps narrow down traffic, but it does not guarantee that every application uses its standard port.

# 🗁 Monitoring Traffic by VLAN

When monitoring a VLAN-aware network, VLAN information can help identify traffic associated with a particular VLAN.

For example:

```routeros
/tool/torch interface=ether1 vlan-id=10
```

This monitors traffic associated with VLAN ID 10 on the selected interface, where VLAN information is available to Torch.

VLAN monitoring is useful when:

* Multiple customer networks share a trunk.
* Several departments use separate VLANs.
* An ISP provides multiple subscriber VLANs.
* Administrators need to investigate traffic on a specific VLAN.

The selected interface must actually carry the VLAN traffic being investigated.

# 🗁 Monitoring WAN Traffic

Torch is useful for identifying which internal devices are communicating through a WAN interface.

For example:

```routeros
/tool/torch interface=ether1
```

Assume:

* `ether1` is the WAN interface.
* `bridge1` is the LAN bridge.
* `192.168.10.0/24` is the LAN subnet.

A WAN monitoring session can reveal traffic flows involving internal clients and external destinations.

Use this information to investigate:

* Unexpected outbound traffic.
* High bandwidth consumption.
* Large downloads or uploads.
* Unusual connection patterns.
* Traffic toward unfamiliar destinations.

Remember that traffic direction is relative to the selected interface. Interpret transmit and receive values according to the router's interface perspective.

# 🗁 Identifying Bandwidth-Heavy Devices

Suppose multiple users share a LAN connection and the WAN bandwidth is frequently saturated.

Start Torch on the LAN bridge:

```routeros
/tool/torch interface=bridge1
```

Observe the source addresses and their traffic rates.

Example:

```text
SRC-ADDRESS       TX
192.168.10.10     15Mbps
192.168.10.20     2Mbps
192.168.10.30     35Mbps
192.168.10.40     1Mbps
```

In this example, `192.168.10.30` is generating the highest transmit rate among the displayed flows.

Investigate that device further to determine whether the traffic is expected.

Possible next steps include:

* Checking the user's activity.
* Reviewing active connections.
* Inspecting queue statistics.
* Reviewing firewall rules.
* Applying bandwidth limits if required.

Do not assume that the highest traffic rate represents unwanted activity. It may be a legitimate download, backup, software update, or business application.

# 🗁 Torch Through WinBox

Torch is also available through the WinBox graphical interface.

Open WinBox and connect to the router.

Navigate to:

**Tools → Torch**

Select the interface you want to monitor.

Configure available filters, such as:

* Source address.
* Destination address.
* Source port.
* Destination port.
* Protocol.
* VLAN ID.

Click **Start** to begin monitoring.

The interface displays observed traffic flows and their current rates.

Click **Stop** when the monitoring session is complete.

# 🗁 Torch vs Interface Statistics

Torch and interface statistics serve different monitoring purposes.

| Feature | Torch | Interface Statistics |
| --- | --- | --- |
| Real-time traffic | Yes | Counters can be monitored |
| Source IP visibility | Yes, when available | No |
| Destination IP visibility | Yes, when available | No |
| Port visibility | Yes, when available | No |
| Protocol visibility | Yes, when available | No |
| Aggregate byte counters | Not its primary purpose | Yes |
| Error and drop counters | Not its primary purpose | Yes |
| Traffic investigation | Detailed flow visibility | Aggregate interface health |

Use interface statistics to investigate packet errors, drops, and cumulative traffic.

Use Torch to investigate which devices and traffic flows are responsible for observed network activity.

# 🗁 Torch Limitations and Considerations

Torch is a diagnostic tool, not a complete network accounting or historical monitoring system.

Consider the following limitations:

* Torch displays live traffic rather than providing long-term historical reports.
* Results depend on the selected interface and its traffic direction.
* Hardware-offloaded traffic may not be visible to CPU-based monitoring tools.
* FastTracked or otherwise hardware-accelerated traffic can affect what is visible to CPU-based inspection tools.
* Encrypted traffic can reveal addresses, ports, and traffic volume, but not necessarily application content.
* High-volume monitoring can add processing overhead.
* A Torch result should be correlated with interface statistics, firewall counters, and other diagnostic tools.

For accurate traffic analysis, understand the router's forwarding path, bridge configuration, hardware offloading, and FastTrack configuration.

# 🗁 Practical Torch Troubleshooting Workflow

Use the following workflow when investigating unexpected bandwidth usage.

**Check interface status**

```routeros
/interface/print
```

**Check aggregate interface traffic**

```routeros
/interface/monitor-traffic ether1
```

**Start Torch on the relevant interface**

```routeros
/tool/torch interface=ether1
```

**Narrow the investigation to a source address**

```routeros
/tool/torch interface=bridge1 src-address=192.168.10.30
```

**Investigate a specific service**

```routeros
/tool/torch interface=ether1 protocol=tcp dst-port=443
```

**Review active connections**

```routeros
/ip/firewall/connection/print
```

**Inspect interface statistics**

```routeros
/interface/print stats
```

Correlate the results before changing firewall, routing, or queue configuration.

# 🗁 Best Practices for Torch

* Select the correct interface before starting a monitoring session.
* Monitor LAN interfaces when identifying internal traffic sources.
* Monitor WAN interfaces when investigating external traffic flows.
* Use source and destination filters to narrow large traffic volumes.
* Use protocol and port filters when investigating specific services.
* Compare Torch results with interface counters.
* Consider FastTrack and hardware offloading when traffic appears missing.
* Avoid leaving intensive monitoring sessions running unnecessarily.
* Use centralized traffic monitoring for long-term reporting.
* Document unusual traffic flows before making configuration changes.

Torch is an essential RouterOS troubleshooting tool for real-time traffic visibility and identifying the devices, protocols, and services responsible for network activity.
