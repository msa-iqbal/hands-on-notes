# GRE and IPIP Tunnels

GRE (Generic Routing Encapsulation) and IPIP (IP-in-IP) are tunneling protocols that encapsulate network packets inside IP packets.

They allow networks to communicate across an intermediate IP network without requiring the original networks to be directly connected.

In MikroTik RouterOS, GRE and IPIP tunnels are commonly used for:

* Connecting remote office networks.
* Building site-to-site tunnels.
* Carrying private network traffic across the internet.
* Connecting routers through an ISP network.
* Establishing routed connectivity between geographically separated networks.
* Supporting routing protocols across tunnel interfaces.

GRE and IPIP provide encapsulation, not encryption. For confidentiality and authentication, combine them with IPsec or use an appropriate encrypted VPN solution.

# 🗁 What Is Tunneling?

Tunneling is a technique that encapsulates one network packet inside another packet so it can travel across an intermediate network.

The original packet becomes the payload of the outer packet.

```text
Original Packet
      |
      v
+-----------------------+
| Original IP Header    |
| Original Payload      |
+-----------------------+
      |
      v
Encapsulation
      |
      v
+-----------------------+
| Outer IP Header       |
| Tunnel Header         |
| Original IP Packet    |
+-----------------------+
      |
      v
Intermediate Network
      |
      v
Decapsulation
      |
      v
Original Packet
```

The receiving router removes the outer encapsulation and forwards the original packet.

# 🗁 What Is GRE?

GRE stands for Generic Routing Encapsulation.

It is a tunneling protocol originally developed to carry different network-layer protocols inside an IP packet.

GRE uses IP protocol number `47`.

GRE can carry:

* IPv4 packets.
* IPv6 packets.
* Other supported network-layer protocols.

GRE supports features such as tunnel keys and sequence numbers, depending on implementation.

GRE does not provide encryption, authentication, or confidentiality by itself.

# 🗁 What Is IPIP?

IPIP stands for IP-in-IP.

It encapsulates an IP packet inside another IP packet.

For IPv4 IPIP, the outer IP header uses protocol number `4`.

IPIP is simpler than GRE because it primarily provides IP packet encapsulation.

It is useful when both tunnel endpoints need straightforward IPv4 or IPv6 packet transport over an IP network.

# 🗁 GRE vs IPIP

| Feature | GRE | IPIP |
| --- | --- | --- |
| Full Name | Generic Routing Encapsulation | IP-in-IP |
| IPv4 Protocol Number | 47 | 4 |
| Encapsulation | GRE header inside IP | IP packet inside IP |
| Protocol Support | Multiple supported network-layer protocols | IP packets |
| Tunnel Key | Supported | Not supported |
| Sequence Number | Supported by implementations | Not supported |
| Encryption | No built-in encryption | No built-in encryption |
| Complexity | More flexible | Simpler |
| Common Use | Flexible routed tunnels | Simple IP tunnels |

# 🗁 GRE Tunnel Architecture

A GRE tunnel creates a logical point-to-point interface between two routers.

```text
                 Internet
                     |
          +----------+----------+
          |                     |
       Router A              Router B
       Site A                Site B
          |                     |
     LAN: 10.10.10.0/24    LAN: 10.20.20.0/24
          |                     |
     GRE Tunnel Endpoint   GRE Tunnel Endpoint
     192.168.100.1         192.168.100.2
          |                     |
          +====== GRE Tunnel ===+
```

Example addressing:

| Component | Router A | Router B |
| --- | --- | --- |
| WAN address | `203.0.113.1` | `198.51.100.1` |
| LAN network | `10.10.10.0/24` | `10.20.20.0/24` |
| Tunnel address | `192.168.100.1/30` | `192.168.100.2/30` |
| Tunnel interface | `gre-to-site-b` | `gre-to-site-a` |

The WAN addresses in this example are documentation addresses. Replace them with the actual reachable endpoint addresses in a real deployment.

# 🗁 GRE Tunnel Requirements

Before configuring GRE:

* Both routers must have reachable WAN endpoints.
* The GRE protocol must be permitted through intermediate firewalls.
* The tunnel endpoints must use matching remote and local addresses.
* The tunnel interfaces must have appropriate IP addresses.
* Routes must exist for remote networks.
* Firewall policies must allow the intended traffic.

GRE uses IP protocol number `47`, not TCP or UDP port 47.

If either router is behind NAT, verify that the NAT device supports and correctly handles GRE traffic.

# 🗁 Configuring a GRE Tunnel on Router A

Create the GRE interface:

```routeros
/interface gre
add name=gre-to-site-b local-address=203.0.113.1 remote-address=198.51.100.1 keepalive=10s,10
```

Assign the tunnel IP address:

```routeros
/ip address
add address=192.168.100.1/30 interface=gre-to-site-b
```

Verify the interface:

```routeros
/interface gre print detail
```

# 🗁 Configuring a GRE Tunnel on Router B

Create the corresponding GRE interface:

```routeros
/interface gre
add name=gre-to-site-a local-address=198.51.100.1 remote-address=203.0.113.1 keepalive=10s,10
```

Assign the tunnel IP address:

```routeros
/ip address
add address=192.168.100.2/30 interface=gre-to-site-a
```

Verify the interface:

```routeros
/interface gre print detail
```

Both routers must use each other's WAN addresses as their remote endpoints.

# 🗁 Testing GRE Connectivity

From Router A, test Router B's tunnel address:

```routeros
/ping 192.168.100.2
```

From Router B, test Router A's tunnel address:

```routeros
/ping 192.168.100.1
```

If the tunnel addresses respond, the GRE tunnel is carrying IP traffic.

If the tunnel is not working, check:

* WAN connectivity.
* GRE endpoint addresses.
* Firewall rules.
* IP protocol 47 filtering.
* Tunnel interface status.
* Keepalive settings.

# 🗁 Configuring Routing Over GRE

A tunnel interface does not automatically provide connectivity to remote LAN networks.

Static routes can be used to direct remote network traffic through the GRE tunnel.

On Router A:

```routeros
/ip route
add dst-address=10.20.20.0/24 gateway=gre-to-site-b
```

On Router B:

```routeros
/ip route
add dst-address=10.10.10.0/24 gateway=gre-to-site-a
```

The routing configuration establishes:

```text
Router A:
10.20.20.0/24 -> gre-to-site-b

Router B:
10.10.10.0/24 -> gre-to-site-a
```

Verify the routes:

```routeros
/ip route print detail
```

# 🗁 Allowing GRE Through the Firewall

GRE traffic is handled by the router's input chain because the outer GRE packets are addressed to the tunnel endpoint itself.

On Router A:

```routeros
/ip firewall filter
add chain=input action=accept protocol=gre src-address=198.51.100.1 dst-address=203.0.113.1 comment="Allow GRE from Site B"
```

On Router B:

```routeros
/ip firewall filter
add chain=input action=accept protocol=gre src-address=203.0.113.1 dst-address=198.51.100.1 comment="Allow GRE from Site A"
```

Place these rules before applicable input-chain drop rules.

The remote endpoint addresses should be restricted to trusted peers whenever possible.

# 🗁 Allowing LAN Traffic Through GRE

Traffic between the LAN networks passes through the forward chain.

On Router A:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.10.10.0/24 dst-address=10.20.20.0/24 comment="Allow Site A to Site B"
```

On Router B:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.20.20.0/24 dst-address=10.10.10.0/24 comment="Allow Site B to Site A"
```

Ensure that the rules are placed before any applicable forward-chain drop rules.

For bidirectional communication, both directions must be permitted by the firewall policy.

# 🗁 Configuring IPIP Tunnels

IPIP creates a tunnel that encapsulates IP packets inside outer IP packets.

RouterOS provides IPIP interfaces through:

```routeros
/interface ipip
```

The configuration process is similar to GRE, but IPIP does not use a GRE header.

# 🗁 IPIP Tunnel Topology

```text
                  Internet
                      |
            +---------+---------+
            |                   |
         Router A            Router B
            |                   |
       WAN: 203.0.113.1    WAN: 198.51.100.1
            |                   |
       IPIP Interface       IPIP Interface
       192.168.200.1        192.168.200.2
            |                   |
       LAN A               LAN B
    10.10.10.0/24       10.20.20.0/24
```

Example tunnel addressing:

| Component | Router A | Router B |
| --- | --- | --- |
| WAN address | `203.0.113.1` | `198.51.100.1` |
| Tunnel address | `192.168.200.1/30` | `192.168.200.2/30` |
| LAN network | `10.10.10.0/24` | `10.20.20.0/24` |

# 🗁 Configuring IPIP on Router A

Create the IPIP interface:

```routeros
/interface ipip
add name=ipip-to-site-b local-address=203.0.113.1 remote-address=198.51.100.1
```

Assign an IP address:

```routeros
/ip address
add address=192.168.200.1/30 interface=ipip-to-site-b
```

# 🗁 Configuring IPIP on Router B

Create the corresponding interface:

```routeros
/interface ipip
add name=ipip-to-site-a local-address=198.51.100.1 remote-address=203.0.113.1
```

Assign an IP address:

```routeros
/ip address
add address=192.168.200.2/30 interface=ipip-to-site-a
```

# 🗁 Allowing IPIP Through the Firewall

IPv4 IPIP uses IP protocol number `4`.

On Router A:

```routeros
/ip firewall filter
add chain=input action=accept protocol=ipencap src-address=198.51.100.1 dst-address=203.0.113.1 comment="Allow IPIP from Site B"
```

On Router B:

```routeros
/ip firewall filter
add chain=input action=accept protocol=ipencap src-address=203.0.113.1 dst-address=198.51.100.1 comment="Allow IPIP from Site A"
```

Place the rules before applicable input-chain drop rules.

# 🗁 Configuring Routing Over IPIP

On Router A:

```routeros
/ip route
add dst-address=10.20.20.0/24 gateway=ipip-to-site-b
```

On Router B:

```routeros
/ip route
add dst-address=10.10.10.0/24 gateway=ipip-to-site-a
```

The remote LAN networks can now be routed through the IPIP tunnel, provided the firewall and return-routing policies permit the traffic.

# 🗁 GRE and IPIP Firewall Considerations

GRE and IPIP are IP protocols, not application-layer TCP or UDP services.

Their firewall requirements are different from those of OpenVPN or WireGuard.

| Tunnel | IPv4 Protocol Number | Firewall Match |
| --- | ---: | --- |
| GRE | 47 | `protocol=gre` |
| IPIP | 4 | `protocol=ipencap` |

The outer tunnel packets are processed by the input chain.

The decapsulated traffic routed through the tunnel is processed by the forward chain.

Do not confuse permission to establish a tunnel with permission for the tunneled traffic to access LAN resources.

# 🗁 NAT Considerations

NAT can interfere with GRE and IPIP tunnels, particularly when an endpoint is behind another NAT device.

Important considerations:

* Use reachable public endpoint addresses when possible.
* Ensure the NAT device supports the required IP protocol.
* Avoid unnecessary NAT between the tunnel endpoints.
* Preserve the expected outer source and destination addresses.
* Ensure that return traffic follows the correct path.

If the routers use masquerade for internet access, make sure tunnel traffic is not unintentionally translated.

For complex deployments, use explicit NAT exemptions before general masquerade rules when necessary.

# 🗁 GRE and IPIP Over IPsec

GRE and IPIP provide encapsulation but do not encrypt their traffic.

IPsec can be used to protect the tunnel traffic.

A common architecture is:

```text
                  Internet
                      |
                Encrypted IPsec
                      |
               +------+------+
               |             |
            Router A      Router B
               |             |
            GRE/IPIP       GRE/IPIP
               |             |
             LAN A         LAN B
```

The GRE or IPIP tunnel provides logical connectivity.

IPsec provides:

* Encryption.
* Peer authentication.
* Integrity protection.
* Anti-replay protection.

The IPsec configuration must match the selected tunnel design and endpoint addresses.

Avoid applying an unrelated NAT or firewall policy that changes the traffic selectors or prevents the IPsec negotiation.

# 🗁 Dynamic Routing Over GRE and IPIP

GRE and IPIP interfaces can carry routed traffic and can be used as paths for dynamic routing protocols.

For example, OSPF can establish neighbor relationships across a point-to-point tunnel.

This is useful when:

* Multiple remote networks must be exchanged.
* Routes change dynamically.
* Redundant tunnel paths exist.
* A larger routed network is being built.

Dynamic routing must be configured separately on the tunnel interfaces.

A tunnel alone does not automatically establish OSPF or BGP relationships.

# 🗁 MTU and MSS Considerations

Encapsulation increases packet size because the tunnel adds outer headers.

This reduces the available payload size unless the underlying network supports a larger MTU.

Possible symptoms include:

* Large packets failing.
* Slow file transfers.
* Websites partially loading.
* Intermittent application failures.
* Fragmentation or packet loss.

Consider:

* Physical WAN MTU.
* Tunnel interface MTU.
* Outer IP header overhead.
* GRE header overhead.
* IPsec overhead, if used.
* Path MTU Discovery.
* TCP MSS adjustment where required.

Measure the actual path before changing MTU or MSS settings.

# 🗁 Monitoring Tunnel Interfaces

Display GRE interfaces:

```routeros
/interface gre print detail
```

Display IPIP interfaces:

```routeros
/interface ipip print detail
```

Display tunnel interface statistics:

```routeros
/interface print stats
```

Display routes:

```routeros
/ip route print detail
```

Test tunnel endpoint connectivity:

```routeros
/ping 192.168.100.2
```

Inspect firewall counters:

```routeros
/ip firewall filter print stats
```

Use these commands to identify interface status, packet counters, routing problems, and firewall drops.

# 🗁 GRE Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| GRE interface does not work | Incorrect local or remote endpoint |
| Tunnel peer unreachable | WAN routing or firewall issue |
| Tunnel repeatedly becomes unavailable | Keepalive failures or unstable connectivity |
| Tunnel works but LAN access fails | Missing routes or firewall permissions |
| One-way communication | Missing return route or asymmetric filtering |
| Large transfers fail | MTU or fragmentation problem |
| GRE packets are dropped | IP protocol 47 blocked |
| Tunnel fails behind NAT | NAT device does not correctly handle GRE |

# 🗁 IPIP Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| IPIP traffic is not received | IP protocol 4 blocked |
| Tunnel endpoints cannot communicate | Incorrect endpoint addresses |
| Tunnel is established but no LAN access | Missing static routes |
| Replies do not return | Incorrect routing or firewall policy |
| Traffic is unexpectedly translated | Incorrect NAT rule ordering |
| Large packets fail | MTU or fragmentation issue |

# 🗁 GRE vs IPIP vs WireGuard

| Feature | GRE | IPIP | WireGuard |
| --- | --- | --- | --- |
| Primary Purpose | Flexible encapsulation | IP encapsulation | Encrypted VPN |
| Encryption | No | No | Yes |
| Authentication | No built-in peer authentication | No built-in peer authentication | Public-key authentication |
| IP Protocol | 47 | 4 | UDP |
| Routing Support | Yes | Yes | Yes |
| Complexity | Moderate | Low | Low to moderate |
| Best Use | Flexible routed tunnels | Simple IP tunnels | Secure routed VPN connectivity |

Choose the protocol according to the requirements:

* Use GRE when flexible encapsulation is required.
* Use IPIP when simple IP packet encapsulation is sufficient.
* Use WireGuard when secure encrypted connectivity is required without an additional IPsec layer.
* Use GRE or IPIP over IPsec when an existing design requires tunnel encapsulation and IPsec protection.

# 🗁 Security Best Practices

* Restrict tunnel endpoints to trusted peer addresses.
* Allow only the required IP protocol through the firewall.
* Protect GRE or IPIP with IPsec when confidentiality is required.
* Use explicit routing and firewall policies.
* Avoid exposing unnecessary LAN services through tunnels.
* Monitor tunnel traffic and interface status.
* Review NAT behavior between tunnel endpoints.
* Keep RouterOS updated.
* Use separate tunnel networks that do not overlap with LAN or VPN networks.

# 🗁 Removing GRE Configuration

Remove the GRE tunnel interface:

```routeros
/interface gre
remove [find name=gre-to-site-b]
```

Remove the corresponding route:

```routeros
/ip route
remove [find dst-address=10.20.20.0/24]
```

Remove firewall rules created specifically for the tunnel:

```routeros
/ip firewall filter
remove [find comment="Allow GRE from Site B"]
```

# 🗁 Removing IPIP Configuration

Remove the IPIP interface:

```routeros
/interface ipip
remove [find name=ipip-to-site-b]
```

Remove the corresponding route:

```routeros
/ip route
remove [find dst-address=10.20.20.0/24]
```

Remove the firewall rule created for the tunnel:

```routeros
/ip firewall filter
remove [find comment="Allow IPIP from Site B"]
```

Only remove routes and firewall rules that belong to the tunnel being deleted.

# 🗁 Practical Exercises

**GRE Tunnel Lab**

* Configure two MikroTik routers with reachable WAN addresses.
* Create a GRE interface on each router.
* Assign tunnel IP addresses.
* Permit GRE protocol traffic through both firewalls.
* Test tunnel connectivity.
* Configure static routes between LAN networks.
* Verify bidirectional LAN communication.

**IPIP Tunnel Lab**

* Create IPIP interfaces on both routers.
* Assign tunnel addresses.
* Permit IP protocol 4.
* Configure static routes.
* Test remote LAN connectivity.
* Compare the configuration with GRE.

**Routing Lab**

* Establish a GRE tunnel.
* Configure OSPF over the tunnel.
* Advertise both LAN networks.
* Verify OSPF neighbor relationships.
* Inspect learned routes.
* Test connectivity after changing a route.

**Security Lab**

* Establish a GRE or IPIP tunnel.
* Protect the tunnel using IPsec.
* Verify that IPsec SAs are active.
* Test communication between the LANs.
* Inspect tunnel and firewall counters.

# 🗁 Lab Completion Checklist

* [ ] Understand IP encapsulation.
* [ ] Understand GRE.
* [ ] Understand IPIP.
* [ ] Know GRE protocol number 47.
* [ ] Know IPIP protocol number 4.
* [ ] Create a GRE tunnel between two routers.
* [ ] Create an IPIP tunnel between two routers.
* [ ] Assign tunnel IP addresses.
* [ ] Configure firewall permissions.
* [ ] Configure static routes.
* [ ] Test remote LAN connectivity.
* [ ] Understand NAT considerations.
* [ ] Understand MTU overhead.
* [ ] Configure dynamic routing over a tunnel.
* [ ] Understand GRE/IPIP protection using IPsec.

# 🗁 Summary

GRE and IPIP are tunneling protocols that encapsulate IP packets for transport across an intermediate network.

GRE provides more flexible encapsulation, while IPIP provides a simpler IP-in-IP mechanism.

Neither protocol provides encryption or peer authentication by itself.

Important concepts include:

* Tunnel encapsulation.
* GRE and IPIP interfaces.
* Tunnel endpoint addresses.
* Tunnel IP addressing.
* Static and dynamic routing.
* Firewall protocol filtering.
* NAT considerations.
* MTU and fragmentation.
* IPsec protection.
