# Network Layer Protocols

Network Layer Protocols define how devices communicate across different networks using logical addressing, packet forwarding, routing support, and network control mechanisms.

The Network Layer is primarily associated with Layer 3 of the OSI model. Its main responsibility is delivering packets from a source network to a destination network.

Important network-layer protocols and mechanisms include:

* **IP (Internet Protocol):** Provides logical addressing and packet delivery.
* **ICMP (Internet Control Message Protocol):** Provides error reporting and diagnostic messages.
* **ARP (Address Resolution Protocol):** Resolves IPv4 addresses into link-layer addresses on local networks.
* **NDP (Neighbor Discovery Protocol):** Performs neighbor discovery and address resolution for IPv6.
* **IGMP (Internet Group Management Protocol):** Manages IPv4 multicast group membership.
* **MLD (Multicast Listener Discovery):** Manages IPv6 multicast group membership.

This section explores these protocols and the mechanisms that support packet delivery across modern IP networks.

# 🗁 Table of Contents

Follow this order to build a clear understanding of Network Layer Protocols.

| SL  | Document                      | Main Focus                              |
| --- | ----------------------------- | --------------------------------------- |
| 1   | `01-icmp.md`                  | Network error reporting and diagnostics |
| 2   | `02-arp.md`                   | IPv4 address resolution                 |
| 3   | `03-ndp.md`                   | IPv6 neighbor discovery                 |
| 4   | `04-igmp-and-mld.md`          | Multicast membership management         |
| 5   | `05-fragmentation-and-mtu.md` | Packet size and fragmentation           |
| 6   | `06-ttl-and-hop-limit.md`     | Packet lifetime and forwarding          |
| 7   | `07-path-mtu-discovery.md`    | Discovering usable packet sizes         |
| 8   | `08-ping-and-traceroute.md`   | Practical network diagnostics           |

# 🗁 Learning Objectives

By completing this section, you will understand:

* The responsibilities of the network layer.
* How IP packets are delivered across networks.
* How ICMP supports network diagnostics and error reporting.
* How IPv4 ARP resolves IP addresses into MAC addresses.
* How IPv6 NDP performs neighbor discovery.
* How multicast group membership works through IGMP and MLD.
* How packet fragmentation and MTU affect network communication.
* How TTL and Hop Limit prevent packets from circulating indefinitely.
* How Path MTU Discovery identifies usable packet sizes.
* How ping and traceroute help troubleshoot network connectivity.

# 🗁 References

* [RFC 791 — Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)
* [RFC 8200 — Internet Protocol, Version 6](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 792 — Internet Control Message Protocol](https://www.rfc-editor.org/rfc/rfc792)
* [RFC 4443 — ICMPv6](https://www.rfc-editor.org/rfc/rfc4443)
* [RFC 826 — Address Resolution Protocol](https://www.rfc-editor.org/rfc/rfc826)
* [RFC 4861 — Neighbor Discovery for IPv6](https://www.rfc-editor.org/rfc/rfc4861)
* [RFC 1112 — Host Extensions for IP Multicasting](https://www.rfc-editor.org/rfc/rfc1112)
* [RFC 3376 — IGMPv3](https://www.rfc-editor.org/rfc/rfc3376)
* [RFC 3810 — MLDv2](https://www.rfc-editor.org/rfc/rfc3810)
* [RFC 1191 — Path MTU Discovery for IPv4](https://www.rfc-editor.org/rfc/rfc1191)
* [RFC 8201 — Path MTU Discovery for IPv6](https://www.rfc-editor.org/rfc/rfc8201)
* [Linux IP Documentation](https://docs.kernel.org/networking/ip-sysctl.html)
