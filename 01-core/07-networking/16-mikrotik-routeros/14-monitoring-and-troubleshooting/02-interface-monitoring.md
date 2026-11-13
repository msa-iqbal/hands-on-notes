# Interface Monitoring

Interface monitoring in MikroTik RouterOS provides visibility into the operational status, traffic activity, packet statistics, errors, and performance of network interfaces.

It helps administrators identify connectivity problems, bandwidth utilization, packet loss, interface errors, duplex mismatches, and physical-layer issues.

RouterOS provides several tools for monitoring Ethernet, wireless, VLAN, bridge, tunnel, and other interfaces.

# 🗁 Understanding Interface Status

Display all configured interfaces:

```routeros
/interface/print
```

Example output:

```routeros
Flags: R - RUNNING, X - DISABLED, D - DYNAMIC

Columns: NAME, TYPE, ACTUAL-MTU, L2MTU, MAX-L2MTU, MAC-ADDRESS
#    NAME       TYPE      ACTUAL-MTU  L2MTU
0 R  ether1     ether           1500   1598
1 R  ether2     ether           1500   1598
2    ether3     ether           1500   1598
3 R  bridge1    bridge          1500
```

Important interface flags:

| Flag | Meaning |
| --- | --- |
| `R` | Interface is running |
| `X` | Interface is disabled |
| `D` | Interface is dynamically created |

An interface can be configured and enabled without being operational.

For example, an Ethernet interface may be enabled but show no running flag because there is no physical link.

# 🗁 Monitoring Interface Traffic

Use the interface monitor command to inspect traffic activity.

Monitor a specific interface:

```routeros
/interface/monitor-traffic ether1
```

Example output:

```routeros
name: ether1
rx-packets-per-second: 1250
rx-bits-per-second: 8500000
tx-packets-per-second: 980
tx-bits-per-second: 6200000
```

Important traffic fields:

| Field | Description |
| --- | --- |
| `rx-packets-per-second` | Incoming packets per second |
| `tx-packets-per-second` | Outgoing packets per second |
| `rx-bits-per-second` | Incoming traffic rate |
| `tx-bits-per-second` | Outgoing traffic rate |
| `rx-drops-per-second` | Incoming packet drops per second, when available |
| `tx-drops-per-second` | Outgoing packet drops per second, when available |

Monitor multiple interfaces:

```routeros
/interface/monitor-traffic ether1,ether2
```

Press `Ctrl+C` to stop continuous monitoring.

# 🗁 Monitoring Ethernet Interface Status

Ethernet interface monitoring helps identify physical connectivity and negotiation problems.

Inspect an Ethernet interface:

```routeros
/interface/ethernet/monitor ether1
```

Example output:

```routeros
name: ether1
status: link-ok
auto-negotiation: done
rate: 1Gbps
full-duplex: yes
tx-flow-control: no
rx-flow-control: no
```

Important fields:

| Field | Description |
| --- | --- |
| `status` | Physical link condition |
| `auto-negotiation` | Negotiation status |
| `rate` | Negotiated link speed |
| `full-duplex` | Whether full-duplex operation is active |
| `tx-flow-control` | Transmit flow-control status |
| `rx-flow-control` | Receive flow-control status |

The exact fields depend on the Ethernet hardware.

Common Ethernet link speeds include:

* 10 Mbps
* 100 Mbps
* 1 Gbps
* 2.5 Gbps
* 5 Gbps
* 10 Gbps

A negotiated speed lower than expected may indicate:

* Faulty Ethernet cable.
* Damaged connector.
* Unsupported link speed.
* Incorrect negotiation settings.
* Problems with the connected switch or device.

# 🗁 Monitoring Interface Statistics

Interface statistics provide information about packets, bytes, errors, and drops.

Display interface statistics:

```routeros
/interface/print stats
```

For detailed statistics of a specific interface:

```routeros
/interface/ethernet/print stats where name=ether1
```

Depending on hardware and RouterOS support, statistics may include:

| Statistic | Description |
| --- | --- |
| `rx-byte` | Total received bytes |
| `tx-byte` | Total transmitted bytes |
| `rx-packet` | Total received packets |
| `tx-packet` | Total transmitted packets |
| `rx-error` | Received packets with errors |
| `tx-error` | Transmitted packets with errors |
| `rx-drop` | Received packets dropped |
| `tx-drop` | Transmitted packets dropped |

These counters are cumulative and generally increase while the interface is active.

Use the statistics to compare traffic levels and identify abnormal error or drop patterns.

# 🗁 Understanding Interface Errors and Drops

Interface errors and drops can indicate physical-layer problems, congestion, hardware limitations, or packet-processing issues.

**Receive errors**

Receive errors indicate problems encountered while receiving packets.

Possible causes:

* Damaged cables or connectors.
* Electrical interference.
* Physical-layer problems.
* Faulty network equipment.

**Transmit errors**

Transmit errors indicate problems encountered while transmitting packets.

Possible causes:

* Hardware problems.
* Link instability.
* Physical connection issues.

**Receive drops**

Receive drops indicate packets discarded before successful processing.

Possible causes include:

* Buffer exhaustion.
* Hardware limitations.
* Receive-side congestion.
* Packet-processing limitations.

**Transmit drops**

Transmit drops indicate packets discarded before successful transmission.

Possible causes include:

* Output queue congestion.
* Interface congestion.
* Queue configuration.
* Hardware resource limitations.

A drop counter does not automatically mean a physical link is faulty. Determine where packets are being discarded before changing the configuration.

# 🗁 Resetting Interface Statistics

Reset interface counters when you need to measure traffic or errors over a specific observation period.

Reset all interface statistics:

```routeros
/interface/reset-counters-all
```

Reset statistics for a specific interface:

```routeros
/interface/reset-counters ether1
```

After resetting counters, observe the interface again:

```routeros
/interface/print stats where name=ether1
```

Resetting counters clears the accumulated statistics. Record existing values first if they are needed for troubleshooting or reporting.

# 🗁 Monitoring Interface Utilization

Interface utilization represents the percentage of available interface bandwidth currently being used.

Calculate utilization using:

```text
Utilization (%) = (Current Traffic Rate / Interface Capacity) × 100
```

For example, if an Ethernet interface has a negotiated capacity of 1 Gbps and current transmit traffic is 250 Mbps:

```text
Utilization = (250 / 1000) × 100
            = 25%
```

For full-duplex Ethernet, transmit and receive directions have independent capacity.

A 1 Gbps full-duplex interface can support up to 1 Gbps in each direction simultaneously.

Consider the following when analyzing utilization:

* Compare receive and transmit traffic separately.
* Use the negotiated interface speed as the capacity.
* Observe peak utilization, not only average utilization.
* Check packet drops and errors alongside bandwidth.
* Consider the capabilities of the connected equipment.

High utilization alone does not prove a problem. Sustained congestion, packet drops, increased latency, and application performance issues provide stronger evidence of a bottleneck.

# 🗁 Monitoring Bridge Interfaces

Bridge monitoring helps administrators inspect Layer 2 connectivity and bridge-port status.

Display bridge configuration:

```routeros
/interface/bridge/print
```

Display bridge ports:

```routeros
/interface/bridge/port/print
```

Monitor bridge traffic:

```routeros
/interface/monitor-traffic bridge1
```

Inspect bridge host entries:

```routeros
/interface/bridge/host/print
```

Bridge host information helps identify learned MAC addresses and the bridge ports through which devices are reachable.

For VLAN-aware bridges, inspect VLAN membership:

```routeros
/interface/bridge/vlan/print
```

When troubleshooting bridge connectivity, check:

* Bridge running status.
* Port membership.
* Port forwarding status.
* VLAN membership and tagging.
* PVID configuration.
* Hardware offloading status, where supported.

# 🗁 Monitoring VLAN Interfaces

VLAN interfaces provide Layer 3 access to specific VLANs.

Display configured VLAN interfaces:

```routeros
/interface/vlan/print
```

Inspect a specific VLAN interface:

```routeros
/interface/vlan/print detail where name=vlan10
```

Monitor VLAN traffic:

```routeros
/interface/monitor-traffic vlan10
```

Check the parent interface and VLAN ID:

```routeros
/interface/vlan/print detail
```

When a VLAN interface is not working, investigate:

* Whether the parent interface is running.
* Whether the VLAN ID is correct.
* Whether the VLAN is permitted across the required trunk links.
* Whether bridge VLAN filtering is configured correctly.
* Whether the bridge CPU port is included as a tagged member when router-level Layer 3 access is required.

# 🗁 Monitoring Wireless Interfaces

Wireless monitoring helps identify radio status, signal quality, connection rates, and client connectivity.

Display wireless interfaces:

```routeros
/interface/wifi/print
```

For legacy wireless packages, the interface menu may instead be:

```routeros
/interface/wireless/print
```

Inspect wireless interface details:

```routeros
/interface/wifi/monitor wifi1
```

For legacy wireless interfaces:

```routeros
/interface/wireless/monitor wlan1
```

Depending on the wireless package and device, available information may include:

* Operating frequency.
* Channel width.
* Signal strength.
* Transmit and receive rates.
* Connected clients.
* Wireless protocol information.

For RouterOS v7, the newer WiFi package and legacy wireless package use different menus and monitoring commands. Use the menu corresponding to the installed wireless implementation.

# 🗁 Using Graphs for Interface Monitoring

RouterOS provides built-in traffic graphing capabilities for supported interfaces.

Enable interface graphing:

```routeros
/tool/graphing/interface/add interface=ether1
```

To configure graphing for another interface:

```routeros
/tool/graphing/interface/add interface=ether2
```

Display graphing configuration:

```routeros
/tool/graphing/interface/print
```

Graphing can provide historical traffic information through the RouterOS graphing service.

The graphing service also has access and retention settings. Configure access restrictions appropriately before exposing graphs to a management network.

For long-term monitoring, centralized monitoring systems are generally more suitable than relying only on the router's built-in graphs.

# 🗁 Practical Interface Monitoring Workflow

Use the following workflow when investigating an interface problem.

**Check interface status**

```routeros
/interface/print
```

**Inspect physical Ethernet link status**

```routeros
/interface/ethernet/monitor ether1
```

**Observe live traffic**

```routeros
/interface/monitor-traffic ether1
```

**Inspect packet and byte counters**

```routeros
/interface/print stats where name=ether1
```

**Inspect detailed Ethernet statistics**

```routeros
/interface/ethernet/print stats where name=ether1
```

**Reset counters if a fresh observation period is required**

```routeros
/interface/reset-counters ether1
```

**Observe the interface again**

```routeros
/interface/print stats where name=ether1
```

Compare the results to identify traffic changes, errors, and drops.

# 🗁 Common Interface Monitoring Problems

| Problem | Possible Cause | Investigation |
| --- | --- | --- |
| Interface is not running | Cable disconnected or remote device unavailable | `/interface/print` |
| Link speed is lower than expected | Cable, negotiation, or hardware issue | `/interface/ethernet/monitor` |
| High packet drops | Congestion or resource limitations | Interface statistics and traffic monitoring |
| Increasing receive errors | Physical-layer problems | Cable and Ethernet statistics |
| No traffic on an interface | Incorrect routing, VLAN, or physical connection | Traffic monitor and interface configuration |
| VLAN interface is inactive | Parent interface or VLAN configuration issue | `/interface/vlan/print detail` |
| Bridge port is not forwarding | Bridge, VLAN, or spanning-tree condition | `/interface/bridge/port/print` |
| Wireless interface has poor performance | Signal, interference, or channel conditions | Wireless monitor |

# 🗁 Best Practices for Interface Monitoring

* Monitor critical WAN and LAN interfaces regularly.
* Record baseline traffic levels during normal operation.
* Compare traffic utilization with negotiated link speed.
* Investigate continuously increasing error and drop counters.
* Reset counters only when a fresh measurement period is needed.
* Monitor physical link negotiation and duplex settings.
* Check bridge and VLAN configuration when Layer 2 connectivity fails.
* Monitor wireless signal quality and connection rates.
* Use centralized monitoring for historical analysis and alerting.
* Correlate interface statistics with CPU, memory, firewall, and queue utilization.

Interface monitoring is an essential troubleshooting practice for identifying physical connectivity problems, traffic congestion, and network performance bottlenecks.
