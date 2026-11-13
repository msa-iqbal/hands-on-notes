# VPN Troubleshooting

VPN troubleshooting is the process of identifying and resolving problems that prevent Virtual Private Network connections from establishing, maintaining secure tunnels, or transferring traffic correctly.

A VPN creates an encrypted or encapsulated communication path across an underlying network. It can connect remote users, offices, data centers, or entire private networks.

Common VPN problems include:

* VPN tunnel does not establish.
* VPN connects but no traffic passes.
* Remote users cannot access internal resources.
* Site-to-site networks cannot communicate.
* VPN disconnects frequently.
* Only some subnets are reachable.
* DNS resolution fails over the VPN.
* VPN performance is significantly slower than expected.
* Authentication or certificate validation fails.
* NAT or firewall rules interfere with VPN traffic.

A reliable investigation separates tunnel establishment from routing, firewall, authentication, and application connectivity.

# 🗁 VPN Fundamentals

A VPN creates a logical connection over an existing network.

Common VPN types:

| VPN Type | Purpose |
| --- | --- |
| Remote-access VPN | Connects individual users to a private network |
| Site-to-site VPN | Connects two or more networks |
| IPsec VPN | Provides network-layer security |
| WireGuard | Provides encrypted IP tunneling |
| OpenVPN | Provides VPN connectivity using TLS-based security |
| GRE tunnel | Encapsulates network traffic without providing encryption by itself |
| L2TP/IPsec | Combines L2TP tunneling with IPsec security |

Example site-to-site topology:

```text
        Office A                         Office B

   LAN: 192.168.10.0/24             LAN: 192.168.20.0/24
           |                                |
        Router A                         Router B
           |                                |
           |                                |
           +---------- Internet -----------+
                     VPN Tunnel
```

The VPN tunnel must be established, but routing and firewall policies must also allow communication between the two LANs.

# 🗁 VPN Troubleshooting Layers

A VPN connection depends on several components.

| Layer or Component | What to Verify |
| --- | --- |
| Underlay network | Internet connectivity and peer reachability |
| Tunnel establishment | Handshake, negotiation, and tunnel state |
| Authentication | Credentials, keys, certificates, and identities |
| Encryption | Compatible algorithms and security parameters |
| Routing | Routes to local and remote networks |
| Firewall | Required protocols and traffic permissions |
| NAT | Correct translation and NAT traversal |
| DNS | Internal name resolution |
| MTU | Packet size and fragmentation behavior |
| Application | Service availability and access permissions |

Troubleshoot these components in order instead of repeatedly changing VPN settings.

# 🗁 Common VPN Symptoms

| Symptom | Possible Cause |
| --- | --- |
| VPN peer is unreachable | Underlay routing or firewall problem |
| Tunnel does not establish | Authentication or negotiation mismatch |
| VPN connects but traffic fails | Missing routes or firewall rules |
| One direction works only | Missing return route or asymmetric routing |
| Some remote subnets are unreachable | Incorrect traffic selectors or route advertisements |
| VPN disconnects frequently | Unstable Internet or keepalive problem |
| DNS fails over VPN | Incorrect DNS configuration or routing |
| Large transfers fail | MTU or MSS problem |
| IPsec works without NAT but fails behind NAT | NAT traversal or upstream filtering |
| WireGuard handshake fails | Endpoint, key, firewall, or reachability problem |

# 🗁 Troubleshooting Methodology

Follow a consistent sequence.

**Verify underlay connectivity**

Before troubleshooting the VPN itself, confirm that the local network can reach the remote VPN endpoint.

```bash
ping -c 4 203.0.113.10
```

If ICMP is blocked, test the actual required VPN protocol and port using packet captures or appropriate connectivity diagnostics.

**Verify the tunnel state**

Check whether the VPN is:

* Disabled.
* Connecting.
* Negotiating.
* Established.
* Disconnected.
* Waiting for traffic.

**Verify authentication and encryption**

Check credentials, keys, certificates, identities, and security parameters.

**Verify routes**

Confirm that the local system knows how to reach the remote VPN network.

**Verify firewall policies**

Check both the VPN transport traffic and the traffic carried inside the tunnel.

**Verify return routing**

Confirm that the remote network has a route back to the local network.

**Test application connectivity**

Test the actual internal service rather than relying exclusively on tunnel status.

# 🗁 IPsec VPN Troubleshooting

IPsec provides security services for IP communication.

It commonly uses:

* IKE for key exchange and security association negotiation.
* ESP for protected data transport.
* AH for authentication and integrity without encryption, although it is uncommon in modern deployments.
* NAT Traversal (NAT-T) to support IPsec communication through NAT devices.

Common IPsec ports and protocols:

| Protocol or Port | Purpose |
| --- | --- |
| UDP 500 | IKE |
| UDP 4500 | IPsec NAT Traversal |
| IP protocol 50 | ESP |
| IP protocol 51 | AH |

ESP and AH are IP protocols, not TCP or UDP ports.

# 🗁 IKE Negotiation Problems

IKE establishes security associations between IPsec peers.

Common causes of negotiation failure:

* Incorrect peer address.
* Authentication mismatch.
* Incorrect pre-shared key.
* Certificate validation failure.
* Incompatible IKE version.
* Encryption algorithm mismatch.
* Integrity algorithm mismatch.
* Diffie-Hellman group mismatch.
* Proposal or policy mismatch.
* Incorrect identity configuration.

Troubleshooting:

* Confirm both peers can reach each other.
* Verify UDP 500 and UDP 4500 reachability where required.
* Compare IKE versions.
* Compare encryption and integrity proposals.
* Verify authentication credentials.
* Check peer identities.
* Inspect negotiation logs.

A successful IKE negotiation does not necessarily mean the IPsec data path is working.

# 🗁 IPsec Phase 1 and Phase 2 Problems

Traditional IPsec terminology commonly describes two negotiation stages.

**Phase 1**

Establishes an IKE security association between peers.

**Phase 2**

Negotiates IPsec security associations for protected traffic.

In IKEv2, these are more accurately represented by the IKE SA and one or more Child SAs.

Common Phase 1 problems:

* Authentication failure.
* IKE proposal mismatch.
* Incorrect peer identity.
* UDP 500 or 4500 blocked.

Common Phase 2 problems:

* Traffic selector mismatch.
* ESP proposal mismatch.
* Incorrect local or remote subnet.
* PFS configuration mismatch.
* Lifetime or rekey negotiation issues.

Verify that both peers agree on the protected networks and security parameters.

# 🗁 IPsec Traffic Selector Problems

Traffic selectors define which traffic should be protected by an IPsec tunnel.

Example:

```text
Local network:  192.168.10.0/24
Remote network: 192.168.20.0/24
```

The peers must have compatible policies for this traffic.

Common problems:

* Incorrect local subnet.
* Incorrect remote subnet.
* Subnet mask mismatch.
* Missing additional subnet.
* Overlapping local and remote networks.
* Firewall or NAT changes the traffic before IPsec policy matching.

A tunnel may be established while packets for an incorrectly configured subnet are not encrypted or accepted.

# 🗁 IPsec NAT Traversal Problems

IPsec ESP does not use TCP or UDP ports. NAT devices may have difficulty handling native ESP traffic.

NAT Traversal encapsulates ESP in UDP, commonly using port 4500.

Common NAT-T problems:

* UDP 4500 blocked.
* Incorrect NAT configuration.
* Multiple NAT devices.
* NAT mapping expiration.
* Incorrect peer reachability.
* Incompatible NAT-T configuration.

Check:

* Whether NAT exists between peers.
* Whether NAT-T is negotiated.
* Whether UDP 4500 passes through the network.
* Whether the remote endpoint receives the expected traffic.

# 🗁 Linux IPsec Diagnostics

Linux IPsec deployments commonly use strongSwan.

Useful commands:

**Display IPsec security associations**

```bash
sudo ipsec statusall
```

**Display loaded IPsec connections**

```bash
sudo ipsec status
```

**Restart the IPsec service**

```bash
sudo systemctl restart strongswan
```

Service names vary by distribution and strongSwan packaging.

**Inspect service logs**

```bash
sudo journalctl -u strongswan
```

**Inspect kernel XFRM policies**

```bash
ip xfrm policy
```

**Inspect kernel XFRM states**

```bash
ip xfrm state
```

**Capture IKE and NAT-T traffic**

```bash
sudo tcpdump -ni any 'udp port 500 or udp port 4500'
```

**Capture native ESP traffic**

```bash
sudo tcpdump -ni any 'ip proto 50'
```

Check:

* IKE security association state.
* Child SA state.
* Installed XFRM policies.
* Installed XFRM states.
* Packet and byte counters.
* Negotiation logs.
* Traffic selectors.

# 🗁 WireGuard Troubleshooting

WireGuard is a modern VPN protocol based on public-key cryptography.

Each peer has:

* A private key.
* A public key.
* One or more allowed IP prefixes.
* An endpoint when needed.
* A listening UDP port.
* Optional persistent keepalive configuration.

Common WireGuard problems:

* Incorrect public key.
* Incorrect private key.
* Wrong endpoint address.
* UDP port blocked.
* Incorrect AllowedIPs.
* Missing route.
* Firewall blocking forwarded traffic.
* Incorrect NAT configuration.
* Overlapping peer address ranges.

WireGuard commonly uses UDP. The listening port is configurable and often defaults to UDP 51820.

# 🗁 WireGuard Handshake Problems

A WireGuard handshake establishes authenticated communication between peers.

A peer may show no recent handshake when:

* The endpoint is incorrect.
* The peer is offline.
* UDP traffic is blocked.
* The public key is incorrect.
* NAT prevents incoming traffic.
* No traffic has triggered a handshake.
* The peer's endpoint has changed.

Linux diagnostics:

```bash
sudo wg show
```

Inspect interface configuration:

```bash
ip address show
ip link show
```

Inspect routes:

```bash
ip route
```

Capture WireGuard traffic:

```bash
sudo tcpdump -ni any udp port 51820
```

Replace `51820` with the configured listening port.

# 🗁 WireGuard AllowedIPs Problems

AllowedIPs has two important roles:

* It identifies which source IP addresses are accepted from a peer.
* It determines which destination prefixes are routed through that peer when used by the system's routing configuration.

Example:

```ini
[Interface]
Address = 10.10.10.1/24
PrivateKey = SERVER_PRIVATE_KEY
ListenPort = 51820

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.10.10.2/32, 192.168.20.0/24
```

Common problems:

* Remote subnet missing from AllowedIPs.
* Incorrect peer address.
* Overlapping AllowedIPs across peers.
* Missing operating-system route.
* Incorrect source address.
* IP forwarding disabled.

A successful handshake does not prove that the correct routes and firewall permissions exist.

# 🗁 WireGuard NAT and Persistent Keepalive

A peer behind NAT may need to maintain its NAT mapping so that the remote peer can continue sending traffic.

Example:

```ini
[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = vpn.example.com:51820
AllowedIPs = 192.168.10.0/24, 10.10.10.0/24
PersistentKeepalive = 25
```

Persistent keepalive periodically sends small packets to maintain NAT or firewall state.

It is commonly useful for peers behind NAT, but it should be configured only where needed.

# 🗁 Linux WireGuard Diagnostics

Useful commands:

**Display WireGuard status**

```bash
sudo wg show
```

**Display interfaces**

```bash
ip link show
```

**Display WireGuard interface addresses**

```bash
ip address show wg0
```

**Inspect routes**

```bash
ip route show
ip route get 192.168.20.10
```

**Inspect firewall rules**

```bash
sudo nft list ruleset
```

**Inspect UDP traffic**

```bash
sudo tcpdump -ni any udp port 51820
```

**Inspect kernel messages**

```bash
sudo journalctl -k
```

Check:

* Latest handshake time.
* Transfer counters.
* Peer endpoint.
* AllowedIPs.
* Interface address.
* Routing table.
* Firewall permissions.
* IP forwarding.
* NAT behavior.

# 🗁 OpenVPN Troubleshooting

OpenVPN supports remote-access and site-to-site VPN deployments using TLS-based security.

It commonly operates over:

* UDP.
* TCP.

The listening port is configurable. UDP 1194 is a common default.

Common OpenVPN problems:

* Incorrect username or password.
* Certificate expiration.
* Certificate authority mismatch.
* Incorrect server address or port.
* TLS negotiation failure.
* Missing route push.
* Incorrect client network configuration.
* Firewall blocking tunnel traffic.
* DNS configuration failure.

# 🗁 OpenVPN Authentication and Certificate Problems

OpenVPN deployments may use certificates, username/password authentication, or both.

Check:

* Certificate validity period.
* Certificate authority.
* Client and server certificates.
* Certificate purpose and key usage.
* Certificate revocation status.
* System clock.
* Authentication logs.
* TLS configuration compatibility.

Linux service diagnostics:

```bash
sudo systemctl status openvpn-server@server
```

Service names depend on the installation and configuration.

Inspect logs:

```bash
sudo journalctl -u openvpn-server@server
```

For client deployments, inspect the relevant client service or process logs.

# 🗁 OpenVPN Routing Problems

A client can successfully connect to OpenVPN but fail to access internal networks.

Common causes:

* Server does not push the required route.
* Client does not install the route.
* Server IP forwarding is disabled.
* Firewall blocks forwarding.
* Missing return route.
* Incorrect NAT configuration.
* Overlapping client and remote subnets.

Inspect routes:

```bash
ip route
```

Check the route to the remote network:

```bash
ip route get 192.168.20.10
```

Verify that the server has a valid path back to the VPN client network.

# 🗁 GRE Tunnel Troubleshooting

Generic Routing Encapsulation (GRE) encapsulates one network-layer packet inside another.

GRE does not provide encryption or authentication by itself.

GRE commonly uses IP protocol number 47.

Common GRE problems:

* Incorrect tunnel source or destination.
* Underlay route missing.
* GRE protocol blocked.
* Tunnel interface down.
* Incorrect tunnel IP addresses.
* MTU problems.
* Routing protocol not operating across the tunnel.

Linux diagnostics:

```bash
ip link show
ip address show
ip route show
```

Capture GRE traffic:

```bash
sudo tcpdump -ni any 'ip proto 47'
```

Verify that both tunnel endpoints have reachability through the underlying network.

If confidentiality is required, GRE can be combined with IPsec.

# 🗁 L2TP/IPsec Troubleshooting

L2TP provides Layer 2 tunneling and is commonly protected using IPsec.

Common transport requirements include:

| Protocol or Port | Purpose |
| --- | --- |
| UDP 500 | IKE negotiation |
| UDP 4500 | IPsec NAT Traversal |
| UDP 1701 | L2TP |
| IP protocol 50 | ESP |

When L2TP is protected by IPsec, the L2TP traffic is normally carried inside the protected IPsec transport.

Common problems:

* IPsec negotiation failure.
* Incorrect L2TP credentials.
* UDP traffic blocked.
* Incorrect PPP configuration.
* Address pool exhausted.
* Missing routes.
* Firewall blocking the assigned client network.

Troubleshoot the IPsec layer first, then the L2TP and PPP layers.

# 🗁 Routing Problems Across VPNs

A working tunnel does not automatically install the required routes.

Example:

```text
Office A LAN: 192.168.10.0/24
Office B LAN: 192.168.20.0/24

VPN Tunnel:
10.10.10.1 ↔ 10.10.10.2
```

Office A needs a route to Office B:

```text
192.168.20.0/24 via VPN
```

Office B needs a route back to Office A:

```text
192.168.10.0/24 via VPN
```

Common routing problems:

* Missing route to the remote subnet.
* Incorrect tunnel next hop.
* Overlapping networks.
* Incorrect policy routing.
* Missing return route.
* Traffic sent through the normal Internet gateway.
* Dynamic routing protocol failure over the tunnel.

Linux route inspection:

```bash
ip route
ip route get 192.168.20.10
```

Test both directions whenever possible.

# 🗁 VPN Firewall Problems

VPN deployments require firewall permissions for both tunnel establishment and tunneled traffic.

There are two separate traffic categories:

**VPN transport traffic**

Examples:

* IKE.
* ESP.
* WireGuard UDP.
* OpenVPN UDP or TCP.
* GRE.
* L2TP.

**Traffic inside the tunnel**

Examples:

* ICMP.
* DNS.
* SSH.
* HTTPS.
* Internal application traffic.

Allowing the VPN transport protocol does not automatically permit every packet inside the tunnel.

Check:

* Input rules for traffic destined for the VPN endpoint.
* Forwarding rules for traffic passing through the router.
* Output rules for locally generated VPN traffic.
* Policies for remote subnets.
* Return traffic and connection tracking.
* NAT rules that may interfere with protected traffic.

# 🗁 NAT Problems Across VPNs

NAT can interfere with site-to-site VPN communication when packets are translated before matching the expected VPN policy.

Example:

```text
Expected:
192.168.10.10 → 192.168.20.10

Incorrect translation:
203.0.113.10 → 192.168.20.10
```

Common symptoms:

* Tunnel establishes but LAN traffic does not pass.
* Traffic selectors do not match.
* Remote network sees unexpected source addresses.
* Return traffic cannot reach the original client.

For policy-based IPsec, configure an appropriate NAT exemption or bypass rule before general Internet source NAT, according to the platform's packet-processing order.

For routed VPNs, verify that NAT and routing policies preserve the intended source and destination addresses.

# 🗁 MTU and MSS Problems

VPN encapsulation adds overhead to packets.

The effective payload size available inside a tunnel is smaller than the underlying interface MTU.

Symptoms of MTU problems:

* Small packets work, large packets fail.
* SSH connects but file transfers stall.
* HTTPS sessions hang.
* Certain websites load partially.
* Large ICMP packets fail.
* VPN traffic experiences intermittent packet loss.

Useful Linux commands:

```bash
ip link show
tracepath 192.168.20.10
```

Test path MTU with ICMP where supported:

```bash
ping -M do -s 1400 192.168.20.10
```

The `-M do` option requests that the packet not be fragmented on the IPv4 path. Adjust the payload size when testing.

Possible solutions:

* Correct tunnel interface MTU.
* Apply appropriate TCP MSS clamping where needed.
* Verify path MTU discovery.
* Permit required ICMP messages.
* Account for additional encapsulation overhead.

Do not reduce MTU arbitrarily without testing.

# 🗁 DNS Problems Over VPN

VPN clients may connect successfully but fail to resolve internal hostnames.

Common causes:

* Internal DNS server not configured on the client.
* DNS server unreachable through the tunnel.
* Missing route to the DNS server.
* Firewall blocks DNS traffic.
* Split-DNS configuration is incorrect.
* Search domain configuration is missing.

Test DNS server reachability:

```bash
ping -c 4 192.168.20.53
```

Query a specific DNS server:

```bash
dig @192.168.20.53 internal.example.com
```

Inspect DNS configuration:

```bash
resolvectl status
```

Check both DNS server reachability and the client's resolver configuration.

# 🗁 MikroTik RouterOS v7 VPN Diagnostics

RouterOS supports several VPN technologies. Diagnostic commands depend on the configured VPN type.

**Inspect IPsec peers**

```routeros
/ip ipsec peer print detail
```

**Inspect IPsec installed security associations**

```routeros
/ip ipsec installed-sa print detail
```

**Inspect IPsec policies**

```routeros
/ip ipsec policy print detail
```

**Inspect IPsec active peers**

```routeros
/ip ipsec active-peers print detail
```

**Inspect WireGuard interfaces**

```routeros
/interface wireguard print detail
```

**Inspect WireGuard peers**

```routeros
/interface wireguard peers print detail
```

**Inspect IP routes**

```routeros
/ip route print detail
```

**Inspect IPv6 routes**

```routeros
/ipv6 route print detail
```

**Inspect firewall filter rules**

```routeros
/ip firewall filter print detail
```

**Inspect NAT rules**

```routeros
/ip firewall nat print detail
```

**Inspect connection tracking**

```routeros
/ip firewall connection print detail
```

**Inspect interface status**

```routeros
/interface print detail
```

For RouterOS troubleshooting, verify:

* VPN peer reachability.
* IPsec or WireGuard state.
* Security association status.
* Peer identities and keys.
* Traffic selectors or AllowedIPs.
* Tunnel interface addressing.
* Routes to remote networks.
* Firewall input and forward policies.
* NAT exemptions.
* Packet and byte counters.

# 🗁 VPN Packet Capture

Packet captures help identify where VPN traffic stops.

Capture IPsec IKE and NAT-T:

```bash
sudo tcpdump -ni any 'udp port 500 or udp port 4500'
```

Capture ESP:

```bash
sudo tcpdump -ni any 'ip proto 50'
```

Capture WireGuard:

```bash
sudo tcpdump -ni any udp port 51820
```

Capture OpenVPN using its configured port:

```bash
sudo tcpdump -ni any udp port 1194
```

Capture GRE:

```bash
sudo tcpdump -ni any 'ip proto 47'
```

Capture traffic inside a Linux tunnel interface:

```bash
sudo tcpdump -ni wg0
```

Replace `wg0` with the actual tunnel interface.

Interpretation:

* No packets leave the endpoint: investigate local routing, firewall, or VPN configuration.
* Packets leave but do not arrive: investigate underlay routing, upstream filtering, or remote reachability.
* Tunnel transport works but inner packets fail: investigate tunnel routes, firewall policies, and encapsulated traffic.
* Requests arrive but replies do not return: investigate the remote route, destination firewall, and return path.

# 🗁 Practical Troubleshooting Scenarios

**Scenario: IPsec tunnel does not establish**

Check:

* Peer IP addresses.
* Underlay connectivity.
* UDP 500 and UDP 4500.
* IKE version.
* Authentication method.
* Encryption and integrity proposals.
* Peer identity.
* Negotiation logs.

Possible cause: The peers use incompatible IKE proposals.

**Scenario: WireGuard handshake works but remote LAN is unreachable**

Check:

* AllowedIPs.
* Tunnel interface addresses.
* IP forwarding.
* Firewall forwarding policies.
* Remote LAN routes.
* Return route.
* NAT behavior.

Possible cause: The remote LAN subnet is missing from the relevant routing and peer configuration.

**Scenario: OpenVPN client connects but cannot access office resources**

Check:

* Pushed routes.
* Client routing table.
* Server forwarding.
* Firewall rules.
* Office return route.
* DNS configuration.

Possible cause: The office router has no route back to the VPN client subnet.

**Scenario: VPN works for small packets but large transfers fail**

Check:

* Tunnel MTU.
* Path MTU discovery.
* ICMP filtering.
* TCP MSS.
* Encapsulation overhead.

Possible cause: The effective tunnel MTU is smaller than the packet size being transmitted.

**Scenario: Site-to-site VPN works in one direction only**

Check:

* Routes on both sites.
* Traffic selectors.
* Firewall policies.
* NAT exemption.
* Asymmetric routing.
* Return traffic capture.

Possible cause: One site is missing a route to the other site's LAN.

# 🗁 VPN Troubleshooting Checklist

* [ ] Verify underlay connectivity.
* [ ] Confirm VPN endpoint addresses.
* [ ] Check required transport protocols and ports.
* [ ] Verify authentication credentials and keys.
* [ ] Inspect certificate validity when applicable.
* [ ] Confirm encryption and negotiation compatibility.
* [ ] Check tunnel or security association state.
* [ ] Verify traffic selectors or AllowedIPs.
* [ ] Inspect tunnel interface addresses.
* [ ] Verify routes to remote networks.
* [ ] Confirm IP forwarding.
* [ ] Inspect firewall input and forward policies.
* [ ] Verify NAT exemptions and translation behavior.
* [ ] Check return routing.
* [ ] Test internal DNS resolution.
* [ ] Investigate MTU and MSS.
* [ ] Inspect VPN logs and packet counters.
* [ ] Capture packets on both tunnel endpoints.
* [ ] Document the root cause and corrective action.

# 🗁 Best Practices

* Document VPN topology and endpoint addresses.
* Maintain an inventory of local and remote subnets.
* Avoid overlapping VPN address ranges.
* Use strong authentication and modern cryptographic settings.
* Keep certificates and keys managed securely.
* Monitor tunnel status and traffic counters.
* Maintain appropriate firewall rules.
* Verify return routes after tunnel deployment.
* Configure NAT exemptions where required.
* Account for tunnel MTU and encapsulation overhead.
* Keep VPN software and router firmware updated.
* Test failover and reconnection behavior.
* Back up VPN configurations.
* Record configuration changes and troubleshooting findings.

# 🗁 Summary

VPN troubleshooting requires examining tunnel establishment, authentication, routing, firewall policies, NAT, and the underlying network.

The most important areas are:

* Underlay connectivity.
* IPsec, WireGuard, and OpenVPN negotiation.
* Authentication and encryption.
* Traffic selectors and AllowedIPs.
* Routing and return paths.
* Firewall and NAT behavior.
* DNS and MTU.
* Packet captures and VPN logs.

A VPN tunnel is only one part of the solution. Successful communication also requires correct routing and permission for the traffic carried inside the tunnel.
