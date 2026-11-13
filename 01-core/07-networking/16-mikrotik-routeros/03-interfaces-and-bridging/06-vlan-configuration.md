# VLAN Configuration in MikroTik RouterOS

A Virtual Local Area Network (VLAN) logically divides a physical network into multiple isolated Layer 2 broadcast domains.

VLANs allow multiple networks to share the same physical switching infrastructure while keeping their traffic logically separated.

For example, a single MikroTik switch can provide separate networks for:

* VLAN 10 — Management
* VLAN 20 — Office
* VLAN 30 — Guest
* VLAN 40 — Servers

Devices belonging to different VLANs cannot communicate directly at Layer 2. Communication between VLANs requires Layer 3 routing.

MikroTik RouterOS v7 supports VLAN configuration through bridge VLAN filtering, allowing a bridge to perform VLAN-aware switching.

# 🗁 VLAN Fundamentals

A VLAN is identified by a VLAN ID (VID).

The IEEE 802.1Q standard defines VLAN tagging. A VLAN tag contains a 12-bit VLAN ID field, supporting IDs from 0 to 4095. Normal VLAN IDs are 1–4094.

VLANs provide:

* Broadcast-domain separation.
* Logical network segmentation.
* Improved network organization.
* Better security boundaries.
* Flexible network design.
* Efficient use of physical switching infrastructure.

# 🗁 Tagged and Untagged Frames

Ethernet frames can travel through a VLAN-aware network as tagged or untagged frames.

**Tagged frames**

A tagged Ethernet frame contains an 802.1Q header identifying its VLAN.

Tagged traffic is commonly used on trunk links connecting switches, routers, and VLAN-aware access points.

**Untagged frames**

An untagged Ethernet frame does not contain an 802.1Q VLAN tag.

Access ports commonly receive untagged traffic from ordinary end devices. The switch associates that traffic with a VLAN using the port's PVID.

**Priority-tagged frames**

An 802.1Q frame can contain a VLAN ID of 0 while carrying priority information. RouterOS can admit these frames using the `admit-only-untagged-and-priority-tagged` frame type.

# 🗁 Access Ports, Trunk Ports, and Hybrid Ports

**Access port**

An access port connects an endpoint that normally sends and receives untagged Ethernet frames.

* Belongs to one access VLAN.
* Uses a PVID to classify incoming untagged traffic.
* Sends traffic to the endpoint without a VLAN tag.

Example: A desktop connected to VLAN 10.

**Trunk port**

A trunk port carries multiple VLANs between VLAN-aware devices.

* Carries tagged traffic for multiple VLANs.
* Commonly connects switches, routers, and access points.
* Can optionally carry a native VLAN as untagged traffic.

Example: A connection between two MikroTik switches carrying VLANs 10, 20, and 30.

**Hybrid port**

A hybrid port carries both tagged and untagged traffic.

For example, an access point may receive:

* Untagged traffic for its management network.
* Tagged VLAN 20 traffic for corporate Wi-Fi.
* Tagged VLAN 30 traffic for guest Wi-Fi.

# 🗁 RouterOS Bridge VLAN Filtering Architecture

RouterOS uses a VLAN-aware bridge to control VLAN membership and forwarding.

The main components are:

| Component | Purpose |
| --- | --- |
| Bridge | Provides the Layer 2 switching domain |
| Bridge port | Physical or logical interface attached to the bridge |
| PVID | Assigns incoming untagged traffic to a VLAN |
| VLAN table | Defines tagged and untagged VLAN membership |
| Ingress filtering | Rejects frames that are not allowed on an ingress port |
| Frame types | Controls which types of Ethernet frames a port accepts |
| VLAN interface | Provides Layer 3 access to a VLAN |

**The bridge interface and CPU port**

The bridge interface is important because it represents the router's CPU-facing connection to the bridged network.

When RouterOS needs to communicate with a VLAN through a VLAN interface attached to the bridge, the bridge itself must be included as a tagged member of that VLAN.

For example:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=bridge-LAN,ether1
```

Here, `bridge-LAN` is the CPU-facing bridge interface, while `ether1` is a physical trunk port.

Without the bridge interface in the appropriate VLAN membership, Layer 3 services on the router may not receive that VLAN's traffic.

# 🗁 PVID (Port VLAN ID)

PVID determines which VLAN receives untagged and priority-tagged ingress traffic on a bridge port.

For example:

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2 pvid=10
```

Incoming untagged traffic on `ether2` is classified into VLAN 10.

PVID does not independently define which VLANs a port can transmit. The bridge VLAN table controls VLAN membership and egress tagging.

# 🗁 Ingress Filtering

Ingress filtering controls whether a bridge port accepts frames belonging to VLANs that are not permitted on that port.

Enable it with:

```routeros
/interface bridge port
set [find interface=ether2] ingress-filtering=yes
```

For example, if `ether2` is an access port assigned to VLAN 10, ingress filtering helps prevent frames belonging to unauthorized VLANs from entering through that port.

# 🗁 Frame Types

The `frame-types` property determines which Ethernet frame types a bridge port accepts.

Common values:

| Frame type | Behavior |
| --- | --- |
| `admit-all` | Accepts tagged and untagged frames |
| `admit-only-untagged-and-priority-tagged` | Accepts untagged and priority-tagged frames |
| `admit-only-vlan-tagged` | Accepts VLAN-tagged frames only |

For a typical access port:

```routeros
frame-types=admit-only-untagged-and-priority-tagged
```

For a trunk port without a native VLAN:

```routeros
frame-types=admit-only-vlan-tagged
```

These settings should match the intended network design.

# 🗁 Example Network Topology

Consider the following network:

```text
                    Internet
                       |
                    ether1
                       |
                MikroTik Router
                       |
                   bridge-LAN
                       |
        +--------------+--------------+
        |              |              |
      ether2         ether3         ether4
        |              |              |
     VLAN 10        VLAN 20       VLAN Trunk
     Office          Guest       Downstream Switch
```

Network configuration:

| Interface | Role | VLAN |
| --- | --- | --- |
| ether1 | Trunk uplink | 10, 20 |
| ether2 | Access port | 10 |
| ether3 | Access port | 20 |
| ether4 | Trunk to switch | 10, 20 |

IP addressing:

| VLAN | Network | Router IP |
| --- | --- | --- |
| VLAN 10 | 192.168.10.0/24 | 192.168.10.1/24 |
| VLAN 20 | 192.168.20.0/24 | 192.168.20.1/24 |

The router will provide Layer 3 connectivity for both VLANs.

# 🗁 Step-by-Step VLAN Configuration

Configure the bridge VLAN filtering carefully. Enabling VLAN filtering before configuring the VLAN table and management path can interrupt access to the router.

The following sequence establishes the bridge, ports, VLAN membership, and Layer 3 interfaces before enabling filtering.

**Create the bridge**

Start with VLAN filtering disabled.

```routeros
/interface bridge
add name=bridge-LAN protocol-mode=rstp vlan-filtering=no
```

**Add physical interfaces to the bridge**

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether1
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

**Configure access ports**

Assign VLAN 10 to `ether2` and VLAN 20 to `ether3`.

```routeros
/interface bridge port
set [find interface=ether2] pvid=10 ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged

set [find interface=ether3] pvid=20 ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged
```

**Configure trunk ports**

Configure `ether1` and `ether4` to accept tagged VLAN traffic only.

```routeros
/interface bridge port
set [find interface=ether1] ingress-filtering=yes frame-types=admit-only-vlan-tagged

set [find interface=ether4] ingress-filtering=yes frame-types=admit-only-vlan-tagged
```

These trunk ports do not carry a native untagged VLAN in this example.

**Configure the bridge VLAN table**

Define which interfaces are members of VLAN 10 and VLAN 20.

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=bridge-LAN,ether1,ether4 untagged=ether2

add bridge=bridge-LAN vlan-ids=20 tagged=bridge-LAN,ether1,ether4 untagged=ether3
```

Membership:

| VLAN | Tagged interfaces | Untagged interfaces |
| --- | --- | --- |
| 10 | bridge-LAN, ether1, ether4 | ether2 |
| 20 | bridge-LAN, ether1, ether4 | ether3 |

The bridge interface is included as a tagged member because the router's CPU needs access to both VLANs.

**Create VLAN interfaces**

Create Layer 3 VLAN interfaces on the bridge.

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10

add name=vlan20 interface=bridge-LAN vlan-id=20
```

**Assign IP addresses**

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10

add address=192.168.20.1/24 interface=vlan20
```

These IP addresses become the default gateways for devices in their respective VLANs.

**Enable VLAN filtering**

After verifying the bridge ports, VLAN membership, and management path, enable VLAN filtering.

```routeros
/interface bridge
set [find name=bridge-LAN] vlan-filtering=yes
```

The bridge now performs VLAN-aware Layer 2 forwarding.

# 🗁 Understanding the VLAN Interface

A VLAN interface provides Layer 3 access to a specific VLAN.

For example:

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10
```

This creates a logical interface associated with VLAN 10.

It can be used for:

* IP address assignment.
* DHCP server configuration.
* Routing.
* Firewall filtering.
* Management access.
* Monitoring and network services.

A VLAN interface attached to a bridge is conceptually similar to an SVI (Switched Virtual Interface) on other networking platforms.

**Important:** For this design, assign the VLAN gateway IP address to the VLAN interface rather than directly to the physical bridge ports.

# 🗁 Inter-VLAN Routing

When multiple VLAN interfaces have IP addresses on the same router, RouterOS can route traffic between their connected networks.

For example:

```text
VLAN 10: 192.168.10.0/24
VLAN 20: 192.168.20.0/24

VLAN 10 Gateway: 192.168.10.1
VLAN 20 Gateway: 192.168.20.1
```

A device in VLAN 10 can communicate with a device in VLAN 20 through the router if routing and firewall policies permit it.

RouterOS automatically installs connected routes for directly attached IP networks.

Inspect the routing table:

```routeros
/ip route print
```

Inter-VLAN communication can be restricted using firewall rules:

```routeros
/ip firewall filter
add chain=forward src-address=192.168.10.0/24 dst-address=192.168.20.0/24 action=drop comment="Block VLAN 10 to VLAN 20"
```

Place firewall rules in an appropriate position relative to existing accept and drop rules. Existing firewall policy determines the final behavior.

# 🗁 Configure a DHCP Server for Each VLAN

Each VLAN can have its own DHCP server and address pool.

Example for VLAN 10:

```routeros
/ip pool
add name=pool-vlan10 ranges=192.168.10.100-192.168.10.200

/ip dhcp-server
add name=dhcp-vlan10 interface=vlan10 address-pool=pool-vlan10 disabled=no

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

Example for VLAN 20:

```routeros
/ip pool
add name=pool-vlan20 ranges=192.168.20.100-192.168.20.200

/ip dhcp-server
add name=dhcp-vlan20 interface=vlan20 address-pool=pool-vlan20 disabled=no

/ip dhcp-server network
add address=192.168.20.0/24 gateway=192.168.20.1 dns-server=192.168.20.1
```

Ensure that the router's DNS service is configured appropriately if clients use the router as their DNS server.

# 🗁 Native VLAN Configuration

A native VLAN is a VLAN whose traffic is transmitted untagged on a trunk link.

For example, VLAN 99 could be used as the native VLAN on `ether1`.

The bridge port can be configured with:

```routeros
/interface bridge port
set [find interface=ether1] pvid=99 ingress-filtering=yes frame-types=admit-all
```

The VLAN table must include the untagged membership:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=99 untagged=ether1
```

If the router itself needs Layer 3 access to VLAN 99, include the bridge interface as a tagged member and create a VLAN interface on the bridge:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=99 tagged=bridge-LAN untagged=ether1

/interface vlan
add name=vlan99 interface=bridge-LAN vlan-id=99
```

A native VLAN must be configured consistently at both ends of the trunk. A mismatch can cause unexpected traffic forwarding or connectivity problems.

For a trunk that should carry only tagged traffic, use `admit-only-vlan-tagged` instead.

# 🗁 VLAN Management Access

Management access should be designed before enabling VLAN filtering.

If the router is managed through VLAN 10, the bridge must be a tagged member of VLAN 10, and the router should have its management IP address on the VLAN 10 interface.

For example:

```routeros
/interface bridge vlan
add bridge=bridge-LAN vlan-ids=10 tagged=bridge-LAN,ether1 untagged=ether2

/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10

/ip address
add address=192.168.10.1/24 interface=vlan10
```

Before enabling filtering:

* Confirm the management VLAN is correctly configured.
* Confirm the management port or upstream trunk carries that VLAN.
* Confirm the router's management IP is reachable through the intended path.
* Use Safe Mode in WinBox or the terminal when making remote bridge changes.
* Keep an alternative management path available where possible.

Safe Mode can automatically roll back changes if the management session unexpectedly terminates, subject to RouterOS Safe Mode behavior.

# 🗁 Verify VLAN Configuration

Use these commands to inspect the configuration.

**Display bridge configuration**

```routeros
/interface bridge print detail
```

**Display bridge ports**

```routeros
/interface bridge port print detail
```

**Display VLAN membership**

```routeros
/interface bridge vlan print detail
```

**Display VLAN interfaces**

```routeros
/interface vlan print detail
```

**Display IP addresses**

```routeros
/ip address print
```

**Display DHCP servers**

```routeros
/ip dhcp-server print
```

**Display learned MAC addresses**

```routeros
/interface bridge host print
```

Check that:

* VLAN IDs are correct.
* Access ports have the expected PVID.
* Trunk ports allow the required tagged VLANs.
* The bridge interface is tagged in VLANs that need CPU access.
* VLAN interfaces use the correct bridge as their parent.
* IP addresses are assigned to the correct VLAN interfaces.
* VLAN filtering is enabled only after the configuration is ready.

# 🗁 Troubleshooting VLAN Problems

**Clients receive no DHCP address**

Possible causes:

* Incorrect access-port PVID.
* Missing untagged VLAN membership.
* Missing bridge VLAN table entry.
* DHCP server attached to the wrong interface.
* Incorrect DHCP pool or network configuration.

Check:

```routeros
/interface bridge port print detail
/interface bridge vlan print detail
/ip dhcp-server print
/ip dhcp-server lease print
```

**VLAN traffic does not pass through a trunk**

Possible causes:

* VLAN missing from the bridge VLAN table.
* Trunk port is not a tagged member.
* The remote switch does not allow the VLAN.
* One side sends untagged traffic while the other expects tagged traffic.
* Frame-type restrictions reject the incoming frames.

Check both ends of the trunk.

**Router cannot communicate with a VLAN**

Possible causes:

* Bridge interface missing from the VLAN's tagged membership.
* VLAN interface attached to the wrong parent.
* Incorrect VLAN ID.
* Missing IP address or incorrect subnet.
* Firewall rules blocking traffic.

Check:

```routeros
/interface bridge vlan print detail
/interface vlan print detail
/ip address print
/ip firewall filter print
```

**Access port receives traffic from the wrong VLAN**

Possible causes:

* Incorrect PVID.
* Incorrect bridge VLAN table membership.
* Unexpected tagged traffic accepted on the access port.
* Ingress filtering disabled or incorrectly configured.

Review the port's PVID, frame types, ingress filtering, and VLAN membership.

**Management access is lost after enabling VLAN filtering**

Possible causes:

* Management VLAN is missing from the VLAN table.
* Bridge interface is not tagged in the management VLAN.
* Management port has the wrong PVID.
* Upstream trunk does not carry the management VLAN.

Use an alternative management path or Safe Mode rollback to recover access.

# 🗁 Hardware Offloading Considerations

VLAN-aware bridging can be hardware-offloaded on supported MikroTik devices.

However, hardware offloading depends on:

* RouterBoard model.
* Switch-chip capabilities.
* RouterOS version.
* Bridge and VLAN configuration.
* Port and feature compatibility.

Some configurations can cause traffic to be processed by the CPU instead of the switch chip.

Inspect bridge port status:

```routeros
/interface bridge port print
```

The `H` flag indicates hardware offloading for supported bridge ports.

Do not assume that enabling VLAN filtering automatically guarantees hardware offloading on every MikroTik device.

# 🗁 Practical VLAN Configuration Lab

Build a small VLAN lab using one MikroTik router and a downstream VLAN-aware switch.

**Lab requirements**

| Device or interface | Configuration |
| --- | --- |
| MikroTik ether1 | Trunk to upstream network |
| MikroTik ether2 | Access VLAN 10 |
| MikroTik ether3 | Access VLAN 20 |
| MikroTik ether4 | Trunk to downstream switch |
| VLAN 10 | 192.168.10.0/24 |
| VLAN 20 | 192.168.20.0/24 |

**Lab tasks**

* Create a VLAN-aware bridge.
* Add the required Ethernet ports.
* Configure access-port PVIDs.
* Configure trunk ports.
* Create bridge VLAN table entries.
* Include the bridge interface as a tagged member.
* Create VLAN interfaces.
* Assign gateway IP addresses.
* Configure DHCP services.
* Enable VLAN filtering.
* Verify VLAN isolation.
* Test inter-VLAN routing.
* Apply firewall rules to control communication.

**Expected results**

* VLAN 10 clients receive addresses from the VLAN 10 DHCP pool.
* VLAN 20 clients receive addresses from the VLAN 20 DHCP pool.
* Clients within the same VLAN can communicate at Layer 2.
* Inter-VLAN communication passes through the router when permitted.
* Trunk links carry the configured VLANs with 802.1Q tags.

# 🗁 Best Practices

* Use a consistent VLAN numbering scheme across the network.
* Document VLAN IDs, names, subnets, and purposes.
* Use dedicated management VLANs where appropriate.
* Configure access ports with the correct PVID.
* Restrict trunk ports to the required VLANs.
* Use ingress filtering on VLAN-aware bridge ports.
* Configure appropriate frame-type restrictions.
* Include the bridge interface in tagged membership when the router CPU needs VLAN access.
* Keep VLAN gateway IP addresses on the corresponding VLAN interfaces.
* Apply firewall policies between VLANs.
* Use Safe Mode when changing VLAN configuration remotely.
* Verify hardware offloading on the target device.
* Maintain a backup before making major network changes.

# 🗁 Official Documentation

* MikroTik Bridge VLAN Table: <https://help.mikrotik.com/docs/spaces/ROS/pages/28606465/Bridge+VLAN+Table>
* MikroTik Bridge Documentation: <https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching>
* MikroTik VLAN Documentation: <https://help.mikrotik.com/docs/>
