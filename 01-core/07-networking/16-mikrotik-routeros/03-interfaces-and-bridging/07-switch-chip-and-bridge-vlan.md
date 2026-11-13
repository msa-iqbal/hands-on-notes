# Switch Chip and Bridge VLAN in MikroTik RouterOS

MikroTik devices use different hardware architectures to forward Ethernet traffic. Depending on the device, traffic may be processed by the CPU, a dedicated switch chip, or a combination of both.

Understanding the relationship between the switch chip and RouterOS bridge is important when designing VLAN-aware networks.

A properly configured switching architecture can provide:

* Hardware-accelerated Layer 2 forwarding.
* VLAN isolation.
* Efficient traffic processing.
* Reduced CPU utilization.
* Better network performance.
* Predictable traffic forwarding between ports.

RouterOS v7 supports VLAN-aware bridging, but hardware capabilities and configuration requirements vary between MikroTik models.

# 🗁 What Is a Switch Chip?

A switch chip is a dedicated hardware component designed to forward Ethernet frames between physical Ethernet ports.

Instead of sending every frame through the main CPU, the switch chip can process eligible Layer 2 traffic directly in hardware.

This process is commonly called hardware switching or hardware offloading.

**Without hardware switching**

```text
             Ethernet Ports
                   |
                   v
              Main CPU
                   |
             Software Bridge
                   |
             Forwarding Logic
```

The CPU processes bridge traffic.

**With hardware switching**

```text
             Ethernet Ports
                   |
                   v
               Switch Chip
                   |
          Hardware Forwarding
                   |
          +--------+--------+
          |                 |
       Port 2            Port 3

                   |
                   v
                Main CPU
```

The switch chip handles supported forwarding operations while the CPU manages routing, configuration, and other system services.

# 🗁 Switch Chip Responsibilities

Depending on hardware capabilities, a switch chip may support:

* Layer 2 switching.
* MAC address learning.
* MAC address table management.
* VLAN filtering.
* VLAN tagging and untagging.
* Port isolation.
* Broadcast and multicast handling.
* Hardware forwarding.
* Traffic statistics.
* Port mirroring.
* Access Control Lists (ACLs).

Not every MikroTik switch chip supports all these features.

# 🗁 CPU-Based Switching vs Hardware Switching

| Feature | CPU-based switching | Hardware switching |
| --- | --- | --- |
| Packet processing | Main CPU | Dedicated switch chip |
| CPU utilization | Generally higher | Generally lower |
| Forwarding performance | Depends on CPU | Depends on switch chip |
| VLAN support | Software bridge | Hardware-dependent |
| Configuration | RouterOS bridge | RouterOS bridge and hardware-specific capabilities |
| Scalability | Limited by CPU resources | Generally better for supported switching workloads |

Hardware offloading is particularly useful when multiple devices exchange large amounts of Layer 2 traffic.

# 🗁 MikroTik Switching Architectures

MikroTik devices do not all use the same switching architecture.

Common architectural differences include:

**Single switch-chip architecture**

Multiple Ethernet ports connect to one switch chip.

The switch chip may handle forwarding between these ports without involving the CPU.

**Multiple switch-chip architecture**

Some devices contain multiple switch chips.

Ports may be divided between different switching groups, and traffic between groups may need to pass through the CPU or another internal connection.

**CPU-centric architecture**

Some devices do not provide a dedicated switch chip capable of hardware-offloading the desired bridge configuration.

In these devices, RouterOS processes bridge traffic through software.

**Integrated switching architecture**

Newer devices may use integrated switching hardware with capabilities that differ from traditional switch-chip designs.

Always check the specific device's hardware documentation before choosing a switching design.

# 🗁 RouterOS Bridge and Switch Chip Relationship

RouterOS provides a unified bridge configuration interface for Layer 2 switching.

A bridge can contain multiple physical Ethernet interfaces:

```routeros
/interface bridge
add name=bridge-LAN protocol-mode=rstp

/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

RouterOS determines whether eligible traffic can be offloaded to the underlying hardware.

The bridge remains responsible for the logical switching configuration, while the hardware performs supported forwarding operations.

The exact division of responsibilities depends on the device architecture and enabled features.

# 🗁 Hardware Offloading

Hardware offloading means that supported bridge forwarding operations are performed by dedicated switching hardware rather than the main CPU.

In RouterOS, bridge ports may display an `H` flag when hardware offloading is active.

Check the bridge ports:

```routeros
/interface bridge port print
```

Example output:

```text
Flags: I - INACTIVE; H - HW-OFFLOAD
Columns: INTERFACE, BRIDGE, HW, PVID
#   INTERFACE  BRIDGE      HW   PVID
0 H ether2     bridge-LAN  yes     1
1 H ether3     bridge-LAN  yes     1
2 H ether4     bridge-LAN  yes     1
```

The `H` flag indicates that the port is hardware-offloaded.

**Important:** Hardware offloading is not guaranteed simply because the device has a switch chip. The bridge configuration and hardware capabilities must support the requested features.

# 🗁 HW Property

The `hw` property controls whether RouterOS should attempt to use hardware offloading for a bridge port when supported.

Example:

```routeros
/interface bridge port
set [find interface=ether2] hw=yes
```

Disable hardware offloading for a port:

```routeros
/interface bridge port
set [find interface=ether2] hw=no
```

When `hw=no`, eligible bridge forwarding through that port is handled by the CPU.

Use this setting only when required for a specific design or troubleshooting task.

# 🗁 Bridge VLAN Filtering and Hardware Offloading

Bridge VLAN filtering allows a bridge to enforce VLAN membership and tagging rules.

Example:

```routeros
/interface bridge
add name=bridge-LAN vlan-filtering=yes
```

The bridge VLAN table defines VLAN membership:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=ether1,bridge-LAN untagged=ether2

add bridge=bridge-LAN vlan-ids=20 tagged=ether1,bridge-LAN untagged=ether3
```

This configuration describes:

| Interface | VLAN 10 | VLAN 20 |
| --- | --- | --- |
| ether1 | Tagged | Tagged |
| ether2 | Untagged | Not a member |
| ether3 | Not a member | Untagged |
| bridge-LAN | Tagged | Tagged |

The bridge interface is included as a tagged member because the router CPU needs access to both VLANs.

On supported devices, compatible bridge VLAN filtering configurations can be hardware-offloaded.

However, the available hardware capabilities differ between switch chips and device models.

# 🗁 Switch Chip VLAN Filtering vs Bridge VLAN Filtering

These approaches are related but should not be treated as identical.

**Bridge VLAN filtering**

RouterOS uses the bridge VLAN table to define VLAN membership and forwarding behavior.

Typical configuration paths:

```routeros
/interface bridge
/interface bridge port
/interface bridge vlan
```

Advantages:

* Consistent bridge-based configuration.
* Centralized VLAN membership management.
* Integration with RouterOS Layer 3 VLAN interfaces.
* Hardware offloading on supported devices.

**Switch-chip-specific VLAN configuration**

Some older MikroTik devices and RouterOS configurations use switch-chip-specific menus and properties.

Examples include:

```routeros
/interface ethernet switch
/interface ethernet switch port
/interface ethernet switch vlan
```

These menus are hardware- and RouterOS-version-dependent.

Advantages may include:

* Direct access to switch-chip capabilities.
* Hardware-specific VLAN configuration.
* Features not exposed through a particular bridge implementation.

For modern RouterOS v7 deployments, bridge VLAN filtering is generally the preferred configuration approach when supported by the device.

Do not mix legacy switch-chip VLAN configuration and bridge VLAN filtering without understanding the device's architecture and the relevant documentation.

# 🗁 VLAN-Aware Switching Example

Consider the following topology:

```text
                  MikroTik Switch
                 +---------------+
                 |               |
       ether1 ---| Trunk          |
                 |               |
       ether2 ---| Access VLAN 10 |
                 |               |
       ether3 ---| Access VLAN 20 |
                 |               |
       ether4 ---| Access VLAN 10 |
                 |               |
                 +---------------+
```

Configuration:

* `ether1`: Trunk carrying VLAN 10 and VLAN 20.
* `ether2`: Access port for VLAN 10.
* `ether3`: Access port for VLAN 20.
* `ether4`: Access port for VLAN 10.

Create the bridge:

```routeros
/interface bridge
add name=bridge-LAN protocol-mode=rstp vlan-filtering=no
```

Add ports:

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether1
add bridge=bridge-LAN interface=ether2 pvid=10
add bridge=bridge-LAN interface=ether3 pvid=20
add bridge=bridge-LAN interface=ether4 pvid=10
```

Configure access ports:

```routeros
/interface bridge port
set [find interface=ether2] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged

set [find interface=ether3] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged

set [find interface=ether4] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged
```

Configure the trunk:

```routeros
/interface bridge port
set [find interface=ether1] ingress-filtering=yes frame-types=admit-only-vlan-tagged
```

Configure VLAN membership:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=ether1 untagged=ether2,ether4

add bridge=bridge-LAN vlan-ids=20 tagged=ether1 untagged=ether3
```

This example configures a Layer 2 switch. The bridge interface is not included as a tagged member because the router CPU is not intended to participate in these VLANs.

If the MikroTik device must provide Layer 3 services for these VLANs, include the bridge interface as a tagged member and create the corresponding VLAN interfaces.

Enable VLAN filtering:

```routeros
/interface bridge
set bridge-LAN vlan-filtering=yes
```

Check hardware offloading:

```routeros
/interface bridge port print
```

# 🗁 CPU Port and VLAN Membership

The CPU port is an important part of MikroTik VLAN architecture.

In RouterOS bridge VLAN filtering, the bridge interface represents the CPU-facing port.

There are two common designs.

**Layer 2 switching only**

The device forwards VLAN traffic between physical ports without providing Layer 3 services for those VLANs.

The bridge interface does not need to be a member of those VLANs.

Example:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=ether1 untagged=ether2
```

**Layer 2 switching with Layer 3 services**

The router provides IP addresses, DHCP, routing, or firewall services for the VLANs.

The bridge interface must be included as a tagged member of the corresponding VLANs.

Example:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=bridge-LAN,ether1 untagged=ether2
```

Create the VLAN interface:

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10
```

Assign an IP address:

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10
```

This distinction is essential when troubleshooting VLAN connectivity.

# 🗁 Switch Chip Hardware Capabilities

Before deploying VLAN-aware switching, identify the hardware capabilities of the target device.

Consider:

* Number of switch chips.
* Ethernet port grouping.
* VLAN filtering support.
* Hardware forwarding capabilities.
* Bridge VLAN filtering compatibility.
* Layer 3 hardware offloading support, if relevant.
* Port mirroring and ACL capabilities.
* RouterOS version requirements.

A feature supported by one MikroTik model may not be available on another.

Consult the official device specifications and RouterOS documentation.

# 🗁 Troubleshooting Hardware Offloading

**Hardware offloading is not active**

Possible causes:

* Unsupported switch chip.
* Unsupported bridge configuration.
* Hardware offloading disabled on a bridge port.
* A feature combination that prevents offloading.
* Device-specific hardware limitations.

Check:

```routeros
/interface bridge port print detail
/interface bridge print detail
```

**CPU utilization is unexpectedly high**

Possible causes:

* Traffic is being forwarded through the CPU.
* Hardware offloading is unavailable or inactive.
* The device is performing additional Layer 3 or firewall processing.
* Broadcast or multicast traffic is excessive.
* The CPU is handling traffic that exceeds its processing capacity.

Check:

```routeros
/system resource monitor
/tool profile
/interface bridge port print
```

**VLAN traffic does not pass between ports**

Possible causes:

* Incorrect VLAN membership.
* Incorrect PVID.
* Missing tagged or untagged membership.
* Ingress filtering rejecting traffic.
* Incorrect frame-type configuration.
* Hardware-specific configuration limitations.

Check:

```routeros
/interface bridge vlan print detail
/interface bridge port print detail
```

**VLAN works in software but not as expected with hardware offloading**

Possible causes:

* Hardware does not support the requested VLAN feature.
* A configuration feature is incompatible with hardware offloading.
* Device-specific switch-chip behavior differs from the expected bridge behavior.

Check the device-specific MikroTik documentation and verify the actual bridge port flags.

# 🗁 Performance Monitoring

Monitor CPU utilization:

```routeros
/system resource monitor
```

Inspect CPU-intensive processes:

```routeros
/tool profile
```

Inspect Ethernet interfaces:

```routeros
/interface ethernet print stats
```

Inspect bridge ports:

```routeros
/interface bridge port print detail
```

Use these commands to determine whether traffic is being processed by hardware or software and to identify possible performance bottlenecks.

# 🗁 Best Practices

* Understand the device's switching architecture before designing VLANs.
* Prefer bridge VLAN filtering for modern RouterOS v7 deployments when supported.
* Verify hardware offloading instead of assuming it is active.
* Use the correct PVID on access ports.
* Restrict trunk ports to the VLANs they actually need.
* Include the bridge interface in tagged VLAN membership when the CPU needs access.
* Avoid mixing legacy switch-chip configuration with bridge VLAN filtering without a documented reason.
* Monitor CPU utilization and interface statistics.
* Test VLAN behavior on the actual hardware model.
* Consult the device-specific hardware documentation.
* Maintain backups before changing switching architecture or VLAN configuration.

# 🗁 Official Documentation

* MikroTik Bridge and Switching: <https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching>
* MikroTik Bridge VLAN Table: <https://help.mikrotik.com/docs/spaces/ROS/pages/28606465/Bridge+VLAN+Table>
* MikroTik Switch Chip Features: <https://help.mikrotik.com/docs/>
