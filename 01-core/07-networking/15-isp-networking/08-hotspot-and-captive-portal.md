# Hotspot and Captive Portal

A network Hotspot is an access-control system that allows users to connect to a network and access internet services after completing an authentication process.

A Captive Portal is a web-based interface that intercepts or redirects a user's initial web access request to a login page, terms-and-conditions page, payment page, or access authorization page.

Internet Service Providers (ISPs), hotels, restaurants, educational institutions, airports, and public Wi-Fi operators commonly use Hotspot systems to manage network access.

In ISP environments, Hotspot services can provide controlled internet access without requiring every customer to configure a PPPoE client.

MikroTik RouterOS includes a built-in Hotspot feature that supports user authentication, access control, session management, and bandwidth policies.

# 🗁 How Hotspot Works

A typical Hotspot connection follows this process:

* A user connects a device to the access network.
* The device receives an IP address through DHCP.
* The user attempts to access an internet resource.
* The Hotspot system identifies that the user is not authenticated.
* The system redirects the user's web request to the captive portal.
* The user enters credentials or completes the required authorization process.
* The Hotspot server validates the user.
* The system grants access according to the assigned policies.
* The user can access permitted network services.

# 🗁 Hotspot Network Architecture

A typical ISP Hotspot architecture contains the following components:

| Component | Responsibility |
| --- | --- |
| User Device | Connects to the Hotspot network |
| Access Point | Provides wireless connectivity |
| Access Switch | Connects access points and network infrastructure |
| DHCP Server | Assigns IP configuration to users |
| Hotspot Gateway | Controls user access and authentication |
| Captive Portal | Displays the login or authorization page |
| RADIUS Server | Provides centralized authentication and accounting |
| ISP Router | Provides routing and internet connectivity |

Example topology:

```text
             INTERNET
                 |
                 |
           ISP Gateway
                 |
                 |
          Hotspot Gateway
           MikroTik Router
                 |
                 |
            Access Switch
                 |
          +------+------+
          |             |
          |             |
       Access Point  Access Point
          |             |
          |             |
       Users          Users
```

The Hotspot gateway may be a dedicated router or a logical interface on an existing ISP router.

# 🗁 Hotspot vs PPPoE

Hotspot and PPPoE are two different subscriber access methods.

| Feature | Hotspot | PPPoE |
| --- | --- | --- |
| Authentication interface | Web-based portal | PPP username and password |
| Connection establishment | IP connectivity followed by portal authentication | PPPoE discovery and PPP negotiation |
| Client configuration | Usually no special client configuration | Requires PPPoE client support |
| IP assignment | Commonly DHCP | Commonly IPCP for IPv4 |
| Access control | Captive portal and gateway policies | PPP session policies |
| User experience | Login through a browser | Login through router or operating system |
| Typical deployment | Public Wi-Fi and shared access | Residential broadband |
| RADIUS integration | Supported | Supported |

Both systems can integrate with RADIUS for centralized subscriber management.

# 🗁 Captive Portal Components

A Captive Portal typically includes the following elements:

**Login Page**

The login page allows users to submit their credentials.

It may contain:

* Username and password fields.
* Terms and conditions.
* Service information.
* Usage policies.
* Payment or voucher instructions.
* Support contact information.

**Authentication System**

The authentication system validates user credentials.

Depending on the implementation, authentication may use:

* Local Hotspot accounts.
* RADIUS authentication.
* Voucher-based access.
* External authentication services.
* Other supported authorization mechanisms.

**Session Management**

After successful authentication, the gateway maintains the user's access session.

Session policies may include:

* Session timeout.
* Idle timeout.
* Data transfer limits.
* Bandwidth restrictions.
* Concurrent session limits.

**Redirection System**

The captive portal redirects unauthenticated users to the login page when they attempt to access supported web resources.

Modern operating systems also use captive-network detection mechanisms to determine whether a network requires login.

# 🗁 MikroTik Hotspot Configuration

This section demonstrates a basic Hotspot deployment using MikroTik RouterOS v7.

Example network parameters:

| Parameter | Value |
| --- | --- |
| Hotspot Interface | `bridge-hotspot` |
| Hotspot Network | `192.168.50.0/24` |
| Gateway Address | `192.168.50.1` |
| DHCP Pool | `192.168.50.10-192.168.50.250` |
| DNS Server | `1.1.1.1` |
| Hotspot Server Name | `hotspot1` |
| Hotspot Profile | `hsprof1` |
| Test Username | `user001` |
| Test Password | `StrongPassword123` |

These values are examples. The bridge and its member interfaces should be configured according to the actual network topology.

# 🗁 Creating the Hotspot Interface

Create a bridge for the Hotspot access network:

```routeros
/interface bridge add \
    name=bridge-hotspot
```

Add the required access interface to the bridge:

```routeros
/interface bridge port add \
    bridge=bridge-hotspot \
    interface=ether2
```

Verify the bridge:

```routeros
/interface bridge print
```

If the access network already uses a bridge or VLAN interface, use the appropriate existing logical interface instead of creating a duplicate bridge.

# 🗁 Assigning an IP Address

Assign the gateway IP address to the Hotspot interface:

```routeros
/ip address add \
    address=192.168.50.1/24 \
    interface=bridge-hotspot
```

This address becomes the default gateway for Hotspot users.

Verify the configuration:

```routeros
/ip address print
```

# 🗁 Creating a DHCP Pool

Create a pool for dynamically assigning addresses:

```routeros
/ip pool add \
    name=hotspot-pool \
    ranges=192.168.50.10-192.168.50.250
```

The gateway address `192.168.50.1` is excluded from the pool.

# 🗁 Configuring DHCP Server

Create a DHCP server:

```routeros
/ip dhcp-server add \
    name=hotspot-dhcp \
    interface=bridge-hotspot \
    address-pool=hotspot-pool \
    disabled=no
```

Configure the network parameters:

```routeros
/ip dhcp-server network add \
    address=192.168.50.0/24 \
    gateway=192.168.50.1 \
    dns-server=192.168.50.1
```

Enable DNS requests on the router:

```routeros
/ip dns set \
    servers=1.1.1.1,8.8.8.8 \
    allow-remote-requests=yes
```

The router can now provide DNS resolution to Hotspot clients.

Ensure that firewall rules restrict DNS access to the intended client networks.

# 🗁 Configuring Hotspot with Setup Wizard

RouterOS provides a Hotspot Setup wizard that automatically creates the basic Hotspot configuration.

Run:

```routeros
/ip hotspot setup
```

The wizard asks for several parameters:

| Parameter | Example |
| --- | --- |
| Hotspot Interface | `bridge-hotspot` |
| Local Address | `192.168.50.1/24` |
| Address Pool | `192.168.50.10-192.168.50.250` |
| Certificate | `none` for a basic lab |
| SMTP Server | `0.0.0.0` |
| DNS Servers | `1.1.1.1,8.8.8.8` |
| DNS Name | `login.example.com` |
| Hotspot Username | `admin` |
| Hotspot Password | A strong password |

The wizard may create or modify IP addressing, DHCP, firewall, DNS, and Hotspot settings.

Review the existing configuration before running it to avoid conflicting settings.

# 🗁 Creating a Hotspot User

Create a local Hotspot user:

```routeros
/ip hotspot user add \
    name=user001 \
    password=StrongPassword123 \
    server=hotspot1
```

The user can authenticate through the captive portal using these credentials.

Verify configured users:

```routeros
/ip hotspot user print
```

# 🗁 Creating Hotspot User Profiles

A Hotspot user profile defines common policies for a group of users.

Create a profile:

```routeros
/ip hotspot user profile add \
    name=basic-profile \
    rate-limit=5M/5M \
    shared-users=1 \
    idle-timeout=10m \
    keepalive-timeout=2m
```

Important parameters:

| Parameter | Description |
| --- | --- |
| `name` | Profile name |
| `rate-limit` | Upload and download bandwidth limit |
| `shared-users` | Maximum simultaneous users allowed for the account |
| `idle-timeout` | Disconnects sessions after the configured idle period |
| `keepalive-timeout` | Controls how long an unresponsive session is retained |

Assign the profile to a user:

```routeros
/ip hotspot user set \
    [find name=user001] \
    profile=basic-profile
```

# 🗁 Creating Different Hotspot Packages

An ISP or Wi-Fi operator may offer multiple access packages.

Example packages:

| Package | Upload | Download | Session Policy |
| --- | ---: | ---: | --- |
| Basic | 2 Mbps | 2 Mbps | Standard |
| Standard | 5 Mbps | 5 Mbps | Standard |
| Premium | 10 Mbps | 10 Mbps | Standard |

Create profiles:

```routeros
/ip hotspot user profile add \
    name=basic-2m \
    rate-limit=2M/2M \
    shared-users=1

/ip hotspot user profile add \
    name=standard-5m \
    rate-limit=5M/5M \
    shared-users=1

/ip hotspot user profile add \
    name=premium-10m \
    rate-limit=10M/10M \
    shared-users=1
```

Assign a package:

```routeros
/ip hotspot user set \
    [find name=user001] \
    profile=standard-5m
```

The profile controls the user's bandwidth and other configured session policies.

# 🗁 Hotspot Login Page

RouterOS stores Hotspot web files in the configured Hotspot HTML directory.

The default directory is commonly named `hotspot`.

Inspect the available files:

```routeros
/file print
```

The login page is generally implemented through:

```text
login.html
```

Other files may include:

```text
logout.html
status.html
error.html
redirect.html
alogin.html
```

The exact files depend on the RouterOS version and Hotspot configuration.

Administrators can customize the portal interface by editing these files.

A custom login page may contain:

* ISP or business branding.
* Username and password fields.
* Package descriptions.
* Terms and conditions.
* Customer support information.
* Payment instructions.

When modifying Hotspot files, preserve the RouterOS-provided login variables and authentication workflow. Test customized pages before deploying them to production.

# 🗁 Captive Portal Redirection

When an unauthenticated user accesses a supported web resource, the Hotspot gateway can redirect the request to its login page.

Example:

```text
User Device
     |
     | HTTP Request
     v
Hotspot Gateway
     |
     | User Not Authenticated
     v
Captive Portal
     |
     | Login Page
     v
User Authentication
     |
     | Successful Login
     v
Internet Access
```

Captive portal redirection has important limitations.

HTTPS traffic cannot be transparently redirected in the same way as ordinary HTTP traffic without causing certificate or security problems.

Modern operating systems may also use dedicated captive-network detection requests instead of relying on users opening an ordinary website.

# 🗁 HTTPS and Captive Portal Considerations

HTTPS protects communication between a client and a destination server.

A Hotspot gateway should not attempt to impersonate arbitrary HTTPS websites to display its login page.

A better deployment approach is:

* Provide a valid HTTPS certificate for the portal domain.
* Configure the Hotspot DNS name correctly.
* Ensure the portal hostname resolves to the intended gateway.
* Configure the required certificate and HTTPS login settings.
* Allow necessary portal access before authentication.
* Test captive-network detection on common mobile and desktop operating systems.

A certificate mismatch can cause browser security errors and prevent users from trusting the login page.

# 🗁 Walled Garden

A Walled Garden allows unauthenticated users to access specific destinations or services before completing Hotspot authentication.

Common examples include:

* ISP payment portals.
* Customer support websites.
* Authentication services.
* Terms and conditions pages.
* Voucher purchase pages.

In MikroTik RouterOS, Walled Garden rules can be configured using:

```routeros
/ip hotspot walled-garden
```

For example, allow access to a specific domain:

```routeros
/ip hotspot walled-garden add \
    dst-host=payment.example.com \
    action=allow
```

For IP-based access rules:

```routeros
/ip hotspot walled-garden ip
```

The exact access requirements depend on the services hosted outside the Hotspot network.

Avoid creating overly broad Walled Garden rules because they can unintentionally allow unauthenticated access to additional resources.

# 🗁 Hotspot Active Sessions

RouterOS provides commands to inspect authenticated Hotspot users.

Display active sessions:

```routeros
/ip hotspot active print
```

Display detailed session information:

```routeros
/ip hotspot active print detail
```

Information may include:

* Username.
* Client IP address.
* Client MAC address.
* Session uptime.
* Session start time.
* Server name.
* Data transfer counters.

Disconnect a specific user:

```routeros
/ip hotspot active remove \
    [find user=user001]
```

This removes the active session. If the account remains enabled, the user can authenticate again.

Disable an account:

```routeros
/ip hotspot user disable \
    [find name=user001]
```

Enable the account again:

```routeros
/ip hotspot user enable \
    [find name=user001]
```

# 🗁 Session Time and Data Limits

Hotspot accounts can have session restrictions.

For example, create a user with a maximum session duration:

```routeros
/ip hotspot user add \
    name=temporary-user \
    password=TemporaryPassword123 \
    limit-uptime=2h
```

A data transfer limit can also be configured:

```routeros
/ip hotspot user set \
    [find name=temporary-user] \
    limit-bytes-total=1073741824
```

The configured limit is 1 GiB of total data transfer.

These restrictions are useful for temporary access, prepaid Wi-Fi packages, and voucher-based services.

# 🗁 RADIUS Integration

For larger Hotspot deployments, centralized authentication and accounting are useful.

RADIUS can manage user credentials and subscriber policies from a central server.

Configure a RADIUS server:

```routeros
/radius add \
    service=hotspot \
    address=10.20.20.10 \
    secret=RadiusSharedSecret \
    authentication-port=1812 \
    accounting-port=1813
```

Enable RADIUS authentication for Hotspot:

```routeros
/ip hotspot profile set \
    [find name=hsprof1] \
    use-radius=yes
```

Enable accounting:

```routeros
/ppp aaa set \
    accounting=yes
```

The RADIUS server must be configured with the MikroTik router as a recognized client.

It can provide centralized:

* User authentication.
* Access authorization.
* Session accounting.
* Bandwidth policies.
* Session timeout attributes.
* Data usage records.

Hotspot-specific accounting and policy attributes depend on the RADIUS implementation and RouterOS support.

# 🗁 Hotspot Firewall and Security

A Hotspot network should be isolated from internal management and infrastructure networks.

Recommended practices include:

* Separate Hotspot users from management networks.
* Restrict access to router management services.
* Use strong user credentials.
* Protect the captive portal with HTTPS.
* Prevent unauthorized access to internal resources.
* Apply appropriate bandwidth limits.
* Monitor active sessions and authentication failures.
* Use RADIUS for centralized management where appropriate.
* Keep RouterOS and Hotspot web files updated.
* Apply firewall rules that restrict access to the router itself.

Remember that Hotspot authentication is an access-control mechanism, not a replacement for complete network security.

# 🗁 Hotspot Troubleshooting

| Problem | Possible Cause | Troubleshooting |
| --- | --- | --- |
| User cannot receive an IP address | DHCP configuration problem | Check DHCP server, pool, and bridge interface |
| Login page does not appear | Hotspot server or redirection issue | Check Hotspot server status and client connectivity |
| Login page appears but login fails | Invalid credentials | Verify the account and password |
| User logs in but cannot access the internet | Routing, NAT, or firewall issue | Check default route, NAT, and firewall policies |
| HTTPS websites show certificate errors | Incorrect portal certificate or redirection behavior | Verify HTTPS configuration and certificate validity |
| User disconnects frequently | Idle timeout or unstable connection | Check session timers and wireless connectivity |
| RADIUS authentication fails | RADIUS connectivity or shared-secret mismatch | Verify RADIUS client settings and server logs |
| User exceeds expected bandwidth | Incorrect profile or queue policy | Check the assigned user profile and rate limit |
| Some websites are inaccessible before login | Walled Garden configuration is incomplete | Verify allowed domains and required dependencies |

Useful diagnostic commands:

```routeros
/ip hotspot print detail

/ip hotspot profile print detail

/ip hotspot user print detail

/ip hotspot active print detail

/ip hotspot host print detail

/ip dhcp-server lease print

/log print where topics~"hotspot"
```

# 🗁 Practical Notes

* DHCP provides IP configuration before Hotspot authentication.
* The Hotspot gateway controls access to permitted network resources.
* A Captive Portal provides the user-facing authentication interface.
* Hotspot and PPPoE are different subscriber access methods.
* Walled Garden rules allow selected access before authentication.
* HTTPS requires proper certificate configuration.
* RADIUS provides centralized authentication and accounting.
* Session limits and bandwidth policies can be assigned through user profiles.
* A successful Hotspot login does not guarantee internet connectivity; routing, firewall, and upstream configuration must also be correct.

# 🗁 References

* [MikroTik RouterOS — HotSpot](https://help.mikrotik.com/docs/spaces/ROS/pages/56459266/HotSpot)
* [MikroTik RouterOS — RADIUS](https://help.mikrotik.com/docs/spaces/ROS/pages/328097/RADIUS)
* [RFC 2865 — Remote Authentication Dial In User Service (RADIUS)](https://www.rfc-editor.org/rfc/rfc2865)
* [RFC 8908 — Captive Portal API](https://www.rfc-editor.org/rfc/rfc8908)
* [RFC 8910 — Captive-Portal Identification in DHCP and Router Advertisements](https://www.rfc-editor.org/rfc/rfc8910)
