# VLAN Exercises

VLAN exercises help you understand how to divide a physical network into multiple logical networks using Virtual LAN technology.

In these labs, you will configure VLANs, access ports, trunk ports, IEEE 802.1Q tagging, and inter-VLAN routing using MikroTik RouterOS v7.

You will also practice VLAN isolation, connectivity testing, and troubleshooting common VLAN configuration problems.

### 🖆 Learning Objectives

By completing these exercises, you will be able to:

* Understand VLAN fundamentals.
* Create and configure VLANs.
* Configure access and trunk ports.
* Understand IEEE 802.1Q tagging.
* Configure VLAN membership on a MikroTik bridge.
* Configure inter-VLAN routing.
* Verify VLAN connectivity.
* Isolate devices using VLAN segmentation.
* Troubleshoot VLAN-related problems.

### 🖆 Lab Requirements

| Component | Requirement |
| --- | --- |
| Router | MikroTik CHR |
| RouterOS version | RouterOS v7 |
| Clients | Ubuntu/Linux virtual machines |
| Virtualization | VirtualBox, GNS3, or EVE-NG |
| Network interfaces | At least three router interfaces |
| VLANs | VLAN 10, VLAN 20, VLAN 30 |

# 🗁 VLAN Fundamentals

### 🖆 What Is a VLAN?

A VLAN (Virtual Local Area Network) is a logical network segment that separates devices into different broadcast domains, even when they share the same physical switching infrastructure.

For example, an organization can separate its departments into different VLANs:

| Department | VLAN ID | Network |
| --- | ---: | --- |
| Administration | 10 | `192.168.10.0/24` |
| Development | 20 | `192.168.20.0/24` |
| Management | 30 | `192.168.30.0/24` |

Devices in different VLANs belong to separate Layer 2 broadcast domains.

Communication between different VLANs requires a Layer 3 device, such as a router or Layer 3 switch.

### 🖆 Benefits of VLANs

* **Network segmentation:** Separates departments and device groups.
* **Broadcast control:** Limits broadcast traffic to individual VLANs.
* **Security:** Helps isolate devices and restrict communication.
* **Network management:** Makes logical network administration easier.
* **Flexibility:** Allows devices to belong to logical networks independently of their physical location.

### 🖆 VLAN ID

A VLAN ID identifies a VLAN.

IEEE 802.1Q uses a 12-bit VLAN identifier.

| VLAN ID | Description |
| --- | --- |
| 0 | Reserved for priority-tagged frames |
| 1 | Common default VLAN on many switches |
| 2–4094 | Normal usable VLAN IDs |
| 4095 | Reserved |

VLAN 1 is commonly the default VLAN, but it should not automatically be treated as a secure management VLAN.

### 🖆 Access Port

An access port normally carries traffic for one VLAN.

The connected endpoint usually sends and receives untagged Ethernet frames. The switch associates those frames with the configured access VLAN.

Example:

```text
PC-A ---- Access Port ---- Switch
             VLAN 10
```

### 🖆 Trunk Port

A trunk port carries traffic for multiple VLANs.

IEEE 802.1Q tagging identifies the VLAN associated with each Ethernet frame.

Example:

```text
                 Trunk Link
Switch-A ======================== Switch-B
          VLAN 10, 20, 30
```

A trunk can carry multiple VLANs over one physical connection.

### 🖆 IEEE 802.1Q Tagging

IEEE 802.1Q defines a VLAN tag inserted into an Ethernet frame.

The tag contains:

* Tag Protocol Identifier (TPID).
* Priority Code Point (PCP).
* Drop Eligible Indicator (DEI).
* VLAN Identifier (VID).

The VLAN ID identifies the VLAN to which the tagged frame belongs.

### 🖆 Tagged and Untagged Frames

| Frame Type | Description |
| --- | --- |
| Tagged | Contains an IEEE 802.1Q VLAN tag. |
| Untagged | Does not contain an IEEE 802.1Q VLAN tag. |

Access ports commonly receive and transmit untagged frames.

Trunk ports commonly transmit tagged frames for their allowed VLANs.

Some trunk configurations also carry an untagged native VLAN. Both ends must agree on how untagged traffic is handled.

# 🗁 Lab Topology

### 🖆 Basic VLAN Lab

Build a topology with one MikroTik router and three clients.

```text
                  MikroTik Router
                  /      |      \
                 /       |       \
              ether2  ether3   ether4
                |        |        |
               PC-A     PC-B     PC-C
              VLAN 10  VLAN 20  VLAN 30
```

Each router interface will initially act as an access port for one VLAN.

### 🖆 IP Addressing Plan

| Device | VLAN | IP Address | Gateway |
| --- | ---: | --- | --- |
| MikroTik | 10 | `192.168.10.1/24` | — |
| PC-A | 10 | `192.168.10.10/24` | `192.168.10.1` |
| MikroTik | 20 | `192.168.20.1/24` | — |
| PC-B | 20 | `192.168.20.10/24` | `192.168.20.1` |
| MikroTik | 30 | `192.168.30.1/24` | — |
| PC-C | 30 | `192.168.30.10/24` | `192.168.30.1` |

# 🗁 Exercise: Create VLAN Interfaces

### 🖆 Understand VLAN Interfaces

A VLAN interface allows a router to process traffic belonging to a particular VLAN.

In MikroTik RouterOS, VLAN interfaces can be created on physical interfaces or bridges.

For this exercise, create VLAN interfaces on a bridge.

### 🖆 Create a Bridge

```routeros
/interface bridge
add name=br-core vlan-filtering=no
```

### 🖆 Add Physical Interfaces to the Bridge

```routeros
/interface bridge port
add bridge=br-core interface=ether2 pvid=10
add bridge=br-core interface=ether3 pvid=20
add bridge=br-core interface=ether4 pvid=30
```

The PVID determines which VLAN receives untagged ingress traffic on each access port.

### 🖆 Create VLAN Interfaces

```routeros
/interface vlan
add name=vlan10 interface=br-core vlan-id=10
add name=vlan20 interface=br-core vlan-id=20
add name=vlan30 interface=br-core vlan-id=30
```

These VLAN interfaces will provide Layer 3 connectivity for the corresponding VLANs.

### 🖆 Assign IP Addresses

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
add address=192.168.30.1/24 interface=vlan30
```

### 🖆 Configure Bridge VLAN Membership

```routeros
/interface bridge vlan
add bridge=br-core vlan-ids=10 tagged=br-core untagged=ether2
add bridge=br-core vlan-ids=20 tagged=br-core untagged=ether3
add bridge=br-core vlan-ids=30 tagged=br-core untagged=ether4
```

The bridge itself is included as a tagged member because the router needs access to the VLANs through its CPU for Layer 3 routing.

### 🖆 Configure Access Port Frame Types

```routeros
/interface bridge port
set [find interface=ether2] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged
set [find interface=ether3] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged
set [find interface=ether4] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged
```

### 🖆 Enable VLAN Filtering

After configuring the bridge ports and VLAN membership, enable VLAN filtering:

```routeros
/interface bridge
set br-core vlan-filtering=yes
```

Enabling VLAN filtering before preparing the correct VLAN membership can interrupt connectivity. Perform this step carefully, especially when managing the router remotely.

### 🖆 Verify VLAN Configuration

```routeros
/interface bridge print
/interface bridge port print
/interface bridge vlan print
/interface vlan print
/ip address print
```

Verify that each VLAN has the correct port membership and IP address.

# 🗁 Exercise: Configure Access Ports

### 🖆 Understand Access Port Configuration

An access port connects an endpoint that normally sends and receives untagged Ethernet frames.

The switch assigns untagged ingress frames to the VLAN configured by the port's PVID.

In the previous exercise:

| Interface | Connected Device | PVID |
| --- | --- | ---: |
| ether2 | PC-A | 10 |
| ether3 | PC-B | 20 |
| ether4 | PC-C | 30 |

### 🖆 Verify Access Port Membership

```routeros
/interface bridge port print detail
/interface bridge vlan print detail
```

Check:

* PC-A belongs to VLAN 10.
* PC-B belongs to VLAN 20.
* PC-C belongs to VLAN 30.
* Untagged ingress traffic is assigned to the correct VLAN.

### 🖆 Test Same-VLAN Connectivity

Connect two clients to different access ports assigned to the same VLAN.

For example, add another access port:

```routeros
/interface bridge port
add bridge=br-core interface=ether5 pvid=10
```

Update VLAN membership:

```routeros
/interface bridge vlan
add bridge=br-core vlan-ids=10 tagged=br-core untagged=ether2,ether5
```

Connect another PC to `ether5` with an IP address in `192.168.10.0/24`.

Test connectivity between the two VLAN 10 clients.

They should communicate at Layer 2 without requiring inter-VLAN routing.

# 🗁 Exercise: Configure a Trunk Port

### 🖆 Understand Trunk Port Configuration

A trunk port transports traffic for multiple VLANs over one physical link.

For example, a trunk between two switches can carry:

* VLAN 10.
* VLAN 20.
* VLAN 30.

The receiving device uses the 802.1Q VLAN tag to identify each frame's VLAN.

### 🖆 Add a Trunk Interface

Assume `ether1` connects to another VLAN-aware switch or router.

Add it to the bridge:

```routeros
/interface bridge port
add bridge=br-core interface=ether1 ingress-filtering=yes frame-types=admit-only-vlan-tagged
```

### 🖆 Configure Tagged VLAN Membership

Update the VLAN membership table:

```routeros
/interface bridge vlan
set [find bridge=br-core vlan-ids=10] tagged=br-core,ether1 untagged=ether2,ether5

set [find bridge=br-core vlan-ids=20] tagged=br-core,ether1 untagged=ether3

set [find bridge=br-core vlan-ids=30] tagged=br-core,ether1 untagged=ether4
```

Now `ether1` carries VLANs 10, 20, and 30 as tagged traffic.

### 🖆 Verify Trunk Configuration

```routeros
/interface bridge port print detail
/interface bridge vlan print detail
```

Check that:

* `ether1` is a bridge port.
* `ether1` accepts tagged frames only.
* VLANs 10, 20, and 30 include `ether1` as a tagged member.
* Access ports remain untagged members of their assigned VLANs.

# 🗁 Exercise: Configure Inter-VLAN Routing

### 🖆 Understand Inter-VLAN Routing

Inter-VLAN routing allows devices in different VLANs to communicate through a Layer 3 device.

For example:

```text
PC-A                         PC-B
VLAN 10                      VLAN 20
192.168.10.10                192.168.20.10
       \                      /
        \                    /
         MikroTik Router
         vlan10  vlan20
```

The router receives traffic from one VLAN, makes a Layer 3 forwarding decision, and sends it into the destination VLAN.

### 🖆 Verify VLAN Gateway Addresses

On MikroTik:

```routeros
/ip address print
/interface vlan print
```

Expected gateway addresses:

| VLAN | Gateway |
| --- | --- |
| VLAN 10 | `192.168.10.1` |
| VLAN 20 | `192.168.20.1` |
| VLAN 30 | `192.168.30.1` |

### 🖆 Configure Client Gateways

PC-A:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
sudo ip route replace default via 192.168.10.1
```

PC-B:

```bash
sudo ip address add 192.168.20.10/24 dev enp0s3
sudo ip route replace default via 192.168.20.1
```

PC-C:

```bash
sudo ip address add 192.168.30.10/24 dev enp0s3
sudo ip route replace default via 192.168.30.1
```

Replace `enp0s3` with the actual interface name on each client.

### 🖆 Test Inter-VLAN Connectivity

From PC-A:

```bash
ping 192.168.10.1
ping 192.168.20.10
ping 192.168.30.10
```

If the bridge VLAN configuration, client gateways, and router forwarding policy are correct, the clients should be able to communicate across VLANs.

**Important:** Inter-VLAN routing does not automatically provide security isolation. If communication between VLANs must be restricted, configure appropriate Layer 3 firewall rules.

# 🗁 Exercise: Isolate VLANs

### 🖆 Understand VLAN Isolation

VLANs separate Layer 2 broadcast domains, but a router with inter-VLAN routing can still forward traffic between them.

To restrict communication, use firewall rules.

For example, assume:

* VLAN 10 is Administration.
* VLAN 20 is Development.
* VLAN 30 is Guest.

The security policy is:

| Source VLAN | Destination VLAN | Policy |
| --- | --- | --- |
| VLAN 10 | VLAN 20 | Allow |
| VLAN 10 | VLAN 30 | Allow |
| VLAN 20 | VLAN 10 | Block new connections |
| VLAN 30 | VLAN 10 | Block |
| VLAN 30 | VLAN 20 | Block |

### 🖆 Block Guest Access to Internal VLANs

```routeros
/ip firewall filter
add chain=forward in-interface=vlan30 out-interface=vlan10 action=drop comment="Block Guest to Administration"

add chain=forward in-interface=vlan30 out-interface=vlan20 action=drop comment="Block Guest to Development"
```

### 🖆 Verify the Isolation Policy

From PC-C, test:

```bash
ping 192.168.10.10
ping 192.168.20.10
```

These connections should fail when the firewall rules are correctly applied and no earlier rule permits the traffic.

Test access to permitted destinations separately.

### 🖆 Inspect Firewall Counters

```routeros
/ip firewall filter print stats
```

Confirm that the expected rules match the test traffic.

# 🗁 Exercise: Configure VLAN Management

### 🖆 Understand Management VLANs

A management VLAN is used to access and administer network infrastructure devices.

For example:

| Purpose | VLAN |
| --- | ---: |
| Administration | 10 |
| Development | 20 |
| Network Management | 30 |

Management interfaces should only be accessible from authorized management devices.

### 🖆 Assign a Management IP Address

For this lab, use the existing VLAN 30 interface:

```routeros
/ip address
add address=192.168.30.1/24 interface=vlan30
```

If this address was already assigned in the earlier exercise, do not add it again.

### 🖆 Restrict Router Management Access

Allow SSH and WinBox from a trusted management workstation:

```routeros
/ip firewall filter
add chain=input src-address=192.168.30.10 protocol=tcp dst-port=22 action=accept comment="Allow management SSH"

add chain=input src-address=192.168.30.10 protocol=tcp dst-port=8291 action=accept comment="Allow management WinBox"
```

Place these rules before the final input-chain drop rule.

For a complete management policy, also consider the required ICMP, DNS, NTP, and other management services.

# 🗁 Exercise: Verify VLAN Traffic

### 🖆 Inspect Bridge VLAN Membership

```routeros
/interface bridge vlan print detail
```

This command displays VLAN membership and tagged or untagged ports.

### 🖆 Inspect Bridge Ports

```routeros
/interface bridge port print detail
```

Check each port's PVID, frame types, and ingress filtering configuration.

### 🖆 Inspect VLAN Interfaces

```routeros
/interface vlan print detail
```

### 🖆 Inspect IP Addresses

```routeros
/ip address print
```

### 🖆 Test Client Connectivity

On Ubuntu:

```bash
ip address
ip route
ping 192.168.10.1
traceroute 192.168.20.10
```

Use the appropriate destination addresses for each VLAN.

### 🖆 Capture VLAN-Tagged Traffic

Install Wireshark:

```bash
sudo apt update
sudo apt install wireshark
```

Capture traffic on a trunk link or a suitable mirrored interface.

Useful Wireshark display filter:

```text
vlan
```

You can inspect the VLAN ID in the IEEE 802.1Q header.

A normal access-port capture may show untagged frames because the switch removes VLAN tags before forwarding traffic to the endpoint.

# 🗁 Linux VLAN Practice

### 🖆 Create a VLAN Interface

Linux supports IEEE 802.1Q VLAN interfaces.

Install the VLAN utility if needed:

```bash
sudo apt update
sudo apt install vlan
```

Create VLAN 10 on a physical interface:

```bash
sudo ip link add link enp0s3 name enp0s3.10 type vlan id 10
```

### 🖆 Assign an IP Address

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3.10
```

### 🖆 Enable the VLAN Interface

```bash
sudo ip link set enp0s3 up
sudo ip link set enp0s3.10 up
```

### 🖆 Verify the VLAN Interface

```bash
ip -d link show enp0s3.10
ip address show enp0s3.10
```

### 🖆 Remove the VLAN Interface

```bash
sudo ip link delete enp0s3.10
```

These commands create a tagged VLAN interface on Linux. The connected switch or router port must be configured as a trunk that carries VLAN 10.

# 🗁 VLAN Troubleshooting Exercises

### 🖆 Problem: Devices in the Same VLAN Cannot Communicate

**Symptoms:**

* Devices belong to the same VLAN.
* Both have valid IP addresses.
* They cannot communicate.

**Investigation:**

```routeros
/interface bridge port print detail
/interface bridge vlan print detail
```

Check:

* Correct PVID.
* Correct VLAN membership.
* Access port configuration.
* Physical or virtual link status.

**Solution:**

Correct the PVID and VLAN membership configuration.

### 🖆 Problem: Trunk Link Does Not Carry VLAN Traffic

**Symptoms:**

* Devices work locally.
* VLAN traffic does not pass between switches.
* Remote VLAN clients cannot communicate.

**Investigation:**

```routeros
/interface bridge port print detail
/interface bridge vlan print detail
```

Check:

* Both ends are configured as trunks.
* Required VLANs are allowed on both ends.
* Tagged membership is correct.
* Frame-type restrictions are compatible.

**Solution:**

Correct the trunk configuration and ensure that both devices agree on VLAN tagging.

### 🖆 Problem: Incorrect PVID

**Symptoms:**

* Untagged devices enter the wrong VLAN.
* Clients receive unexpected connectivity.
* VLAN isolation behaves incorrectly.

**Investigation:**

```routeros
/interface bridge port print detail
```

**Solution:**

Assign the correct PVID to each access port.

### 🖆 Problem: Inter-VLAN Routing Does Not Work

**Symptoms:**

* Clients can reach their own VLAN gateway.
* Devices in other VLANs are unreachable.

**Investigation:**

```routeros
/interface vlan print
/ip address print
/ip route print
/ip firewall filter print stats
```

Check:

* VLAN interfaces are configured on the correct bridge.
* VLAN membership includes the bridge CPU port.
* Gateway addresses are correct.
* Client default gateways are correct.
* Firewall rules permit the required forwarding traffic.

**Solution:**

Correct the VLAN interface configuration, bridge membership, client gateway, or forwarding policy.

### 🖆 Problem: VLAN Interface Is Not Operational

**Symptoms:**

* VLAN interface exists.
* It does not become operational.
* Traffic cannot pass through it.

**Investigation:**

```routeros
/interface vlan print detail
/interface bridge print detail
/interface bridge vlan print detail
```

**Solution:**

Verify that the parent interface or bridge is operational and that the VLAN is correctly configured and present on the required ports.

# 🗁 Practice Challenges

### 🖆 Challenge: Three-Department Network

Build a network for three departments:

| Department | VLAN | Network |
| --- | ---: | --- |
| Administration | 10 | `192.168.10.0/24` |
| Development | 20 | `192.168.20.0/24` |
| Accounts | 30 | `192.168.30.0/24` |

Requirements:

* Create three VLANs.
* Configure access ports.
* Configure one trunk port.
* Assign gateway IP addresses.
* Test same-VLAN connectivity.
* Test inter-VLAN connectivity.
* Verify VLAN membership.

### 🖆 Challenge: Guest Network Isolation

Create a guest VLAN.

Requirements:

* Guest devices must communicate with their gateway.
* Guest devices must not access internal VLANs.
* Guest devices may access permitted external destinations.
* Management VLAN access must remain restricted.
* Verify the firewall rules using counters.

### 🖆 Challenge: Multi-Switch VLAN Network

Build a topology with two switches or MikroTik routers acting as Layer 2 bridges.

Requirements:

* Create VLAN 10 and VLAN 20 on both devices.
* Configure a trunk between devices.
* Configure access ports for each VLAN.
* Connect clients to both devices.
* Verify same-VLAN communication across the trunk.
* Verify that different VLANs remain separated at Layer 2.

### 🖆 Challenge: Inter-VLAN Routing and Security

Build a network with three VLANs.

Requirements:

* Configure Layer 3 gateway interfaces.
* Allow selected communication between VLANs.
* Block unauthorized communication.
* Permit management access only from the management VLAN.
* Verify the resulting security policy.

# 🗁 Knowledge Check

### 🖆 Questions

* What is a VLAN?
* What is a broadcast domain?
* What is a VLAN ID?
* What is the purpose of IEEE 802.1Q?
* What is the difference between an access port and a trunk port?
* What is a PVID?
* What is the difference between tagged and untagged frames?
* Why is the bridge interface included as a tagged VLAN member when routing VLANs through a MikroTik bridge?
* What is inter-VLAN routing?
* Do VLANs automatically prevent communication between different IP networks?
* Why is VLAN isolation important?
* How can you inspect VLAN membership in MikroTik RouterOS?
* Why must trunk configurations agree on both connected devices?

### 🖆 Practical Verification Checklist

* [ ] VLAN fundamentals are understood.
* [ ] VLAN 10, VLAN 20, and VLAN 30 are configured.
* [ ] Bridge ports are configured correctly.
* [ ] Access ports use the correct PVID.
* [ ] Trunk ports carry the required tagged VLANs.
* [ ] IEEE 802.1Q tagging is understood.
* [ ] Bridge VLAN membership is verified.
* [ ] VLAN interfaces have correct gateway addresses.
* [ ] Inter-VLAN routing works.
* [ ] VLAN isolation policies are tested.
* [ ] Management access is restricted.
* [ ] VLAN traffic can be inspected.
* [ ] Common VLAN problems can be diagnosed.

# 🗁 Expected Learning Outcome

After completing these exercises, you should understand how VLANs separate Layer 2 broadcast domains and how tagged and untagged Ethernet frames are handled.

You should be able to configure VLANs, access ports, trunk ports, bridge VLAN filtering, and inter-VLAN routing using MikroTik RouterOS v7.

You should also understand how VLAN segmentation and firewall policies work together to create a more organized and secure network.
