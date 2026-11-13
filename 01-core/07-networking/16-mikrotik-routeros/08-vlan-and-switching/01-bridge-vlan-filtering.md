# Bridge VLAN Filtering in MikroTik RouterOS

Bridge VLAN filtering is a MikroTik RouterOS feature that allows a single bridge to handle multiple VLANs while enforcing VLAN membership and traffic separation at Layer 2.

It enables a MikroTik router or switch to operate as a VLAN-aware bridge.

With bridge VLAN filtering, you can:

* Separate multiple networks using VLAN IDs.
* Configure access ports for untagged devices.
* Configure trunk ports to carry tagged VLAN traffic.
* Control which VLANs are allowed on each bridge port.
* Prevent unauthorized VLAN traffic from crossing the bridge.
* Integrate VLANs with Layer 3 routing.
* Build structured networks for offices, campuses, data centers, and ISPs.

RouterOS implements VLAN-aware bridging through the bridge VLAN table and the `vlan-filtering` bridge property.

# 🗁 Understanding VLAN-Aware Bridging

A traditional bridge forwards Ethernet frames based on MAC addresses without enforcing VLAN membership in the same way as a VLAN-aware bridge.

When VLAN filtering is enabled, the bridge checks VLAN information and determines whether a frame is permitted on a particular port.

Consider this topology:

```text
                  MikroTik Router
                 Bridge: br0
                       |
          +------------+------------+
          |            |            |
        ether1       ether2       ether3
        Trunk        Access       Access
          |            |            |
        Switch       VLAN 10      VLAN 20
          |
      VLAN 10, 20
```

In this example:

* `ether1` is a trunk port carrying tagged VLAN 10 and VLAN 20.
* `ether2` is an access port belonging to VLAN 10.
* `ether3` is an access port belonging to VLAN 20.

Devices connected to VLAN 10 and VLAN 20 remain separated at Layer 2.

# 🗁 Important VLAN Filtering Concepts

**PVID**

PVID stands for Port VLAN ID.

It determines which VLAN is assigned to untagged ingress frames received on a bridge port.

For example:

```routeros
/interface/bridge/port
set [find interface=ether2] pvid=10
```

Untagged frames entering through `ether2` are classified into VLAN 10.

PVID does not mean that every frame leaving the port will be tagged. The bridge VLAN table determines whether egress traffic is tagged or untagged.

**Tagged Ports**

A tagged port carries Ethernet frames containing an IEEE 802.1Q VLAN tag.

Tagged ports are commonly used for:

* Switch-to-switch connections.
* Router-to-switch connections.
* VLAN-aware access points.
* Hypervisors and virtualization hosts.

**Untagged Ports**

An untagged port sends Ethernet frames without an 802.1Q VLAN tag.

Untagged ports are commonly used for:

* Desktop computers.
* Printers.
* IP phones configured for untagged access.
* Devices that do not understand VLAN tagging.

**Bridge VLAN Table**

The bridge VLAN table defines which VLANs are permitted on bridge ports and whether frames are tagged or untagged when leaving those ports.

It is configured through:

```routeros
/interface/bridge/vlan
```

**VLAN Filtering**

The `vlan-filtering` property enables VLAN-aware forwarding on a bridge.

```routeros
/interface/bridge
set br0 vlan-filtering=yes
```

When enabled, the bridge enforces VLAN membership according to its VLAN configuration.

# 🗁 Understanding the Bridge CPU Port

In MikroTik RouterOS, the bridge interface itself represents the bridge's CPU-facing port.

This is important because the router's CPU needs to participate in a VLAN when the router must perform Layer 3 operations for that VLAN.

For example, if the router must provide a gateway address for VLAN 10, the bridge interface generally needs to be a tagged member of VLAN 10.

Without the correct bridge VLAN membership, the router may not receive the VLAN traffic required for routing or management.

Example:

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1 untagged=ether2
```

Here:

* `br0` is the bridge CPU-facing port.
* `ether1` is a tagged trunk port.
* `ether2` is an untagged access port.

# 🗁 Configure a VLAN-Aware Bridge

This example creates a bridge with one trunk port and two access ports.

**Network topology**

```text
                    MikroTik Router
                         br0
                          |
              +-----------+-----------+
              |           |           |
            ether1      ether2      ether3
            Trunk       Access      Access
              |           |           |
          VLAN 10,20    VLAN 10     VLAN 20
```

**VLAN plan**

| VLAN ID | VLAN Name | Subnet | Port |
| --- | --- | --- | --- |
| 10 | ADMIN | 192.168.10.0/24 | ether2 |
| 20 | STAFF | 192.168.20.0/24 | ether3 |

`ether1` carries both VLANs as tagged traffic.

# 🗁 Create the Bridge

Create a bridge named `br0`:

```routeros
/interface/bridge
add name=br0 vlan-filtering=no
```

VLAN filtering is initially disabled so that the bridge and VLAN configuration can be prepared before enforcement begins.

# 🗁 Add Bridge Ports

Add the Ethernet interfaces to the bridge:

```routeros
/interface/bridge/port
add bridge=br0 interface=ether1
add bridge=br0 interface=ether2 pvid=10
add bridge=br0 interface=ether3 pvid=20
```

Port configuration:

| Interface | Role | PVID |
| --- | --- | --- |
| ether1 | Trunk | 1 |
| ether2 | Access VLAN 10 | 10 |
| ether3 | Access VLAN 20 | 20 |

The trunk's PVID is not used for the tagged VLAN 10 and VLAN 20 traffic in this example.

# 🗁 Configure the Bridge VLAN Table

Define VLAN membership:

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1 untagged=ether2
add bridge=br0 vlan-ids=20 tagged=br0,ether1 untagged=ether3
```

The VLAN membership is:

| VLAN | Tagged Ports | Untagged Ports |
| --- | --- | --- |
| 10 | br0, ether1 | ether2 |
| 20 | br0, ether1 | ether3 |

The bridge VLAN table determines the permitted VLAN membership and egress tagging behavior.

# 🗁 Enable VLAN Filtering

After configuring the bridge ports and VLAN table, enable VLAN filtering:

```routeros
/interface/bridge
set br0 vlan-filtering=yes
```

The bridge now performs VLAN-aware forwarding.

**Important:** Apply this configuration carefully when managing a router remotely. Incorrect VLAN membership can disconnect your management session. Use RouterOS Safe Mode or a local console during initial configuration.

# 🗁 Verify Bridge VLAN Configuration

Inspect bridge ports:

```routeros
/interface/bridge/port
print detail
```

Inspect the bridge VLAN table:

```routeros
/interface/bridge/vlan
print detail
```

Inspect bridge settings:

```routeros
/interface/bridge
print detail
```

Check the following:

* The correct interfaces belong to the bridge.
* Access ports have the expected PVID.
* Trunk ports are tagged members of the required VLANs.
* Access ports are untagged members of their assigned VLANs.
* The bridge interface is included as a tagged member when the CPU needs access to the VLAN.
* VLAN filtering is enabled.

# 🗁 Ingress Filtering

Ingress filtering controls whether a bridge port accepts incoming VLAN-tagged frames belonging to VLANs that are not configured as members of that port.

Example:

```routeros
/interface/bridge/port
set [find interface=ether1] ingress-filtering=yes
```

With ingress filtering enabled, the bridge checks incoming VLAN membership against the bridge VLAN table.

This is particularly important for trunk ports because it prevents unauthorized VLAN traffic from entering through a port that is not configured to carry that VLAN.

# 🗁 Frame Types

The `frame-types` property controls which types of Ethernet frames a bridge port accepts.

Common values include:

| Value | Behavior |
| --- | --- |
| `admit-all` | Accept tagged and untagged frames |
| `admit-only-vlan-tagged` | Accept only VLAN-tagged frames |
| `admit-only-untagged-and-priority-tagged` | Accept untagged and priority-tagged frames |

For a trunk port carrying tagged VLAN traffic:

```routeros
/interface/bridge/port
set [find interface=ether1] \
    ingress-filtering=yes \
    frame-types=admit-only-vlan-tagged
```

For an access port accepting untagged devices:

```routeros
/interface/bridge/port
set [find interface=ether2] \
    ingress-filtering=yes \
    frame-types=admit-only-untagged-and-priority-tagged
```

Choose frame types according to the actual network design. A trunk that must also carry untagged traffic requires a different configuration.

# 🗁 Configure a Strict Trunk and Access Port Design

The previous configuration can be strengthened by explicitly configuring ingress filtering and frame types.

**Trunk port:**

```routeros
/interface/bridge/port
set [find interface=ether1] \
    ingress-filtering=yes \
    frame-types=admit-only-vlan-tagged
```

**Access port for VLAN 10:**

```routeros
/interface/bridge/port
set [find interface=ether2] \
    pvid=10 \
    ingress-filtering=yes \
    frame-types=admit-only-untagged-and-priority-tagged
```

**Access port for VLAN 20:**

```routeros
/interface/bridge/port
set [find interface=ether3] \
    pvid=20 \
    ingress-filtering=yes \
    frame-types=admit-only-untagged-and-priority-tagged
```

These settings provide a clearer separation between tagged trunk traffic and untagged access traffic.

# 🗁 VLAN Filtering and Layer 3 Routing

Bridge VLAN filtering provides Layer 2 VLAN separation. It does not automatically create IP addresses, gateways, DHCP servers, or inter-VLAN routing.

To route between VLANs, the router needs Layer 3 interfaces for the relevant VLANs.

A common RouterOS design creates VLAN interfaces on the bridge:

```routeros
/interface/vlan
add name=vlan10 interface=br0 vlan-id=10
add name=vlan20 interface=br0 vlan-id=20
```

Then assign IP addresses:

```routeros
/ip/address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
```

The bridge must be a tagged member of both VLANs, as configured in the bridge VLAN table.

RouterOS can now route traffic between VLAN 10 and VLAN 20, subject to its IP firewall rules and routing configuration.

# 🗁 Hardware Offloading Considerations

MikroTik devices may support hardware-assisted bridge VLAN filtering through their switch chips.

Hardware offloading can improve forwarding performance by processing eligible traffic in hardware rather than sending every frame through the router CPU.

However, support depends on:

* Router model.
* Switch chip architecture.
* RouterOS version.
* Bridge configuration.
* VLAN configuration.
* Enabled bridge features.

Inspect bridge port status:

```routeros
/interface/bridge/port
print
```

On supported devices, the `H` flag indicates hardware offloading.

Not every configuration or device supports hardware offloading for every bridge feature. Verify the capabilities of the specific MikroTik hardware before relying on hardware forwarding.

# 🗁 Common Configuration Mistakes

**Bridge VLAN Table Is Missing**

If a VLAN is not defined correctly in the bridge VLAN table, traffic belonging to that VLAN may be dropped when VLAN filtering is enabled.

**Bridge CPU Port Is Missing**

If the router needs to communicate with a VLAN but the bridge interface is not included as a tagged member, the router may not receive the required VLAN traffic.

**Incorrect PVID**

An incorrect PVID assigns untagged ingress frames to the wrong VLAN.

**Trunk Port Configured as Untagged**

If a trunk port is configured as an untagged member of a VLAN unintentionally, devices connected through that trunk may receive traffic without the expected VLAN tags.

**Ingress Filtering Is Disabled**

Without ingress filtering, a port may accept VLAN-tagged traffic that is not intended to be carried by that port, depending on the rest of the bridge configuration.

**Enabling VLAN Filtering Too Early**

Enabling VLAN filtering before completing the bridge VLAN table can interrupt network connectivity.

Prepare the bridge ports and VLAN table first, then enable filtering.

# 🗁 Best Practices

* Use a clear VLAN ID and naming convention.
* Configure PVID values explicitly on access ports.
* Define tagged and untagged membership carefully.
* Enable ingress filtering where appropriate.
* Use strict frame types for access and trunk ports.
* Include the bridge interface as a tagged member when the router CPU needs VLAN access.
* Enable VLAN filtering only after validating the configuration.
* Use Safe Mode during remote changes.
* Verify hardware offloading support on the target device.
* Keep management VLAN access separate from ordinary user VLANs.
* Document trunk allowed VLANs and access port assignments.

# 🗁 Summary

Bridge VLAN filtering allows MikroTik RouterOS to enforce VLAN membership and forwarding rules at Layer 2.

The key components are:

* **Bridge:** Provides the VLAN-aware Layer 2 switching domain.
* **Bridge ports:** Define which physical interfaces participate in the bridge.
* **PVID:** Assigns untagged ingress frames to a VLAN.
* **Bridge VLAN table:** Defines tagged and untagged VLAN membership.
* **Ingress filtering:** Checks incoming VLAN membership.
* **Frame types:** Controls which Ethernet frame types a port accepts.
* **VLAN filtering:** Enables VLAN-aware forwarding.
* **Bridge CPU port:** Allows the router itself to participate in VLANs.

Understanding these components is essential before configuring access ports, trunk ports, and inter-VLAN routing in MikroTik RouterOS.
