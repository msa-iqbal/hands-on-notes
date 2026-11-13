# MikroTik Hardware Offloading

Hardware offloading is a networking capability that allows supported MikroTik hardware to process certain networking operations directly through dedicated hardware instead of relying entirely on the router's CPU.

In Layer 2 switching, hardware offloading allows a supported switch chip to forward Ethernet frames between ports without sending every frame through the CPU.

This improves packet-forwarding performance and reduces CPU utilization.

Hardware offloading is particularly important when building:

* High-performance LAN networks.
* Multi-port switching environments.
* VLAN-aware bridges.
* ISP aggregation networks.
* Enterprise switching infrastructure.
* Networks requiring high throughput and low CPU overhead.

RouterOS supports hardware offloading on compatible MikroTik devices, but available features depend on the hardware architecture and RouterOS version.

# 🗁 Understanding Hardware Offloading

Without hardware offloading, bridge traffic may need to be processed by the router's CPU.

With hardware offloading, supported traffic can be forwarded directly by the switch chip.

**Software forwarding:**

```text
       Ethernet Port
             |
             v
        Router CPU
             |
             v
       Ethernet Port
```

**Hardware-offloaded forwarding:**

```text
       Ethernet Port
             |
             v
         Switch Chip
             |
             v
       Ethernet Port
```

The switch chip handles eligible Layer 2 forwarding operations independently of the CPU.

The CPU remains responsible for operations that require software processing, including management traffic and many Layer 3 services.

# 🗁 Understanding Switch Chips

A switch chip is dedicated hardware designed to perform Ethernet switching operations.

It can provide functions such as:

* MAC address learning.
* Ethernet frame forwarding.
* VLAN filtering.
* Port isolation.
* Hardware-based packet filtering.
* Traffic prioritization.
* Other switching features supported by the chip.

MikroTik devices use different switch chip architectures depending on their model.

Some routers contain multiple switch chips, while others use a CPU-based bridge without a dedicated switch chip capable of offloading the required operations.

Hardware capabilities must be checked against the specific device model.

# 🗁 Understanding CPU-Based and Hardware-Based Switching

| Feature | CPU-Based Switching | Hardware-Based Switching |
| --- | --- | --- |
| Frame forwarding | CPU processes traffic | Switch chip processes eligible traffic |
| CPU utilization | Generally higher | Generally lower |
| Forwarding performance | Depends on CPU and software path | Depends on switch chip capabilities |
| Hardware dependency | Uses available CPU resources | Requires compatible hardware |
| Advanced features | Depends on RouterOS software support | Limited to switch chip capabilities and supported configuration |

Hardware offloading does not mean that all router traffic bypasses the CPU.

Traffic directed to the router itself, routed traffic, and unsupported bridge operations may still require CPU processing.

# 🗁 Checking Hardware Offloading Support

Before configuring hardware offloading, identify the router model and its hardware capabilities.

Display system resources:

```routeros
/system resource print
```

Display system routerboard information:

```routeros
/system routerboard print
```

Display available Ethernet interfaces:

```routeros
/interface ethernet print
```

Display bridge ports:

```routeros
/interface bridge port print
```

The hardware-offloading flag is commonly displayed as:

| Flag | Meaning |
|---|---|
| `H` | Hardware offloading is active |

Example:

```text
Flags: H - HARDWARE-OFFLOADED
 #   INTERFACE  BRIDGE       HW
 0 H ether2     bridge-LAN   yes
 1 H ether3     bridge-LAN   yes
 2 H ether4     bridge-LAN   yes
```

The exact output depends on the RouterOS version and device.

If the `H` flag is absent, investigate hardware support, bridge configuration, and interface compatibility.

# 🗁 Understanding Hardware Offloading in RouterOS v7

RouterOS v7 provides bridge hardware offloading on supported devices.

The bridge configuration is managed through:

```routeros
/interface bridge
```

Bridge port configuration is managed through:

```routeros
/interface bridge port
```

Create a bridge:

```routeros
/interface bridge add name=bridge-LAN protocol-mode=rstp
```

Add Ethernet interfaces:

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

Inspect bridge ports:

```routeros
/interface bridge port print
```

On supported hardware and compatible configurations, RouterOS may automatically enable hardware offloading for eligible ports.

The `hw` property controls hardware offloading where supported.

Inspect the property:

```routeros
/interface bridge port print detail
```

# 🗁 Understanding the HW Property

The `hw` property indicates whether hardware offloading is permitted for a bridge port on supported hardware.

Enable hardware offloading for a port:

```routeros
/interface bridge port set [find interface=ether2] hw=yes
```

Enable it for another port:

```routeros
/interface bridge port set [find interface=ether3] hw=yes
```

Disable hardware offloading for a port:

```routeros
/interface bridge port set [find interface=ether2] hw=no
```

Verify the configuration:

```routeros
/interface bridge port print detail
```

**Important:** Setting `hw=yes` does not guarantee that hardware offloading will become active. The device, switch chip, interface arrangement, and configured bridge features must support the operation.

The `H` flag is the important indication that hardware offloading is actually active.

# 🗁 Understanding Switch Chip Architecture

MikroTik devices use different hardware architectures.

The capabilities of a device depend on:

* Switch chip model.
* Ethernet port arrangement.
* CPU connection to the switch chip.
* Hardware VLAN support.
* Hardware filtering capabilities.
* RouterOS driver support.

Some devices support extensive hardware switching features, while others provide only basic switching capabilities.

A device may also have multiple groups of Ethernet ports connected to different switch chips or internal hardware paths.

Traffic crossing between different hardware switching domains may require CPU processing.

Always consult the hardware documentation for the exact model.

# 🗁 Understanding Hardware VLAN Filtering

Hardware VLAN filtering allows compatible switch chips to process VLAN-aware forwarding operations directly in hardware.

A bridge can be configured with VLAN filtering:

```routeros
/interface bridge set bridge-LAN vlan-filtering=yes
```

Inspect the bridge:

```routeros
/interface bridge print detail
```

Inspect bridge VLAN configuration:

```routeros
/interface bridge vlan print detail
```

Inspect bridge ports:

```routeros
/interface bridge port print
```

On compatible devices, VLAN filtering can be hardware-offloaded.

However, not every switch chip supports every VLAN configuration or bridge feature in hardware.

Hardware support may also depend on the exact arrangement of access ports, trunk ports, CPU-facing ports, and bridge VLAN entries.

VLAN configuration is covered in more detail in the dedicated VLAN documentation.

# 🗁 Understanding Hardware Offloading Limitations

Hardware offloading has limitations because switch chips support a specific set of networking operations.

Potential limitations include:

* Unsupported bridge features.
* Unsupported VLAN configurations.
* CPU-dependent packet processing.
* Unsupported port combinations.
* Hardware-specific filtering restrictions.
* Different capabilities between switch chip models.

Some bridge features can disable hardware offloading for particular ports or traffic paths.

For example, certain bridge firewall processing requirements may require packets to be handled by the CPU.

Do not assume that enabling a feature will preserve hardware forwarding.

Always verify the resulting configuration and traffic path.

# 🗁 Understanding CPU and Switch Chip Traffic

Not all traffic follows the same forwarding path.

Consider a router with three Ethernet ports in one bridge.

**Traffic between two hardware-offloaded ports**

```text
PC-01
  |
ether2
  |
Switch Chip
  |
ether3
  |
PC-02
```

Eligible traffic can be forwarded directly through the switch chip.

**Traffic addressed to the router**

```text
PC-01
  |
ether2
  |
Switch Chip
  |
CPU
  |
Router Services
```

Traffic intended for the router's own IP address must be delivered to the CPU.

**Routed traffic**

```text
LAN Device
    |
 LAN Port
    |
 Router CPU
    |
 Routing Table
    |
 WAN Port
    |
 Internet
```

Ordinary Layer 3 routing generally requires CPU processing unless the device and configuration support a separate hardware-routing acceleration feature.

# 🗁 Monitoring Hardware Offloading

Display bridge ports:

```routeros
/interface bridge port print
```

Display detailed bridge port information:

```routeros
/interface bridge port print detail
```

Monitor CPU utilization:

```routeros
/system resource monitor
```

Inspect Ethernet traffic:

```routeros
/interface monitor-traffic ether2
```

Compare CPU utilization and interface traffic while generating traffic between two connected devices.

When hardware offloading is active, eligible Layer 2 traffic may pass through the switch chip without producing CPU utilization proportional to the forwarded traffic.

# 🗁 Understanding Hardware Offloading and Bridge Firewall

Bridge firewall rules provide Layer 2 filtering capabilities.

However, software-based bridge firewall processing can require traffic to pass through the CPU.

Display bridge filter rules:

```routeros
/interface bridge filter print
```

Display bridge NAT rules:

```routeros
/interface bridge nat print
```

Display bridge settings:

```routeros
/interface bridge settings print
```

If hardware offloading is important, review the impact of bridge firewall features before enabling them.

Use IP firewall rules for ordinary Layer 3 security policies when appropriate.

The correct design depends on whether filtering must occur at Layer 2 or Layer 3.

# 🗁 Practical Lab: Hardware Offloading Verification

This lab demonstrates how to inspect and verify hardware offloading.

**Lab requirements:**

* MikroTik router with documented switch-chip hardware offloading support.
* RouterOS v7.
* Two or more Ethernet ports supported by the same hardware switching domain.
* Two client devices.
* WinBox or SSH access.

MikroTik CHR can be used to practice bridge configuration, but it does not reproduce physical switch-chip hardware offloading.

**Inspect hardware information**

```routeros
/system resource print
/system routerboard print
```

**Create a bridge**

```routeros
/interface bridge add name=bridge-LAN protocol-mode=rstp
```

**Add Ethernet ports**

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2 hw=yes
add bridge=bridge-LAN interface=ether3 hw=yes
```

**Verify bridge port status**

```routeros
/interface bridge port print
```

Look for the `H` hardware-offloading flag.

**Inspect detailed properties**

```routeros
/interface bridge port print detail
```

**Monitor CPU utilization**

```routeros
/system resource monitor
```

**Generate traffic**

Connect two computers to the selected Ethernet ports.

Configure addresses in the same subnet:

```text
PC-01:
IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0

PC-02:
IP Address: 192.168.10.11
Subnet Mask: 255.255.255.0
```

Test communication:

```bash
ping 192.168.10.11
```

Generate additional traffic between the computers and monitor the router's CPU utilization.

**Verify the final configuration**

```routeros
/interface bridge print detail
/interface bridge port print detail
/system resource monitor
```

**Lab objectives:**

* Identify the router's hardware capabilities.
* Understand the switch chip's role.
* Configure bridge ports.
* Inspect the `hw` property.
* Identify active hardware offloading through the `H` flag.
* Understand the difference between CPU forwarding and hardware forwarding.
* Observe CPU utilization during Layer 2 traffic.

# 🗁 Hardware Offloading Troubleshooting

**The H flag is missing**

Check:

```routeros
/interface bridge port print detail
/system resource print
/system routerboard print
```

Verify that the router supports hardware offloading and that the selected ports belong to a compatible switching domain.

**Hardware offloading stops after configuration changes**

Review:

```routeros
/interface bridge print detail
/interface bridge port print detail
/interface bridge vlan print detail
/interface bridge filter print
```

Identify bridge features or filtering requirements that may prevent hardware forwarding.

**High CPU utilization during Layer 2 traffic**

Check:

```routeros
/system resource monitor
/interface bridge port print
```

Investigate whether traffic is being forwarded by the CPU instead of the switch chip.

Also review firewall processing, VLAN configuration, and bridge port compatibility.

**VLAN traffic is not hardware-offloaded**

Check:

```routeros
/interface bridge print detail
/interface bridge port print detail
/interface bridge vlan print detail
```

Compare the configuration with the switch chip's documented VLAN capabilities.

**Only some bridge ports show hardware offloading**

Check the physical port arrangement and switch chip architecture.

Ports connected to different hardware switching domains may not share the same hardware forwarding path.

# 🗁 Hardware Offloading Best Practices

* Verify switch chip capabilities before designing the network.
* Use compatible bridge and port configurations.
* Check the `H` flag after configuration changes.
* Understand which traffic requires CPU processing.
* Review bridge firewall features before enabling them.
* Verify VLAN hardware support on the exact device.
* Monitor CPU utilization and interface statistics.
* Avoid assuming that every Ethernet port belongs to the same switching domain.
* Consult model-specific MikroTik documentation.
* Test forwarding performance under realistic network conditions.

# 🗁 Official Documentation

* [MikroTik Bridging and Switching](https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [MikroTik Product Documentation](https://mikrotik.com/products)
