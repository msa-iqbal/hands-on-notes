# MikroTik Bridge Ports

A bridge port is an interface that has been added to a MikroTik bridge.

Bridge ports allow physical and logical interfaces to participate in the same Layer 2 network. The bridge forwards Ethernet frames between these ports based on MAC address learning and forwarding decisions.

In RouterOS, bridge ports are managed through:

```routeros
/interface bridge port
```

Understanding bridge ports is important for configuring LAN connectivity, VLANs, hardware offloading, spanning tree, and Layer 2 network security.

# 🗁 Understanding Bridge Ports

A bridge connects multiple interfaces into one Layer 2 forwarding domain.

For example:

```text
                 MikroTik Router
                 ┌──────────────┐
                 │  bridge-LAN  │
                 └──────┬───────┘
                        │
              ┌─────────┼─────────┐
              │         │         │
           ether2     ether3    ether4
              │         │         │
            PC-01     Switch     Access Point
```

In this topology:

* `bridge-LAN` is the logical bridge interface.
* `ether2`, `ether3`, and `ether4` are bridge ports.
* The bridge forwards Ethernet frames between eligible ports.
* Connected devices can communicate within the same Layer 2 network.

A physical interface can generally belong to only one bridge at a time.

# 🗁 Viewing Bridge Ports

Display all configured bridge ports:

```routeros
/interface bridge port print
```

Display detailed information:

```routeros
/interface bridge port print detail
```

Display ports belonging to a specific bridge:

```routeros
/interface bridge port print where bridge=bridge-LAN
```

Display a specific interface:

```routeros
/interface bridge port print where interface=ether2
```

Important properties include:

| Property | Description |
| --- | --- |
| `interface` | Interface assigned to the bridge |
| `bridge` | Bridge containing the interface |
| `pvid` | Port VLAN ID used for untagged ingress traffic when VLAN filtering is enabled |
| `priority` | Bridge port priority used by spanning tree |
| `path-cost` | Spanning-tree path cost |
| `hw` | Hardware offloading setting, where supported |
| `edge` | Edge-port behavior |
| `point-to-point` | Point-to-point link behavior |
| `frame-types` | Accepted Ethernet frame types |
| `ingress-filtering` | Controls VLAN ingress filtering |

Available properties depend on the RouterOS version and interface type.

# 🗁 Adding Interfaces to a Bridge

Create a bridge:

```routeros
/interface bridge add name=bridge-LAN protocol-mode=rstp
```

Add Ethernet interfaces:

```routeros
/interface bridge port add bridge=bridge-LAN interface=ether2
/interface bridge port add bridge=bridge-LAN interface=ether3
/interface bridge port add bridge=bridge-LAN interface=ether4
```

Verify the configuration:

```routeros
/interface bridge port print
```

The selected interfaces are now members of `bridge-LAN`.

# 🗁 Adding Multiple Bridge Ports

Multiple interfaces can be added through separate commands.

For example:

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

This approach is useful when configuring several LAN ports.

Verify:

```routeros
/interface bridge port print where bridge=bridge-LAN
```

# 🗁 Understanding PVID

PVID stands for Port VLAN ID.

It identifies the VLAN assigned to untagged ingress traffic received on a bridge port when VLAN filtering is enabled.

For example, if a bridge port has PVID 10, untagged incoming frames are classified into VLAN 10.

Configure a PVID:

```routeros
/interface bridge port set [find interface=ether2] pvid=10
```

Verify:

```routeros
/interface bridge port print detail where interface=ether2
```

PVID is especially important in VLAN-aware bridge configurations.

**Important:** Setting a PVID alone does not complete VLAN configuration. The bridge VLAN table and VLAN filtering must also be configured correctly.

# 🗁 Understanding Bridge Port Priority

Bridge port priority is used by Spanning Tree Protocol when selecting paths through a Layer 2 topology.

A lower numerical priority is preferred when other relevant path-selection conditions are equal.

Configure port priority:

```routeros
/interface bridge port set [find interface=ether2] priority=0x80
```

Verify:

```routeros
/interface bridge port print detail where interface=ether2
```

The default priority is commonly `0x80`.

Changing bridge port priority can influence spanning-tree topology decisions, but it does not guarantee that a particular port will become the root port or designated port.

# 🗁 Understanding Path Cost

Path cost is a spanning-tree parameter used to determine the preferred path toward the root bridge.

Lower path cost is generally preferred.

Configure path cost:

```routeros
/interface bridge port set [find interface=ether2] path-cost=10
```

Verify:

```routeros
/interface bridge port print detail where interface=ether2
```

Path cost can be used to influence redundant Layer 2 path selection.

The appropriate values depend on the network design, link speeds, and spanning-tree configuration.

# 🗁 Understanding Edge Ports

An edge port is a bridge port expected to connect to an endpoint rather than another switching bridge.

Examples include ports connected to:

* Desktop computers.
* Servers.
* Printers.
* End-user devices.

Configure a port as an edge port:

```routeros
/interface bridge port set [find interface=ether2] edge=yes
```

A port connected to another switch should generally not be treated as an edge port.

Edge-port behavior can help reduce the time required for endpoint connectivity after a link becomes active.

Use edge-port settings according to the actual Layer 2 topology.

# 🗁 Understanding Point-to-Point Ports

Point-to-point settings describe the expected link characteristics for spanning-tree operation.

A full-duplex Ethernet connection between two devices is commonly treated as a point-to-point link.

Configure the setting:

```routeros
/interface bridge port set [find interface=ether2] point-to-point=yes
```

Automatic detection is generally appropriate when the connection characteristics are not being explicitly controlled.

Verify:

```routeros
/interface bridge port print detail where interface=ether2
```

# 🗁 Understanding Ingress Filtering

Ingress filtering controls whether VLAN-tagged frames are accepted based on the VLAN membership configured for a bridge port.

Enable ingress filtering:

```routeros
/interface bridge port set [find interface=ether2] ingress-filtering=yes
```

This setting is used with VLAN-aware bridging.

Ingress filtering helps prevent frames belonging to unauthorized VLANs from entering through a port.

The bridge VLAN table and port frame-type configuration must be designed consistently with ingress filtering.

# 🗁 Understanding Frame Types

The `frame-types` property controls which Ethernet frame types are accepted on a bridge port.

Common options include:

| Setting | Description |
| --- | --- |
| `admit-all` | Accept tagged and untagged frames |
| `admit-only-vlan-tagged` | Accept only VLAN-tagged frames |
| `admit-only-untagged-and-priority-tagged` | Accept untagged and priority-tagged frames |

Allow all frame types:

```routeros
/interface bridge port set [find interface=ether2] frame-types=admit-all
```

Allow only tagged frames:

```routeros
/interface bridge port set [find interface=ether3] frame-types=admit-only-vlan-tagged
```

Allow only untagged and priority-tagged frames:

```routeros
/interface bridge port set [find interface=ether4] frame-types=admit-only-untagged-and-priority-tagged
```

These settings are particularly important when configuring access and trunk ports.

# 🗁 Understanding Hardware Offloading

Hardware offloading allows supported switch chips to forward eligible Ethernet traffic without requiring the CPU to process every frame.

RouterOS can display whether a bridge port is hardware-offloaded.

Inspect bridge ports:

```routeros
/interface bridge port print
```

A common flag is:

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

Hardware offloading depends on the router model, switch chip, port arrangement, and enabled bridge features.

Not every RouterOS device supports hardware offloading.

# 🗁 Disabling a Bridge Port

Disable a bridge port:

```routeros
/interface bridge port disable [find interface=ether4]
```

Verify:

```routeros
/interface bridge port print
```

A disabled bridge port no longer participates in normal bridge forwarding.

# 🗁 Enabling a Bridge Port

Enable a bridge port:

```routeros
/interface bridge port enable [find interface=ether4]
```

Verify:

```routeros
/interface bridge port print
```

Enabling a bridge port restores its participation in bridge forwarding, subject to the interface's operational state and other configuration.

# 🗁 Removing a Bridge Port

Remove a port from a bridge:

```routeros
/interface bridge port remove [find interface=ether4]
```

Verify:

```routeros
/interface bridge port print
```

Removing a bridge port does not delete the physical Ethernet interface. It only removes that interface's membership in the bridge.

# 🗁 Understanding Bridge Port Isolation

Bridge port isolation controls whether traffic can be forwarded between selected bridge ports.

RouterOS provides the `horizon` property for split-horizon forwarding behavior.

Ports with the same non-zero horizon value are prevented from forwarding traffic to one another through the bridge.

Example:

```routeros
/interface bridge port set [find interface=ether2] horizon=10
/interface bridge port set [find interface=ether3] horizon=10
```

This prevents normal bridge forwarding between those two ports.

Other ports with a different horizon value can still participate in forwarding according to the bridge configuration.

Split-horizon behavior is useful in some Layer 2 service-provider and isolation designs.

It is not a replacement for a complete VLAN or firewall security policy.

# 🗁 Practical Lab: Bridge Port Configuration

This lab demonstrates bridge port management using RouterOS v7.

**Lab topology:**

```text
                  MikroTik Router
                  ┌──────────────┐
                  │ bridge-LAN   │
                  └──────┬───────┘
                         │
               ┌─────────┼─────────┐
               │         │         │
             ether2    ether3    ether4
               │         │         │
              PC-01    Switch    Access Point
```

**Lab requirements:**

* MikroTik CHR or physical MikroTik router.
* Three available Ethernet interfaces, or equivalent virtual interfaces.
* WinBox or SSH access.

**Create the bridge**

```routeros
/interface bridge add name=bridge-LAN protocol-mode=rstp
```

**Add bridge ports**

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

**Configure port comments**

```routeros
/interface bridge port set [find interface=ether2] comment="Office Computer"
/interface bridge port set [find interface=ether3] comment="LAN Switch"
/interface bridge port set [find interface=ether4] comment="Wireless Access Point"
```

**Configure edge-port behavior**

For an endpoint directly connected to `ether2`:

```routeros
/interface bridge port set [find interface=ether2] edge=yes
```

**Inspect bridge port configuration**

```routeros
/interface bridge port print detail
```

**Inspect hardware offloading**

```routeros
/interface bridge port print
```

**Temporarily disable a bridge port**

```routeros
/interface bridge port disable [find interface=ether4]
```

Enable it again:

```routeros
/interface bridge port enable [find interface=ether4]
```

**Verify the final configuration**

```routeros
/interface bridge print detail
/interface bridge port print detail
```

**Lab objectives:**

* Add physical interfaces to a bridge.
* Understand bridge port properties.
* Configure PVID and port priority.
* Understand edge-port behavior.
* Inspect hardware offloading.
* Enable and disable bridge ports.
* Remove bridge port membership without deleting the physical interface.

# 🗁 Bridge Port Troubleshooting

**Interface does not appear in the expected bridge**

Check:

```routeros
/interface bridge port print detail
```

Confirm that the interface is assigned to the correct bridge.

**Bridge port is inactive**

Check:

```routeros
/interface bridge port print
/interface ethernet monitor ether2 once
```

Verify that the physical interface is enabled and has an active link.

**Hardware offloading is unavailable**

Check:

```routeros
/interface bridge port print
/interface bridge print detail
```

Confirm that the device supports hardware offloading and that the selected bridge configuration is compatible with its switch chip.

**VLAN traffic is not passing**

Check:

```routeros
/interface bridge port print detail
/interface bridge vlan print detail
/interface bridge print detail
```

Review PVID, frame types, ingress filtering, VLAN membership, and VLAN filtering.

**Spanning-tree topology is unexpected**

Check:

```routeros
/interface bridge monitor bridge-LAN
/interface bridge port print detail
```

Review port priority, path cost, edge-port configuration, and the Layer 2 topology.

# 🗁 Bridge Port Best Practices

* Use descriptive comments for important bridge ports.
* Verify bridge membership before assigning IP addresses.
* Configure PVID according to the VLAN design.
* Use appropriate frame types for access and trunk ports.
* Enable ingress filtering in VLAN-aware designs where appropriate.
* Configure edge ports only for endpoint-facing connections.
* Use spanning-tree settings to protect against Layer 2 loops.
* Verify hardware offloading on supported devices.
* Avoid unnecessary changes to bridge port priority and path cost.
* Document the purpose and physical connection of every bridge port.

# 🗁 Official Documentation

* [MikroTik Bridge and Switching](https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
