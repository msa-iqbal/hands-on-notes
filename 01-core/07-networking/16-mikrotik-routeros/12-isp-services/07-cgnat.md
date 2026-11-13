# Carrier-Grade NAT (CGNAT)

# 🗁 What Is CGNAT?

Carrier-Grade Network Address Translation (CGNAT) is a NAT technique used by Internet Service Providers (ISPs) to allow multiple subscribers to share a limited number of public IPv4 addresses.

Traditional NAT translates private IP addresses into public IP addresses at a customer premises router. CGNAT performs another translation inside the ISP network, allowing many customers to share the ISP's public IPv4 address pool.

CGNAT is commonly used because publicly routable IPv4 addresses are limited.

# 🗁 Why ISPs Use CGNAT

ISPs use CGNAT for several reasons:

* IPv4 address conservation.
* Supporting large subscriber networks with limited public IPv4 allocations.
* Reducing the number of public IPv4 addresses required per customer.
* Extending IPv4 service availability while transitioning toward IPv6.

CGNAT is generally an IPv4 address conservation strategy rather than a replacement for IPv6.

# 🗁 Private IPv4 and Shared Address Space

An ISP needs an internal addressing plan for subscribers behind CGNAT.

Common address ranges include:

**RFC 1918 private IPv4 ranges**

| Address range | Description |
| --- | --- |
| `10.0.0.0/8` | Private Class A range |
| `172.16.0.0/12` | Private Class B range |
| `192.168.0.0/16` | Private Class C range |

**RFC 6598 Shared Address Space**

| Address range | Description |
|---|---|
| `100.64.0.0/10` | Shared Address Space |

The `100.64.0.0/10` range is specifically reserved for shared address space used in carrier-grade NAT environments.

It is not an RFC 1918 private address range, although it is commonly used for subscriber addressing inside ISP networks.

Example subscriber allocation:

```text
100.64.0.0/10
```

An ISP may divide this range into smaller subnetworks and allocate them to different access routers or subscriber groups.

# 🗁 CGNAT Network Architecture

A typical CGNAT architecture contains the following components:

```text
                  Internet
                     |
             Public IPv4 Network
                     |
               ISP Upstream
                     |
              CGNAT Router
                     |
             ISP Core Network
                     |
               Access Router
                     |
              Subscriber VLAN
                     |
               Customer CPE
                     |
                LAN Devices
```

The traffic flow is:

* A customer device sends traffic through its CPE router.
* The CPE may translate the device's private address into a subscriber address.
* The ISP access router forwards the subscriber traffic toward the CGNAT router.
* The CGNAT router translates the subscriber address into an ISP public IPv4 address and source port.
* The translated traffic reaches the Internet.
* Return traffic is translated back to the correct subscriber connection.

# 🗁 Understanding NAT444

NAT444 describes a common topology where traffic passes through three IPv4 addressing domains:

* Customer private IPv4 network.
* ISP shared address network.
* Public IPv4 network.

The customer router performs the first NAT operation, and the ISP CGNAT device performs the second.

Example:

```text
Customer Device
192.168.1.100
       |
       v
Customer CPE
192.168.1.1
       |
       | Customer NAT
       v
Subscriber Address
100.64.10.25
       |
       | ISP CGNAT
       v
Public IPv4
198.51.100.10
       |
       v
Internet
```

This arrangement is commonly called NAT444 because it involves three IPv4 address domains and two NAT stages.

# 🗁 CGNAT Translation and PAT

CGNAT commonly uses Port Address Translation (PAT).

PAT allows multiple internal subscribers to share one public IPv4 address by assigning different source ports to their connections.

Example:

| Subscriber source | Public translated source |
| --- | --- |
| `100.64.10.25:51000` | `198.51.100.10:30001` |
| `100.64.10.26:51000` | `198.51.100.10:30002` |
| `100.64.10.27:51000` | `198.51.100.10:30003` |

Although the subscribers use the same source port internally, the CGNAT device assigns different public source ports.

The translation table tracks the original and translated connection information so return traffic reaches the correct subscriber.

# 🗁 Public IPv4 Address Pool

An ISP needs a public IPv4 address pool for CGNAT translation.

Example documentation-only address:

```text
198.51.100.10-198.51.100.20
```

The `198.51.100.0/24` range is reserved for documentation and examples. It must not be used as a real public Internet address pool.

In production, replace example addresses with public IPv4 addresses assigned to the ISP.

A larger public pool can support more simultaneous connections and reduce port exhaustion.

# 🗁 CGNAT Deployment Planning

Before deploying CGNAT, an ISP should plan the following:

* Subscriber address allocation.
* Public IPv4 address pool.
* Routing between access routers and CGNAT.
* NAT rule design.
* Firewall policy.
* Connection tracking capacity.
* Port allocation strategy.
* Logging and subscriber identification.
* Monitoring and troubleshooting.
* IPv6 deployment strategy.

A poorly planned CGNAT deployment can cause port exhaustion, performance problems, and difficulty identifying subscribers associated with public IP activity.

# 🗁 Preparing the RouterOS CGNAT Network

This example uses two RouterOS routers:

| Router | Responsibility |
| --- | --- |
| Access Router | Connects subscribers and routes their traffic |
| CGNAT Router | Performs translation into public IPv4 addresses |

Example addressing plan:

| Component | Address |
| --- | --- |
| Subscriber network | `100.64.10.0/24` |
| Access Router | `10.10.10.1/30` |
| CGNAT Router | `10.10.10.2/30` |
| Example public address | `198.51.100.10` |

The subscriber network must be routed toward the CGNAT router.

# 🗁 Configure the Access Router

Assign the subscriber-facing gateway address:

```routeros
/ip/address
add address=100.64.10.1/24 interface=bridge-subscribers comment="CGNAT subscriber gateway"
```

Assign the point-to-point address toward the CGNAT router:

```routeros
/ip/address
add address=10.10.10.1/30 interface=ether2 comment="CGNAT transit"
```

Configure a default route toward the CGNAT router:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=10.10.10.2 comment="Default route to CGNAT"
```

Subscriber traffic should use addresses from the planned shared address range.

The subscriber gateway and access network must be configured according to the ISP's actual PPPoE, VLAN, or DHCP architecture.

# 🗁 Configure the CGNAT Router

Assign the transit address:

```routeros
/ip/address
add address=10.10.10.2/30 interface=ether2 comment="Access router transit"
```

Configure the public-facing address:

```routeros
/ip/address
add address=198.51.100.10/24 interface=ether1 comment="Documentation example only"
```

Add a route back to the subscriber network:

```routeros
/ip/route
add dst-address=100.64.10.0/24 gateway=10.10.10.1 comment="Subscriber return route"
```

The example public address is documentation-only. A production router must use the ISP's assigned public address and correct upstream routing.

# 🗁 Configure Basic CGNAT with Source NAT

Create a source NAT rule on the CGNAT router:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=100.64.10.0/24 out-interface=ether1 action=src-nat to-addresses=198.51.100.10 comment="CGNAT subscriber translation"
```

This rule translates traffic originating from the subscriber network into the configured public address.

Replace the example address with a real ISP-assigned public IPv4 address before deployment.

For a dynamic public-facing address, masquerade can be used instead:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=100.64.10.0/24 out-interface=ether1 action=masquerade comment="CGNAT using dynamic WAN address"
```

Use either the static source NAT rule or the masquerade rule for the same traffic path, according to the address allocation design.

# 🗁 Using a Public Address Pool

When an ISP has multiple public IPv4 addresses, it can configure a public address pool.

Example using documentation-only addresses:

```routeros
/ip/pool
add name=cgnat-public-pool ranges=198.51.100.10-198.51.100.20
```

A source NAT rule can reference the range:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=100.64.10.0/24 out-interface=ether1 action=src-nat to-addresses=198.51.100.10-198.51.100.20 comment="CGNAT public address pool"
```

RouterOS selects a translation address from the configured range as connections are established.

The public addresses must be routed to the CGNAT router by the upstream network. Merely configuring addresses in a NAT rule does not make them reachable from the Internet.

# 🗁 Configure Firewall Forwarding

NAT does not automatically permit traffic through the firewall.

The CGNAT router needs an appropriate forward-chain policy.

Example for a controlled lab environment:

```routeros
/ip/firewall/filter
add chain=forward connection-state=established,related action=accept comment="Allow established and related"
add chain=forward connection-state=invalid action=drop comment="Drop invalid connections"
add chain=forward src-address=100.64.10.0/24 out-interface=ether1 action=accept comment="Allow subscriber Internet traffic"
```

These rules are illustrative. Integrate them into the existing firewall policy, preserving rule order and any required restrictions.

The router's input chain should separately restrict management access to authorized administration networks.

# 🗁 Understanding Connection Tracking

RouterOS connection tracking maintains information about active connections and their NAT translations.

Inspect connection tracking:

```routeros
/ip/firewall/connection/print detail
```

Inspect NAT rules:

```routeros
/ip/firewall/nat/print detail
```

Monitor connection tracking capacity:

```routeros
/ip/firewall/connection/tracking/print
```

Connection tracking is essential for translating return traffic to the correct subscriber.

Existing connection entries may retain their original NAT translations after NAT rules are changed. Test new connections when validating NAT configuration changes.

# 🗁 Port Exhaustion

A public IPv4 address has a finite TCP and UDP port space.

When too many subscribers share the same public address, available source ports and connection-tracking resources can become limiting factors.

Common causes include:

* Large numbers of simultaneous connections.
* Applications creating many short-lived sessions.
* Peer-to-peer traffic.
* Malware generating excessive outbound connections.
* Poorly designed port allocation.
* Insufficient public IPv4 capacity.

Possible mitigation strategies:

* Increase the public IPv4 address pool.
* Distribute subscribers across multiple CGNAT routers.
* Monitor connection and port utilization.
* Apply reasonable subscriber connection limits.
* Identify abnormal traffic patterns.
* Design port allocation and capacity around actual subscriber demand.

A larger public address pool can reduce contention, but it does not eliminate connection-tracking or hardware capacity limits.

# 🗁 CGNAT Logging and Subscriber Identification

CGNAT logging is essential for correlating public Internet activity with the original subscriber.

A useful translation record should identify:

* Original subscriber IPv4 address.
* Original source port.
* Translated public IPv4 address.
* Translated source port.
* Protocol, such as TCP or UDP.
* Accurate timestamp.
* Relevant translation or connection event.

A public IP address alone is not sufficient to identify a subscriber when many subscribers share it.

The ISP needs a reliable mapping between the translated public endpoint and the subscriber's original address at the relevant time.

For large deployments, logging every connection can generate substantial storage and processing requirements. Logging architecture, time synchronization, retention, and access controls should be designed before production deployment.

# 🗁 CGNAT Limitations

CGNAT introduces limitations for some applications and services.

**Inbound connections**

Unsolicited inbound connections generally cannot reach a subscriber directly because the subscriber does not own the shared public IPv4 address.

**Port forwarding**

A customer cannot independently configure ordinary public-IP port forwarding through the ISP's CGNAT layer.

**Online gaming**

Some games may report strict NAT types or experience connectivity limitations.

**VoIP and real-time communication**

Certain applications may require NAT traversal techniques such as STUN, TURN, or ICE.

**Peer-to-peer applications**

Direct inbound peer connections may not work reliably.

**IP geolocation**

Multiple subscribers can appear to originate from the same public IP address and geographic location.

**Reputation and abuse reports**

An abuse report containing only a shared public IP address may refer to multiple subscribers. Accurate port and timestamp correlation is therefore important.

# 🗁 CGNAT and IPv6

IPv6 provides a long-term alternative to IPv4 address sharing.

With IPv6, an ISP can allocate globally unique IPv6 prefixes to subscribers, reducing the need for carrier-grade IPv4 translation.

A practical ISP migration strategy may include:

* IPv4 CGNAT for legacy IPv4 connectivity.
* Native IPv6 addressing for IPv6-capable subscribers.
* Dual-stack service where possible.
* IPv6-only access with an appropriate IPv4 transition mechanism where suitable.

CGNAT and IPv6 can operate together during the transition period.

# 🗁 CGNAT Security Considerations

A CGNAT router is a critical network component and should be secured carefully.

Recommended practices:

* Restrict router management access to trusted administration networks.
* Use a controlled forward-chain firewall policy.
* Avoid exposing unnecessary management services to the Internet.
* Monitor CPU, memory, connection tracking, and interface utilization.
* Keep RouterOS updated.
* Use accurate time synchronization.
* Protect subscriber translation logs.
* Maintain configuration backups.
* Monitor abnormal connection and traffic patterns.
* Separate management, subscriber, and transit networks where appropriate.

CGNAT is not a substitute for firewall security. Translation alone does not provide a complete security policy.

# 🗁 Monitoring CGNAT

Monitor the CGNAT router regularly.

Check active connections:

```routeros
/ip/firewall/connection/print count-only
```

Inspect connections:

```routeros
/ip/firewall/connection/print detail
```

Check NAT configuration:

```routeros
/ip/firewall/nat/print
```

Monitor interfaces:

```routeros
/interface/monitor-traffic ether1
/interface/monitor-traffic ether2
```

Check system resources:

```routeros
/system/resource/print
```

Monitor:

* Active connection count.
* CPU utilization.
* Memory usage.
* Public address utilization.
* Interface throughput.
* Packet drops.
* Subscriber complaints.
* Translation and logging health.

# 🗁 Troubleshooting CGNAT

**Subscribers cannot access the Internet**

Check subscriber addressing, gateway configuration, routes, firewall forwarding, NAT rules, and upstream connectivity.

```routeros
/ip/address/print
/ip/route/print
/ip/firewall/nat/print
/ip/firewall/filter/print
```

**NAT translation is not working**

Verify that traffic matches the NAT rule's source address and outgoing interface.

```routeros
/ip/firewall/nat/print stats
```

Generate a new connection after correcting a rule because existing connection-tracking entries may retain previous translations.

**Return traffic is not reaching subscribers**

Check the route from the CGNAT router to the subscriber network and verify that the access router has a valid route back to the CGNAT router.

**Some applications fail**

Investigate port allocation, application NAT requirements, connection tracking, and whether the application depends on unsolicited inbound connectivity.

**Public IP pool is not working**

Verify that the public addresses are assigned or routed correctly by the upstream provider. Check the source NAT rule and upstream return routing.

**CGNAT router performance is poor**

Inspect CPU, memory, interface utilization, and connection count. Review traffic distribution and hardware capacity.

# 🗁 CGNAT Lab

Build a basic CGNAT lab using two RouterOS CHR instances.

**Topology**

```text
                  Internet / Test Network
                           |
                           |
                     ether1
                    CGNAT Router
                     ether2
                           |
                    10.10.10.0/30
                           |
                     ether2
                    Access Router
                     ether3
                           |
                   Subscriber LAN
                    100.64.10.0/24
                           |
                     Test Client
```

**Lab objectives**

* Configure the subscriber network.
* Configure transit addressing between routers.
* Add routes between the access and CGNAT routers.
* Configure source NAT on the CGNAT router.
* Configure firewall forwarding.
* Verify subscriber Internet access.
* Inspect connection tracking.
* Test multiple subscriber clients.
* Observe how different connections share a public IPv4 address.
* Troubleshoot routing and NAT failures.

**Verification checklist**

* [ ] Subscriber clients receive valid shared addresses.
* [ ] Subscriber clients can reach their gateway.
* [ ] The access router can reach the CGNAT router.
* [ ] The CGNAT router has upstream connectivity.
* [ ] Subscriber routes reach the CGNAT router.
* [ ] Return routes reach the subscriber network.
* [ ] Source NAT translates subscriber traffic.
* [ ] Firewall forwarding permits the intended traffic.
* [ ] Connection tracking shows active translations.
* [ ] Multiple clients can access external services.

**Important lab note:** Use documentation-only public addresses only in an isolated simulation. For Internet connectivity, use a real upstream network and addresses assigned for the lab.

# 🗁 Best Practices

* Use `100.64.0.0/10` for subscriber addressing when appropriate.
* Plan subscriber subnet allocation before deployment.
* Route subscriber networks explicitly toward CGNAT routers.
* Use a properly routed public IPv4 pool.
* Monitor connection tracking and public address utilization.
* Design subscriber identification and translation logging.
* Keep router management isolated.
* Distribute traffic across multiple CGNAT devices when required.
* Test applications that depend on inbound connectivity.
* Introduce IPv6 to reduce long-term dependence on CGNAT.
* Document NAT policies, address assignments, and routing design.
