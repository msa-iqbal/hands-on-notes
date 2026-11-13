# Access and Trunk Ports in MikroTik RouterOS

Access ports and trunk ports are fundamental components of VLAN-based network design.

They determine how Ethernet frames enter and leave a switch or VLAN-aware bridge.

In MikroTik RouterOS, access and trunk behavior is configured through bridge port properties and the bridge VLAN table.

**Access port**

An access port connects an endpoint to a specific VLAN. It typically receives untagged Ethernet frames and assigns them to a VLAN using its PVID.

**Trunk port**

A trunk port carries traffic for multiple VLANs between VLAN-aware devices. It typically transports Ethernet frames with IEEE 802.1Q VLAN tags.

Understanding these port types is essential when connecting MikroTik routers to managed switches, access points, servers, and other network devices.

# 🗁 Access Port

An access port belongs to a specific VLAN and normally connects devices that do not need to understand VLAN tagging.

Examples include:

* Desktop computers.
* Printers.
* IP cameras.
* Ordinary servers.
* Network devices connected to a single VLAN.

Consider this topology:

```text
             MikroTik Switch
                  br0
                   |
                ether2
              Access Port
               VLAN 10
                   |
                   |
                Computer
             192.168.10.10
```

The computer sends and receives ordinary untagged Ethernet frames.

The MikroTik bridge classifies incoming untagged frames into VLAN 10 using the port's PVID. Outgoing VLAN 10 traffic is transmitted untagged through that access port.

# 🗁 Configure an Access Port

Assume the following configuration:

| Parameter | Value |
| --- | --- |
| Bridge | br0 |
| Interface | ether2 |
| VLAN ID | 10 |
| PVID | 10 |
| Port type | Access |

**Create the bridge:**

```routeros
/interface/bridge
add name=br0 vlan-filtering=no
```

**Add the access port:**

```routeros
/interface/bridge/port
add bridge=br0 interface=ether2 pvid=10
```

**Configure VLAN membership:**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0 untagged=ether2
```

**Configure ingress filtering and frame types:**

```routeros
/interface/bridge/port
set [find interface=ether2] \
    ingress-filtering=yes \
    frame-types=admit-only-untagged-and-priority-tagged
```

**Enable VLAN filtering:**

```routeros
/interface/bridge
set br0 vlan-filtering=yes
```

The port now behaves as an access port for VLAN 10.

# 🗁 Multiple Access Ports

A bridge can contain multiple access ports belonging to different VLANs.

Example topology:

```text
                 MikroTik Router
                      br0
                       |
          +------------+------------+
          |            |            |
        ether2       ether3       ether4
          |            |            |
       VLAN 10      VLAN 20      VLAN 30
          |            |            |
        PC 1         PC 2         PC 3
```

**Configure the bridge ports:**

```routeros
/interface/bridge/port
add bridge=br0 interface=ether2 pvid=10
add bridge=br0 interface=ether3 pvid=20
add bridge=br0 interface=ether4 pvid=30
```

**Configure VLAN membership:**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0 untagged=ether2
add bridge=br0 vlan-ids=20 tagged=br0 untagged=ether3
add bridge=br0 vlan-ids=30 tagged=br0 untagged=ether4
```

Each access port belongs to one VLAN.

Devices connected to different VLANs remain isolated at Layer 2 unless Layer 3 routing is configured.

# 🗁 Trunk Port

A trunk port carries multiple VLANs over a single physical Ethernet connection.

Unlike an access port, a trunk port normally transports tagged Ethernet frames.

Trunk ports are commonly used for:

* Router-to-switch connections.
* Switch-to-switch connections.
* VLAN-aware wireless access points.
* Hypervisors.
* Virtualization servers.
* Network infrastructure links.

Example topology:

```text
             MikroTik Router
                   |
                 ether1
               Trunk Port
                   |
                   |
             Managed Switch
              /          \
             /            \
          VLAN 10        VLAN 20
            |              |
           PC 1           PC 2
```

The trunk carries VLAN 10 and VLAN 20 between the MikroTik router and the managed switch.

# 🗁 Configure a Trunk Port

Assume the following configuration:

| Parameter | Value |
| --- | --- |
| Bridge | br0 |
| Interface | ether1 |
| VLAN 10 | Tagged |
| VLAN 20 | Tagged |
| Port type | Trunk |

**Add the trunk port:**

```routeros
/interface/bridge/port
add bridge=br0 interface=ether1 \
    ingress-filtering=yes \
    frame-types=admit-only-vlan-tagged
```

**Configure VLAN membership:**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1
add bridge=br0 vlan-ids=20 tagged=br0,ether1
```

This configuration permits tagged VLAN 10 and VLAN 20 traffic on `ether1`.

The bridge interface is also included as a tagged member so the router CPU can participate in these VLANs.

# 🗁 Configure Access and Trunk Ports Together

Consider a network with:

* One trunk port connected to a managed switch.
* One access port for VLAN 10.
* One access port for VLAN 20.

**Topology:**

```text
                  MikroTik Router
                       br0
                        |
           +------------+------------+
           |            |            |
         ether1       ether2       ether3
         Trunk        Access       Access
           |            |            |
       Managed         PC 1         PC 2
        Switch        VLAN 10      VLAN 20
           |
       VLAN 10, 20
```

**VLAN plan:**

| VLAN ID | Name | Interface | Port Mode |
| --- | --- | --- | --- |
| 10 | ADMIN | ether2 | Access |
| 20 | STAFF | ether3 | Access |
| 10, 20 | Multiple VLANs | ether1 | Trunk |

**Create the bridge:**

```routeros
/interface/bridge
add name=br0 vlan-filtering=no
```

**Configure bridge ports:**

```routeros
/interface/bridge/port
add bridge=br0 interface=ether1 \
    ingress-filtering=yes \
    frame-types=admit-only-vlan-tagged

add bridge=br0 interface=ether2 \
    pvid=10 \
    ingress-filtering=yes \
    frame-types=admit-only-untagged-and-priority-tagged

add bridge=br0 interface=ether3 \
    pvid=20 \
    ingress-filtering=yes \
    frame-types=admit-only-untagged-and-priority-tagged
```

**Configure the bridge VLAN table:**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1 untagged=ether2
add bridge=br0 vlan-ids=20 tagged=br0,ether1 untagged=ether3
```

**Enable VLAN filtering:**

```routeros
/interface/bridge
set br0 vlan-filtering=yes
```

The resulting behavior is:

| Interface | VLAN 10 | VLAN 20 |
| --- | --- | --- |
| ether1 | Tagged | Tagged |
| ether2 | Untagged | Not permitted |
| ether3 | Not permitted | Untagged |
| br0 | Tagged | Tagged |

# 🗁 Native VLAN and Untagged Traffic on Trunks

A trunk can optionally carry untagged traffic in addition to tagged VLAN traffic.

The VLAN associated with untagged ingress traffic is determined by the trunk port's PVID.

For example:

```routeros
/interface/bridge/port
set [find interface=ether1] pvid=99
```

If VLAN 99 is configured as an untagged member of `ether1`, untagged egress traffic for VLAN 99 will leave the trunk without an 802.1Q tag.

Example:

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=99 tagged=br0 untagged=ether1
```

This creates an untagged VLAN 99 membership on the trunk port.

The term *native VLAN* is commonly used for this behavior in networking environments, although MikroTik configuration uses PVID and bridge VLAN membership rather than a separate native VLAN setting.

For consistency and security, many network designs prefer tagged-only trunks.

# 🗁 Tagged and Untagged VLAN Membership

The bridge VLAN table controls how VLAN traffic is handled on each port.

**Tagged membership**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1
```

VLAN 10 frames leave `br0` and `ether1` tagged.

**Untagged membership**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 untagged=ether2
```

VLAN 10 frames leave `ether2` untagged.

**Combined membership**

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1 untagged=ether2
```

This configuration allows VLAN 10 to travel tagged through `ether1` and untagged through `ether2`.

# 🗁 Restrict VLANs on Trunk Ports

A trunk should carry only the VLANs required by the connected device.

For example, if `ether1` must carry VLAN 10 and VLAN 20 but not VLAN 30, do not include VLAN 30 in its bridge VLAN membership.

Example:

```routeros
/interface/bridge/vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1
add bridge=br0 vlan-ids=20 tagged=br0,ether1
```

With ingress filtering enabled, incoming tagged traffic for VLANs not permitted on the trunk is rejected.

This reduces unnecessary VLAN exposure between network devices.

# 🗁 Verify Access and Trunk Configuration

Inspect bridge ports:

```routeros
/interface/bridge/port
print detail
```

Inspect VLAN membership:

```routeros
/interface/bridge/vlan
print detail
```

Inspect the bridge:

```routeros
/interface/bridge
print detail
```

Check:

* Access port PVID values.
* Trunk port frame types.
* Ingress filtering settings.
* Tagged VLAN membership.
* Untagged VLAN membership.
* Bridge CPU port membership.
* VLAN filtering status.

# 🗁 Common Problems and Troubleshooting

**Access Device Does Not Receive Network Connectivity**

Possible causes:

* Incorrect PVID.
* Missing untagged VLAN membership.
* VLAN filtering is enabled before VLAN configuration is complete.
* Incorrect bridge port assignment.

Check:

```routeros
/interface/bridge/port/print detail
/interface/bridge/vlan/print detail
```

**Trunk Does Not Carry VLAN Traffic**

Possible causes:

* VLAN is missing from the bridge VLAN table.
* Trunk port is not a tagged member.
* The connected switch has a mismatched VLAN configuration.
* Ingress filtering rejects the incoming VLAN.
* The connected device sends untagged frames while the trunk accepts tagged frames only.

Verify VLAN membership and the configuration at both ends of the trunk.

**VLAN Traffic Is Untagged Unexpectedly**

Possible causes:

* The port is configured as an untagged VLAN member.
* The port PVID is incorrect.
* The connected device expects tagged traffic but receives untagged traffic.

Inspect the bridge VLAN table and port PVID.

**Router Cannot Communicate with a VLAN**

Possible causes:

* The bridge interface is missing from the VLAN's tagged membership.
* The VLAN interface is attached to the wrong parent interface.
* The router's IP address is assigned to the wrong interface.

For router CPU access, verify that the bridge interface is a tagged member of the relevant VLAN.

# 🗁 Best Practices

* Use access ports for ordinary single-VLAN endpoints.
* Use trunk ports for links that transport multiple VLANs.
* Explicitly configure PVID values on access ports.
* Configure tagged VLAN membership on trunks.
* Enable ingress filtering where appropriate.
* Use tagged-only trunks when untagged traffic is unnecessary.
* Restrict each trunk to the VLANs required by its connected device.
* Keep VLAN configurations consistent at both ends of a trunk.
* Include the bridge CPU port when the router needs to participate in a VLAN.
* Use Safe Mode when making remote VLAN changes.
* Verify the configuration before enabling VLAN filtering.

# 🗁 Summary

Access and trunk ports control how VLAN traffic is transported through a MikroTik VLAN-aware bridge.

* **Access port:** Connects an endpoint to a specific VLAN, usually using untagged frames.
* **Trunk port:** Carries multiple VLANs, usually using tagged frames.
* **PVID:** Classifies untagged ingress frames into a VLAN.
* **Tagged membership:** Defines which ports transmit a VLAN with its tag.
* **Untagged membership:** Defines which ports transmit a VLAN without its tag.
* **Ingress filtering:** Rejects VLAN traffic that is not permitted on a port.
* **Frame types:** Controls which Ethernet frame formats a port accepts.

Correct access and trunk configuration provides predictable VLAN forwarding and helps maintain Layer 2 network separation.
