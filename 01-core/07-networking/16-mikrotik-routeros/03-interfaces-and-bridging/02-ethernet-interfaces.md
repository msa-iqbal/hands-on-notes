# MikroTik Ethernet Interfaces

Ethernet interfaces are physical network ports that allow MikroTik routers to communicate with other devices through wired connections.

They are commonly used to connect:

* Internet service provider (ISP) uplinks.
* Local area network (LAN) switches.
* Servers and workstations.
* Wireless access points.
* Other routers.
* Network monitoring and management devices.

RouterOS provides Ethernet interface configuration and monitoring tools for managing link speed, auto-negotiation, duplex settings, flow control, PoE, and interface statistics.

This document focuses on physical Ethernet interfaces in RouterOS v7.

# 🗁 Understanding Ethernet Interfaces

A MikroTik router may have several Ethernet ports, depending on its hardware model.

Common default interface names include:

```text
ether1
ether2
ether3
ether4
ether5
```

Some devices also include:

* SFP ports for fiber-optic transceivers.
* SFP+ ports for higher-speed fiber or DAC connections.
* Ethernet ports with PoE input or output support.
* Multi-gigabit Ethernet ports.

The interface name does not necessarily indicate its supported speed. Always check the hardware specifications and RouterOS interface properties.

# 🗁 Viewing Ethernet Interfaces

Display all Ethernet interfaces:

```routeros
/interface ethernet print
```

Display detailed Ethernet interface information:

```routeros
/interface ethernet print detail
```

Display one specific interface:

```routeros
/interface ethernet print detail where name=ether1
```

These commands help identify interface properties such as:

* Interface name.
* MAC address.
* Auto-negotiation status.
* Supported speed settings.
* Link status.
* Flow-control configuration.
* PoE-related properties, where supported.

Available properties depend on the router hardware and Ethernet driver.

# 🗁 Understanding Ethernet Link Status

Ethernet link status indicates whether a physical connection has been established between two devices.

Monitor an Ethernet interface:

```routeros
/interface ethernet monitor ether1
```

Display the status once:

```routeros
/interface ethernet monitor ether1 once
```

Typical monitoring information includes:

| Property | Description |
| --- | --- |
| `status` | Physical link status |
| `auto-negotiation` | Whether automatic link negotiation is enabled |
| `rate` | Negotiated link speed |
| `full-duplex` | Whether the link operates in full-duplex mode |
| `tx-flow-control` | Transmit flow-control status |
| `rx-flow-control` | Receive flow-control status |
| `advertising` | Advertised link capabilities, where supported |
| `link-partner-advertising` | Capabilities advertised by the connected device, where supported |

The exact output varies by interface and hardware platform.

A connected Ethernet interface should normally report an active link and a negotiated speed.

# 🗁 Understanding Ethernet Speed

Ethernet speed represents the data rate negotiated between two connected Ethernet devices.

Common Ethernet speeds include:

| Ethernet Standard | Speed |
| --- | ---: |
| Fast Ethernet | 100 Mbps |
| Gigabit Ethernet | 1 Gbps |
| 2.5 Gigabit Ethernet | 2.5 Gbps |
| 5 Gigabit Ethernet | 5 Gbps |
| 10 Gigabit Ethernet | 10 Gbps |
| 25 Gigabit Ethernet | 25 Gbps |
| 40 Gigabit Ethernet | 40 Gbps |
| 100 Gigabit Ethernet | 100 Gbps |

Not every MikroTik router supports every speed.

The actual connection speed depends on both connected devices, the physical medium, transceiver compatibility, and negotiated capabilities.

For example:

```text
Router Ethernet Port: 1 Gbps
Switch Ethernet Port: 1 Gbps

Negotiated Link Speed: 1 Gbps
```

If one device supports only 100 Mbps, the connection may negotiate at 100 Mbps instead.

# 🗁 Understanding Auto-Negotiation

Auto-negotiation allows two Ethernet devices to automatically determine compatible link capabilities.

These capabilities can include:

* Link speed.
* Duplex mode.
* Flow-control capabilities.

Enable auto-negotiation:

```routeros
/interface ethernet set ether1 auto-negotiation=yes
```

Verify the configuration:

```routeros
/interface ethernet print detail where name=ether1
```

Monitor the negotiated link:

```routeros
/interface ethernet monitor ether1 once
```

Auto-negotiation is generally recommended because it allows connected devices to select a mutually supported configuration.

Disabling it without coordinating the configuration on the connected device can cause connectivity problems.

# 🗁 Understanding Duplex Mode

Duplex determines whether an Ethernet interface can transmit and receive data simultaneously.

**Half-duplex**

A half-duplex connection can transmit or receive data at a given time, but not both simultaneously.

**Full-duplex**

A full-duplex connection can transmit and receive data simultaneously.

Modern switched Ethernet networks generally use full-duplex operation.

Inspect duplex status:

```routeros
/interface ethernet monitor ether1 once
```

Look for the reported `full-duplex` property.

A duplex mismatch between connected devices can cause:

* Poor network performance.
* Packet errors.
* Retransmissions.
* Unexpected latency.
* Communication instability.

Use compatible settings on both sides of an Ethernet connection.

# 🗁 Configuring Ethernet Speed

In most networks, auto-negotiation should remain enabled.

For supported interfaces, RouterOS also allows administrators to configure the advertised link capabilities.

Inspect the current configuration:

```routeros
/interface ethernet print detail where name=ether1
```

Example of configuring advertised capabilities:

```routeros
/interface ethernet set ether1 advertise=10M-baseT-half,10M-baseT-full,100M-baseT-half,100M-baseT-full,1G-baseT-half,1G-baseT-full
```

The supported advertisement values depend on the Ethernet interface and RouterOS version.

Verify the result:

```routeros
/interface ethernet monitor ether1 once
```

**Important:** Do not copy advertisement settings blindly. Use only capabilities supported by the physical interface and the connected device. Some interfaces expose different speed configuration options.

# 🗁 Understanding Flow Control

Ethernet flow control helps manage congestion between directly connected Ethernet devices.

It uses PAUSE frames to request that the connected device temporarily stop transmitting Ethernet frames.

RouterOS provides receive and transmit flow-control settings on supported Ethernet interfaces.

Inspect flow-control status:

```routeros
/interface ethernet monitor ether1 once
```

Configure receive flow control:

```routeros
/interface ethernet set ether1 rx-flow-control=on
```

Configure transmit flow control:

```routeros
/interface ethernet set ether1 tx-flow-control=on
```

Disable both:

```routeros
/interface ethernet set ether1 rx-flow-control=off tx-flow-control=off
```

Possible settings and defaults depend on hardware support.

Flow control should be configured according to the network design. Enabling it everywhere does not automatically improve performance and can sometimes contribute to congestion problems.

# 🗁 Understanding Ethernet MTU

MTU stands for Maximum Transmission Unit.

For a standard Ethernet interface, the common IP MTU is 1500 bytes.

Inspect the interface:

```routeros
/interface ethernet print detail where name=ether1
```

Configure the MTU:

```routeros
/interface ethernet set ether1 mtu=1500
```

MTU is important when working with:

* VLAN tagging.
* PPPoE.
* VPN tunnels.
* MPLS.
* Other encapsulation technologies.

Changing MTU without considering the complete network path can cause packet loss or connectivity problems.

# 🗁 Understanding MAC Address

Every Ethernet interface uses a MAC address for Layer 2 communication.

Display Ethernet MAC addresses:

```routeros
/interface ethernet print
```

Display detailed information:

```routeros
/interface ethernet print detail
```

A typical MAC address looks like:

```text
AA:BB:CC:DD:EE:FF
```

Configure a MAC address on a supported interface:

```routeros
/interface ethernet set ether2 mac-address=02:11:22:33:44:55
```

Verify the configuration:

```routeros
/interface ethernet print detail where name=ether2
```

Use manually configured MAC addresses carefully. Duplicate MAC addresses within the same Layer 2 network can create forwarding problems.

# 🗁 Understanding Ethernet Interface Statistics

Ethernet statistics help administrators identify traffic levels and possible interface problems.

Display Ethernet interface statistics:

```routeros
/interface ethernet print stats
```

Display general interface statistics:

```routeros
/interface print stats
```

Monitor live traffic:

```routeros
/interface monitor-traffic ether1
```

Common statistics include:

| Statistic | Description |
| --- | --- |
| RX Bytes | Total received data |
| TX Bytes | Total transmitted data |
| RX Packets | Total received packets |
| TX Packets | Total transmitted packets |
| RX Errors | Receive-side errors |
| TX Errors | Transmit-side errors |
| RX Drops | Received packets dropped |
| TX Drops | Transmitted packets dropped |

The exact counters available depend on the device and interface driver.

Growing error counters may indicate a physical-layer problem, incompatible link settings, congestion, or another interface-specific issue.

# 🗁 Understanding PoE

PoE stands for Power over Ethernet.

It allows compatible network devices to receive electrical power through an Ethernet cable.

Common PoE-powered devices include:

* Wireless access points.
* IP cameras.
* VoIP phones.
* Network switches.
* Other compatible network equipment.

Some MikroTik devices support PoE input, PoE output, or both.

Inspect Ethernet interface properties:

```routeros
/interface ethernet print detail
```

On supported devices, monitor PoE status:

```routeros
/interface ethernet poe monitor ether2 once
```

Configure PoE output on a supported port:

```routeros
/interface ethernet poe set ether2 poe-out=auto-on
```

Possible PoE output modes depend on the hardware model. Common modes include:

| Mode | Description |
| --- | --- |
| `off` | Disable PoE output |
| `auto-on` | Enable output when a compatible powered device is detected |
| `forced-on` | Force PoE output regardless of device detection, where supported |

Verify the available options before configuring a port:

```routeros
/interface ethernet poe print
```

**Important:** Confirm the voltage, polarity, power budget, and PoE standard supported by both devices before connecting powered equipment.

# 🗁 Understanding Ethernet Cable Types

Ethernet interfaces use copper cables or compatible optical connections.

Common copper Ethernet cable categories include:

| Cable Category | Common Applications |
| --- | --- |
| Cat5e | 1 Gbps Ethernet |
| Cat6 | 1 Gbps and supported 10 Gbps installations |
| Cat6a | 10 Gbps Ethernet |
| Cat7 | Shielded high-performance cabling |
| Cat8 | Short-distance high-speed data-center connections |

Actual performance depends on cable quality, length, installation, connectors, and the Ethernet standard being used.

For fiber connections, Ethernet interfaces may use SFP or SFP+ transceivers instead of copper RJ45 connectors.

# 🗁 Understanding SFP and SFP+

SFP and SFP+ are modular transceiver form factors used for network connectivity.

**SFP**

Commonly used for 1 Gbps fiber or copper connections.

**SFP+**

Commonly used for 10 Gbps connections.

SFP and SFP+ interfaces may support different transceiver types, depending on the device.

Common optical transceiver types include:

| Transceiver | Typical Application |
| --- | --- |
| 1000BASE-SX | Short-range multimode fiber |
| 1000BASE-LX | Longer-range single-mode or supported multimode fiber |
| 10GBASE-SR | Short-range 10 Gbps multimode fiber |
| 10GBASE-LR | Longer-range 10 Gbps single-mode fiber |

Inspect available interfaces:

```routeros
/interface ethernet print
```

Monitor an SFP interface:

```routeros
/interface ethernet monitor sfp1 once
```

Some MikroTik devices expose SFP diagnostics, including optical transmit and receive power, temperature, and voltage, when supported by the transceiver.

Use compatible transceivers and fiber types at both ends of the connection.

# 🗁 Disabling and Enabling Ethernet Ports

Disable an Ethernet interface:

```routeros
/interface ethernet disable ether3
```

Enable it again:

```routeros
/interface ethernet enable ether3
```

Verify its status:

```routeros
/interface ethernet print
```

Disabling a physical Ethernet port interrupts communication through that port.

Before disabling an interface, check whether it carries management, WAN, LAN, or other critical traffic.

# 🗁 Ethernet Interface Troubleshooting

**Link is down**

Check:

```routeros
/interface ethernet monitor ether1 once
```

Possible causes:

* Disconnected cable.
* Damaged cable or connector.
* Remote device powered off.
* Incorrect transceiver.
* Unsupported link speed.
* Faulty switch port.

**Link speed is lower than expected**

Check:

```routeros
/interface ethernet monitor ether1 once
```

Verify:

* Both devices support the expected speed.
* Cable category and condition.
* Auto-negotiation settings.
* Transceiver compatibility.
* Switch port configuration.

**High RX or TX errors**

Check:

```routeros
/interface ethernet print stats
```

Inspect cable quality, connectors, transceivers, negotiated speed, and duplex configuration.

**Interface is running but traffic is not passing**

Check:

```routeros
/interface print
/interface ethernet print stats
/ip address print
/ip route print
```

Also inspect bridge membership, VLAN configuration, firewall rules, and routing.

**PoE device is not powering on**

Check:

```routeros
/interface ethernet poe print
/interface ethernet poe monitor ether2 once
```

Verify the device's power requirements, cable condition, port PoE support, and available power budget.

# 🗁 Practical Lab: Ethernet Interface Configuration

This lab demonstrates basic Ethernet interface inspection and configuration.

**Lab requirements:**

* MikroTik CHR or physical MikroTik router.
* At least two Ethernet interfaces on physical hardware, or multiple virtual Ethernet interfaces in CHR.
* WinBox or SSH access.

**Inspect Ethernet interfaces**

```routeros
/interface ethernet print
/interface ethernet print detail
```

**Inspect the first Ethernet port**

```routeros
/interface ethernet monitor ether1 once
```

**Rename an Ethernet interface**

```routeros
/interface ethernet set ether2 name=LAN
```

**Add a comment**

```routeros
/interface ethernet set LAN comment="Main LAN Connection"
```

**Configure MTU**

```routeros
/interface ethernet set LAN mtu=1500
```

**Inspect interface statistics**

```routeros
/interface ethernet print stats
```

**Monitor live traffic**

```routeros
/interface monitor-traffic LAN
```

**Disable and enable the interface**

```routeros
/interface ethernet disable LAN
/interface ethernet enable LAN
```

**Verify the final configuration**

```routeros
/interface ethernet print detail
/interface ethernet monitor ether1 once
/interface ethernet print stats
```

**Lab objectives:**

* Identify physical Ethernet interfaces.
* Inspect link status and negotiated speed.
* Understand auto-negotiation and duplex.
* Configure interface names and comments.
* Understand MTU and MAC addresses.
* Inspect Ethernet traffic statistics.
* Practice enabling and disabling Ethernet ports.

# 🗁 Ethernet Interface Best Practices

* Keep auto-negotiation enabled unless a specific network requirement dictates otherwise.
* Use compatible Ethernet speed and duplex settings.
* Select appropriate cables for the required link speed.
* Use supported SFP and SFP+ transceivers.
* Monitor interface errors and traffic counters.
* Document Ethernet port connections.
* Avoid unnecessary MAC address changes.
* Configure MTU consistently across the network path.
* Verify PoE compatibility before powering external devices.
* Avoid disabling interfaces that carry management or production traffic.

# 🗁 Official Documentation

* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [MikroTik Ethernet Documentation](https://help.mikrotik.com/docs/spaces/ROS/pages/8323191/Ethernet)
* [MikroTik RouterOS Interface Documentation](https://help.mikrotik.com/docs/)
