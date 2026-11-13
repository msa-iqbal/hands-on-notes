# RADIUS Integration

RADIUS (Remote Authentication Dial-In User Service) is a centralized authentication, authorization, and accounting (AAA) protocol used to manage network access.

In ISP networks, RADIUS allows network administrators to manage subscriber accounts centrally instead of maintaining individual authentication databases on every MikroTik router.

MikroTik RouterOS supports RADIUS integration for services such as:

* PPPoE
* Hotspot
* PPP-based VPN services
* Wireless authentication
* Router administrative login

RADIUS is especially useful for ISPs that operate multiple routers, access concentrators, or subscriber networks.

# 🗁 What Is RADIUS?

RADIUS is a client-server protocol that allows network access devices to communicate with a centralized authentication server.

The MikroTik router acts as a RADIUS client, while a separate server handles authentication, authorization, and accounting.

A typical ISP architecture looks like this:

```text
                  Internet
                     |
               +-----------+
               |  Core     |
               |  Router   |
               +-----------+
                     |
              +-------------+
              | MikroTik    |
              | PPPoE       |
              | Server      |
              +-------------+
                     |
                Subscribers

                     |
              RADIUS Requests
                     |
              +-------------+
              | RADIUS      |
              | Server      |
              +-------------+
                     |
              Subscriber Database
```

The MikroTik router sends authentication requests to the RADIUS server. The server checks the subscriber's credentials and returns an authentication decision with optional service attributes.

# 🗁 Understand AAA

RADIUS commonly provides three major functions.

**Authentication**

Authentication verifies the identity of a user.

Examples:

* Username and password validation
* Subscriber account verification
* Access credential validation

**Authorization**

Authorization determines what an authenticated user is allowed to do.

Examples:

* IP address assignment
* Bandwidth limits
* Session duration
* Simultaneous session limits
* Service permissions

**Accounting**

Accounting records information about user sessions and network usage.

Examples:

* Login time
* Logout time
* Session duration
* Data transferred
* Assigned IP address
* Disconnect reason

# 🗁 Understand RADIUS Message Types

RADIUS uses different message types for authentication and accounting.

| Message | Purpose |
| --- | --- |
| Access-Request | Requests authentication and authorization |
| Access-Accept | Approves the authentication request |
| Access-Reject | Rejects the authentication request |
| Access-Challenge | Requests additional authentication information |
| Accounting-Request | Sends accounting information |
| Accounting-Response | Acknowledges an accounting request |

A simplified authentication flow:

```text
       MikroTik Router
        RADIUS Client
              |
              | Access-Request
              |
              v
        RADIUS Server
              |
        Verify Credentials
              |
        +-----+-----+
        |           |
        v           v
  Access-Accept  Access-Reject
        |           |
        v           v
   Allow Access  Deny Access
```

# 🗁 Understand RADIUS Ports

RADIUS traditionally uses UDP.

| Port | Protocol | Purpose |
| --- | --- | --- |
| 1812 | UDP | Authentication and authorization |
| 1813 | UDP | Accounting |
| 1645 | UDP | Legacy authentication port |
| 1646 | UDP | Legacy accounting port |

Modern RADIUS deployments generally use UDP ports 1812 and 1813.

Ensure that the firewall between the MikroTik router and RADIUS server permits the required traffic.

# 🗁 RADIUS Server Options

Several RADIUS server implementations can be used with MikroTik.

**FreeRADIUS**

FreeRADIUS is a widely used open-source RADIUS server.

It supports:

* User authentication
* SQL database integration
* LDAP integration
* Accounting
* Policy-based authorization
* Multiple network access devices

**MikroTik User Manager**

User Manager is MikroTik's built-in RADIUS server package for RouterOS.

It provides centralized subscriber management and can be used with compatible RouterOS services.

**Other RADIUS Platforms**

An ISP may also use commercial or hosted RADIUS platforms that provide subscriber management, billing integration, accounting, and service automation.

# 🗁 Prepare the RADIUS Server

Assume the following network configuration:

| Component | Address |
| --- | --- |
| MikroTik router | 192.168.10.1 |
| RADIUS server | 192.168.10.10 |
| Authentication port | UDP 1812 |
| Accounting port | UDP 1813 |
| Shared secret | Strong shared secret |

The RADIUS server must:

* Be reachable from the MikroTik router.
* Recognize the MikroTik router as an authorized RADIUS client.
* Use the same shared secret configured on the router.
* Have subscriber authentication data.
* Support the authentication method used by the MikroTik service.

The shared secret is configured on both systems. It is not the subscriber's password.

# 🗁 Configure RADIUS on MikroTik

Add the RADIUS server:

```routeros
/radius
add service=ppp \
    address=192.168.10.10 \
    secret=StrongRadiusSecret \
    authentication-port=1812 \
    accounting-port=1813 \
    timeout=3s \
    comment="ISP RADIUS Server"
```

The `service` property specifies which RouterOS services can use this RADIUS entry.

For PPPoE authentication, use `service=ppp`.

For Hotspot authentication, use `service=hotspot`.

You can specify multiple services in a single entry when required:

```routeros
/radius
add service=ppp,hotspot \
    address=192.168.10.10 \
    secret=StrongRadiusSecret
```

Inspect the configuration:

```routeros
/radius print detail
```

# 🗁 Configure the RADIUS Client Source Address

The MikroTik router must send RADIUS requests from an address that the RADIUS server recognizes.

When the router has multiple interfaces or routing paths, configure the source address explicitly if necessary.

Example:

```routeros
/radius
set [find address=192.168.10.10] src-address=192.168.10.1
```

The selected source address must be assigned to the router and reachable by the RADIUS server.

The RADIUS server must recognize this address as the MikroTik client's IP address.

# 🗁 Enable RADIUS Authentication for PPPoE

Enable RADIUS authentication for PPP services:

```routeros
/ppp aaa
set use-radius=yes accounting=yes interim-update=5m
```

Important properties:

| Property | Description |
| --- | --- |
| `use-radius` | Enables RADIUS authentication for PPP services |
| `accounting` | Enables PPP accounting |
| `interim-update` | Defines the interval for interim accounting updates |

With accounting enabled, RouterOS sends session information to the RADIUS server.

Interim updates allow the server to receive periodic usage information while a subscriber remains connected.

# 🗁 Enable RADIUS Authentication for Hotspot

Enable RADIUS authentication in the Hotspot profile:

```routeros
/ip hotspot profile
set [find name=hotspot-profile] use-radius=yes
```

Replace `hotspot-profile` with the actual Hotspot profile name.

The Hotspot server can now use the configured RADIUS server for authentication.

For Hotspot accounting, verify the applicable profile and RADIUS settings.

# 🗁 Understand Subscriber Authentication

When a subscriber connects through PPPoE, the authentication process generally follows this sequence:

* The subscriber initiates a PPPoE connection.
* The MikroTik PPPoE server receives the connection.
* The router receives the subscriber's credentials.
* The router sends a RADIUS Access-Request.
* The RADIUS server validates the account.
* The server returns Access-Accept or Access-Reject.
* If accepted, the router establishes the PPP session using the returned authorization attributes and local configuration.

If the server rejects the request, the subscriber cannot establish the authenticated PPP session.

# 🗁 Understand RADIUS Authorization Attributes

RADIUS can return attributes that control subscriber service behavior.

Common attributes used in MikroTik environments include:

| RADIUS attribute | Purpose |
| --- | --- |
| `Framed-IP-Address` | Assigns a specific subscriber IP address |
| `Framed-Pool` | Selects an address pool |
| `Mikrotik-Rate-Limit` | Defines subscriber bandwidth limits |
| `Session-Timeout` | Limits session duration |
| `Idle-Timeout` | Limits idle session duration |
| `Port-Limit` | Controls the number of simultaneous sessions |
| `Framed-Route` | Supplies routing information where supported |

MikroTik also supports vendor-specific RADIUS attributes.

`Mikrotik-Rate-Limit` is particularly useful for ISP subscriber bandwidth management.

Example attribute:

```text
Mikrotik-Rate-Limit = "10M/25M"
```

This represents a 10 Mbps upload limit and a 25 Mbps download limit from the subscriber's perspective.

The RADIUS server must return attributes in a format supported by RouterOS and the corresponding service.

# 🗁 Configure RADIUS Accounting

Accounting allows the RADIUS server to record subscriber session information.

Enable PPP accounting:

```routeros
/ppp aaa
set accounting=yes interim-update=5m
```

Accounting records commonly include:

* Subscriber username
* Session identifier
* Session start time
* Session stop time
* Session duration
* Input octets
* Output octets
* Assigned IP address
* NAS identifier

The Network Access Server (NAS) is the device providing network access. In this environment, the MikroTik router acts as the NAS.

Accounting data is useful for:

* Subscriber usage reports
* Billing systems
* Session history
* Network capacity planning
* Troubleshooting
* Fraud detection

# 🗁 Configure the NAS Identity

RADIUS servers often identify network access devices using NAS attributes.

Set the MikroTik identity:

```routeros
/system identity
set name=ISP-PPPoE-Router
```

Configure the NAS identifier when required:

```routeros
/radius
set [find address=192.168.10.10] \
    called-id=ISP-PPPoE \
    realm=isp.example
```

The exact NAS attributes sent depend on the RouterOS service and configuration.

Ensure the RADIUS server's client definition matches the source IP address and expected NAS identity.

# 🗁 Configure RADIUS Server Firewall Access

The RADIUS server must accept authentication and accounting traffic from authorized MikroTik routers.

For a Linux server using UFW, an example is:

```bash
sudo ufw allow from 192.168.10.1 to any port 1812 proto udp
sudo ufw allow from 192.168.10.1 to any port 1813 proto udp
```

Replace the source address with the actual MikroTik RADIUS client address.

On the MikroTik router, permit outbound communication to the RADIUS server if the output firewall policy restricts it.

If the RADIUS server is behind a firewall, ensure return traffic is allowed.

Do not expose RADIUS ports to the public internet unnecessarily.

# 🗁 Configure Multiple RADIUS Servers

For larger ISP networks, multiple RADIUS servers can provide redundancy.

Example:

```routeros
/radius
add service=ppp \
    address=192.168.10.10 \
    secret=PrimaryRadiusSecret \
    comment="Primary RADIUS"

add service=ppp \
    address=192.168.10.11 \
    secret=SecondaryRadiusSecret \
    comment="Secondary RADIUS"
```

RouterOS can use multiple configured RADIUS servers according to its server selection and retry behavior.

Plan the server order, timeout, reachability, and failover behavior carefully.

A secondary server should have consistent subscriber data and authorization policies if it is expected to provide uninterrupted authentication service.

# 🗁 Understand RADIUS and Local PPP Secrets

MikroTik can authenticate PPP users using local `/ppp secret` entries or an external RADIUS server.

| Feature | Local PPP secrets | RADIUS |
| --- | --- | --- |
| User database | Stored on MikroTik | Stored on RADIUS server |
| Centralized management | Limited | Supported |
| Multiple routers | Requires synchronization | Centralized authentication |
| Accounting | Router-local information | Centralized accounting |
| Subscriber policies | Local profiles and settings | Centralized policies and attributes |
| Scalability | Suitable for smaller deployments | Suitable for larger deployments |

A key operational consideration is authentication fallback.

When RADIUS is enabled, local PPP secret entries are not a universal fallback for every RADIUS failure scenario. Test the intended behavior and design an explicit recovery procedure before relying on local accounts for emergency access.

# 🗁 Monitor RADIUS Configuration

Display configured RADIUS servers:

```routeros
/radius print detail
```

Inspect PPP authentication settings:

```routeros
/ppp aaa print
```

View active PPP sessions:

```routeros
/ppp active print detail
```

Inspect Hotspot sessions:

```routeros
/ip hotspot active print detail
```

Enable RADIUS debugging:

```routeros
/system logging
add topics=radius,debug action=memory
```

Inspect relevant log entries:

```routeros
/log print where topics~"radius"
```

Disable verbose debugging when troubleshooting is complete.

# 🗁 Troubleshoot RADIUS Integration

| Problem | Possible cause | What to check |
| --- | --- | --- |
| Authentication timeout | RADIUS server unreachable | Routing, firewall, and server status |
| Access-Reject | Invalid credentials or policy | Subscriber record and authorization rules |
| Invalid shared secret | Secret mismatch | MikroTik and server configuration |
| Accounting records missing | Accounting disabled or blocked | `/ppp aaa` and UDP 1813 |
| Subscriber receives wrong IP | Incorrect returned attributes | `Framed-IP-Address`, `Framed-Pool`, and local settings |
| Bandwidth limit not applied | Missing or invalid vendor attribute | `Mikrotik-Rate-Limit` |
| One router authenticates but another fails | Incorrect NAS client definition | Source IP and RADIUS client configuration |
| Intermittent authentication failures | Server overload or unstable connectivity | RADIUS logs, timeout, and server health |
| Hotspot authentication fails | RADIUS not enabled for Hotspot | Hotspot profile and RADIUS service |
| Secondary server does not authenticate users | Inconsistent subscriber data | Server synchronization and policies |

Useful checks:

```routeros
/ping 192.168.10.10
```

```routeros
/radius print detail
```

```routeros
/ppp aaa print
```

```routeros
/log print where topics~"radius"
```

On a Linux RADIUS server, inspect the service status:

```bash
sudo systemctl status freeradius
```

Follow server logs:

```bash
sudo journalctl -u freeradius -f
```

The service name may differ depending on the Linux distribution and package.

# 🗁 RADIUS Security Best Practices

* Use strong, unique shared secrets for each network access device.
* Restrict RADIUS traffic to authorized router addresses.
* Keep the RADIUS server on a protected management network.
* Use firewall rules to permit only required authentication and accounting traffic.
* Protect subscriber databases and accounting records.
* Use secure transport or protected network tunnels when RADIUS traffic crosses untrusted networks.
* Monitor authentication failures and unusual account activity.
* Synchronize subscriber policies across redundant RADIUS servers.
* Back up RADIUS configuration and subscriber databases.
* Avoid exposing RADIUS services directly to the public internet.

Traditional RADIUS over UDP does not provide end-to-end encryption for all attributes. For sensitive or untrusted network paths, use an appropriately secured transport design.

# 🗁 Practical Lab

Build a centralized RADIUS authentication environment using MikroTik RouterOS v7 and a Linux FreeRADIUS server.

**Topology**

```text
                  Internet
                     |
               +-----------+
               | MikroTik  |
               | PPPoE NAS |
               +-----------+
                     |
                Subscribers

                     |
              RADIUS Requests
                     |
               +-----------+
               | Ubuntu    |
               | FreeRADIUS|
               +-----------+
                     |
              Subscriber Database
```

**Network configuration**

| Component | Address |
| --- | --- |
| MikroTik router | 192.168.10.1 |
| FreeRADIUS server | 192.168.10.10 |
| Authentication | UDP 1812 |
| Accounting | UDP 1813 |

**Configuration requirements**

* Install and configure FreeRADIUS.
* Register the MikroTik router as a RADIUS client.
* Configure a shared secret.
* Add a test subscriber to the RADIUS database.
* Configure the RADIUS server on MikroTik.
* Enable PPP RADIUS authentication.
* Enable PPP accounting.
* Configure a PPPoE server.
* Connect a test PPPoE client using the RADIUS subscriber credentials.

**Verification tasks**

* Confirm that the MikroTik router can reach the RADIUS server.
* Authenticate a valid subscriber.
* Attempt authentication with an invalid password.
* Confirm that the RADIUS server returns the appropriate decision.
* Inspect the active PPP session.
* Verify the assigned IP address.
* Test a bandwidth attribute if configured.
* Inspect accounting start and interim records.
* Disconnect the subscriber.
* Verify that an accounting stop record is received.
* Test the behavior when the primary RADIUS server becomes unavailable.

# 🗁 Key Takeaways

* RADIUS provides centralized authentication, authorization, and accounting.
* MikroTik RouterOS acts as a RADIUS client or Network Access Server.
* Modern RADIUS deployments commonly use UDP 1812 for authentication and UDP 1813 for accounting.
* A shared secret authenticates communication between the MikroTik router and RADIUS server.
* PPPoE and Hotspot can use RADIUS for subscriber authentication.
* RADIUS attributes can control IP assignment, bandwidth, and session behavior.
* Accounting records provide subscriber session and usage information.
* Multiple RADIUS servers can improve service availability.
* Proper firewall restrictions and secure network transport are important for RADIUS deployments.
