# VLAN Troubleshooting

VLAN troubleshooting is the process of identifying and resolving problems that prevent devices in a Virtual Local Area Network from communicating correctly.

A VLAN logically separates a physical Layer 2 network into multiple broadcast domains. Switch ports, VLAN membership, trunk links, tagging, and Layer 3 routing must be configured consistently for communication to work.

Common VLAN-related symptoms include:

* Devices in the same VLAN cannot communicate.
* Devices in different VLANs cannot communicate.
* A VLAN works on one switch but not another.
* A device receives an IP address from the wrong subnet.
* DHCP works on one VLAN but fails on another.
* A trunk link carries some VLANs but not others.
* Inter-VLAN routing fails.
* VLAN traffic is unexpectedly untagged or dropped.
* A native VLAN mismatch causes connectivity problems.

# 🗁 VLAN Fundamentals

A VLAN divides a switched network into separate logical broadcast domains.

Example network:

```text
                 Router / Layer 3 Switch
                         |
                       Trunk
                         |
                    Main Switch
                    /         \
                   /           \
             VLAN 10           VLAN 20
             Finance           Engineering
             /    \             /     \
           PC1    PC2         PC3     PC4
```

Example VLAN assignments:

| VLAN ID | Name | Subnet |
| --- | --- | --- |
| 10 | Finance | 192.168.10.0/24 |
| 20 | Engineering | 192.168.20.0/24 |
| 30 | Management | 192.168.30.0/24 |

Devices in the same VLAN can communicate at Layer 2 when their switching paths are correctly configured.

Communication between different VLANs requires Layer 3 routing.

# 🗁 Access Ports

An access port normally belongs to one VLAN and connects to an endpoint such as:

* Desktop computers.
* Printers.
* IP phones.
* Servers.
* Wireless access points using an untagged management or client network.

Example:

```text
Switch Port: ether2
VLAN: 10
Mode: Access
Connected Device: Finance PC
```

If an access port is assigned to the wrong VLAN, the connected device may enter the wrong broadcast domain.

Common access-port problems:

* Incorrect VLAN assignment.
* Port configured as a trunk instead of access.
* VLAN does not exist on the switch.
* Port is administratively disabled.
* Incorrect PVID on a VLAN-aware bridge.
* Inconsistent configuration across switches.

# 🗁 Trunk Ports

A trunk carries traffic for multiple VLANs between network devices.

IEEE 802.1Q adds a VLAN tag to Ethernet frames so receiving devices can identify the VLAN.

Example:

```text
Switch A
   |
   | Trunk: VLAN 10, 20, 30
   |
Switch B
```

Common trunk problems:

* VLAN missing from the allowed VLAN list.
* VLAN not created or active on the switch.
* Incorrect trunk configuration.
* Native VLAN mismatch.
* Incorrect tagging behavior.
* Physical link failure.
* VLAN filtering or bridge configuration error.

# 🗁 IEEE 802.1Q Tagging

802.1Q tagging identifies the VLAN associated with an Ethernet frame.

The VLAN tag contains a 12-bit VLAN identifier, allowing VLAN IDs from 0 to 4095. VLAN IDs 0 and 4095 have reserved meanings, so the commonly usable VLAN ID range is 1–4094.

A tagged frame contains VLAN identification information, while an untagged frame does not carry an 802.1Q VLAN tag.

When troubleshooting, verify whether each link expects tagged or untagged traffic.

# 🗁 Common VLAN Problems

| Symptom | Possible cause |
| --- | --- |
| Devices in the same VLAN cannot communicate | Wrong VLAN assignment or Layer 2 problem |
| Devices receive addresses from another subnet | Incorrect VLAN membership or DHCP scope |
| VLAN works locally but not across switches | VLAN missing from trunk |
| Some VLANs work and others fail | Allowed VLAN list or filtering issue |
| Inter-VLAN communication fails | Missing Layer 3 interface or route |
| DHCP fails on one VLAN | DHCP relay, VLAN tagging, or scope issue |
| Network works until a switch is replaced | Configuration mismatch |
| Native VLAN traffic fails | Native VLAN or untagged-frame mismatch |
| MikroTik VLAN interface does not work | Bridge VLAN table or PVID configuration issue |

# 🗁 Troubleshooting Methodology

Use a systematic approach to isolate the failure.

**Identify the affected VLAN**

Determine:

* VLAN ID.
* VLAN name.
* IP subnet.
* Default gateway.
* Connected switch.
* Access port.
* Trunk path.
* DHCP server or relay.
* Expected communication destination.

**Verify physical connectivity**

Check the physical link and interface status.

Linux:

```bash
ip link show
```

Cisco IOS:

```cisco
show interfaces status
```

MikroTik RouterOS v7:

```routeros
/interface print detail
```

**Verify VLAN membership**

Check whether the endpoint's switch port belongs to the expected VLAN.

**Verify trunk configuration**

Check whether every intermediate trunk carries the required VLAN.

**Verify tagging**

Determine whether frames should be tagged or untagged on each link.

**Verify Layer 3 configuration**

Check the VLAN interface, gateway address, routing table, and firewall policies.

**Verify DHCP**

Check whether the device receives an address from the correct DHCP scope.

# 🗁 Cisco IOS VLAN Diagnostics

Cisco switches provide commands for inspecting VLANs, access ports, and trunks.

**Display VLAN information**

```cisco
show vlan brief
```

**Display trunk interfaces**

```cisco
show interfaces trunk
```

**Inspect a specific interface**

```cisco
show interfaces gigabitEthernet 0/1 switchport
```

**Display interface status**

```cisco
show interfaces status
```

**Display interface configuration**

```cisco
show running-config interface gigabitEthernet 0/1
```

**Display spanning-tree information**

```cisco
show spanning-tree vlan 10
```

**Display MAC address table**

```cisco
show mac address-table dynamic
```

Check:

* Does the VLAN exist?
* Is the endpoint port assigned to the correct access VLAN?
* Is the trunk operational?
* Is the required VLAN allowed and active?
* Are frames being learned in the expected VLAN?
* Is spanning tree blocking the expected path?

# 🗁 Cisco Access Port Troubleshooting

Example access-port configuration:

```cisco
interface gigabitEthernet 0/2
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
```

Verify the port:

```cisco
show interfaces gigabitEthernet 0/2 switchport
show vlan brief
```

If the port is assigned to VLAN 20 instead of VLAN 10, the connected device will become part of the wrong broadcast domain.

Correct the VLAN assignment only after confirming the intended network design.

# 🗁 Cisco Trunk Troubleshooting

Example trunk configuration:

```cisco
interface gigabitEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

Verify:

```cisco
show interfaces trunk
show interfaces gigabitEthernet 0/1 switchport
```

Look for:

* Operational trunk status.
* Correct encapsulation where applicable.
* Expected native VLAN.
* Required VLANs in the allowed list.
* VLANs that are active and forwarding.

A VLAN configured on one switch may still fail across the network if it is not permitted on an intermediate trunk.

# 🗁 MikroTik RouterOS v7 VLAN Diagnostics

MikroTik VLAN configuration commonly uses VLAN-aware bridges.

Useful commands:

**Display bridge configuration**

```routeros
/interface bridge print detail
```

**Display bridge ports**

```routeros
/interface bridge port print detail
```

**Display VLAN table**

```routeros
/interface bridge vlan print detail
```

**Display VLAN interfaces**

```routeros
/interface vlan print detail
```

**Display interface status**

```routeros
/interface print detail
```

**Display bridge host table**

```routeros
/interface bridge host print
```

Inspect:

* Bridge VLAN filtering.
* Port PVID values.
* Tagged and untagged port membership.
* VLAN interface parent bridge.
* VLAN IDs.
* Bridge port settings.
* Hardware offloading status where relevant.

# 🗁 MikroTik PVID Problems

PVID (Port VLAN ID) determines the VLAN assigned to untagged ingress traffic on a VLAN-aware bridge port.

Example:

```routeros
/interface bridge port
set [find interface=ether2] pvid=10
```

If an untagged endpoint is connected to `ether2`, its ingress traffic is classified into VLAN 10.

Common problems:

* Incorrect PVID.
* Expected VLAN missing from bridge VLAN table.
* Port incorrectly configured as tagged.
* Port incorrectly configured as untagged.
* Inconsistent bridge VLAN filtering.
* Incorrect management VLAN membership.

A PVID setting alone does not guarantee correct VLAN forwarding. The bridge VLAN table must also define the intended tagged and untagged membership.

# 🗁 MikroTik Bridge VLAN Table Problems

Example VLAN table:

```routeros
/interface bridge vlan
add bridge=bridge1 vlan-ids=10 tagged=bridge1,ether1 untagged=ether2
```

In this example:

* `bridge1` is tagged so the router's VLAN interface can participate.
* `ether1` is a tagged trunk.
* `ether2` is an untagged access port.

Inspect the result:

```routeros
/interface bridge vlan print detail
/interface bridge port print detail
```

Common mistakes:

* Omitting the bridge interface from tagged membership when the router itself must access the VLAN through a VLAN interface.
* Forgetting to include a trunk port in the tagged list.
* Adding an access port to tagged membership instead of untagged membership.
* Configuring an incorrect PVID.
* Enabling VLAN filtering before the required VLAN configuration is ready.

When enabling bridge VLAN filtering remotely, plan management access carefully. An incorrect configuration can disconnect the management session.

# 🗁 Inter-VLAN Routing Problems

Inter-VLAN routing allows devices in different VLANs to communicate through a Layer 3 device.

Example:

```text
VLAN 10: 192.168.10.0/24
Gateway: 192.168.10.1

VLAN 20: 192.168.20.0/24
Gateway: 192.168.20.1
```

Common inter-VLAN routing problems:

* Missing VLAN interface.
* Incorrect gateway IP address.
* Missing route.
* VLAN not carried to the Layer 3 device.
* Firewall blocks forwarding.
* IP subnet overlap.
* Incorrect routing policy.

Verify:

* Both VLAN interfaces are operational.
* Each interface has the correct IP address.
* The router has connected routes for both networks.
* Firewall policies permit the intended traffic.
* Return traffic has a valid path.

A router having both VLAN interfaces does not automatically mean its firewall policy permits communication between them.

# 🗁 Router-on-a-Stick Troubleshooting

Router-on-a-stick uses one physical router interface with multiple VLAN subinterfaces.

Example:

```text
             Router
          eth0.10  eth0.20
              |
            Trunk
              |
            Switch
           /      \
       VLAN 10   VLAN 20
```

Common problems:

* Router subinterface VLAN ID mismatch.
* Switch trunk not operational.
* VLAN missing from allowed list.
* Incorrect native VLAN.
* Incorrect gateway address.
* Subinterface or physical interface down.

On Linux, VLAN interfaces can be inspected using:

```bash
ip -d link show
ip address show
```

On Cisco routers:

```cisco
show ip interface brief
show running-config
```

Verify that each subinterface uses the intended VLAN encapsulation and IP addressing.

# 🗁 DHCP Problems in VLANs

DHCP requests normally use broadcast communication within the local VLAN.

A DHCP server outside the VLAN generally requires a DHCP relay to forward client requests.

Common issues:

* DHCP server not connected to the VLAN.
* Missing DHCP relay configuration.
* Incorrect relay destination.
* Incorrect DHCP scope.
* VLAN tagging problem.
* Firewall blocks DHCP traffic.
* DHCP server has no matching subnet scope.

DHCP commonly uses:

* UDP 67 for server-side communication.
* UDP 68 for client-side communication.

Verify that the DHCP relay is configured on the appropriate Layer 3 interface and that the DHCP server has a matching scope.

# 🗁 VLAN and Spanning Tree Problems

Spanning Tree Protocol (STP) prevents Layer 2 switching loops.

STP may place a port into a blocking or discarding state to prevent a loop.

Common symptoms:

* VLAN traffic works through one path but not another.
* A trunk is operational but not forwarding for a particular VLAN.
* Network connectivity changes after a topology change.
* Unexpected redundant links are blocked.

Cisco diagnostics:

```cisco
show spanning-tree
show spanning-tree vlan 10
show spanning-tree interface gigabitEthernet 0/1 detail
```

MikroTik diagnostics:

```routeros
/interface bridge monitor bridge1
/interface bridge port print detail
```

Check the spanning-tree topology, root bridge, port roles, and port states.

Do not disable spanning tree merely to make a blocked port forward. First determine why the topology is producing that state.

# 🗁 VLAN and MTU Issues

802.1Q tagging adds four bytes to an Ethernet frame.

Devices and links must support the required frame size.

Potential symptoms:

* Small packets work but larger packets fail.
* Certain applications experience intermittent connectivity.
* Tagged traffic fails across a particular device.
* Encapsulated traffic experiences unexpected packet loss.

Verify:

* Interface MTU settings.
* Switch frame-size support.
* VLAN tagging behavior.
* Additional encapsulation overhead.
* Path MTU and fragmentation behavior.

Do not assume every switch supports the same maximum frame size or jumbo-frame configuration.

# 🗁 Packet Capture for VLAN Analysis

Packet captures can reveal whether VLAN tags are present and whether expected traffic reaches an interface.

Capture traffic on Linux:

```bash
sudo tcpdump -eni eth0
```

The `-e` option displays Ethernet header information, including VLAN information when available.

Capture traffic for a particular VLAN:

```bash
sudo tcpdump -eni eth0 vlan 10
```

Capture traffic with Wireshark using a display filter:

```text
vlan.id == 10
```

Useful filters:

```text
vlan
vlan.id == 10
arp
dhcp
icmp
```

Check:

* Is the expected VLAN tag present?
* Is the VLAN ID correct?
* Are frames unexpectedly untagged?
* Are DHCP requests visible?
* Are ARP requests and replies exchanged?
* Are packets arriving on the expected interface?

Some NIC offloading and capture configurations can affect how VLAN tags appear in packet captures. Consider this when interpreting results.

# 🗁 Practical Troubleshooting Scenarios

**Scenario: Two devices in the same VLAN cannot communicate**

Check:

* Both devices' IP addresses and subnet masks.
* Access-port VLAN assignments.
* Switch port status.
* VLAN existence.
* MAC address learning.
* STP state.
* ARP resolution.

Possible cause: One device is connected to a port assigned to a different VLAN.

**Scenario: VLAN works on one switch but not another**

Check:

* Trunk status between switches.
* Allowed VLAN lists.
* VLAN existence on both switches.
* Native VLAN configuration.
* STP forwarding state.

Possible cause: VLAN 20 is missing from an intermediate trunk's allowed VLAN list.

**Scenario: Client receives an IP address from the wrong subnet**

Check:

* Access-port VLAN assignment.
* PVID.
* DHCP server scopes.
* DHCP relay configuration.
* Trunk tagging.
* Multiple DHCP servers.

Possible cause: The endpoint's untagged traffic is assigned to the wrong VLAN.

**Scenario: Inter-VLAN communication fails**

Check:

* VLAN interfaces.
* Gateway addresses.
* Connected routes.
* Firewall forwarding policies.
* Trunk configuration.
* Return routing.

Possible cause: A firewall rule blocks forwarding between VLAN 10 and VLAN 20.

**Scenario: MikroTik VLAN clients lose connectivity after enabling VLAN filtering**

Check:

* Bridge VLAN table.
* PVID values.
* Tagged and untagged membership.
* Bridge interface membership.
* Management VLAN configuration.
* Trunk port configuration.

Possible cause: The bridge VLAN table does not include the management VLAN or the required tagged ports.

# 🗁 VLAN Troubleshooting Checklist

* [ ] Identify the affected VLAN ID.
* [ ] Verify endpoint IP configuration.
* [ ] Check physical interface status.
* [ ] Confirm access-port VLAN assignment.
* [ ] Verify PVID on VLAN-aware bridges.
* [ ] Confirm VLAN existence on switches.
* [ ] Inspect trunk configuration.
* [ ] Verify allowed VLAN lists.
* [ ] Check tagged and untagged membership.
* [ ] Inspect native VLAN settings.
* [ ] Check MAC address learning.
* [ ] Verify ARP communication.
* [ ] Inspect spanning-tree state.
* [ ] Check DHCP server and relay configuration.
* [ ] Verify Layer 3 VLAN interfaces.
* [ ] Inspect inter-VLAN firewall policies.
* [ ] Check routing and return paths.
* [ ] Investigate MTU and encapsulation issues.
* [ ] Capture packets when necessary.
* [ ] Document the root cause and corrective action.

# 🗁 Best Practices

* Maintain a VLAN inventory with IDs, names, and subnets.
* Document access and trunk port assignments.
* Use consistent VLAN IDs across interconnected switches.
* Explicitly define allowed VLANs on trunk links.
* Keep native VLAN configuration consistent.
* Avoid using VLAN 1 for sensitive management traffic where practical.
* Restrict access to management VLANs.
* Use Layer 3 firewall policies to control inter-VLAN communication.
* Maintain configuration backups.
* Validate bridge VLAN tables before enabling VLAN filtering.
* Keep spanning tree enabled and properly configured.
* Monitor VLAN interfaces and trunk links.
* Test VLAN connectivity after configuration changes.

# 🗁 Summary

VLAN troubleshooting requires checking the complete Layer 2 and Layer 3 path.

The most important areas are:

* Access-port VLAN membership.
* Trunk configuration and allowed VLANs.
* IEEE 802.1Q tagging.
* PVID and bridge VLAN tables.
* Inter-VLAN routing.
* DHCP relay and address assignment.
* Spanning tree.
* Firewall policies and packet captures.

A VLAN is only functional when every relevant switch, trunk, bridge, and Layer 3 device handles its traffic consistently.
