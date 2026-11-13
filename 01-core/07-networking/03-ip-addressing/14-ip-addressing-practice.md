# IP Addressing Practice

IP addressing is one of the fundamental concepts of computer networking. Understanding it requires more than memorizing IP address formats and address ranges.

You should be able to:

* Identify IPv4 and IPv6 addresses.
* Convert between binary and decimal representations.
* Identify network and host portions.
* Calculate subnet masks and network ranges.
* Determine usable host addresses.
* Perform subnetting and supernetting.
* Understand CIDR notation.
* Identify public, private, loopback, and link-local addresses.
* Configure IP addresses and default gateways.
* Troubleshoot common IP addressing problems.

This document provides practical exercises, configuration examples, troubleshooting scenarios, and interview questions to strengthen your understanding.

# 🗁 IPv4 Addressing Practice

### 🖆 Exercise: Identify IPv4 Addresses

Determine which of the following are valid IPv4 addresses.

| Address | Valid or Invalid |
| --- | --- |
| `192.168.1.10` | Valid |
| `10.0.0.1` | Valid |
| `172.16.10.20` | Valid |
| `256.10.10.10` | Invalid |
| `192.168.1` | Invalid |
| `192.168.1.300` | Invalid |
| `10.10.10.10.10` | Invalid |
| `172.32.1.1` | Valid |

**Explanation:**

An IPv4 address contains four decimal octets separated by dots.

Each octet must contain a value from `0` to `255`.

A valid IPv4 address has exactly four octets.

### 🖆 Exercise: Identify the Address Class

Although modern networking uses CIDR rather than classful addressing, understanding historical IPv4 classes is useful.

Identify the historical class of each address:

| IP Address | Class |
| --- | --- |
| `10.1.1.1` | Class A |
| `172.16.1.1` | Class B |
| `192.168.1.1` | Class C |
| `224.0.0.1` | Class D |
| `240.0.0.1` | Class E |

**Remember:** Classful addressing is historical. Modern networks use Classless Inter-Domain Routing (CIDR).

# 🗁 Binary and Decimal Conversion

IPv4 addresses use 32 bits divided into four 8-bit octets.

Each octet can represent values from `0` to `255`.

### 🖆 Binary to Decimal Conversion

Convert the following binary octets into decimal values.

| Binary | Decimal |
| --- | ---: |
| `00000000` | 0 |
| `00000001` | 1 |
| `00001010` | 10 |
| `00011000` | 24 |
| `01100100` | 100 |
| `11000000` | 192 |
| `11111111` | 255 |

Example:

```text
11000000
```

Calculate using positional values:

```text
128 + 64 + 0 + 0 + 0 + 0 + 0 + 0 = 192
```

Therefore:

```text
11000000 = 192
```

### 🖆 Decimal to Binary Conversion

Convert these decimal values into 8-bit binary.

| Decimal | Binary |
| --- | --- |
| 10 | `00001010` |
| 25 | `00011001` |
| 50 | `00110010` |
| 100 | `01100100` |
| 127 | `01111111` |
| 168 | `10101000` |
| 192 | `11000000` |
| 255 | `11111111` |

### 🖆 Complete IPv4 Conversion

Convert this IPv4 address into binary:

```text
192.168.1.10
```

Result:

```text
192 = 11000000
168 = 10101000
  1 = 00000001
 10 = 00001010
```

Complete binary representation:

```text
11000000.10101000.00000001.00001010
```

The complete address contains 32 bits.

### 🖆 Practice Questions

Convert the following addresses into binary:

* `192.168.1.1`
* `10.0.0.1`
* `172.16.10.20`
* `8.8.8.8`
* `255.255.255.0`

Convert the following binary addresses into decimal:

* `11000000.10101000.00000001.00000001`
* `00001010.00000000.00000000.00000001`
* `10101100.00010000.00001010.00010100`

# 🗁 Subnet Mask Practice

A subnet mask identifies the network and host portions of an IPv4 address.

For example:

```text
IP Address:  192.168.1.10
Subnet Mask: 255.255.255.0
```

Equivalent CIDR notation:

```text
192.168.1.10/24
```

The `/24` prefix means:

* Network bits: 24
* Host bits: 8

### 🖆 Common Subnet Masks

| CIDR | Subnet Mask | Total Addresses | Traditional Usable Hosts |
| --- | --- | ---: | ---: |
| `/8` | `255.0.0.0` | 16,777,216 | 16,777,214 |
| `/16` | `255.255.0.0` | 65,536 | 65,534 |
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/29` | `255.255.255.248` | 8 | 6 |
| `/30` | `255.255.255.252` | 4 | 2 |
| `/31` | `255.255.255.254` | 2 | 2 on supported point-to-point links |
| `/32` | `255.255.255.255` | 1 | Single address |

Traditional usable-host counts exclude the network and broadcast addresses. The `/31` point-to-point case is an important exception.

### 🖆 Practice Questions

Determine the subnet mask for each prefix:

| CIDR | Subnet Mask |
| --- | --- |
| `/20` | `255.255.240.0` |
| `/22` | `255.255.252.0` |
| `/23` | `255.255.254.0` |
| `/25` | `255.255.255.128` |
| `/26` | `255.255.255.192` |
| `/27` | `255.255.255.224` |
| `/28` | `255.255.255.240` |

# 🗁 CIDR Practice

CIDR represents an IP network using a prefix length.

Example:

```text
192.168.1.0/24
```

The `/24` indicates that the first 24 bits identify the network.

### 🖆 Exercise: Calculate Network Properties

Complete the following table.

| IP Address | CIDR | Network Address | Broadcast Address |
| --- | ---: | --- | --- |
| `192.168.1.10` | `/24` | `192.168.1.0` | `192.168.1.255` |
| `192.168.1.130` | `/25` | `192.168.1.128` | `192.168.1.255` |
| `192.168.1.70` | `/26` | `192.168.1.64` | `192.168.1.127` |
| `192.168.1.35` | `/27` | `192.168.1.32` | `192.168.1.63` |
| `192.168.1.18` | `/28` | `192.168.1.16` | `192.168.1.31` |
| `10.10.10.10` | `/16` | `10.10.0.0` | `10.10.255.255` |

### 🖆 Exercise: Calculate Host Capacity

Use the formula:

\[
\text{Host Bits} = 32 - \text{Prefix Length}
\]

For conventional IPv4 subnets with network and broadcast addresses reserved:

\[
\text{Usable Hosts} = 2^h - 2
\]

Where \(h\) is the number of host bits.

Calculate the traditional usable host capacity:

| Prefix | Host Bits | Total Addresses | Usable Hosts |
| --- | ---: | ---: | ---: |
| `/24` | 8 | 256 | 254 |
| `/25` | 7 | 128 | 126 |
| `/26` | 6 | 64 | 62 |
| `/27` | 5 | 32 | 30 |
| `/28` | 4 | 16 | 14 |
| `/29` | 3 | 8 | 6 |
| `/30` | 2 | 4 | 2 |

# 🗁 Subnetting Exercises

Subnetting divides a larger network into smaller logical networks.

Consider:

```text
192.168.1.0/24
```

Suppose you need to divide it into four equal-sized subnets.

Borrow two host bits:

\[
2^2 = 4
\]

New prefix:

\[
/24 + 2 = /26
\]

Each subnet contains:

\[
2^{6} = 64
\]

Total addresses, with 62 traditional usable host addresses.

### 🖆 Exercise: Divide a /24 Network

Divide:

```text
192.168.1.0/24
```

Into four `/26` subnets.

Expected result:

| Subnet | Network Address | First Usable | Last Usable | Broadcast |
| --- | --- | --- | --- | --- |
| 1 | `192.168.1.0/26` | `192.168.1.1` | `192.168.1.62` | `192.168.1.63` |
| 2 | `192.168.1.64/26` | `192.168.1.65` | `192.168.1.126` | `192.168.1.127` |
| 3 | `192.168.1.128/26` | `192.168.1.129` | `192.168.1.190` | `192.168.1.191` |
| 4 | `192.168.1.192/26` | `192.168.1.193` | `192.168.1.254` | `192.168.1.255` |

### 🖆 Exercise: Divide a /24 Into Eight Subnets

Given:

```text
192.168.10.0/24
```

Requirements:

* Create eight equal-sized subnets.
* Determine the new prefix.
* Calculate the subnet mask.
* Calculate the number of addresses per subnet.
* Identify the first and last usable addresses.
* Identify the broadcast address of each subnet.

Solution:

Borrow three bits:

\[
2^3 = 8
\]

New prefix:

\[
/24 + 3 = /27
\]

Subnet mask:

```text
255.255.255.224
```

Each subnet contains 32 addresses and traditionally supports 30 usable hosts.

| Subnet | Network Address | Usable Host Range | Broadcast |
| --- | --- | --- | --- |
| 1 | `192.168.10.0/27` | `.1 - .30` | `.31` |
| 2 | `192.168.10.32/27` | `.33 - .62` | `.63` |
| 3 | `192.168.10.64/27` | `.65 - .94` | `.95` |
| 4 | `192.168.10.96/27` | `.97 - .126` | `.127` |
| 5 | `192.168.10.128/27` | `.129 - .158` | `.159` |
| 6 | `192.168.10.160/27` | `.161 - .190` | `.191` |
| 7 | `192.168.10.192/27` | `.193 - .222` | `.223` |
| 8 | `192.168.10.224/27` | `.225 - .254` | `.255` |

### 🖆 Exercise: Identify the Subnet

Given:

```text
IP Address:  172.16.35.123
Subnet Mask: 255.255.255.192
```

Determine:

* CIDR prefix.
* Network address.
* Broadcast address.
* First usable address.
* Last usable address.
* Number of usable hosts.

Solution:

```text
CIDR:             /26
Network Address:  172.16.35.64
Broadcast:        172.16.35.127
First Usable:     172.16.35.65
Last Usable:      172.16.35.126
Usable Hosts:     62
```

# 🗁 VLSM Practice

Variable Length Subnet Masking (VLSM) allows a network administrator to create subnets of different sizes based on actual requirements.

### 🖆 Scenario: Design a Network

You have been assigned:

```text
Network: 192.168.10.0/24
```

You need to create networks for:

| Department | Required Hosts |
| --- | ---: |
| Administration | 100 |
| Development | 50 |
| Sales | 25 |
| Management | 10 |

Allocate the largest subnet first.

### 🖆 Solution

**Administration: 100 Hosts**

A `/25` provides 128 total addresses and 126 traditional usable hosts.

```text
Network:    192.168.10.0/25
Usable:     192.168.10.1 - 192.168.10.126
Broadcast:  192.168.10.127
```

**Development: 50 Hosts**

A `/26` provides 64 total addresses and 62 usable hosts.

```text
Network:    192.168.10.128/26
Usable:     192.168.10.129 - 192.168.10.190
Broadcast:  192.168.10.191
```

**Sales: 25 Hosts**

A `/27` provides 32 total addresses and 30 usable hosts.

```text
Network:    192.168.10.192/27
Usable:     192.168.10.193 - 192.168.10.222
Broadcast:  192.168.10.223
```

**Management: 10 Hosts**

A `/28` provides 16 total addresses and 14 usable hosts.

```text
Network:    192.168.10.224/28
Usable:     192.168.10.225 - 192.168.10.238
Broadcast:  192.168.10.239
```

### 🖆 Final VLSM Allocation

| Department | Network | Prefix | Usable Hosts |
| --- | --- | ---: | ---: |
| Administration | `192.168.10.0` | `/25` | 126 |
| Development | `192.168.10.128` | `/26` | 62 |
| Sales | `192.168.10.192` | `/27` | 30 |
| Management | `192.168.10.224` | `/28` | 14 |

Remaining address space:

```text
192.168.10.240/28
```

This remaining block contains 16 addresses and can be reserved for future requirements.

# 🗁 Supernetting Practice

Supernetting combines multiple contiguous, correctly aligned networks into a larger summarized network.

### 🖆 Exercise: Aggregate Four /24 Networks

Given:

```text
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
```

Determine the smallest single CIDR summary that represents all four networks exactly.

Solution:

Four `/24` networks can be aggregated into one `/22` because:

\[
4 \times 256 = 1024
\]

The resulting network is:

```text
192.168.0.0/22
```

Address range:

```text
192.168.0.0 - 192.168.3.255
```

### 🖆 Exercise: Identify Invalid Aggregation

Consider:

```text
192.168.10.0/24
192.168.11.0/24
192.168.12.0/24
192.168.13.0/24
```

These four networks cannot be represented by one exact `/22` summary.

The correct exact aggregation is:

```text
192.168.10.0/23
192.168.12.0/23
```

The reason is that a `/22` network must begin at a correctly aligned third-octet boundary divisible by four.

`192.168.8.0/22` would include additional address space, so it would not be an exact aggregation of the four supplied networks.

# 🗁 IPv6 Addressing Practice

IPv6 uses 128-bit addresses represented in hexadecimal notation.

Example:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

Compressed representation:

```text
2001:db8::1
```

### 🖆 Exercise: Compress IPv6 Addresses

Compress the following addresses.

**Address A**

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

Answer:

```text
2001:db8::1
```

**Address B**

```text
fe80:0000:0000:0000:0200:5eff:fe00:1234
```

Answer:

```text
fe80::200:5eff:fe00:1234
```

**Address C**

```text
2001:0db8:0000:0001:0000:0000:0000:0005
```

Answer:

```text
2001:db8:0:1::5
```

Remember:

* Leading zeros inside a hextet can be removed.
* Consecutive zero hextets can be compressed using `::`.
* The `::` notation can appear only once in an address.

### 🖆 Exercise: Identify IPv6 Address Types

| IPv6 Address | Type |
| --- | --- |
| `::1` | Loopback |
| `fe80::1` | Link-local |
| `2001:db8::1` | Documentation |
| `fd12:3456:789a::1` | Unique Local Address |
| `ff02::1` | Link-local multicast |
| `2001:4860:4860::8888` | Global unicast |

### 🖆 Exercise: Calculate IPv6 Prefixes

Given:

```text
Address: 2001:db8:abcd:1234::1/64
```

Determine:

* Address length.
* Network prefix.
* Interface identifier length.

Answer:

```text
IPv6 Address Length:      128 bits
Network Prefix Length:    64 bits
Interface Identifier:     64 bits
```

The corresponding network prefix is:

```text
2001:db8:abcd:1234::/64
```

# 🗁 Special-Purpose Address Practice

Identify the address category for each example.

| IP Address | Category |
| --- | --- |
| `10.0.0.1` | Private IPv4 |
| `172.16.0.1` | Private IPv4 |
| `192.168.1.1` | Private IPv4 |
| `8.8.8.8` | Public IPv4 |
| `127.0.0.1` | Loopback |
| `169.254.10.20` | IPv4 Link-Local |
| `100.64.0.1` | Shared Address Space (CGNAT) |
| `224.0.0.1` | IPv4 Multicast |
| `::1` | IPv6 Loopback |
| `fe80::1` | IPv6 Link-Local |
| `fd00::1` | IPv6 Unique Local |
| `ff02::1` | IPv6 Multicast |

### 🖆 Practice Questions

Determine whether each statement is true or false.

* `192.168.1.1` is a private IPv4 address.
* `169.254.1.1` is an RFC 1918 private address.
* `127.0.0.1` represents the local host.
* IPv6 supports broadcast addressing.
* `fe80::1` is an IPv6 link-local address.
* `100.64.0.1` belongs to the RFC 1918 private address space.

Answers:

| Statement | Answer |
| --- | --- |
| `192.168.1.1` is private | True |
| `169.254.1.1` is RFC 1918 private | False |
| `127.0.0.1` represents the local host | True |
| IPv6 supports broadcast | False |
| `fe80::1` is link-local | True |
| `100.64.0.1` is RFC 1918 private | False |

# 🗁 Default Gateway Practice

A default gateway provides a path to destinations outside the local network.

### 🖆 Exercise: Identify the Default Gateway

Given:

```text
IP Address:       192.168.10.25
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.10.1
```

Determine whether the following destinations are local or remote.

| Destination | Classification |
| --- | --- |
| `192.168.10.50` | Local |
| `192.168.10.200` | Local |
| `192.168.11.10` | Remote |
| `8.8.8.8` | Remote |
| `10.0.0.1` | Remote |

Local destinations can be reached directly at Layer 2. Remote destinations require routing through an appropriate next hop.

### 🖆 Exercise: Identify Configuration Errors

Consider:

```text
IP Address:       192.168.10.25
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.20.1
```

The host belongs to:

```text
192.168.10.0/24
```

The configured gateway belongs to:

```text
192.168.20.0/24
```

The gateway is outside the host's directly connected subnet. Without another valid route or an appropriate on-link configuration, the host cannot directly reach this gateway.

A typical corrected configuration is:

```text
IP Address:       192.168.10.25
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.10.1
```

# 🗁 Linux IP Configuration Lab

This lab uses Ubuntu and Linux networking commands to inspect and configure IP addresses.

### 🖆 Lab Requirements

* Ubuntu Linux.
* Terminal access.
* A network interface.
* Administrative privileges for configuration changes.

### 🖆 Inspect Network Interfaces

Display network interfaces:

```bash
ip link show
```

Display IPv4 addresses:

```bash
ip -4 address show
```

Display IPv6 addresses:

```bash
ip -6 address show
```

### 🖆 Inspect the Routing Table

Display IPv4 routes:

```bash
ip route
```

Display IPv6 routes:

```bash
ip -6 route
```

Identify the default gateway by examining the route beginning with:

```text
default via
```

### 🖆 Assign an IPv4 Address Temporarily

Identify your interface:

```bash
ip link show
```

For example, assume the interface is `eth0`.

Assign an IPv4 address:

```bash
sudo ip address add 192.168.10.10/24 dev eth0
```

Bring the interface up:

```bash
sudo ip link set eth0 up
```

This adds an address temporarily. It does not automatically configure persistent network settings.

### 🖆 Configure a Default Route Temporarily

Add a default route:

```bash
sudo ip route add default via 192.168.10.1 dev eth0
```

Verify:

```bash
ip route
```

### 🖆 Test Network Connectivity

Test the local loopback interface:

```bash
ping -c 4 127.0.0.1
```

Test the default gateway:

```bash
ping -c 4 192.168.10.1
```

Test an external IP address:

```bash
ping -c 4 8.8.8.8
```

Test DNS resolution:

```bash
getent hosts google.com
```

### 🖆 Remove the Temporary Configuration

Remove the added IPv4 address:

```bash
sudo ip address del 192.168.10.10/24 dev eth0
```

Remove the default route if it was added by this lab:

```bash
sudo ip route del default via 192.168.10.1 dev eth0
```

These commands affect live network configuration. Use an isolated virtual machine or lab interface when practicing.

# 🗁 Troubleshooting Scenarios

### 🖆 Scenario: Cannot Access the Internet

A computer has:

```text
IP Address:       192.168.1.100
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1
DNS Server:       8.8.8.8
```

The computer can communicate with other local devices but cannot access the internet.

Investigate:

* Whether the default gateway is reachable.
* Whether the router has a valid upstream route.
* Whether NAT is configured where required.
* Whether firewall rules allow outbound traffic.
* Whether the ISP connection is operational.

Useful tests:

```bash
ping -c 4 192.168.1.1
ping -c 4 8.8.8.8
getent hosts google.com
```

### 🖆 Scenario: Duplicate IP Address

Two devices are configured with:

```text
Device A: 192.168.1.50
Device B: 192.168.1.50
```

Both devices are connected to the same network.

Potential consequences:

* Intermittent connectivity.
* ARP table inconsistencies.
* Traffic delivered to the wrong device.
* Duplicate IP address notifications.

Solution:

* Identify both devices.
* Assign unique addresses.
* Check DHCP reservations and static IP assignments.
* Verify that the DHCP address pool does not overlap with manually assigned addresses.

### 🖆 Scenario: Incorrect Subnet Mask

A computer has:

```text
IP Address:  192.168.1.10
Subnet Mask: 255.255.0.0
```

However, the intended network is:

```text
192.168.1.0/24
```

The configured `/16` mask causes the computer to consider a much larger address range directly connected.

This can cause incorrect local-versus-remote destination decisions.

Correct configuration:

```text
IP Address:  192.168.1.10
Subnet Mask: 255.255.255.0
```

### 🖆 Scenario: DHCP Failure

A device receives:

```text
IP Address: 169.254.25.10
Subnet Mask: 255.255.0.0
```

Likely explanation:

The device could not obtain a valid DHCP configuration and selected an IPv4 link-local address.

Investigate:

* DHCP server availability.
* DHCP address pool capacity.
* VLAN configuration.
* DHCP relay.
* Physical and wireless connectivity.

### 🖆 Scenario: IPv6 Address Exists but Internet Access Fails

A host has:

```text
IPv6 Address: 2001:db8:abcd:1234::10/64
```

However, it cannot reach external IPv6 destinations.

Investigate:

* Whether the address is actually a usable global unicast address.
* Whether a default IPv6 route exists.
* Whether Router Advertisements are received.
* Whether the router has IPv6 upstream connectivity.
* Whether IPv6 firewall rules allow the traffic.

Inspect:

```bash
ip -6 address show
ip -6 route
ip -6 neigh
```

# 🗁 Interview Questions

### 🖆 Fundamental Questions

**What is an IP address?**

An IP address is a logical network-layer identifier used to identify an interface and support packet delivery across IP networks.

**What is the difference between IPv4 and IPv6?**

IPv4 uses 32-bit addresses, while IPv6 uses 128-bit addresses.

**Why is an IPv4 octet limited to 255?**

An octet contains eight bits, and eight bits represent 256 possible values from 0 to 255.

**What is a subnet mask?**

A subnet mask identifies which bits of an IPv4 address represent the network prefix and which represent the host portion.

**What is CIDR?**

CIDR is a classless addressing and routing method that represents network prefixes using variable-length prefix notation.

### 🖆 Subnetting Questions

**What is subnetting?**

Subnetting divides an IP network into smaller logical networks.

**What is VLSM?**

VLSM allows different subnet sizes within an address allocation based on host requirements.

**What is the difference between network and broadcast addresses?**

The network address identifies the subnet, while the IPv4 broadcast address represents all hosts on that subnet.

**How many traditional usable hosts exist in a `/26` IPv4 subnet?**

A `/26` contains six host bits:

\[
2^6 - 2 = 62
\]

**What is the difference between subnetting and supernetting?**

Subnetting divides a network into smaller prefixes, while supernetting aggregates multiple contiguous prefixes into a larger summarized prefix.

### 🖆 Practical Networking Questions

**What is a default gateway?**

A default gateway is the next-hop router a host uses for destinations without a more specific matching route.

**What is APIPA?**

APIPA is automatic IPv4 link-local addressing, commonly used when DHCP configuration is unsuccessful.

**What is the IPv4 loopback address?**

`127.0.0.1` is the commonly used IPv4 loopback address.

**What is the IPv6 loopback address?**

`::1` is the IPv6 loopback address.

**Does IPv6 use broadcast addresses?**

No. IPv6 uses multicast instead of broadcast.

**What is the IPv6 link-local range?**

`fe80::/10`.

# 🗁 Knowledge Assessment

Use the following questions to evaluate your understanding.

{@body const questions = [{q:"How many bits are in an IPv4 address?",o:["16 bits","32 bits","64 bits","128 bits"],a:1,e:"IPv4 addresses contain 32 bits, divided into four 8-bit octets."},{q:"What is the subnet mask for /26?",o:["255.255.255.0","255.255.255.128","255.255.255.192","255.255.255.224"],a:2,e:"A /26 prefix contains 26 network bits and has the mask 255.255.255.192."},{q:"How many traditional usable host addresses are available in a /27 subnet?",o:["14","30","32","62"],a:1,e:"A /27 has five host bits: 2⁵ − 2 = 30 traditional usable addresses."},{q:"Which address is an IPv4 loopback address?",o:["192.168.1.1","169.254.1.1","127.0.0.1","10.0.0.1"],a:2,e:"127.0.0.1 belongs to the IPv4 loopback range 127.0.0.0/8."},{q:"Which range is used for IPv4 link-local addressing?",o:["10.0.0.0/8","172.16.0.0/12","169.254.0.0/16","192.168.0.0/16"],a:2,e:"IPv4 link-local addressing uses 169.254.0.0/16."},{q:"Which prefix represents the IPv4 default route?",o:["127.0.0.0/8","0.0.0.0/0","255.255.255.255/32","192.168.1.0/24"],a:1,e:"0.0.0.0/0 matches all IPv4 destinations when no more specific route applies."},{q:"Which address is the IPv6 loopback address?",o:["fe80::1","ff02::1","::1","2001:db8::1"],a:2,e:"The IPv6 loopback address is ::1/128."},{q:"Which IPv6 range is used for link-local addressing?",o:["2000::/3","fc00::/7","fe80::/10","ff00::/8"],a:2,e:"IPv6 link-local addresses belong to fe80::/10."},{q:"Which protocol is used for IPv4-to-MAC address resolution on a local network?",o:["DNS","ARP","DHCP","ICMP"],a:1,e:"ARP resolves an IPv4 address to a link-layer address on a local network."},{q:"What is the purpose of VLSM?",o:["Encrypt IP packets","Assign different subnet sizes according to requirements","Replace IPv6","Convert public addresses into private addresses"],a:1,e:"VLSM allows subnet allocations of different sizes to use address space efficiently."}]}
{@body const [answers,setAnswers] = DIL.useState({})}
{@body const [submitted,setSubmitted] = DIL.useState(false)}
{@body const score = questions.filter((q,i)=>answers[i]===String(q.a)).length}
<box border radius="lg" padding={3} gap={3}>
  <row align="center" justify="between">
    **IP Addressing Quiz**
    <badge color="info">{Object.keys(answers).length}/{questions.length} answered</badge>
  </row>
  <box background="surface-tertiary" radius="full" height="6px" clip>
    <box background="surface-inverted" height="6px" radius="full" width={`${Object.keys(answers).length/questions.length*100}%`}/>
  </box>
  {#each questions as q,i}
    <box gap={2}>
      **{i+1}. {q.q}**
      <radio-group direction="col" value={answers[i]??""} disabled={submitted} onChange={v=>setAnswers(old=>({...old,[i]:v}))}>
        {#each q.o as option,j}
          <radio value={String(j)}>{option}</radio>
        {/each}
      </radio-group>
      {#if submitted}
        <box background="surface-secondary" padding={2} radius="md" gap={1}>
          <row align="center" gap={2}>
            {#if answers[i]===String(q.a)}
              <icon name="check-circle" color="success"/>
              <text color="success" weight="semibold" size="sm">Correct</text>
            {:else}
              <icon name="x-circle" color="danger"/>
              <text color="danger" weight="semibold" size="sm">Incorrect — {q.o[q.a]}</text>
            {/if}
          </row>
          <text size="sm">{q.e}</text>
        </box>
      {/if}
    </box>
    {#if i!==questions.length-1}
      <divider color="subtle"/>
    {/if}
  {/each}
  {#if submitted}
    <box background="surface-secondary" radius="lg" padding={3} gap={1}>
      <title size="2xl" tabularNums>{score}/{questions.length}</title>
      **{score===10?"Excellent work!":score>=7?"Good understanding!":score>=5?"Keep practicing.":"Review the concepts and try again."}**
      <text color="secondary" size="sm">Your score is {Math.round(score/questions.length*100)}%.</text>
    </box>
    <button block onClick={()=>{setAnswers({});setSubmitted(false)}}>Retry quiz</button>
  {:else}
    <button block disabled={Object.keys(answers).length!==questions.length} onClick={()=>setSubmitted(true)}>Submit answers</button>
  {/if}
</box>

# 🗁 Summary

IP addressing is essential for understanding how devices communicate across local and remote networks.

After completing this section, you should be able to:

* Understand IPv4 and IPv6 addressing structures.
* Convert IPv4 addresses between binary and decimal.
* Identify public, private, loopback, and link-local addresses.
* Understand subnet masks and CIDR notation.
* Calculate network addresses, broadcast addresses, and usable host ranges.
* Perform subnetting, VLSM, and supernetting.
* Understand default gateways and routing behavior.
* Identify APIPA addresses and troubleshoot DHCP failures.
* Inspect and configure IP networking on Ubuntu Linux.
* Solve practical network addressing problems.

# 🗁 References

* [RFC 791 – Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)
* [RFC 1918 – Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918)
* [RFC 3021 – Using 31-Bit Prefixes on IPv4 Point-to-Point Links](https://www.rfc-editor.org/rfc/rfc3021)
* [RFC 3927 – Dynamic Configuration of IPv4 Link-Local Addresses](https://www.rfc-editor.org/rfc/rfc3927)
* [RFC 4291 – IPv6 Addressing Architecture](https://www.rfc-editor.org/rfc/rfc4291)
* [RFC 4632 – Classless Inter-Domain Routing (CIDR)](https://www.rfc-editor.org/rfc/rfc4632)
* [RFC 5737 – IPv4 Address Blocks Reserved for Documentation](https://www.rfc-editor.org/rfc/rfc5737)
* [RFC 6890 – Special-Purpose IP Address Registries](https://www.rfc-editor.org/rfc/rfc6890)
* [Linux `ip` Command Manual](https://man7.org/linux/man-pages/man8/ip.8.html)
