# Site-to-Site VPN

A site-to-site VPN connects two or more geographically separated networks through an encrypted tunnel over the internet.

Instead of requiring users at each location to connect individually to a VPN server, the routers establish a persistent VPN connection between the networks. Devices at either location can then communicate through the tunnel according to the configured routing and firewall policies.

MikroTik RouterOS v7 supports several site-to-site VPN technologies, including WireGuard, IPsec, OpenVPN, and GRE over IPsec.

This project uses **WireGuard** to establish a secure site-to-site connection between two MikroTik routers.

# 🗁 Project Objectives

By completing this project, you will learn how to:

* Design a site-to-site VPN topology.
* Configure two MikroTik routers.
* Create WireGuard interfaces and key pairs.
* Configure WireGuard peers.
* Assign tunnel IP addresses.
* Configure static routes between remote networks.
* Allow VPN traffic through firewall policies.
* Restrict access to authorized remote networks.
* Verify encrypted connectivity.
* Troubleshoot common VPN problems.

# 🗁 Understanding Site-to-Site VPN

A site-to-site VPN creates an encrypted communication path between two private networks.

Consider an organization with two offices:

* **Site A:** Main office.
* **Site B:** Branch office.

Each office has its own internet connection and private LAN.

The VPN connects their routers so that devices in both networks can communicate without exposing private traffic directly to the internet.

```text
                 Internet
                    |
          +---------+---------+
          |                   |
       ISP A                 ISP B
          |                   |
    +-------------+     +-------------+
    | MikroTik A  |     | MikroTik B  |
    |             |     |             |
    | WireGuard   |=====| WireGuard   |
    | VPN         | VPN |             |
    +-------------+     +-------------+
          |                   |
       LAN A                LAN B
          |                   |
    192.168.10.0/24     192.168.20.0/24

             Encrypted Tunnel

```

# 🗁 Network Addressing Plan

This project uses the following addressing scheme:

| Component | Site A | Site B |
|---|---|---|
| Router | MikroTik A | MikroTik B |
| WAN interface | `ether1` | `ether1` |
| LAN interface | `bridge1` | `bridge1` |
| LAN network | `192.168.10.0/24` | `192.168.20.0/24` |
| LAN gateway | `192.168.10.1` | `192.168.20.1` |
| WireGuard interface | `wg-site` | `wg-site` |
| Tunnel IP | `10.10.10.1/30` | `10.10.10.2/30` |
| WireGuard UDP port | `13231` | `13231` |

For the example, assume both sites have publicly reachable WAN addresses:

| Site | Example public address |
|---|---|
| Site A | `203.0.113.10` |
| Site B | `198.51.100.20` |

These public addresses belong to documentation-only address ranges. Replace them with the actual public addresses or reachable endpoints provided by your ISPs.

The example assumes Site A can be reached at its public address on UDP port `13231`.

# 🗁 Understanding WireGuard

WireGuard is a modern VPN protocol that uses public-key cryptography to authenticate peers and encrypt traffic.

Each WireGuard interface has:

- A private key.
- A corresponding public key.
- A UDP listening port.
- One or more IP addresses.

Each peer configuration identifies the remote public key and the IP address ranges that should be routed through that peer.

The private key must remain secret. Only the corresponding public key is shared with the remote router.

# 🗁 Preparing Both Routers

Before configuring the VPN:

- Confirm both routers have working internet connectivity.
- Confirm the LAN networks do not overlap.
- Confirm the routers have appropriate WAN and LAN interface configurations.
- Identify the public endpoint of each site.
- Confirm that UDP port `13231` can reach Site A.
- Back up both router configurations.

If either router is behind NAT, configure the upstream device to forward the WireGuard UDP port to the appropriate MikroTik router.

If an ISP uses CGNAT and does not provide inbound reachability, a direct connection may not work without an alternative reachable endpoint or relay architecture.

# 🗁 Configuring Site A WireGuard Interface

On MikroTik A, create the WireGuard interface:

```routeros
/interface/wireguard/add name=wg-site listen-port=13231 mtu=1420 comment="Site-to-Site VPN"
```

Inspect the generated public key:

```routeros
/interface/wireguard/print detail
```

Copy the public key belonging to `wg-site`.

Keep the private key confidential.

# 🗁 Configuring Site B WireGuard Interface

On MikroTik B, create the WireGuard interface:

```routeros
/interface/wireguard/add name=wg-site listen-port=13231 mtu=1420 comment="Site-to-Site VPN"
```

Inspect the interface:

```routeros
/interface/wireguard/print detail
```

Copy Site B's public key.

At this point, you should have:

| Router | Required information |
|---|---|
| MikroTik A | Site A public key |
| MikroTik B | Site B public key |

Each router needs the other router's public key to establish the peer relationship.

# 🗁 Assigning Tunnel IP Addresses

Assign an IP address to Site A's WireGuard interface:

```routeros
/ip/address/add address=10.10.10.1/30 interface=wg-site comment="VPN Tunnel"
```

Assign an IP address to Site B's WireGuard interface:

```routeros
/ip/address/add address=10.10.10.2/30 interface=wg-site comment="VPN Tunnel"
```

The tunnel network is:

```text
10.10.10.0/30
```

The two usable addresses are:

* Site A: `10.10.10.1`
* Site B: `10.10.10.2`

# 🗁 Configuring the Site A Peer

On MikroTik A, add Site B as a WireGuard peer.

Replace `SITE_B_PUBLIC_KEY` with the actual public key copied from MikroTik B.

```routeros
/interface/wireguard/peers/add interface=wg-site public-key="SITE_B_PUBLIC_KEY" endpoint-address=198.51.100.20 endpoint-port=13231 allowed-address=10.10.10.2/32,192.168.20.0/24 persistent-keepalive=25s comment="Site B"
```

The important parameters are:

| Parameter | Purpose |
|---|---|
| `interface` | Local WireGuard interface |
| `public-key` | Remote peer's public key |
| `endpoint-address` | Remote peer's reachable public address |
| `endpoint-port` | Remote WireGuard UDP port |
| `allowed-address` | Remote tunnel and LAN networks |
| `persistent-keepalive` | Periodic keepalive for NAT traversal |

The `allowed-address` parameter identifies the remote addresses associated with this peer. It also participates in WireGuard peer selection for outgoing traffic.

# 🗁 Configuring the Site B Peer

On MikroTik B, add Site A as a WireGuard peer.

Replace `SITE_A_PUBLIC_KEY` with the actual public key copied from MikroTik A.

```routeros
/interface/wireguard/peers/add interface=wg-site public-key="SITE_A_PUBLIC_KEY" endpoint-address=203.0.113.10 endpoint-port=13231 allowed-address=10.10.10.1/32,192.168.10.0/24 persistent-keepalive=25s comment="Site A"
```

Site B now knows that the Site A LAN and tunnel address belong to its remote WireGuard peer.

# 🗁 Understanding Allowed Addresses

WireGuard uses `allowed-address` to associate destination IP ranges with peers.

For this project:

| Router | Remote allowed addresses |
|---|---|
| Site A | `10.10.10.2/32`, `192.168.20.0/24` |
| Site B | `10.10.10.1/32`, `192.168.10.0/24` |

This configuration means:

* Site A sends traffic for Site B's LAN through the Site B peer.
* Site B sends traffic for Site A's LAN through the Site A peer.
* Tunnel endpoint addresses are associated with their respective remote peers.

Avoid assigning overlapping allowed-address ranges to different peers on the same WireGuard interface unless the design explicitly requires it.

# 🗁 Configuring Static Routes on Site A

Site A needs a route to Site B's LAN through the WireGuard tunnel.

On MikroTik A:

```routeros
/ip/route/add dst-address=192.168.20.0/24 gateway=wg-site comment="Route to Site B LAN"
```

This directs traffic destined for `192.168.20.0/24` through the WireGuard interface.

# 🗁 Configuring Static Routes on Site B

Site B needs a route to Site A's LAN.

On MikroTik B:

```routeros
/ip/route/add dst-address=192.168.10.0/24 gateway=wg-site comment="Route to Site A LAN"
```

This directs traffic destined for `192.168.10.0/24` through the VPN.

RouterOS also uses WireGuard peer `allowed-address` information when selecting the peer for outgoing tunnel traffic. The static routes establish the Layer 3 path to the remote LAN.

# 🗁 Allowing WireGuard Traffic Through the Firewall

WireGuard uses UDP transport.

The router receiving incoming WireGuard packets must permit the configured UDP listening port in its input firewall chain.

On Site A:

```routeros
/ip/firewall/filter/add chain=input action=accept protocol=udp dst-port=13231 in-interface=ether1 comment="Allow WireGuard from Site B"
```

If Site B also accepts incoming WireGuard connections on UDP port `13231`, add the corresponding rule on Site B:

```routeros
/ip/firewall/filter/add chain=input action=accept protocol=udp dst-port=13231 in-interface=ether1 comment="Allow WireGuard from Site A"
```

Place these rules before any general WAN input drop rule.

If the router uses a different WAN interface or interface list, adjust the matching condition accordingly.

# 🗁 Allowing LAN-to-LAN VPN Traffic

The VPN tunnel must also be permitted through the routers' forward firewall chains.

On Site A:

```routeros
/ip/firewall/filter/add chain=forward action=accept in-interface=bridge1 out-interface=wg-site src-address=192.168.10.0/24 dst-address=192.168.20.0/24 comment="Site A LAN to Site B LAN"
/ip/firewall/filter/add chain=forward action=accept in-interface=wg-site out-interface=bridge1 src-address=192.168.20.0/24 dst-address=192.168.10.0/24 comment="Site B LAN to Site A LAN"
```

On Site B:

```routeros
/ip/firewall/filter/add chain=forward action=accept in-interface=bridge1 out-interface=wg-site src-address=192.168.20.0/24 dst-address=192.168.10.0/24 comment="Site B LAN to Site A LAN"
/ip/firewall/filter/add chain=forward action=accept in-interface=wg-site out-interface=bridge1 src-address=192.168.10.0/24 dst-address=192.168.20.0/24 comment="Site A LAN to Site B LAN"
```

These rules should be placed before restrictive forwarding drop rules.

If the LAN interfaces are represented by VLAN interfaces rather than `bridge1`, use the actual routed interfaces in the firewall rules.

# 🗁 Reviewing NAT Configuration

Site-to-site VPN traffic should normally preserve its original private source addresses.

For example:

```text
Site A Device: 192.168.10.50
Site B Device: 192.168.20.50
```

When Site A communicates with Site B, Site B should see the source address as `192.168.10.50`, not a translated WAN address.

If the routers use a broad masquerade rule, add a NAT bypass rule before the general WAN masquerade rule.

On Site A:

```routeros
/ip/firewall/nat/add chain=srcnat action=accept src-address=192.168.10.0/24 dst-address=192.168.20.0/24 comment="Do not NAT VPN traffic"
```

On Site B:

```routeros
/ip/firewall/nat/add chain=srcnat action=accept src-address=192.168.20.0/24 dst-address=192.168.10.0/24 comment="Do not NAT VPN traffic"
```

The bypass rules must be evaluated before general source NAT rules.

NAT bypass is important when the general masquerade rule could otherwise match VPN traffic.

# 🗁 Configuring DNS for Remote Networks

If both sites use their MikroTik routers as DNS resolvers, each site's DNS configuration should allow the necessary remote LAN clients to query the appropriate resolver.

For example, Site A may use:

```routeros
/ip/dns/set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

Site B may use:

```routeros
/ip/dns/set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

If Site B clients need to use Site A's DNS resolver, configure their DHCP network to advertise `192.168.10.1` as the DNS server.

Ensure the input firewall on Site A permits DNS queries from the authorized remote network:

```routeros
/ip/firewall/filter/add chain=input action=accept in-interface=wg-site src-address=192.168.20.0/24 protocol=udp dst-port=53 comment="Allow Site B DNS UDP"
/ip/firewall/filter/add chain=input action=accept in-interface=wg-site src-address=192.168.20.0/24 protocol=tcp dst-port=53 comment="Allow Site B DNS TCP"
```

Place these rules before the final input drop rule.

# 🗁 Verifying WireGuard Peer Status

On both routers, inspect the WireGuard interface:

```routeros
/interface/wireguard/print detail
```

Inspect peer information:

```routeros
/interface/wireguard/peers/print detail
```

Look for:

* The correct public key.
* The expected endpoint address and port.
* A recent handshake.
* Increasing transmit and receive counters.
* The expected allowed-address ranges.

A successful handshake confirms that the WireGuard peers have authenticated and exchanged encrypted protocol traffic. It does not, by itself, prove that LAN-to-LAN routing and firewall policies are correct.

# 🗁 Testing the VPN Tunnel

From Site A, ping Site B's tunnel address:

```routeros
/ping 10.10.10.2
```

From Site B, ping Site A's tunnel address:

```routeros
/ping 10.10.10.1
```

Next, test the remote LAN gateway.

From Site A:

```routeros
/ping 192.168.20.1
```

From Site B:

```routeros
/ping 192.168.10.1
```

Finally, test communication between actual devices on both LANs.

For example:

```text
Site A PC: 192.168.10.50
Site B PC: 192.168.20.50
```

Test connectivity:

```bash
ping 192.168.20.50
```

A failed ping does not always mean the VPN is down. The destination device may block ICMP or have a local firewall policy that prevents the test.

# 🗁 Verifying Routing

On Site A:

```routeros
/ip/route/print detail
```

Confirm that the route to `192.168.20.0/24` uses `wg-site`.

On Site B:

```routeros
/ip/route/print detail
```

Confirm that the route to `192.168.10.0/24` uses `wg-site`.

Also inspect the active routing table and confirm that no competing route overrides the intended VPN path.

# 🗁 Monitoring VPN Traffic

Inspect peer counters:

```routeros
/interface/wireguard/peers/print stats
```

Monitor the WireGuard interface:

```routeros
/interface/monitor-traffic wg-site
```

Inspect connection tracking:

```routeros
/ip/firewall/connection/print
```

These commands help identify whether traffic is reaching the tunnel and whether packets are being forwarded as expected.

# 🗁 Common Troubleshooting Scenarios

| Problem | Possible cause | Investigation |
|---|---|---|
| No WireGuard handshake | Incorrect public key or unreachable endpoint | Check peer configuration and UDP reachability |
| Handshake works but LANs cannot communicate | Missing routes or firewall rules | Inspect routing and forward chains |
| Tunnel IPs cannot communicate | Incorrect tunnel addresses or allowed-address | Inspect WireGuard interface and peer configuration |
| One-way communication | Missing reverse route or asymmetric firewall policy | Check both routers |
| VPN works only while one side sends traffic | NAT mapping expires | Configure persistent keepalive where appropriate |
| VPN connects but remote DNS fails | DNS input policy or DHCP configuration | Check DNS services and firewall |
| Internet works but VPN traffic is NATed | General masquerade rule matches tunnel traffic | Add NAT bypass before masquerade |
| VPN traffic does not reach the intended peer | Incorrect allowed-address configuration | Check peer address assignments |
| Large transfers fail | MTU or path-MTU issue | Investigate tunnel MTU and packet size |

Useful diagnostic commands:

```routeros
/interface/wireguard/print detail
/interface/wireguard/peers/print detail
/ip/address/print
/ip/route/print detail
/ip/firewall/filter/print
/ip/firewall/nat/print
```

# 🗁 Security Considerations

A site-to-site VPN connects separate network environments, so access should be deliberately controlled.

Recommended practices:

* Protect WireGuard private keys.
* Use unique keys for different routers and peers.
* Restrict allowed-address ranges to the actual remote networks.
* Permit only required VPN traffic through the firewall.
* Avoid exposing router management services to remote networks unnecessarily.
* Keep RouterOS updated.
* Monitor peer handshakes and traffic.
* Document tunnel addresses, endpoints, and routing policies.
* Back up configurations securely.
* Review access policies whenever either site's network changes.

A VPN encrypts traffic between the routers, but it does not automatically make every device or service inside either LAN trustworthy.

# 🗁 Final Configuration Checklist

* [ ] Both WAN connections are operational.
* [ ] Site LAN networks do not overlap.
* [ ] WireGuard interface created on Site A.
* [ ] WireGuard interface created on Site B.
* [ ] Public keys exchanged correctly.
* [ ] Tunnel IP addresses assigned.
* [ ] Peer configurations completed.
* [ ] Endpoint addresses and ports verified.
* [ ] Allowed-address ranges configured.
* [ ] Static routes added on both routers.
* [ ] UDP firewall rules configured.
* [ ] LAN-to-LAN forwarding permitted.
* [ ] NAT bypass configured where required.
* [ ] WireGuard handshake verified.
* [ ] Tunnel IP connectivity tested.
* [ ] Remote LAN connectivity tested.
* [ ] DNS behavior verified if required.
* [ ] Firewall access restrictions reviewed.
* [ ] Both router configurations backed up.

# 🗁 Expected Result

After completing this project, both office networks should communicate through an encrypted WireGuard tunnel.

Site A devices can reach authorized resources in Site B, and Site B devices can reach authorized resources in Site A.

The MikroTik routers handle VPN encryption, peer authentication, routing, and firewall enforcement.

This architecture provides a practical foundation for connecting branch offices, remote infrastructure, data centers, and distributed business networks.
