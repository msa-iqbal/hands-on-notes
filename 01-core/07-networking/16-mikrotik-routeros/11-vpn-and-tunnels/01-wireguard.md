# WireGuard VPN

WireGuard is a modern VPN protocol designed to provide secure, fast, and relatively simple encrypted network communication.

MikroTik RouterOS v7 includes native WireGuard support, allowing routers to establish secure VPN connections without installing additional VPN software on the router.

WireGuard uses public-key cryptography, UDP transport, and a lightweight protocol design.

It is suitable for:

* Site-to-site VPN connections.
* Remote access VPN.
* Connecting branch offices.
* Secure administration of remote routers.
* Connecting Linux, Windows, macOS, Android, and iOS clients.
* Secure communication between private networks over the internet.

# 🗁 What Is WireGuard?

WireGuard is a Layer 3 VPN protocol that creates encrypted IP tunnels between devices.

Each WireGuard device has:

* A private key.
* A public key.
* One or more peer configurations.
* An IP address assigned to its WireGuard interface.

The private key remains secret on the device. The public key is shared with other peers to establish authenticated connections.

A WireGuard peer configuration identifies the remote device using its public key.

```text
          WireGuard VPN Connection

       Router A                  Router B
    +-------------+          +-------------+
    | Private Key |          | Private Key |
    | Public Key  |          | Public Key  |
    +-------------+          +-------------+
           |                        |
           |                        |
           +========================+
                 Encrypted Tunnel
                      UDP
```

WireGuard does not use usernames and passwords for peer authentication. Public-key cryptography identifies and authenticates peers.

# 🗁 WireGuard Architecture

WireGuard uses a peer-based architecture.

A peer is another WireGuard device with which a router communicates.

Each peer configuration can contain:

| Property | Description |
| --- | --- |
| Public Key | Public key of the remote peer |
| Endpoint | Remote peer's reachable IP address or hostname and UDP port |
| Allowed Address | IP prefixes associated with the peer |
| Persistent Keepalive | Periodic packet interval used to maintain NAT mappings |
| Preshared Key | Optional additional symmetric key |

A WireGuard interface can have multiple peers.

```text
                 MikroTik Router
                       |
                WireGuard Interface
                       |
          +------------+------------+
          |            |            |
        Peer A       Peer B       Peer C
          |            |            |
       Branch 1     Branch 2     Remote User
```

# 🗁 WireGuard Terminology

**Interface**

A WireGuard interface represents the local VPN tunnel endpoint.

It contains the local private key, listening port, and interface configuration.

**Peer**

A peer represents a remote WireGuard device identified by its public key.

**Private Key**

A secret cryptographic key used by the local device. It must not be shared.

**Public Key**

The public part of the device's key pair. It is shared with remote peers.

**Endpoint**

The IP address or hostname and UDP port used to reach a remote peer.

**Allowed Address**

An IP address or prefix associated with a peer. RouterOS uses it to determine which destination addresses belong to that peer and to validate source addresses received from it.

**Persistent Keepalive**

A periodic transmission that can help maintain a NAT mapping for a peer behind a NAT device.

# 🗁 WireGuard Requirements

Before configuring WireGuard, ensure that:

* RouterOS is running a version with WireGuard support.
* The router has working IP connectivity.
* The remote peer is reachable over the internet or another IP network.
* UDP traffic can reach the WireGuard listening port.
* Firewall rules permit the required VPN traffic.
* Each peer has a unique key pair.
* VPN address ranges do not conflict with existing networks.

WireGuard is supported in RouterOS v7.

# 🗁 WireGuard Network Topology

The following topology will be used for a basic site-to-site VPN lab.

```text
              Internet
                  |
          +-------+-------+
          |               |
      Router A         Router B
      MikroTik         MikroTik
      WAN: A.B.C.D     WAN: E.F.G.H
          |               |
      ether1            ether1
          |               |
      LAN A             LAN B
   192.168.10.0/24   192.168.20.0/24
          |               |
      Clients           Clients

        WireGuard Tunnel
       10.10.10.1/30
              |
       10.10.10.2/30
```

Example addressing:

| Component | Router A | Router B |
| --- | --- | --- |
| WAN address | `A.B.C.D` | `E.F.G.H` |
| LAN network | `192.168.10.0/24` | `192.168.20.0/24` |
| WireGuard IP | `10.10.10.1/30` | `10.10.10.2/30` |
| UDP port | `13231` | `13231` |

Replace the example WAN addresses with the actual public IP addresses or reachable endpoint addresses.

# 🗁 Creating a WireGuard Interface

Create a WireGuard interface on Router A:

```routeros
/interface wireguard
add name=wireguard1 listen-port=13231
```

Create the interface on Router B:

```routeros
/interface wireguard
add name=wireguard1 listen-port=13231
```

RouterOS automatically generates a private key and corresponding public key when the interface is created.

Display the interface configuration:

```routeros
/interface wireguard print detail
```

The output includes the interface's public key and other configuration details.

The private key is sensitive. Do not publish it or include it in shared documentation.

# 🗁 Assigning WireGuard IP Addresses

Assign an IP address to Router A's WireGuard interface:

```routeros
/ip address
add address=10.10.10.1/30 interface=wireguard1
```

Assign an IP address to Router B's WireGuard interface:

```routeros
/ip address
add address=10.10.10.2/30 interface=wireguard1
```

These addresses are used for communication inside the VPN tunnel.

# 🗁 Obtaining Public Keys

Display Router A's public key:

```routeros
/interface wireguard print detail where name=wireguard1
```

Display Router B's public key:

```routeros
/interface wireguard print detail where name=wireguard1
```

Copy the public key from each router.

You will need:

* Router A's public key when configuring Router B.
* Router B's public key when configuring Router A.

Only exchange public keys. Never exchange private keys.

# 🗁 Configuring Router A Peer

Router A needs a peer configuration representing Router B.

Assume Router B's public key is represented by `ROUTER_B_PUBLIC_KEY`.

Configure the peer on Router A:

```routeros
/interface wireguard peers
add interface=wireguard1 public-key="ROUTER_B_PUBLIC_KEY" endpoint-address=E.F.G.H endpoint-port=13231 allowed-address=10.10.10.2/32,192.168.20.0/24 persistent-keepalive=25s
```

Replace:

* `ROUTER_B_PUBLIC_KEY` with Router B's actual public key.
* `E.F.G.H` with Router B's reachable WAN address.

The `allowed-address` property identifies the remote tunnel address and the remote LAN network.

The `persistent-keepalive=25s` setting helps maintain NAT mappings when required.

# 🗁 Configuring Router B Peer

Router B needs a peer configuration representing Router A.

Assume Router A's public key is represented by `ROUTER_A_PUBLIC_KEY`.

Configure the peer on Router B:

```routeros
/interface wireguard peers
add interface=wireguard1 public-key="ROUTER_A_PUBLIC_KEY" endpoint-address=A.B.C.D endpoint-port=13231 allowed-address=10.10.10.1/32,192.168.10.0/24 persistent-keepalive=25s
```

Replace:

* `ROUTER_A_PUBLIC_KEY` with Router A's actual public key.
* `A.B.C.D` with Router A's reachable WAN address.

Router B now has a peer configuration for Router A.

# 🗁 Understanding Allowed Addresses

The `allowed-address` property is important in WireGuard.

It identifies the IP addresses and networks associated with a peer.

For Router A:

```routeros
allowed-address=10.10.10.2/32,192.168.20.0/24
```

This means:

* `10.10.10.2/32` belongs to Router B's tunnel endpoint.
* `192.168.20.0/24` is the remote LAN behind Router B.

For Router B:

```routeros
allowed-address=10.10.10.1/32,192.168.10.0/24
```

This identifies Router A's tunnel address and LAN network.

Allowed addresses also participate in peer selection for outgoing traffic and source-address validation for incoming traffic.

Avoid assigning overlapping allowed-address prefixes to different peers on the same WireGuard interface unless the design specifically accounts for that behavior.

# 🗁 Allowing WireGuard Traffic Through the Firewall

The WireGuard listening port must be reachable through the router's input firewall.

On Router A, allow UDP port `13231`:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=13231 in-interface=ether1 comment="Allow WireGuard"
```

Apply the corresponding rule on Router B:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=13231 in-interface=ether1 comment="Allow WireGuard"
```

Place the rules before any input-chain drop rule that would otherwise block the connection.

Restricting the rule to the WAN interface helps limit exposure to the intended network path.

If the router is behind another NAT device, configure UDP port forwarding on that upstream device to the MikroTik router.

# 🗁 Allowing VPN Traffic Through the Forward Chain

The input chain handles traffic destined for the router itself.

Traffic passing between the LAN and remote VPN network uses the forward chain.

On Router A:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=192.168.10.0/24 dst-address=192.168.20.0/24 comment="Allow LAN to remote LAN"

add chain=forward action=accept src-address=192.168.20.0/24 dst-address=192.168.10.0/24 comment="Allow remote LAN to local LAN"
```

On Router B, equivalent rules can be added if its firewall policy requires explicit LAN-to-VPN and VPN-to-LAN permissions.

Place these rules before relevant forward-chain drop rules.

Existing established and related connection rules may already handle return traffic, but the initial connection must be permitted by the firewall policy.

# 🗁 Configuring Routes to Remote LAN Networks

Router A needs a route to Router B's LAN network.

```routeros
/ip route
add dst-address=192.168.20.0/24 gateway=wireguard1
```

Router B needs a route to Router A's LAN network.

```routeros
/ip route
add dst-address=192.168.10.0/24 gateway=wireguard1
```

These routes direct traffic for the remote LAN through the WireGuard interface.

The peer's allowed-address configuration must also include the corresponding remote LAN prefix.

# 🗁 Verifying the WireGuard Connection

Display WireGuard interfaces:

```routeros
/interface wireguard print detail
```

Display configured peers:

```routeros
/interface wireguard peers print detail
```

Inspect peer status and handshake information:

```routeros
/interface wireguard peers print stats
```

Look for:

* Correct public keys.
* Correct endpoint addresses and ports.
* Recent handshake timestamps.
* Increasing transmitted and received byte counters.

A recent handshake indicates that the peers have successfully completed a WireGuard handshake.

# 🗁 Testing Tunnel Connectivity

From Router A, ping Router B's WireGuard IP:

```routeros
/ping 10.10.10.2 src-address=10.10.10.1
```

From Router B, ping Router A's WireGuard IP:

```routeros
/ping 10.10.10.1 src-address=10.10.10.2
```

Test the remote LAN gateway:

```routeros
/ping 192.168.20.1
```

From Router B, test Router A's LAN gateway:

```routeros
/ping 192.168.10.1
```

Finally, test connectivity between actual LAN clients.

If the tunnel IPs communicate but remote LAN clients cannot communicate, inspect routes, firewall rules, client gateways, and return paths.

# 🗁 Configuring a WireGuard Road Warrior VPN

A road warrior VPN allows a remote user to connect to an organization's private network from a laptop or mobile device.

Example topology:

```text
              Internet
                  |
                  |
             MikroTik Router
                  |
             wireguard1
             10.20.20.1/24
                  |
            WireGuard Tunnel
                  |
             Remote Client
             10.20.20.2/32
                  |
             Remote Device
```

Assume:

| Component | Address |
| --- | --- |
| Router LAN | `192.168.10.0/24` |
| Router WireGuard | `10.20.20.1/24` |
| Client WireGuard | `10.20.20.2/32` |
| UDP port | `13231` |

Create a WireGuard interface:

```routeros
/interface wireguard
add name=wireguard-roadwarrior listen-port=13231
```

Assign its IP address:

```routeros
/ip address
add address=10.20.20.1/24 interface=wireguard-roadwarrior
```

Display the router's public key:

```routeros
/interface wireguard print detail where name=wireguard-roadwarrior
```

Create a peer after generating a key pair on the remote client.

```routeros
/interface wireguard peers
add interface=wireguard-roadwarrior public-key="CLIENT_PUBLIC_KEY" allowed-address=10.20.20.2/32
```

The client public key must be replaced with the actual public key generated by the remote device.

Allow the WireGuard UDP port through the input firewall:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=13231 in-interface=ether1 comment="Allow Road Warrior WireGuard"
```

Allow the remote client to access the LAN:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.20.20.0/24 dst-address=192.168.10.0/24 comment="Allow VPN clients to LAN"
```

If LAN devices must initiate connections to VPN clients, configure the corresponding reverse-direction firewall permission.

# 🗁 Remote Client Configuration

A WireGuard client configuration contains the client's private key and the router's public key.

Example client configuration:

```ini
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.20.20.2/32
DNS = 192.168.10.1

[Peer]
PublicKey = ROUTER_PUBLIC_KEY
Endpoint = A.B.C.D:13231
AllowedIPs = 10.20.20.0/24, 192.168.10.0/24
PersistentKeepalive = 25
```

Replace the example values with the actual keys and reachable endpoint.

Important properties:

* `PrivateKey` is the remote client's private key.
* `Address` is the client's VPN interface address.
* `DNS` specifies a DNS server for the client.
* `PublicKey` is the MikroTik router's public key.
* `Endpoint` identifies the router's reachable address and UDP port.
* `AllowedIPs` determines which destinations the client sends through the VPN.
* `PersistentKeepalive` helps maintain NAT mappings when necessary.

The example uses split tunneling: only the VPN subnet and organization LAN subnet are sent through the tunnel.

For full-tunnel routing, use:

```ini
AllowedIPs = 0.0.0.0/0
```

For IPv6 full-tunnel routing, include:

```ini
AllowedIPs = 0.0.0.0/0, ::/0
```

Full-tunnel operation also requires appropriate routing, firewall, DNS, and internet egress configuration on the MikroTik router.

# 🗁 WireGuard Key Generation on Linux

The WireGuard tools package provides utilities for generating key pairs.

Install the tools on Ubuntu or Debian:

```bash
sudo apt update
sudo apt install wireguard-tools
```

Generate a private key:

```bash
wg genkey | tee privatekey
```

Generate the corresponding public key:

```bash
cat privatekey | wg pubkey
```

The private key must remain secret.

The public key can be shared with the remote WireGuard peer.

Generate a key pair with appropriate file permissions:

```bash
umask 077
wg genkey | tee privatekey | wg pubkey > publickey
```

Display the public key:

```bash
cat publickey
```

# 🗁 WireGuard and NAT

WireGuard can operate through NAT devices.

However, NAT may affect peer reachability, especially when both peers are behind restrictive NAT configurations.

Important considerations:

* The listening UDP port must be reachable at the endpoint.
* Upstream NAT devices may require UDP port forwarding.
* Dynamic public IP addresses may require a DNS hostname.
* `persistent-keepalive` can help maintain NAT mappings.
* Firewall rules must permit the WireGuard UDP traffic.

A common keepalive interval is 25 seconds for a peer that needs to maintain a NAT mapping.

Keepalive is not required in every network topology.

# 🗁 WireGuard with Dynamic WAN Addresses

If a peer has a dynamic public IP address, its endpoint may change.

A DNS hostname can be used instead of a fixed IP address.

For example:

```routeros
/interface wireguard peers
set [find where interface=wireguard1] endpoint-address=vpn.example.com
```

The hostname should resolve to the current reachable public IP address.

Configure a Dynamic DNS update mechanism if the WAN address changes frequently.

A peer that initiates traffic can also learn the endpoint of an authenticated peer from received packets, which can be useful when one side has a changing address.

# 🗁 WireGuard Security Best Practices

* Protect private keys.
* Exchange public keys through a trusted channel.
* Use unique key pairs for individual devices.
* Avoid reusing one client key across multiple users.
* Restrict `allowed-address` to the necessary networks.
* Allow only the required UDP listening port.
* Apply appropriate input and forward firewall policies.
* Keep RouterOS updated.
* Remove obsolete peer configurations.
* Use persistent keepalive only when necessary.
* Monitor peer handshakes and traffic counters.
* Use a preshared key when additional shared-secret protection is required.
* Avoid exposing router management services to the public internet unnecessarily.

# 🗁 WireGuard Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| No handshake | Incorrect public key, unreachable endpoint, or blocked UDP port |
| Handshake works but ping fails | Incorrect allowed addresses, routes, or firewall rules |
| Tunnel IP works but LAN access fails | Missing route or forward-chain permission |
| One direction works only | Missing return route or asymmetric firewall policy |
| Connection stops after WAN address changes | Stale endpoint or DNS update problem |
| Intermittent connection behind NAT | NAT mapping expiration or missing keepalive |
| No traffic counters increase | Traffic is not routed through the peer |
| Client connects but cannot browse the internet | Missing full-tunnel routing, NAT, DNS, or firewall configuration |

Useful diagnostic commands:

Display interfaces:

```routeros
/interface wireguard print detail
```

Display peers:

```routeros
/interface wireguard peers print detail
```

Display peer statistics:

```routeros
/interface wireguard peers print stats
```

Inspect IP routes:

```routeros
/ip route print detail
```

Inspect firewall rules:

```routeros
/ip firewall filter print detail
```

Inspect firewall NAT rules:

```routeros
/ip firewall nat print detail
```

Inspect active connections:

```routeros
/ip firewall connection print
```

Monitor UDP traffic:

```routeros
/tool torch interface=ether1 port=13231
```

# 🗁 Removing WireGuard Configuration

To remove a specific peer:

```routeros
/interface wireguard peers
remove [find where interface=wireguard1 and public-key="ROUTER_B_PUBLIC_KEY"]
```

Remove the WireGuard interface:

```routeros
/interface wireguard
remove [find where name=wireguard1]
```

Removing an interface also removes its associated peer configurations.

Remove associated IP addresses, routes, and firewall rules separately if they are no longer required.

Always verify that the configuration is not being used by another service before removing it.

# 🗁 Summary

WireGuard provides encrypted, peer-based VPN connectivity in MikroTik RouterOS v7.

Important concepts include:

* WireGuard interfaces and peers.
* Public and private keys.
* UDP listening ports.
* Allowed addresses.
* Endpoint configuration.
* Persistent keepalive.
* Site-to-site VPN.
* Road warrior VPN.
* Routing and firewall configuration.
* NAT traversal.
* Monitoring and troubleshooting.

WireGuard is a practical solution for connecting remote users, branch offices, and private networks through secure encrypted tunnels.
