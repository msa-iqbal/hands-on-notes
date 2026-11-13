# Inter-VLAN Routing

Inter-VLAN routing allows devices in different VLANs to communicate through a Layer 3 device, such as a MikroTik router.

VLANs separate a network into isolated Layer 2 broadcast domains. Devices in different VLANs cannot communicate directly at Layer 2. They need a router or Layer 3 switch to forward traffic between their IP networks.

In MikroTik RouterOS, inter-VLAN routing is commonly implemented using VLAN interfaces attached to a VLAN-aware bridge.

# 🗁 Understanding Inter-VLAN Routing

Consider a network with two VLANs:

| VLAN | Purpose | Network | Gateway |
| --- | --- | --- | --- |
| VLAN 10 | Administration | `192.168.10.0/24` | `192.168.10.1` |
| VLAN 20 | Development | `192.168.20.0/24` | `192.168.20.1` |

Each VLAN has its own IP subnet and default gateway.

For example:

* A device in VLAN 10 uses `192.168.10.1` as its default gateway.
* A device in VLAN 20 uses `192.168.20.1` as its default gateway.
* When a device communicates with another device in a different subnet, it sends the traffic to its default gateway.
* The MikroTik router routes the packet between the VLAN interfaces.

# 🗁 Layer 2 Switching vs Layer 3 Routing

**Layer 2 switching:**

* Forwards Ethernet frames within the same VLAN.
* Uses MAC addresses to make forwarding decisions.
* Keeps broadcast traffic within its VLAN.

**Layer 3 routing:**

* Forwards IP packets between different networks.
* Uses destination IP addresses and the routing table.
* Allows communication between VLANs when routing and firewall policies permit it.

A VLAN-aware bridge handles Layer 2 forwarding, while VLAN interfaces on the MikroTik router provide Layer 3 gateways.

# 🗁 Inter-VLAN Routing Topology

The following topology uses a MikroTik router connected to a managed switch through one trunk link.

```text
                    Internet
                       |
                       |
                +--------------+
                | MikroTik     |
                | Router       |
                |              |
                | VLAN 10      |
                | 192.168.10.1 |
                |              |
                | VLAN 20      |
                | 192.168.20.1 |
                +------+-------+
                       |
                 Trunk: VLAN 10, 20
                       |
                +------+-------+
                | Managed      |
                | Switch       |
                +------+-------+
                       |
              +--------+--------+
              |                 |
         Access VLAN 10    Access VLAN 20
              |                 |
         +----+-----+      +----+-----+
         | Admin PC |      | Dev PC   |
         |          |      |          |
         | .10.10   |      | .20.10   |
         +----------+      +----------+
```

The router receives tagged traffic from the switch and routes packets between the VLAN subnets.

# 🗁 Router Configuration Requirements

Before configuring inter-VLAN routing, ensure that:

* A VLAN-aware bridge exists.
* VLAN filtering is enabled on the bridge.
* The trunk port carries the required VLANs.
* The bridge CPU port is a tagged member of the VLANs that need Layer 3 routing.
* VLAN interfaces are created on the bridge.
* Each VLAN interface has an IP address from its corresponding subnet.

The bridge CPU port is important because the router itself must receive traffic belonging to those VLANs.

# 🗁 Configure the VLAN-Aware Bridge

Create a bridge and enable VLAN filtering.

```routeros
/interface bridge
add name=br0 vlan-filtering=yes
```

Add the physical trunk interface.

```routeros
/interface bridge port
add bridge=br0 interface=ether1
```

In this example, `ether1` connects to the managed switch.

Configure VLAN membership:

```routeros
/interface bridge vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1
add bridge=br0 vlan-ids=20 tagged=br0,ether1
```

Both VLANs are tagged on the trunk interface and the bridge CPU port.

**Important:** This example assumes that the switch is configured to carry tagged VLAN 10 and VLAN 20 on its corresponding trunk port.

# 🗁 Create VLAN Interfaces

Create VLAN interfaces on the bridge.

```routeros
/interface vlan
add name=vlan10-admin interface=br0 vlan-id=10
add name=vlan20-dev interface=br0 vlan-id=20
```

The VLAN interfaces provide Layer 3 access to their respective VLANs.

The parent interface should be the VLAN-aware bridge, not the physical trunk interface, when using this bridge VLAN filtering design.

# 🗁 Assign IP Addresses to VLAN Interfaces

Assign the gateway IP addresses.

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10-admin
add address=192.168.20.1/24 interface=vlan20-dev
```

The resulting gateway addresses are:

| VLAN Interface | IP Address |
| --- | --- |
| `vlan10-admin` | `192.168.10.1/24` |
| `vlan20-dev` | `192.168.20.1/24` |

Do not assign these gateway addresses directly to the physical trunk interface or the bridge itself. They belong on their respective VLAN interfaces.

# 🗁 Verify Connected Routes

RouterOS automatically installs connected routes when valid IP addresses are assigned to active interfaces.

Check the routing table:

```routeros
/ip route print
```

You should see connected routes similar to:

```text
DAC 192.168.10.0/24  vlan10-admin
DAC 192.168.20.0/24  vlan20-dev
```

The exact flags and displayed information may vary by RouterOS version and interface state.

These connected routes allow the router to forward traffic between the two directly connected VLAN networks.

# 🗁 Configure the Managed Switch

The switch port connected to MikroTik must operate as a trunk carrying VLAN 10 and VLAN 20.

The endpoint ports should be configured as access ports:

| Switch Port | Mode | VLAN |
| --- | --- | --- |
| Port 1 | Trunk | VLAN 10, VLAN 20 |
| Port 2 | Access | VLAN 10 |
| Port 3 | Access | VLAN 20 |

The exact commands depend on the switch vendor and operating system.

Make sure the switch does not remove VLAN tags from the trunk traffic.

# 🗁 Configure End Devices

Assign IP addresses and default gateways to the devices.

**Administration PC:**

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

**Development PC:**

```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

The devices can use manually configured addresses or obtain them through DHCP.

# 🗁 Test Inter-VLAN Connectivity

From the Administration PC, test the router gateway:

```bash
ping 192.168.10.1
```

Then test the Development PC:

```bash
ping 192.168.20.10
```

From the Development PC, test the Administration PC:

```bash
ping 192.168.10.10
```

If routing and firewall policies permit the traffic, the devices should be able to communicate across VLANs.

A successful ping to the gateway confirms connectivity to the router, but does not by itself prove that inter-VLAN forwarding is permitted.

# 🗁 Control Inter-VLAN Traffic with Firewall Rules

By default, a router can route packets between its connected networks. Firewall filter rules can control which VLANs are allowed to communicate.

For example, you may want to:

* Allow Administration devices to access Development devices.
* Prevent Development devices from initiating connections to Administration devices.
* Allow established and related connections to return.
* Permit only specific services between VLANs.

The following example blocks new connections initiated from VLAN 20 toward VLAN 10 while allowing established and related traffic.

```routeros
/ip firewall filter
add chain=forward action=accept connection-state=established,related comment="Allow established and related"
add chain=forward action=drop connection-state=invalid comment="Drop invalid connections"
add chain=forward action=drop in-interface=vlan20-dev out-interface=vlan10-admin connection-state=new comment="Block new connections from VLAN 20 to VLAN 10"
```

**Important:** Firewall rules are evaluated in order. Place these rules appropriately within your existing forward-chain policy. Existing broad accept rules placed before the drop rule can prevent the intended restriction from taking effect.

This example is a focused policy fragment, not a complete firewall configuration.

# 🗁 Allow Only Specific Services Between VLANs

You can also allow selected services instead of permitting unrestricted communication.

For example, allow VLAN 20 devices to access an application server in VLAN 10 over HTTPS:

```routeros
/ip firewall filter
add chain=forward action=accept \
    in-interface=vlan20-dev \
    out-interface=vlan10-admin \
    dst-address=192.168.10.50 \
    protocol=tcp \
    dst-port=443 \
    connection-state=new \
    comment="Allow HTTPS to application server"
```

Place this rule before any rule that blocks the same traffic.

A complete restrictive policy should also account for established connections, other permitted services, and the default handling of unmatched traffic.

# 🗁 Verify VLAN Configuration

Check bridge VLAN membership:

```routeros
/interface bridge vlan print
```

Check bridge ports:

```routeros
/interface bridge port print
```

Check VLAN interfaces:

```routeros
/interface vlan print
```

Check IP addresses:

```routeros
/ip address print
```

Check routing:

```routeros
/ip route print
```

Check firewall rules:

```routeros
/ip firewall filter print
```

These commands help identify configuration problems across Layer 2, Layer 3, and firewall processing.

# 🗁 Common Inter-VLAN Routing Problems

**VLAN interface is not receiving traffic**

Possible causes:

* The bridge CPU port is missing from the tagged VLAN membership.
* The trunk does not carry the required VLAN.
* VLAN filtering is configured incorrectly.
* The VLAN interface uses the wrong parent interface.

**Devices can reach their gateway but cannot reach another VLAN**

Possible causes:

* A forward-chain firewall rule blocks the traffic.
* The destination device has an incorrect default gateway.
* The destination host firewall blocks incoming connections.
* The router does not have an active connected route for the destination subnet.

**One VLAN works but another does not**

Possible causes:

* Incorrect VLAN ID.
* Missing bridge VLAN table entry.
* Incorrect switch trunk configuration.
* Incorrect access-port VLAN assignment.
* Incorrect PVID on an access port.

**Router can ping devices, but devices cannot communicate with each other**

Possible causes:

* Firewall policies restrict forwarded traffic.
* Host firewalls block incoming requests.
* The devices use incorrect subnet masks or gateways.

**Intermittent connectivity**

Possible causes:

* Duplicate IP addresses.
* Inconsistent VLAN membership across switches.
* Incorrect trunk configuration.
* Physical link or switch configuration problems.

# 🗁 Best Practices

* Use a separate IP subnet for each VLAN.
* Assign gateway IP addresses to VLAN interfaces.
* Create VLAN interfaces on the VLAN-aware bridge when using bridge VLAN filtering.
* Include the bridge CPU port as a tagged member of VLANs requiring router Layer 3 access.
* Configure trunk ports to carry only the VLANs they need.
* Use explicit firewall policies to control inter-VLAN communication.
* Keep management VLAN access restricted to trusted devices.
* Document VLAN IDs, subnets, gateway addresses, and switch port assignments.
* Test connectivity after each configuration change.
* Back up the router configuration before making major VLAN changes.

# 🗁 Summary

Inter-VLAN routing enables communication between separate VLAN networks through a Layer 3 router.

In MikroTik RouterOS, the common bridge VLAN filtering design uses:

* A VLAN-aware bridge for Layer 2 switching.
* Tagged trunk ports for transporting multiple VLANs.
* VLAN interfaces on the bridge for Layer 3 connectivity.
* IP addresses on VLAN interfaces as default gateways.
* Connected routes for forwarding between VLAN subnets.
* Firewall filter rules to control communication between VLANs.

Correct coordination between bridge VLAN membership, switch configuration, VLAN interfaces, IP addressing, and firewall policies is essential for reliable inter-VLAN routing.
