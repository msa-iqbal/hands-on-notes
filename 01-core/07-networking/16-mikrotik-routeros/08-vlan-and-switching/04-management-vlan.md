# Management VLAN

A Management VLAN is a dedicated VLAN used to access and administer network infrastructure devices, such as routers, switches, wireless access points, and other network equipment.

Instead of allowing management access from every network, a Management VLAN separates administrative traffic from regular user traffic.

In MikroTik RouterOS, a Management VLAN can be configured using a VLAN-aware bridge, a VLAN interface, and firewall rules that restrict access to trusted management devices.

# 🗁 Why Use a Management VLAN?

A dedicated Management VLAN provides several benefits:

* **Network separation:** Keeps management traffic separate from user traffic.
* **Access control:** Restricts administrative access to authorized devices.
* **Reduced exposure:** Prevents ordinary users from directly accessing infrastructure management services.
* **Centralized administration:** Provides a dedicated network for managing multiple devices.
* **Improved troubleshooting:** Makes management traffic easier to identify and monitor.

A Management VLAN is a network segmentation mechanism, not a replacement for authentication, encryption, or firewall security.

# 🗁 Management VLAN Topology

Consider a network with three VLANs:

| VLAN | Purpose | Network | Gateway |
| --- | --- | --- | --- |
| VLAN 10 | Administration | `192.168.10.0/24` | `192.168.10.1` |
| VLAN 20 | Development | `192.168.20.0/24` | `192.168.20.1` |
| VLAN 99 | Network Management | `192.168.99.0/24` | `192.168.99.1` |

The Management VLAN is used by authorized administrators to access network infrastructure.

Example management devices:

| Device | IP Address |
| --- | --- |
| MikroTik Router | `192.168.99.1` |
| Managed Switch | `192.168.99.2` |
| Wireless Access Point | `192.168.99.3` |
| Administrator PC | `192.168.99.10` |

```text
                    Internet
                       |
                +--------------+
                | MikroTik     |
                | Router       |
                +------+-------+
                       |
                VLAN Trunk Link
                       |
                +------+-------+
                | Managed      |
                | Switch       |
                +------+-------+
                       |
          +------------+-------------+
          |            |             |
       VLAN 10      VLAN 20       VLAN 99
       Admin        Development   Management
          |            |             |
       User PCs     User PCs     Admin PC
                                  |
                           Router / Switch /
                           Access Point
                           Management
```

The switch and access point management interfaces should be reachable through VLAN 99 only, according to the intended management policy.

# 🗁 Choose a Management VLAN ID

You can use a dedicated VLAN ID such as:

```text
VLAN ID: 99
VLAN Name: MANAGEMENT
Network: 192.168.99.0/24
Gateway: 192.168.99.1
```

VLAN 99 is only an example. You can select another VLAN ID based on your network design.

Avoid assuming that a particular VLAN ID is inherently secure. Security depends on correct VLAN membership, firewall policies, and management-service restrictions.

# 🗁 Configure the VLAN-Aware Bridge

Create a VLAN-aware bridge if one does not already exist.

```routeros
/interface bridge
add name=br0 vlan-filtering=no
```

Add the trunk interface connected to the managed switch.

```routeros
/interface bridge port
add bridge=br0 interface=ether1
```

Here, `ether1` is the trunk interface.

Configure the bridge VLAN table:

```routeros
/interface bridge vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1
add bridge=br0 vlan-ids=20 tagged=br0,ether1
add bridge=br0 vlan-ids=99 tagged=br0,ether1
```

The bridge CPU port (`br0`) must be a tagged member of VLAN 99 because RouterOS needs to receive management traffic through its VLAN interface.

If the other VLANs also require Layer 3 routing on the router, their CPU-port membership must be configured as shown.

# 🗁 Create the Management VLAN Interface

Create a VLAN interface on the bridge.

```routeros
/interface vlan
add name=vlan99-management interface=br0 vlan-id=99
```

This interface provides Layer 3 connectivity to the Management VLAN.

# 🗁 Assign the Management Gateway Address

Assign an IP address to the Management VLAN interface.

```routeros
/ip address
add address=192.168.99.1/24 interface=vlan99-management
```

The router's Management VLAN gateway is now:

```text
192.168.99.1
```

Check the interface and IP address:

```routeros
/interface vlan print
/ip address print
```

# 🗁 Configure the Management VLAN on the Switch

The switch port connected to MikroTik should carry VLAN 99 as tagged traffic.

Example switch port assignment:

| Switch Port | Mode | VLAN Membership |
| --- | --- | --- |
| Port 1 | Trunk | Tagged VLAN 10, 20, 99 |
| Port 2 | Access | Untagged VLAN 10 |
| Port 3 | Access | Untagged VLAN 20 |
| Port 4 | Access | Untagged VLAN 99 |

Port 4 can be used by an administrator's workstation.

Configure the switch's own management interface to use VLAN 99 with an address such as:

```text
IP Address:      192.168.99.2
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.99.1
```

The exact configuration depends on the switch vendor.

# 🗁 Configure the Administrator Workstation

Connect the administrator workstation to an access port assigned to VLAN 99.

Configure its network settings:

```text
IP Address:      192.168.99.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.99.1
```

Test connectivity to the router:

```bash
ping 192.168.99.1
```

Test connectivity to the managed switch:

```bash
ping 192.168.99.2
```

If the network is correctly configured, the administrator workstation should be able to reach the management interfaces.

# 🗁 Restrict Router Management Access

RouterOS provides several management services, including:

* WinBox
* SSH
* WebFig
* API
* API-SSL

You should enable only the services required for administration and restrict access to trusted management addresses.

Inspect the current service configuration:

```routeros
/ip service print
```

For example, disable services that are not required:

```routeros
/ip service
disable telnet
disable ftp
disable www
disable api
disable api-ssl
```

The commands above are examples. Keep any service that your management workflow requires.

Restrict WinBox and SSH to the administrator workstation:

```routeros
/ip service
set winbox address=192.168.99.10/32
set ssh address=192.168.99.10/32
```

This limits those services to the specified source address.

If multiple administrator workstations need access, use an appropriate trusted management subnet or address list.

# 🗁 Restrict Access with the Input Firewall Chain

The `input` chain controls traffic destined for the MikroTik router itself.

You can use it to restrict management access to VLAN 99.

The following is a focused example of management-access rules:

```routeros
/ip firewall filter
add chain=input action=accept connection-state=established,related comment="Allow established and related"
add chain=input action=drop connection-state=invalid comment="Drop invalid connections"
add chain=input action=accept in-interface=vlan99-management src-address=192.168.99.10 protocol=tcp dst-port=22,8291 comment="Allow SSH and WinBox from administrator"
```

The final rule permits SSH and WinBox connections from the administrator workstation through the Management VLAN.

Place these rules before any conflicting rules that would accept or drop the same traffic.

This is not a complete input firewall policy. Preserve the rules required for DHCP, DNS, ICMP, VPN access, and other legitimate router services. Do not add a blanket input drop without accounting for those requirements and your existing firewall configuration.

# 🗁 Restrict Access from Other VLANs

A Management VLAN should not be accessible to ordinary user networks unless explicitly required.

For example, block new connections from VLAN 10 and VLAN 20 to the Management VLAN:

```routeros
/ip firewall filter
add chain=forward action=drop in-interface=vlan10-admin out-interface=vlan99-management connection-state=new comment="Block new connections from VLAN 10 to management"
add chain=forward action=drop in-interface=vlan20-dev out-interface=vlan99-management connection-state=new comment="Block new connections from VLAN 20 to management"
```

These are forward-chain rules because the traffic is routed through the MikroTik router rather than destined for the router itself.

Place them before broader forward-chain accept rules that would otherwise permit the traffic.

If you need specific exceptions, add narrowly scoped allow rules before the drops.

# 🗁 Enable VLAN Filtering Safely

After confirming the bridge ports, VLAN membership, trunk configuration, and management connectivity, enable VLAN filtering:

```routeros
/interface bridge
set br0 vlan-filtering=yes
```

**Important:** Incorrect bridge VLAN filtering configuration can disconnect your management session.

Before enabling it on a remote router:

* Confirm that the bridge CPU port is tagged in VLAN 99.
* Confirm that the trunk carries VLAN 99.
* Confirm that the switch management interface uses the correct VLAN.
* Verify the administrator workstation's VLAN assignment.
* Use RouterOS Safe Mode when making remote configuration changes.
* Keep an alternative recovery method available.

Do not enable VLAN filtering until the complete VLAN membership configuration is ready.

# 🗁 Verify Management VLAN Configuration

Check the bridge VLAN table:

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

Check management services:

```routeros
/ip service print
```

Check firewall rules:

```routeros
/ip firewall filter print
```

Test connectivity from the administrator workstation:

```bash
ping 192.168.99.1
ping 192.168.99.2
```

Then test the required management services:

```bash
ssh admin@192.168.99.1
```

You can also connect using WinBox through the router's Management VLAN IP address.

# 🗁 Common Management VLAN Problems

**Cannot access the router after enabling VLAN filtering**

Possible causes:

* Bridge CPU port is missing from VLAN 99 tagged membership.
* VLAN 99 is not allowed on the trunk.
* Administrator workstation is connected to the wrong access VLAN.
* Management VLAN interface is attached to the wrong parent interface.
* Incorrect bridge VLAN table configuration.

**Can ping the router but cannot connect through WinBox or SSH**

Possible causes:

* Management services are disabled.
* The administrator IP address is not included in the service's allowed-address configuration.
* Input firewall rules block the management connection.
* The management service is listening on a different port.

**Switch is reachable but access points are not**

Possible causes:

* Access point management VLAN is configured incorrectly.
* The switch port connected to the access point does not carry VLAN 99 correctly.
* The access point uses an incorrect management IP address or gateway.
* Management access is restricted by the access point's own firewall or service settings.

**Management access works from one device but not another**

Possible causes:

* The second workstation is outside the permitted source subnet.
* RouterOS service restrictions allow only specific source addresses.
* Firewall rules permit only selected administrator devices.
* The workstation has an incorrect IP configuration.

# 🗁 Management VLAN Best Practices

* Use a dedicated VLAN for infrastructure management.
* Permit management access only from trusted administrator devices or networks.
* Disable unused management services.
* Prefer SSH and WinBox over insecure management protocols.
* Use strong credentials and separate administrator accounts.
* Restrict management services with both service-level address restrictions and firewall rules.
* Avoid exposing management interfaces directly to the public Internet.
* Keep management VLAN access consistent across switches and access points.
* Use Safe Mode when changing VLAN configuration remotely.
* Document management IP addresses and device assignments.
* Maintain an alternative recovery method for critical infrastructure.

# 🗁 Summary

A Management VLAN separates infrastructure administration from ordinary user traffic.

In MikroTik RouterOS, the configuration involves:

* Creating a dedicated VLAN on the VLAN-aware bridge.
* Including the bridge CPU port as a tagged member.
* Creating a VLAN interface for management.
* Assigning a management gateway IP address.
* Configuring the switch and administrator workstation.
* Restricting management services to trusted sources.
* Applying input and forward firewall policies.

A properly configured Management VLAN provides a more controlled and manageable infrastructure network.
