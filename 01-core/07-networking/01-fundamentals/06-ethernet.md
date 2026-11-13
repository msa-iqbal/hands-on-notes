# Ethernet

Ethernet is a widely used networking technology for connecting devices within Local Area Networks (LANs).

It defines how devices communicate over wired network connections, including frame formatting, MAC addressing, media access, and physical transmission characteristics.

Ethernet is standardized primarily under the IEEE 802.3 family of standards.

It is commonly used in:

* Home networks.
* Office networks.
* Enterprise LANs.
* Data centers.
* ISP infrastructure.
* Campus networks.
* Industrial networks.

Ethernet provides the foundation for communication between computers, switches, routers, servers, and other network devices.

# 🗁 What Is Ethernet?

### 🖆 Definition

Ethernet is a family of wired networking technologies that defines how devices exchange data over local network connections.

It primarily operates at two layers of the OSI model:

* **Physical Layer (Layer 1):** Defines electrical, optical, or other physical signaling characteristics.
* **Data Link Layer (Layer 2):** Defines Ethernet frame formatting, MAC addressing, and media access behavior.

Ethernet uses frames to transport data across a local network.

A typical Ethernet network contains:

* End devices.
* Network Interface Cards (NICs).
* Ethernet cables or fiber links.
* Ethernet switches.
* Routers or Layer 3 switches.

### 🖆 Basic Ethernet Communication

Consider two computers connected to the same Ethernet switch.

<pre>
       PC-01                         PC-02
  192.168.1.10                  192.168.1.20
  MAC: AA:AA:01                 MAC: BB:BB:02
        |                            |
        | Ethernet                   | Ethernet
        |                            |
        +---------- SWITCH ----------+
</pre>

When PC-01 sends data to PC-02:

* PC-01 creates an Ethernet frame.
* The frame contains source and destination MAC addresses.
* The switch receives the frame.
* The switch examines the destination MAC address.
* The switch forwards the frame through the appropriate port.
* PC-02 receives and processes the frame.

Ethernet handles local frame delivery. IP handles logical addressing and communication between networks.

# 🗁 History of Ethernet

### 🖆 Development

Ethernet was developed at Xerox PARC in the 1970s.

It became one of the most widely adopted LAN technologies because of its simplicity, scalability, and compatibility.

Important milestones include:

| Year | Development |
| --- | --- |
| 1973 | Early Ethernet concept developed at Xerox PARC |
| 1980 | Original DIX Ethernet specification published |
| 1983 | IEEE 802.3 standard published |
| 1990 | 10BASE-T introduced |
| 1995 | Fast Ethernet standardized |
| 1999 | Gigabit Ethernet over fiber and copper standardized |
| 2002 | 10 Gigabit Ethernet over fiber standardized |
| 2006 | 10GBASE-T standardized |
| 2010 | 40 Gigabit and 100 Gigabit Ethernet standards introduced |
| 2017 | 200 Gigabit and 400 Gigabit Ethernet standards introduced |
| 2022 | 800 Gigabit Ethernet standardization progressed |

Ethernet has evolved from shared coaxial networks to modern switched, full-duplex networks operating at hundreds of gigabits per second.

# 🗁 IEEE 802.3 Standards

### 🖆 What Is IEEE 802.3?

IEEE 802.3 is the family of standards that defines Ethernet operation.

It specifies important characteristics such as:

* Ethernet frame format.
* MAC operation.
* Physical-layer signaling.
* Transmission media.
* Data rates.
* Link operation.

Different IEEE 802.3 amendments define Ethernet technologies for different speeds and transmission media.

### 🖆 Common Ethernet Standards

| Standard | Speed | Medium | Typical Maximum Distance |
| --- | ---: | --- | ---: |
| 10BASE-T | 10 Mbps | Twisted pair | 100 m |
| 100BASE-TX | 100 Mbps | Twisted pair | 100 m |
| 1000BASE-T | 1 Gbps | Twisted pair | 100 m |
| 2.5GBASE-T | 2.5 Gbps | Twisted pair | 100 m |
| 5GBASE-T | 5 Gbps | Twisted pair | 100 m |
| 10GBASE-T | 10 Gbps | Twisted pair | 100 m with appropriate cabling |
| 10GBASE-SR | 10 Gbps | Multimode fiber | Depends on fiber type |
| 10GBASE-LR | 10 Gbps | Single-mode fiber | Up to 10 km |
| 25GBASE-SR | 25 Gbps | Multimode fiber | Depends on fiber type |
| 40GBASE-SR4 | 40 Gbps | Multimode fiber | Depends on fiber type |
| 100GBASE-LR4 | 100 Gbps | Single-mode fiber | Up to 10 km |

The maximum distance depends on the specific Ethernet PHY, cabling category, optical transceiver, and installation conditions.

### 🖆 Understanding Ethernet Naming

Consider the standard:

```text
1000BASE-T
```

Its components represent:

* `1000`: 1000 Mbps or 1 Gbps.
* `BASE`: Baseband signaling.
* `T`: Twisted-pair copper medium.

Another example:

```text
10GBASE-SR
```

* `10G`: 10 Gigabit Ethernet.
* `BASE`: Baseband signaling.
* `SR`: Short Reach optical transmission.

Common optical suffixes include:

| Suffix | Meaning |
| --- | --- |
| SR | Short Reach |
| LR | Long Reach |
| ER | Extended Reach |
| ZR | Extended-reach terminology used in certain optical products and specifications |

# 🗁 Ethernet Frame

### 🖆 What Is an Ethernet Frame?

An Ethernet frame is the Layer 2 data unit used to transport information across an Ethernet network.

It contains addressing information, payload data, and error-detection information.

A basic Ethernet frame has the following structure:

<pre>
+----------+-----+----------+----------+---------+------+
| Preamble | SFD | Dest MAC | Src MAC  | Type/   | FCS  |
|          |     | Address  | Address  | Length  |      |
+----------+-----+----------+----------+---------+------+
                             |
                             v
                         Payload
</pre>

A more complete conceptual representation is:

```text
+----------+-----+----------------+----------------+----------------+----------+-----+
| Preamble | SFD | Destination MAC| Source MAC     | Type/Length    | Payload  | FCS |
+----------+-----+----------------+----------------+----------------+----------+-----+
   7 bytes 1 byte    6 bytes         6 bytes          2 bytes       46-1500*  4 bytes
```

*The traditional minimum and maximum Ethernet payload sizes shown here apply to standard untagged Ethernet frames. VLAN tags and other extensions affect the total frame size.*

### 🖆 Ethernet Frame Size

For standard Ethernet:

| Component | Size |
| --- | ---: |
| Preamble | 7 bytes |
| Start Frame Delimiter | 1 byte |
| Destination MAC | 6 bytes |
| Source MAC | 6 bytes |
| Type/Length | 2 bytes |
| Minimum Payload | 46 bytes |
| Maximum Standard Payload | 1500 bytes |
| Frame Check Sequence | 4 bytes |

The traditional Ethernet frame size is:

* Minimum: 64 bytes.
* Maximum: 1518 bytes.

These frame sizes are measured from the Destination MAC address through the Frame Check Sequence. The preamble and SFD are not included.

A VLAN-tagged Ethernet frame normally adds four bytes, making its maximum frame size 1522 bytes under the conventional 1500-byte payload limit.

# 🗁 Ethernet Frame Fields

### 🖆 Preamble

The preamble is a 7-byte field transmitted before the Ethernet frame.

Its purpose is to help the receiving interface synchronize with the incoming signal.

It consists of a repeating bit pattern.

### 🖆 Start Frame Delimiter (SFD)

The Start Frame Delimiter is a 1-byte field that identifies the beginning of the actual Ethernet frame.

It follows the preamble.

### 🖆 Destination MAC Address

The Destination MAC Address identifies the intended receiving interface.

Example:

```text
BB:BB:BB:BB:BB:02
```

It may represent:

* A unicast destination.
* A multicast destination.
* A broadcast destination.

### 🖆 Source MAC Address

The Source MAC Address identifies the transmitting interface.

Example:

```text
AA:AA:AA:AA:AA:01
```

Switches use source MAC addresses to learn which devices are reachable through their ports.

### 🖆 Type/Length Field

This 2-byte field has two possible interpretations depending on its value.

**EtherType**

When the value is at least `1536` decimal (`0x0600`), it identifies the upper-layer protocol.

Common EtherTypes include:

| EtherType | Protocol |
| --- | --- |
| `0x0800` | IPv4 |
| `0x0806` | ARP |
| `0x86DD` | IPv6 |
| `0x8100` | IEEE 802.1Q VLAN tagging |
| `0x88CC` | LLDP |

**Length**

Values from 1500 decimal (`0x05DC`) and below can represent the payload length in IEEE 802.3 framing.

The intermediate range between 1501 and 1535 is reserved.

### 🖆 Payload

The payload contains the data being transported.

It may contain:

* IPv4 packets.
* IPv6 packets.
* ARP messages.
* Other supported Layer 2 protocol data.

For standard Ethernet, the payload is generally between 46 and 1500 bytes.

Smaller payloads are padded to meet the minimum frame size.

### 🖆 Frame Check Sequence (FCS)

The FCS is a 4-byte field used for error detection.

Ethernet uses a 32-bit Cyclic Redundancy Check (CRC-32).

The receiving device calculates the CRC and compares it with the received FCS.

If the frame fails the CRC check, it is normally discarded.

FCS detects transmission errors but does not correct them.

# 🗁 MAC Addressing

### 🖆 What Is a MAC Address?

A Media Access Control (MAC) address is a Layer 2 address used to identify network interfaces within Ethernet communication.

A conventional Ethernet MAC address is 48 bits long.

Example:

```text
00:1A:2B:3C:4D:5E
```

It is commonly written using:

* Hexadecimal notation.
* Six groups of two hexadecimal digits.
* Colon or hyphen separators.

### 🖆 MAC Address Structure

A traditional globally administered MAC address is divided into two main parts:

```text
+--------------------------+--------------------------+
| Organizational Identifier| Device Identifier        |
| First 24 Bits            | Last 24 Bits             |
+--------------------------+--------------------------+
```

The first 24 bits commonly identify the assigned organization through an IEEE-assigned Organizationally Unique Identifier (OUI).

The remaining 24 bits identify the interface within that assignment.

Actual MAC address allocation formats include additional conventions, including locally administered addresses.

### 🖆 Unicast MAC Address

A unicast MAC address identifies an individual destination interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

The least significant bit of the first octet is zero for an individual address.

### 🖆 Multicast MAC Address

A multicast MAC address identifies a group of receiving interfaces.

An Ethernet multicast address has the Individual/Group bit set to one.

Example:

```text
01:00:5E:00:00:01
```

This address belongs to the IPv4 multicast Ethernet address range.

### 🖆 Broadcast MAC Address

The Ethernet broadcast address is:

```text
FF:FF:FF:FF:FF:FF
```

It delivers an Ethernet frame to all interfaces within the applicable Layer 2 broadcast domain.

Switches normally flood broadcast frames through eligible ports in the same VLAN, excluding the incoming port.

# 🗁 Ethernet Communication Types

### 🖆 Unicast Communication

Unicast communication sends data from one source to one destination.

Example:

```text
PC-01  --------->  PC-02
```

The destination MAC address identifies one receiving interface.

### 🖆 Broadcast Communication

Broadcast communication sends a frame to every device in the applicable Layer 2 broadcast domain.

Example:

```text
                  PC-01
                    |
                    |
                 SWITCH
                /  |  \
               /   |   \
             PC-02 PC-03 PC-04
```

The broadcast destination MAC address is:

```text
FF:FF:FF:FF:FF:FF
```

### 🖆 Multicast Communication

Multicast communication sends data to a selected group of network interfaces.

Example:

```text
                  SOURCE
                     |
                   SWITCH
                 /   |   \
                /    |    \
             GROUP  GROUP  OTHER
             MEMBER MEMBER DEVICE
```

Only interfaces interested in the multicast group should process the traffic, although Layer 2 forwarding behavior depends on switch configuration and multicast membership information.

# 🗁 Ethernet Media Access

### 🖆 What Is Media Access Control?

Media Access Control defines how devices access a communication medium and transmit data at the Data Link layer.

Ethernet uses MAC mechanisms for frame transmission and addressing.

Traditional shared Ethernet used Carrier Sense Multiple Access with Collision Detection (CSMA/CD).

Modern switched full-duplex Ethernet generally does not use CSMA/CD.

### 🖆 CSMA/CD

CSMA/CD stands for:

* Carrier Sense.
* Multiple Access.
* Collision Detection.

It was used in traditional shared, half-duplex Ethernet networks.

The process works as follows:

* A device listens to the medium.
* If the medium is idle, it begins transmitting.
* If another device transmits simultaneously, a collision may occur.
* The transmitting devices detect the collision.
* They stop transmitting and send a jam signal.
* Each device waits for a randomized backoff interval.
* Transmission is attempted again.

### 🖆 Binary Exponential Backoff

After a collision, Ethernet uses a randomized waiting mechanism.

The backoff interval increases according to the number of collisions, within the algorithm's defined limits.

This helps reduce repeated collisions when multiple devices attempt to transmit simultaneously.

### 🖆 Modern Ethernet

Modern Ethernet commonly uses:

* Dedicated switch ports.
* Full-duplex communication.
* Separate transmit and receive paths.
* No shared collision domain between switched ports.

Therefore, CSMA/CD is not used during normal full-duplex switched Ethernet operation.

# 🗁 Collision Domains

### 🖆 What Is a Collision Domain?

A collision domain is a network segment in which simultaneous transmissions can interfere with one another.

Collisions were an important concern in shared, half-duplex Ethernet networks.

### 🖆 Hub Collision Domain

All devices connected to a traditional Ethernet hub share one collision domain.

```text
        PC-01
          |
        +-----+
        | HUB |
        +-----+
       /   |   \
     PC-02 PC-03 PC-04

     One Collision Domain
```

### 🖆 Switch Collision Domains

Each port on a traditional Ethernet switch provides a separate collision domain.

```text
             SWITCH
          /     |     \
         /      |      \
       PC-01  PC-02  PC-03

       Separate Collision Domains
```

In modern full-duplex switched Ethernet, collisions do not occur during normal operation.

### 🖆 Broadcast Domains

A broadcast domain is the collection of Layer 2 interfaces that receive a particular broadcast.

A Layer 2 switch forwards broadcasts within the applicable VLAN.

Different VLANs represent separate broadcast domains unless Layer 3 routing or another forwarding mechanism connects them.

# 🗁 Duplex Modes

### 🖆 What Is Duplex?

Duplex describes whether communication can occur in one or both directions over a network connection.

Ethernet supports two traditional duplex modes.

### 🖆 Half-Duplex

Half-duplex communication allows data transmission in both directions, but not simultaneously.

A device must wait until the medium is available before transmitting.

Characteristics:

* Communication occurs in both directions.
* Only one direction transmits at a time.
* Traditional shared Ethernet used collision detection.
* Collisions can occur.

### 🖆 Full-Duplex

Full-duplex communication allows devices to transmit and receive simultaneously.

Characteristics:

* Simultaneous bidirectional communication.
* No normal Ethernet collisions.
* Higher efficiency than half-duplex operation.
* Commonly used in modern switched Ethernet.

### 🖆 Half-Duplex vs Full-Duplex

| Feature | Half-Duplex | Full-Duplex |
| --- | --- | --- |
| Communication | Both directions, one at a time | Both directions simultaneously |
| Collisions | Possible on shared media | No normal collisions |
| CSMA/CD | Applicable to traditional shared Ethernet | Not used |
| Performance | Lower efficiency | Higher efficiency |
| Modern Usage | Legacy and specialized environments | Standard modern switched Ethernet |

# 🗁 Ethernet Speed and Negotiation

### 🖆 Ethernet Link Speed

Ethernet devices communicate using a negotiated or configured link speed.

Common Ethernet speeds include:

* 10 Mbps.
* 100 Mbps.
* 1 Gbps.
* 2.5 Gbps.
* 5 Gbps.
* 10 Gbps.
* 25 Gbps.
* 40 Gbps.
* 100 Gbps.
* 400 Gbps and higher.

The actual supported speed depends on the NIC, switch port, cable, transceiver, and Ethernet standard.

### 🖆 Auto-Negotiation

Auto-negotiation allows two connected Ethernet interfaces to exchange supported capabilities and select a compatible operating mode.

It can negotiate:

* Link speed.
* Duplex mode.
* Certain physical-layer capabilities.

For example:

```text
NIC:
  10 Mbps
  100 Mbps
  1 Gbps

Switch:
  10 Mbps
  100 Mbps
  1 Gbps

Negotiated Speed:
  1 Gbps
```

The selected speed is the highest mutually supported mode permitted by the link and configuration.

### 🖆 Speed and Duplex Mismatch

A speed or duplex mismatch can cause network performance problems.

Possible symptoms:

* Slow network communication.
* Packet errors.
* Frame retransmissions at higher layers.
* CRC errors.
* Late collisions in legacy half-duplex environments.
* Poor application performance.

In modern networks, allowing compatible auto-negotiation on both ends is generally recommended unless a specific design requires manual configuration.

### 🖆 Linux Ethernet Link Information

Install `ethtool`:

```bash
sudo apt update
sudo apt install ethtool
```

Display link information:

```bash
sudo ethtool eth0
```

Example output:

```text
Speed: 1000Mb/s
Duplex: Full
Auto-negotiation: on
Link detected: yes
```

Replace `eth0` with the actual network interface name.

# 🗁 Ethernet Cabling

### 🖆 Copper Ethernet

Copper Ethernet commonly uses twisted-pair cables.

Examples include:

* Cat 5e.
* Cat 6.
* Cat 6A.

Common connectors use the 8P8C modular connector, frequently called RJ45.

### 🖆 Fiber Ethernet

Fiber Ethernet uses optical transceivers and fiber-optic cables.

Examples include:

* 1000BASE-SX.
* 1000BASE-LX.
* 10GBASE-SR.
* 10GBASE-LR.
* 100GBASE-SR4.
* 100GBASE-LR4.

### 🖆 Ethernet Port Types

Common Ethernet interfaces include:

* RJ45 copper ports.
* SFP ports.
* SFP+ ports.
* SFP28 ports.
* QSFP+ ports.
* QSFP28 ports.

The port form factor does not by itself guarantee a particular Ethernet speed or optical compatibility.

Always verify supported transceivers and interface standards.

# 🗁 VLAN and Ethernet

### 🖆 What Is a VLAN?

A Virtual LAN (VLAN) logically separates a Layer 2 network into multiple broadcast domains.

VLANs allow multiple logical networks to operate over the same physical switching infrastructure.

Example:

```text
                 SWITCH
                    |
          +---------+---------+
          |                   |
       VLAN 10              VLAN 20
          |                   |
       PC-01               PC-02
       PC-03               PC-04
```

VLAN 10 and VLAN 20 are separate broadcast domains.

### 🖆 IEEE 802.1Q Tagging

IEEE 802.1Q defines VLAN tagging for Ethernet frames.

A VLAN tag is inserted into the Ethernet frame between the Source MAC address and the original Type/Length field.

The tag contains:

* Tag Protocol Identifier (TPID).
* Tag Control Information (TCI).

The TCI includes:

* Priority Code Point (PCP).
* Drop Eligible Indicator (DEI).
* VLAN Identifier (VID).

The VLAN Identifier is 12 bits long.

This provides 4096 possible VID values, although VLAN IDs 0 and 4095 are reserved.

The commonly usable VLAN ID range is 1–4094.

### 🖆 Access Ports

An access port generally carries traffic for one configured VLAN.

Connected endpoint devices typically send and receive untagged Ethernet frames on an access port.

The switch associates those frames with the configured VLAN.

### 🖆 Trunk Ports

A trunk port carries traffic for multiple VLANs.

It commonly uses IEEE 802.1Q tags to identify VLAN membership.

Trunks are frequently used between:

* Switches.
* Switches and routers.
* Switches and firewalls.
* Switches and virtualization hosts.

# 🗁 MTU and Ethernet

### 🖆 What Is MTU?

MTU stands for Maximum Transmission Unit.

It defines the maximum size of a Layer 3 packet that can be carried by a network interface without fragmentation at that layer.

For conventional Ethernet, the standard IP MTU is commonly 1500 bytes.

This means an Ethernet payload can carry an IP packet of up to 1500 bytes under standard Ethernet configuration.

### 🖆 Standard Ethernet MTU

For an IPv4 packet:

```text
Ethernet Payload
+------------------------------+
| IPv4 Header | Transport Data |
+------------------------------+
          1500 bytes
```

If the IPv4 header is 20 bytes:

\[
1500 - 20 = 1480 \text{ bytes}
\]

The remaining 1480 bytes are available for the transport-layer payload, assuming no additional IP options.

For a typical TCP connection without additional TCP options:

\[
1500 - 20 - 20 = 1460 \text{ bytes}
\]

This is the commonly encountered TCP Maximum Segment Size (MSS) for a 1500-byte IPv4 MTU with standard IP and TCP headers.

### 🖆 Jumbo Frames

Jumbo frames are Ethernet frames larger than the conventional standard frame size.

They are commonly used in environments such as:

* Data centers.
* Storage networks.
* High-performance computing.

A common configuration uses an IP MTU of 9000 bytes.

Jumbo frames require compatible support across the entire Layer 2 path.

# 🗁 Ethernet vs Wi-Fi

### 🖆 Comparison

| Feature | Ethernet | Wi-Fi |
| --- | --- | --- |
| Medium | Wired | Wireless |
| Standard | IEEE 802.3 | IEEE 802.11 |
| Reliability | Generally consistent | Affected by radio conditions |
| Interference | Less susceptible to radio interference | Susceptible to RF interference |
| Mobility | Limited by cable | Supports mobility |
| Latency | Generally lower and more predictable | More variable |
| Security | Physical access and network controls | Wireless authentication and encryption |
| Installation | Requires cabling | Requires wireless infrastructure |

### 🖆 When to Use Ethernet

Ethernet is commonly preferred for:

* Servers.
* Desktop computers.
* Network switches.
* High-performance workstations.
* Gaming systems requiring predictable connectivity.
* Network infrastructure links.

### 🖆 When to Use Wi-Fi

Wi-Fi is useful for:

* Smartphones.
* Tablets.
* Laptops.
* Mobile devices.
* Guest networks.
* Devices where installing cables is impractical.

Most modern networks combine Ethernet and Wi-Fi.

# 🗁 Ethernet Troubleshooting

### 🖆 No Physical Link

Symptoms:

* Ethernet interface shows no carrier.
* Switch port LED remains inactive.
* Network connection is unavailable.

Possible causes:

* Damaged Ethernet cable.
* Incorrect cable termination.
* Disabled switch port.
* Faulty NIC.
* Faulty switch port.
* Unsupported or incorrectly installed transceiver.

Linux command:

```bash
ip link show
```

Check the physical link:

```bash
sudo ethtool eth0
```

### 🖆 Incorrect Link Speed

Symptoms:

* 1 Gbps connection negotiates at 100 Mbps.
* Unexpectedly low throughput.
* Unstable connection.

Possible causes:

* Damaged cable pairs.
* Incorrect termination.
* Unsupported NIC or switch speed.
* Faulty transceiver.
* Incorrect configuration.

Inspect the negotiated speed:

```bash
sudo ethtool eth0
```

### 🖆 High Packet Errors

Check interface statistics:

```bash
ip -s link show eth0
```

Inspect detailed interface counters:

```bash
sudo ethtool -S eth0
```

Possible causes include:

* Cable problems.
* Electrical interference.
* Faulty hardware.
* Duplex or speed mismatch.
* Transceiver problems.

### 🖆 IP Connectivity Problems

Check IP configuration:

```bash
ip addr show
```

Check the routing table:

```bash
ip route
```

Test the default gateway:

```bash
ping -c 4 192.168.1.1
```

Test an external IP address:

```bash
ping -c 4 1.1.1.1
```

Test DNS resolution:

```bash
getent hosts example.com
```

### 🖆 Inspect Ethernet Traffic

Capture packets with `tcpdump`:

```bash
sudo tcpdump -i eth0
```

Capture only ARP traffic:

```bash
sudo tcpdump -i eth0 arp
```

Capture ICMP traffic:

```bash
sudo tcpdump -i eth0 icmp
```

Capture Ethernet frames and display their link-layer headers:

```bash
sudo tcpdump -i eth0 -e
```

# 🗁 Practical Exercises

### 🖆 Exercise: Inspect Ethernet Interfaces

On Ubuntu, list network interfaces:

```bash
ip link
```

Display interface addresses:

```bash
ip addr
```

Display interface statistics:

```bash
ip -s link
```

Identify:

* Interface name.
* MAC address.
* IP address.
* Interface state.
* MTU.
* RX packet count.
* TX packet count.
* RX and TX errors.

### 🖆 Exercise: Inspect Ethernet Link Speed

Install `ethtool`:

```bash
sudo apt install ethtool
```

Check link details:

```bash
sudo ethtool eth0
```

Record:

* Supported link modes.
* Advertised link modes.
* Speed.
* Duplex.
* Auto-negotiation status.
* Link detection status.

### 🖆 Exercise: Capture Ethernet Frames

Install Wireshark and tcpdump:

```bash
sudo apt update
sudo apt install wireshark tcpdump
```

Start packet capture:

```bash
sudo tcpdump -i eth0 -e -n
```

Generate network traffic:

```bash
ping -c 4 8.8.8.8
```

Observe:

* Source MAC address.
* Destination MAC address.
* Ethernet frame type.
* IPv4 or IPv6 packets.
* ICMP messages.

### 🖆 Exercise: Build a Switched Ethernet LAN

Create a topology with:

* One Ethernet switch.
* Three computers.
* Ethernet connections between all devices and the switch.

Assign these IP addresses:

| Device | IP Address | Subnet Mask |
| --- | --- | --- |
| PC-01 | 192.168.10.10 | 255.255.255.0 |
| PC-02 | 192.168.10.20 | 255.255.255.0 |
| PC-03 | 192.168.10.30 | 255.255.255.0 |

Test communication using:

```bash
ping -c 4 192.168.10.20
```

Inspect the ARP cache:

```bash
ip neigh
```

Observe MAC address learning on the switch.

### 🖆 Exercise: Configure VLANs

Create two VLANs:

* VLAN 10 for administration.
* VLAN 20 for development.

Connect devices to their respective VLANs.

Verify that devices in the same VLAN can communicate and that communication between different VLANs requires Layer 3 routing.

### 🖆 Exercise: Investigate Ethernet MTU

Check the current interface MTU:

```bash
ip link show eth0
```

Test IPv4 path MTU behavior using ICMP:

```bash
ping -M do -s 1472 -c 4 8.8.8.8
```

The packet contains:

* 1472 bytes of ICMP data.
* 8 bytes of ICMP header.
* 20 bytes of IPv4 header.

Total:

\[
1472 + 8 + 20 = 1500 \text{ bytes}
\]

The test may fail if an intermediate network path has a smaller MTU or blocks the required ICMP messages.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is Ethernet?**

Ethernet is a family of wired networking technologies standardized primarily under IEEE 802.3.

**Which OSI layers does Ethernet operate at?**

Ethernet primarily operates at the Physical layer and Data Link layer.

**What is an Ethernet frame?**

An Ethernet frame is the Layer 2 data unit used to transport information over an Ethernet network.

**What is the standard Ethernet MTU?**

The conventional IP MTU for standard Ethernet is 1500 bytes.

**What is the Ethernet broadcast MAC address?**

```text
FF:FF:FF:FF:FF:FF
```

### 🖆 Intermediate Questions

**What is the purpose of the Ethernet FCS field?**

It provides error detection using CRC-32.

**What is the difference between half-duplex and full-duplex Ethernet?**

Half-duplex allows transmission in both directions but not simultaneously. Full-duplex supports simultaneous bidirectional communication.

**What is CSMA/CD?**

Carrier Sense Multiple Access with Collision Detection is the traditional Ethernet media access mechanism used in shared half-duplex environments.

**Why are collisions uncommon in modern Ethernet networks?**

Modern switched Ethernet generally uses dedicated full-duplex links, eliminating normal Ethernet collisions.

**What is the difference between unicast, broadcast, and multicast?**

Unicast targets one destination, broadcast targets all devices in a broadcast domain, and multicast targets a selected group of devices.

**What is auto-negotiation?**

It allows connected Ethernet interfaces to exchange capabilities and select a compatible link operating mode.

### 🖆 Advanced Questions

**What is the difference between Ethernet MTU and Ethernet frame size?**

MTU commonly refers to the maximum Layer 3 packet size carried in the Ethernet payload. Ethernet frame size includes the Layer 2 header and FCS, excluding the preamble and SFD.

**Why does an Ethernet switch learn source MAC addresses?**

It builds a forwarding database that associates MAC addresses with switch ports and VLANs.

**What happens when a switch receives an unknown unicast frame?**

It normally floods the frame through eligible ports in the same VLAN, excluding the incoming port.

**What is the purpose of IEEE 802.1Q?**

It defines VLAN tagging for Ethernet frames, allowing multiple VLANs to share a physical trunk link.

**What happens when Ethernet CRC validation fails?**

The receiving Ethernet interface normally discards the corrupted frame.

**Does Ethernet guarantee packet delivery?**

No. Ethernet provides link-layer frame transmission and error detection, but it does not guarantee end-to-end delivery. Higher-layer protocols such as TCP provide reliability mechanisms.

**What is the difference between 1000BASE-T and 10GBASE-T?**

1000BASE-T supports 1 Gbps Ethernet over twisted-pair copper, while 10GBASE-T supports 10 Gbps Ethernet over suitable twisted-pair cabling.

# 🗁 References

* [IEEE 802.3 Ethernet Standard](https://standards.ieee.org/ieee/802.3/10422/)
* [IEEE 802.1Q VLAN Standard](https://standards.ieee.org/ieee/802.1Q/6847/)
* [Cisco Ethernet Technology](https://www.cisco.com/)
* [Wireshark Documentation](https://www.wireshark.org/docs/)
* [Linux ip Command Manual](https://man7.org/linux/man-pages/man8/ip.8.html)
* [Linux ethtool Manual](https://man7.org/linux/man-pages/man8/ethtool.8.html)
* [Linux tcpdump Manual](https://man7.org/linux/man-pages/man8/tcpdump.8.html)
* [IEEE Standards Association](https://standards.ieee.org/)
