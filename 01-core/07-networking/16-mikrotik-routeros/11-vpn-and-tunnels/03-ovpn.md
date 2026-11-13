# OpenVPN (OVPN)

OpenVPN is an open-source VPN solution that creates encrypted tunnels between remote devices and VPN servers.

MikroTik RouterOS supports OpenVPN for secure remote access and site-to-site connectivity.

OpenVPN uses TLS-based authentication and can operate over TCP or UDP, depending on the RouterOS version and configuration.

It is useful for:

* Remote employee access.
* Secure administration of MikroTik routers.
* Connecting branch offices.
* Accessing private LAN resources remotely.
* Connecting Windows and Linux clients.
* Establishing VPN connectivity across networks with restrictive firewall policies.

# 🗁 What Is OpenVPN?

OpenVPN is a VPN protocol that uses TLS for authentication and secure key exchange.

It creates a virtual network interface through which encrypted traffic passes.

A typical deployment consists of:

* **OpenVPN Server:** Accepts incoming VPN connections.
* **OpenVPN Client:** Connects to the server.
* **Certificate Authority (CA):** Issues and signs certificates.
* **Server Certificate:** Identifies the VPN server.
* **Client Certificate:** Identifies a client when certificate-based client authentication is configured.
* **VPN Address Pool:** Provides IP addresses to connected clients.

```text
                  Internet
                      |
                      |
                OpenVPN Server
                 MikroTik
                      |
                Encrypted Tunnel
                      |
             +--------+--------+
             |                 |
          Client A          Client B
          Windows           Linux
             |                 |
             +--------+--------+
                      |
                 Private LAN
               192.168.10.0/24
```

# 🗁 How OpenVPN Works

OpenVPN establishes a secure connection using TLS-based authentication and negotiation.

A simplified connection process:

```text
       OpenVPN Client             OpenVPN Server
              |                         |
              |------ Connection ------->|
              |                         |
              |<----- TLS Exchange -----|
              |                         |
              |---- Authentication --->|
              |                         |
              |<--- VPN Parameters -----|
              |                         |
              |====== Encrypted Tunnel =|
              |                         |
              |---- VPN Data Traffic -->|
              |<--- VPN Data Traffic ---|
```

The exact negotiation and authentication steps depend on the OpenVPN configuration and software version.

After successful connection, the client can communicate through the configured VPN routes and access policies.

# 🗁 OpenVPN Components

| Component | Description |
| --- | --- |
| OpenVPN Server | Accepts and manages VPN connections |
| OpenVPN Client | Initiates the VPN connection |
| Certificate Authority | Signs and validates certificates |
| Server Certificate | Identifies the VPN server |
| Client Certificate | Identifies a client when required |
| Private Key | Secret key associated with a certificate |
| TLS | Protects authentication and key negotiation |
| VPN Address Pool | Provides addresses to connected clients |
| Virtual Interface | Carries VPN network traffic |

# 🗁 OpenVPN in MikroTik RouterOS

RouterOS v7 includes OpenVPN server and client functionality.

Important configuration areas include:

* PPP profiles.
* PPP secrets.
* OpenVPN server configuration.
* OpenVPN client configuration.
* Certificates.
* IP address pools.
* Firewall rules.
* Routes.
* NAT configuration.

RouterOS OpenVPN capabilities and supported cryptographic options vary by RouterOS release.

For a new deployment, verify the supported options on the installed RouterOS version and ensure compatibility with the remote OpenVPN client.

# 🗁 OpenVPN Network Topology

The following lab configures a MikroTik router as an OpenVPN server.

```text
                    Internet
                        |
                        |
                 MikroTik Router
                  OpenVPN Server
                        |
                  WAN: ether1
                        |
                  LAN: bridge
                        |
                192.168.10.0/24
                        |
                  Private Devices

                        ^
                        |
                 Encrypted VPN
                        |
                  Remote Client
                  10.20.20.2
```

Example addressing:

| Component | Address |
| --- | --- |
| Router LAN | `192.168.10.0/24` |
| Router LAN gateway | `192.168.10.1` |
| VPN address pool | `10.20.20.10-10.20.20.100` |
| VPN server address | `10.20.20.1` |
| Remote client | `10.20.20.2` |
| OpenVPN port | `1194` |
| Transport | TCP |

The example uses TCP port 1194 for broad client compatibility.

Replace the WAN interface, public address, and network values according to your environment.

# 🗁 Preparing the OpenVPN Server

Before configuring the server:

* Ensure the router has working internet connectivity.
* Verify that the WAN endpoint is reachable.
* Confirm that the selected OpenVPN port is allowed through upstream firewalls.
* Prepare the required certificates.
* Select a VPN address range that does not overlap with existing networks.
* Confirm that the remote client supports the configured authentication and encryption options.

For this lab, use certificate-based server authentication and username/password authentication for clients.

# 🗁 Creating a Certificate Authority

A Certificate Authority (CA) signs certificates used by the OpenVPN deployment.

Create the CA template:

```routeros
/certificate
add name=ovpn-ca-template common-name=ovpn-ca key-usage=key-cert-sign,crl-sign
```

Sign the CA certificate:

```routeros
/certificate
sign ovpn-ca-template name=ovpn-ca
```

The resulting CA certificate is used to sign the server certificate.

# 🗁 Creating the OpenVPN Server Certificate

Create a server certificate template:

```routeros
/certificate
add name=ovpn-server-template common-name=vpn.example.com subject-alt-name=DNS:vpn.example.com key-usage=digital-signature,key-encipherment,tls-server
```

Sign the server certificate using the CA:

```routeros
/certificate
sign ovpn-server-template ca=ovpn-ca name=ovpn-server
```

The certificate's identity must match the hostname used by clients when hostname validation is enabled.

For production deployments, use a real DNS hostname and appropriate certificate validity periods.

# 🗁 Verifying Certificates

Display configured certificates:

```routeros
/certificate print detail
```

Verify that:

* The CA certificate is signed.
* The server certificate is signed by the intended CA.
* The server certificate has the required key usage.
* The certificate is valid.
* The private key is available on the server.

The CA certificate must be trusted by the remote clients.

# 🗁 Exporting the CA Certificate

Export the CA certificate without its private key:

```routeros
/certificate
export-certificate ovpn-ca type=pem file-name=ovpn-ca
```

The exported CA certificate is typically available through the router's file storage.

Download the CA certificate and securely distribute it to authorized clients.

The CA private key must remain protected on the server and must never be distributed to VPN clients.

# 🗁 Creating an OpenVPN IP Pool

Create an address pool for remote VPN clients:

```routeros
/ip pool
add name=ovpn-pool ranges=10.20.20.10-10.20.20.100
```

This pool provides addresses to connected VPN clients.

Ensure that the range does not overlap with:

* The local LAN.
* Other VPN networks.
* Existing routed networks.
* DHCP address pools.

# 🗁 Creating an OpenVPN PPP Profile

OpenVPN server connections in RouterOS use PPP profiles.

Create a profile:

```routeros
/ppp profile
add name=ovpn-profile local-address=10.20.20.1 remote-address=ovpn-pool use-encryption=required
```

The profile defines:

* Local VPN address.
* Remote client address pool.
* Encryption requirements.

The `local-address` is the server-side address used for the PPP connection.

The `remote-address` specifies the address pool assigned to clients.

# 🗁 Creating an OpenVPN User

Create a VPN account:

```routeros
/ppp secret
add name=ovpn-user password=REPLACE_WITH_STRONG_PASSWORD service=ovpn profile=ovpn-profile
```

Replace the example password with a strong, unique credential.

The account is used by the OpenVPN client to authenticate.

Each user should have an individual account rather than sharing one account across multiple people.

# 🗁 Configuring the OpenVPN Server

Enable the OpenVPN server:

```routeros
/interface ovpn-server server
set enabled=yes port=1194 mode=ip protocol=tcp netmask=24 certificate=ovpn-server require-client-certificate=no default-profile=ovpn-profile auth=sha256 cipher=aes256-cbc
```

This example configures:

| Parameter | Value | Purpose |
| --- | --- | --- |
| `enabled` | `yes` | Enables the server |
| `port` | `1194` | Listening TCP port |
| `mode` | `ip` | Layer 3 VPN mode |
| `protocol` | `tcp` | TCP transport |
| `certificate` | `ovpn-server` | Server certificate |
| `require-client-certificate` | `no` | Does not require individual client certificates |
| `default-profile` | `ovpn-profile` | Default PPP profile |
| `auth` | `sha256` | Packet authentication algorithm |
| `cipher` | `aes256-cbc` | Data encryption algorithm |

The selected algorithms must be supported by both RouterOS and the OpenVPN client.

OpenVPN implementations and RouterOS releases differ in supported TLS and data-channel options. Verify the actual server properties with:

```routeros
/interface ovpn-server server print detail
```

For production deployments, use the strongest mutually supported settings and prefer modern data-channel cipher suites where the installed RouterOS release supports them.

# 🗁 Allowing OpenVPN Traffic Through the Firewall

The OpenVPN server must accept incoming connections on its listening port.

For this example, allow TCP port 1194:

```routeros
/ip firewall filter
add chain=input action=accept protocol=tcp dst-port=1194 in-interface=ether1 comment="Allow OpenVPN"
```

Place this rule before any input-chain drop rule that would otherwise block the connection.

If the router is behind an upstream NAT device, forward TCP port 1194 to the MikroTik router.

Restrict access to the intended WAN interface and remote source addresses where practical.

# 🗁 Allowing VPN Clients to Access the LAN

VPN clients may need access to private LAN resources.

Allow traffic from the VPN subnet to the LAN:

```routeros
/ip firewall filter
add chain=forward action=accept src-address=10.20.20.0/24 dst-address=192.168.10.0/24 comment="Allow OVPN clients to LAN"
```

If LAN devices must initiate connections to VPN clients, configure the corresponding reverse-direction firewall permission.

Place the rules before relevant forward-chain drop rules.

# 🗁 Configuring Return Routing

The local LAN must know how to return traffic to the VPN client network.

If the MikroTik router is the default gateway for the LAN, it normally already has the required connected network and can route replies toward the VPN clients.

If another router is the LAN gateway, configure a route on that router:

```text
Destination: 10.20.20.0/24
Next Hop:    192.168.10.1
```

This ensures that return traffic reaches the MikroTik router.

An alternative is source NAT, but routing the VPN subnet is generally preferable when the LAN infrastructure supports it.

# 🗁 Exporting the OpenVPN Client Configuration

OpenVPN clients need connection parameters and the CA certificate.

A typical client configuration for a compatible client is:

```ini
client
dev tun
proto tcp-client
remote vpn.example.com 1194
nobind
persist-key
persist-tun
remote-cert-tls server
auth SHA256
cipher AES-256-CBC
verb 3

auth-user-pass
ca ovpn-ca.crt
```

Replace `vpn.example.com` with the router's reachable public hostname.

The `ovpn-ca.crt` file must contain the exported CA certificate.

This example reflects the TCP and cipher settings configured on the RouterOS server. Client options must match the actual RouterOS capabilities and negotiated settings.

The client will request the PPP username and password when connecting.

# 🗁 Connecting from a Linux Client

Install OpenVPN on Ubuntu or Debian:

```bash
sudo apt update
sudo apt install openvpn
```

Place the client configuration and CA certificate in the same directory.

Connect:

```bash
sudo openvpn --config client.ovpn
```

The client prompts for the configured VPN username and password.

After successful authentication, inspect the network interfaces:

```bash
ip addr
```

Inspect the routing table:

```bash
ip route
```

Test the VPN server's tunnel address:

```bash
ping 10.20.20.1
```

Test access to the private LAN:

```bash
ping 192.168.10.1
```

# 🗁 Connecting from a Windows Client

Install a compatible OpenVPN client on Windows.

Prepare the configuration files:

```text
client.ovpn
ovpn-ca.crt
```

Import the configuration into the OpenVPN client application.

Connect using the configured VPN username and password.

After connecting, verify:

* The VPN connection status.
* The assigned VPN address.
* The installed routes.
* Access to private LAN resources.

The exact interface and import process depend on the OpenVPN client version.

# 🗁 Monitoring OpenVPN Connections

Display the OpenVPN server configuration:

```routeros
/interface ovpn-server server print detail
```

Display active PPP connections:

```routeros
/ppp active print detail
```

Display PPP secrets:

```routeros
/ppp secret print
```

Display VPN address pools:

```routeros
/ip pool print detail
```

Inspect firewall counters:

```routeros
/ip firewall filter print stats
```

These commands help verify server configuration, active clients, assigned addresses, and firewall traffic.

# 🗁 Understanding OpenVPN and Routing

OpenVPN remote access can use split tunneling or full tunneling.

**Split Tunneling**

Only selected private networks use the VPN.

For example:

```text
VPN Network: 10.20.20.0/24
LAN Network: 192.168.10.0/24
```

Internet traffic continues through the client's normal internet connection.

**Full Tunneling**

All client internet traffic is sent through the VPN.

The client must route the default IPv4 route through the VPN, and the MikroTik router must provide suitable internet routing and source NAT.

For a full-tunnel deployment, the client configuration commonly includes:

```text
redirect-gateway def1
```

The router must also permit VPN client traffic to reach the WAN and apply the required NAT policy.

For example:

```routeros
/ip firewall nat
add chain=srcnat action=masquerade src-address=10.20.20.0/24 out-interface=ether1 comment="NAT OpenVPN clients"
```

This rule is relevant when VPN clients need internet access through the MikroTik WAN connection.

# 🗁 OpenVPN and MTU

VPN encapsulation adds overhead to network packets.

If packet sizes exceed the effective path MTU, fragmentation or packet loss may occur.

Symptoms can include:

* Some websites load while others do not.
* Large downloads stall.
* Connections work inconsistently.
* Small ping packets succeed but larger transfers fail.

Investigate:

* WAN MTU.
* Tunnel MTU.
* Path MTU Discovery.
* TCP MSS behavior.
* Fragmentation and firewall handling.

Do not apply arbitrary MTU values without testing the actual path.

# 🗁 OpenVPN Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| Client cannot connect | Incorrect endpoint, blocked port, or disabled server |
| TLS handshake fails | Invalid certificate or incompatible TLS settings |
| Authentication fails | Incorrect username or password |
| Client connects but cannot access LAN | Missing forward permission or route |
| Client receives no VPN address | Incorrect PPP profile or IP pool |
| Internet stops after connecting | Incorrect full-tunnel routing or NAT |
| Some websites fail to load | MTU or fragmentation issue |
| Server certificate rejected | Incorrect CA trust or certificate identity |
| Connection drops repeatedly | Unstable WAN, client network, or timeout behavior |

Useful diagnostic commands:

```routeros
/interface ovpn-server server print detail
/ppp active print detail
/ppp profile print detail
/ppp secret print
/ip pool print detail
```

Inspect firewall rules and counters:

```routeros
/ip firewall filter print stats
/ip firewall nat print stats
```

Inspect router logs:

```routeros
/log print where topics~"ovpn"
```

The exact log topics available depend on the RouterOS release and logging configuration.

# 🗁 OpenVPN Security Best Practices

* Use IKEv2 or WireGuard when they better meet the deployment requirements; choose OpenVPN when its compatibility or architecture is needed.
* Use strong, unique user credentials.
* Use certificates signed by a trusted CA.
* Protect the CA private key.
* Validate the server certificate on clients.
* Prefer modern cryptographic algorithms supported by the installed RouterOS release and clients.
* Restrict the listening port to the intended WAN interface.
* Apply explicit forward-chain access policies.
* Avoid unnecessary access to router management services.
* Use individual accounts for remote users.
* Remove unused PPP secrets and certificates.
* Keep RouterOS and client software updated.
* Monitor active sessions and authentication failures.

# 🗁 Removing OpenVPN Configuration

Disable the OpenVPN server:

```routeros
/interface ovpn-server server
set enabled=no
```

Remove the VPN user:

```routeros
/ppp secret
remove [find name="ovpn-user"]
```

Remove the PPP profile:

```routeros
/ppp profile
remove [find name="ovpn-profile"]
```

Remove the VPN address pool:

```routeros
/ip pool
remove [find name="ovpn-pool"]
```

Remove the firewall rule created for OpenVPN:

```routeros
/ip firewall filter
remove [find comment="Allow OpenVPN"]
```

Remove the VPN client forwarding rule:

```routeros
/ip firewall filter
remove [find comment="Allow OVPN clients to LAN"]
```

Remove the NAT rule if it was created specifically for this lab:

```routeros
/ip firewall nat
remove [find comment="NAT OpenVPN clients"]
```

Remove the server certificate and CA only when they are no longer used by any other service or certificate-dependent configuration.

Only remove entries created specifically for the lab.

# 🗁 Practical Exercises

**Exercise: Basic OpenVPN Server**

* Create a CA certificate.
* Create and sign a server certificate.
* Create a VPN address pool.
* Create a PPP profile.
* Create an OpenVPN user.
* Enable the OpenVPN server.
* Allow the listening port through the firewall.

**Exercise: Remote Access**

* Export the CA certificate.
* Configure a Linux or Windows OpenVPN client.
* Import the client configuration.
* Connect to the VPN.
* Verify the assigned VPN address.
* Access the router's LAN.

**Exercise: Routing and Firewall**

* Configure split tunneling.
* Permit access to selected LAN resources.
* Verify return routing.
* Test firewall restrictions.
* Configure full tunneling and internet NAT.

**Exercise: Troubleshooting**

* Test an incorrect username and password.
* Test a missing firewall permission.
* Inspect certificate validation errors.
* Check PPP active sessions.
* Inspect logs and firewall counters.

# 🗁 Lab Completion Checklist

* [ ] Understand OpenVPN architecture.
* [ ] Understand TLS and certificate authentication.
* [ ] Create a Certificate Authority.
* [ ] Create and sign a server certificate.
* [ ] Export the CA certificate.
* [ ] Create an IP pool.
* [ ] Configure a PPP profile.
* [ ] Create an OpenVPN user.
* [ ] Enable the OpenVPN server.
* [ ] Configure firewall permissions.
* [ ] Connect from a Linux or Windows client.
* [ ] Verify VPN address assignment.
* [ ] Configure LAN access and routing.
* [ ] Test split and full tunneling.
* [ ] Troubleshoot common connection problems.
* [ ] Remove temporary lab configurations.

# 🗁 Summary

OpenVPN provides encrypted remote-access and site-to-site connectivity using TLS-based authentication and VPN interfaces.

Important concepts include:

* OpenVPN server and client.
* TLS authentication.
* Certificate Authority.
* Server certificates.
* PPP profiles and secrets.
* VPN address pools.
* TCP and UDP transport.
* Firewall and routing configuration.
* Split tunneling and full tunneling.
* NAT and MTU considerations.
* Monitoring and troubleshooting.

OpenVPN is useful when interoperability with existing OpenVPN clients and infrastructure is required.
