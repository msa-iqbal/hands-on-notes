# Public vs Private IP Address

IP addresses are commonly classified into two categories: **public IP addresses** and **private IP addresses**.

A public IP address is intended to be globally unique and routable across the public Internet. A private IP address is used within private networks and is not directly routable across the public Internet.

Understanding the difference is essential for network design, subnetting, routing, NAT, firewall configuration, and troubleshooting.

# 🗁 What Is a Public IP Address?

A **public IP address** is an IP address allocated for use on the public Internet. It is globally unique within the applicable Internet addressing system and can be routed between networks across the Internet.

Public IP addresses are generally assigned by an Internet Service Provider (ISP) or allocated through an organization responsible for IP address management.

### 🖆 Example

```text
8.8.8.8
1.1.1.1
203.0.113.10
```

The first two addresses are public IPv4 addresses. `203.0.113.10` is reserved for documentation and examples, so it should not be used as a real public host address.

### 🖆 Characteristics of Public IP Addresses

* Globally unique within the public addressing system.
* Routable across the public Internet.
* Assigned by an ISP or an authorized address allocation process.
* Can be used to expose Internet-facing services.
* May be static or dynamically assigned.
* Can be associated with servers, routers, firewalls, and other Internet-connected devices.

### 🖆 Common Uses

| Use Case | Description |
| --- | --- |
| Web servers | Hosting publicly accessible websites and applications. |
| Mail servers | Sending and receiving email over the Internet. |
| DNS servers | Providing publicly accessible DNS services. |
| VPN servers | Allowing remote users to connect to private networks. |
| Internet routers | Connecting an organization or customer network to an ISP. |

# 🗁 What Is a Private IP Address?

A **private IP address** is an IP address reserved for communication within private networks.

Private IPv4 addresses are not globally routable across the public Internet. They can be reused by different organizations and networks because they do not need to be globally unique.

For example, two separate homes can both use `192.168.1.10` without creating a public IP address conflict.

### 🖆 Example

```text
192.168.1.10
10.0.0.15
172.16.5.20
```

These addresses belong to the private IPv4 ranges defined by RFC 1918.

### 🖆 Characteristics of Private IP Addresses

* Used inside private networks.
* Not routed across the public Internet as ordinary destination addresses.
* Can be reused across independent networks.
* Commonly assigned by a DHCP server or configured manually.
* Usually communicate with external IPv4 networks through NAT.
* Can be static or dynamically assigned.

### 🖆 Common Uses

| Use Case | Description |
| --- | --- |
| Home networks | Assigning addresses to computers, phones, and smart devices. |
| Office networks | Connecting workstations, printers, and internal servers. |
| Data centers | Communicating between internal servers and services. |
| Cloud networks | Connecting virtual machines and internal cloud resources. |
| Network infrastructure | Managing routers, switches, and other internal devices. |

# 🗁 Private IPv4 Address Ranges

RFC 1918 defines three private IPv4 address blocks.

### 🖆 RFC 1918 Address Blocks

| Private Range | CIDR | Subnet Mask | Total Addresses |
| --- | --- | --- | ---: |
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` | `255.0.0.0` | 16,777,216 |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` | `255.240.0.0` | 1,048,576 |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` | `255.255.0.0` | 65,536 |

These totals represent the complete address counts in each block, including network and broadcast addresses where applicable.

### 🖆 Understanding Each Range

**10.0.0.0/8**

```text
10.0.0.0 - 10.255.255.255
```

This is the largest RFC 1918 private IPv4 block.

Commonly used in:

* Large enterprise networks.
* Data centers.
* Cloud infrastructure.
* Large internal network environments.

**172.16.0.0/12**

```text
172.16.0.0 - 172.31.255.255
```

This range contains 16 consecutive `/16` blocks.

Commonly used in:

* Medium-sized organizations.
* Internal network segmentation.
* Virtual private networks.
* Enterprise infrastructure.

**192.168.0.0/16**

```text
192.168.0.0 - 192.168.255.255
```

This range is commonly used in home and small office networks.

Examples:

```text
192.168.0.1
192.168.1.1
192.168.10.1
192.168.100.1
```

### 🖆 Important: Not Every 172.x.x.x Address Is Private

Only the following portion of the `172.0.0.0/8` space is private:

```text
172.16.0.0 - 172.31.255.255
```

For example:

| IP Address | Classification |
| --- | --- |
| `172.16.0.1` | Private |
| `172.20.10.5` | Private |
| `172.31.255.254` | Private |
| `172.15.0.1` | Not RFC 1918 private |
| `172.32.0.1` | Not RFC 1918 private |

# 🗁 Public vs Private IP Addresses

The primary difference is their intended addressing scope and Internet routing behavior.

### 🖆 Comparison Table

| Feature | Public IP | Private IP |
| --- | --- | --- |
| Scope | Public Internet | Private network |
| Global uniqueness | Globally unique for public routing | Can be reused across networks |
| Internet routing | Globally routable in principle | Not globally routable |
| Assignment | ISP or authorized allocation | Network administrator or DHCP |
| Address reuse | Not normally reusable across unrelated public networks | Reusable across private networks |
| NAT requirement | Not inherently required | Usually required for direct IPv4 Internet access |
| Common usage | Public servers and Internet-facing routers | Internal computers and network devices |
| Example | `8.8.8.8` | `192.168.1.10` |

### 🖆 Example Network

Consider a home network containing three devices:

```text
                  Internet
                     |
                     |
              Public IP Address
                203.0.113.10
                     |
               Home Router
               NAT / Firewall
                     |
              Private Network
              192.168.1.0/24
                     |
          +----------+----------+
          |          |          |
       Laptop      Phone      Desktop
    192.168.1.10  .1.20       .1.30
```

Each device has its own private IP address, while the router uses a public IP address for Internet connectivity.

The documentation address `203.0.113.10` is used here only as an example.

# 🗁 How Public and Private IP Addresses Work Together

Most traditional home and office IPv4 networks use private addresses internally and a public address at their Internet connection.

When an internal device communicates with an Internet server, the router typically performs Network Address Translation (NAT).

### 🖆 Outgoing Communication

Suppose a laptop accesses a website.

**Laptop configuration:**

```text
Private IP: 192.168.1.10
Default Gateway: 192.168.1.1
```

**Router configuration:**

```text
LAN IP: 192.168.1.1
WAN IP: 203.0.113.10
```

**Website server:**

```text
Server IP: 93.184.216.34
```

The communication flow:

```text
Laptop
192.168.1.10
     |
     | Request to 93.184.216.34
     |
Home Router
192.168.1.1
     |
     | NAT translates source address
     |
Public IP
203.0.113.10
     |
     |
Internet
     |
     |
Web Server
93.184.216.34
```

The router translates the private source address into its public source address when forwarding the traffic to the Internet.

The response is returned to the router, which uses its connection-tracking information to forward the response to the correct internal device.

# 🗁 Understanding NAT

**Network Address Translation (NAT)** is a technique that modifies IP address information in packet headers as traffic passes through a network device.

NAT is commonly used to allow multiple private IPv4 devices to share one or more public IPv4 addresses.

### 🖆 Example Without NAT

Imagine a private network with three devices:

```text
Laptop:  192.168.1.10
Phone:   192.168.1.20
Desktop: 192.168.1.30
```

These private addresses cannot be used as ordinary source addresses for communication across the public Internet.

### 🖆 Example With NAT

The router translates outgoing connections to its public IP address:

```text
Laptop:  192.168.1.10 ──┐
Phone:   192.168.1.20 ──┼── Router ── 203.0.113.10
Desktop: 192.168.1.30 ──┘
```

With Port Address Translation (PAT), the router can distinguish multiple connections using transport-layer port information.

Example:

| Internal Address | Translated Address |
| --- | --- |
| `192.168.1.10:51000` | `203.0.113.10:40001` |
| `192.168.1.20:52000` | `203.0.113.10:40002` |
| `192.168.1.30:53000` | `203.0.113.10:40003` |

The router maintains translation and connection-tracking information so that returning traffic reaches the correct internal device.

### 🖆 Common NAT Types

| NAT Type | Description |
| --- | --- |
| Static NAT | Maps one private address to one public address. |
| Dynamic NAT | Maps private addresses to available public addresses from a pool. |
| PAT / NAT Overload | Allows multiple private devices to share a public IP using different transport ports. |
| Destination NAT (DNAT) | Translates the destination address of incoming traffic. |
| Source NAT (SNAT) | Translates the source address of outgoing traffic. |

NAT and firewall filtering are separate functions, even though many routers implement both together.

# 🗁 Special-Purpose IP Address Ranges

Not every IPv4 address is classified as public or RFC 1918 private. Several address blocks have special purposes.

### 🖆 Important IPv4 Ranges

| Address Range | Purpose |
| --- | --- |
| `0.0.0.0/8` | Current network and special-purpose use. |
| `10.0.0.0/8` | Private network addresses. |
| `100.64.0.0/10` | Shared address space used for carrier-grade NAT (CGNAT). |
| `127.0.0.0/8` | Loopback addresses. |
| `169.254.0.0/16` | IPv4 link-local addresses. |
| `172.16.0.0/12` | Private network addresses. |
| `192.0.2.0/24` | Documentation and examples. |
| `192.168.0.0/16` | Private network addresses. |
| `198.51.100.0/24` | Documentation and examples. |
| `203.0.113.0/24` | Documentation and examples. |
| `224.0.0.0/4` | IPv4 multicast addresses. |
| `240.0.0.0/4` | Reserved for future use and special purposes. |

### 🖆 Carrier-Grade NAT (CGNAT)

The `100.64.0.0/10` range is reserved as shared address space by RFC 6598.

It is commonly used by ISPs to share public IPv4 addresses among multiple customers.

Example:

```text
Customer Router WAN IP: 100.64.10.20
ISP Public IPv4:        198.51.100.25
```

The customer router receives a shared address rather than a dedicated public IPv4 address.

The ISP performs another layer of address translation between the customer and the public Internet.

This is commonly called **double NAT** from the customer's perspective when the customer also performs NAT on their own router.

Important distinction:

* RFC 1918 addresses are private IPv4 addresses.
* `100.64.0.0/10` is shared address space.
* Shared address space is not RFC 1918 private space.
* Neither is globally routable as an ordinary public destination address.

# 🗁 Public and Private IPv6 Addresses

IPv6 uses a different addressing model from IPv4.

IPv6 does not use RFC 1918 private address ranges. Instead, it defines different address scopes and types.

### 🖆 IPv6 Global Unicast Addresses

Global Unicast Addresses (GUAs) are globally routable IPv6 addresses.

The commonly used global unicast allocation is within:

```text
2000::/3
```

Example:

```text
2001:4860:4860::8888
```

This is a globally routable IPv6 address used by Google's public DNS service.

### 🖆 IPv6 Unique Local Addresses (ULA)

Unique Local Addresses are intended for communication within private networks.

Range:

```text
fc00::/7
```

Locally assigned ULA addresses commonly use the `fd00::/8` portion.

Example:

```text
fd12:3456:789a::1
```

ULA addresses are not intended to be routed across the public Internet.

### 🖆 IPv6 Link-Local Addresses

Link-local addresses are automatically used for communication within a local network link.

Range:

```text
fe80::/10
```

Example:

```text
fe80::1
```

Link-local addresses are not routed beyond their local link.

### 🖆 IPv6 Address Comparison

| IPv6 Address Type | Range | Purpose |
| --- | --- | --- |
| Global Unicast | `2000::/3` | Globally routable communication |
| Unique Local | `fc00::/7` | Private network communication |
| Link-Local | `fe80::/10` | Communication within a local link |
| Multicast | `ff00::/8` | Communication with multicast groups |
| Loopback | `::1/128` | Communication with the local device |
| Unspecified | `::/128` | Represents an unspecified address |

### 🖆 IPv4 vs IPv6 Private Addressing

| Feature | IPv4 | IPv6 |
| --- | --- | --- |
| Private addressing | RFC 1918 | Unique Local Addresses |
| Private ranges | Three defined blocks | `fc00::/7` |
| Public addressing | Public IPv4 addresses | Global Unicast Addresses |
| Link-local | `169.254.0.0/16` | `fe80::/10` |
| NAT usage | Common for Internet access | Not inherently required for normal global connectivity |

IPv6 is designed to provide globally unique addresses at a much larger scale. NAT is not required as a standard mechanism for IPv6 Internet connectivity.

# 🗁 How to Identify Your IP Address

Linux provides several commands for checking local IP configuration and determining the public IP address visible to an external service.

### 🖆 Find Private IP Addresses

Use the `ip` command:

```bash
ip addr show
```

Or:

```bash
ip -4 addr show
```

To display IPv6 addresses:

```bash
ip -6 addr show
```

Example output:

```text
2: enp0s3:
    inet 192.168.1.10/24
    inet6 fe80::a00:27ff:fe4e:66a1/64
```

Here:

* `192.168.1.10` is a private IPv4 address.
* `fe80::a00:27ff:fe4e:66a1` is an IPv6 link-local address.

### 🖆 Find the Default Gateway

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev enp0s3
```

The default gateway is `192.168.1.1`.

### 🖆 Find the Public IPv4 Address

Use an external IP discovery service:

```bash
curl -4 https://api.ipify.org
```

### 🖆 Find the Public IPv6 Address

```bash
curl -6 https://api64.ipify.org
```

These commands report the address observed by the external service. If an ISP uses CGNAT, the reported public IPv4 address may belong to the ISP's NAT infrastructure rather than the customer's router.

# 🗁 Common Misconceptions

### 🖆 Private IP Addresses Are Not Secure by Themselves

Using a private IP address does not automatically make a device secure.

Security depends on firewall rules, access controls, network segmentation, authentication, and other security measures.

### 🖆 NAT Is Not the Same as a Firewall

NAT translates addresses. A firewall controls whether traffic is permitted or denied.

A router can implement both functions, but they are conceptually different.

### 🖆 Private IP Addresses Can Be Reused

Multiple independent networks can use the same private IPv4 address ranges.

For example:

```text
Home Network A: 192.168.1.10
Home Network B: 192.168.1.10
Office Network: 192.168.1.10
```

These addresses do not conflict while the networks remain separate.

### 🖆 Public IP Addresses Can Be Dynamic

A public IP address does not have to be permanent.

An ISP may assign an address dynamically, and it can change over time.

### 🖆 Not Every Non-RFC1918 Address Is Public

Special-purpose ranges, such as loopback, link-local, multicast, documentation, and shared address space, are not ordinary public unicast addresses.

### 🖆 Private IPv4 Addresses Can Communicate Without NAT

Devices on the same private network can communicate directly using their private addresses.

NAT is generally needed when private IPv4 devices access external networks through a public IPv4 address, not for ordinary communication within the private network.

# 🗁 Best Practices

* Use RFC 1918 address ranges for internal IPv4 networks.
* Plan private address ranges carefully to avoid overlapping networks.
* Use CIDR notation to allocate address blocks efficiently.
* Separate networks using VLANs and appropriate routing policies.
* Configure firewall rules to control incoming and outgoing traffic.
* Use DHCP for centralized dynamic IPv4 address assignment where appropriate.
* Reserve static addresses for infrastructure and important services when needed.
* Document public IP assignments and internal network addressing.
* Check whether an ISP connection uses CGNAT before configuring inbound services.
* Use IPv6 Global Unicast and Unique Local addressing according to the network design.
* Do not treat NAT as a replacement for firewall security.

# 🗁 Practical Exercises

### 🖆 Exercise: Classify IPv4 Addresses

Identify whether each address belongs to an RFC 1918 private range, a special-purpose range, or ordinary public unicast space.

| IP Address | Classification |
| --- | --- |
| `192.168.1.100` | ? |
| `10.10.10.10` | ? |
| `172.20.5.10` | ? |
| `172.35.10.10` | ? |
| `8.8.8.8` | ? |
| `127.0.0.1` | ? |
| `169.254.10.20` | ? |
| `100.64.10.10` | ? |
| `192.0.2.10` | ? |
| `224.0.0.1` | ? |

### 🖆 Exercise: Analyze a Home Network

Consider the following network:

```text
Internet
   |
   |
Router
WAN: 203.0.113.10
LAN: 192.168.1.1
   |
   +--- Laptop: 192.168.1.10
   |
   +--- Phone: 192.168.1.20
   |
   +--- Desktop: 192.168.1.30
```

Answer the following questions:

* Which address is the router's WAN address?
* Which address is the router's LAN address?
* Which devices have private IP addresses?
* Can the laptop communicate directly with the phone?
* What happens when the laptop accesses an Internet website?
* Which networking technique allows the devices to share an IPv4 Internet connection?

### 🖆 Exercise: Inspect Your Linux Network

Run these commands:

```bash
ip addr show
ip route
ip -4 addr show
ip -6 addr show
curl -4 https://api.ipify.org
```

Identify:

* Your local IPv4 address.
* Your local IPv6 addresses.
* Your default gateway.
* Your externally observed public IPv4 address.
* Whether your router's WAN address matches the externally observed public address, if you can access its configuration.

# 🗁 Interview Questions

### 🖆 Basic Questions

* What is a public IP address?
* What is a private IP address?
* What is the difference between public and private IP addresses?
* Which RFC defines private IPv4 address ranges?
* What are the three RFC 1918 address blocks?
* Can two different networks use the same private IP address?
* Can a public IP address be dynamic?

### 🖆 Intermediate Questions

* Why do private IPv4 networks commonly use NAT?
* What is the difference between NAT and PAT?
* Can two devices with private IP addresses communicate without NAT?
* What is the difference between RFC 1918 private space and CGNAT shared address space?
* Why might a customer behind CGNAT be unable to host an externally accessible service?
* What is the difference between an IPv6 Global Unicast Address and a Unique Local Address?

### 🖆 Advanced Questions

* Why is NAT not considered a complete security mechanism?
* How does an ISP implement CGNAT?
* What happens when two private networks with overlapping address ranges are connected through a VPN?
* Why does IPv6 generally not require NAT for Internet connectivity?
* How would you design an internal IP addressing plan for a large enterprise network?

# 🗁 Summary

Public and private IP addresses serve different networking purposes.

* **Public IP addresses** are globally routable addresses used for communication across the public Internet.
* **Private IP addresses** are used within internal networks and can be reused across independent networks.
* **RFC 1918** defines three private IPv4 address blocks.
* **NAT** commonly allows private IPv4 devices to access the Internet through public IPv4 addresses.
* **CGNAT** allows ISPs to share public IPv4 addresses among multiple customers.
* **IPv6** uses Global Unicast Addresses for global communication and Unique Local Addresses for private network communication.

Understanding public and private addressing is fundamental to IP addressing, routing, NAT, firewall configuration, and network architecture.

# 🗁 References

* [RFC 1918 — Address Allocation for Private Internets](https://datatracker.ietf.org/doc/html/rfc1918)
* [RFC 6598 — IANA-Reserved IPv4 Prefix for Shared Address Space](https://datatracker.ietf.org/doc/html/rfc6598)
* [RFC 4193 — Unique Local IPv6 Unicast Addresses](https://datatracker.ietf.org/doc/html/rfc4193)
* [RFC 4291 — IP Version 6 Addressing Architecture](https://datatracker.ietf.org/doc/html/rfc4291)
* [IANA IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)
* [IANA IPv6 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml)
