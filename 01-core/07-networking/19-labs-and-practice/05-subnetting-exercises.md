# Subnetting Exercises

Subnetting is the process of dividing a larger IP network into smaller logical networks called subnets.

It helps network administrators use IP address space efficiently, organize network infrastructure, reduce broadcast domains, and control network segmentation.

Subnetting is an essential skill for:

* Network engineers.
* System administrators.
* ISP engineers.
* Network security engineers.
* Cloud engineers.
* Infrastructure architects.

In these exercises, you will practice IPv4 subnet calculations, CIDR notation, network addresses, broadcast addresses, usable host ranges, VLSM, and practical subnet configuration.

# 🗁 Learning Objectives

After completing these exercises, you should be able to:

* Identify IPv4 network and host portions.
* Calculate subnet masks.
* Convert between CIDR prefixes and subnet masks.
* Calculate network and broadcast addresses.
* Determine usable host ranges.
* Calculate the number of available hosts.
* Divide networks into equal-sized subnets.
* Perform Variable Length Subnet Masking (VLSM).
* Design an IP addressing plan.
* Configure subnet addresses on MikroTik and Linux.
* Troubleshoot subnetting problems.

# 🗁 IPv4 Address Structure

An IPv4 address contains 32 bits divided into four octets.

Example:

```text
192.168.10.25
```

Binary representation:

```text
11000000.10101000.00001010.00011001
```

Each octet contains 8 bits.

| Octet | Decimal | Binary |
| --- | ---: | --- |
| First | 192 | 11000000 |
| Second | 168 | 10101000 |
| Third | 10 | 00001010 |
| Fourth | 25 | 00011001 |

An IPv4 address consists of:

* Network portion.
* Host portion.

The subnet mask determines which bits represent the network and which bits represent hosts.

# 🗁 CIDR Notation

CIDR stands for Classless Inter-Domain Routing.

CIDR notation represents the number of network bits in an IP address.

Example:

```text
192.168.10.25/24
```

The `/24` indicates that the first 24 bits represent the network portion.

The remaining 8 bits represent the host portion.

Common CIDR prefixes:

| CIDR | Subnet Mask | Total Addresses | Traditional Usable Hosts |
| --- | --- | ---: | ---: |
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
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
| /31 | 255.255.255.254 | 2 | 2 for point-to-point use |
| /32 | 255.255.255.255 | 1 | 1 address |

Traditional usable-host calculations exclude the network and broadcast addresses. `/31` point-to-point links follow a special rule defined in RFC 3021. A `/32` represents one individual address, not a conventional multi-host subnet.

# 🗁 Subnet Mask Fundamentals

A subnet mask identifies the network bits of an IPv4 address.

Example:

```text
IP Address:  192.168.10.25
Subnet Mask: 255.255.255.0
CIDR:        /24
```

Binary representation:

```text
IP Address:
11000000.10101000.00001010.00011001

Subnet Mask:
11111111.11111111.11111111.00000000
```

The `1` bits identify the network portion.

The `0` bits identify the host portion.

# 🗁 Host Calculation Formula

For a conventional IPv4 subnet, the number of usable host addresses is:

\[
2^h - 2
\]

Where:

* \(h\) = Number of host bits.
* \(2^h\) = Total number of addresses.
* Subtract 2 for the network and broadcast addresses.

Example:

```text
192.168.10.0/24
```

Host bits:

\[
32 - 24 = 8
\]

Total addresses:

\[
2^8 = 256
\]

Traditional usable hosts:

\[
256 - 2 = 254
\]

# 🗁 Exercise: Identify Network and Host Bits

**Objective**

Identify the network and host portions of IPv4 addresses.

Determine the number of network bits and host bits for each prefix.

| IPv4 Prefix | Network Bits | Host Bits |
| --- | ---: | ---: |
| /8 | 8 | 24 |
| /16 | 16 | 16 |
| /20 | 20 | 12 |
| /24 | 24 | 8 |
| /25 | 25 | 7 |
| /26 | 26 | 6 |
| /27 | 27 | 5 |
| /28 | 28 | 4 |
| /30 | 30 | 2 |

**Practice Questions**

* How many host bits exist in a `/24` network?
* How many host bits exist in a `/27` network?
* How many network bits exist in a `/20` network?
* What is the total number of bits in IPv4?
* How many host addresses are available in a conventional `/29` subnet?

# 🗁 Exercise: Calculate Subnet Masks

**Objective**

Convert CIDR prefixes into dotted-decimal subnet masks.

Examples:

```text
/24 = 255.255.255.0
/25 = 255.255.255.128
/26 = 255.255.255.192
/27 = 255.255.255.224
/28 = 255.255.255.240
/29 = 255.255.255.248
```

**Practice Questions**

Convert the following prefixes into subnet masks:

| CIDR Prefix | Your Answer |
| --- | --- |
| /18 | |
| /19 | |
| /20 | |
| /21 | |
| /22 | |
| /23 | |
| /25 | |
| /26 | |
| /27 | |
| /28 | |

# 🗁 Exercise: Calculate Network Addresses

**Objective**

Determine the network address from an IPv4 address and its subnet mask.

The network address is calculated using a bitwise AND operation between the IP address and subnet mask.

Example:

```text
IP Address:  192.168.10.25
Subnet Mask: 255.255.255.0
```

Result:

```text
Network Address: 192.168.10.0
```

**Practice Questions**

Calculate the network address for each assignment:

| IP Address | CIDR Prefix | Network Address |
| --- | --- | --- |
| 192.168.1.100 | /24 | |
| 192.168.1.100 | /25 | |
| 192.168.1.100 | /26 | |
| 192.168.10.75 | /27 | |
| 10.10.5.20 | /16 | |
| 172.16.20.50 | /20 | |
| 192.168.50.200 | /28 | |
| 10.20.30.40 | /30 | |

# 🗁 Exercise: Calculate Broadcast Addresses

**Objective**

Find the broadcast address of an IPv4 subnet.

The broadcast address is the highest address in a conventional IPv4 subnet.

Example:

```text
Network:   192.168.10.0/24
Broadcast: 192.168.10.255
```

**Practice Questions**

Calculate the broadcast address:

| Network | Broadcast Address |
| --- | --- |
| 192.168.1.0/24 | |
| 192.168.1.0/25 | |
| 192.168.1.64/26 | |
| 192.168.10.32/27 | |
| 192.168.10.128/28 | |
| 10.10.0.0/16 | |
| 172.16.16.0/20 | |
| 192.168.50.192/26 | |

# 🗁 Exercise: Calculate Usable Host Ranges

**Objective**

Determine the first and last usable host addresses in a conventional IPv4 subnet.

Example:

```text
Network:          192.168.10.0/24
First Usable IP:  192.168.10.1
Last Usable IP:   192.168.10.254
Broadcast:        192.168.10.255
```

**Practice Questions**

Calculate the usable host range:

| Network | First Usable IP | Last Usable IP |
| --- | --- | --- |
| 192.168.1.0/24 | | |
| 192.168.1.0/25 | | |
| 192.168.1.128/25 | | |
| 192.168.10.64/26 | | |
| 192.168.10.96/27 | | |
| 192.168.10.128/28 | | |
| 10.10.10.0/30 | | |

# 🗁 Exercise: Determine Subnet Size

**Objective**

Calculate the total number of addresses and conventional usable hosts for different CIDR prefixes.

Use:

\[
\text{Total Addresses} = 2^{32-p}
\]

\[
\text{Usable Hosts} = 2^{32-p} - 2
\]

Where \(p\) represents the prefix length.

**Practice Questions**

| CIDR Prefix | Total Addresses | Usable Hosts |
| --- | ---: | ---: |
| /21 | | |
| /22 | | |
| /23 | | |
| /24 | | |
| /25 | | |
| /26 | | |
| /27 | | |
| /28 | | |
| /29 | | |
| /30 | | |

# 🗁 Exercise: Find the Block Size

**Objective**

Determine the subnet increment, commonly called the block size.

For the relevant octet:

\[
\text{Block Size} = 256 - \text{Subnet Mask Octet}
\]

Example:

```text
CIDR:       /26
Subnet Mask: 255.255.255.192

Block Size:
256 - 192 = 64
```

The subnet ranges in the final octet are:

```text
192.168.10.0/26
192.168.10.64/26
192.168.10.128/26
192.168.10.192/26
```

**Practice Questions**

Calculate the block size:

| CIDR Prefix | Relevant Mask Octet | Block Size |
| --- | ---: | ---: |
| /25 | 128 | |
| /26 | 192 | |
| /27 | 224 | |
| /28 | 240 | |
| /29 | 248 | |
| /30 | 252 | |

# 🗁 Exercise: Divide a Network into Equal Subnets

**Objective**

Divide one larger network into multiple equal-sized subnets.

Consider:

```text
Original Network: 192.168.10.0/24
Required Subnets: 4
```

To create four equal subnets, borrow two host bits:

\[
2^2 = 4
\]

New prefix:

```text
/24 + 2 = /26
```

Each subnet contains 64 total addresses and 62 conventional usable host addresses.

**Solution**

| Subnet | Network Address | Usable Host Range | Broadcast |
| --- | --- | --- | --- |
| 1 | 192.168.10.0/26 | 192.168.10.1–62 | 192.168.10.63 |
| 2 | 192.168.10.64/26 | 192.168.10.65–126 | 192.168.10.127 |
| 3 | 192.168.10.128/26 | 192.168.10.129–190 | 192.168.10.191 |
| 4 | 192.168.10.192/26 | 192.168.10.193–254 | 192.168.10.255 |

**Practice Questions**

Divide the following networks into the requested number of equal-sized subnets:

* `192.168.20.0/24` into 2 subnets.
* `192.168.30.0/24` into 4 subnets.
* `192.168.40.0/24` into 8 subnets.
* `10.10.0.0/16` into 16 subnets.
* `172.16.0.0/16` into 8 subnets.

For each result, identify:

* New CIDR prefix.
* Subnet mask.
* Network addresses.
* First usable addresses.
* Last usable addresses.
* Broadcast addresses.

# 🗁 Exercise: Variable Length Subnet Masking

**Objective**

Use VLSM to allocate different-sized subnets from a larger address block.

VLSM allows subnets of different sizes to be created within the same address space.

This helps reduce unnecessary IP address allocation.

Consider the following requirements:

| Department | Required Hosts |
| --- | ---: |
| Administration | 100 |
| Engineering | 50 |
| Sales | 25 |
| Management | 10 |
| Point-to-Point Link | 2 |

Available network:

```text
192.168.100.0/24
```

**Solution Approach**

Allocate the largest subnet first.

For each requirement, select the smallest subnet that provides enough usable addresses.

| Department | Required Hosts | Selected Prefix | Usable Hosts |
| --- | ---: | ---: | ---: |
| Administration | 100 | /25 | 126 |
| Engineering | 50 | /26 | 62 |
| Sales | 25 | /27 | 30 |
| Management | 10 | /28 | 14 |
| Point-to-Point Link | 2 | /30 | 2 |

**VLSM Allocation**

| Department | Network Address | Usable Host Range | Broadcast |
| --- | --- | --- | --- |
| Administration | 192.168.100.0/25 | 192.168.100.1–126 | 192.168.100.127 |
| Engineering | 192.168.100.128/26 | 192.168.100.129–190 | 192.168.100.191 |
| Sales | 192.168.100.192/27 | 192.168.100.193–222 | 192.168.100.223 |
| Management | 192.168.100.224/28 | 192.168.100.225–238 | 192.168.100.239 |
| Point-to-Point | 192.168.100.240/30 | 192.168.100.241–242 | 192.168.100.243 |

Remaining address space:

```text
192.168.100.244–192.168.100.255
```

**Practice Questions**

Perform VLSM allocation for the following requirements.

Available network:

```text
192.168.50.0/24
```

| Department | Required Hosts |
| --- | ---: |
| IT | 80 |
| HR | 40 |
| Finance | 20 |
| Support | 10 |
| Network Link | 2 |

Calculate the subnet allocation, ensuring that:

* The largest subnet is allocated first.
* Every subnet begins at a valid network boundary.
* No subnet overlaps another.
* Every department receives sufficient host addresses.

# 🗁 Exercise: Design an Enterprise IP Addressing Plan

**Objective**

Design an IP addressing plan for a small organization.

Organization requirements:

| Department | Required Hosts |
| --- | ---: |
| Administration | 50 |
| Development | 100 |
| Finance | 25 |
| Support | 20 |
| Server Network | 15 |
| Network Management | 10 |

Available private network:

```text
10.10.0.0/16
```

Design suitable subnets using VLSM.

For every department, document:

* VLAN or network name.
* Network address.
* CIDR prefix.
* Subnet mask.
* Default gateway.
* Usable host range.
* Broadcast address.
* DHCP address pool, if applicable.

Keep infrastructure addresses such as gateways, servers, and network devices reserved according to a consistent addressing policy.

# 🗁 Exercise: Subnet Configuration on MikroTik

**Objective**

Configure multiple subnets on a MikroTik CHR router.

Topology:

```text
                 MikroTik CHR
                      |
          +-----------+-----------+
          |           |           |
         LAN-1       LAN-2       LAN-3
          |           |           |
       Client-1    Client-2    Client-3
```

Addressing plan:

| Network | Router Interface | IP Address |
| --- | --- | --- |
| LAN-1 | ether1 | 192.168.10.1/24 |
| LAN-2 | ether2 | 192.168.20.1/24 |
| LAN-3 | ether3 | 192.168.30.1/24 |

Configure the addresses:

```routeros
/ip address
add address=192.168.10.1/24 interface=ether1 comment="LAN-1"
add address=192.168.20.1/24 interface=ether2 comment="LAN-2"
add address=192.168.30.1/24 interface=ether3 comment="LAN-3"
```

Verify:

```routeros
/ip address print
/ip route print
```

Configure the client addresses and default gateways according to the addressing plan.

Test communication between the networks:

```bash
ping -c 4 192.168.20.10
```

MikroTik automatically creates connected routes for the configured directly connected networks.

# 🗁 Exercise: Subnet Configuration on Ubuntu

**Objective**

Configure an Ubuntu interface using a subnet address.

Assign an IP address:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
```

Bring the interface up:

```bash
sudo ip link set enp0s3 up
```

Inspect the address:

```bash
ip -br address
```

Inspect the routing table:

```bash
ip route
```

Remove the address:

```bash
sudo ip address del 192.168.10.10/24 dev enp0s3
```

These commands modify runtime network configuration. For persistent configuration, use the network management system configured on the Ubuntu installation, such as Netplan or NetworkManager.

# 🗁 Exercise: Verify Subnet Calculations with Linux

**Objective**

Use Linux networking utilities to verify subnet information.

Install `ipcalc`:

```bash
sudo apt update
sudo apt install ipcalc
```

Calculate subnet information:

```bash
ipcalc 192.168.10.25/24
```

Calculate a smaller subnet:

```bash
ipcalc 192.168.10.100/26
```

You can use `ipcalc` to inspect:

* Network address.
* Broadcast address.
* Subnet mask.
* Host range.
* Number of host addresses.
* Wildcard mask.

Compare the results with your manual calculations.

# 🗁 Exercise: Identify Subnetting Errors

**Objective**

Find and correct common subnetting mistakes.

**Scenario: Incorrect Subnet Mask**

Device A:

```text
192.168.10.10/24
```

Device B:

```text
192.168.20.10/24
```

Both devices are connected to the same Layer 2 network without a router.

Question: Can they communicate directly using their configured subnet masks?

Explain the routing decision and how to correct the topology.

**Scenario: Duplicate IP Address**

Two devices use:

```text
192.168.10.10/24
```

Question: What problems can occur when both devices use the same IPv4 address?

**Scenario: Incorrect Gateway**

Client:

```text
IP Address:      192.168.10.10/24
Default Gateway: 192.168.20.1
```

Question: Why is this gateway configuration invalid for a typical directly connected Ethernet network?

**Scenario: Overlapping Subnets**

Router configuration:

```text
Interface 1: 192.168.10.1/24
Interface 2: 192.168.10.129/25
```

Question: Why can overlapping connected networks create routing and interface-configuration problems?

Identify a suitable non-overlapping addressing plan.

# 🗁 Subnetting Practice Challenge

Complete the following questions without using a subnet calculator.

**Challenge: Basic Calculations**

* Convert `255.255.255.192` into CIDR notation.
* Calculate the usable hosts in `/27`.
* Find the network address of `192.168.5.75/26`.
* Find the broadcast address of `192.168.5.64/26`.
* Find the usable host range of `10.10.10.0/28`.

**Challenge: Equal Subnetting**

Divide:

```text
192.168.200.0/24
```

Into 8 equal subnets.

Identify every network address, usable host range, and broadcast address.

**Challenge: VLSM**

Allocate:

```text
172.16.10.0/24
```

For these requirements:

| Department | Required Hosts |
| --- | ---: |
| Engineering | 60 |
| Administration | 30 |
| Sales | 20 |
| Support | 10 |
| Router Link | 2 |

Provide a complete VLSM allocation table.

**Challenge: Network Design**

Design an addressing plan for an ISP customer network with:

* One management subnet.
* One server subnet.
* Four customer LANs.
* Two point-to-point router links.

Use an available private IPv4 address block and document the complete subnet allocation.

# 🗁 Best Practices

* Always identify the required host count before selecting a subnet.
* Allocate the largest VLSM subnet first.
* Avoid overlapping network ranges.
* Reserve addresses for gateways and infrastructure.
* Document every subnet.
* Use consistent addressing conventions.
* Verify calculations with tools after solving manually.
* Consider future network growth.
* Use `/31` for supported point-to-point IPv4 links where appropriate.
* Remember that subnetting and VLAN configuration are related but separate concepts.

# 🗁 Summary

Subnetting is a fundamental networking skill used to organize IP networks efficiently.

After completing these exercises, you should be able to:

* Understand IPv4 network and host bits.
* Convert CIDR prefixes into subnet masks.
* Calculate network and broadcast addresses.
* Determine usable host ranges.
* Calculate subnet sizes.
* Divide networks into equal subnets.
* Perform VLSM allocation.
* Design IP addressing plans.
* Configure subnets on MikroTik and Linux.
* Troubleshoot subnetting errors.

Practice these calculations until you can solve common subnetting problems manually and verify your results using networking tools.
