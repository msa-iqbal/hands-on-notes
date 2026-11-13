# Carrier-Grade NAT (CGNAT)

Carrier-Grade Network Address Translation (CGNAT) is a network technology used by Internet Service Providers (ISPs) to share a limited number of public IPv4 addresses among multiple subscribers.

IPv4 provides approximately 4.3 billion address combinations. However, a significant portion is reserved for special purposes, private networks, and other uses. The growing number of internet-connected devices has made globally routable IPv4 addresses increasingly scarce.

CGNAT allows an ISP to translate subscriber traffic from private or shared IPv4 addresses into public IPv4 addresses at the provider network level.

Instead of assigning a dedicated public IPv4 address to every subscriber, an ISP can allow multiple subscribers to share a smaller public address pool.

CGNAT is also commonly known as:

* Carrier-Grade NAT.
* Large-Scale NAT (LSN).
* NAT444.
* Provider-Level NAT.

# 🗁 Why ISPs Use CGNAT

ISPs use CGNAT primarily to conserve public IPv4 address space.

Common reasons include:

* **IPv4 address conservation:** Reduce the number of public IPv4 addresses required by subscribers.
* **Subscriber scalability:** Support more customers using a limited public IPv4 allocation.
* **Centralized translation:** Perform address translation at the ISP network level.
* **Address management:** Manage public IPv4 resources more efficiently.
* **Operational flexibility:** Allocate public IPv4 addresses dynamically from shared pools.

CGNAT is particularly useful for ISPs that have a large subscriber base but insufficient public IPv4 addresses.

# 🗁 Understanding NAT444

NAT444 describes a network architecture in which IPv4 traffic passes through two separate NAT devices.

The name comes from the three IPv4 network domains involved:

* Customer private IPv4 network.
* ISP shared IPv4 network.
* Public IPv4 network.

The first NAT operation happens on the customer's router.

The second NAT operation happens inside the ISP network.

Example:

```text
Customer Device
      |
      | 192.168.1.10
      |
Customer Router
   (CPE NAT)
      |
      | 100.64.10.20
      |
   ISP Network
      |
      |
  CGNAT Router
   (ISP NAT)
      |
      | 203.0.113.10
      |
    Internet
```

In this example:

| Network Layer | IP Address | Purpose |
| --- | --- | --- |
| Customer device | `192.168.1.10` | Private customer address |
| CPE WAN interface | `100.64.10.20` | ISP shared address |
| CGNAT public interface | `203.0.113.10` | Public IPv4 address |

The customer router translates the customer's private address into an ISP-assigned shared address.

The ISP CGNAT router then translates that shared address into a public IPv4 address.

This is the fundamental NAT444 architecture.

# 🗁 Shared Address Space

RFC 6598 defines a special IPv4 address block for shared address space used in service-provider networks.

<badge color="success">RFC 6598</badge>

<title size="2xl" color="default" tabularNums>100.64.0.0/10</title>

The address range is:

```text
100.64.0.0 - 100.127.255.255
```

This block contains 4,194,304 IPv4 addresses.

It is commonly used between subscriber networks and ISP CGNAT infrastructure.

Important characteristics:

* It is not globally routable public IPv4 space.
* It is specifically reserved for shared address use in service-provider networks.
* It is different from RFC 1918 private address ranges.
* It should not be advertised as globally reachable internet address space.

RFC 1918 private IPv4 ranges include:

| Network | Address Range |
| --- | --- |
| `10.0.0.0/8` | `10.0.0.0 - 10.255.255.255` |
| `172.16.0.0/12` | `172.16.0.0 - 172.31.255.255` |
| `192.168.0.0/16` | `192.168.0.0 - 192.168.255.255` |

The shared address block is intended for ISP-side use, while RFC 1918 addresses are commonly used in customer and organizational networks.

# 🗁 How CGNAT Works

CGNAT translates subscriber source addresses and, when necessary, transport-layer source ports.

A typical outbound connection follows these steps:

* A customer device sends a packet to an internet destination.
* The customer's router performs its own NAT operation, if configured.
* The packet reaches the ISP network with a shared IPv4 source address.
* The CGNAT router checks its connection-tracking and translation tables.
* The CGNAT router selects a public IPv4 address and source port.
* The translated packet is forwarded to the internet.
* The destination server responds to the translated public address and port.
* The CGNAT router looks up the translation entry and forwards the response to the correct subscriber.

Example:

<text weight="semibold" color="default">Before CGNAT</text>

```text
Source IP:       100.64.10.20
Source Port:     51500
Destination IP:  93.184.216.34
Destination Port: 443
Protocol:        TCP
```

<text weight="semibold" color="default">After CGNAT</text>

```text
Source IP:       203.0.113.10
Source Port:     30001
Destination IP:  93.184.216.34
Destination Port: 443
Protocol:        TCP
```

The CGNAT router maintains a translation entry so that return traffic can reach the correct subscriber.

The public source port is important because multiple subscribers may use the same public IPv4 address simultaneously.

# 🗁 Port Address Translation (PAT)

Port Address Translation (PAT) allows multiple internal connections to share a single public IPv4 address by using different source ports.

For example:

| Subscriber | Internal Address | Public Address | Public Port |
| --- | --- | --- | ---: |
| Customer A | `100.64.10.20` | `203.0.113.10` | 30001 |
| Customer B | `100.64.10.21` | `203.0.113.10` | 30002 |
| Customer C | `100.64.10.22` | `203.0.113.10` | 30003 |
| Customer D | `100.64.10.23` | `203.0.113.10` | 30004 |

These are illustrative mappings for separate connections.

The CGNAT device must maintain unique, valid mappings for concurrent connections.

PAT is essential to large-scale address sharing because a public IPv4 address can support many simultaneous transport-layer connections.

# 🗁 CGNAT Network Architecture

A typical ISP CGNAT architecture looks like this:

```text
                 INTERNET
                     |
                     |
              Public IP Pool
                     |
                     |
               CGNAT Router
                     |
                     |
              ISP Core Network
                     |
                     |
             Aggregation Network
                     |
             +-------+-------+
             |               |
             |               |
          Access A         Access B
             |               |
          Subscribers     Subscribers
             |               |
          CPE Routers     CPE Routers
             |               |
          Customer        Customer
          Networks        Networks
```

A more detailed subscriber traffic path:

```text
Customer Device
      |
      |
Customer Router
      |
      | Private IPv4 NAT
      |
PPPoE / IPoE Access
      |
      |
BNG / BRAS
      |
      | 100.64.0.0/10
      |
ISP Core Router
      |
      |
CGNAT Router
      |
      | Public IPv4 NAT
      |
Internet
```

The exact placement of CGNAT depends on ISP architecture, routing design, subscriber scale, and traffic engineering requirements.

# 🗁 Public IPv4 Address Pool

An ISP generally maintains a pool of public IPv4 addresses for CGNAT translation.

Example:

```text
Public IPv4 Pool:

203.0.113.10
203.0.113.11
203.0.113.12
203.0.113.13
203.0.113.14
```

These addresses are from the documentation-only range `203.0.113.0/24` and are not usable as real internet addresses.

In production, the ISP uses public IPv4 addresses assigned to its organization.

The CGNAT platform can allocate addresses and ports dynamically according to its translation policy.

Public IPv4 pool planning should consider:

* Number of subscribers.
* Expected concurrent connections.
* Connection creation rate.
* Port utilization.
* Traffic volume.
* Application requirements.
* Logging capacity.
* Peak-hour usage.

The number of required public addresses cannot be calculated from subscriber count alone. Connection behavior and port consumption are important factors.

# 🗁 CGNAT Port Allocation

A CGNAT platform must allocate public source ports while preventing conflicting mappings.

There are several port allocation approaches.

**Dynamic Port Allocation**

The CGNAT device dynamically selects available public source ports for subscriber connections.

Advantages:

* Flexible allocation.
* Efficient use of available ports.
* Suitable for varying connection patterns.

Disadvantages:

* Requires maintaining translation state.
* Can generate significant logging and processing overhead.
* Port exhaustion can occur under heavy usage.

**Port Block Allocation**

The CGNAT device assigns a predefined range of public ports to a subscriber.

Example:

| Subscriber | Public IP | Allocated Port Range |
| --- | --- | --- |
| Customer A | `203.0.113.10` | 10000–10999 |
| Customer B | `203.0.113.10` | 11000–11999 |
| Customer C | `203.0.113.10` | 12000–12999 |
| Customer D | `203.0.113.10` | 13000–13999 |

Advantages:

* Predictable port allocation.
* Can simplify subscriber identification.
* Can reduce per-connection mapping-log requirements when deterministic allocation is used.

Disadvantages:

* A subscriber may exhaust an assigned block.
* Reserved ports may remain unused.
* Block size requires careful planning.

**Deterministic Port Mapping**

A deterministic mapping algorithm assigns public address and port ranges to subscribers according to predefined rules.

The ISP can calculate the subscriber associated with a public IP address and port using the mapping algorithm and relevant allocation information.

This can reduce the volume of per-connection logging.

However, deterministic mapping still requires accurate records of subscriber assignments and allocation changes.

# 🗁 CGNAT Logging

Logging is a critical part of CGNAT operations.

When multiple subscribers share a public IPv4 address, an external server generally sees the translated public IP address and source port rather than the subscriber's original shared address.

To identify the subscriber associated with a connection, an ISP may need translation records.

Important information includes:

| Field | Description |
| --- | --- |
| Timestamp | Time of the translation event |
| Protocol | TCP, UDP, or another supported protocol |
| Subscriber Identifier | Internal IP address or another subscriber identifier |
| Public IP Address | Translated public IPv4 address |
| Public Port | Translated source port |
| Allocation Information | Assigned port block, if applicable |

Example:

```text
Timestamp:       2026-11-11 10:30:00
Protocol:        TCP
Subscriber IP:   100.64.10.20
Public IP:       203.0.113.10
Public Port:     30001
```

This record is illustrative.

Accurate timestamps, synchronized clocks, reliable storage, and consistent subscriber assignment records are essential.

ISPs should follow applicable legal, regulatory, and privacy requirements when collecting and retaining CGNAT records.

RFC 6888 provides common operational requirements and recommendations for CGNAT logging.

# 🗁 CGNAT and MikroTik RouterOS

MikroTik RouterOS supports IPv4 source NAT and connection tracking, which can demonstrate the basic CGNAT translation concept.

However, a simple RouterOS source NAT rule should not automatically be treated as a complete carrier-grade deployment design.

Large ISP deployments require careful consideration of session capacity, port allocation, logging, redundancy, throughput, and operational visibility.

**Example Network**

| Parameter | Value |
| --- | --- |
| Subscriber Network | `100.64.0.0/10` |
| CGNAT Public IP | `203.0.113.10` |
| Subscriber Interface | `ether2` |
| Internet Interface | `ether1` |

The addresses are illustrative.

**Configure Source NAT**

For a single fixed public address:

```routeros
/ip firewall nat add \
    chain=srcnat \
    src-address=100.64.0.0/10 \
    out-interface=ether1 \
    action=src-nat \
    to-addresses=203.0.113.10 \
    comment="CGNAT subscriber translation"
```

This demonstrates source address translation for the shared subscriber network.

The public address must be assigned or routed to the router appropriately.

**Using a Public IP Pool**

An ISP with multiple public addresses can define a public address range:

```routeros
/ip firewall nat add \
    chain=srcnat \
    src-address=100.64.0.0/10 \
    out-interface=ether1 \
    action=src-nat \
    to-addresses=203.0.113.10-203.0.113.14 \
    comment="CGNAT public address pool"
```

This example demonstrates source NAT using an address range.

It does not implement deterministic subscriber-to-port-block allocation.

**Verify NAT Rules**

```routeros
/ip firewall nat print detail
```

**Inspect Connection Tracking**

```routeros
/ip firewall connection print
```

**Inspect Connection Statistics**

```routeros
/ip firewall connection tracking print
```

**Check Interface Traffic**

```routeros
/interface monitor-traffic ether1
```

**Important Design Consideration**

RouterOS NAT uses connection tracking to maintain translation state. The router must have sufficient resources to handle the number of concurrent connections and packet-processing workload.

For a production ISP CGNAT platform, verify that the selected platform supports the required scale, port-allocation model, logging, redundancy, and operational features.

# 🗁 CGNAT vs Regular NAT

| Feature | Regular NAT | CGNAT |
| --- | --- | --- |
| Deployment | Home or organizational network | ISP network |
| Translation location | Customer or enterprise router | Provider network |
| Typical source address | RFC 1918 private IPv4 | `100.64.0.0/10` shared IPv4 |
| Public address sharing | Often one public IP per customer router | Multiple subscribers share public IPv4 addresses |
| Scale | Small to medium | Large subscriber populations |
| Logging requirements | Usually basic connection tracking | Subscriber-to-public-IP-and-port correlation |
| Port allocation | Usually dynamic PAT | Dynamic, port-block, or deterministic allocation |
| Operational complexity | Relatively low | High |

# 🗁 CGNAT Advantages

* Conserves scarce public IPv4 addresses.
* Allows ISPs to support more subscribers.
* Reduces the need for one public IPv4 address per customer.
* Supports centralized address translation.
* Enables flexible public IPv4 pool management.
* Can extend the operational lifetime of IPv4 infrastructure.

# 🗁 CGNAT Limitations

CGNAT introduces several technical and operational limitations.

**Inbound Connectivity**

Subscribers generally cannot directly accept unsolicited inbound connections through CGNAT.

This affects:

* Hosting services at home.
* Remote desktop access.
* Self-hosted applications.
* Game servers.
* Network cameras.
* Direct peer-to-peer connections.

A public IP assignment, suitable port mapping, or another supported access method may be required.

**Port Exhaustion**

A subscriber or the entire CGNAT platform can consume available source ports.

If no suitable ports remain, new connections may fail.

**Application Compatibility**

Some applications depend on direct inbound connectivity or specific NAT behavior.

Examples include:

* Peer-to-peer applications.
* Online gaming.
* VoIP and SIP services.
* Certain VPN implementations.
* Real-time communication applications.

**Shared Public Reputation**

Multiple subscribers may use the same public IPv4 address.

If one subscriber generates abusive traffic, other subscribers sharing that address may experience restrictions from external services.

**Geolocation Issues**

External services generally identify the CGNAT public IP address rather than the subscriber's actual access location.

**Troubleshooting Complexity**

A public IP address alone may not identify the originating subscriber.

The ISP may need the exact timestamp, protocol, source port, and relevant allocation records.

# 🗁 CGNAT and Inbound Access Solutions

Several approaches can provide inbound access or improve compatibility for subscribers behind CGNAT.

| Solution | Description |
| --- | --- |
| Public IPv4 Assignment | Assigns a dedicated public address to a subscriber |
| Static NAT | Creates a fixed address translation |
| Port Forwarding | Maps selected public ports to internal services |
| VPN | Provides remote access through a reachable VPN endpoint |
| Reverse Proxy | Publishes selected internal services through an external proxy |
| IPv6 | Provides globally routable IPv6 connectivity where supported |
| PCP | Allows supported applications or devices to request NAT mappings |

The appropriate approach depends on the application, ISP architecture, security requirements, and available public address resources.

# 🗁 CGNAT and IPv6

IPv6 provides a much larger address space than IPv4.

A properly designed IPv6 network can assign globally unique IPv6 addresses to subscriber devices or customer networks without requiring IPv4-style address sharing.

ISPs can use dual-stack deployments to provide:

* IPv4 connectivity through CGNAT.
* Native IPv6 connectivity through routed IPv6 prefixes.

Example:

```text
Customer Network
       |
       +--------------------+
       |                    |
       |                    |
     IPv4                 IPv6
       |                    |
       |                    |
    CGNAT              Native IPv6
       |                    |
       |                    |
    Internet              Internet
```

IPv6 does not automatically eliminate the need for CGNAT because many internet services remain IPv4-only.

A dual-stack deployment allows subscribers to use native IPv6 where available while retaining IPv4 connectivity through the ISP's IPv4 infrastructure.

# 🗁 CGNAT vs NAT64

CGNAT and NAT64 solve different networking problems.

| Feature | CGNAT | NAT64 |
| --- | --- | --- |
| Main Purpose | Share public IPv4 addresses | Enable communication between IPv6 and IPv4 networks |
| Translation | IPv4 to IPv4 | IPv6 to IPv4 and return |
| Typical Use | IPv4 subscriber networks | IPv6-only subscriber networks accessing IPv4 services |
| Common Architecture | NAT444 | NAT64 with DNS64 or other address-discovery mechanisms |
| Address Sharing | Yes | Depends on NAT64 deployment |

NAT64 is commonly deployed with DNS64, which synthesizes IPv6 addresses for IPv4-only destinations.

# 🗁 CGNAT Troubleshooting

| Problem | Possible Cause | Troubleshooting |
| --- | --- | --- |
| Subscriber cannot access the internet | Incorrect NAT or routing | Check subscriber routes, NAT rules, and upstream connectivity |
| Some connections fail | Port exhaustion | Inspect port utilization and allocation capacity |
| Online games have connectivity problems | Restricted inbound connectivity or NAT behavior | Check application requirements and available NAT traversal options |
| Subscriber cannot host a server | Unsolicited inbound traffic blocked by CGNAT | Provide public IPv4 or another supported access method |
| External service blocks multiple customers | Shared public IP reputation | Investigate abuse reports and consider address allocation changes |
| Subscriber cannot be identified from a public IP | Missing translation records | Check timestamp, protocol, source port, and logging records |
| Connections drop unexpectedly | Connection tracking or resource exhaustion | Inspect connection counts, CPU, memory, and platform logs |
| High latency during peak hours | CGNAT processing or capacity bottleneck | Monitor throughput, packet processing, and hardware utilization |
| Some applications fail while others work | Application-specific NAT compatibility | Check required protocols and NAT traversal behavior |

Useful MikroTik diagnostic commands:

```routeros
/ip firewall nat print stats

/ip firewall connection print count-only

/ip firewall connection tracking print

/system resource print

/interface monitor-traffic ether1
```

# 🗁 CGNAT Best Practices

* Use `100.64.0.0/10` for ISP shared address allocation where appropriate.
* Maintain clear separation between subscriber, shared, and public address spaces.
* Plan public IPv4 pools according to expected connection demand.
* Monitor concurrent connections and port utilization.
* Use a suitable port-allocation strategy.
* Maintain accurate subscriber-to-public-address-and-port correlation records.
* Synchronize network device clocks using reliable time sources.
* Design redundant CGNAT infrastructure where service availability requires it.
* Monitor CPU, memory, throughput, and connection-table utilization.
* Provide public IPv4 options for customers requiring inbound connectivity.
* Deploy IPv6 to reduce long-term dependence on IPv4 address sharing.
* Protect subscriber records and follow applicable data-retention and privacy requirements.

# 🗁 Practical Notes

* CGNAT allows multiple subscribers to share a limited public IPv4 address pool.
* NAT444 uses two layers of IPv4 NAT.
* `100.64.0.0/10` is the shared address block defined by RFC 6598.
* PAT uses transport-layer ports to distinguish simultaneous connections.
* CGNAT requires connection tracking and suitable translation capacity.
* Public IP address and source port information are important for subscriber identification.
* CGNAT generally prevents unsolicited inbound connections to subscribers.
* IPv6 can provide native end-to-end addressing where supported.
* CGNAT is an IPv4 conservation technique, not a replacement for IPv6 deployment.

# 🗁 References

* [RFC 6598 — IANA-Reserved IPv4 Prefix for Shared Address Space](https://www.rfc-editor.org/rfc/rfc6598)
* [RFC 6888 — Common Requirements for Carrier-Grade NATs (CGNs)](https://www.rfc-editor.org/rfc/rfc6888)
* [RFC 6269 — Issues with IP Address Sharing](https://www.rfc-editor.org/rfc/rfc6269)
* [RFC 7422 — Deterministic Address Mapping to Reduce Logging in Carrier-Grade NAT Deployments](https://www.rfc-editor.org/rfc/rfc7422)
* [RFC 6146 — Stateful NAT64](https://www.rfc-editor.org/rfc/rfc6146)
* [MikroTik RouterOS — NAT](https://help.mikrotik.com/docs/spaces/ROS/pages/3211299/NAT)
* [MikroTik RouterOS — Connection Tracking](https://manual.mikrotik.com/docs/firewall-and-quality-of-service/connection-tracking/)
