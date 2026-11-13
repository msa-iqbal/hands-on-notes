# MikroTik Bridge Configuration

A bridge in MikroTik RouterOS is a logical Layer 2 interface that connects multiple network interfaces into a single broadcast domain.

It allows devices connected to different physical ports to communicate as if they were connected to the same Ethernet switch.

For example, a MikroTik router can use a bridge to connect several Ethernet ports and provide a common LAN network.

A bridge is commonly used for:

* Connecting multiple Ethernet ports.
* Combining Ethernet and wireless interfaces.
* Creating a local Layer 2 network.
* Carrying VLAN traffic.
* Connecting virtual interfaces.
* Building LAN networks for homes, offices, and ISP environments.

RouterOS implements software bridging and, on supported hardware, hardware-offloaded bridging.

# 🗁 Understanding Layer 2 Bridging

Ethernet bridging operates at Layer 2 of the OSI model.

A bridge forwards Ethernet frames based on destination MAC addresses.

When a frame arrives at a bridge port, the bridge examines its source MAC address and learns which port is associated with that address.

It then uses its forwarding database to decide where to send the frame.

Example network:

```text
                 MikroTik Router
                 ┌──────────────┐
                 │              │
                 │  bridge-LAN  │
                 │              │
                 └──────┬───────┘
                        │
             ┌──────────┼──────────┐
             │          │          │
          ether2      ether3     ether4
             │          │          │
          Computer    Switch     Access Point
```

All connected devices can communicate within the same Layer 2 network, subject to VLAN configuration and firewall or other filtering policies.

# 🗁 Understanding a Bridge Interface

A bridge is itself a logical interface in RouterOS.

Create a bridge:

```routeros
/interface bridge add name=bridge-LAN
```

Display bridge interfaces:

```routeros
/interface bridge print
```

Display detailed bridge information:

```routeros
/interface bridge print detail
```

A bridge can have its own:

* Interface name.
* MAC address.
* MTU.
* Protocol mode.
* VLAN filtering configuration.
* Bridge-specific settings.

The bridge interface can also be used for Layer 3 configuration, such as assigning an IP address to the LAN.

# 🗁 Creating a Bridge

Create a bridge named `bridge-LAN`:

```routeros
/interface bridge add name=bridge-LAN
```

Verify the bridge:

```routeros
/interface bridge print
```

Add a comment:

```routeros
/interface bridge set bridge-LAN comment="Main LAN Bridge"
```

Inspect the configuration:

```routeros
/interface bridge print detail
```

A bridge can be created before adding any physical ports.

# 🗁 Adding Interfaces to a Bridge

Physical or logical interfaces become bridge ports when added to a bridge.

Add `ether2` to `bridge-LAN`:

```routeros
/interface bridge port add bridge=bridge-LAN interface=ether2
```

Add additional interfaces:

```routeros
/interface bridge port add bridge=bridge-LAN interface=ether3
/interface bridge port add bridge=bridge-LAN interface=ether4
```

Display bridge ports:

```routeros
/interface bridge port print
```

Display detailed bridge port information:

```routeros
/interface bridge port print detail
```

Example topology:

```text
                 bridge-LAN
                     │
          ┌──────────┼──────────┐
          │          │          │
        ether2     ether3     ether4
          │          │          │
        PC-01      Switch      AP-01
```

All three ports belong to the same bridge and can participate in the same Layer 2 broadcast domain when no VLAN separation is configured.

# 🗁 Assigning an IP Address to a Bridge

In a typical LAN design, the LAN IP address is assigned to the bridge interface rather than an individual physical bridge port.

Example:

```routeros
/ip address add address=192.168.10.1/24 interface=bridge-LAN
```

Verify the IP address:

```routeros
/ip address print
```

The bridge interface now represents the Layer 3 gateway interface for the LAN.

Devices connected through its member ports can use `192.168.10.1` as their default gateway when the rest of the network configuration is in place.

**Important:** If physical interfaces are already members of a bridge, avoid assigning the same LAN subnet directly to those individual ports. Configure the IP address on the bridge instead.

# 🗁 Understanding Bridge Protocol Mode

RouterOS supports different bridge protocol modes.

The most common modes are:

| Mode | Description |
| --- | --- |
| `none` | Disables bridge loop-prevention protocol processing |
| `stp` | Enables Spanning Tree Protocol |
| `rstp` | Enables Rapid Spanning Tree Protocol |
| `mstp` | Enables Multiple Spanning Tree Protocol |

Display bridge configuration:

```routeros
/interface bridge print detail
```

Configure RSTP:

```routeros
/interface bridge set bridge-LAN protocol-mode=rstp
```

Verify:

```routeros
/interface bridge print detail
```

RSTP helps prevent Layer 2 switching loops and provides faster convergence than traditional STP.

For most ordinary bridge deployments, RSTP is a suitable starting point.

Disabling spanning-tree protection in a network with redundant Layer 2 paths can result in broadcast storms and network instability.

# 🗁 Understanding Bridge MAC Address

A bridge has a MAC address used for its Layer 2 identity.

RouterOS can select a bridge MAC address automatically or use a configured address.

Inspect the bridge:

```routeros
/interface bridge print detail
```

The bridge MAC address can be relevant to:

* LAN gateway identity.
* Bridge protocol operation.
* DHCP and ARP behavior.
* Network management.
* Layer 2 forwarding.

Avoid changing the bridge MAC address without understanding its impact on connected devices and network services.

# 🗁 Understanding Bridge MTU

The bridge interface has MTU-related properties that influence the maximum packet size it can handle.

Inspect bridge properties:

```routeros
/interface bridge print detail
```

The effective MTU and L2MTU depend on the bridge and its member interfaces.

When working with VLANs, tunnels, and other encapsulation technologies, ensure that the underlying interfaces support the required frame sizes.

# 🗁 Understanding Bridge Hardware Offloading

Hardware offloading allows supported switching hardware to forward eligible Layer 2 traffic without requiring the router's CPU to process every frame.

This can improve:

* Forwarding performance.
* CPU efficiency.
* Throughput.
* Scalability.

Hardware offloading support depends on the MikroTik model, switch chip, interface arrangement, and bridge configuration.

Display bridge ports:

```routeros
/interface bridge port print
```

A common flag is:

| Flag | Meaning |
|---|---|
| `H` | Hardware offloading is active |

Example output:

```text
Flags: H - HARDWARE-OFFLOADED
 #   INTERFACE  BRIDGE       HW
 0 H ether2     bridge-LAN   yes
 1 H ether3     bridge-LAN   yes
 2 H ether4     bridge-LAN   yes
```

The exact output format varies by RouterOS version.

Some bridge features or combinations may prevent hardware offloading on certain devices.

For performance-sensitive deployments, verify hardware offloading after applying bridge configuration changes.

# 🗁 Understanding Bridge Firewall

RouterOS provides bridge firewall facilities for filtering Layer 2 traffic.

Bridge firewall rules are configured under:

```routeros
/interface bridge filter
```

Bridge NAT rules are configured under:

```routeros
/interface bridge nat
```

Display bridge filter rules:

```routeros
/interface bridge filter print
```

Display bridge NAT rules:

```routeros
/interface bridge nat print
```

Bridge firewall processing can introduce additional CPU load and may affect hardware offloading.

Use bridge firewall only when Layer 2 filtering requirements justify it.

For ordinary routed firewall policies, the IP firewall is generally the appropriate place to configure filtering.

# 🗁 Removing an Interface from a Bridge

Remove a bridge port using its interface name:

```routeros
/interface bridge port remove [find interface=ether4]
```

Verify:

```routeros
/interface bridge port print
```

The physical interface is no longer a member of that bridge.

Its traffic will no longer be forwarded through the bridge as one of its ports.

# 🗁 Disabling and Enabling a Bridge

Disable a bridge:

```routeros
/interface bridge disable bridge-LAN
```

Enable it again:

```routeros
/interface bridge enable bridge-LAN
```

Verify the status:

```routeros
/interface bridge print
```

Disabling a bridge can interrupt communication for all connected bridge ports and services using that bridge interface.

# 🗁 Removing a Bridge

Before removing a bridge, inspect its configuration and dependencies.

Display bridge ports:

```routeros
/interface bridge port print
```

Remove its member ports individually:

```routeros
/interface bridge port remove [find bridge=bridge-LAN]
```

Remove the bridge:

```routeros
/interface bridge remove bridge-LAN
```

Verify:

```routeros
/interface bridge print
```

**Important:** Removing a bridge can interrupt IP addressing, DHCP, VLAN, firewall, and other configurations that depend on it. Review those dependencies before deletion.

# 🗁 Practical Lab: Creating a LAN Bridge

This lab demonstrates how to create a simple Layer 2 LAN using MikroTik RouterOS.

**Lab topology:**

```text
                 MikroTik Router
                 ┌──────────────┐
                 │              │
                 │  bridge-LAN  │
                 │              │
                 └──────┬───────┘
                        │
                ┌───────┼───────┐
                │       │       │
              ether2  ether3  ether4
                │       │       │
               PC-01  PC-02   Switch
```

**Lab requirements:**

* MikroTik CHR or a physical MikroTik router.
* Three available Ethernet interfaces, or equivalent virtual interfaces.
* WinBox or SSH access.
* Two client devices for connectivity testing, if available.

**Create the bridge**

```routeros
/interface bridge add name=bridge-LAN protocol-mode=rstp
```

**Add Ethernet ports**

```routeros
/interface bridge port add bridge=bridge-LAN interface=ether2
/interface bridge port add bridge=bridge-LAN interface=ether3
/interface bridge port add bridge=bridge-LAN interface=ether4
```

**Assign the LAN IP address**

```routeros
/ip address add address=192.168.10.1/24 interface=bridge-LAN
```

**Inspect the bridge**

```routeros
/interface bridge print detail
```

**Inspect bridge ports**

```routeros
/interface bridge port print detail
```

**Inspect IP addressing**

```routeros
/ip address print
```

**Configure a test client**

Configure a connected computer with:

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

Configure a second client:

```text
IP Address:      192.168.10.11
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

**Test connectivity**

From the first client:

```bash
ping 192.168.10.1
```

Test communication with the second client:

```bash
ping 192.168.10.11
```

If both clients are connected to ports in the same bridge and their local firewall policies permit ICMP, they should be able to communicate at Layer 2.

**Inspect hardware offloading**

```routeros
/interface bridge port print
```

Check whether supported ports show the hardware-offloading flag.

**Lab objectives:**

* Create a bridge interface.
* Add physical interfaces as bridge ports.
* Assign an IP address to the bridge.
* Understand Layer 2 forwarding.
* Verify bridge membership.
* Inspect hardware offloading status.
* Test connectivity between LAN devices.

# 🗁 Bridge Troubleshooting

**Clients cannot communicate through the bridge**

Inspect:

```routeros
/interface bridge print
/interface bridge port print
/ip address print
```

Verify that the expected interfaces belong to the correct bridge and that the client IP configuration is valid.

**Bridge is not running**

Inspect:

```routeros
/interface bridge print detail
/interface bridge port print
```

Check whether member interfaces are enabled and whether at least one relevant port has an active link.

**IP address is configured on the wrong interface**

Inspect:

```routeros
/ip address print
```

For a typical bridged LAN, configure the LAN gateway address on the bridge interface rather than on an individual member port.

**Hardware offloading is not active**

Inspect:

```routeros
/interface bridge port print
/interface bridge print detail
```

Check the router's hardware capabilities and whether the selected bridge features and port arrangement support offloading.

**Network experiences broadcast storms**

Inspect the Layer 2 topology and spanning-tree configuration.

Check:

```routeros
/interface bridge print detail
```

Ensure that redundant Layer 2 paths are protected by an appropriate loop-prevention design.

# 🗁 Bridge Configuration Best Practices

* Use meaningful bridge names such as `bridge-LAN`.
* Enable RSTP or another suitable spanning-tree mode where appropriate.
* Assign LAN IP addresses to the bridge interface rather than its member ports.
* Verify bridge membership before applying IP or firewall configuration.
* Check hardware offloading on supported devices.
* Avoid unnecessary bridge firewall processing.
* Understand VLAN filtering before deploying multiple VLANs.
* Document physical port connections.
* Review dependent services before removing a bridge.
* Use safe configuration practices when modifying the bridge carrying management traffic.

# 🗁 Official Documentation

* [MikroTik Bridge Documentation](https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
