# Physical Layer Issues

The physical layer is responsible for transmitting raw bits across a physical medium. It includes network cables, fiber-optic links, connectors, transceivers, repeaters, and the electrical or optical signals used to carry data.

Physical layer problems can affect an entire network connection or cause intermittent connectivity, packet loss, reduced throughput, and unstable links.

Common physical layer symptoms include:

* Network interface showing `DOWN` or `NO-CARRIER`.
* Link repeatedly disconnecting and reconnecting.
* Unexpectedly low negotiated link speed.
* CRC/FCS errors and other interface errors.
* Intermittent packet loss.
* Poor throughput despite sufficient bandwidth.
* Devices restarting or disconnecting because of power issues.
* Fiber links failing due to incompatible or faulty optical components.

# 🗁 Common Causes

### 🖆 Damaged Ethernet Cables

Copper Ethernet cables can become damaged through excessive bending, crushing, stretching, poor installation, or repeated movement.

Common cable-related problems include:

* Broken or damaged internal conductors.
* Damaged RJ45 connectors.
* Loose connector contacts.
* Incorrect wire termination.
* Split pairs.
* Poor-quality patch cables.
* Excessive cable length.
* Electromagnetic interference from nearby electrical equipment.

A cable may still establish a link while producing errors or negotiating at a lower speed.

### 🖆 Ethernet Cable Categories

Different cable categories support different Ethernet speeds and frequency requirements.

| Cable category | Common Ethernet capabilities |
| --- | --- |
| Cat5e | 1 Gbps up to 100 meters |
| Cat6 | 1 Gbps up to 100 meters; 10 Gbps over shorter distances |
| Cat6a | 10 Gbps up to 100 meters |
| Cat7 | 10 Gbps up to 100 meters under its specified construction and installation requirements |
| Cat8 | 25/40 Gbps up to 30 meters |

For conventional twisted-pair Ethernet, the typical maximum channel length is 100 meters:

* Permanent link: up to 90 meters.
* Patch cords: typically up to 10 meters combined.

Actual capabilities depend on cable quality, installation, termination, and the Ethernet standard being used.

### 🖆 Poor Cable Termination

Incorrect termination can cause intermittent connectivity and transmission errors.

Typical issues include:

* Incorrect T568A or T568B wiring.
* Conductors not fully inserted into the connector.
* Excessive untwisting of cable pairs.
* Poor crimping.
* Damaged connector pins.

Both ends of a cable should use the intended wiring scheme. T568A-to-T568A and T568B-to-T568B are common straight-through configurations.

### 🖆 Electromagnetic Interference

Electromagnetic interference (EMI) can affect copper Ethernet transmission.

Potential sources include:

* Electric motors.
* Power cables.
* Transformers.
* Industrial equipment.
* Fluorescent lighting and other electrical devices.

Use appropriate cable types, separation from power wiring, grounding, and installation practices to reduce interference.

### 🖆 Fiber-Optic Problems

Fiber connections can fail because of:

* Dirty fiber connectors.
* Damaged fiber cables.
* Excessive bending.
* Incorrect transceiver type.
* Incompatible wavelengths.
* Incorrect fiber polarity.
* TX and RX connections being reversed.
* Excessive optical attenuation.
* Transceiver incompatibility.

Fiber connectors must be inspected and cleaned using appropriate fiber-cleaning equipment.

# 🗁 Link Status and Negotiation

### 🖆 Understanding Link Status

Ethernet interfaces generally have two important states:

* Administrative state: whether the interface has been enabled or disabled by configuration.
* Operational state: whether the interface currently has an active link.

A network interface can be administratively enabled but operationally down because of a disconnected cable, faulty port, or incompatible physical connection.

Link LEDs can provide useful clues, but their colors and blinking patterns vary by manufacturer.

### 🖆 Speed and Duplex Negotiation

Ethernet devices commonly use auto-negotiation to determine the highest mutually supported speed and duplex mode.

Examples of Ethernet speeds include:

* 10 Mbps
* 100 Mbps
* 1 Gbps
* 2.5 Gbps
* 5 Gbps
* 10 Gbps

Full-duplex Ethernet allows simultaneous transmission and reception. Half-duplex Ethernet allows communication in both directions, but not simultaneously.

A duplex mismatch can cause:

* Poor throughput.
* Increased frame errors.
* Late collisions on the half-duplex side.
* Retransmissions.
* Unstable application performance.

**Best practice:** Keep auto-negotiation enabled on both ends unless a specific compatibility requirement justifies manual configuration. If settings must be forced, ensure both ends use compatible speed and duplex settings.

# 🗁 Ethernet Interface Errors

Interface counters help identify physical transmission problems.

| Counter | Meaning | Possible cause |
| --- | --- | --- |
| CRC/FCS errors | Frames fail their frame check sequence | Cable faults, interference, damaged connectors |
| Alignment errors | Frames have invalid bit alignment | Physical transmission problems |
| Runts | Frames smaller than the minimum valid Ethernet frame size | Collisions or transmission faults |
| Giants | Frames larger than the configured maximum frame size | Oversized frames or configuration issues |
| Input errors | General errors receiving frames | Physical faults or interface problems |
| Output errors | Errors transmitting frames | Interface, hardware, or transmission problems |
| Carrier errors | Problems detecting or maintaining the carrier signal | Cable, transceiver, or physical connection |
| Collisions | Multiple devices transmit simultaneously on a shared half-duplex medium | Legacy half-duplex conditions |
| Drops | Packets discarded by the interface or its queues | Congestion, resource limits, or hardware issues |

Not every counter indicates a physical layer fault. For example, output drops can result from congestion rather than a defective cable.

Compare counters over time instead of relying only on their accumulated values since the device started.

# 🗁 Fiber-Optic Troubleshooting

### 🖆 Single-Mode and Multimode Fiber

Fiber-optic connections use light to transmit data.

| Feature | Single-mode fiber | Multimode fiber |
| --- | --- | --- |
| Core size | Typically 9 µm | Commonly 50 µm or 62.5 µm |
| Light source | Laser | VCSEL or other supported sources |
| Typical use | Longer-distance links | Shorter-distance links |
| Common types | OS1, OS2 | OM1, OM2, OM3, OM4, OM5 |

The fiber type must match the transceiver's supported medium and optical specifications.

### 🖆 Fiber Transceiver Compatibility

SFP and SFP+ modules provide optical or electrical interfaces for network devices.

Before using a transceiver, verify:

* Supported Ethernet speed.
* Single-mode or multimode fiber compatibility.
* Operating wavelength.
* Supported transmission distance.
* Connector type.
* Optical power budget.
* Device vendor compatibility.

A transceiver that physically fits a port is not necessarily compatible with the device or the other end of the link.

### 🖆 TX and RX Polarity

Duplex fiber connections use separate transmit and receive paths.

The transmit side of one device must connect to the receive side of the other device.

If TX connects to TX and RX connects to RX, the link may fail to establish.

Check fiber polarity and reverse the duplex connector positions when the wiring is incorrect.

### 🖆 Optical Power Levels

Optical transceivers may expose Digital Optical Monitoring (DOM), also known as Digital Diagnostic Monitoring (DDM).

Useful readings include:

* Transmit optical power.
* Receive optical power.
* Transceiver temperature.
* Supply voltage.
* Laser bias current.

Compare measured optical levels against the transceiver's supported operating range. A receive power level that is too low may indicate excessive attenuation, dirty connectors, damaged fiber, or excessive distance.

# 🗁 Power over Ethernet Issues

Power over Ethernet (PoE) supplies electrical power to network devices through Ethernet cabling.

Common powered devices include:

* Wireless access points.
* IP cameras.
* VoIP phones.
* Network sensors.
* Other PoE-compatible devices.

### 🖆 PoE Components

* **PSE (Power Sourcing Equipment):** Supplies power, such as a PoE switch or injector.
* **PD (Powered Device):** Receives power, such as an IP camera or access point.

### 🖆 Common PoE Problems

* The switch does not support the required PoE standard.
* The powered device requires more power than the port can provide.
* The switch has exhausted its total PoE power budget.
* The cable or connector is damaged.
* PoE is disabled on the port.
* The device has a hardware or power-management problem.

Common IEEE PoE standards include:

| Standard | Common name | Maximum PSE power |
| --- | --- | ---: |
| IEEE 802.3af | PoE | 15.4 W |
| IEEE 802.3at | PoE+ | 30 W |
| IEEE 802.3bt | PoE++ Type 3 | 60 W |
| IEEE 802.3bt | PoE++ Type 4 | 90 W |

These figures describe maximum power supplied by the PSE. The power available to the PD is lower because of cable losses.

Always verify device compatibility and available power before connecting PoE equipment.

# 🗁 Troubleshooting on Linux

Linux provides several tools for inspecting network interface state, negotiated speed, and hardware errors.

### 🖆 Inspect Network Interfaces

```bash
ip link show
```

Inspect a specific interface:

```bash
ip link show dev enp3s0
```

Look for:

* `UP`: administratively enabled.
* `LOWER_UP`: physical link detected.
* `NO-CARRIER`: no physical carrier detected.

### 🖆 Check Negotiated Speed and Duplex

Install `ethtool`:

```bash
sudo apt update
sudo apt install ethtool
```

Inspect interface information:

```bash
sudo ethtool enp3s0
```

Important fields include:

* Speed
* Duplex
* Auto-negotiation
* Link detected

Example:

```text
Speed: 1000Mb/s
Duplex: Full
Auto-negotiation: on
Link detected: yes
```

If the interface unexpectedly negotiates at 100 Mbps instead of 1 Gbps, investigate the cable, connectors, port capabilities, and negotiation settings.

### 🖆 Inspect Interface Statistics

```bash
ip -s link show dev enp3s0
```

Inspect hardware-specific counters:

```bash
sudo ethtool -S enp3s0
```

Look for increasing values related to:

* CRC errors.
* Receive errors.
* Transmit errors.
* Dropped packets.
* Carrier errors.

Counter names vary by network driver.

### 🖆 Inspect Kernel Logs

```bash
dmesg | grep -iE 'ethernet|link|network|firmware'
```

Inspect kernel logs using systemd:

```bash
sudo journalctl -k
```

Filter for interface-related messages:

```bash
sudo journalctl -k | grep -iE 'link up|link down|ethernet'
```

Repeated link-up and link-down messages may indicate a faulty cable, unstable port, transceiver problem, or device power issue.

### 🖆 Identify Network Hardware

```bash
lspci -nnk | grep -A3 -i ethernet
```

This helps identify the network controller and its associated kernel driver.

# 🗁 Troubleshooting on MikroTik RouterOS v7

RouterOS provides interface monitoring and Ethernet statistics for diagnosing physical connection problems.

### 🖆 Inspect Interface Status

```routeros
/interface print
```

Check a specific Ethernet interface:

```routeros
/interface ethernet monitor ether1 once
```

Review relevant properties such as:

* Link status.
* Rate.
* Full-duplex state.
* Auto-negotiation state.
* Supported link modes.

### 🖆 Inspect Ethernet Statistics

```routeros
/interface ethernet print stats
```

Review available counters for errors and transmission problems.

### 🖆 Monitor Traffic

```routeros
/interface monitor-traffic ether1
```

This displays real-time traffic statistics. It can help establish whether traffic is flowing, but it does not independently prove that the physical link is healthy.

### 🖆 Review System Logs

```routeros
/log print
```

Look for interface events indicating link changes or repeated disconnections.

### 🖆 Check PoE Status

On supported MikroTik devices:

```routeros
/interface ethernet poe print
```

Inspect the PoE status and power information available for the relevant interface.

Command availability and displayed properties depend on the device model and RouterOS version.

# 🗁 Troubleshooting on Cisco IOS

Cisco switches provide interface status and error counters.

### 🖆 Check Interface Status

```cisco
show interfaces status
```

Inspect a specific interface:

```cisco
show interfaces GigabitEthernet0/1
```

Review:

* Interface status.
* Line protocol.
* Speed and duplex.
* Input errors.
* CRC errors.
* Output errors.
* Interface resets.
* Collision counters.

### 🖆 Check Device Logs

```cisco
show logging
```

Look for repeated interface state changes and transceiver-related messages.

# 🗁 Physical Layer Troubleshooting Workflow

Use a systematic approach instead of replacing hardware randomly.

### 🖆 Verify the Physical Connection

* Confirm that both devices are powered on.
* Check whether the network interfaces are enabled.
* Inspect link LEDs on both devices.
* Verify that cables are connected securely.
* Check for damaged connectors and visible cable damage.
* Confirm that the correct ports are being used.

### 🖆 Check Interface Negotiation

* Inspect the negotiated speed.
* Verify duplex settings.
* Check auto-negotiation status.
* Compare both ends of the connection.
* Investigate unexpected speed reductions or duplex mismatches.

### 🖆 Inspect Interface Counters

Record the current interface counters.

Wait for a reasonable observation period and inspect them again.

If CRC or physical receive errors continue increasing, investigate the cable, connector, transceiver, and physical port.

If only output drops increase under heavy traffic, investigate congestion and queue capacity before assuming a physical fault.

### 🖆 Replace One Component at a Time

Use a known-good component to isolate the fault.

A practical testing order is:

* Replace the Ethernet patch cable.
* Test another switch port.
* Test another network interface if available.
* Replace the fiber patch cord.
* Test with a known-compatible transceiver.
* Check the remote device.

Changing one component at a time makes it easier to identify the actual cause.

### 🖆 Test the Cable

For copper cabling, use an appropriate cable tester.

Basic testers can identify:

* Open conductors.
* Short circuits.
* Reversed pairs.
* Incorrect wire mapping.

A cable certifier can provide more detailed measurements, including insertion loss, return loss, and crosstalk.

For fiber links, use appropriate optical inspection and testing equipment, such as:

* Fiber inspection microscope.
* Optical power meter.
* Light source.
* OTDR for locating faults along a fiber path.

# 🗁 Practical Troubleshooting Scenarios

### 🖆 Scenario: Link Negotiates at 100 Mbps Instead of 1 Gbps

**Symptoms:**

* Both devices support Gigabit Ethernet.
* The interface connects successfully.
* Negotiated speed remains at 100 Mbps.

**Possible causes:**

* Damaged cable conductors.
* Poor RJ45 termination.
* A cable that does not meet the required specification.
* A port limited to 100 Mbps.
* Incorrect negotiation configuration.

**Troubleshooting:**

```bash
sudo ethtool enp3s0
```

Replace the cable with a known-good Cat5e or better cable and inspect both port configurations.

### 🖆 Scenario: Frequent Link Disconnections

**Symptoms:**

* Network connectivity repeatedly disappears.
* Interface logs show link-down and link-up events.
* The connection works temporarily after reconnecting the cable.

**Possible causes:**

* Damaged cable.
* Loose connector.
* Faulty switch port.
* Unstable transceiver.
* Device power problems.

**Troubleshooting:**

* Inspect the cable and connectors.
* Replace the patch cable.
* Test another switch port.
* Review interface logs.
* Check device power stability.

### 🖆 Scenario: Increasing CRC Errors

**Symptoms:**

* Connectivity remains active.
* CRC counters increase over time.
* Packet loss or application performance problems occur.

**Possible causes:**

* Cable damage.
* EMI.
* Poor termination.
* Faulty transceiver.
* Defective interface hardware.

**Troubleshooting:**

Inspect both ends of the link, replace the cable, and compare error counters before and after the replacement.

### 🖆 Scenario: Fiber Link Is Down

**Symptoms:**

* Fiber transceivers are installed.
* The interface does not establish a link.
* Optical receive power is absent or outside the supported range.

**Possible causes:**

* TX/RX polarity mismatch.
* Dirty connectors.
* Incompatible transceivers.
* Incorrect wavelength.
* Broken fiber.
* Excessive optical attenuation.

**Troubleshooting:**

* Verify transceiver compatibility at both ends.
* Inspect and clean fiber connectors.
* Check TX/RX polarity.
* Inspect DOM/DDM readings.
* Test with known-good fiber patch cords.

# 🗁 Best Practices

* Use certified Ethernet cabling appropriate for the required speed and distance.
* Keep copper Ethernet channel lengths within the applicable standard.
* Avoid excessive cable bending and mechanical stress.
* Separate copper data cables from sources of electrical interference.
* Label cables and patch-panel connections.
* Keep fiber connectors clean and protected.
* Verify transceiver compatibility before deployment.
* Monitor interface errors and link state changes.
* Keep auto-negotiation consistent across connected devices.
* Check PoE standards and power budgets before deploying powered devices.
* Maintain spare patch cables and compatible transceivers.
* Document physical connections and port assignments.

# 🗁 Troubleshooting Checklist

* [ ] Verify both devices are powered on.
* [ ] Confirm both interfaces are enabled.
* [ ] Check link LEDs and operational interface state.
* [ ] Inspect cables, connectors, and patch panels.
* [ ] Verify cable type and maximum distance.
* [ ] Check negotiated speed and duplex.
* [ ] Inspect CRC, FCS, and other interface counters.
* [ ] Review interface logs for link flapping.
* [ ] Test using a known-good cable or port.
* [ ] Verify fiber type, polarity, wavelength, and transceiver compatibility when applicable.
* [ ] Check optical power levels on fiber links.
* [ ] Verify PoE compatibility and power budget when applicable.
* [ ] Record the root cause and corrective action.

# 🗁 Summary

Physical layer troubleshooting focuses on the hardware and transmission medium responsible for carrying network signals.

The most common causes are damaged cables, poor termination, incompatible transceivers, fiber connection problems, incorrect speed or duplex negotiation, and insufficient PoE power.

A reliable troubleshooting process combines physical inspection, interface-state verification, negotiation checks, error-counter analysis, and controlled component replacement.
