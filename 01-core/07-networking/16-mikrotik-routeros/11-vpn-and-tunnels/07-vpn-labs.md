# VPN Labs and Practical Exercises

VPNs (Virtual Private Networks) provide secure or logical connectivity between remote networks and devices.

MikroTik RouterOS supports several VPN technologies, including:

* WireGuard.
* IPsec.
* OpenVPN.
* GRE.
* IPIP.
* EoIP.
* L2TP/IPsec.

Each technology has different characteristics, configuration requirements, and use cases.

These labs provide practical experience configuring, testing, monitoring, and troubleshooting VPN connections using MikroTik RouterOS v7.

# 🗁 Lab Environment

You can perform these labs using:

* MikroTik physical routers.
* MikroTik CHR.
* VirtualBox.
* VMware.
* GNS3.
* EVE-NG.
* Windows or Linux VPN clients.

For multi-router labs, use two MikroTik routers and connect them through a simulated WAN network.

# 🗁 Lab Network Topology

The following topology will be used for most site-to-site VPN exercises.

```text
                  Simulated Internet
                         |
                +--------+--------+
                |                 |
             Router A          Router B
              Site A            Site B
                |                 |
          WAN: 203.0.113.1   WAN: 198.51.100.1
                |                 |
                +-----------------+
                         |
                   VPN Tunnel
                         |
                +--------+--------+
                |                 |
             LAN A              LAN B
         10.10.10.0/24      10.20.20.0/24
                |                 |
             Client A          Client B
         10.10.10.10         10.20.20.10
```

The WAN addresses are documentation addresses. In a virtual lab, assign addresses from your simulated WAN network instead.

# 🗁 Lab Addressing Plan

| Device | Interface | IP Address |
| --- | --- | --- |
| Router A | WAN | `203.0.113.1` |
| Router A | LAN | `10.10.10.1/24` |
| Router B | WAN | `198.51.100.1` |
| Router B | LAN | `10.20.20.1/24` |
| Client A | LAN | `10.10.10.10/24` |
| Client B | LAN | `10.20.20.10/24` |

Use these networks consistently throughout the site-to-site exercises.

# 🗁 Preparing the MikroTik Routers

Configure WAN addresses on Router A:

```routeros
/ip address
add address=203.0.113.1/24 interface=ether1
```

Configure WAN addresses on Router B:

```routeros
/ip address
add address=198.51.100.1/24 interface=ether1
```

Configure LAN addresses on Router A:

```routeros
/ip address
add address=10.10.10.1/24 interface=ether2
```

Configure LAN addresses on Router B:

```routeros
/ip address
add address=10.20.20.1/24 interface=ether2
```

These commands assume that the selected interfaces are not already configured with conflicting addresses or bridge memberships.

# 🗁 Verifying WAN Connectivity

Before creating a VPN, verify that the routers can reach each other's WAN addresses.

From Router A:

```routeros
/ping 198.51.100.1
```

From Router B:

```routeros
/ping 203.0.113.1
```

In a real deployment, the WAN endpoints must be reachable through the internet or the relevant upstream network.

In a virtual lab, configure the simulated WAN network so that both routers can communicate.

# 🗁 Understanding VPN Lab Objectives

The exercises cover different VPN requirements.

| Lab | Technology | Objective |
| --- | --- | --- |
| WireGuard Remote Access | WireGuard | Connect a remote client |
| WireGuard Site-to-Site | WireGuard | Connect two LANs |
| IPsec Site-to-Site | IPsec | Protect communication between sites |
| OpenVPN Remote Access | OpenVPN | Connect desktop clients |
| GRE Tunnel | GRE | Create a routed tunnel |
| IPIP Tunnel | IPIP | Build a simple IP tunnel |
| EoIP Tunnel | EoIP | Extend Layer 2 connectivity |
| L2TP/IPsec Remote Access | L2TP/IPsec | Connect remote users |
| VPN Troubleshooting | Multiple | Diagnose common problems |

# 🗁 WireGuard Remote Access Lab

**Objective**

Connect a remote client to a MikroTik WireGuard server.

Topology:

```text
          Remote Client
             |
          WireGuard
             |
          Internet
             |
       MikroTik Router
        WireGuard Server
             |
          Private LAN
       192.168.10.0/24
```

Example addressing:

| Component | Address |
| --- | --- |
| Router LAN | `192.168.10.1/24` |
| WireGuard server | `10.50.50.1/24` |
| Remote client | `10.50.50.2/32` |

Create the WireGuard interface:

```routeros
/interface wireguard
add name=wg-remote listen-port=51820
```

Assign its IP address:

```routeros
/ip address
add address=10.50.50.1/24 interface=wg-remote
```

Display the public key:

```routeros
/interface wireguard print detail
```

Configure the remote client's peer using the router's public key.

Add the client peer:

```routeros
/interface wireguard peers
add interface=wg-remote public-key="CLIENT_PUBLIC_KEY" allowed-address=10.50.50.2/32
```

Allow the WireGuard UDP port through the WAN firewall:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=51820 in-interface=ether1 comment="Allow WireGuard"
```

Allow the VPN client to access the LAN:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.50.50.2 dst-address=192.168.10.0/24 comment="Allow WireGuard client to LAN"
```

Configure the client with:

* Its own private key.
* The router's public key.
* The router's public endpoint.
* The assigned VPN address.
* The required LAN route.

Test connectivity to the router's VPN address and private LAN.

**Expected Result**

The remote client establishes a WireGuard connection and accesses permitted LAN resources.

# 🗁 WireGuard Site-to-Site Lab

**Objective**

Connect two private LANs through WireGuard.

Topology:

```text
       Site A                         Site B
   10.10.10.0/24                  10.20.20.0/24
        |                              |
     Router A                       Router B
        |                              |
        +========= WireGuard ==========+
```

Configure Router A:

```routeros
/interface wireguard
add name=wg-site-b listen-port=51820
```

Assign its tunnel address:

```routeros
/ip address
add address=10.60.60.1/30 interface=wg-site-b
```

Configure Router B:

```routeros
/interface wireguard
add name=wg-site-a listen-port=51820
```

Assign its tunnel address:

```routeros
/ip address
add address=10.60.60.2/30 interface=wg-site-a
```

Exchange public keys between routers.

On Router A:

```routeros
/interface wireguard peers
add interface=wg-site-b public-key="ROUTER_B_PUBLIC_KEY" endpoint-address=198.51.100.1 endpoint-port=51820 allowed-address=10.60.60.2/32,10.20.20.0/24 persistent-keepalive=25s
```

On Router B:

```routeros
/interface wireguard peers
add interface=wg-site-a public-key="ROUTER_A_PUBLIC_KEY" endpoint-address=203.0.113.1 endpoint-port=51820 allowed-address=10.60.60.1/32,10.10.10.0/24 persistent-keepalive=25s
```

Configure routes on Router A:

```routeros
/ip route
add dst-address=10.20.20.0/24 gateway=wg-site-b
```

Configure routes on Router B:

```routeros
/ip route
add dst-address=10.10.10.0/24 gateway=wg-site-a
```

Allow the necessary forwarding traffic through both routers' firewalls.

**Expected Result**

Devices in `10.10.10.0/24` can communicate with permitted devices in `10.20.20.0/24`.

# 🗁 IPsec Site-to-Site Lab

**Objective**

Create an encrypted site-to-site connection using IPsec.

Topology:

```text
       Router A                      Router B
     Site A LAN                    Site B LAN
   10.10.10.0/24                10.20.20.0/24
         |                            |
         +======= IPsec Tunnel =======+
```

Configure an IPsec proposal on both routers:

```routeros
/ip ipsec proposal
add name=site-proposal auth-algorithms=sha256 enc-algorithms=aes-256-cbc pfs-group=modp2048
```

Configure a peer on Router A:

```routeros
/ip ipsec peer
add name=peer-site-b address=198.51.100.1/32 exchange-mode=ike2 profile=default
```

Configure a peer on Router B:

```routeros
/ip ipsec peer
add name=peer-site-a address=203.0.113.1/32 exchange-mode=ike2 profile=default
```

Create identities using the same strong pre-shared key.

On Router A:

```routeros
/ip ipsec identity
add peer=peer-site-b auth-method=pre-shared-key secret=REPLACE_WITH_STRONG_SECRET
```

On Router B:

```routeros
/ip ipsec identity
add peer=peer-site-a auth-method=pre-shared-key secret=REPLACE_WITH_STRONG_SECRET
```

Create the policy on Router A:

```routeros
/ip ipsec policy
add src-address=10.10.10.0/24 dst-address=10.20.20.0/24 tunnel=yes peer=peer-site-b proposal=site-proposal
```

Create the corresponding policy on Router B:

```routeros
/ip ipsec policy
add src-address=10.20.20.0/24 dst-address=10.10.10.0/24 tunnel=yes peer=peer-site-a proposal=site-proposal
```

Verify the configuration:

```routeros
/ip ipsec active-peers print detail
/ip ipsec installed-sa print detail
```

Check firewall and NAT policies to ensure that the protected traffic is not accidentally masqueraded before IPsec policy matching.

**Expected Result**

Traffic between the two LAN subnets is protected by IPsec.

The exact proposal and IKE profile settings should be verified on both RouterOS installations and adjusted to the supported cryptographic requirements.

# 🗁 OpenVPN Remote Access Lab

**Objective**

Configure MikroTik as an OpenVPN server and connect a desktop client.

Topology:

```text
       Windows / Linux Client
                 |
              OpenVPN
                 |
              Internet
                 |
           MikroTik Router
            OVPN Server
                 |
             Private LAN
```

Configure the following on the router:

* Certificate Authority.
* Server certificate.
* VPN address pool.
* PPP profile.
* PPP user account.
* OpenVPN server.
* Firewall permission.

Example VPN address pool:

```routeros
/ip pool
add name=ovpn-lab-pool ranges=10.70.70.10-10.70.70.50
```

Create a PPP profile:

```routeros
/ppp profile
add name=ovpn-lab-profile local-address=10.70.70.1 remote-address=ovpn-lab-pool use-encryption=required
```

Create a user:

```routeros
/ppp secret
add name=ovpn-lab-user password=REPLACE_WITH_STRONG_PASSWORD service=ovpn profile=ovpn-lab-profile
```

Configure the server using the certificate and algorithms supported by the installed RouterOS release.

Example TCP server configuration:

```routeros
/interface ovpn-server server
set enabled=yes port=1194 protocol=tcp mode=ip certificate=ovpn-server require-client-certificate=no default-profile=ovpn-lab-profile auth=sha256 cipher=aes256-cbc
```

Allow the OpenVPN listening port through the firewall:

```routeros
/ip firewall filter
add chain=input action=accept protocol=tcp dst-port=1194 in-interface=ether1 comment="Allow OpenVPN Lab"
```

Export the CA certificate and prepare the client configuration.

Connect from Windows or Linux and verify:

* Successful authentication.
* Assigned VPN address.
* Tunnel connectivity.
* Access to permitted LAN resources.

**Expected Result**

The remote client establishes an OpenVPN connection and accesses the configured private network.

# 🗁 GRE Tunnel Lab

**Objective**

Create a routed GRE tunnel between two MikroTik routers.

Configure Router A:

```routeros
/interface gre
add name=gre-lab-b local-address=203.0.113.1 remote-address=198.51.100.1
```

Assign the tunnel address:

```routeros
/ip address
add address=10.80.80.1/30 interface=gre-lab-b
```

Configure Router B:

```routeros
/interface gre
add name=gre-lab-a local-address=198.51.100.1 remote-address=203.0.113.1
```

Assign the tunnel address:

```routeros
/ip address
add address=10.80.80.2/30 interface=gre-lab-a
```

Permit GRE protocol traffic between the WAN endpoints.

Configure a route on Router A:

```routeros
/ip route
add dst-address=10.20.20.0/24 gateway=gre-lab-b
```

Configure a route on Router B:

```routeros
/ip route
add dst-address=10.10.10.0/24 gateway=gre-lab-a
```

Test the tunnel:

```routeros
/ping 10.80.80.2
```

**Expected Result**

The routers communicate through GRE, and traffic can be routed between their LAN networks.

# 🗁 IPIP Tunnel Lab

**Objective**

Create a simple IPIP tunnel.

On Router A:

```routeros
/interface ipip
add name=ipip-lab-b local-address=203.0.113.1 remote-address=198.51.100.1
```

Assign an address:

```routeros
/ip address
add address=10.90.90.1/30 interface=ipip-lab-b
```

On Router B:

```routeros
/interface ipip
add name=ipip-lab-a local-address=198.51.100.1 remote-address=203.0.113.1
```

Assign an address:

```routeros
/ip address
add address=10.90.90.2/30 interface=ipip-lab-a
```

Permit IP protocol 4 through the endpoint firewalls.

Configure static routes for the remote LAN networks.

Test tunnel connectivity:

```routeros
/ping 10.90.90.2
```

**Expected Result**

Both routers exchange IP traffic through the IPIP tunnel.

# 🗁 EoIP Layer 2 Extension Lab

**Objective**

Extend an Ethernet network between two MikroTik routers.

Topology:

```text
          Site A                       Site B
       MikroTik A                   MikroTik B
           |                            |
        Bridge A                     Bridge B
           |                            |
        EoIP A                        EoIP B
           |                            |
           +========== EoIP ============+
           |                            |
        LAN Switch                   LAN Switch
```

Create the EoIP interface on Router A:

```routeros
/interface eoip
add name=eoip-lab-b local-address=203.0.113.1 remote-address=198.51.100.1 tunnel-id=200
```

Create the corresponding interface on Router B:

```routeros
/interface eoip
add name=eoip-lab-a local-address=198.51.100.1 remote-address=203.0.113.1 tunnel-id=200
```

Create bridges:

```routeros
/interface bridge
add name=bridge-lab
```

Add the local Ethernet and EoIP interfaces to the bridge on each router:

```routeros
/interface bridge port
add bridge=bridge-lab interface=ether2
add bridge=bridge-lab interface=eoip-lab-b
```

Use the corresponding EoIP interface name on Router B.

Permit GRE-based EoIP traffic between the WAN endpoints.

Connect test devices to both LANs and verify that their MAC addresses are learned across the bridge.

**Expected Result**

The two sites share Layer 2 connectivity through the EoIP tunnel.

# 🗁 L2TP/IPsec Remote Access Lab

**Objective**

Allow remote users to connect to a MikroTik router using L2TP/IPsec.

Create an address pool:

```routeros
/ip pool
add name=l2tp-lab-pool ranges=10.100.100.10-10.100.100.50
```

Create a PPP profile:

```routeros
/ppp profile
add name=l2tp-lab-profile local-address=10.100.100.1 remote-address=l2tp-lab-pool use-encryption=required
```

Create a PPP user:

```routeros
/ppp secret
add name=l2tp-lab-user password=REPLACE_WITH_STRONG_PASSWORD service=l2tp profile=l2tp-lab-profile
```

Enable the L2TP server:

```routeros
/interface l2tp-server server
set enabled=yes use-ipsec=required ipsec-secret=REPLACE_WITH_STRONG_IPSEC_SECRET default-profile=l2tp-lab-profile authentication=mschap2
```

Configure the firewall to permit the required IKE, NAT-T, and L2TP/IPsec traffic.

Connect using a Windows or Linux client.

Verify the assigned VPN address:

```routeros
/ppp active print detail
```

**Expected Result**

The remote client establishes an IPsec-protected L2TP session and receives a VPN address.

# 🗁 VPN Routing Lab

**Objective**

Understand how routing controls access between VPN clients and private networks.

Use the following networks:

```text
Site A LAN:    10.10.10.0/24
Site B LAN:    10.20.20.0/24
VPN Network:   10.60.60.0/30
```

Inspect routes:

```routeros
/ip route print detail
```

Verify:

* The VPN tunnel interface is operational.
* The remote LAN route exists.
* The route points to the correct tunnel interface or next hop.
* Return routing is available.
* Firewall rules permit the traffic.

Test connectivity from both sides.

# 🗁 Split Tunnel and Full Tunnel Lab

**Objective**

Compare split tunneling and full tunneling using a remote-access VPN.

For split tunneling:

* Configure the client to route only the private LAN subnet through the VPN.
* Keep general internet traffic on the client's local connection.
* Verify access to internal services.

For full tunneling:

* Configure the client to route its default traffic through the VPN.
* Allow VPN traffic toward the router's WAN.
* Configure source NAT where required.
* Verify internet access through the VPN endpoint.

Compare the routing tables before and after connecting.

# 🗁 VPN Firewall Lab

**Objective**

Apply firewall restrictions to VPN traffic.

Create an example policy that allows VPN clients to access only a specific server.

Example:

```text
VPN Network:       10.50.50.0/24
Allowed Server:    192.168.10.20
Protected LAN:     192.168.10.0/24
```

Allow access to the selected server:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.50.50.0/24 dst-address=192.168.10.20 comment="Allow VPN to approved server"
```

Drop access to the rest of the LAN:

```routeros
/ip firewall filter
add chain=forward action=drop src-address=10.50.50.0/24 dst-address=192.168.10.0/24 comment="Block VPN access to remaining LAN"
```

Place these rules in the correct order and before broader forwarding permissions.

Test access to the approved server and another LAN device.

**Expected Result**

VPN clients can access only the explicitly permitted resource.

# 🗁 VPN Monitoring Lab

**Objective**

Monitor VPN connections and identify operational problems.

Inspect WireGuard peers:

```routeros
/interface wireguard peers print detail
```

Inspect IPsec peers:

```routeros
/ip ipsec active-peers print detail
/ip ipsec installed-sa print detail
```

Inspect OpenVPN and L2TP sessions:

```routeros
/ppp active print detail
```

Inspect GRE and IPIP interfaces:

```routeros
/interface gre print detail
/interface ipip print detail
```

Inspect EoIP interfaces:

```routeros
/interface eoip print detail
```

Inspect interface statistics:

```routeros
/interface print stats
```

Inspect logs:

```routeros
/log print
```

Record:

* Connection status.
* Tunnel addresses.
* Peer endpoints.
* Transmitted packets.
* Received packets.
* Session duration.
* Authentication failures.
* Tunnel disconnections.

# 🗁 VPN Troubleshooting Lab

**Objective**

Diagnose common VPN failures.

Introduce the following test failures one at a time:

| Test Failure | Expected Diagnostic Area |
| --- | --- |
| Incorrect WireGuard public key | Peer configuration |
| Incorrect IPsec pre-shared key | IKE authentication |
| Blocked OpenVPN port | WAN firewall |
| Incorrect PPP password | PPP authentication |
| Incorrect GRE endpoint | Tunnel configuration |
| Blocked IPIP protocol | Firewall |
| Different EoIP Tunnel IDs | EoIP configuration |
| Missing remote route | Routing table |
| Missing return route | Remote routing |
| Incorrect MTU | Packet delivery |

Use the following diagnostic sequence:

```text
1. Verify WAN connectivity.
2. Verify tunnel endpoint addresses.
3. Check firewall permissions.
4. Check tunnel interface status.
5. Check authentication and encryption.
6. Verify assigned tunnel addresses.
7. Inspect routing tables.
8. Check forward-chain permissions.
9. Verify return routing.
10. Test application connectivity.
```

Record the failure, diagnostic evidence, root cause, and correction for each exercise.

# 🗁 VPN Performance Lab

**Objective**

Compare VPN performance under different configurations.

Measure:

* Latency.
* Throughput.
* Packet loss.
* CPU utilization.
* Interface traffic.
* Tunnel stability.

Use MikroTik bandwidth testing only in a controlled lab and avoid saturating production links.

Example interface monitoring:

```routeros
/interface monitor-traffic wg-site-b
```

Check router resource usage:

```routeros
/system resource print
```

Compare the results of:

* WireGuard.
* IPsec.
* OpenVPN.
* GRE.
* IPIP.
* EoIP.

Consider encryption overhead, encapsulation overhead, CPU capacity, WAN bandwidth, and MTU.

# 🗁 VPN Security Lab

**Objective**

Apply security controls to VPN connections.

Perform the following tasks:

* Use individual VPN accounts.
* Configure strong authentication.
* Use modern cryptographic settings.
* Restrict peer endpoint addresses.
* Restrict access to internal networks.
* Disable unused VPN services.
* Review active sessions.
* Inspect authentication logs.
* Verify IPsec protection for unencrypted tunnel protocols.
* Test firewall restrictions.

Confirm that unauthorized clients cannot establish a session or access protected resources.

# 🗁 VPN Lab Documentation Template

Use this template to document each completed lab.

```markdown
# Lab Title

### Objective

Describe what the lab demonstrates.

### Topology

Document the router connections and network layout.

### IP Addressing

| Device | Interface | IP Address |
|---|---|---|
| Router A | WAN | |
| Router A | LAN | |
| Router B | WAN | |
| Router B | LAN | |

### Configuration

Document the commands and settings used.

### Verification

Record the tests performed and their results.

### Troubleshooting

Document any problems and their solutions.

### Conclusion

Summarize what was learned.
```

# 🗁 Final VPN Lab Checklist

**Network Preparation**

* [ ] Build the two-router lab topology.
* [ ] Configure WAN addressing.
* [ ] Configure LAN addressing.
* [ ] Verify WAN connectivity.
* [ ] Document the IP addressing plan.

**VPN Configuration**

* [ ] Configure WireGuard remote access.
* [ ] Configure WireGuard site-to-site.
* [ ] Configure IPsec site-to-site.
* [ ] Configure OpenVPN remote access.
* [ ] Configure GRE tunneling.
* [ ] Configure IPIP tunneling.
* [ ] Configure EoIP bridging.
* [ ] Configure L2TP/IPsec remote access.

**Routing and Firewall**

* [ ] Configure remote network routes.
* [ ] Verify return routing.
* [ ] Configure firewall permissions.
* [ ] Test split tunneling.
* [ ] Test full tunneling.
* [ ] Restrict access to selected LAN resources.

**Monitoring and Security**

* [ ] Monitor active VPN sessions.
* [ ] Inspect tunnel interfaces.
* [ ] Inspect IPsec security associations.
* [ ] Review authentication logs.
* [ ] Test invalid credentials.
* [ ] Test blocked tunnel traffic.
* [ ] Compare VPN performance.
* [ ] Apply appropriate security controls.

# 🗁 Summary

These practical exercises demonstrate how to configure and manage VPN technologies in MikroTik RouterOS v7.

The labs cover remote access, site-to-site connectivity, Layer 2 extension, routing, firewall policies, monitoring, troubleshooting, and security.

Completing them provides practical experience with different VPN architectures and helps develop the skills required to deploy and maintain VPN connectivity in home, enterprise, and ISP environments.
