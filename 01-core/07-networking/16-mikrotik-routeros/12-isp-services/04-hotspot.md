# MikroTik Hotspot

MikroTik Hotspot is a captive portal feature in RouterOS that controls network access through a web-based authentication system.

When a user connects to a Hotspot network, the router redirects unauthenticated traffic to a login page. The user must authenticate before receiving normal network access.

Hotspot is commonly used in:

* Hotels and guest houses
* Restaurants and cafés
* Schools and universities
* Shopping malls
* Airports and public Wi-Fi networks
* ISP wireless access networks
* Residential and commercial guest networks

RouterOS provides Hotspot management through `/ip hotspot`.

# 🗁 What Is a Hotspot?

A Hotspot is a network access-control system that requires users to authenticate before accessing permitted network resources.

Unlike PPPoE, which establishes a point-to-point session using a dedicated PPP client, Hotspot commonly authenticates users through a web browser.

A typical Hotspot network looks like this:

```text
                 Internet
                    |
              +-----------+
              | MikroTik  |
              |  Router   |
              +-----------+
                    |
               Hotspot LAN
                    |
              +-----------+
              | Access    |
              | Point     |
              +-----------+
                    |
              +-----------+
              | User      |
              | Device    |
              +-----------+
                    |
              Hotspot Login
                    |
              Internet Access
```

The MikroTik router acts as the Hotspot gateway and authentication server.

# 🗁 How Hotspot Authentication Works

The general authentication process is:

* A user connects to the Hotspot network.
* The device receives an IP address through DHCP.
* The user attempts to access a website.
* The router detects that the user is not authenticated.
* The router redirects the browser to the Hotspot login page.
* The user submits authentication credentials.
* RouterOS validates the credentials.
* If authentication succeeds, the user receives permitted network access.

The login page may use:

* Username and password
* Username-only authentication
* Trial access
* RADIUS authentication
* Other supported Hotspot authentication methods

# 🗁 Hotspot Components

| Component | Responsibility |
| --- | --- |
| Hotspot server | Controls access on a selected interface |
| Hotspot profile | Defines login, DNS, and authentication settings |
| Hotspot user | Stores local user credentials and limits |
| IP pool | Provides addresses to connected clients |
| DHCP server | Assigns IP configuration to clients |
| Login page | Presents the authentication interface |
| RADIUS | Provides centralized authentication and accounting |
| Walled garden | Allows selected destinations before authentication |

# 🗁 Hotspot Network Planning

Before configuring Hotspot, define the network topology.

Example:

| Component | Configuration |
| --- | --- |
| WAN interface | ether1 |
| Hotspot interface | bridge-hotspot |
| Hotspot gateway | 192.168.20.1 |
| Hotspot subnet | 192.168.20.0/24 |
| DHCP range | 192.168.20.10-192.168.20.254 |
| DNS server | 192.168.20.1 |
| Hotspot profile | hotspot-profile |
| Hotspot server | hotspot1 |

The Hotspot interface should be dedicated to the intended client network.

Avoid placing untrusted Hotspot users on the same unrestricted Layer 2 network as router management devices.

# 🗁 Prepare the Hotspot Interface

Create a bridge for Hotspot clients:

```routeros
/interface bridge
add name=bridge-hotspot
```

Add the client-facing Ethernet interface:

```routeros
/interface bridge port
add bridge=bridge-hotspot interface=ether2
```

If wireless access points are used, connect them to the Hotspot client network through an appropriate Ethernet or VLAN configuration.

Assign an IP address to the Hotspot bridge:

```routeros
/ip address
add address=192.168.20.1/24 interface=bridge-hotspot
```

The router will use `192.168.20.1` as the gateway for Hotspot clients.

# 🗁 Configure an IP Pool

Create an address pool for Hotspot clients:

```routeros
/ip pool
add name=hotspot-pool ranges=192.168.20.10-192.168.20.254
```

The pool provides addresses for devices connecting to the Hotspot network.

Verify the pool:

```routeros
/ip pool print
```

# 🗁 Configure DHCP Server

Hotspot clients need IP configuration before they can access the login page.

Create a DHCP server:

```routeros
/ip dhcp-server
add name=dhcp-hotspot \
    interface=bridge-hotspot \
    address-pool=hotspot-pool \
    lease-time=1h \
    disabled=no
```

Create the DHCP network configuration:

```routeros
/ip dhcp-server network
add address=192.168.20.0/24 \
    gateway=192.168.20.1 \
    dns-server=192.168.20.1
```

The DHCP server assigns addresses and gateway information to connected devices.

If a DHCP server already exists on the interface, do not create a duplicate server.

# 🗁 Configure DNS

Enable DNS requests from the Hotspot client network:

```routeros
/ip dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

The router can now provide DNS resolution to clients using its own address.

Make sure firewall rules prevent DNS access from untrusted WAN networks.

# 🗁 Create a Hotspot Server

RouterOS provides a Hotspot setup wizard.

Run:

```routeros
/ip hotspot setup
```

The wizard asks for several configuration values:

| Prompt | Example |
| --- | --- |
| Hotspot interface | bridge-hotspot |
| Local address of network | 192.168.20.1/24 |
| Address pool | 192.168.20.10-192.168.20.254 |
| Select certificate | none |
| IP address of SMTP server | 0.0.0.0 |
| DNS servers | 1.1.1.1,8.8.8.8 |
| DNS name | login.example.local |
| Local Hotspot user | hotspotadmin |
| Password | Strong password |

The wizard creates the required Hotspot configuration.

Use an appropriate DNS name that clients can resolve through the Hotspot network. For production environments, plan certificate and domain configuration if secure HTTPS login is required.

Inspect the generated configuration:

```routeros
/ip hotspot print detail
```

# 🗁 Configure a Hotspot Server Manually

You can create a Hotspot server without using the setup wizard.

Create a Hotspot profile:

```routeros
/ip hotspot profile
add name=hotspot-profile \
    hotspot-address=192.168.20.1 \
    dns-name=login.example.local \
    html-directory=hotspot \
    login-by=http-chap
```

Create the Hotspot server:

```routeros
/ip hotspot
add name=hotspot1 \
    interface=bridge-hotspot \
    address-pool=hotspot-pool \
    profile=hotspot-profile \
    disabled=no
```

The server uses the specified profile and client address pool.

If the wizard has already created a Hotspot server and profile, inspect and modify those objects instead of creating duplicate configurations.

# 🗁 Understand Hotspot Login Methods

RouterOS supports several Hotspot authentication methods.

| Method | Description |
| --- | --- |
| `http-chap` | Uses HTTP Challenge-Handshake Authentication Protocol |
| `http-pap` | Sends the password through HTTP without CHAP protection |
| `https` | Provides HTTPS-based Hotspot login when certificates are configured |
| `cookie` | Allows a previously authenticated browser to reconnect using a cookie |
| `mac` | Authenticates users using their MAC address |
| `mac-cookie` | Uses a MAC-based cookie for subsequent authentication |
| `trial` | Allows limited trial access |

Configure the supported methods through the Hotspot profile.

Example:

```routeros
/ip hotspot profile
set hotspot-profile login-by=http-chap,cookie
```

For production networks, use a properly configured HTTPS login page and certificate where browser-based credential protection is required.

Do not treat HTTP PAP or an ordinary HTTP login page as secure credential transport.

# 🗁 Create a Hotspot User

Create a local Hotspot user:

```routeros
/ip hotspot user
add name=user001 \
    password=StrongPassword001 \
    server=hotspot1
```

The user can authenticate through the Hotspot login page.

Create additional users:

```routeros
/ip hotspot user
add name=user002 password=StrongPassword002 server=hotspot1
add name=user003 password=StrongPassword003 server=hotspot1
```

View configured users:

```routeros
/ip hotspot user print
```

Display detailed user configuration:

```routeros
/ip hotspot user print detail
```

# 🗁 Configure User Bandwidth Limits

Hotspot users can have individual bandwidth limits.

Example:

```routeros
/ip hotspot user
set [find name=user001] rate-limit=5M/10M
```

The rate-limit format is:

```text
upload/download
```

From the router's perspective, the first value represents the user's upload limit and the second value represents the download limit.

Example:

```routeros
/ip hotspot user
add name=basic-user \
    password=StrongPassword003 \
    server=hotspot1 \
    rate-limit=2M/5M
```

This configuration limits the user's upload to 2 Mbps and download to 5 Mbps.

# 🗁 Configure Session Time Limits

Hotspot users can be restricted by session duration.

Example:

```routeros
/ip hotspot user
set [find name=user001] limit-uptime=2h
```

The user is limited to two hours of total uptime.

You can also configure simultaneous-session restrictions:

```routeros
/ip hotspot user
set [find name=user001] shared-users=1
```

This limits the account to one simultaneous Hotspot session.

Other useful user properties include:

| Property | Purpose |
| --- | --- |
| `limit-uptime` | Limits total session uptime |
| `limit-bytes-in` | Limits incoming data volume |
| `limit-bytes-out` | Limits outgoing data volume |
| `limit-bytes-total` | Limits combined data volume |
| `shared-users` | Controls simultaneous sessions |
| `rate-limit` | Defines bandwidth limitations |
| `server` | Associates the user with a specific Hotspot server |

# 🗁 Configure Hotspot User Profiles

Hotspot user profiles allow administrators to define reusable settings for groups of Hotspot users.

Inspect existing profiles:

```routeros
/ip hotspot user profile print
```

Create a profile:

```routeros
/ip hotspot user profile
add name=basic-users \
    shared-users=1 \
    rate-limit=2M/5M \
    idle-timeout=5m \
    keepalive-timeout=2m \
    status-autorefresh=1m
```

Assign the profile to a user:

```routeros
/ip hotspot user
set [find name=user001] profile=basic-users
```

Create another profile for premium users:

```routeros
/ip hotspot user profile
add name=premium-users \
    shared-users=2 \
    rate-limit=10M/25M \
    idle-timeout=10m
```

Assign it:

```routeros
/ip hotspot user
set [find name=user002] profile=premium-users
```

User profiles make it easier to manage service packages without configuring every user independently.

# 🗁 Understand the Hotspot Login Page

RouterOS provides default Hotspot HTML files.

These files are stored in the configured Hotspot HTML directory, commonly named `hotspot`.

The login page typically includes:

* Username field
* Password field
* Login button
* Status information
* Error messages
* Redirect information

Inspect the Hotspot profile:

```routeros
/ip hotspot profile print detail
```

The `html-directory` property identifies the directory containing the login page files.

Administrators can customize the login page to include:

* ISP or business branding
* Terms and conditions
* Customer support information
* Service package information
* Promotional messages
* Login instructions

Keep the original login variables and authentication form behavior intact when modifying the HTML files.

# 🗁 Understand the Walled Garden

The Hotspot walled garden allows selected destinations to be accessed before authentication.

This is useful when users need access to certain resources before logging in.

Examples include:

* Payment portals
* Customer registration pages
* ISP websites
* Terms and conditions pages
* Captive portal support resources

Add a permitted HTTP destination:

```routeros
/ip hotspot walled-garden
add dst-host=example.com action=allow
```

Inspect configured entries:

```routeros
/ip hotspot walled-garden print
```

For destinations that require IP-level or non-HTTP access, use the appropriate walled-garden IP configuration and firewall policy.

Only permit destinations that are genuinely required before authentication.

# 🗁 Understand Hotspot Active Sessions

RouterOS maintains information about authenticated Hotspot users.

Display active sessions:

```routeros
/ip hotspot active print
```

Display detailed session information:

```routeros
/ip hotspot active print detail
```

Administrators can inspect:

* Username
* Client IP address
* Client MAC address
* Login time
* Session uptime
* Traffic counters

Disconnect an active user:

```routeros
/ip hotspot active
remove [find user=user001]
```

The user is disconnected, but the account remains configured.

If the device reconnects and the account is still valid, it can authenticate again.

# 🗁 Monitor Hotspot Hosts

The Hotspot host table displays devices detected by the Hotspot system.

Inspect hosts:

```routeros
/ip hotspot host print
```

The host table helps administrators identify connected devices and their authentication state.

It can be useful when investigating:

* Devices that have connected but not authenticated
* Unexpected client IP addresses
* MAC address changes
* Duplicate or stale client entries
* Captive portal connectivity problems

# 🗁 Configure NAT for Hotspot Users

If the router provides internet access through a WAN interface, Hotspot client traffic usually requires source NAT.

Example:

```routeros
/ip firewall nat
add chain=srcnat \
    src-address=192.168.20.0/24 \
    out-interface=ether1 \
    action=masquerade \
    comment="Hotspot internet access"
```

Replace `ether1` with the actual WAN interface.

The firewall filter rules must also permit the intended client traffic.

NAT translates source addresses; it does not independently authorize traffic through the firewall.

If the router already has an appropriate masquerade rule covering the Hotspot subnet, avoid adding a redundant rule.

# 🗁 Understand Hotspot Firewall Behavior

Hotspot relies on RouterOS firewall and connection-tracking mechanisms to control unauthenticated traffic.

Important considerations:

* The Hotspot interface must be reachable by client devices.
* DHCP must provide valid network settings.
* DNS resolution must work for the intended login process.
* Unauthenticated traffic must be redirected or restricted appropriately.
* Authenticated traffic must be permitted by the forwarding policy.
* Router management services must not be exposed to ordinary Hotspot clients.

Review existing firewall rules:

```routeros
/ip firewall filter print detail
```

Review NAT rules:

```routeros
/ip firewall nat print detail
```

Avoid replacing an existing production firewall policy with generic Hotspot rules without understanding the current traffic flow and rule order.

# 🗁 Configure Trial Access

Trial access allows users to access a Hotspot network for a limited period without creating a conventional username and password account.

Enable trial authentication in the Hotspot profile:

```routeros
/ip hotspot profile
set hotspot-profile login-by=http-chap,trial
```

Configure trial parameters:

```routeros
/ip hotspot profile
set hotspot-profile trial-uptime-limit=15m
```

This permits trial access for up to 15 minutes, subject to the profile's trial configuration and tracking behavior.

Trial access is useful for cafés, hotels, and public Wi-Fi environments where visitors need temporary access.

# 🗁 Configure RADIUS Authentication

Hotspot can use a RADIUS server for centralized authentication and accounting.

Enable RADIUS for Hotspot:

```routeros
/radius
add service=hotspot \
    address=192.168.10.10 \
    secret=RadiusSharedSecret
```

Enable RADIUS authentication in the Hotspot profile:

```routeros
/ip hotspot profile
set hotspot-profile use-radius=yes
```

The RADIUS server can authenticate users and return supported authorization attributes.

For production environments, configure the correct RADIUS source address, shared secret, reachability, and accounting behavior.

RADIUS integration is covered in detail in the dedicated ISP RADIUS documentation.

# 🗁 Troubleshooting Hotspot

| Problem | Possible cause | What to check |
| --- | --- | --- |
| Login page does not appear | DNS, DHCP, or redirect issue | Client IP settings and Hotspot server |
| User cannot authenticate | Incorrect credentials | Hotspot user or RADIUS configuration |
| Login succeeds but internet does not work | NAT or firewall issue | Forward chain and source NAT |
| Client does not receive an IP address | DHCP problem | DHCP server and address pool |
| Login page shows certificate errors | Invalid or untrusted certificate | HTTPS certificate configuration |
| User is disconnected frequently | Session or idle timeout | User and profile settings |
| Trial login is unavailable | Trial authentication disabled | Hotspot profile |
| Some websites work before login | Walled garden permissions | Walled-garden entries |
| Users cannot access the router login page | DNS name or Hotspot configuration | DNS and profile settings |
| Multiple users share one account | Shared-user limit too high | User `shared-users` property |

Inspect the Hotspot server:

```routeros
/ip hotspot print detail
```

Inspect Hotspot profiles:

```routeros
/ip hotspot profile print detail
```

Inspect configured users:

```routeros
/ip hotspot user print detail
```

Inspect active sessions:

```routeros
/ip hotspot active print detail
```

Inspect DHCP leases:

```routeros
/ip dhcp-server lease print
```

Inspect router logs:

```routeros
/log print
```

# 🗁 Security Best Practices

* Use a dedicated VLAN or isolated bridge for Hotspot clients.
* Protect router management services from guest users.
* Use HTTPS with a valid certificate for credential-based login.
* Avoid transmitting passwords through unencrypted HTTP.
* Use strong passwords for administrative and subscriber accounts.
* Apply appropriate bandwidth and session limits.
* Restrict pre-authentication access through the walled garden.
* Monitor active sessions and suspicious traffic.
* Keep RouterOS and Hotspot page files updated.
* Use RADIUS for centralized authentication when managing many users.
* Keep backups of customized login pages and router configuration.

# 🗁 Practical Lab

Build a basic MikroTik Hotspot environment using RouterOS v7.

**Topology**

```text
                 Internet
                    |
                ether1
                    |
              MikroTik Router
                    |
             bridge-hotspot
                    |
                 ether2
                    |
              Wireless AP
                    |
              Client Devices
                    |
              Hotspot Login
```

**Configuration requirements**

* Configure `bridge-hotspot`.
* Assign `192.168.20.1/24` to the bridge.
* Create `hotspot-pool`.
* Configure a DHCP server.
* Configure DNS.
* Create a Hotspot server and profile.
* Create two Hotspot users.
* Configure bandwidth limits.
* Configure a session timeout.
* Configure source NAT for internet access.

**Verification tasks**

* Connect a client device to the Hotspot network.
* Confirm that the client receives a DHCP address.
* Open a website and verify that the login page appears.
* Authenticate using the first user account.
* Confirm internet connectivity.
* Test the bandwidth limit.
* Connect a second device and test the shared-user restriction.
* Disconnect an active user from RouterOS.
* Test trial access if configured.
* Inspect the active sessions and host table.
* Verify that guest clients cannot access router management services.

# 🗁 Key Takeaways

* MikroTik Hotspot provides captive portal authentication for network access.
* Clients receive IP configuration through DHCP before authentication.
* Hotspot servers operate on selected RouterOS interfaces.
* Hotspot profiles define authentication and login behavior.
* Hotspot users can have individual bandwidth and session limits.
* User profiles provide reusable policies for groups of users.
* Walled garden rules permit selected resources before authentication.
* Active sessions and connected hosts can be monitored through RouterOS.
* RADIUS enables centralized authentication and accounting.
* Network isolation and firewall policies are essential for secure public Wi-Fi deployments.
