# IPsec VPN

IPsec (Internet Protocol Security) is a suite of protocols used to secure IP communication over untrusted networks.

It provides encryption, authentication, integrity protection, and anti-replay protection for network traffic.

MikroTik RouterOS v7 supports IPsec for establishing secure VPN connections between routers, branch offices, remote networks, and other IPsec-compatible devices.

IPsec is commonly used for:

* Site-to-site VPN connections.
* Branch office connectivity.
* Secure communication between private networks.
* Remote access VPN deployments.
* Protecting traffic over public internet connections.
* Connecting MikroTik routers with other vendors' VPN gateways.

# 🗁 What Is IPsec?

IPsec is a collection of protocols and mechanisms that secure IP packets.

Unlike application-level encryption, IPsec can protect traffic at the network layer without requiring individual applications to implement encryption.

A typical site-to-site IPsec VPN establishes an encrypted communication path between two gateways.

```text
             Private Network A
             192.168.10.0/24
                    |
                    |
              MikroTik A
                    |
                 Internet
                    |
             Encrypted IPsec
                  Tunnel
                    |
              MikroTik B
                    |
                    |
             Private Network B
             192.168.20.0/24
```

Traffic between the two private networks is protected while traversing the untrusted network.

# 🗁 IPsec Security Services

IPsec provides several security services.

| Security Service | Description |
| --- | --- |
| Confidentiality | Encrypts data to prevent unauthorized reading |
| Authentication | Verifies the identity of communicating peers |
| Integrity | Detects unauthorized modification of packets |
| Anti-replay protection | Prevents reuse of previously captured packets |
| Access control | Protects traffic according to configured security policies |

These services help protect network communication against interception, modification, and impersonation.

# 🗁 IPsec Components

IPsec uses several components to establish and maintain secure communication.

**IKE (Internet Key Exchange)**

IKE negotiates security parameters and establishes authenticated security associations between IPsec peers.

RouterOS supports IKEv1 and IKEv2.

**ESP (Encapsulating Security Payload)**

ESP provides encryption, integrity protection, and authentication according to the negotiated algorithms.

ESP is commonly used for IPsec VPN tunnels.

**Security Association (SA)**

A Security Association defines the security parameters used to protect traffic.

It includes information such as encryption algorithms, keys, and lifetime settings.

**Peer**

An IPsec peer represents the remote device with which the router establishes an IKE relationship.

**Identity**

An identity defines how a peer is authenticated, for example through a pre-shared key.

**Proposal**

A proposal defines the encryption, authentication, and other cryptographic parameters negotiated for an IPsec security association.

**Policy**

An IPsec policy identifies the traffic that must be protected and specifies how that traffic is handled.

# 🗁 Understanding IKE

IKE stands for Internet Key Exchange.

It establishes secure communication between IPsec peers by negotiating cryptographic parameters and authenticating the remote device.

IKE also establishes the keys used to protect subsequent IPsec communication.

RouterOS supports:

* IKEv1.
* IKEv2.

IKEv2 is generally preferred for new deployments because of its improved protocol design and negotiation behavior.

# 🗁 IKEv1 vs IKEv2

| Feature | IKEv1 | IKEv2 |
| --- | --- | --- |
| Protocol design | Older | Newer |
| Negotiation | More complex exchange | More streamlined exchange |
| Authentication | Main Mode or Aggressive Mode | Integrated IKE authentication exchange |
| NAT traversal | Supported | Supported |
| Mobility extensions | Not native to IKEv1 | Supports MOBIKE |
| Recommendation | Legacy compatibility | Preferred for new deployments |

Use IKEv2 whenever the remote device supports it and the deployment requirements permit it.

# 🗁 Understanding ESP

ESP stands for Encapsulating Security Payload.

It is the primary IPsec protocol used to protect data traffic.

ESP can provide:

* Encryption.
* Data integrity.
* Data-origin authentication.
* Anti-replay protection.

ESP can operate in two modes:

* Transport mode.
* Tunnel mode.

For site-to-site VPN deployments, tunnel mode is commonly used.

# 🗁 Transport Mode vs Tunnel Mode

**Transport Mode**

Transport mode protects the payload of an IP packet while retaining the original IP header.

It is generally used for host-to-host protection.

```text
Original IP Packet

+------------------+----------------------+
| Original IP      | Transport Payload    |
| Header           |                      |
+------------------+----------------------+

Protected Packet

+------------------+----------------------+
| Original IP      | Encrypted Payload    |
| Header           |                      |
+------------------+----------------------+
```

**Tunnel Mode**

Tunnel mode protects an entire original IP packet and encapsulates it inside a new IP packet.

It is commonly used between VPN gateways.

```text
Original IP Packet

+------------------+----------------------+
| Original IP      | Original Payload     |
| Header           |                      |
+------------------+----------------------+

IPsec Tunnel Packet

+------------------+----------------------+
| New IP Header    | Protected Original   |
|                  | IP Packet             |
+------------------+----------------------+
```

The new outer IP header is used to transport the protected packet between the VPN endpoints.

# 🗁 Understanding IPsec Ports and Protocols

IPsec uses specific IP protocols and UDP ports.

| Protocol or Port | Purpose |
| --- | --- |
| UDP 500 | IKE negotiation |
| UDP 4500 | NAT Traversal |
| ESP (IP protocol 50) | Encrypted IPsec traffic |
| AH (IP protocol 51) | Authentication Header |

ESP and AH are IP protocols, not TCP or UDP ports.

NAT Traversal encapsulates ESP traffic inside UDP to allow IPsec communication across NAT devices.

AH is rarely used in modern deployments because its protection model is incompatible with address translation in common NAT environments.

# 🗁 IPsec NAT Traversal

NAT Traversal (NAT-T) allows IPsec communication through NAT devices.

Without NAT Traversal, address translation can interfere with IPsec packet authentication and delivery.

NAT-T encapsulates ESP traffic inside UDP, usually using port 4500.

A typical IPsec connection may use:

```text
UDP 500
   |
IKE Negotiation
   |
NAT Detection
   |
UDP 4500
   |
Encapsulated ESP Traffic
```

NAT Traversal is particularly important when one or both VPN gateways are behind NAT.

# 🗁 IPsec Network Topology

The following lab establishes a site-to-site IPsec VPN between two MikroTik routers.

```text
                 Internet
                     |
          +----------+----------+
          |                     |
       Router A              Router B
       MikroTik              MikroTik
          |                     |
       ether1                 ether1
          |                     |
        LAN A                  LAN B
   192.168.10.0/24        192.168.20.0/24
          |                     |
       Clients                Clients
```

Example addressing:

| Component | Router A | Router B |
| --- | --- | --- |
| WAN address | `A.B.C.D` | `E.F.G.H` |
| LAN network | `192.168.10.0/24` | `192.168.20.0/24` |
| LAN gateway | `192.168.10.1` | `192.168.20.1` |
| IKE version | IKEv2 | IKEv2 |
| Authentication | Pre-shared key | Pre-shared key |

Replace the example WAN addresses with the actual reachable public addresses.

Both LAN networks must be distinct and non-overlapping.

# 🗁 Preparing the IPsec Configuration

Before configuring IPsec, verify:

* Both routers have working internet connectivity.
* Both WAN endpoints are reachable.
* UDP ports 500 and 4500 are permitted.
* ESP traffic is permitted when required.
* The LAN networks do not overlap.
* Both routers use compatible cryptographic settings.
* The same pre-shared key is configured on both peers.
* Existing firewall and NAT rules do not interfere with the VPN.

For this example, use a strong, randomly generated pre-shared key and replace `REPLACE_WITH_STRONG_SECRET` in both configurations.

# 🗁 Configuring an IPsec Proposal

An IPsec proposal defines the cryptographic parameters used to protect traffic.

For Router A:

```routeros
/ip ipsec proposal
add name=site-to-site-proposal auth-algorithms=sha256 enc-algorithms=aes-256-cbc pfs-group=modp2048
```

Create the same proposal on Router B:

```routeros
/ip ipsec proposal
add name=site-to-site-proposal auth-algorithms=sha256 enc-algorithms=aes-256-cbc pfs-group=modp2048
```

Both routers must have compatible Phase 2 settings.

The selected algorithms should also be supported by the remote device.

# 🗁 Configuring an IPsec Peer on Router A

Create an IPsec peer representing Router B.

```routeros
/ip ipsec peer
add name=router-b-peer address=E.F.G.H/32 exchange-mode=ike2
```

Replace `E.F.G.H` with Router B's reachable WAN address.

This configuration specifies IKEv2 negotiation with Router B.

# 🗁 Configuring an IPsec Identity on Router A

Create an identity for Router B.

```routeros
/ip ipsec identity
add peer=router-b-peer auth-method=pre-shared-key secret="REPLACE_WITH_STRONG_SECRET"
```

The secret must match the value configured on Router B.

A pre-shared key is a shared authentication secret, not an encryption key manually assigned to the ESP tunnel.

IKE negotiates the cryptographic keys used for the connection.

# 🗁 Configuring an IPsec Peer on Router B

Create the peer representing Router A.

```routeros
/ip ipsec peer
add name=router-a-peer address=A.B.C.D/32 exchange-mode=ike2
```

Replace `A.B.C.D` with Router A's reachable WAN address.

# 🗁 Configuring an IPsec Identity on Router B

Create the matching identity.

```routeros
/ip ipsec identity
add peer=router-a-peer auth-method=pre-shared-key secret="REPLACE_WITH_STRONG_SECRET"
```

The pre-shared key must be identical on both routers.

# 🗁 Configuring an IPsec Policy on Router A

An IPsec policy identifies which traffic must be encrypted.

Router A protects traffic from its local LAN to Router B's LAN.

```routeros
/ip ipsec policy
add src-address=192.168.10.0/24 dst-address=192.168.20.0/24 tunnel=yes action=encrypt proposal=site-to-site-proposal peer=router-b-peer
```

This policy matches traffic between the two private networks.

# 🗁 Configuring an IPsec Policy on Router B

Create the reverse policy on Router B.

```routeros
/ip ipsec policy
add src-address=192.168.20.0/24 dst-address=192.168.10.0/24 tunnel=yes action=encrypt proposal=site-to-site-proposal peer=router-a-peer
```

The source and destination networks are reversed compared with Router A.

Both policies must describe the same protected traffic from opposite directions.

# 🗁 Allowing IKE and IPsec Traffic Through the Firewall

IPsec requires the appropriate traffic to reach the router.

On Router A:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=500,4500 in-interface=ether1 comment="Allow IPsec IKE and NAT-T"

add chain=input action=accept protocol=ipsec-esp in-interface=ether1 comment="Allow IPsec ESP"
```

Apply equivalent rules on Router B.

Place these rules before any input-chain drop rule that would otherwise block the traffic.

If the deployment always uses NAT Traversal, ESP is carried over UDP 4500. Allowing native ESP is still useful when direct ESP transport is possible.

Restrict the rules to the intended WAN interface and remote endpoints where practical.

# 🗁 Allowing Protected Traffic Through the Forward Chain

Traffic between the two private LANs must be permitted by the firewall policy.

On Router A:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=192.168.10.0/24 dst-address=192.168.20.0/24 ipsec-policy=out,ipsec comment="Allow LAN traffic into IPsec"

add chain=forward action=accept src-address=192.168.20.0/24 dst-address=192.168.10.0/24 ipsec-policy=in,ipsec comment="Allow traffic from IPsec"
```

On Router B, create equivalent policies with the local and remote networks reversed if its firewall requires explicit permissions.

Place these rules before relevant forward-chain drop rules.

# 🗁 Excluding IPsec Traffic from NAT

A common IPsec configuration uses policy-based IPsec.

Traffic between protected private networks should normally be excluded from ordinary internet masquerading.

On Router A:

```routeros
/ip firewall nat
add chain=srcnat action=accept src-address=192.168.10.0/24 dst-address=192.168.20.0/24 place-before=0 comment="Bypass NAT for IPsec traffic"
```

On Router B:

```routeros
/ip firewall nat
add chain=srcnat action=accept src-address=192.168.20.0/24 dst-address=192.168.10.0/24 place-before=0 comment="Bypass NAT for IPsec traffic"
```

These rules must be evaluated before general masquerade rules.

The IPsec policy must match the original private source and destination addresses.

NAT exemption is important because translating protected traffic can prevent it from matching the intended IPsec policy.

# 🗁 Understanding IPsec and Routing

Policy-based IPsec uses IPsec policies to identify traffic that must be encrypted.

Unlike a route-based VPN interface, a policy-based IPsec tunnel does not require a normal route through a dedicated tunnel interface for every protected network.

The routing table must still provide a valid path toward the remote peer's WAN endpoint.

The IPsec policy determines which matching traffic is encrypted.

This distinction is important when troubleshooting policy-based IPsec deployments.

# 🗁 Verifying IPsec Configuration

Display configured peers:

```routeros
/ip ipsec peer print detail
```

Display identities:

```routeros
/ip ipsec identity print detail
```

Display policies:

```routeros
/ip ipsec policy print detail
```

Display active installed policies:

```routeros
/ip ipsec installed-sa print detail
```

Display active Security Associations:

```routeros
/ip ipsec active-peers print detail
```

Look for:

* The correct remote peer address.
* IKEv2 configuration.
* Successful peer authentication.
* Active Security Associations.
* Matching encryption and authentication algorithms.
* Increasing packet counters.

An installed Security Association indicates that the IPsec negotiation has established the required security parameters.

# 🗁 Testing IPsec Connectivity

From Router A, test Router B's LAN gateway:

```routeros
/ping 192.168.20.1 src-address=192.168.10.1
```

From Router B, test Router A's LAN gateway:

```routeros
/ping 192.168.10.1 src-address=192.168.20.1
```

Then test communication between actual LAN clients.

For example:

```text
Client A: 192.168.10.10
Client B: 192.168.20.10
```

Verify that traffic is protected by inspecting the IPsec installed Security Association counters.

A successful ping alone does not prove that traffic is encrypted. Confirm that the matching IPsec policy and SA counters increase during the test.

# 🗁 Understanding IPsec NAT and Port Forwarding

When a MikroTik router is behind another NAT device, the upstream device must allow the required IPsec traffic.

For common IKEv2 NAT-T deployments, forward:

* UDP 500.
* UDP 4500.

Native ESP uses IP protocol 50 rather than a TCP or UDP port.

NAT Traversal generally encapsulates ESP traffic inside UDP 4500.

If both peers are behind restrictive NAT devices, endpoint reachability and NAT behavior must be considered carefully.

# 🗁 IPsec and FastTrack

FastTrack can bypass packet-processing facilities that IPsec policy-based traffic may depend on.

RouterOS handles certain IPsec-related traffic specially, but broad FastTrack rules can still interfere with intended processing when traffic is not properly excluded.

Review FastTrack rules when IPsec traffic behaves unexpectedly.

Inspect the filter rules:

```routeros
/ip firewall filter print detail
```

For a policy-based IPsec deployment, ensure protected traffic follows the required IPsec processing path and is not incorrectly FastTracked.

# 🗁 IPsec Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| No active peer | Remote endpoint unreachable or UDP blocked |
| IKE authentication failure | Incorrect pre-shared key or identity configuration |
| Phase 1 succeeds but no installed SA | Proposal or policy mismatch |
| Installed SA exists but traffic fails | Firewall, routing, NAT, or policy mismatch |
| Only one direction works | Missing reverse policy or asymmetric filtering |
| Traffic is not encrypted | Policy does not match the original traffic |
| Tunnel stops after WAN IP changes | Stale peer endpoint |
| Large packets fail | MTU or fragmentation issue |
| Connection fails behind NAT | NAT-T or upstream port-forwarding problem |

Useful diagnostic commands:

```routeros
/ip ipsec peer print detail
/ip ipsec identity print detail
/ip ipsec policy print detail
/ip ipsec active-peers print detail
/ip ipsec installed-sa print detail
```

Inspect firewall counters:

```routeros
/ip firewall filter print stats
/ip firewall nat print stats
```

Inspect active connections:

```routeros
/ip firewall connection print
```

Monitor IKE and NAT-T traffic:

```routeros
/tool torch interface=ether1 port=500,4500
```

Monitor ESP traffic:

```routeros
/tool sniffer quick interface=ether1 ip-protocol=ipsec-esp
```

These commands help identify whether negotiation, packet matching, or forwarding is failing.

# 🗁 IPsec Security Best Practices

* Prefer IKEv2 for new deployments.
* Use strong pre-shared keys or certificate-based authentication.
* Use modern cryptographic algorithms supported by both peers.
* Avoid weak algorithms such as DES and obsolete hashes.
* Restrict IKE and ESP access to intended WAN interfaces and peers.
* Use NAT exemption for protected LAN-to-LAN traffic when required.
* Keep local and remote protected networks non-overlapping.
* Protect router management services.
* Monitor Security Associations and packet counters.
* Keep RouterOS updated.
* Review firewall and FastTrack behavior.
* Document peer addresses, policies, and cryptographic settings.

# 🗁 Removing IPsec Configuration

Remove the policies created for the lab:

```routeros
/ip ipsec policy
remove [find src-address=192.168.10.0/24 dst-address=192.168.20.0/24]
```

On Router B, remove the reverse policy:

```routeros
/ip ipsec policy
remove [find src-address=192.168.20.0/24 dst-address=192.168.10.0/24]
```

Remove the peer identities:

```routeros
/ip ipsec identity
remove [find peer=router-b-peer]
```

On Router B:

```routeros
/ip ipsec identity
remove [find peer=router-a-peer]
```

Remove the peers:

```routeros
/ip ipsec peer
remove [find name=router-b-peer]
```

On Router B:

```routeros
/ip ipsec peer
remove [find name=router-a-peer]
```

Remove the custom proposals:

```routeros
/ip ipsec proposal
remove [find name=site-to-site-proposal]
```

Remove the firewall and NAT rules created for the lab separately.

Only remove entries created specifically for the lab. Do not remove production IPsec policies, peers, or firewall rules.

# 🗁 Summary

IPsec provides network-layer security for communication between routers and private networks.

Important concepts include:

* IKE and IKEv2.
* ESP and AH.
* Transport and tunnel modes.
* Security Associations.
* Peers and identities.
* Proposals and policies.
* Pre-shared key authentication.
* NAT Traversal.
* Firewall and NAT configuration.
* Policy-based site-to-site VPN.
* Monitoring and troubleshooting.

IPsec is widely used for secure site-to-site connectivity and interoperability with enterprise VPN gateways.
