# Subnetting Cheatsheet

Subnetting is the process of dividing a larger IP network into smaller logical networks called **subnets**.

It is an essential networking skill used in:

* Enterprise network design.
* ISP network planning.
* VLAN segmentation.
* Routing and firewall configuration.
* IP address allocation.
* Network security and isolation.
* Efficient IPv4 address utilization.

Subnetting helps administrators organize networks, control broadcast domains, allocate addresses efficiently, and simplify routing.

This cheatsheet covers IPv4 subnetting, CIDR, VLSM, IPv6 prefixes, and practical command-line examples.

# 🗁 IPv4 Address Structure

An IPv4 address is a 32-bit logical address divided into four 8-bit sections called octets.

Example:

```text
192.168.10.25
```

Binary representation:

```text
11000000.10101000.00001010.00011001
```

Each octet contains 8 bits and has a decimal range of 0–255.

### 🖆 Network and Host Portions

An IPv4 address contains two logical portions:

* **Network portion:** Identifies the network or subnet.
* **Host portion:** Identifies an individual interface within that subnet.

A subnet mask or CIDR prefix determines how many bits belong to the network portion.

Example:

```text
IP Address:  192.168.10.25
Subnet Mask: 255.255.255.0
CIDR:        /24
```

Binary subnet mask:

```text
11111111.11111111.11111111.00000000
```

The first 24 bits represent the network portion, while the remaining 8 bits represent the host portion.

### 🖆 IPv4 Address Components

Consider:

```text
IP Address:  192.168.10.25/24
Network:     192.168.10.0
Broadcast:   192.168.10.255
First Host:  192.168.10.1
Last Host:   192.168.10.254
```

For a conventional IPv4 subnet, the network address identifies the subnet, and the broadcast address is used to reach all hosts on that subnet.

The first and last addresses are generally excluded from ordinary host allocation.

Special cases include `/31` point-to-point links and `/32` host routes.

# 🗁 Subnet Mask Reference

A subnet mask identifies the network portion of an IPv4 address.

Each network bit is represented by `1`, and each host bit is represented by `0`.

### 🖆 Complete IPv4 Subnet Mask Table

| CIDR | Subnet Mask | Total Addresses | Traditional Usable Hosts |
| --- | --- | ---: | ---: |
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |
| /9 | 255.128.0.0 | 8,388,608 | 8,388,606 |
| /10 | 255.192.0.0 | 4,194,304 | 4,194,302 |
| /11 | 255.224.0.0 | 2,097,152 | 2,097,150 |
| /12 | 255.240.0.0 | 1,048,576 | 1,048,574 |
| /13 | 255.248.0.0 | 524,288 | 524,286 |
| /14 | 255.252.0.0 | 262,144 | 262,142 |
| /15 | 255.254.0.0 | 131,072 | 131,070 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /17 | 255.255.128.0 | 32,768 | 32,766 |
| /18 | 255.255.192.0 | 16,384 | 16,382 |
| /19 | 255.255.224.0 | 8,192 | 8,190 |
| /20 | 255.255.240.0 | 4,096 | 4,094 |
| /21 | 255.255.248.0 | 2,048 | 2,046 |
| /22 | 255.255.252.0 | 1,024 | 1,022 |
| /23 | 255.255.254.0 | 512 | 510 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |
| /31 | 255.255.255.254 | 2 | 2 on supported point-to-point links |
| /32 | 255.255.255.255 | 1 | 1 host address |

Traditional usable host counts subtract the network and broadcast addresses. `/31` point-to-point links use both addresses under RFC 3021, while `/32` represents a single host route rather than a conventional subnet.

### 🖆 Important Subnet Masks to Memorize

| CIDR | Subnet Mask | Common Use |
| --- | --- | --- |
| /8 | 255.0.0.0 | Large address block |
| /16 | 255.255.0.0 | Large private network |
| /24 | 255.255.255.0 | Typical LAN |
| /25 | 255.255.255.128 | Small network |
| /26 | 255.255.255.192 | Small office or VLAN |
| /27 | 255.255.255.224 | Small department |
| /28 | 255.255.255.240 | Small segment |
| /29 | 255.255.255.248 | Small allocation |
| /30 | 255.255.255.252 | Traditional point-to-point link |
| /31 | 255.255.255.254 | Point-to-point link |
| /32 | 255.255.255.255 | Single host route |

# 🗁 CIDR Notation

CIDR stands for **Classless Inter-Domain Routing**.

It represents an IP network using an address followed by a prefix length.

Example:

```text
192.168.10.0/24
```

Here:

* `192.168.10.0` is the network address.
* `/24` means the first 24 bits are network bits.
* The remaining 8 bits are host bits.

### 🖆 CIDR Prefix Interpretation

| Prefix | Network Bits | Host Bits |
| --- | ---: | ---: |
| /8 | 8 | 24 |
| /16 | 16 | 16 |
| /20 | 20 | 12 |
| /24 | 24 | 8 |
| /25 | 25 | 7 |
| /26 | 26 | 6 |
| /27 | 27 | 5 |
| /28 | 28 | 4 |
| /29 | 29 | 3 |
| /30 | 30 | 2 |
| /31 | 31 | 1 |
| /32 | 32 | 0 |

### 🖆 CIDR and Subnet Size

A smaller prefix number means a larger network.

For example:

```text
192.168.10.0/24
192.168.10.0/25
192.168.10.0/26
```

* `/24` contains 256 IPv4 addresses.
* `/25` contains 128 IPv4 addresses.
* `/26` contains 64 IPv4 addresses.

As the prefix length increases, the number of available addresses decreases.

# 🗁 Subnetting Formulas

These formulas are fundamental to IPv4 subnetting.

### 🖆 Total Number of Addresses

\[
\text{Total Addresses}=2^h
\]

Where `h` is the number of host bits.

Example:

```text
Prefix: /24
Host Bits: 32 - 24 = 8

Total Addresses = 2^8
                = 256
```

### 🖆 Traditional Usable Host Addresses

For conventional IPv4 subnets with a prefix from `/0` through `/30`:

\[
\text{Usable Hosts}=2^h-2
\]

The subtraction accounts for the network and broadcast addresses.

Example:

```text
Prefix: /26
Host Bits: 32 - 26 = 6

Usable Hosts = 2^6 - 2
             = 64 - 2
             = 62
```

Remember:

* `/31` supports two addresses on point-to-point links.
* `/32` represents one individual address.

### 🖆 Number of Subnets

When subnetting an existing network:

\[
\text{Number of Subnets}=2^s
\]

Where `s` is the number of bits borrowed from the original host portion.

Example:

```text
Original Network: /24
New Prefix:       /27

Borrowed Bits = 27 - 24
              = 3

Number of Subnets = 2^3
                  = 8
```

### 🖆 Number of Borrowed Bits

\[
s=\text{New Prefix}-\text{Original Prefix}
\]

Example:

```text
Original Prefix: /24
New Prefix:      /26

Borrowed Bits = 26 - 24 = 2
```

### 🖆 Host Bits

\[
h=32-\text{Prefix Length}
\]

Example:

```text
Prefix: /27

Host Bits = 32 - 27
          = 5
```

### 🖆 Required Subnet Size

For a conventional subnet requiring a specific number of usable hosts:

\[
h=\lceil\log_2(H+2)\rceil
\]

Where:

* `H` is the required number of usable hosts.
* `h` is the required number of host bits.
* `+2` accounts for the network and broadcast addresses.

Then:

\[
\text{Prefix Length}=32-h
\]

Example: A network requires 50 usable host addresses.

```text
Required Hosts = 50

50 + 2 = 52

2^5 = 32   (Not enough)
2^6 = 64   (Enough)

Host Bits = 6
Prefix = 32 - 6 = /26
```

A `/26` provides 62 traditionally usable host addresses.

# 🗁 Subnet Mask and Block Size

Block size is one of the fastest ways to calculate subnet ranges.

### 🖆 Block Size Formula

\[
\text{Block Size}=256-\text{Mask Value}
\]

Use the octet containing the subnet boundary.

Example:

```text
Subnet Mask: 255.255.255.192

Block Size = 256 - 192
           = 64
```

The subnet ranges in the fourth octet are:

```text
0 - 63
64 - 127
128 - 191
192 - 255
```

### 🖆 Common Block Sizes

| Subnet Mask in Relevant Octet | Block Size |
| ---: | ---: |
| 128 | 128 |
| 192 | 64 |
| 224 | 32 |
| 240 | 16 |
| 248 | 8 |
| 252 | 4 |
| 254 | 2 |
| 255 | 1 |

### 🖆 Subnet Increment Table

| CIDR | Mask in Relevant Octet | Block Size |
| --- | ---: | ---: |
| /9 | 128 | 128 |
| /10 | 192 | 64 |
| /11 | 224 | 32 |
| /12 | 240 | 16 |
| /13 | 248 | 8 |
| /14 | 252 | 4 |
| /15 | 254 | 2 |
| /17 | 128 | 128 |
| /18 | 192 | 64 |
| /19 | 224 | 32 |
| /20 | 240 | 16 |
| /21 | 248 | 8 |
| /22 | 252 | 4 |
| /23 | 254 | 2 |
| /25 | 128 | 128 |
| /26 | 192 | 64 |
| /27 | 224 | 32 |
| /28 | 240 | 16 |
| /29 | 248 | 8 |
| /30 | 252 | 4 |

The relevant octet depends on the prefix:

* `/1` through `/8`: First octet.
* `/9` through `/16`: Second octet.
* `/17` through `/24`: Third octet.
* `/25` through `/32`: Fourth octet.

# 🗁 Finding Network and Broadcast Addresses

The network address identifies the subnet. The broadcast address is the highest address in a conventional IPv4 subnet.

### 🖆 Network Address Calculation

The network address is calculated using a bitwise AND operation between the IP address and subnet mask.

\[
\text{Network Address}=\text{IP Address}\ \text{AND}\ \text{Subnet Mask}
\]

Example:

```text
IP Address:  192.168.10.70
Subnet Mask: 255.255.255.192
```

The fourth octet in binary:

```text
IP:    01000110  (70)
Mask:  11000000  (192)
       --------
AND:   01000000  (64)
```

Therefore:

```text
Network Address: 192.168.10.64
```

### 🖆 Broadcast Address Calculation

For a conventional IPv4 subnet, the broadcast address is the final address in the subnet range.

```text
Network Address: 192.168.10.64
Block Size:      64

Broadcast = 64 + 64 - 1
          = 127
```

Therefore:

```text
Broadcast Address: 192.168.10.127
```

### 🖆 First and Last Usable Host

For this example:

```text
Network:   192.168.10.64
Broadcast: 192.168.10.127
```

The usable range is:

```text
First Host: 192.168.10.65
Last Host:  192.168.10.126
```

Complete result:

| Property | Value |
| --- | --- |
| IP Address | 192.168.10.70 |
| CIDR | /26 |
| Subnet Mask | 255.255.255.192 |
| Network Address | 192.168.10.64 |
| Broadcast Address | 192.168.10.127 |
| First Usable Host | 192.168.10.65 |
| Last Usable Host | 192.168.10.126 |
| Total Addresses | 64 |
| Usable Hosts | 62 |

# 🗁 Subnetting Examples

### 🖆 Example: Divide a /24 Network into Four Subnets

Given:

```text
Original Network: 192.168.1.0/24
Required Subnets: 4
```

Calculate the borrowed bits:

```text
2^s = 4

s = 2
```

New prefix:

```text
Original Prefix: /24
Borrowed Bits:     2

New Prefix:       /26
Subnet Mask:      255.255.255.192
Block Size:       64
```

The four subnets are:

| Subnet | Network Address | First Host | Last Host | Broadcast |
| --- | --- | --- | --- | --- |
| 1 | 192.168.1.0/26 | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 |
| 2 | 192.168.1.64/26 | 192.168.1.65 | 192.168.1.126 | 192.168.1.127 |
| 3 | 192.168.1.128/26 | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 |
| 4 | 192.168.1.192/26 | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 |

Each subnet contains 64 total addresses and 62 traditionally usable host addresses.

### 🖆 Example: Divide a /24 Network into Eight Subnets

Given:

```text
Original Network: 192.168.10.0/24
Required Subnets: 8
```

Borrowed bits:

```text
2^3 = 8
```

New prefix:

```text
/24 + 3 = /27
```

Subnet mask:

```text
255.255.255.224
```

Block size:

```text
256 - 224 = 32
```

Subnet ranges:

| Subnet | Network | Host Range | Broadcast |
| --- | --- | --- | --- |
| 1 | 192.168.10.0/27 | .1 – .30 | .31 |
| 2 | 192.168.10.32/27 | .33 – .62 | .63 |
| 3 | 192.168.10.64/27 | .65 – .94 | .95 |
| 4 | 192.168.10.96/27 | .97 – .126 | .127 |
| 5 | 192.168.10.128/27 | .129 – .158 | .159 |
| 6 | 192.168.10.160/27 | .161 – .190 | .191 |
| 7 | 192.168.10.192/27 | .193 – .222 | .223 |
| 8 | 192.168.10.224/27 | .225 – .254 | .255 |

Each subnet contains 32 total addresses and 30 traditionally usable host addresses.

### 🖆 Example: Find the Subnet of an IP Address

Given:

```text
IP Address: 172.16.35.75/20
```

Subnet mask:

```text
255.255.240.0
```

The relevant octet is the third octet.

```text
Block Size = 256 - 240
           = 16
```

Third-octet ranges:

```text
0 - 15
16 - 31
32 - 47
48 - 63
64 - 79
...
```

The value `35` belongs to the range `32–47`.

Therefore:

| Property | Value |
| --- | --- |
| IP Address | 172.16.35.75 |
| CIDR | /20 |
| Subnet Mask | 255.255.240.0 |
| Network Address | 172.16.32.0 |
| First Usable Host | 172.16.32.1 |
| Last Usable Host | 172.16.47.254 |
| Broadcast Address | 172.16.47.255 |
| Total Addresses | 4,096 |
| Usable Hosts | 4,094 |

### 🖆 Example: Find the Subnet of an IP Address with /23

Given:

```text
IP Address: 192.168.5.100/23
```

Subnet mask:

```text
255.255.254.0
```

The relevant octet is the third octet.

```text
Block Size = 256 - 254
           = 2
```

Third-octet ranges:

```text
0 - 1
2 - 3
4 - 5
6 - 7
8 - 9
...
```

The third octet `5` belongs to the range `4–5`.

Therefore:

| Property | Value |
| --- | --- |
| Network Address | 192.168.4.0 |
| First Usable Host | 192.168.4.1 |
| Last Usable Host | 192.168.5.254 |
| Broadcast Address | 192.168.5.255 |
| Total Addresses | 512 |
| Usable Hosts | 510 |

# 🗁 VLSM Subnetting

VLSM stands for **Variable Length Subnet Mask**.

It allows a network administrator to create subnets of different sizes from one larger address block.

Instead of assigning the same subnet size to every network, VLSM allocates address space according to actual host requirements.

### 🖆 Why VLSM Is Important

Consider an organization with these requirements:

| Department | Required Hosts |
| --- | ---: |
| Administration | 100 |
| Engineering | 50 |
| Accounts | 25 |
| Management | 10 |
| Router Link | 2 |

Using one fixed subnet size for every department would waste address space.

VLSM allows each department to receive a suitable prefix.

### 🖆 VLSM Subnet Selection

Select the smallest conventional subnet that provides enough usable addresses.

| Required Hosts | Prefix | Usable Hosts |
| ---: | --- | ---: |
| 100 | /25 | 126 |
| 50 | /26 | 62 |
| 25 | /27 | 30 |
| 10 | /28 | 14 |
| 2 | /30 | 2 |

### 🖆 Complete VLSM Example

Given:

```text
Available Network: 192.168.100.0/24
```

Requirements:

```text
Administration: 100 Hosts
Engineering:      50 Hosts
Accounts:         25 Hosts
Management:       10 Hosts
Router Link:       2 Hosts
```

Allocate the largest subnet first.

**Administration — /25**

```text
Network:   192.168.100.0/25
First Host: 192.168.100.1
Last Host:  192.168.100.126
Broadcast:  192.168.100.127
```

**Engineering — /26**

```text
Network:   192.168.100.128/26
First Host: 192.168.100.129
Last Host:  192.168.100.190
Broadcast:  192.168.100.191
```

**Accounts — /27**

```text
Network:   192.168.100.192/27
First Host: 192.168.100.193
Last Host:  192.168.100.222
Broadcast:  192.168.100.223
```

**Management — /28**

```text
Network:   192.168.100.224/28
First Host: 192.168.100.225
Last Host:  192.168.100.238
Broadcast:  192.168.100.239
```

**Router Link — /30**

```text
Network:   192.168.100.240/30
First Host: 192.168.100.241
Last Host:  192.168.100.242
Broadcast:  192.168.100.243
```

### 🖆 VLSM Allocation Summary

| Department | Network | Prefix | Usable Hosts |
| --- | --- | --- | ---: |
| Administration | 192.168.100.0 | /25 | 126 |
| Engineering | 192.168.100.128 | /26 | 62 |
| Accounts | 192.168.100.192 | /27 | 30 |
| Management | 192.168.100.224 | /28 | 14 |
| Router Link | 192.168.100.240 | /30 | 2 |

Remaining unallocated address space:

```text
192.168.100.244 - 192.168.100.255
```

This remaining block contains 12 addresses, but it is not one complete conventional subnet boundary of a single CIDR prefix. It can be used only after considering valid subnet alignment and allocation requirements.

### 🖆 VLSM Planning Rules

* Sort subnet requirements from largest to smallest.
* Select the smallest suitable subnet for each requirement.
* Allocate each subnet on a valid network boundary.
* Avoid overlapping address ranges.
* Reserve additional space for future expansion.
* Document network, prefix, gateway, host range, and broadcast address.
* Use `/31` for suitable point-to-point links when both endpoints support it.

# 🗁 Private and Special IPv4 Ranges

Private IPv4 addresses are used inside private networks and are not globally routed on the public Internet.

### 🖆 RFC 1918 Private IPv4 Ranges

| Private Range | CIDR | Address Count |
| --- | --- | ---: |
| 10.0.0.0 – 10.255.255.255 | /8 | 16,777,216 |
| 172.16.0.0 – 172.31.255.255 | /12 | 1,048,576 |
| 192.168.0.0 – 192.168.255.255 | /16 | 65,536 |

### 🖆 Other Important IPv4 Ranges

| Address Range | Purpose |
| --- | --- |
| 127.0.0.0/8 | Loopback |
| 169.254.0.0/16 | IPv4 link-local |
| 100.64.0.0/10 | Shared address space, commonly used for CGNAT |
| 0.0.0.0/0 | Default route |
| 255.255.255.255 | Limited broadcast |
| 224.0.0.0/4 | IPv4 multicast |

### 🖆 CGNAT Address Range

Carrier-Grade NAT commonly uses:

```text
100.64.0.0/10
```

Address range:

```text
100.64.0.0 - 100.127.255.255
```

This address space is defined by RFC 6598 and is distinct from the RFC 1918 private ranges.

ISPs can use this range to assign shared addresses to customers behind carrier-grade NAT infrastructure.

# 🗁 IPv6 Prefix Reference

IPv6 uses 128-bit addresses rather than IPv4's 32-bit addresses.

Example:

```text
2001:db8:1234:0001:0000:0000:0000:0001
```

Compressed representation:

```text
2001:db8:1234:1::1
```

IPv6 prefix notation identifies the network portion of an IPv6 address.

### 🖆 Common IPv6 Prefixes

| Prefix | Common Purpose |
| --- | --- |
| /32 | Large provider allocation |
| /48 | Typical site allocation |
| /56 | Smaller site allocation |
| /64 | Common IPv6 LAN subnet |
| /127 | Point-to-point link |
| /128 | Single IPv6 address or host route |

These are common allocation patterns, not universal requirements. Actual prefix assignments depend on the provider and network design.

### 🖆 Important IPv6 Address Ranges

| Range | Purpose |
| --- | --- |
| 2000::/3 | Global unicast |
| fc00::/7 | Unique local addresses |
| fe80::/10 | Link-local addresses |
| ::1/128 | Loopback |
| ::/128 | Unspecified address |
| ff00::/8 | Multicast |
| 2001:db8::/32 | Documentation examples |

### 🖆 IPv6 Subnetting Principles

* IPv6 addresses contain 128 bits.
* A `/64` prefix is commonly used for an IPv6 LAN.
* IPv6 does not use broadcast addresses.
* Neighbor Discovery Protocol replaces the role of ARP.
* SLAAC can allow hosts to configure addresses automatically.
* DHCPv6 can provide additional or managed configuration.
* IPv6 subnetting generally focuses on prefix allocation rather than calculating traditional usable host ranges.

# 🗁 Linux Subnetting Commands

Linux provides useful commands for inspecting addresses, masks, routes, and network configuration.

### 🖆 Display IP Address and Prefix

```bash
ip address show
```

Display IPv4 addresses:

```bash
ip -4 address show
```

Display a specific interface:

```bash
ip address show dev enp0s3
```

### 🖆 Display Network Routes

```bash
ip route show
```

Display the default route:

```bash
ip route show default
```

Inspect how a destination will be reached:

```bash
ip route get 192.168.10.50
```

### 🖆 Calculate Subnets with ipcalc

Install `ipcalc`:

```bash
sudo apt install ipcalc
```

Calculate a subnet:

```bash
ipcalc 192.168.10.70/26
```

Calculate a network:

```bash
ipcalc 172.16.35.75/20
```

The output commonly includes:

* Network address.
* Netmask.
* Wildcard mask.
* Broadcast address.
* Host range.
* Number of addresses.

### 🖆 Use Python for Subnet Calculations

Python includes the built-in `ipaddress` module.

```python
import ipaddress

network = ipaddress.ip_network("192.168.10.70/26", strict=False)

print("Network:", network.network_address)
print("Netmask:", network.netmask)
print("Broadcast:", network.broadcast_address)
print("Total Addresses:", network.num_addresses)
print("Usable Hosts:", network.num_addresses - 2)

print("Host Range:")
print("First:", network[1])
print("Last:", network[-2])
```

For a conventional IPv4 subnet, this calculates the first and last usable addresses by excluding the network and broadcast addresses.

### 🖆 Python Subnet Iteration

Display every subnet when dividing a network into smaller prefixes:

```python
import ipaddress

network = ipaddress.ip_network("192.168.1.0/24")

for subnet in network.subnets(new_prefix=27):
    print(subnet)
```

Find the subnet containing an IP address:

```python
import ipaddress

network = ipaddress.ip_network("192.168.1.0/24")
address = ipaddress.ip_address("192.168.1.70")

for subnet in network.subnets(new_prefix=27):
    if address in subnet:
        print("Matching subnet:", subnet)
```

# 🗁 MikroTik RouterOS Subnetting Commands

MikroTik RouterOS uses CIDR notation when configuring IP addresses and routes.

### 🖆 Assign a Subnet Address

Assign a `/24` address to a bridge:

```routeros
/ip address add address=192.168.10.1/24 interface=bridge
```

Assign a `/26` address:

```routeros
/ip address add address=192.168.20.1/26 interface=bridge
```

Display configured addresses:

```routeros
/ip address print detail
```

### 🖆 Configure a Static Route

Route traffic to another subnet:

```routeros
/ip route add dst-address=192.168.30.0/24 gateway=192.168.10.2
```

Configure a default route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.10.254
```

Display routes:

```routeros
/ip route print detail
```

### 🖆 Configure Multiple Subnets with VLAN Interfaces

Create VLAN interfaces:

```routeros
/interface vlan
add name=vlan10 vlan-id=10 interface=bridge
add name=vlan20 vlan-id=20 interface=bridge
add name=vlan30 vlan-id=30 interface=bridge
```

Assign an IP address to each VLAN:

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
add address=192.168.30.1/24 interface=vlan30
```

This example assumes the bridge and VLAN tagging configuration have been designed appropriately. Creating VLAN interfaces alone does not configure the complete Layer 2 VLAN topology.

### 🖆 Create DHCP Pools for Subnets

Create an address pool:

```routeros
/ip pool
add name=pool-vlan10 ranges=192.168.10.10-192.168.10.254
```

Create another pool:

```routeros
/ip pool
add name=pool-vlan20 ranges=192.168.20.10-192.168.20.254
```

These ranges should be coordinated with the subnet's gateway, reserved addresses, and DHCP server network configuration.

### 🖆 Configure DHCP Network Entries

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
add address=192.168.20.0/24 gateway=192.168.20.1 dns-server=192.168.20.1
```

The corresponding DHCP server instances and interfaces must also be configured and enabled.

# 🗁 Common Subnetting Mistakes

### 🖆 Confusing Network and Broadcast Addresses

For a conventional `/26` subnet:

```text
Network:   192.168.1.64
Broadcast: 192.168.1.127
```

Neither address is ordinarily assigned to a host.

### 🖆 Using the Wrong Block Size

For `/27`:

```text
Mask:      255.255.255.224
Block Size: 256 - 224 = 32
```

Valid network boundaries are:

```text
0
32
64
96
128
160
192
224
```

A network address must align with the appropriate subnet boundary.

### 🖆 Assuming Every Subnet Has 254 Usable Hosts

A `/24` has 254 traditionally usable hosts, but smaller prefixes provide fewer addresses.

For example:

* `/25` = 126 usable hosts.
* `/26` = 62 usable hosts.
* `/27` = 30 usable hosts.
* `/28` = 14 usable hosts.
* `/29` = 6 usable hosts.
* `/30` = 2 usable hosts.

### 🖆 Overlapping Subnets

Two subnets must not unintentionally use overlapping address ranges within the same routing domain.

For example:

```text
Network A: 192.168.1.0/24
Network B: 192.168.1.128/25
```

Network B is completely contained inside Network A. These are overlapping networks.

### 🖆 Ignoring Point-to-Point Subnets

Traditional subnetting calculations treat `/30` as a two-host subnet.

However, `/31` is also valid for point-to-point links when both endpoints support RFC 3021.

### 🖆 Forgetting IPv6 Has No Broadcast

IPv6 uses multicast and other mechanisms instead of broadcast addressing.

Do not apply IPv4 broadcast calculations to IPv6 networks.

# 🗁 Quick Reference

### 🖆 IPv4 Subnetting Formulas

| Calculation | Formula |
| --- | --- |
| Host Bits | `32 - Prefix` |
| Total Addresses | `2 ^ Host Bits` |
| Traditional Usable Hosts | `2 ^ Host Bits - 2` |
| Number of Subnets | `2 ^ Borrowed Bits` |
| Borrowed Bits | `New Prefix - Original Prefix` |
| Block Size | `256 - Relevant Mask Octet` |
| New Prefix | `32 - Required Host Bits` |

### 🖆 Common Host Requirements

| Required Hosts | Recommended Prefix | Usable Hosts |
| ---: | --- | ---: |
| 500 | /23 | 510 |
| 250 | /24 | 254 |
| 120 | /25 | 126 |
| 60 | /26 | 62 |
| 28 | /27 | 30 |
| 12 | /28 | 14 |
| 6 | /29 | 6 |
| 2 | /30 | 2 |

The `/29` row provides 6 usable addresses under traditional network and broadcast exclusions.

### 🖆 Subnetting Calculation Checklist

When solving a subnetting problem, follow this sequence:

* Identify the original IP address and prefix.
* Determine the required number of hosts or subnets.
* Calculate the required host bits or borrowed bits.
* Determine the new prefix length.
* Find the subnet mask.
* Calculate the block size.
* Identify the correct network boundary.
* Calculate the broadcast address for conventional IPv4 subnets.
* Determine the first and last usable host addresses.
* Verify that the subnet does not overlap another allocation.

### 🖆 Recommended Practice

Practice these calculations until you can solve them without relying on a calculator:

* Convert IPv4 addresses between decimal and binary.
* Convert CIDR prefixes into subnet masks.
* Calculate network and broadcast addresses.
* Find usable host ranges.
* Divide `/24` networks into `/25`, `/26`, `/27`, and `/28` subnets.
* Solve VLSM allocation problems.
* Plan IPv4 subnets for VLANs and point-to-point links.
* Design IPv6 prefix allocations.

Subnetting becomes much easier when you understand binary arithmetic, prefix lengths, and block sizes. Mastering these concepts will make IP planning, routing, VLAN design, and ISP network engineering considerably more straightforward.
