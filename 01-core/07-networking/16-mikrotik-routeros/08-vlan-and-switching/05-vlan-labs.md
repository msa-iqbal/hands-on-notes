# VLAN Labs

Practical VLAN labs help you understand how VLANs work in MikroTik RouterOS, how bridge VLAN filtering controls Layer 2 traffic, and how VLAN interfaces enable Layer 3 communication.

These labs use MikroTik RouterOS v7 and focus on practical configuration, verification, and troubleshooting.

# 🗁 Lab Environment

You can perform these labs using:

* MikroTik physical router.
* MikroTik CHR (Cloud Hosted Router).
* Managed switch with IEEE 802.1Q VLAN support.
* Virtual machines or network simulators that support VLAN tagging.

For virtual labs, make sure the virtual network environment supports VLAN-tagged frames.

# 🗁 Lab Topology

The main lab uses one MikroTik router and one managed switch.

```text
                  +------------------+
                  | MikroTik Router  |
                  |                  |
                  | Bridge: br0      |
                  |                  |
                  | VLAN 10          |
                  | 192.168.10.1/24  |
                  |                  |
                  | VLAN 20          |
                  | 192.168.20.1/24  |
                  |                  |
                  | VLAN 99          |
                  | 192.168.99.1/24  |
                  +--------+---------+
                           |
                      ether1 (Trunk)
                           |
                  +--------+---------+
                  | Managed Switch   |
                  +--------+---------+
                           |
              +------------+-------------+
              |            |             |
           VLAN 10       VLAN 20       VLAN 99
           Access        Access        Access
              |            |             |
          +---+---+    +---+---+     +---+---+
          | PC 1  |    | PC 2  |     | Admin |
          |       |    |       |     | PC    |
          +-------+    +-------+     +-------+
```

# 🗁 Lab IP Addressing Plan

| Device / Interface | VLAN | IP Address |
| --- | ---: | --- |
| MikroTik VLAN 10 | 10 | `192.168.10.1/24` |
| PC 1 | 10 | `192.168.10.10/24` |
| MikroTik VLAN 20 | 20 | `192.168.20.1/24` |
| PC 2 | 20 | `192.168.20.10/24` |
| MikroTik VLAN 99 | 99 | `192.168.99.1/24` |
| Admin PC | 99 | `192.168.99.10/24` |

Default gateways:

* PC 1: `192.168.10.1`
* PC 2: `192.168.20.1`
* Admin PC: `192.168.99.1`

# 🗁 Lab: Create a VLAN-Aware Bridge

**Objective:** Create a bridge that supports VLAN-aware Layer 2 switching.

Create the bridge:

```routeros
/interface bridge
add name=br0 vlan-filtering=no
```

Add the trunk interface:

```routeros
/interface bridge port
add bridge=br0 interface=ether1
```

At this stage, VLAN filtering is disabled so you can prepare the VLAN membership configuration before enabling it.

Verify the bridge:

```routeros
/interface bridge print
/interface bridge port print
```

**Expected result:**

* A bridge named `br0` exists.
* Interface `ether1` is a member of `br0`.

# 🗁 Lab: Configure VLAN Membership

**Objective:** Configure VLAN 10, VLAN 20, and VLAN 99 as tagged VLANs on the trunk and bridge CPU port.

```routeros
/interface bridge vlan
add bridge=br0 vlan-ids=10 tagged=br0,ether1
add bridge=br0 vlan-ids=20 tagged=br0,ether1
add bridge=br0 vlan-ids=99 tagged=br0,ether1
```

The bridge itself is included as a tagged member because RouterOS needs CPU access to these VLANs for Layer 3 interfaces.

Verify the VLAN table:

```routeros
/interface bridge vlan print
```

**Expected result:**

| VLAN ID | Tagged Members |
| --- | --- |
| 10 | `br0`, `ether1` |
| 20 | `br0`, `ether1` |
| 99 | `br0`, `ether1` |

# 🗁 Lab: Create VLAN Interfaces

**Objective:** Create Layer 3 interfaces for the VLAN networks.

```routeros
/interface vlan
add name=vlan10 interface=br0 vlan-id=10
add name=vlan20 interface=br0 vlan-id=20
add name=vlan99 interface=br0 vlan-id=99
```

Verify:

```routeros
/interface vlan print
```

**Expected result:**

* `vlan10` uses VLAN ID 10.
* `vlan20` uses VLAN ID 20.
* `vlan99` uses VLAN ID 99.
* All VLAN interfaces use `br0` as their parent interface.

# 🗁 Lab: Assign IP Addresses

**Objective:** Configure the gateway address for each VLAN.

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
add address=192.168.99.1/24 interface=vlan99
```

Verify:

```routeros
/ip address print
```

Check the connected routes:

```routeros
/ip route print
```

**Expected result:**

The router should have connected routes for:

```text
192.168.10.0/24
192.168.20.0/24
192.168.99.0/24
```

These routes appear when the corresponding VLAN interfaces and IP addresses are active.

# 🗁 Lab: Configure Access Ports on the Switch

**Objective:** Assign endpoint ports to the correct VLANs.

Configure the managed switch as follows:

| Switch Port | Mode | VLAN |
| --- | --- | --- |
| Port 1 | Trunk | Tagged 10, 20, 99 |
| Port 2 | Access | Untagged 10 |
| Port 3 | Access | Untagged 20 |
| Port 4 | Access | Untagged 99 |

Configure the switch's trunk port to connect to MikroTik `ether1`.

Configure the endpoint ports with the appropriate untagged VLAN membership and PVID.

The exact commands depend on the switch platform.

**Expected result:**

* PC 1 belongs to VLAN 10.
* PC 2 belongs to VLAN 20.
* Admin PC belongs to VLAN 99.
* The trunk carries all three VLANs as tagged traffic.

# 🗁 Lab: Enable Bridge VLAN Filtering

**Objective:** Activate VLAN-aware forwarding on the MikroTik bridge.

Before enabling VLAN filtering, verify:

* The trunk port is a bridge port.
* All required VLAN entries exist.
* The bridge CPU port is tagged in every VLAN that needs router access.
* The managed switch trunk configuration is correct.

Enable VLAN filtering:

```routeros
/interface bridge
set br0 vlan-filtering=yes
```

Verify:

```routeros
/interface bridge print
/interface bridge vlan print
```

**Expected result:**

The bridge should operate with VLAN filtering enabled.

If your management connection uses one of these VLANs, confirm that the management path is correctly configured before applying the change.

# 🗁 Lab: Test Same-VLAN Communication

**Objective:** Verify Layer 2 communication within a VLAN.

If you have two devices connected to access ports in VLAN 10, assign addresses from the same subnet.

Example:

```text
PC 1: 192.168.10.10/24
PC 2: 192.168.10.11/24
```

From PC 1:

```bash
ping 192.168.10.11
```

**Expected result:**

The devices should communicate within VLAN 10 if their switch ports, IP settings, and host firewalls are configured correctly.

Repeat the test with two devices in VLAN 20.

# 🗁 Lab: Test Inter-VLAN Routing

**Objective:** Verify communication between different VLANs through the MikroTik router.

Configure PC 1:

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

Configure PC 2:

```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

From PC 1, test the VLAN 20 gateway:

```bash
ping 192.168.20.1
```

Then test PC 2:

```bash
ping 192.168.20.10
```

**Expected result:**

PC 1 should be able to communicate with PC 2 through the router if firewall rules and host firewalls permit the traffic.

# 🗁 Lab: Restrict Inter-VLAN Communication

**Objective:** Prevent VLAN 20 from initiating connections to VLAN 10.

Add a forward-chain rule:

```routeros
/ip firewall filter
add chain=forward action=drop in-interface=vlan20 out-interface=vlan10 connection-state=new comment="Block new VLAN 20 to VLAN 10 connections"
```

Place the rule before broader forward-chain accept rules that would otherwise permit the traffic.

Test from PC 2:

```bash
ping 192.168.10.10
```

**Expected result:**

New connections initiated from VLAN 20 toward VLAN 10 should be blocked by this rule.

This is a focused example. Existing connection-tracking and firewall policies may affect the result. Test with a new connection after applying the rule.

# 🗁 Lab: Verify Management VLAN Access

**Objective:** Allow administration from VLAN 99 while keeping management services restricted.

Configure the Admin PC:

```text
IP Address:      192.168.99.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.99.1
```

Restrict WinBox and SSH to the administrator address:

```routeros
/ip service
set winbox address=192.168.99.10/32
set ssh address=192.168.99.10/32
```

Test connectivity:

```bash
ping 192.168.99.1
ssh admin@192.168.99.1
```

You can also connect through WinBox using `192.168.99.1`.

**Expected result:**

The administrator workstation should reach the router through VLAN 99, while other source addresses are not permitted by these service restrictions.

Ensure the input firewall also permits the intended management traffic.

# 🗁 Lab: Troubleshoot VLAN Configuration

**Objective:** Identify common VLAN configuration problems.

Use the following commands to inspect the configuration:

```routeros
/interface bridge print
/interface bridge port print
/interface bridge vlan print
/interface vlan print
/ip address print
/ip route print
/ip firewall filter print
```

Check interface counters:

```routeros
/interface print stats
```

Inspect bridge host learning:

```routeros
/interface bridge host print
```

Useful troubleshooting checks:

| Problem | What to Check |
| --- | --- |
| VLAN interface does not receive traffic | Bridge CPU tagged membership |
| Endpoint cannot reach gateway | Access VLAN, PVID, trunk membership |
| Same-VLAN communication fails | Switch access ports and VLAN membership |
| Inter-VLAN communication fails | IP gateways, connected routes, forward firewall |
| Management access fails | VLAN 99 tagging, input firewall, service restrictions |
| Unexpected VLAN leakage | Bridge VLAN table, ingress filtering, frame types |

# 🗁 Lab: Packet Capture and Traffic Inspection

**Objective:** Inspect VLAN traffic and verify whether frames are tagged.

Use the MikroTik sniffer:

```routeros
/tool sniffer
set filter-interface=ether1
start
```

Stop the capture:

```routeros
/tool sniffer
stop
```

You can inspect the captured traffic using the available sniffer output or export options.

You can also use Torch:

```routeros
/tool torch interface=ether1
```

For detailed 802.1Q tag inspection, use a packet capture tool such as Wireshark on a suitable switch mirror port or capture point.

**Expected result:**

You should be able to identify traffic traversing the trunk and determine whether the expected VLANs are present.

# 🗁 Lab: Save and Document the Configuration

**Objective:** Export the VLAN configuration for documentation and backup.

Export the router configuration:

```routeros
/export file=vlan-lab
```

The export is saved in the router's file storage.

Download the export through WinBox or another supported file-transfer method.

Record:

* VLAN IDs and names.
* Subnet addresses.
* Gateway IP addresses.
* Trunk ports.
* Access ports.
* Management addresses.
* Firewall policies.

# 🗁 Lab Completion Checklist

* [ ] Create a VLAN-aware bridge.
* [ ] Add the trunk interface to the bridge.
* [ ] Configure VLAN 10, VLAN 20, and VLAN 99 membership.
* [ ] Include the bridge CPU port as a tagged member.
* [ ] Create VLAN interfaces on the bridge.
* [ ] Assign IP addresses to VLAN interfaces.
* [ ] Configure the managed switch trunk.
* [ ] Configure access ports and PVIDs.
* [ ] Enable bridge VLAN filtering.
* [ ] Test same-VLAN communication.
* [ ] Test inter-VLAN routing.
* [ ] Apply inter-VLAN firewall restrictions.
* [ ] Verify Management VLAN access.
* [ ] Inspect traffic and troubleshoot connectivity.
* [ ] Export and document the configuration.

# 🗁 Summary

These labs demonstrate how to configure and troubleshoot VLANs in MikroTik RouterOS v7.

You practiced:

* VLAN-aware bridge configuration.
* Access and trunk port design.
* Bridge VLAN filtering.
* VLAN interfaces and gateway addressing.
* Inter-VLAN routing.
* Management VLAN configuration.
* Firewall-based VLAN isolation.
* Traffic inspection and troubleshooting.

A successful VLAN deployment requires correct Layer 2 membership, Layer 3 addressing, trunk configuration, and firewall policy working together.
