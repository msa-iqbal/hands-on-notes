# MikroTik RouterOS Interface Management

Interfaces are the connection points through which a MikroTik router communicates with other network devices.

An interface can represent a physical Ethernet port, wireless radio, VLAN, bridge, tunnel, virtual interface, or other logical networking component.

RouterOS uses interfaces to receive and transmit network traffic. They are also referenced by IP addressing, firewall rules, routing, DHCP, NAT, queues, and other networking services.

Understanding interface management is essential before configuring bridges, VLANs, routing, and network services.

# 🗁 Understanding RouterOS Interfaces

RouterOS supports different interface types depending on the hardware and installed packages.

Common interface types include:

| Interface Type | Description | Example |
| --- | --- | --- |
| Ethernet | Physical wired network connection | `ether1` |
| Wireless | Wireless network interface | `wlan1` |
| Wi-Fi | Newer wireless interface implementation | `wifi1` |
| Bridge | Logical Layer 2 interface connecting multiple ports | `bridge-LAN` |
| VLAN | Logical interface for a VLAN | `vlan10` |
| Bonding | Combines multiple physical interfaces | `bond1` |
| PPPoE | Point-to-Point Protocol over Ethernet interface | `pppoe-out1` |
| GRE | Generic Routing Encapsulation tunnel | `gre-tunnel1` |
| WireGuard | Secure VPN interface | `wireguard1` |
| IPIP | IP-in-IP tunnel interface | `ipip-tunnel1` |
| Loopback | Logical interface used for stable addressing and routing designs | `lo` |

Interface availability depends on the router model, hardware architecture, RouterOS version, and installed packages.

# 🗁 Viewing All Interfaces

To display all available interfaces:

```routeros
/interface print
```

Example output:

```text
Flags: R - RUNNING, X - DISABLED, I - INACTIVE
Columns: NAME, TYPE, ACTUAL-MTU, L2MTU, MAX-L2MTU
 #    NAME       TYPE       ACTUAL-MTU  L2MTU  MAX-L2MTU
 0 R  ether1     ether            1500   1598       9578
 1 R  ether2     ether            1500   1598       9578
 2    ether3     ether            1500   1598       9578
 3 R  bridge-LAN bridge           1500   65535
```

Important interface flags:

| Flag | Meaning |
| --- | --- |
| `R` | Running |
| `X` | Disabled |
| `I` | Inactive |
| `S` | Slave interface, where applicable |
| `D` | Dynamic interface |

Flags can vary depending on the interface type and RouterOS version.

An interface without the `R` flag is not necessarily disabled. It may simply have no active link or may depend on another interface or service.

# 🗁 Viewing Detailed Interface Information

To display detailed information about interfaces:

```routeros
/interface print detail
```

To inspect one interface:

```routeros
/interface ethernet print detail where name=ether1
```

For general interface information:

```routeros
/interface print detail where name=ether1
```

Detailed output can include:

* Interface name.
* Interface type.
* MTU.
* MAC address.
* Running status.
* Disabled status.
* Configuration properties.
* Interface-specific settings.

The available properties depend on the interface type.

# 🗁 Renaming Interfaces

RouterOS allows administrators to rename interfaces to make network configurations easier to understand.

For example, rename `ether1` to `WAN`:

```routeros
/interface set ether1 name=WAN
```

Rename `ether2` to `LAN`:

```routeros
/interface set ether2 name=LAN
```

Verify the changes:

```routeros
/interface print
```

A meaningful naming convention can make complex network configurations easier to maintain.

Example:

```text
WAN
LAN
LAN-SWITCH
MGMT
BACKHAUL
UPLINK
```

**Important:** Renaming an interface can affect configurations that reference its name, including scripts, firewall rules, routing settings, and monitoring systems. Review dependent configuration before making changes.

# 🗁 Adding Interface Comments

Comments help administrators document the purpose of individual interfaces.

Add a comment:

```routeros
/interface set ether1 comment="Primary ISP Uplink"
```

Add another comment:

```routeros
/interface set ether2 comment="Main LAN Connection"
```

Display interface comments:

```routeros
/interface print
```

Comments are particularly useful when managing routers with multiple WAN connections, VLANs, or physical uplinks.

# 🗁 Enabling and Disabling Interfaces

An interface can be enabled or disabled manually.

Enable an interface:

```routeros
/interface enable ether1
```

Disable an interface:

```routeros
/interface disable ether1
```

Disable multiple interfaces:

```routeros
/interface disable ether3,ether4
```

Enable multiple interfaces:

```routeros
/interface enable ether3,ether4
```

Verify interface status:

```routeros
/interface print
```

Disabling an interface can interrupt traffic and affect services that depend on it.

# 🗁 Understanding Interface Status

Interface status helps administrators determine whether a network connection is operational.

Check interface status:

```routeros
/interface print
```

For an Ethernet interface, inspect its link state:

```routeros
/interface ethernet monitor ether1
```

A running Ethernet interface generally indicates that a physical link is established.

A physical Ethernet interface may fail to become running because of:

* Disconnected network cable.
* Faulty cable or connector.
* Powered-off remote device.
* Incorrect speed or duplex negotiation.
* Faulty switch port.
* Hardware failure.

Virtual interfaces have different running-state conditions. For example, a VLAN interface depends on its underlying interface, while a PPPoE interface depends on successful session establishment.

# 🗁 Monitoring Interface Traffic

RouterOS provides tools for monitoring interface traffic.

Monitor a specific interface:

```routeros
/interface monitor-traffic ether1
```

Monitor multiple interfaces:

```routeros
/interface monitor-traffic ether1,ether2
```

Display interface statistics:

```routeros
/interface print stats
```

Traffic monitoring helps administrators observe:

* Receive traffic.
* Transmit traffic.
* Packet rates.
* Traffic spikes.
* Unexpected bandwidth usage.

For deeper traffic analysis, RouterOS also provides Torch and traffic-sniffing tools.

# 🗁 Understanding Interface MTU

MTU stands for Maximum Transmission Unit.

It represents the maximum Layer 3 packet size that an interface can transmit without requiring fragmentation at that layer.

The common Ethernet IP MTU is:

```text
1500 bytes
```

Display interface MTU information:

```routeros
/interface print
```

Set an interface MTU:

```routeros
/interface set ether1 mtu=1500
```

The MTU setting must be compatible with the network path and encapsulation requirements.

# 🗁 Understanding L2MTU

L2MTU represents the maximum Layer 2 frame size supported by an interface, excluding the Ethernet header and FCS according to the platform's reporting conventions.

It is particularly important when working with:

* VLAN tagging.
* MPLS.
* QinQ.
* Other Layer 2 encapsulations.

Display L2MTU information:

```routeros
/interface print
```

The maximum supported L2MTU varies between hardware models and interface types.

Do not assume that every MikroTik interface supports the same L2MTU.

# 🗁 Understanding MAC Addresses

Ethernet interfaces have MAC addresses used for Layer 2 communication.

Display interface MAC addresses:

```routeros
/interface print
```

Display detailed Ethernet information:

```routeros
/interface ethernet print detail
```

A MAC address is commonly represented as six hexadecimal octets:

```text
AA:BB:CC:DD:EE:FF
```

RouterOS can assign or configure MAC addresses on supported interface types.

For example:

```routeros
/interface ethernet set ether2 mac-address=02:11:22:33:44:55
```

Use manually assigned MAC addresses carefully. Duplicate MAC addresses on the same Layer 2 network can cause connectivity problems.

# 🗁 Finding Interfaces by Name

RouterOS supports the `find` command for selecting configuration objects.

Find an interface by name:

```routeros
/interface find name=ether1
```

Display information about the matching interface:

```routeros
/interface print where name=ether1
```

Find all disabled interfaces:

```routeros
/interface print where disabled=yes
```

Find running interfaces:

```routeros
/interface print where running=yes
```

These filtering techniques are useful when managing routers with many interfaces.

# 🗁 Interface Lists

Interface lists allow administrators to group interfaces logically.

Instead of referencing individual interfaces repeatedly, firewall rules and other RouterOS features can reference an interface list.

Common examples include:

* WAN
* LAN
* MANAGEMENT

Create an interface list:

```routeros
/interface list add name=WAN
/interface list add name=LAN
```

Add interfaces to the lists:

```routeros
/interface list member add list=WAN interface=ether1
/interface list member add list=LAN interface=ether2
```

Display interface lists:

```routeros
/interface list print
```

Display list members:

```routeros
/interface list member print
```

Example firewall rule using an interface list:

```routeros
/ip firewall filter add chain=input \
    in-interface-list=WAN \
    action=drop \
    comment="Drop unsolicited WAN input"
```

Interface lists simplify configuration management and reduce the need to repeat individual interface names.

**Important:** This example is only one firewall rule. It is not a complete firewall policy.

# 🗁 Interface Naming Strategy

A consistent naming convention improves operational clarity.

For a small network:

```text
WAN
LAN
MGMT
```

For an ISP or enterprise network:

```text
WAN-ISP-01
WAN-ISP-02
LAN-OFFICE
LAN-SERVER
MGMT-ADMIN
UPLINK-CORE
BACKHAUL-01
```

For logical interfaces:

```text
bridge-LAN
vlan10-MGMT
vlan20-OFFICE
vlan30-GUEST
wireguard-REMOTE
pppoe-ISP
```

A good naming convention should be:

* Descriptive.
* Consistent.
* Easy to understand.
* Unique within the router.
* Suitable for scripts and monitoring.

# 🗁 Practical Lab: Interface Management

This lab demonstrates basic interface management using MikroTik CHR or compatible RouterBOARD hardware.

**Lab requirements:**

* MikroTik CHR or a physical MikroTik router.
* WinBox or SSH access.
* At least two available Ethernet interfaces on physical hardware, or multiple virtual interfaces in CHR.

**Inspect available interfaces**

```routeros
/interface print
```

**Rename interfaces**

```routeros
/interface set ether1 name=WAN
/interface set ether2 name=LAN
```

**Add interface comments**

```routeros
/interface set WAN comment="Primary Internet Connection"
/interface set LAN comment="Local Network"
```

**Create interface lists**

```routeros
/interface list add name=WAN-LIST
/interface list add name=LAN-LIST
```

**Add interfaces to lists**

```routeros
/interface list member add list=WAN-LIST interface=WAN
/interface list member add list=LAN-LIST interface=LAN
```

**Inspect interface status**

```routeros
/interface print detail
/interface list print
/interface list member print
```

**Monitor interface traffic**

```routeros
/interface monitor-traffic WAN
```

**Lab objectives:**

* Identify available RouterOS interfaces.
* Understand interface types and status flags.
* Rename interfaces.
* Add interface comments.
* Create interface lists.
* Assign interfaces to lists.
* Monitor interface traffic.

# 🗁 Troubleshooting Interface Problems

**Interface is not running**

Check:

```routeros
/interface print
/interface ethernet monitor ether1
```

Verify the cable, remote device, physical port, and link negotiation.

**Interface is disabled**

Inspect the interface:

```routeros
/interface print
```

Enable it if appropriate:

```routeros
/interface enable ether1
```

**Interface traffic is not passing**

Check:

```routeros
/interface print stats
/ip address print
/ip route print
```

Also inspect bridge membership, VLAN configuration, firewall rules, and routing.

**Interface rename causes configuration problems**

Review scripts and configuration entries that reference the previous interface name.

Inspect relevant firewall, routing, bridge, and monitoring configurations.

**Unexpected interface errors**

Check interface statistics, cable condition, negotiated speed, duplex, and remote switch port status.

# 🗁 Interface Management Best Practices

* Use meaningful interface names.
* Document the purpose of physical and logical interfaces.
* Use interface lists for logical grouping.
* Monitor interface status and traffic.
* Verify MTU and L2MTU requirements.
* Avoid unnecessary MAC address changes.
* Review dependent configurations before renaming interfaces.
* Disable unused physical interfaces when appropriate.
* Monitor link errors and unexpected disconnections.
* Maintain consistent naming conventions across network devices.
* Document important uplinks and management interfaces.

# 🗁 Official Documentation

* [MikroTik RouterOS Interfaces](https://help.mikrotik.com/docs/)
* [MikroTik RouterOS Interface Lists](https://help.mikrotik.com/docs/)
* [MikroTik RouterOS Ethernet](https://help.mikrotik.com/docs/)
