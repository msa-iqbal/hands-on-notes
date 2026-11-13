# 🗁 IP Addressing

**IP addressing** is a fundamental concept in computer networking. It provides logical addresses that identify devices and network interfaces, allowing them to communicate across interconnected networks.

An IP address enables devices to send and receive packets, while routers use destination IP addresses to forward packets toward their intended networks.

IP addressing is essential for:

* Identifying devices and network interfaces.
* Communicating within local and remote networks.
* Routing packets across the Internet.
* Dividing networks into smaller subnets.
* Managing public and private network resources.
* Configuring routers, servers, firewalls, and network services.

This section covers IPv4, IPv6, subnet masks, CIDR notation, subnetting, supernetting, and practical IP address configuration.

# 🗁 Topics

| File                            | Topic                   | Description                                                 |
| ------------------------------- | ----------------------- | ----------------------------------------------------------- |
| `01-ip.md`                      | IP Fundamentals         | Introduction to Internet Protocol and logical addressing    |
| `02-ipv4.md`                    | IPv4                    | IPv4 structure, format, and addressing                      |
| `03-ipv6.md`                    | IPv6                    | IPv6 architecture, notation, and address types              |
| `04-public-vs-private-ip.md`    | Public vs Private IP    | Differences between publicly routable and private addresses |
| `05-static-vs-dynamic-ip.md`    | Static vs Dynamic IP    | Manual and automatic IP configuration                       |
| `06-subnet-mask.md`             | Subnet Mask             | Network and host portions of IPv4 addresses                 |
| `07-cidr-notation.md`           | CIDR Notation           | Prefix lengths and address allocation                       |
| `08-subnetting.md`              | Subnetting              | Dividing networks into smaller networks                     |
| `09-supernetting.md`            | Supernetting            | Combining contiguous network prefixes                       |
| `10-network-vs-host-address.md` | Network vs Host Address | Understanding network identifiers and host identifiers      |
| `11-default-gateway.md`         | Default Gateway         | Forwarding traffic to other networks                        |
| `12-loopback-and-link-local.md` | Loopback and Link-Local | Special-purpose IP addresses                                |
| `13-apipa.md`                   | APIPA                   | Automatic IPv4 link-local addressing                        |
| `14-ip-addressing-practice.md`  | IP Addressing Practice  | Exercises and practical configuration                       |

# 🗁 Learning Objectives

After completing this section, you should be able to:

* Explain the purpose and structure of IP addresses.
* Understand IPv4 and IPv6 addressing.
* Distinguish public and private IP addresses.
* Understand static and dynamic IP configuration.
* Interpret subnet masks and CIDR notation.
* Calculate network addresses, host ranges, and broadcast addresses.
* Perform IPv4 subnetting and supernetting.
* Identify network and host portions of an IP address.
* Understand default gateways, loopback, link-local, and APIPA addresses.
* Configure and troubleshoot IP addresses on Ubuntu/Linux.

# 🗁 Learning Path

Follow the files in their listed order.

```text
IP Addressing
      |
      v
IP Fundamentals
      |
      v
IPv4 and IPv6
      |
      v
Public vs Private IP
      |
      v
Static vs Dynamic IP
      |
      v
Subnet Mask and CIDR
      |
      v
Subnetting and Supernetting
      |
      v
Network vs Host Address
      |
      v
Default Gateway
      |
      v
Loopback, Link-Local and APIPA
      |
      v
IP Addressing Practice
```

Understanding IPv4 addressing and binary arithmetic before studying subnetting will make the calculations much easier.

# 🗁 References

* [RFC 791 — Internet Protocol (IPv4)](https://www.rfc-editor.org/rfc/rfc791)
* [RFC 8200 — Internet Protocol, Version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 1918 — Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918)
* [RFC 4632 — Classless Inter-domain Routing (CIDR)](https://www.rfc-editor.org/rfc/rfc4632)
* [RFC 4291 — IP Version 6 Addressing Architecture](https://www.rfc-editor.org/rfc/rfc4291)
* [RFC 2131 — Dynamic Host Configuration Protocol (DHCP)](https://www.rfc-editor.org/rfc/rfc2131)
* [IANA IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)
* [IANA IPv6 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml)
* [Linux `ip` Command Manual](https://man7.org/linux/man-pages/man8/ip.8.html)
