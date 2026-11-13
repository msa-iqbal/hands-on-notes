# Unicast, Broadcast, and Multicast

Unicast, broadcast, and multicast are three fundamental communication methods used in computer networks.

They describe how network traffic is delivered from a source to one or more destinations.

Understanding these communication methods is essential for learning Ethernet, IP addressing, switching, routing, VLANs, and network protocols.

# 🗁 Overview

Network communication can be classified according to the number of intended destinations.

The three primary communication methods are:

* **Unicast:** One sender communicates with one specific receiver.
* **Broadcast:** One sender communicates with all devices in a broadcast domain.
* **Multicast:** One sender communicates with a selected group of receivers.

### 🖆 Communication Models

```text
UNCAST

Sender ───────────────> Receiver


BROADCAST

                  ┌───> Device A
                  |
Sender ───────────┼───> Device B
                  |
                  └───> Device C


MULTICAST

                  ┌───> Member A
                  |
Sender ───────────┼───> Member B
                  |
                  └───> Member C

             Other devices are not
             intended recipients.
```

### 🖆 Comparison

| Feature | Unicast | Broadcast | Multicast |
| --- | --- | --- | --- |
| Communication | One-to-one | One-to-all | One-to-group |
| Destination | Individual host | All hosts in broadcast domain | Group members |
| Traffic scope | Specific destination | Broadcast domain | Multicast group and forwarding scope |
| Ethernet destination | Individual MAC | Broadcast MAC | Multicast MAC |
| IPv4 destination | Unicast IP | Broadcast IP | Multicast IP |
| Typical example | Web browsing | ARP request | Streaming and routing protocols |

# 🗁 Unicast

Unicast is a communication method in which one sender transmits data to one specific receiver.

It is the most common communication model in ordinary IP networking.

For example, when a computer accesses a website, it normally establishes communication with a specific server IP address.

### 🖆 Unicast Communication Diagram

```text
+----------+                 +----------+
| Sender A | ──────────────> | Receiver |
+----------+                 +----------+

       One sender
           |
           |
    One destination
```

### 🖆 Unicast MAC Address

An Ethernet unicast frame contains an individual destination MAC address.

Example:

```text
Source MAC:
AA:AA:AA:AA:AA:AA

Destination MAC:
BB:BB:BB:BB:BB:BB
```

The destination MAC address identifies one network interface.

A unicast MAC address has its Individual/Group (I/G) bit set to `0`.

### 🖆 Unicast IP Address

A unicast IP address identifies an individual network interface within an IP network.

IPv4 example:

```text
192.168.1.10
```

IPv6 example:

```text
2001:db8::10
```

A packet sent to a unicast IP address is intended for the corresponding destination interface.

### 🖆 Unicast Communication Example

Consider two computers connected to the same switch:

```text
PC A                              PC B

IP: 192.168.1.10                  IP: 192.168.1.20
MAC: AA:AA:AA:AA:AA:AA            MAC: BB:BB:BB:BB:BB:BB

       |                                |
       +------------- Switch -----------+
```

When PC A sends data to PC B:

* PC A determines that PC B is on the same local network.
* PC A resolves PC B's MAC address using ARP if necessary.
* PC A creates an Ethernet frame with PC B's MAC address as the destination.
* The switch forwards the frame toward PC B.
* PC B receives and processes the frame.

### 🖆 Common Unicast Applications

* Web browsing using HTTP or HTTPS.
* SSH connections.
* Remote desktop sessions.
* File transfers.
* Database connections.
* Email communication.
* Direct communication between network devices.

### 🖆 Unicast Advantages

* Traffic is directed to a specific destination.
* Devices that are not destinations do not need to process the application payload.
* Supports individual client-server communication.
* Works naturally with IP routing.

### 🖆 Unicast Limitations

* Sending the same data separately to many receivers creates multiple transmissions.
* Network bandwidth consumption can increase with the number of receivers.
* A server may need to maintain multiple independent connections.

# 🗁 Broadcast

Broadcast is a communication method in which one sender transmits data to all devices within a broadcast domain.

In an Ethernet network, a broadcast frame is addressed to the special destination MAC address:

```text
FF:FF:FF:FF:FF:FF
```

Every device in the same Layer 2 broadcast domain receives the frame, subject to its network interface and VLAN configuration.

### 🖆 Broadcast Communication Diagram

```text
                       +----------+
                       | Device A |
                       +----------+
                            ^
                            |
+----------+          +-----+-----+          +----------+
| Sender   | ------> |   Switch  | -------> | Device B |
+----------+          +-----+-----+          +----------+
                            |
                            v
                       +----------+
                       | Device C |
                       +----------+

All devices in the broadcast domain
receive the broadcast frame.
```

### 🖆 Broadcast MAC Address

The Ethernet broadcast address is:

```text
FF:FF:FF:FF:FF:FF
```

Its binary representation contains 48 bits set to `1`:

```text
11111111:11111111:11111111:
11111111:11111111:11111111
```

A switch floods broadcast frames to other forwarding ports in the same VLAN, excluding the ingress port.

### 🖆 IPv4 Broadcast Address

IPv4 supports broadcast addressing.

There are two important forms:

* Limited broadcast.
* Directed broadcast.

### 🖆 Limited Broadcast

The limited broadcast address is:

```text
255.255.255.255
```

It is used to reach hosts on the local network.

Routers do not forward limited broadcast packets.

### 🖆 Directed Broadcast

A directed broadcast targets all hosts within a specific IPv4 subnet.

For example:

```text
Network:   192.168.1.0/24
Broadcast: 192.168.1.255
```

A packet sent to `192.168.1.255` is intended for the broadcast address of that subnet.

Routers generally disable forwarding of directed broadcasts by default because of security concerns.

### 🖆 How to Calculate a Broadcast Address

Consider:

```text
IP Address: 192.168.1.25
Subnet Mask: 255.255.255.0
CIDR: /24
```

The network address is:

```text
192.168.1.0
```

The broadcast address is:

```text
192.168.1.255
```

The last address in the subnet is the broadcast address for this example.

Broadcast address calculation becomes especially important when studying subnetting.

### 🖆 Broadcast Communication Example: ARP

ARP (Address Resolution Protocol) uses broadcast frames to discover the MAC address associated with an IPv4 address on a local network.

Suppose:

```text
PC A IP: 192.168.1.10
PC B IP: 192.168.1.20
```

PC A knows PC B's IP address but does not know its MAC address.

PC A sends an ARP request:

```text
Who has 192.168.1.20?

Tell 192.168.1.10
```

The Ethernet frame uses:

```text
Destination MAC:
FF:FF:FF:FF:FF:FF
```

All devices in the local broadcast domain receive the request.

PC B recognizes its IP address and sends an ARP reply containing its MAC address.

### 🖆 Common Broadcast Applications

* ARP requests in IPv4.
* DHCP discovery messages in common IPv4 client configurations.
* Certain service discovery mechanisms.
* Local network discovery protocols.

### 🖆 Broadcast Advantages

* Reaches all devices within a broadcast domain.
* Useful when the sender does not know which device should respond.
* Simplifies local discovery mechanisms.
* Supports protocols such as ARP and DHCP.

### 🖆 Broadcast Limitations

* Every device in the broadcast domain receives the frame.
* Excessive broadcast traffic can consume network resources.
* Large broadcast domains can increase unnecessary traffic.
* Broadcast traffic can contribute to network performance problems.

### 🖆 Broadcast Domain

A broadcast domain is a logical Layer 2 network in which broadcast frames can be delivered.

A router separates broadcast domains.

VLANs also create separate broadcast domains on VLAN-aware switches.

Example:

```text
                 Router
                   |
          +--------+--------+
          |                 |
       VLAN 10           VLAN 20
          |                 |
      +-------+         +-------+
      | Switch|         | Switch|
      +-------+         +-------+
          |                 |
       PC A, B            PC C, D

     Separate broadcast domains
```

A broadcast originating in VLAN 10 does not normally reach VLAN 20.

# 🗁 Multicast

Multicast is a communication method in which one sender transmits data to a selected group of receivers.

Unlike broadcast, multicast is not intended for every device in the broadcast domain.

Only devices that participate in the relevant multicast group are intended recipients.

Multicast is useful when the same data needs to be delivered to multiple receivers without creating a separate sender transmission for every receiver.

### 🖆 Multicast Communication Diagram

```text
                       +----------+
                       | Member A |
                       +----------+
                            ^
                            |
+----------+          +-----+-----+          +----------+
| Sender   | ------> |   Network | -------> | Member B |
+----------+          +-----+-----+          +----------+
                            |
                            v
                       +----------+
                       | Member C |
                       +----------+

                Multicast group members

              +----------------------+
              |      Device D        |
              | Not a group member   |
              +----------------------+
```

### 🖆 Multicast MAC Address

An Ethernet multicast MAC address identifies a group rather than one individual interface.

The Individual/Group (I/G) bit is set to `1`.

IPv4 multicast Ethernet MAC addresses use the prefix:

```text
01:00:5E
```

IPv6 multicast Ethernet MAC addresses use the prefix:

```text
33:33
```

### 🖆 IPv4 Multicast Addresses

IPv4 multicast addresses belong to the following address range:

```text
224.0.0.0 - 239.255.255.255
```

This range is also represented as:

```text
224.0.0.0/4
```

Examples:

| IPv4 Address | Purpose |
| --- | --- |
| 224.0.0.1 | All systems on the local subnet |
| 224.0.0.2 | All routers on the local subnet |
| 224.0.0.5 | OSPF AllSPFRouters |
| 224.0.0.6 | OSPF AllDRouters |
| 224.0.0.9 | RIPv2 routers |

The `224.0.0.0/24` range is reserved for local network control traffic and is not forwarded by multicast routers.

### 🖆 IPv6 Multicast Addresses

IPv6 multicast addresses use the prefix:

```text
FF00::/8
```

Examples:

| IPv6 Address | Purpose |
| --- | --- |
| FF02::1 | All nodes on the local link |
| FF02::2 | All routers on the local link |
| FF02::5 | OSPFv3 AllSPFRouters |
| FF02::6 | OSPFv3 AllDRouters |

IPv6 does not use broadcast addressing. It relies on multicast for several functions that use broadcast in IPv4.

### 🖆 Multicast Group

A multicast group is a collection of network interfaces that receive traffic addressed to a particular multicast group.

For example:

```text
Multicast Group:
239.1.1.1
```

Devices that join this group can receive traffic sent to the group address, subject to multicast routing and network configuration.

### 🖆 Multicast Group Membership

Hosts use multicast membership protocols to join and leave groups.

The main protocols are:

* IGMP (Internet Group Management Protocol) for IPv4.
* MLD (Multicast Listener Discovery) for IPv6.

These protocols help routers and switches determine which network segments have interested receivers.

### 🖆 IGMP

IGMP is used by IPv4 hosts and multicast routers to manage multicast group membership.

Common IGMP versions:

| Version | Key Feature |
| --- | --- |
| IGMPv1 | Basic multicast group membership |
| IGMPv2 | Adds group leave messages and improves membership handling |
| IGMPv3 | Supports source-specific multicast filtering |

### 🖆 IGMP Snooping

IGMP snooping is a Layer 2 switch feature.

It examines IGMP messages to identify which switch ports have multicast group members.

The switch can then forward multicast traffic only to interested ports instead of flooding it to every port in the VLAN.

Without suitable multicast-aware forwarding, multicast traffic may be flooded within a VLAN depending on switch behavior and configuration.

### 🖆 Common Multicast Applications

* IPTV.
* Live video distribution.
* Audio and video conferencing.
* Routing protocols.
* Network service discovery.
* Financial market data distribution.
* One-to-many data distribution.

### 🖆 Multicast Advantages

* One sender transmission can serve multiple receivers.
* Reduces duplicate traffic compared with separate unicast streams.
* Efficient for one-to-many distribution.
* Supports scalable media and data distribution when the network is configured correctly.

### 🖆 Multicast Limitations

* Requires multicast-aware network configuration.
* Group membership must be managed.
* Multicast routing may be required across different IP networks.
* Troubleshooting can be more complex than ordinary unicast communication.
* Network devices must support the required multicast features.

# 🗁 Ethernet MAC Address Comparison

Ethernet uses different destination MAC address types for unicast, multicast, and broadcast communication.

| Type | Destination MAC | I/G Bit |
| --- | --- | --- |
| Unicast | `00:11:22:33:44:55` | 0 |
| Multicast | `01:00:5E:00:00:01` | 1 |
| Broadcast | `FF:FF:FF:FF:FF:FF` | 1 |

### 🖆 Understanding the I/G Bit

The I/G bit is the least significant bit of the first octet.

For example:

```text
Unicast first octet: 00

Binary:
00000000

I/G bit:
0
```

```text
Multicast first octet: 01

Binary:
00000001

I/G bit:
1
```

```text
Broadcast first octet: FF

Binary:
11111111

I/G bit:
1
```

The I/G bit distinguishes individual addresses from group addresses.

A broadcast address is a special group address.

# 🗁 IPv4 Addressing Comparison

IPv4 supports unicast, broadcast, and multicast addressing.

| Type | Example | Description |
| --- | --- | --- |
| Unicast | `192.168.1.10` | One destination interface |
| Limited broadcast | `255.255.255.255` | All hosts on the local network |
| Directed broadcast | `192.168.1.255` | All hosts in a particular subnet |
| Multicast | `239.1.1.1` | A selected multicast group |

### 🖆 Does IPv6 Support Broadcast?

No.

IPv6 does not define broadcast addresses.

Instead, IPv6 uses multicast for communication with groups of interfaces.

For example:

```text
FF02::1
```

This multicast address identifies all IPv6 nodes on the local link.

This is one important architectural difference between IPv4 and IPv6.

# 🗁 Network Layer and Data Link Layer Behavior

Unicast, broadcast, and multicast can exist at different networking layers.

It is important to distinguish between an IP packet's destination address and an Ethernet frame's destination MAC address.

### 🖆 Layer 2 Delivery

Ethernet uses MAC addresses.

Examples:

```text
Unicast:
00:11:22:33:44:55

Multicast:
01:00:5E:00:00:01

Broadcast:
FF:FF:FF:FF:FF:FF
```

### 🖆 Layer 3 Delivery

IP uses IP addresses.

IPv4 supports:

* Unicast.
* Broadcast.
* Multicast.

IPv6 supports:

* Unicast.
* Multicast.
* Anycast.

IPv6 does not support broadcast.

### 🖆 Important Difference

An Ethernet broadcast frame and an IPv4 broadcast packet are related concepts, but they are not identical.

An IPv4 broadcast packet is normally encapsulated in an Ethernet broadcast frame when sent over Ethernet.

However, Layer 2 and Layer 3 addressing operate independently.

For example, a router may receive a unicast Ethernet frame containing an IP multicast packet. The Ethernet destination MAC and IP destination address serve different purposes.

# 🗁 Anycast

Anycast is another IP communication model, particularly important in IPv6 and modern network architectures.

With anycast, the same IP address is configured on multiple network interfaces or nodes.

Routing delivers traffic to one suitable destination, commonly the topologically nearest destination according to routing policy and metrics.

### 🖆 Anycast Communication Diagram

```text
                       +----------+
                       | Server A |
                       | 10.0.0.1 |
                       +----------+
                            ^
                            |
                       +----+----+
                       | Routing |
                       | Network |
                       +----+----+
                            |
             +--------------+--------------+
             |                             |
        +----------+                  +----------+
        | Server B |                  | Server C |
        | 10.0.0.1 |                  | 10.0.0.1 |
        +----------+                  +----------+

      Same IP address configured on multiple nodes.

      Routing selects one suitable destination.
```

### 🖆 Common Anycast Applications

* DNS infrastructure.
* Content delivery networks.
* Distributed service infrastructure.
* Global application delivery.

### 🖆 Comparison of Communication Models

| Model | Communication | Number of Intended Receivers |
| --- | --- | --- |
| Unicast | One-to-one | One |
| Broadcast | One-to-all | All devices in the broadcast domain |
| Multicast | One-to-group | Selected group members |
| Anycast | One-to-nearest suitable destination | One selected node |

# 🗁 Practical Examples

### 🖆 Example: Accessing a Website

When a computer accesses a website:

```text
Client
  |
  | Unicast request
  v
Web Server
```

The client normally sends unicast traffic to the server's IP address.

### 🖆 Example: Discovering a Gateway with ARP

A computer needs the MAC address associated with its default gateway.

```text
Client
  |
  | ARP Request
  | Destination MAC: FF:FF:FF:FF:FF:FF
  v
Local Broadcast Domain
```

The ARP request is broadcast to the local Layer 2 network.

### 🖆 Example: IPTV Distribution

An IPTV server sends a video stream to a multicast group.

```text
IPTV Server
     |
     | Multicast Stream
     v
Network
  /  |  \
 TV1 TV2 TV3
```

Only receivers that join the multicast group are intended to receive the stream.

### 🖆 Example: OSPF Routing

OSPF uses multicast addresses on certain network types to exchange routing information.

For IPv4:

```text
224.0.0.5
224.0.0.6
```

For IPv6:

```text
FF02::5
FF02::6
```

These addresses support communication between OSPF routers.

# 🗁 Common Mistakes

### 🖆 Confusing Broadcast with Multicast

Broadcast targets all devices in a broadcast domain.

Multicast targets members of a specific multicast group.

### 🖆 Assuming Broadcast Crosses Routers

Routers do not normally forward Layer 2 broadcast frames between interfaces.

They separate broadcast domains.

### 🖆 Assuming Multicast Automatically Reaches Every Network

Multicast forwarding across routers requires appropriate multicast routing and configuration.

Group membership and routing state affect delivery.

### 🖆 Assuming IPv6 Uses Broadcast

IPv6 does not support broadcast addresses.

It uses multicast for group communication and several discovery functions.

### 🖆 Confusing Ethernet Multicast with IP Multicast

Ethernet multicast uses multicast MAC addresses.

IP multicast uses multicast IP addresses.

They are related through encapsulation and address mapping, but they are different address types.

### 🖆 Assuming Every Switch Handles Multicast Efficiently

Multicast forwarding behavior depends on switch capabilities and configuration.

IGMP snooping and multicast routing may be necessary for efficient delivery.

# 🗁 Best Practices

* Understand the difference between unicast, broadcast, and multicast.
* Learn how MAC and IP addressing represent these communication models.
* Keep broadcast domains appropriately sized.
* Use VLANs to separate Layer 2 broadcast domains where appropriate.
* Understand ARP broadcast behavior in IPv4 networks.
* Learn IGMP and MLD for multicast membership management.
* Enable and configure IGMP snooping when required by the network design.
* Use multicast routing only where the network architecture requires it.
* Understand IPv6 multicast because IPv6 does not use broadcast.
* Monitor broadcast and multicast traffic when troubleshooting network performance.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify Address Types

Classify each address.

```text
00:11:22:33:44:55
FF:FF:FF:FF:FF:FF
01:00:5E:00:00:01
192.168.1.10
255.255.255.255
224.0.0.5
FF02::1
```

Determine whether each is:

* Unicast.
* Broadcast.
* Multicast.

Also identify whether it is a MAC address or an IP address.

### 🖆 Exercise: Inspect Network Interfaces

On Ubuntu, run:

```bash
ip link show
```

Identify the MAC address of your active interface.

Determine whether it is configured as a unicast or group address by inspecting the I/G bit of its first octet.

### 🖆 Exercise: Inspect Multicast Membership

Run:

```bash
ip maddr show
```

This displays multicast addresses associated with network interfaces.

Inspect the output and identify the multicast memberships.

### 🖆 Exercise: Inspect IPv4 Broadcast and Multicast Traffic

Install tcpdump:

```bash
sudo apt update
sudo apt install tcpdump
```

Capture Ethernet frames:

```bash
sudo tcpdump -i enp0s3 -e
```

Look for:

* Broadcast destination MAC addresses.
* Multicast destination MAC addresses.
* Unicast destination MAC addresses.

Replace `enp0s3` with your actual interface name.

### 🖆 Exercise: Inspect ARP Broadcast Traffic

Run:

```bash
sudo tcpdump -i enp0s3 -e arp
```

Generate local network traffic:

```bash
ping -c 3 192.168.1.1
```

Observe ARP requests and replies.

Identify the broadcast destination MAC address used by ARP requests.

### 🖆 Exercise: Explore IPv4 Multicast

Inspect multicast addresses configured on your interface:

```bash
ip maddr show
```

Explore the IPv4 multicast address range:

```text
224.0.0.0/4
```

Research the purpose of these addresses:

```text
224.0.0.1
224.0.0.2
224.0.0.5
224.0.0.6
224.0.0.9
```

# 🗁 Interview Questions

### 🖆 What is unicast communication?

Unicast is a one-to-one communication method in which data is sent from one source to one specific destination.

### 🖆 What is broadcast communication?

Broadcast is a one-to-all communication method in which traffic is delivered to all devices within a broadcast domain.

### 🖆 What is multicast communication?

Multicast is a one-to-group communication method in which traffic is delivered to selected members of a multicast group.

### 🖆 What is the Ethernet broadcast MAC address?

```text
FF:FF:FF:FF:FF:FF
```

### 🖆 What is the IPv4 limited broadcast address?

```text
255.255.255.255
```

### 🖆 What is the IPv4 multicast address range?

```text
224.0.0.0/4
```

### 🖆 Does IPv6 support broadcast?

No. IPv6 uses multicast for group communication instead of broadcast.

### 🖆 What is an Ethernet multicast MAC address?

It is a group MAC address used to deliver Ethernet frames to multiple network interfaces.

### 🖆 What is IGMP?

Internet Group Management Protocol. It manages IPv4 multicast group membership between hosts and multicast routers.

### 🖆 What is MLD?

Multicast Listener Discovery. It performs multicast group membership management for IPv6.

### 🖆 What is IGMP snooping?

IGMP snooping is a switch feature that examines IGMP messages to determine which ports have multicast group members and forward multicast traffic more selectively.

### 🖆 What is a broadcast domain?

A broadcast domain is a Layer 2 network in which broadcast frames can be delivered.

### 🖆 What separates broadcast domains?

Routers and VLAN boundaries separate broadcast domains.

### 🖆 What is anycast?

Anycast is an addressing and routing method in which multiple nodes share an IP address and routing delivers traffic to one suitable destination.

### 🖆 What is the difference between multicast and broadcast?

Broadcast targets all devices within a broadcast domain, whereas multicast targets only members of a selected multicast group.

# 🗁 References

* [RFC 919 - Broadcasting Internet Datagrams](https://www.rfc-editor.org/rfc/rfc919)
* [RFC 922 - Broadcasting Internet Datagrams in the Presence of Subnets](https://www.rfc-editor.org/rfc/rfc922)
* [RFC 1112 - Host Extensions for IP Multicasting](https://www.rfc-editor.org/rfc/rfc1112)
* [RFC 2236 - Internet Group Management Protocol, Version 2](https://www.rfc-editor.org/rfc/rfc2236)
* [RFC 3376 - Internet Group Management Protocol, Version 3](https://www.rfc-editor.org/rfc/rfc3376)
* [RFC 4291 - IP Version 6 Addressing Architecture](https://www.rfc-editor.org/rfc/rfc4291)
* [RFC 3810 - Multicast Listener Discovery Version 2](https://www.rfc-editor.org/rfc/rfc3810)
* [RFC 4541 - Considerations for Internet Group Management Protocol and Multicast Listener Discovery Snooping Switches](https://www.rfc-editor.org/rfc/rfc4541)
* [IANA IPv4 Multicast Address Space Registry](https://www.iana.org/assignments/multicast-addresses/multicast-addresses.xhtml)
* [Linux ip-maddress Manual](https://man7.org/linux/man-pages/man8/ip-maddress.8.html)
