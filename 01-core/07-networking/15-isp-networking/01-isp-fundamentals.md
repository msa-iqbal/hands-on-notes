# ISP Fundamentals

An Internet Service Provider (ISP) is an organization that provides internet connectivity and related network services to individuals, businesses, and other organizations.

An ISP operates network infrastructure that connects customer networks to other networks across the internet.

This infrastructure includes routers, switches, fiber-optic links, wireless access systems, authentication servers, IP address management systems, and connections to upstream providers.

Understanding ISP fundamentals is essential for learning how internet connectivity is delivered, managed, monitored, and maintained.

# 🗁 What Is an ISP?

An Internet Service Provider provides customers with access to the internet through a communication network.

When a customer subscribes to an internet service, the ISP establishes a connection between the customer's network and the ISP's infrastructure.

The ISP then provides a path through which customer traffic can reach external networks.

Common ISP services include:

* Residential broadband internet.
* Business internet connectivity.
* Dedicated internet access.
* Fiber-optic broadband.
* Wireless broadband.
* Leased-line connectivity.
* Public IP address allocation.
* IPv6 connectivity.
* VPN and private network services.
* Managed network services.
* Hosting and domain services.

# 🗁 How an ISP Works

An ISP connects customer devices to the global internet through multiple network infrastructure layers.

Consider a customer accessing a website.

1. The customer's device sends a request through the local router.
2. The router forwards the traffic through the ISP access network.
3. The access network transports the traffic to the ISP aggregation network.
4. The ISP core network routes the traffic toward an external network.
5. An upstream provider or peering connection provides a path to the destination network.
6. The destination server processes the request and sends a response.
7. The response travels back through the internet to the customer.

**Simplified ISP connectivity**

```text
                 INTERNET
                     |
              UPSTREAM / IXP
                     |
                ISP CORE
                     |
              ISP AGGREGATION
                     |
                ACCESS NODE
                     |
                LAST MILE
                     |
              CUSTOMER ROUTER
                     |
              CUSTOMER DEVICES
```

The actual path depends on the ISP's architecture, routing policies, peering relationships, and network topology.

# 🗁 Types of Internet Service Providers

ISPs can be classified according to their network infrastructure, service coverage, and business model.

**Tier 1 ISP**

A Tier 1 network can reach the entire global internet through settlement-free peering with other Tier 1 networks, without purchasing IP transit to reach the rest of the internet.

Characteristics:

* Extensive global network infrastructure.
* Large-scale backbone connectivity.
* Extensive peering relationships.
* Direct interconnection with other major networks.

**Tier 2 ISP**

A Tier 2 ISP typically combines peering with purchased transit to reach destinations outside its peering relationships.

Characteristics:

* Regional or national coverage.
* Peering with other networks.
* Purchased upstream connectivity.
* May provide services to smaller ISPs and customers.

**Tier 3 ISP**

A Tier 3 ISP generally purchases upstream connectivity to provide internet services to its customers.

Characteristics:

* Local or regional service coverage.
* Residential and business connectivity.
* Customer access infrastructure.
* Dependence on upstream providers for internet reachability.

These classifications describe common commercial and routing relationships rather than a strict hierarchy of network quality.

# 🗁 ISP Service Models

ISPs provide different types of connectivity depending on customer requirements.

| Service Model | Description | Common Use |
| --- | --- | --- |
| Residential Broadband | Shared broadband internet access | Homes |
| Business Broadband | Business-oriented internet service | Small and medium businesses |
| Dedicated Internet Access | Dedicated bandwidth with defined service commitments | Enterprises |
| Leased Line | Private point-to-point connectivity | Enterprise networks |
| Wireless Broadband | Internet access through radio links | Rural and remote areas |
| Fiber Broadband | High-speed optical connectivity | Residential and business customers |
| Wholesale Internet | Connectivity supplied to other providers | Smaller ISPs |

# 🗁 ISP Network Components

An ISP consists of multiple interconnected components.

**Core Router**

The core router handles traffic between major parts of the ISP network and external networks.

Typical responsibilities:

* High-capacity IP forwarding.
* BGP routing.
* Internal routing.
* Traffic engineering.
* Redundant network connectivity.

**Aggregation Switch or Router**

Aggregation infrastructure collects traffic from multiple access networks and forwards it toward the ISP core.

Typical responsibilities:

* VLAN aggregation.
* Subscriber traffic transport.
* Layer 2 and Layer 3 forwarding.
* Traffic separation.
* Uplink redundancy.

**Access Network**

The access network connects subscribers to the ISP infrastructure.

Common technologies include:

* GPON.
* EPON.
* Ethernet.
* Wireless access.
* DOCSIS cable networks.
* DSL.

**OLT**

An Optical Line Terminal manages optical access connections in a passive optical network.

It is commonly installed at an ISP central office or distribution facility.

**ONU / ONT**

An Optical Network Unit or Optical Network Terminal provides the optical network termination at the subscriber side.

It converts optical signals into interfaces that customer equipment can use.

**Customer Premises Equipment (CPE)**

CPE refers to equipment installed at the customer's location.

Examples include:

* Customer routers.
* Optical network terminals.
* Wireless receivers.
* Modems.
* Ethernet switches.

# 🗁 ISP Network Layers

A common ISP architecture can be divided into four logical layers.

| Layer | Responsibility |
| --- | --- |
| Core Layer | High-speed transport and routing |
| Aggregation Layer | Collects and distributes access traffic |
| Access Layer | Connects individual subscribers |
| Customer Layer | Provides connectivity to customer devices |

**Example architecture**

```text
                   CORE LAYER
                        |
                        |
                AGGREGATION LAYER
                        |
              +---------+---------+
              |                   |
         ACCESS NODE A       ACCESS NODE B
              |                   |
          CUSTOMERS            CUSTOMERS
              |                   |
          CPE / ONT             CPE / ONT
```

This layered architecture improves scalability, network organization, and fault isolation.

# 🗁 ISP Subscriber Connectivity

Subscribers can connect to an ISP through several access and authentication methods.

**PPPoE**

Point-to-Point Protocol over Ethernet establishes logical subscriber sessions over Ethernet infrastructure.

It is widely used in broadband networks where subscriber authentication and session management are required.

**DHCP-Based Access**

The ISP assigns IP configuration dynamically through DHCP.

The customer device or router receives network configuration from the ISP.

**Static IP Assignment**

The ISP manually assigns a fixed IP address or prefix to a customer.

This is commonly used for business connectivity, servers, and dedicated services.

**Hotspot Authentication**

A customer connects to an access network and authenticates through a captive portal before receiving authorized internet access.

# 🗁 ISP Addressing and Routing

IP addressing and routing are fundamental to ISP operations.

ISPs manage IPv4 and IPv6 address allocations and determine how traffic travels between networks.

Important concepts include:

* Public IPv4 addresses.
* Private IPv4 addresses.
* IPv6 prefixes.
* Subnetting and CIDR.
* Customer IP allocation.
* Point-to-point addressing.
* Carrier-Grade NAT.
* Static routing.
* OSPF.
* BGP.
* Default routing.
* Route filtering.

An ISP may assign a public IP address directly to a customer or use private addressing with Carrier-Grade NAT, depending on its service model and available address resources.

# 🗁 ISP Upstream Connectivity

An ISP needs connectivity to external networks to provide access to the global internet.

Common upstream connectivity models include:

**IP Transit**

An ISP purchases connectivity from another network to reach destinations across the global internet.

**Peering**

Two networks exchange traffic directly between their networks under an agreed peering arrangement.

**Internet Exchange Point (IXP)**

An IXP provides a shared infrastructure where multiple independent networks can interconnect and exchange traffic.

**Simplified upstream connectivity**

```text
                  GLOBAL INTERNET
                         |
               +---------+---------+
               |                   |
          UPSTREAM ISP          INTERNET EXCHANGE
               |                   |
               +---------+---------+
                         |
                      ISP CORE
                         |
                   ISP CUSTOMERS
```

An ISP can use multiple upstream providers and peering connections to improve connectivity, resilience, and routing efficiency.

# 🗁 ISP Authentication and Subscriber Management

ISPs need to identify subscribers, authorize network access, and track service usage.

Common technologies include:

| Technology | Purpose |
| --- | --- |
| PPPoE | Establishes authenticated subscriber sessions |
| RADIUS | Centralized authentication, authorization, and accounting |
| DHCP | Assigns IP configuration |
| Hotspot | Provides captive portal authentication |
| IP Address Management | Tracks IP address allocations |
| Subscriber Management System | Manages customer accounts and service profiles |

RADIUS is particularly useful when subscriber authentication must be centralized across multiple access routers.

# 🗁 ISP Bandwidth Management

ISPs must manage available bandwidth across their infrastructure.

The total capacity of an ISP's upstream connections is usually greater than the bandwidth continuously consumed by all subscribers at the same time.

This is related to statistical multiplexing and oversubscription.

Important bandwidth concepts include:

* Bandwidth.
* Throughput.
* Utilization.
* Peak traffic.
* Average traffic.
* Contention ratio.
* Oversubscription.
* Quality of Service (QoS).
* Traffic shaping.
* Traffic prioritization.

For example, an ISP may have a 10 Gbps upstream connection and serve hundreds of customers whose combined advertised package speeds exceed 10 Gbps.

This can work because subscribers do not usually consume their maximum package speed simultaneously.

However, insufficient capacity during peak hours can cause congestion and reduced customer performance.

# 🗁 ISP Network Redundancy

Network redundancy reduces the impact of infrastructure failures.

Common redundancy mechanisms include:

**Multiple Upstream Providers**

Connecting to more than one upstream provider reduces dependence on a single external network.

**Redundant Core Routers**

Multiple core routers can provide alternative forwarding paths if one device fails.

**Redundant Fiber Links**

Separate physical fiber paths can protect connectivity against cable cuts.

**Power Redundancy**

UPS systems and backup generators help maintain network operation during power failures.

**Routing Redundancy**

Dynamic routing protocols and redundant paths allow traffic to use alternative routes when failures occur.

Redundancy must consider shared physical risks. Two logical links using the same fiber route do not provide full physical path diversity.

# 🗁 ISP Network Monitoring

ISPs must continuously monitor their infrastructure to maintain service availability and performance.

Important monitoring areas include:

* Router CPU and memory usage.
* Interface bandwidth utilization.
* Packet loss.
* Latency and jitter.
* Optical signal levels.
* Subscriber session status.
* Routing protocol status.
* Upstream link availability.
* Power and environmental conditions.
* Network device availability.

Common monitoring technologies include:

* SNMP.
* Syslog.
* NetFlow.
* IPFIX.
* ICMP monitoring.
* Network telemetry.
* Zabbix.
* LibreNMS.
* The Dude.

# 🗁 ISP Security Fundamentals

ISP infrastructure must be protected against unauthorized access, network attacks, and service disruption.

Important security practices include:

* Restrict router management access.
* Use secure administrative authentication.
* Apply firewall policies.
* Protect routing protocols.
* Filter invalid and unauthorized routes.
* Secure subscriber authentication.
* Monitor unusual traffic patterns.
* Protect customer isolation.
* Maintain configuration backups.
* Monitor network infrastructure continuously.

ISPs should also implement appropriate anti-spoofing controls, route filtering, and DDoS mitigation strategies.

# 🗁 Common ISP Terminologies

| Term | Meaning |
| --- | --- |
| ISP | Internet Service Provider |
| ASN | Autonomous System Number |
| BGP | Border Gateway Protocol |
| IXP | Internet Exchange Point |
| IP Transit | Purchased connectivity to external networks |
| Peering | Direct traffic exchange between networks |
| POP | Point of Presence |
| OLT | Optical Line Terminal |
| ONU | Optical Network Unit |
| ONT | Optical Network Terminal |
| CPE | Customer Premises Equipment |
| GPON | Gigabit Passive Optical Network |
| EPON | Ethernet Passive Optical Network |
| PPPoE | Point-to-Point Protocol over Ethernet |
| CGNAT | Carrier-Grade Network Address Translation |
| RADIUS | Remote Authentication Dial-In User Service |
| NOC | Network Operations Center |
| SLA | Service Level Agreement |
| QoS | Quality of Service |
| FTTH | Fiber to the Home |
| FTTB | Fiber to the Building |
| FTTx | Fiber to the x |

# 🗁 Practical ISP Learning Checklist

* [ ] Understand ISP service models.
* [ ] Identify core, aggregation, and access network components.
* [ ] Understand ISP network layers.
* [ ] Study subscriber connectivity methods.
* [ ] Understand public and private IP allocation.
* [ ] Learn upstream connectivity and peering.
* [ ] Understand subscriber authentication.
* [ ] Study bandwidth management and oversubscription.
* [ ] Understand network redundancy.
* [ ] Learn ISP monitoring and security fundamentals.

# 🗁 Key Takeaways

* An ISP provides internet connectivity and related network services.
* ISP infrastructure connects customer networks to external networks.
* Core, aggregation, access, and last-mile networks form the foundation of ISP architecture.
* Fiber, Ethernet, wireless, and other access technologies connect subscribers.
* PPPoE, DHCP, and static IP assignment are common subscriber connectivity methods.
* RADIUS supports centralized subscriber authentication and accounting.
* BGP enables routing between independent autonomous systems.
* Peering and IP transit provide connectivity to external networks.
* Bandwidth management and oversubscription help ISPs manage shared network capacity.
* Redundancy and monitoring are essential for reliable ISP operations.
