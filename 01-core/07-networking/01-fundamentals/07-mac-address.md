# MAC Address

A **MAC address (Media Access Control address)** is a hardware-level identifier used to identify a network interface within a local network.

MAC addresses operate at the **Data Link Layer (Layer 2)** of the OSI model. Ethernet switches use MAC addresses to forward Ethernet frames to the correct network interfaces.

Every network interface that communicates over an Ethernet or Wi-Fi network has a MAC address.

For example:

```text
00:1A:2B:3C:4D:5E
```

A MAC address is commonly represented as six groups of two hexadecimal digits, separated by colons.

### 🖆 What You Will Learn

* What a MAC address is and why it is needed.
* How MAC addresses are structured.
* How manufacturers are associated with MAC addresses.
* The difference between unicast, multicast, and broadcast MAC addresses.
* How switches learn and use MAC addresses.
* The difference between MAC addresses and IP addresses.
* How to inspect and manage MAC addresses in Linux.
* How MAC addresses work in practical network communication.

# 🗁 Introduction

A MAC address identifies a network interface at the Data Link Layer.

When two devices communicate over the same Ethernet network, they use MAC addresses to deliver Ethernet frames between their network interfaces.

Consider two computers connected to the same switch:

```text
Computer A                         Computer B
192.168.1.10                       192.168.1.20
00:11:22:33:44:AA                  00:11:22:33:44:BB
     |                                  |
     +------------- Switch -------------+
```

Computer A uses Computer B's MAC address as the destination MAC address when sending an Ethernet frame directly to Computer B.

The switch examines the destination MAC address and forwards the frame through the appropriate port.

### 🖆 Why MAC Addresses Are Needed

MAC addresses provide several important functions:

* Identify network interfaces on a local network.
* Support Ethernet frame delivery.
* Allow switches to learn which devices are connected to which ports.
* Support Layer 2 network communication.
* Help network protocols map network-layer addresses to link-layer addresses.

### 🖆 Is a MAC Address Always Permanent?

Not necessarily.

A MAC address is often assigned by the network interface manufacturer, but modern operating systems and network configurations can override it.

For example:

* A system administrator can configure a different MAC address.
* A Linux network manager can use a randomized MAC address.
* Virtual machines can have software-generated MAC addresses.
* Some Wi-Fi devices use randomized MAC addresses for privacy.

Therefore, a MAC address should not be treated as an unchangeable or globally reliable device identity.

# 🗁 MAC Address Structure

A traditional Ethernet MAC address is 48 bits long, equivalent to 6 bytes.

Each byte contains 8 bits.

```text
48 bits = 6 bytes = 12 hexadecimal digits
```

Example:

```text
00:1A:2B:3C:4D:5E
```

The same address can be represented in different formats:

```text
00:1A:2B:3C:4D:5E
00-1A-2B-3C-4D-5E
001A.2B3C.4D5E
001A2B3C4D5E
```

All these formats represent the same 48-bit value.

### 🖆 Hexadecimal Representation

MAC addresses use hexadecimal notation.

Hexadecimal is a base-16 numbering system that uses:

```text
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

Each hexadecimal digit represents four bits.

For example:

```text
A = 1010
B = 1011
C = 1100
```

A MAC address contains 12 hexadecimal digits:

```text
00:1A:2B:3C:4D:5E
```

Each pair represents one byte.

### 🖆 Binary Representation

The MAC address can also be represented in binary:

```text
00000000:00011010:00101011:00111100:01001101:01011110
```

This representation is useful when studying MAC address bits, particularly the individual/group and universal/local bits.

### 🖆 MAC Address Length

| Property | Value |
| --- | --- |
| Address length | 48 bits |
| Total bytes | 6 |
| Bits per byte | 8 |
| Hexadecimal digits | 12 |
| Hexadecimal base | 16 |
| OSI layer | Layer 2 |
| Common technology | Ethernet and Wi-Fi |

# 🗁 OUI and NIC-Specific Identifier

A traditional MAC address is divided into two logical parts:

* Organizationally Unique Identifier (OUI).
* Interface-specific portion.

For example:

```text
00:1A:2B:3C:4D:5E
```

Conceptually:

```text
00:1A:2B | 3C:4D:5E
    OUI  | Interface-specific portion
```

The first 24 bits identify an assigned organization or address block. The remaining 24 bits are used to identify an interface within that block.

### 🖆 Organizationally Unique Identifier (OUI)

The OUI is a 24-bit identifier assigned by the IEEE Registration Authority.

It identifies the organization associated with an assigned MAC address block.

Examples of organizations with registered address assignments include network equipment manufacturers and computer hardware vendors.

The OUI can sometimes help identify the manufacturer associated with a network interface.

However, it does not reliably identify the current owner or physical device because MAC addresses can be changed, randomized, or locally administered.

### 🖆 Interface-Specific Portion

The remaining 24 bits distinguish addresses within the assigned block.

A manufacturer or organization uses this portion according to its address allocation practices.

Example:

```text
00:1A:2B:3C:4D:5E
         └──────── Interface-specific portion
```

Not every MAC address follows this traditional manufacturer-assigned structure. Locally administered addresses and other IEEE address allocation formats may use the bits differently.

### 🖆 Important MAC Address Bits

The first octet contains two important bits:

* **I/G bit:** Individual/Group bit.
* **U/L bit:** Universal/Local bit.

These bits determine the address type and administration scope.

| Bit | Meaning | Description |
| --- | --- | --- |
| I/G = 0 | Individual | Unicast address |
| I/G = 1 | Group | Multicast address |
| U/L = 0 | Universal | Universally administered address |
| U/L = 1 | Local | Locally administered address |

The least significant bit of the first octet is the I/G bit. The second least significant bit is the U/L bit.

For example, the first octet `02` in binary is:

```text
00000010
       ^^
       ||
       |+-- U/L = 1 (Locally administered)
       +--- I/G = 0 (Individual / Unicast)
```

Therefore, a MAC address beginning with `02` is an example of a locally administered unicast address.

# 🗁 Types of MAC Addresses

MAC addresses can be classified according to their delivery type and administration scope.

The main delivery types are:

* Unicast.
* Multicast.
* Broadcast.

### 🖆 Unicast MAC Address

A unicast MAC address identifies an individual network interface.

It is used when a frame is intended for one specific destination interface.

Example:

```text
Source MAC:      00:11:22:33:44:AA
Destination MAC: 00:11:22:33:44:BB
```

The destination address has its I/G bit set to `0`.

Most ordinary Ethernet communication between two network interfaces uses unicast MAC addresses.

### 🖆 Multicast MAC Address

A multicast MAC address identifies a group of network interfaces.

A frame sent to a multicast MAC address can be received by multiple interfaces that belong to the corresponding multicast group.

Example Ethernet IPv4 multicast address:

```text
01:00:5E:00:00:01
```

IPv6 multicast Ethernet addresses use the prefix:

```text
33:33
```

Multicast is useful for protocols and applications that need to communicate with a group rather than one specific interface.

The I/G bit is set to `1`.

### 🖆 Broadcast MAC Address

A broadcast MAC address targets all devices on the local Layer 2 broadcast domain.

The Ethernet broadcast address is:

```text
FF:FF:FF:FF:FF:FF
```

All 48 bits are set to `1`.

Example:

```text
Source MAC:      00:11:22:33:44:AA
Destination MAC: FF:FF:FF:FF:FF:FF
```

Switches normally flood broadcast frames to all other forwarding ports in the same VLAN, excluding the port on which the frame was received.

Broadcast traffic is commonly used by protocols such as ARP in IPv4 networks.

### 🖆 Comparison of MAC Address Types

| Type | Destination | Purpose |
| --- | --- | --- |
| Unicast | One interface | One-to-one communication |
| Multicast | Group of interfaces | One-to-group communication |
| Broadcast | All interfaces in the broadcast domain | One-to-all communication |

### 🖆 Special Note: Broadcast Is a Group Address

A broadcast MAC address is a special form of group address.

Its I/G bit is `1`, just like multicast addresses.

However, broadcast and multicast have different delivery scopes and purposes.

# 🗁 MAC Address Communication

MAC addresses are used to deliver Ethernet frames within a local Layer 2 network.

Consider two devices connected to the same switch:

```text
PC A
IP: 192.168.1.10
MAC: AA:AA:AA:AA:AA:AA

        |
        |
      Switch
        |
        |
PC B
IP: 192.168.1.20
MAC: BB:BB:BB:BB:BB:BB
```

When PC A wants to communicate with PC B, it needs PC B's MAC address to construct the Ethernet frame.

In IPv4 networks, ARP (Address Resolution Protocol) performs this address resolution.

### 🖆 Example: ARP-Based MAC Resolution

Assume PC A does not know PC B's MAC address.

The communication process is:

* PC A checks its ARP cache.
* If no valid entry exists, PC A sends an ARP request.
* The ARP request is sent using the broadcast destination MAC address.
* All devices in the local broadcast domain receive the request.
* PC B recognizes its IP address in the request.
* PC B responds with its MAC address.
* PC A stores the mapping in its ARP cache.
* PC A can now send Ethernet frames directly to PC B.

Example ARP mapping:

```text
192.168.1.20 → BB:BB:BB:BB:BB:BB
```

### 🖆 Example Ethernet Frame

After resolving the destination MAC address, PC A sends an Ethernet frame.

```text
+------------------+
| Destination MAC  |
+------------------+
| Source MAC       |
+------------------+
| EtherType        |
+------------------+
| Payload          |
+------------------+
| Frame Check Seq. |
+------------------+
```

Example:

```text
Destination MAC: BB:BB:BB:BB:BB:BB
Source MAC:      AA:AA:AA:AA:AA:AA
EtherType:       IPv4
Payload:         IP packet
```

The destination MAC address tells the local network where the frame should be delivered.

### 🖆 Communication Across Different Networks

MAC addresses are not normally used end-to-end across routed networks.

Consider:

```text
PC A                Router                 PC B
192.168.1.10        192.168.1.1            10.0.0.20
     |                   |                     |
     +-------------------+---------------------+
```

When PC A communicates with PC B, which is on another IP network:

* PC A determines that the destination is outside its local subnet.
* PC A sends the frame to its default gateway.
* The destination MAC address is the router's local interface MAC address.
* The router receives the frame and examines the IP packet.
* The router forwards the packet through the appropriate outgoing interface.
* The router creates a new Layer 2 frame for the next network segment.

The IP source and destination addresses normally remain unchanged across ordinary routing, while the Ethernet source and destination MAC addresses are replaced at each routed Layer 2 hop.

This is a fundamental distinction between Layer 2 and Layer 3 addressing.

# 🗁 MAC Address and Ethernet Switching

Ethernet switches use MAC addresses to make forwarding decisions.

A switch maintains a MAC address table, also called a forwarding database (FDB).

The table maps learned MAC addresses to switch ports and, in VLAN-aware environments, VLAN identifiers.

Example:

| MAC Address | Switch Port | VLAN |
| --- | --- | --- |
| AA:AA:AA:AA:AA:AA | ether1 | 10 |
| BB:BB:BB:BB:BB:BB | ether2 | 10 |
| CC:CC:CC:CC:CC:CC | ether3 | 20 |

### 🖆 MAC Address Learning

When a switch receives an Ethernet frame, it examines the source MAC address.

The switch learns that the source MAC address is reachable through the port on which the frame arrived.

Example:

```text
PC A
MAC: AA:AA:AA:AA:AA:AA
        |
      ether1
        |
      Switch
```

When PC A sends a frame, the switch learns:

```text
AA:AA:AA:AA:AA:AA → ether1
```

This information is stored in the MAC address table.

### 🖆 MAC Address Forwarding

When a switch receives a frame, it checks the destination MAC address.

Its forwarding behavior depends on whether the destination is known and on the frame's type.

| Condition | Switch Behavior |
| --- | --- |
| Known unicast | Forward through the learned destination port |
| Unknown unicast | Flood within the applicable VLAN |
| Broadcast | Flood within the applicable VLAN |
| Multicast | Forward according to multicast handling and configuration |

Flooding normally excludes the ingress port and respects VLAN boundaries.

### 🖆 MAC Address Table Aging

Dynamic MAC address entries are not permanent.

Switches generally remove inactive dynamic entries after an aging period.

This allows the forwarding database to adapt when devices disconnect or move to another port.

The aging interval depends on the switch platform and configuration.

### 🖆 Why MAC Address Tables Matter

MAC address tables allow switches to forward traffic efficiently instead of sending every frame to every port.

They are essential to normal Ethernet switching.

# 🗁 MAC Address vs IP Address

MAC addresses and IP addresses both identify endpoints in network communication, but they operate at different layers and serve different purposes.

| Feature | MAC Address | IP Address |
| --- | --- | --- |
| OSI layer | Data Link Layer | Network Layer |
| Purpose | Local link-layer delivery | Logical network addressing and routing |
| Common size | 48 bits for traditional Ethernet MAC | 32 bits for IPv4, 128 bits for IPv6 |
| Representation | Hexadecimal | Decimal dotted notation for IPv4; hexadecimal notation for IPv6 |
| Scope | Local Layer 2 network | Can identify hosts across routed networks |
| Used by | Ethernet switches | Routers |
| Assignment | Manufacturer, software, or administrator | Static or dynamic configuration |
| Example | `00:1A:2B:3C:4D:5E` | `192.168.1.10` |

### 🖆 Practical Example

A computer has:

```text
IP Address: 192.168.1.10
MAC Address: AA:BB:CC:DD:EE:FF
```

The IP address identifies the device logically within an IP network.

The MAC address identifies the network interface for local link-layer communication.

When communicating with a remote IP network, the computer uses its default gateway's MAC address as the Ethernet destination rather than the remote server's MAC address.

### 🖆 Can Two Devices Have the Same MAC Address?

A properly managed network should avoid duplicate MAC addresses within the same Layer 2 domain.

If two active interfaces use the same MAC address in the same VLAN, switches may repeatedly update their MAC tables as frames arrive from different ports.

This can cause MAC address flapping and unpredictable connectivity.

Duplicate MAC addresses can result from:

* Incorrect manual configuration.
* Cloned virtual machine configurations.
* Misconfigured network devices.
* Software-based MAC address changes.

# 🗁 MAC Address in Linux

Linux provides several commands for inspecting and managing network interface MAC addresses.

### 🖆 Display Network Interfaces

Use:

```bash
ip link show
```

Example output:

```text
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP>
    link/ether 08:00:27:12:34:56 brd ff:ff:ff:ff:ff:ff
```

The relevant fields are:

```text
link/ether 08:00:27:12:34:56
```

* `link/ether` indicates an Ethernet-style link-layer address.
* `08:00:27:12:34:56` is the interface's current MAC address.
* `brd ff:ff:ff:ff:ff:ff` indicates the broadcast MAC address.

### 🖆 Display a Specific Interface

```bash
ip link show dev enp0s3
```

Replace `enp0s3` with your actual interface name.

### 🖆 Display MAC Address Only

```bash
cat /sys/class/net/enp0s3/address
```

Example:

```text
08:00:27:12:34:56
```

### 🖆 Display Interfaces with NetworkManager

```bash
nmcli device status
```

Display connection information:

```bash
nmcli connection show
```

Inspect a connection's configured MAC-related properties:

```bash
nmcli connection show "Wired connection 1"
```

### 🖆 Change a MAC Address Temporarily

A MAC address can be changed using the `ip` command.

Example:

```bash
sudo ip link set dev enp0s3 down
sudo ip link set dev enp0s3 address 02:11:22:33:44:55
sudo ip link set dev enp0s3 up
```

Verify the change:

```bash
ip link show dev enp0s3
```

This changes the active interface address at runtime. The change may be lost after reboot or when the network connection is reconfigured.

### 🖆 Restore the Original MAC Address

If the original address was recorded, it can be configured again:

```bash
sudo ip link set dev enp0s3 down
sudo ip link set dev enp0s3 address 08:00:27:12:34:56
sudo ip link set dev enp0s3 up
```

Use the actual original address of your interface.

### 🖆 Display the Neighbor Table

Linux can display IP-to-link-layer address mappings using:

```bash
ip neigh show
```

Example:

```text
192.168.1.1 dev enp0s3 lladdr 00:11:22:33:44:55 REACHABLE
192.168.1.20 dev enp0s3 lladdr AA:BB:CC:DD:EE:FF STALE
```

This table contains neighbor information, including IP-to-MAC mappings for IPv4 and IPv6 communication.

### 🖆 Important Linux Commands

| Command | Purpose |
| --- | --- |
| `ip link show` | Display network interfaces and MAC addresses |
| `ip link show dev enp0s3` | Inspect a specific interface |
| `ip addr show` | Display IP address configuration |
| `ip neigh show` | Display neighbor address mappings |
| `nmcli device status` | Display NetworkManager devices |
| `cat /sys/class/net/enp0s3/address` | Read an interface's current MAC address |

# 🗁 MAC Address Randomization and Spoofing

A MAC address can be changed by software. This is commonly called MAC spoofing or MAC address modification.

MAC address randomization is a related technique used to improve privacy, particularly on Wi-Fi networks.

### 🖆 MAC Address Spoofing

MAC spoofing means configuring a network interface to use a different MAC address from its factory-assigned address.

Common reasons include:

* Network testing.
* Troubleshooting.
* Virtual machine configuration.
* Replacing a network interface while preserving a configured address.
* Privacy-related configuration.

Changing a MAC address does not change the interface's IP address automatically in every environment. Network configuration may need to be renewed or adjusted separately.

### 🖆 MAC Address Randomization

Modern Wi-Fi clients can use randomized MAC addresses when scanning for or connecting to wireless networks.

This reduces the ability of nearby observers to track a device using one consistent hardware address.

Depending on the operating system, randomization may occur:

* During Wi-Fi scanning.
* When connecting to a particular wireless network.
* For each new connection.
* According to a persistent per-network randomized identity.

### 🖆 MAC Spoofing vs MAC Randomization

| Feature | MAC Spoofing | MAC Randomization |
| --- | --- | --- |
| Main purpose | Use a different MAC address | Improve privacy |
| Configuration | Often manual | Usually automatic |
| Common use | Testing and administration | Wi-Fi privacy |
| Address selection | Administrator or software | Operating system or network manager |

### 🖆 Important Limitation

MAC addresses are not authentication credentials.

A network that relies only on MAC address filtering can be vulnerable because an attacker may observe an authorized MAC address and configure another device to use it.

For stronger network access control, use appropriate authentication and security mechanisms such as WPA2/WPA3, 802.1X, or other suitable access-control systems.

# 🗁 Common Mistakes

### 🖆 Confusing MAC Addresses with IP Addresses

MAC addresses operate at Layer 2, while IP addresses operate at Layer 3.

They serve different purposes and are both important for normal IP communication over Ethernet.

### 🖆 Assuming MAC Addresses Never Change

A MAC address can be overridden, randomized, or assigned by software.

The factory-assigned address is not necessarily the active address.

### 🖆 Assuming the Destination MAC Is Always the Remote Device

When sending traffic to a remote network, the destination MAC address is normally the next-hop router's interface address.

The remote server's IP address remains the IP packet destination.

### 🖆 Assuming Switches Forward Frames Using IP Addresses

A traditional Layer 2 switch makes forwarding decisions using destination MAC addresses.

Layer 3 switches can also perform IP routing, but that is a separate forwarding function.

### 🖆 Treating Every Unknown Unicast as a Broadcast

Unknown unicast frames are flooded within the relevant VLAN, but they remain unicast frames. Their destination MAC address is still an individual address.

### 🖆 Assuming a MAC Address Identifies a Person or Device Permanently

MAC addresses identify network interfaces, not people. They can be changed, cloned, or randomized.

# 🗁 Best Practices

* Understand the difference between MAC addressing and IP addressing.
* Learn to inspect MAC addresses using Linux networking commands.
* Use unique MAC addresses for network interfaces within the same Layer 2 domain.
* Avoid manually changing MAC addresses unless there is a clear administrative or testing requirement.
* Do not rely on MAC filtering as the only network security mechanism.
* Understand how switches learn and age MAC address table entries.
* Use VLANs to separate Layer 2 broadcast domains where appropriate.
* Investigate MAC address flapping when troubleshooting switching problems.
* Understand how MAC randomization affects Wi-Fi client identification.
* Learn ARP and IPv6 Neighbor Discovery to understand address resolution.

# 🗁 Practical Exercises

### 🖆 Exercise: Inspect Your MAC Address

Display all network interfaces:

```bash
ip link show
```

Identify your active Ethernet or Wi-Fi interface.

Record:

* Interface name.
* Current MAC address.
* Broadcast MAC address.
* Interface state.

### 🖆 Exercise: Inspect Neighbor Information

Run:

```bash
ip neigh show
```

Identify the IP-to-MAC mappings available on your system.

Try to identify your default gateway's MAC address.

You can inspect your default route with:

```bash
ip route show default
```

### 🖆 Exercise: Compare MAC and IP Addresses

Run:

```bash
ip addr show
```

For an active interface, record:

* IPv4 address.
* IPv6 address, if available.
* MAC address.
* Interface name.

Explain the purpose of each address.

### 🖆 Exercise: Observe ARP Resolution

On an IPv4 local network, inspect the neighbor table:

```bash
ip neigh show
```

Remove a specific neighbor entry if you have permission:

```bash
sudo ip neigh del 192.168.1.1 dev enp0s3
```

Generate traffic to the gateway:

```bash
ping -c 3 192.168.1.1
```

Inspect the neighbor table again:

```bash
ip neigh show
```

Observe whether the entry is learned again.

Replace the example gateway address and interface name with your actual configuration.

### 🖆 Exercise: Inspect Ethernet Frames

Install Wireshark or use `tcpdump` to observe Ethernet traffic.

Install tcpdump on Ubuntu:

```bash
sudo apt update
sudo apt install tcpdump
```

Capture traffic on an interface:

```bash
sudo tcpdump -i enp0s3 -e
```

The `-e` option displays link-layer header information, including source and destination MAC addresses.

Generate traffic while the capture is running.

Observe:

* Source MAC address.
* Destination MAC address.
* Ethernet frame type.
* Whether frames are unicast or broadcast.

Stop the capture with `Ctrl+C`.

# 🗁 Interview Questions

### 🖆 What is a MAC address?

A MAC address is a link-layer identifier used to identify a network interface for communication within a Layer 2 network.

### 🖆 Which OSI layer uses MAC addresses?

The Data Link Layer, which is Layer 2.

### 🖆 How many bits are in a traditional Ethernet MAC address?

48 bits, or 6 bytes.

### 🖆 What is the full form of OUI?

Organizationally Unique Identifier.

### 🖆 What is the purpose of an OUI?

It identifies an organization or assigned address block within the IEEE MAC address allocation system.

### 🖆 What is the broadcast MAC address?

```text
FF:FF:FF:FF:FF:FF
```

It is used to address all interfaces in the local Layer 2 broadcast domain.

### 🖆 What is the difference between unicast and multicast MAC addresses?

Unicast addresses identify an individual interface, while multicast addresses identify a group of interfaces.

### 🖆 How does a switch learn MAC addresses?

A switch examines the source MAC address of incoming frames and associates that address with the ingress port and VLAN.

### 🖆 What happens when a switch receives a frame with an unknown destination MAC address?

The switch normally floods the frame to other forwarding ports within the same VLAN, excluding the ingress port.

### 🖆 Can a MAC address be changed?

Yes. Software or an administrator can configure a different MAC address on many network interfaces.

### 🖆 What is MAC spoofing?

MAC spoofing is the process of configuring a network interface to use a different MAC address.

### 🖆 Why is MAC filtering not strong authentication?

Because MAC addresses can be observed and spoofed. MAC filtering alone cannot reliably prove the identity of a device.

### 🖆 Does a router change MAC addresses when forwarding packets?

A router removes the incoming Layer 2 frame and creates a new frame for the outgoing link. Consequently, the source and destination MAC addresses are normally different on the next link.

### 🖆 What is MAC address flapping?

MAC address flapping occurs when a switch repeatedly learns the same MAC address on different ports, often indicating duplicate addressing, a Layer 2 loop, or a network topology issue.

# 🗁 References

* [IEEE Registration Authority](https://standards.ieee.org/products-programs/regauth/)
* [IEEE 802.3 Ethernet Working Group](https://www.ieee802.org/3/)
* [RFC 826 - An Ethernet Address Resolution Protocol](https://www.rfc-editor.org/rfc/rfc826)
* [RFC 894 - A Standard for the Transmission of IP Datagrams over Ethernet Networks](https://www.rfc-editor.org/rfc/rfc894)
* [Linux ip-link Manual](https://man7.org/linux/man-pages/man8/ip-link.8.html)
* [Linux ip-neighbour Manual](https://man7.org/linux/man-pages/man8/ip-neighbour.8.html)
