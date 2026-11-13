# VPN Exercises

VPN exercises help you understand how to establish secure communication between remote networks over an untrusted network such as the Internet.

In these labs, you will configure VPN tunnels using MikroTik RouterOS v7 and Ubuntu/Linux. You will practice WireGuard, site-to-site connectivity, remote access, routing through VPN tunnels, and VPN troubleshooting.

### 🖆 Learning Objectives

By completing these exercises, you will be able to:

* Understand VPN fundamentals.
* Differentiate between site-to-site and remote-access VPNs.
* Understand VPN tunneling and encryption.
* Configure WireGuard tunnels on MikroTik RouterOS v7.
* Configure a site-to-site VPN.
* Route traffic through a VPN tunnel.
* Configure a Linux WireGuard client.
* Verify VPN connectivity.
* Troubleshoot common VPN problems.

### 🖆 Lab Requirements

| Component | Requirement |
| --- | --- |
| Router platform | MikroTik CHR |
| RouterOS version | RouterOS v7 |
| Client operating system | Ubuntu/Linux |
| Virtualization | VirtualBox, GNS3, or EVE-NG |
| Routers | Two |
| Client networks | Two |
| VPN protocol | WireGuard |

# 🗁 VPN Fundamentals

### 🖆 What Is a VPN?

A VPN (Virtual Private Network) creates a logical private communication path over an existing network.

VPNs commonly provide:

* Confidentiality through encryption.
* Authentication of VPN peers or users.
* Integrity protection for transmitted data.
* Secure access to remote networks.
* Connectivity between geographically separated locations.

A VPN does not necessarily replace routing or firewall policies. It provides a secure communication mechanism that works alongside them.

### 🖆 Why Use a VPN?

Common VPN use cases include:

* Connecting branch offices to headquarters.
* Allowing employees to access private company networks remotely.
* Connecting cloud infrastructure to on-premises networks.
* Providing secure administrative access to network devices.
* Connecting geographically separated private networks.

### 🖆 Types of VPN

| VPN Type | Description |
| --- | --- |
| Site-to-site VPN | Connects two or more private networks. |
| Remote-access VPN | Connects an individual device to a private network. |
| Client-to-site VPN | Allows a client device to access an organization's network. |
| Host-to-host VPN | Creates a secure connection between two individual hosts. |

### 🖆 VPN Protocols

| Protocol   | Description                                                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------------- |
| IPsec      | A suite of protocols for securing IP communication.                                                                 |
| WireGuard  | A modern, lightweight VPN protocol using public-key cryptography.                                                   |
| OpenVPN    | A VPN solution based on TLS and SSL technologies.                                                                   |
| L2TP/IPsec | Combines Layer 2 Tunneling Protocol with IPsec protection.                                                          |
| GRE        | A tunneling protocol that encapsulates network-layer packets; encryption requires an additional security mechanism. |
| SSTP       | A VPN protocol that tunnels PPP over TLS.                                                                           |

### 🖆 VPN Tunneling

Tunneling encapsulates packets from one network protocol inside another packet format.

For example, a VPN can encapsulate private network traffic inside encrypted UDP or IP packets.

```text
Private Network A
       |
    Router A
       |
   VPN Tunnel
  (Encrypted)
       |
    Router B
       |
Private Network B
```

The tunnel allows private network traffic to travel across an untrusted network.

### 🖆 Site-to-Site vs Remote-Access VPN

| Feature | Site-to-Site VPN | Remote-Access VPN |
| --- | --- | --- |
| Connection | Network to network | Device to network |
| Typical users | Branch offices | Remote employees |
| Configuration | Usually router or firewall | Usually client and VPN gateway |
| Example | Headquarters to branch | Laptop to office network |

# 🗁 WireGuard Fundamentals

### 🖆 What Is WireGuard?

WireGuard is a modern VPN protocol designed to provide secure, simple, and efficient encrypted communication.

It uses public-key cryptography and operates through a virtual network interface.

WireGuard commonly uses UDP for transport.

### 🖆 WireGuard Concepts

| Component | Description |
| --- | --- |
| Interface | Virtual network interface used for VPN traffic. |
| Private key | Secret key kept by the local peer. |
| Public key | Key shared with remote peers. |
| Peer | Another WireGuard endpoint. |
| Endpoint | Remote peer's reachable IP address and UDP port. |
| Allowed IPs | Addresses routed to a peer and accepted from that peer. |
| Listen port | UDP port used to receive WireGuard traffic. |
| Persistent keepalive | Periodic packets that help maintain NAT mappings. |

### 🖆 Public and Private Keys

Each WireGuard peer has a key pair:

* Private key: Must remain secret on the device.
* Public key: Shared with the remote peer.

The public key identifies the peer during WireGuard authentication.

Never share or publish a private key.

### 🖆 Allowed IPs

The `allowed-address` property is important in WireGuard configuration.

It determines which source addresses are accepted from a peer and which destination addresses are routed through that peer.

For example:

```text
10.10.10.2/32
192.168.20.0/24
```

This configuration can identify the remote tunnel address and the private network reachable through that peer.

# 🗁 Lab Topology

### 🖆 Site-to-Site WireGuard Lab

Build a topology with two MikroTik routers and two client networks.

```text
LAN A                         LAN B
192.168.10.0/24              192.168.20.0/24
     |                            |
   PC-A                         PC-B
     |                            |
 Router A                      Router B
     |                            |
   WireGuard Tunnel
   10.10.10.0/30
     |
   Internet / WAN
```

For a simple virtual lab, connect the routers through a simulated WAN network.

### 🖆 IP Addressing Plan

| Device | Interface | IP Address |
| --- | --- | --- |
| Router A | WAN | `203.0.113.1/24` |
| Router A | LAN | `192.168.10.1/24` |
| Router A | WireGuard | `10.10.10.1/30` |
| Router B | WAN | `203.0.113.2/24` |
| Router B | LAN | `192.168.20.1/24` |
| Router B | WireGuard | `10.10.10.2/30` |
| PC-A | LAN | `192.168.10.10/24` |
| PC-B | LAN | `192.168.20.10/24` |

The WAN addresses above are documentation addresses for an isolated lab. Use addresses appropriate to your actual virtual WAN topology.

# 🗁 Exercise: Configure WireGuard on MikroTik

### 🖆 Create the WireGuard Interface on Router A

```routeros
/interface wireguard
add name=wg-site listen-port=13231
```

Inspect the interface and its public key:

```routeros
/interface wireguard print detail
```

Record Router A's public key. You will need it when configuring Router B.

### 🖆 Assign the Tunnel IP Address

```routeros
/ip address
add address=10.10.10.1/30 interface=wg-site
```

### 🖆 Create the WireGuard Interface on Router B

```routeros
/interface wireguard
add name=wg-site listen-port=13232
```

Inspect the public key:

```routeros
/interface wireguard print detail
```

Record Router B's public key.

### 🖆 Assign Router B's Tunnel IP Address

```routeros
/ip address
add address=10.10.10.2/30 interface=wg-site
```

### 🖆 Understand the Key Exchange

You now have two WireGuard peers.

Exchange only the public keys:

| Router | Public Key Needed |
| --- | --- |
| Router A | Router B's public key |
| Router B | Router A's public key |

Do not exchange private keys.

# 🗁 Exercise: Configure a Site-to-Site VPN

### 🖆 Configure Router A's Peer

Replace `ROUTER_B_PUBLIC_KEY` with the actual public key generated on Router B.

```routeros
/interface wireguard peers
add interface=wg-site \
    public-key="ROUTER_B_PUBLIC_KEY" \
    endpoint-address=203.0.113.2 \
    endpoint-port=13232 \
    allowed-address=10.10.10.2/32,192.168.20.0/24 \
    persistent-keepalive=25s
```

This tells Router A that the remote tunnel address and Router B's LAN are reachable through this peer.

### 🖆 Configure Router B's Peer

Replace `ROUTER_A_PUBLIC_KEY` with the actual public key generated on Router A.

```routeros
/interface wireguard peers
add interface=wg-site \
    public-key="ROUTER_A_PUBLIC_KEY" \
    endpoint-address=203.0.113.1 \
    endpoint-port=13231 \
    allowed-address=10.10.10.1/32,192.168.10.0/24 \
    persistent-keepalive=25s
```

### 🖆 Understand Persistent Keepalive

Persistent keepalive sends periodic packets to maintain connectivity through certain NAT devices.

The example uses 25 seconds.

It is especially useful when a peer is behind NAT and needs to keep its UDP mapping active.

It is not necessary in every network.

### 🖆 Verify Peer Configuration

On Router A:

```routeros
/interface wireguard peers print detail
```

On Router B:

```routeros
/interface wireguard peers print detail
```

Check:

* Public keys.
* Endpoint addresses.
* Endpoint ports.
* Allowed addresses.
* Latest handshake.
* Transmitted and received bytes.

# 🗁 Exercise: Configure Routing Through the VPN

### 🖆 Understand VPN Routing

Creating a VPN interface does not automatically guarantee that private networks can communicate.

Both routers need routes to their remote LANs.

WireGuard `allowed-address` entries also define which destination prefixes are associated with each peer.

### 🖆 Add a Route on Router A

```routeros
/ip route
add dst-address=192.168.20.0/24 gateway=wg-site
```

This routes traffic destined for Router B's LAN through the WireGuard interface.

### 🖆 Add a Route on Router B

```routeros
/ip route
add dst-address=192.168.10.0/24 gateway=wg-site
```

This routes traffic destined for Router A's LAN through the WireGuard interface.

### 🖆 Verify Routing Tables

On both routers:

```routeros
/ip route print detail
```

Check that the remote LAN route is present and active.

### 🖆 Verify Tunnel Connectivity

From Router A:

```routeros
/ping 10.10.10.2
```

From Router B:

```routeros
/ping 10.10.10.1
```

If the tunnel is configured correctly and the peers can reach each other over the WAN, the tunnel IP addresses should respond.

# 🗁 Exercise: Allow VPN Traffic Through the Firewall

### 🖆 Understand VPN Firewall Requirements

A WireGuard tunnel requires the UDP listening port to be reachable on the receiving router.

The firewall must also permit the required traffic between the LAN and VPN interface.

### 🖆 Allow WireGuard on Router A

```routeros
/ip firewall filter
add chain=input in-interface=ether1 protocol=udp dst-port=13231 action=accept comment="Allow WireGuard from WAN"
```

### 🖆 Allow WireGuard on Router B

```routeros
/ip firewall filter
add chain=input in-interface=ether1 protocol=udp dst-port=13232 action=accept comment="Allow WireGuard from WAN"
```

Place these rules before any input-chain rule that drops the corresponding WAN traffic.

### 🖆 Allow LAN-to-VPN Forwarding on Router A

```routeros
/ip firewall filter
add chain=forward src-address=192.168.10.0/24 dst-address=192.168.20.0/24 action=accept comment="Allow LAN A to LAN B"
```

### 🖆 Allow VPN-to-LAN Forwarding on Router A

```routeros
/ip firewall filter
add chain=forward src-address=192.168.20.0/24 dst-address=192.168.10.0/24 action=accept comment="Allow LAN B to LAN A"
```

### 🖆 Configure Forwarding on Router B

Apply corresponding rules on Router B if its firewall policy requires explicit permission for these networks.

```routeros
/ip firewall filter
add chain=forward src-address=192.168.20.0/24 dst-address=192.168.10.0/24 action=accept comment="Allow LAN B to LAN A"

add chain=forward src-address=192.168.10.0/24 dst-address=192.168.20.0/24 action=accept comment="Allow LAN A to LAN B"
```

Place the rules before a general forwarding drop rule.

### 🖆 Review NAT Bypass Requirements

If either router uses a broad masquerade rule, make sure traffic between the two private LANs is not unintentionally masqueraded.

For example, on Router A:

```routeros
/ip firewall nat
add chain=srcnat src-address=192.168.10.0/24 dst-address=192.168.20.0/24 action=accept comment="No NAT between VPN LANs"
```

Place this rule before the general Internet masquerade rule.

Apply the corresponding rule on Router B:

```routeros
/ip firewall nat
add chain=srcnat src-address=192.168.20.0/24 dst-address=192.168.10.0/24 action=accept comment="No NAT between VPN LANs"
```

# 🗁 Exercise: Test Site-to-Site Connectivity

### 🖆 Configure PC-A

On Ubuntu:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
sudo ip link set enp0s3 up
sudo ip route replace default via 192.168.10.1
```

### 🖆 Configure PC-B

```bash
sudo ip address add 192.168.20.10/24 dev enp0s3
sudo ip link set enp0s3 up
sudo ip route replace default via 192.168.20.1
```

Replace `enp0s3` with the actual interface name on each client.

### 🖆 Test the Remote Gateway

From PC-A:

```bash
ping 192.168.10.1
ping 10.10.10.2
```

From PC-B:

```bash
ping 192.168.20.1
ping 10.10.10.1
```

### 🖆 Test Remote LAN Connectivity

From PC-A:

```bash
ping 192.168.20.10
```

From PC-B:

```bash
ping 192.168.10.10
```

Successful communication confirms that routing and the VPN forwarding policy are working for the tested traffic.

### 🖆 Verify the WireGuard Handshake

On both routers:

```routeros
/interface wireguard peers print detail
```

Check the latest handshake and traffic counters.

A recent handshake indicates that the peers have successfully exchanged authenticated WireGuard packets. It does not, by itself, prove that remote LAN routing and firewall policies are correct.

# 🗁 Exercise: Configure a WireGuard Client on Ubuntu

### 🖆 Install WireGuard

On Ubuntu:

```bash
sudo apt update
sudo apt install wireguard
```

### 🖆 Generate Client Keys

```bash
wg genkey | tee client-private.key | wg pubkey > client-public.key
```

Protect the private key:

```bash
chmod 600 client-private.key
```

Display the public key:

```bash
cat client-public.key
```

Add this public key as a peer on the MikroTik router.

### 🖆 Create a WireGuard Configuration File

Create:

```bash
sudo nano /etc/wireguard/wg0.conf
```

Example configuration:

```ini
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.20.20.2/24

[Peer]
PublicKey = MIKROTIK_PUBLIC_KEY
Endpoint = VPN_SERVER_PUBLIC_IP:13231
AllowedIPs = 10.20.20.0/24, 192.168.10.0/24
PersistentKeepalive = 25
```

Replace the placeholders with actual values.

The MikroTik peer configuration must contain the client's public key and the appropriate allowed addresses.

### 🖆 Configure the MikroTik Client Peer

Assume the Ubuntu client uses:

* Tunnel IP: `10.20.20.2/32`
* Remote LAN: `192.168.10.0/24`

Add the peer on MikroTik:

```routeros
/interface wireguard peers
add interface=wg-remote \
    public-key="CLIENT_PUBLIC_KEY" \
    allowed-address=10.20.20.2/32
```

Create the server-side WireGuard interface and assign it an address in the same tunnel subnet before adding the peer.

For example:

```routeros
/interface wireguard
add name=wg-remote listen-port=13231

/ip address
add address=10.20.20.1/24 interface=wg-remote
```

If UDP port `13231` is already used by another WireGuard interface, choose a different port and update the client configuration.

Add appropriate input and forward firewall rules to permit the client to reach the intended private network.

### 🖆 Start the WireGuard Tunnel

```bash
sudo wg-quick up wg0
```

Check the interface:

```bash
ip address show wg0
```

Check WireGuard status:

```bash
sudo wg show
```

### 🖆 Test Remote Access

```bash
ping 10.20.20.1
ping 192.168.10.1
```

The results depend on the server-side peer configuration, routing, and firewall policy.

### 🖆 Stop the VPN Tunnel

```bash
sudo wg-quick down wg0
```

# 🗁 Exercise: Configure Full-Tunnel VPN Access

### 🖆 Understand Full-Tunnel Routing

A full-tunnel VPN sends most or all client IP traffic through the VPN connection.

This differs from split tunneling, where only selected destination networks use the VPN.

| VPN Mode | Routing Behavior |
| --- | --- |
| Split tunnel | Selected networks use the VPN. |
| Full tunnel | General Internet and private network traffic use the VPN. |

### 🖆 Configure Client Allowed IPs

For a full-tunnel IPv4 configuration:

```ini
[Peer]
PublicKey = VPN_SERVER_PUBLIC_KEY
Endpoint = VPN_SERVER_PUBLIC_IP:13231
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

This directs IPv4 traffic through the WireGuard peer.

For full IPv6 tunneling, the IPv6 default prefix must also be configured and supported by the VPN server.

### 🖆 Understand Server Requirements

For full-tunnel access, the VPN gateway must:

* Have a working Internet connection.
* Permit forwarding from the VPN interface.
* Provide appropriate source NAT or routing for client traffic.
* Have suitable DNS configuration if clients need DNS resolution.
* Permit the required traffic through its firewall.

A client-side default route alone does not provide Internet connectivity through the VPN.

# 🗁 Exercise: Monitor VPN Connectivity

### 🖆 Inspect WireGuard Interfaces

```routeros
/interface wireguard print detail
```

### 🖆 Inspect WireGuard Peers

```routeros
/interface wireguard peers print detail
```

### 🖆 Inspect Tunnel Addresses

```routeros
/ip address print
```

### 🖆 Inspect Routes

```routeros
/ip route print detail
```

### 🖆 Inspect Firewall Counters

```routeros
/ip firewall filter print stats
```

### 🖆 Inspect Linux WireGuard Status

```bash
sudo wg show
```

### 🖆 Inspect Tunnel Traffic

On Linux:

```bash
ip -s link show wg0
```

On MikroTik:

```routeros
/interface print stats
```

Compare received and transmitted traffic while generating test traffic across the VPN.

# 🗁 VPN Troubleshooting Exercises

### 🖆 Problem: WireGuard Handshake Does Not Occur

**Symptoms:**

* Peer configuration exists.
* Latest handshake is empty or outdated.
* No tunnel traffic is received.

**Investigation:**

```routeros
/interface wireguard peers print detail
/ip firewall filter print stats
```

Check:

* Correct public keys.
* Correct endpoint IP address.
* Correct UDP port.
* WAN reachability.
* Input firewall policy.
* NAT or upstream port forwarding.

**Solution:**

Correct the peer information and permit the WireGuard UDP listening port through the receiving router's firewall.

### 🖆 Problem: VPN Handshake Works but LAN Communication Fails

**Symptoms:**

* WireGuard peers establish a handshake.
* Tunnel IP addresses may communicate.
* Remote LAN devices cannot communicate.

**Investigation:**

```routeros
/ip route print detail
/interface wireguard peers print detail
/ip firewall filter print stats
/ip firewall nat print
```

Check:

* Remote LAN routes.
* Allowed addresses.
* Forward-chain firewall rules.
* Return routes.
* NAT bypass rules.

**Solution:**

Correct routing and forwarding policies on both routers.

### 🖆 Problem: One-Way Communication

**Symptoms:**

* PC-A can reach PC-B.
* PC-B cannot reach PC-A.
* Traffic works in only one direction.

**Investigation:**

Check the route and firewall policy on both routers.

```routeros
/ip route print detail
/ip firewall filter print stats
```

**Solution:**

Verify that both routers have correct remote-network routes and that both directions are permitted by the firewall policy.

### 🖆 Problem: Incorrect Allowed Addresses

**Symptoms:**

* Handshake succeeds.
* Some tunnel traffic is missing.
* Traffic to remote networks does not use the expected peer.

**Investigation:**

```routeros
/interface wireguard peers print detail
/ip route print detail
```

**Solution:**

Correct the peer's `allowed-address` configuration and ensure that the required routes exist.

### 🖆 Problem: VPN Works but Internet Access Fails

**Symptoms:**

* Private VPN networks are reachable.
* Internet access through a full-tunnel VPN fails.

**Investigation:**

Check:

* Default route through the VPN.
* Gateway forwarding.
* Source NAT.
* DNS configuration.
* Upstream connectivity.

**Solution:**

Correct the VPN gateway's forwarding and NAT policy, then verify DNS resolution and external connectivity separately.

### 🖆 Problem: VPN Disconnects Behind NAT

**Symptoms:**

* VPN works initially.
* Connectivity stops after inactivity.
* The client is behind a NAT device.

**Investigation:**

Check the peer's endpoint and latest handshake.

**Solution:**

Configure an appropriate persistent keepalive interval on the peer behind NAT, such as:

```routeros
persistent-keepalive=25s
```

# 🗁 Practice Challenges

### 🖆 Challenge: Basic Site-to-Site VPN

Build a VPN between two MikroTik routers.

Requirements:

* Configure two WireGuard interfaces.
* Generate public and private keys.
* Exchange public keys.
* Configure peer endpoints.
* Assign tunnel IP addresses.
* Configure remote LAN routes.
* Permit the required VPN traffic.
* Verify connectivity between both LANs.

### 🖆 Challenge: Remote-Access VPN

Configure an Ubuntu client to connect to a MikroTik WireGuard server.

Requirements:

* Generate client keys.
* Configure the server peer.
* Configure the client interface.
* Assign a tunnel IP address.
* Allow access to a selected private network.
* Verify the handshake and connectivity.

### 🖆 Challenge: Full-Tunnel VPN

Configure an Ubuntu client to send IPv4 Internet traffic through a VPN gateway.

Requirements:

* Configure the default route through WireGuard.
* Permit forwarding.
* Configure source NAT or an equivalent return-routing design.
* Configure DNS.
* Verify private and public destination connectivity.

### 🖆 Challenge: VPN Network Segmentation

Create a VPN connection between two offices.

Requirements:

* Allow access to selected remote subnets.
* Block access to unauthorized subnets.
* Permit management access only from a trusted network.
* Verify the firewall policy.
* Inspect VPN and firewall counters.

### 🖆 Challenge: VPN Troubleshooting

Create several intentional configuration errors:

* Incorrect peer public key.
* Incorrect endpoint port.
* Missing remote route.
* Incorrect allowed-address.
* Missing firewall permission.
* Missing return route.

Diagnose each issue and document the corrective action.

# 🗁 Knowledge Check

### 🖆 Questions

* What is a VPN?
* What is VPN tunneling?
* What is the difference between site-to-site and remote-access VPN?
* What is the difference between IPsec and WireGuard?
* What is a WireGuard peer?
* What is the purpose of a public key?
* Why must a private key remain secret?
* What is the purpose of `allowed-address`?
* What is a WireGuard endpoint?
* Why is persistent keepalive useful?
* Does a successful VPN handshake guarantee remote LAN connectivity?
* What is the difference between split tunneling and full tunneling?
* Why are routes required for site-to-site VPN communication?
* Why might NAT bypass rules be necessary for VPN traffic?
* Which commands can you use to inspect WireGuard status in RouterOS and Linux?

### 🖆 Practical Verification Checklist

* [ ] VPN fundamentals are understood.
* [ ] Site-to-site and remote-access VPNs are understood.
* [ ] WireGuard keys are generated.
* [ ] WireGuard interfaces are configured.
* [ ] Peer public keys are exchanged.
* [ ] Peer endpoints are configured.
* [ ] Tunnel IP addresses are assigned.
* [ ] Allowed addresses are configured.
* [ ] Remote LAN routes are configured.
* [ ] Firewall policies permit required traffic.
* [ ] NAT behavior is verified.
* [ ] Site-to-site connectivity works.
* [ ] Linux WireGuard client connects successfully.
* [ ] VPN handshake and traffic counters are inspected.
* [ ] Common VPN problems can be diagnosed.

# 🗁 Expected Learning Outcome

After completing these exercises, you should understand VPN architecture, tunneling, peer authentication, and encrypted communication.

You should be able to configure a WireGuard site-to-site VPN between MikroTik routers, connect an Ubuntu client to a VPN gateway, route traffic through a tunnel, and troubleshoot common VPN connectivity problems.
