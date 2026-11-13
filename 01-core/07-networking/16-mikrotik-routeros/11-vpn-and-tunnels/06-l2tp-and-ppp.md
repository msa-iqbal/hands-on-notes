# L2TP and PPP

L2TP (Layer 2 Tunneling Protocol) is a tunneling protocol used to establish point-to-point connections between remote devices and VPN servers.

PPP (Point-to-Point Protocol) provides authentication, link configuration, and network-layer transport over point-to-point connections.

In MikroTik RouterOS, L2TP and PPP are commonly used together to provide remote-access VPN connectivity.

L2TP itself does not encrypt traffic. For secure deployments, L2TP is commonly combined with IPsec, creating an L2TP/IPsec VPN.

Common use cases include:

* Remote employee access.
* Secure administrative connectivity.
* Connecting branch offices.
* Remote network access.
* PPP-based user authentication.
* ISP subscriber connectivity.
* Centralized VPN account management.

# 🗁 What Is L2TP?

L2TP stands for Layer 2 Tunneling Protocol.

It creates a logical tunnel between two endpoints and transports PPP frames through that tunnel.

L2TP commonly uses UDP port `1701`.

A typical L2TP deployment includes:

* **L2TP Server:** Accepts incoming tunnel connections.
* **L2TP Client:** Initiates a connection to the server.
* **PPP:** Handles authentication and connection parameters.
* **PPP Profile:** Defines connection settings.
* **PPP Secret:** Stores individual user credentials.
* **IP Pool:** Assigns addresses to connected clients.
* **IPsec:** Protects the L2TP tunnel when enabled.

# 🗁 What Is PPP?

PPP stands for Point-to-Point Protocol.

It is a data-link protocol used to establish direct connections between two network endpoints.

PPP provides several important functions:

* Link establishment.
* Authentication.
* Network-layer configuration.
* Connection management.
* Error detection.

PPP supports authentication methods such as:

| Method | Description |
| --- | --- |
| PAP | Password Authentication Protocol |
| CHAP | Challenge Handshake Authentication Protocol |
| MS-CHAPv1 | Microsoft Challenge Handshake Authentication Protocol v1 |
| MS-CHAPv2 | Microsoft Challenge Handshake Authentication Protocol v2 |
| EAP | Extensible Authentication Protocol |

Use secure authentication methods supported by the client and server. Avoid PAP where credentials could be exposed without an encrypted transport.

# 🗁 How L2TP/IPsec Works

L2TP provides the tunnel, while IPsec protects the traffic.

A simplified connection flow:

```text
       L2TP Client                MikroTik Router
            |                           |
            |------ IKE/IPsec --------->|
            |                           |
            |<---- IPsec Negotiation ---|
            |                           |
            |====== IPsec Tunnel =======|
            |                           |
            |------ L2TP Session ------>|
            |                           |
            |<----- PPP Negotiation ----|
            |                           |
            |------ Authentication ---->|
            |                           |
            |<----- IP Address ---------|
            |                           |
            |====== VPN Connected ======|
```

The client first establishes IPsec protection.

L2TP then operates inside the protected connection, and PPP handles user authentication and network configuration.

# 🗁 L2TP and PPP Architecture

```text
                    Internet
                        |
                        |
                 MikroTik Router
                   L2TP Server
                        |
                   IPsec Layer
                        |
                   L2TP Tunnel
                        |
                    PPP Layer
                        |
                 VPN Address Pool
                        |
                 Remote VPN Client
```

The resulting PPP connection receives an IP address and can access permitted networks according to routing and firewall policies.

# 🗁 L2TP vs PPPoE

L2TP and PPPoE both use PPP, but they serve different purposes.

| Feature | L2TP | PPPoE |
| --- | --- | --- |
| Full Name | Layer 2 Tunneling Protocol | PPP over Ethernet |
| Primary Purpose | Tunnel remote connections | Establish PPP sessions over Ethernet |
| Transport | UDP | Ethernet |
| Common Use | Remote-access VPN | ISP broadband access |
| Authentication | PPP authentication | PPP authentication |
| Encryption | Requires IPsec or another secure layer | No built-in encryption |
| MikroTik Interface | L2TP server/client | PPPoE server/client |

L2TP is commonly used for VPN connectivity, while PPPoE is widely used by ISPs to authenticate and manage subscribers.

# 🗁 L2TP/IPsec Network Topology

This example configures a MikroTik router as an L2TP/IPsec server.

```text
                       Internet
                           |
                           |
                    MikroTik Router
                     L2TP/IPsec
                           |
                      Private LAN
                    192.168.10.0/24
                           |
                     LAN Devices

                           ^
                           |
                      IPsec Tunnel
                           |
                     Remote Client
                     10.30.30.2
```

Example addressing:

| Component | Address |
| --- | --- |
| Router LAN | `192.168.10.0/24` |
| Router LAN gateway | `192.168.10.1` |
| VPN server address | `10.30.30.1` |
| VPN client pool | `10.30.30.10-10.30.30.100` |
| VPN client example | `10.30.30.2` |
| L2TP port | UDP `1701` |
| IPsec IKE | UDP `500` |
| IPsec NAT-T | UDP `4500` |

The example assumes that the router has a reachable public WAN address or is correctly reachable through an upstream NAT device.

# 🗁 Preparing the Router

Before configuring the L2TP server:

* Ensure internet connectivity.
* Confirm that the WAN address is reachable.
* Choose a VPN subnet that does not overlap with existing networks.
* Prepare a strong IPsec pre-shared key.
* Prepare individual VPN usernames and passwords.
* Review the existing firewall policy.
* Confirm that remote clients support L2TP/IPsec.

# 🗁 Creating an L2TP Address Pool

Create an address pool for remote VPN clients:

```routeros
/ip pool
add name=l2tp-pool ranges=10.30.30.10-10.30.30.100
```

The pool provides addresses to connected clients.

Keep the pool separate from the LAN DHCP range and other VPN address pools.

# 🗁 Creating a PPP Profile

Create a PPP profile:

```routeros
/ppp profile
add name=l2tp-profile local-address=10.30.30.1 remote-address=l2tp-pool use-encryption=required
```

The profile defines:

* Local VPN address.
* Remote client address assignment.
* PPP encryption requirements.

The `local-address` is the server-side address used for the PPP connection.

The `remote-address` identifies the pool from which client addresses are assigned.

# 🗁 Creating an L2TP User

Create a PPP secret:

```routeros
/ppp secret
add name=l2tp-user password=REPLACE_WITH_STRONG_PASSWORD service=l2tp profile=l2tp-profile
```

Replace the example password with a strong, unique password.

The `service=l2tp` setting restricts this account to L2TP connections.

For multiple remote users, create separate accounts.

# 🗁 Configuring the L2TP Server

Enable the L2TP server:

```routeros
/interface l2tp-server server
set enabled=yes use-ipsec=required ipsec-secret=REPLACE_WITH_STRONG_IPSEC_SECRET default-profile=l2tp-profile authentication=mschap2
```

Configuration details:

| Parameter | Purpose |
| --- | --- |
| `enabled` | Enables the L2TP server |
| `use-ipsec` | Requires IPsec protection |
| `ipsec-secret` | Shared IPsec authentication secret |
| `default-profile` | Default PPP profile |
| `authentication` | Permitted PPP authentication methods |

The client must use the same IPsec pre-shared key.

Use a long, random secret rather than a short or predictable password.

# 🗁 Understanding L2TP and IPsec Ports

L2TP/IPsec commonly requires the following protocols:

| Protocol | Port or Number | Purpose |
| --- | ---: | --- |
| UDP | 500 | IKE negotiation |
| UDP | 4500 | IPsec NAT Traversal |
| UDP | 1701 | L2TP |
| ESP | IP protocol 50 | IPsec Encapsulating Security Payload |

When NAT Traversal is used, ESP is generally encapsulated inside UDP port 4500.

Do not assume that allowing UDP 1701 alone is sufficient for an L2TP/IPsec connection.

# 🗁 Configuring the Firewall

Allow IKE negotiation:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=500 in-interface=ether1 comment="Allow IPsec IKE"
```

Allow NAT Traversal:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=4500 in-interface=ether1 comment="Allow IPsec NAT-T"
```

Allow L2TP traffic:

```routeros
/ip firewall filter
add chain=input action=accept protocol=udp dst-port=1701 ipsec-policy=in,ipsec in-interface=ether1 comment="Allow L2TP through IPsec"
```

Allow native ESP when required by the network path:

```routeros
/ip firewall filter
add chain=input action=accept protocol=ipsec-esp in-interface=ether1 comment="Allow IPsec ESP"
```

The L2TP rule is restricted to IPsec-protected traffic through the `ipsec-policy` matcher.

Place these rules before applicable input-chain drop rules.

Replace `ether1` with the actual WAN interface.

# 🗁 Allowing VPN Clients to Access the LAN

Allow VPN clients to access the private LAN:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.30.30.0/24 dst-address=192.168.10.0/24 comment="Allow L2TP clients to LAN"
```

For bidirectional communication, ensure that the reverse direction is permitted by the firewall policy.

If the MikroTik router is the LAN's default gateway, it can normally route return traffic to the VPN subnet.

If another router is the LAN gateway, configure a route for `10.30.30.0/24` through the MikroTik router.

# 🗁 Connecting from Windows

Windows includes a built-in L2TP/IPsec VPN client.

To configure a connection:

* Open Windows Settings.
* Navigate to Network and Internet.
* Open VPN.
* Select Add VPN.
* Choose Windows (built-in) as the VPN provider.
* Enter the public IP address or hostname of the MikroTik router.
* Select L2TP/IPsec with pre-shared key as the VPN type.
* Enter the IPsec pre-shared key.
* Select username and password authentication.
* Enter the PPP username and password.
* Save the connection.

Connect to the VPN and verify that the client receives an address from the configured pool.

The exact menu names may vary between Windows versions.

# 🗁 Connecting from Linux

NetworkManager supports L2TP connections through an additional plugin.

On Ubuntu or Debian, install the relevant packages:

```bash
sudo apt update
sudo apt install network-manager-l2tp network-manager-l2tp-gnome
```

Restart NetworkManager:

```bash
sudo systemctl restart NetworkManager
```

Create an L2TP VPN connection through the network settings interface.

Configure:

* Gateway hostname or IP address.
* PPP username and password.
* L2TP/IPsec mode.
* IPsec pre-shared key.
* Compatible authentication settings.

The exact configuration options depend on the installed NetworkManager plugin version.

# 🗁 Split Tunneling

Split tunneling sends only selected network traffic through the VPN.

For example:

```text
Remote Client
     |
     +---- Internet Traffic ----> Local ISP
     |
     +---- 192.168.10.0/24 -----> VPN Tunnel
```

The remote client continues using its local internet connection while private LAN traffic travels through the VPN.

This is useful when remote users only need access to specific internal services.

The necessary routes must be installed on the client.

# 🗁 Full Tunneling

Full tunneling sends the client's default internet traffic through the VPN.

```text
Remote Client
     |
     |
  VPN Tunnel
     |
     |
MikroTik Router
     |
     |
  Internet
```

For full tunneling:

* The client must route its default traffic through the VPN.
* The router must allow VPN traffic toward the WAN.
* Source NAT must be configured where required.
* DNS resolution must work through the selected path.

Example NAT rule:

```routeros
/ip firewall nat
add chain=srcnat action=masquerade src-address=10.30.30.0/24 out-interface=ether1 comment="NAT L2TP clients"
```

This rule allows VPN clients to use the router's WAN connection for internet access.

# 🗁 Monitoring Active L2TP Connections

Display the L2TP server configuration:

```routeros
/interface l2tp-server server print detail
```

Display active PPP sessions:

```routeros
/ppp active print detail
```

Display configured VPN accounts:

```routeros
/ppp secret print
```

Display the PPP profile:

```routeros
/ppp profile print detail
```

Display address pools:

```routeros
/ip pool print detail
```

Display active IPsec peers:

```routeros
/ip ipsec active-peers print
```

Display installed IPsec security associations:

```routeros
/ip ipsec installed-sa print
```

These commands help verify the L2TP configuration, active users, assigned addresses, and IPsec protection.

# 🗁 Understanding PPP Profiles

PPP profiles allow administrators to apply common settings to multiple VPN accounts.

A profile can define:

* Local address.
* Remote address or address pool.
* DNS servers.
* Session limits.
* Encryption requirements.
* Queue settings.
* Routing-related settings.

For example, assign DNS servers through a profile:

```routeros
/ppp profile
set l2tp-profile dns-server=192.168.10.1
```

The DNS server should be reachable by VPN clients and configured to resolve the required names.

# 🗁 Managing PPP Users

Create another VPN user:

```routeros
/ppp secret
add name=remote-user-2 password=REPLACE_WITH_ANOTHER_STRONG_PASSWORD service=l2tp profile=l2tp-profile
```

Disable an account:

```routeros
/ppp secret
disable [find name=remote-user-2]
```

Enable an account:

```routeros
/ppp secret
enable [find name=remote-user-2]
```

Remove an account:

```routeros
/ppp secret
remove [find name=remote-user-2]
```

Individual accounts make access management and auditing easier.

# 🗁 Understanding PPPoE and PPP in ISP Networks

PPP is also widely used in ISP environments.

Common PPP-based services include:

* PPPoE.
* L2TP.
* PPTP.
* Serial PPP links.
* Other supported point-to-point access technologies.

MikroTik RouterOS provides centralized PPP management through:

```routeros
/ppp
```

Administrators can use PPP profiles and secrets to manage subscriber or remote-access connection parameters.

For ISP deployments, PPPoE is commonly used for subscriber authentication, while L2TP is more commonly associated with tunneling and VPN connectivity.

# 🗁 L2TP/IPsec Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| Client cannot establish VPN | Incorrect server address or blocked ports |
| IPsec negotiation fails | Incorrect pre-shared key or incompatible IPsec settings |
| L2TP connection fails | UDP 1701 blocked or L2TP server disabled |
| PPP authentication fails | Incorrect username or password |
| Client receives no IP address | Incorrect PPP profile or address pool |
| VPN connects but LAN is unreachable | Missing forward permission or return route |
| VPN connects but internet does not work | Missing default route or NAT |
| Connection fails behind NAT | NAT Traversal or upstream firewall issue |
| Connection drops frequently | Unstable network or timeout settings |
| DNS resolution fails | Incorrect or unreachable DNS server |

# 🗁 Troubleshooting Commands

Inspect the L2TP server:

```routeros
/interface l2tp-server server print detail
```

Inspect active PPP sessions:

```routeros
/ppp active print detail
```

Inspect PPP secrets and profiles:

```routeros
/ppp secret print detail
/ppp profile print detail
```

Inspect IPsec peers and security associations:

```routeros
/ip ipsec active-peers print detail
/ip ipsec installed-sa print detail
```

Inspect firewall counters:

```routeros
/ip firewall filter print stats
/ip firewall nat print stats
```

Inspect router logs:

```routeros
/log print
```

Look for IPsec negotiation errors, authentication failures, and PPP session events.

# 🗁 L2TP/IPsec Security Best Practices

* Require IPsec protection for L2TP over untrusted networks.
* Use a strong, unique pre-shared key.
* Use individual PPP accounts.
* Use strong passwords.
* Restrict VPN access to required LAN resources.
* Keep RouterOS updated.
* Avoid unnecessary management-service exposure.
* Monitor active PPP sessions.
* Disable unused VPN accounts.
* Review authentication and IPsec logs.
* Avoid weak or obsolete authentication methods.
* Apply firewall rules before general drop rules.

# 🗁 Removing L2TP Configuration

Disable the L2TP server:

```routeros
/interface l2tp-server server
set enabled=no
```

Remove the dedicated VPN user:

```routeros
/ppp secret
remove [find name=l2tp-user]
```

Remove the PPP profile:

```routeros
/ppp profile
remove [find name=l2tp-profile]
```

Remove the address pool:

```routeros
/ip pool
remove [find name=l2tp-pool]
```

Remove the firewall rules created for this lab:

```routeros
/ip firewall filter
remove [find comment="Allow IPsec IKE"]
remove [find comment="Allow IPsec NAT-T"]
remove [find comment="Allow L2TP through IPsec"]
remove [find comment="Allow IPsec ESP"]
remove [find comment="Allow L2TP clients to LAN"]
```

Remove the NAT rule if it was created specifically for this deployment:

```routeros
/ip firewall nat
remove [find comment="NAT L2TP clients"]
```

Do not remove IPsec configuration or firewall rules shared by other VPN services.

# 🗁 Practical Exercises

**Basic L2TP/IPsec Server**

* Create a VPN address pool.
* Create a PPP profile.
* Create a PPP secret.
* Enable the L2TP server.
* Configure IPsec protection.
* Configure firewall permissions.

**Windows Client Lab**

* Create an L2TP/IPsec VPN connection in Windows.
* Configure the server hostname.
* Enter the pre-shared key.
* Enter PPP credentials.
* Connect to the VPN.
* Verify the assigned VPN address.

**Remote LAN Access Lab**

* Allow VPN clients to access the LAN.
* Verify routing.
* Test access to internal devices.
* Inspect firewall counters.

**Full Tunnel Lab**

* Configure client default routing through the VPN.
* Configure source NAT.
* Verify internet access.
* Test DNS resolution.

**PPP Management Lab**

* Create multiple PPP users.
* Assign different PPP profiles.
* Disable and enable accounts.
* Inspect active sessions.
* Remove an unused account.

# 🗁 Lab Completion Checklist

* [ ] Understand L2TP.
* [ ] Understand PPP.
* [ ] Understand L2TP/IPsec architecture.
* [ ] Understand PPP authentication methods.
* [ ] Create a VPN address pool.
* [ ] Configure a PPP profile.
* [ ] Create PPP user accounts.
* [ ] Enable the L2TP server.
* [ ] Configure IPsec protection.
* [ ] Configure required firewall permissions.
* [ ] Connect from Windows or Linux.
* [ ] Verify assigned VPN addresses.
* [ ] Configure remote LAN access.
* [ ] Understand split tunneling.
* [ ] Understand full tunneling.
* [ ] Monitor active PPP sessions.
* [ ] Troubleshoot IPsec and PPP authentication.
* [ ] Remove lab configuration safely.

# 🗁 Summary

L2TP and PPP are commonly used together to provide remote-access VPN connectivity.

L2TP establishes the tunnel, while PPP handles authentication and connection configuration. IPsec protects the connection against interception and tampering.

Important concepts include:

* L2TP tunneling.
* PPP authentication.
* PPP profiles and secrets.
* VPN address pools.
* L2TP/IPsec.
* Firewall and NAT configuration.
* Split and full tunneling.
* Active session monitoring.
* Troubleshooting and security.
