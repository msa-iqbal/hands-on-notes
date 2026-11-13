# MTC-SWE — MikroTik Certified Switching Engineer

MikroTik switching technologies provide Layer two connectivity between network devices. RouterOS supports software-based bridging and, on compatible hardware, hardware-offloaded switching.

Switching is an essential part of enterprise networks, data centers, wireless infrastructure, and Internet Service Provider networks.

A MikroTik switching engineer should understand how Ethernet frames are forwarded, how MAC addresses are learned, how VLANs provide network segmentation, and how switching loops are prevented.

This roadmap focuses on the switching concepts and RouterOS features required to design, configure, secure, and troubleshoot MikroTik switching environments.

# 🗁 Learning Objectives

After completing this roadmap, you should be able to:

* Understand Ethernet switching fundamentals.
* Explain MAC address learning and frame forwarding.
* Configure RouterOS bridges.
* Understand hardware and software switching.
* Design VLAN-based network segmentation.
* Configure access and trunk ports.
* Implement VLAN filtering.
* Configure inter-VLAN communication.
* Understand Spanning Tree Protocol.
* Configure link aggregation.
* Apply Layer two security controls.
* Monitor switching performance.
* Troubleshoot VLAN and bridge connectivity issues.
* Design practical MikroTik switching environments.

# 🗁 Ethernet Switching Fundamentals

### 🖆 What Is Switching?

Switching is the process of forwarding Ethernet frames between network interfaces based on destination MAC addresses.

A switch connects devices within a Layer two network and learns which devices are reachable through its ports.

For example:

```text
PC-A ─────┐
          │
PC-B ─────┤
          │
       SWITCH ───── Router
          │
PC-C ─────┤
          │
Server ───┘
```

When PC-A sends a frame to PC-B, the switch uses its MAC address table to determine the appropriate outgoing port.

### 🖆 Layer Two Switching

Ethernet switching operates primarily at the Data Link layer of the OSI model.

Important Ethernet frame fields include:

* Destination MAC address.
* Source MAC address.
* EtherType or Length.
* Payload.
* Frame Check Sequence.

Understand how switches use MAC addresses to forward frames without requiring IP routing for communication within the same VLAN.

### 🖆 Collision Domains and Broadcast Domains

**Collision domain**

A collision domain is a network segment where simultaneous transmissions can collide. Modern full-duplex switched Ethernet largely eliminates traditional Ethernet collisions.

**Broadcast domain**

A broadcast domain is the network area in which Layer two broadcast frames are distributed.

VLANs divide a switched network into separate broadcast domains.

### 🖆 Types of Ethernet Frames

Study:

* Unicast frames.
* Broadcast frames.
* Multicast frames.
* Tagged Ethernet frames.
* Untagged Ethernet frames.

Understand how switches handle each frame type.

# 🗁 MAC Address Learning and Forwarding

### 🖆 MAC Address Table

A switch maintains a forwarding database containing learned MAC addresses and their associated ports and VLANs.

Example:

| MAC Address | VLAN | Port |
| --- | ---: | --- |
| AA:AA:AA:AA:AA:01 | 10 | ether2 |
| BB:BB:BB:BB:BB:02 | 10 | ether3 |
| CC:CC:CC:CC:CC:03 | 20 | ether4 |

The switch uses this information to forward known unicast frames.

### 🖆 MAC Learning Process

A switch learns a source MAC address when it receives a frame.

The general process is:

* Receive an Ethernet frame.
* Read the source MAC address.
* Associate the source MAC with the receiving port and VLAN.
* Update the forwarding database.
* Use the destination MAC address to determine the outgoing port.

### 🖆 Frame Forwarding Behavior

Understand three common forwarding behaviors:

**Known unicast**

The destination MAC address is already present in the forwarding database. The switch forwards the frame through the associated port.

**Unknown unicast**

The destination MAC address is not present in the forwarding database. The switch floods the frame within the relevant VLAN, excluding the receiving port.

**Broadcast**

The frame is forwarded to the other eligible ports within the same broadcast domain.

### 🖆 MAC Table Aging

Dynamic MAC entries are typically removed after an aging period if they are not refreshed.

This allows the forwarding database to adapt when devices move between ports or disappear from the network.

Inspect the MAC address table on MikroTik:

```routeros
/interface bridge host print
```

# 🗁 Bridge Fundamentals

### 🖆 What Is a Bridge?

A bridge connects multiple network interfaces into a single Layer two forwarding domain.

A RouterOS bridge can connect Ethernet interfaces, VLAN interfaces, wireless interfaces, and other supported interface types.

Example:

```text
             MikroTik Bridge
          ┌────────────────────┐
          │                    │
       ether2                ether3
          │                    │
         PC-A                 PC-B
```

Devices connected to the bridge can communicate at Layer two when their VLAN and forwarding configurations permit it.

### 🖆 Software Bridging

Software bridging processes frames through the router's CPU.

It provides flexible forwarding and configuration capabilities, but performance depends on CPU resources and traffic processing requirements.

### 🖆 Hardware-Offloaded Bridging

Compatible MikroTik devices can offload supported switching operations to their switch chip.

Benefits include:

* Reduced CPU utilization.
* Higher switching throughput.
* Improved packet-forwarding efficiency.

Hardware offloading depends on the device model, switch chip, interface configuration, and supported feature combinations.

### 🖆 Bridge Port Concepts

Understand:

* Bridge interface.
* Bridge ports.
* Port priority.
* Path cost.
* Horizon.
* Edge ports.
* BPDU handling.
* VLAN filtering.
* Hardware offloading.

# 🗁 RouterOS Bridge Configuration

### 🖆 Create a Bridge

Create a bridge:

```routeros
/interface bridge
add name=bridge-lan
```

Add Ethernet interfaces:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2
add bridge=bridge-lan interface=ether3
add bridge=bridge-lan interface=ether4
```

The interfaces now participate in the same bridge forwarding domain, subject to the bridge configuration.

### 🖆 Assign an IP Address to a Bridge

When an IP address is required for Layer three communication, it is normally assigned to the bridge interface rather than an individual bridged Ethernet port.

Example:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-lan
```

### 🖆 Inspect Bridge Configuration

```routeros
/interface bridge print detail
/interface bridge port print detail
/interface bridge host print
```

### 🖆 Bridge Port Isolation

Port isolation can restrict communication between selected bridge ports.

Understand the `horizon` property and how it can be used to prevent communication between ports assigned to the same isolation group.

Example:

```routeros
/interface bridge port
set [find interface=ether2] horizon=10
set [find interface=ether3] horizon=10
```

Test the resulting forwarding behavior carefully. Port isolation should be designed around the intended network topology.

# 🗁 VLAN Fundamentals

### 🖆 What Is a VLAN?

A Virtual Local Area Network logically divides a physical switching infrastructure into separate Layer two networks.

For example:

| VLAN ID | Name | Network |
| ---: | --- | --- |
| 10 | Management | 192.168.10.0/24 |
| 20 | Staff | 192.168.20.0/24 |
| 30 | Guest | 192.168.30.0/24 |
| 40 | Servers | 192.168.40.0/24 |

Even when these VLANs share the same physical switch, they remain separate Layer two broadcast domains.

### 🖆 IEEE 802.1Q

IEEE 802.1Q defines VLAN tagging for Ethernet frames.

A VLAN tag contains:

* Tag Protocol Identifier (TPID).
* Priority Code Point (PCP).
* Drop Eligible Indicator (DEI).
* VLAN Identifier (VID).

The VLAN ID is 12 bits, providing 4094 commonly usable VLAN identifiers.

VLAN IDs 0 and 4095 have reserved meanings.

### 🖆 Tagged and Untagged Frames

**Tagged frame**

Contains an 802.1Q VLAN tag identifying the VLAN.

**Untagged frame**

Does not contain an 802.1Q VLAN tag.

Switch ports can be configured to receive and transmit tagged or untagged traffic according to their role.

### 🖆 VLAN Advantages

* Network segmentation.
* Broadcast-domain separation.
* Improved traffic organization.
* Better access control.
* Easier management of large networks.
* Logical separation without requiring separate physical switches.

# 🗁 RouterOS VLAN Configuration

### 🖆 VLAN-Aware Bridge

RouterOS supports VLAN-aware bridging through bridge VLAN filtering.

This allows a single bridge to handle multiple VLANs using a VLAN membership table.

### 🖆 Basic VLAN Configuration

Consider the following topology:

```text
                 MikroTik Switch
                 ┌──────────────┐
PC-A ── ether2 ──┤ VLAN 10      │
PC-B ── ether3 ──┤ VLAN 20      │
Router ─ ether1 ─┤ Trunk        │
                 └──────────────┘
```

Assume:

* ether1 connects to another VLAN-aware router or switch.
* ether2 is an access port for VLAN 10.
* ether3 is an access port for VLAN 20.

Create the bridge:

```routeros
/interface bridge
add name=bridge1 vlan-filtering=no
```

Add the ports:

```routeros
/interface bridge port
add bridge=bridge1 interface=ether1
add bridge=bridge1 interface=ether2 pvid=10
add bridge=bridge1 interface=ether3 pvid=20
```

Configure VLAN membership:

```routeros
/interface bridge vlan
add bridge=bridge1 tagged=bridge1,ether1 vlan-ids=10
add bridge=bridge1 tagged=bridge1,ether1 vlan-ids=20
```

Enable VLAN filtering:

```routeros
/interface bridge
set bridge1 vlan-filtering=yes
```

The `bridge1` interface is included as a tagged member because it is required for VLAN-tagged communication with the CPU when the bridge itself participates in those VLANs.

The `pvid` property classifies untagged ingress frames into the specified VLAN.

### 🖆 Configure Management VLAN Interface

If the MikroTik device needs an IP address in VLAN 10, create a VLAN interface on the bridge:

```routeros
/interface vlan
add name=vlan10-management interface=bridge1 vlan-id=10
```

Assign an IP address:

```routeros
/ip address
add address=192.168.10.2/24 interface=vlan10-management
```

Ensure the VLAN membership table permits VLAN 10 to reach the bridge CPU.

### 🖆 Verify VLAN Configuration

```routeros
/interface bridge vlan print detail
/interface bridge port print detail
/interface bridge print detail
```

Check:

* VLAN IDs.
* Tagged ports.
* Untagged ports.
* PVID values.
* VLAN filtering state.
* Hardware offloading status.

### 🖆 Important Configuration Considerations

Enable VLAN filtering only after the bridge VLAN table and port configuration have been prepared.

Incorrect VLAN filtering can disconnect management access or interrupt network connectivity.

For production devices, use a controlled deployment process and maintain an out-of-band recovery path.

# 🗁 Access Ports and Trunk Ports

### 🖆 Access Port

An access port normally connects an endpoint that sends and receives untagged Ethernet frames.

Examples:

* Desktop computers.
* Printers.
* IP cameras.
* Standard servers.
* Access points configured for a single untagged network.

Example:

```routeros
/interface bridge port
set [find interface=ether2] pvid=10
```

The corresponding VLAN table entry must list ether2 as an untagged member of VLAN 10.

### 🖆 Trunk Port

A trunk port carries multiple VLANs using tagged Ethernet frames.

Examples:

* Switch-to-switch links.
* Router-to-switch links.
* Hypervisor connections.
* VLAN-aware wireless access points.

Example:

```routeros
/interface bridge vlan
add bridge=bridge1 tagged=bridge1,ether1 vlan-ids=10,20
```

### 🖆 Native VLAN and PVID

A native VLAN commonly refers to the VLAN carried untagged on a trunk.

In RouterOS bridge configuration, the PVID determines the VLAN assigned to untagged ingress frames.

Understand how the PVID, ingress filtering, frame-type restrictions, and VLAN table interact.

### 🖆 Restrict Tagged and Untagged Traffic

For an access port:

```routeros
/interface bridge port
set [find interface=ether2] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged
```

For a tagged trunk:

```routeros
/interface bridge port
set [find interface=ether1] ingress-filtering=yes frame-types=admit-only-vlan-tagged
```

These are examples for specific port roles. Configure them according to the actual topology and required management traffic.

# 🗁 VLAN Filtering and Security

### 🖆 VLAN Filtering

VLAN filtering ensures that frames are forwarded only through ports that are members of the relevant VLAN.

It provides the basis for enforcing VLAN separation within a RouterOS bridge.

### 🖆 Ingress Filtering

Ingress filtering checks whether an incoming VLAN-tagged frame belongs to a VLAN permitted on the receiving bridge port.

This helps prevent unauthorized VLAN traffic from entering through a port.

### 🖆 Frame Type Restrictions

RouterOS supports bridge-port frame-type restrictions.

Understand these settings:

* `admit-all`
* `admit-only-untagged-and-priority-tagged`
* `admit-only-vlan-tagged`

Select the appropriate mode for access and trunk interfaces.

### 🖆 VLAN Security Best Practices

* Assign access ports to explicit VLANs.
* Permit only required VLANs on trunk ports.
* Enable ingress filtering.
* Restrict management VLAN access.
* Avoid unnecessary untagged traffic.
* Disable unused switch ports.
* Review PVID assignments.
* Prevent unauthorized inter-VLAN communication.
* Document VLAN membership.

# 🗁 Inter-VLAN Communication

### 🖆 Inter-VLAN Routing

VLANs create separate Layer two broadcast domains.

Communication between different VLANs requires Layer three routing.

A MikroTik router can provide inter-VLAN routing through VLAN interfaces and IP addressing.

Example:

```text
VLAN 10 ─────┐
             │
             │
          MikroTik
             │
             │
VLAN 20 ─────┘
```

### 🖆 Configure VLAN Interfaces

Assume the bridge is named `bridge1`.

Create VLAN interfaces:

```routeros
/interface vlan
add name=vlan10 interface=bridge1 vlan-id=10
add name=vlan20 interface=bridge1 vlan-id=20
```

Assign gateway addresses:

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
```

The bridge VLAN table must permit the relevant VLANs to reach the bridge CPU.

### 🖆 Control Inter-VLAN Traffic

Use the forward firewall chain to control communication between VLAN networks.

Example:

```routeros
/ip firewall filter
add chain=forward action=accept connection-state=established,related
add chain=forward action=drop connection-state=invalid
add chain=forward action=drop src-address=192.168.30.0/24 dst-address=192.168.10.0/24 comment="Block guest access to management"
add chain=forward action=accept src-address=192.168.10.0/24 dst-address=192.168.20.0/24 comment="Allow management to staff"
```

Build a complete policy that explicitly defines permitted and denied traffic. The order of rules matters.

# 🗁 Spanning Tree Protocol

### 🖆 Why Spanning Tree Is Required

Redundant Layer two links can create switching loops.

A switching loop can cause:

* Broadcast storms.
* Duplicate frames.
* MAC address table instability.
* Network congestion.
* Loss of connectivity.

Spanning Tree Protocol prevents Layer two loops by logically blocking redundant paths while maintaining a loop-free forwarding topology.

### 🖆 STP Fundamentals

Study these concepts:

* Root bridge.
* Root port.
* Designated port.
* Alternate port.
* Bridge ID.
* Port ID.
* Path cost.
* BPDU.
* Topology change.

### 🖆 RSTP

Rapid Spanning Tree Protocol improves convergence compared with traditional STP.

RouterOS supports RSTP and MSTP bridge protocols, depending on the configuration and version.

Example:

```routeros
/interface bridge
add name=bridge1 protocol-mode=rstp
```

### 🖆 Root Bridge Selection

The bridge with the lowest Bridge ID becomes the root bridge.

Bridge ID is based on bridge priority and MAC address.

A deliberately selected root bridge helps create predictable network topology.

### 🖆 Edge Ports

Edge ports connect to endpoint devices rather than other switches.

Configure edge-port behavior appropriately:

```routeros
/interface bridge port
set [find interface=ether2] edge=yes
```

Do not mark a port as an edge port if it connects to another switch or could receive BPDUs from network infrastructure.

### 🖆 BPDU Protection

Understand BPDU Guard concepts and the protection mechanisms available for your RouterOS version and device.

The goal is to prevent an unexpected switching device from changing the intended spanning-tree topology.

# 🗁 Link Aggregation

### 🖆 Link Aggregation Fundamentals

Link aggregation combines multiple physical Ethernet links into a logical interface.

It can provide:

* Increased aggregate bandwidth.
* Link redundancy.
* Improved network resilience.
* Simplified logical interface management.

Actual throughput for an individual flow depends on the hashing method and traffic distribution.

### 🖆 IEEE 802.3ad and LACP

LACP stands for Link Aggregation Control Protocol.

It dynamically negotiates link aggregation between compatible devices.

Understand:

* Active mode.
* Passive mode.
* LACPDU.
* Member links.
* Aggregator.
* Load distribution.

### 🖆 Configure Bonding in RouterOS

Example:

```routeros
/interface bonding
add name=bond1 mode=802.3ad slaves=ether2,ether3
```

Connect the bonding interface to a bridge if required:

```routeros
/interface bridge port
add bridge=bridge1 interface=bond1
```

Both ends of the aggregated link must use compatible configuration.

### 🖆 Bonding Modes

Study common RouterOS bonding modes:

* `802.3ad`
* `active-backup`
* `balance-xor`
* `broadcast`
* `balance-rr`
* `balance-tlb`
* `balance-alb`

Understand their differences, compatibility requirements, and operational trade-offs.

# 🗁 Layer Two Security

### 🖆 MAC Address Security

Study MAC address security concepts:

* MAC address filtering.
* Static MAC entries.
* MAC address limits.
* Port security concepts.
* MAC address spoofing.
* MAC table flooding.

Understand the limitations of MAC-based security. MAC addresses can be spoofed and should not be treated as strong identity credentials.

### 🖆 DHCP Snooping

DHCP snooping can help prevent unauthorized DHCP servers from providing network configuration to clients.

Understand trusted and untrusted bridge ports.

Example:

```routeros
/interface bridge
set bridge1 dhcp-snooping=yes
```

Verify that the selected RouterOS version and device support the intended bridge DHCP snooping features.

### 🖆 ARP Inspection

ARP inspection concepts help protect networks from malicious or incorrect IP-to-MAC address mappings.

Study:

* Static ARP.
* Dynamic ARP.
* DHCP lease integration.
* Reply-only ARP.
* ARP validation.

### 🖆 Port Isolation

Port isolation restricts communication between selected ports while allowing communication with other permitted network ports.

Use it where endpoint-to-endpoint communication should be prevented, such as certain guest or customer access networks.

### 🖆 Broadcast Storm Control

Understand how broadcast and unknown-unicast traffic can affect network performance.

Study traffic limiting and storm-control capabilities supported by the specific MikroTik device and switch chip.

# 🗁 Switch Chip and Hardware Offloading

### 🖆 Switch Chip Fundamentals

A switch chip performs Ethernet switching operations in hardware.

It can forward frames between physical ports without requiring every frame to pass through the router CPU.

Hardware switching capabilities vary between MikroTik devices.

### 🖆 Hardware Offloading

RouterOS may indicate hardware offloading using the `H` flag in bridge port output.

Inspect bridge ports:

```routeros
/interface bridge port print
```

Understand which configurations preserve hardware offloading and which require CPU processing.

### 🖆 Switch Chip Configuration

Some MikroTik devices provide switch-chip-specific features and configuration menus.

Inspect available interfaces and hardware information:

```routeros
/interface ethernet print
/system resource print
```

Study the device-specific switch-chip documentation before applying hardware-specific settings.

### 🖆 Hardware and Software Switching Differences

| Feature | Hardware Switching | Software Switching |
| --- | --- | --- |
| Processing | Switch chip | Router CPU |
| Performance | Generally higher for supported traffic | Depends on CPU resources |
| CPU usage | Usually lower | Usually higher |
| Flexibility | Depends on hardware capabilities | Depends on RouterOS software support |
| Configuration | Device-dependent | RouterOS bridge features |

Do not assume that every bridge feature can be hardware-offloaded on every MikroTik device.

# 🗁 Switching Monitoring and Troubleshooting

### 🖆 Monitor Bridge Interfaces

Useful commands:

```routeros
/interface bridge print detail
/interface bridge port print detail
/interface bridge vlan print detail
/interface bridge host print
/interface ethernet print stats
```

### 🖆 Monitor MAC Learning

Check whether devices are being learned on the expected ports:

```routeros
/interface bridge host print detail
```

Look for:

* Missing MAC addresses.
* Unexpected port changes.
* Duplicate or frequently moving MAC addresses.
* Incorrect VLAN associations.

### 🖆 Troubleshoot VLAN Connectivity

If a client cannot communicate:

* Verify the physical link.
* Check bridge membership.
* Check the port PVID.
* Inspect tagged and untagged VLAN membership.
* Verify ingress filtering.
* Check frame-type restrictions.
* Confirm VLAN filtering is enabled.
* Verify the destination VLAN membership.
* Check the endpoint's IP configuration.

### 🖆 Troubleshoot Trunk Links

Verify:

* Both devices support the required VLANs.
* VLAN IDs match.
* Tagged traffic is permitted.
* Native or untagged VLAN behavior is consistent.
* The trunk is connected to the correct interfaces.
* The bridge VLAN table includes the intended ports.

### 🖆 Troubleshoot Spanning Tree

Inspect bridge and port details:

```routeros
/interface bridge monitor bridge1
/interface bridge port print detail
```

Look for:

* Unexpected root bridge selection.
* Ports in blocking or alternate states.
* Incorrect path costs.
* Unexpected topology changes.
* Redundant links without proper loop prevention.

### 🖆 Troubleshoot Hardware Offloading

If hardware offloading is not active:

* Verify device switch-chip capabilities.
* Check bridge port configuration.
* Review VLAN filtering compatibility.
* Inspect bonding and bridge combinations.
* Check whether configured features require CPU processing.
* Consult the device-specific MikroTik documentation.

# 🗁 Practical Switching Labs

### 🖆 Basic Bridge Lab

**Objective:** Connect multiple devices through a RouterOS bridge.

Tasks:

* Create a bridge.
* Add Ethernet interfaces.
* Assign an IP address to the bridge.
* Connect two client devices.
* Test communication.
* Inspect the MAC address table.

### 🖆 VLAN Segmentation Lab

**Objective:** Create separate VLANs for different departments.

Tasks:

* Create VLAN 10 and VLAN 20.
* Configure access ports.
* Configure a trunk port.
* Add bridge VLAN table entries.
* Enable VLAN filtering.
* Test communication within each VLAN.
* Verify that separate VLANs remain isolated at Layer two.

### 🖆 Inter-VLAN Routing Lab

**Objective:** Enable controlled communication between VLANs.

Tasks:

* Create VLAN interfaces.
* Assign gateway addresses.
* Configure client IP addresses.
* Verify routing between VLANs.
* Create firewall policies.
* Block unauthorized communication.
* Test permitted and denied traffic.

### 🖆 Spanning Tree Lab

**Objective:** Prevent switching loops in a redundant topology.

Tasks:

* Connect multiple switches with redundant links.
* Enable RSTP.
* Observe root bridge selection.
* Inspect port roles and states.
* Disconnect an active link.
* Observe topology reconvergence.
* Restore the link and review the resulting topology.

### 🖆 Link Aggregation Lab

**Objective:** Configure a redundant aggregated link.

Tasks:

* Connect two MikroTik devices using two Ethernet links.
* Configure 802.3ad bonding.
* Verify LACP operation.
* Test connectivity.
* Disconnect one member link.
* Observe failover.
* Review traffic distribution.

### 🖆 Complete Switching Project

**Objective:** Build a segmented small-office switching environment.

Requirements:

* Management VLAN.
* Staff VLAN.
* Guest VLAN.
* Server VLAN.
* Access and trunk ports.
* Inter-VLAN routing.
* Firewall-based isolation.
* RSTP loop prevention.
* Link aggregation where supported.
* Centralized monitoring.
* Documented VLAN and port assignments.

# 🗁 Common Switching Mistakes

Avoid these mistakes:

* Assigning an incorrect PVID.
* Forgetting to add VLAN membership entries.
* Enabling VLAN filtering before preparing the VLAN table.
* Forgetting bridge CPU membership for routed VLANs.
* Configuring mismatched trunk VLANs.
* Allowing unnecessary VLANs on trunk links.
* Creating Layer two loops without STP.
* Misconfiguring LACP on one side.
* Assuming hardware offloading is always available.
* Using VLANs without appropriate Layer three access controls.
* Treating MAC address filtering as strong authentication.
* Ignoring unexpected MAC address movement.
* Failing to document switch port assignments.

# 🗁 Study and Preparation Strategy

### 🖆 Build Your Switching Foundation

Understand these subjects first:

* Ethernet fundamentals.
* OSI and TCP/IP models.
* MAC addressing.
* Broadcast domains.
* Switching and bridging.
* IPv4 and IPv6 basics.
* VLAN tagging.
* Basic routing.

### 🖆 Practice RouterOS Switching

Build practical experience with:

* Bridge configuration.
* Bridge port management.
* MAC address learning.
* VLAN filtering.
* Access and trunk ports.
* Inter-VLAN routing.
* RSTP.
* Link aggregation.
* DHCP snooping.
* Hardware offloading.

### 🖆 Build a Switching Lab

Recommended environment:

* Two or more MikroTik CHR routers for software-based switching practice.
* Physical MikroTik switches for hardware-offloading experiments.
* Multiple client devices or virtual machines.
* VLAN-aware network interfaces.
* A Linux machine for testing and monitoring.

CHR is useful for learning RouterOS configuration, but physical hardware is needed to evaluate actual switch-chip capabilities and hardware offloading.

### 🖆 Validate Your Knowledge

For every lab, answer:

* How does the switch learn a MAC address?
* Why does a switch flood unknown unicast traffic?
* How does a VLAN separate broadcast domains?
* What is the difference between an access port and a trunk port?
* Why is the bridge interface included as a tagged VLAN member?
* How does RSTP prevent Layer two loops?
* How does LACP provide link aggregation?
* Which features are hardware-offloaded on the selected device?
* How can switching problems be identified and resolved?

### 🖆 Official Certification Preparation

Review the current MikroTik training catalog and official certification information to verify the availability, scope, prerequisites, and examination requirements of the switching certification track.

# 🗁 Additional Resources

* MikroTik Official Website: <https://mikrotik.com/>
* MikroTik Training: <https://mikrotik.com/training>
* MikroTik RouterOS Documentation: <https://help.mikrotik.com/docs/>
* MikroTik Bridge Documentation: <https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching>
* MikroTik VLAN Documentation: <https://help.mikrotik.com/docs/>
* IEEE 802.1Q Standard: <https://standards.ieee.org/>
* IEEE 802.1D and 802.1w Switching Concepts: <https://standards.ieee.org/>
* IEEE 802.3ad / 802.1AX Link Aggregation: <https://standards.ieee.org/>

Always verify RouterOS syntax and feature support against the documentation for your specific MikroTik device and RouterOS version.
