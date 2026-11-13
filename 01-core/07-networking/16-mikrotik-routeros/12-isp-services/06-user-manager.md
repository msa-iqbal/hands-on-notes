# MikroTik User Manager

MikroTik User Manager is a RADIUS server and subscriber management system designed to centralize authentication, authorization, and accounting (AAA).

It allows network administrators to manage users, service profiles, session limits, and usage information from a centralized system.

User Manager is particularly useful for MikroTik-based ISP networks that provide:

* PPPoE internet services
* Hotspot internet access
* Wireless authentication
* Subscriber bandwidth packages
* Prepaid internet access
* Time-based internet packages
* Usage-based subscriber accounts

Instead of creating every subscriber independently on multiple routers, administrators can manage subscriber accounts through User Manager and authenticate network access devices using RADIUS.

# 🗁 What Is User Manager?

User Manager is a MikroTik RouterOS package that provides RADIUS-based subscriber management.

It can manage authentication requests from compatible RouterOS services and maintain subscriber information centrally.

A typical ISP architecture looks like this:

```text
                  Internet
                     |
               +-----------+
               | Core      |
               | Router    |
               +-----------+
                     |
              +--------------+
              | MikroTik     |
              | PPPoE Server |
              +--------------+
                     |
                Subscribers

                     |
              RADIUS Requests
                     |
              +--------------+
              | User Manager |
              | RADIUS Server|
              +--------------+
                     |
             Subscriber Database
```

The PPPoE router sends authentication requests to User Manager. User Manager validates subscriber credentials and returns the applicable authorization information.

# 🗁 User Manager Features

User Manager provides several subscriber management capabilities.

| Feature | Description |
| --- | --- |
| User management | Create and manage subscriber accounts |
| RADIUS authentication | Authenticate network users centrally |
| Profiles | Define reusable service policies |
| Limitations | Control session time and usage |
| Accounting | Record session and traffic information |
| Payments | Manage prepaid account payments where configured |
| Sessions | View subscriber connection information |
| Reports | Inspect usage and account activity |

The available functionality depends on the RouterOS version, installed package, and User Manager configuration.

# 🗁 User Manager and RouterOS Versions

User Manager is available as a RouterOS package.

Its installation and configuration differ between RouterOS v6 and v7.

For RouterOS v7, use the User Manager package that matches the router's:

* RouterOS version
* CPU architecture
* Package release
* Device compatibility

Do not install a package intended for a different architecture or RouterOS release.

User Manager v7 uses a different management architecture from the older v6 implementation. Follow the documentation and configuration syntax for the installed version.

# 🗁 Check RouterOS Architecture and Version

Inspect the RouterOS version:

```routeros
/system resource print
```

Inspect the system architecture:

```routeros
/system resource print
```

Look for the following properties:

```text
version
architecture-name
board-name
```

Check installed packages:

```routeros
/system package print
```

This information helps identify the correct User Manager package.

# 🗁 Install User Manager on RouterOS v7

Download the matching User Manager package from the official MikroTik download page.

<Link url="https://mikrotik.com/download" title="MikroTik RouterOS Downloads"/>

Installation workflow:

* Identify the router architecture.
* Identify the installed RouterOS version.
* Download the matching extra packages archive.
* Extract the archive.
* Locate the User Manager package.
* Upload the package to the router's Files section.
* Reboot the router if required.
* Verify that the package is installed and enabled.

After uploading the package, reboot:

```routeros
/system reboot
```

Verify installed packages:

```routeros
/system package print
```

If the package is not listed, check the architecture, version compatibility, and upload location.

# 🗁 Verify User Manager Availability

After installation, inspect the available User Manager commands:

```routeros
/user-manager print
```

You can also inspect the package:

```routeros
/system package print
```

If the User Manager menu or commands are unavailable, verify that the package is installed, enabled, and compatible with the current RouterOS release.

# 🗁 Understand User Manager Components

User Manager uses several concepts to organize subscriber access.

| Component | Responsibility |
| --- | --- |
| Router | Defines a network access device that communicates with User Manager |
| User | Stores subscriber identity and authentication credentials |
| Profile | Defines reusable service settings |
| Limitation | Defines time, bandwidth, or usage restrictions |
| Session | Represents an authenticated network connection |
| Payment | Records prepaid account payment information where supported |
| Accounting | Stores usage and session records |

These components work together to provide centralized subscriber management.

# 🗁 Configure User Manager RADIUS Router

User Manager must recognize the MikroTik router that sends RADIUS requests.

The router is identified by its IP address and shared secret.

For the RouterOS v7 User Manager interface, configure the RADIUS client using:

```routeros
/user-manager router
add name=pppoe-router \
    address=192.168.10.1 \
    shared-secret=StrongRadiusSecret
```

Example network:

| Component | Address |
| --- | --- |
| User Manager router | 192.168.10.1 |
| PPPoE NAS | 192.168.10.1 |
| RADIUS authentication | UDP 1812 |
| RADIUS accounting | UDP 1813 |

The address must match the source IP address used by the PPPoE router when sending RADIUS requests.

If User Manager and the PPPoE server run on the same MikroTik router, use an appropriate local address and matching RADIUS client configuration.

# 🗁 Configure RADIUS on the PPPoE Router

Configure the RADIUS client entry:

```routeros
/radius
add service=ppp \
    address=127.0.0.1 \
    secret=StrongRadiusSecret \
    authentication-port=1812 \
    accounting-port=1813 \
    timeout=3s
```

This example assumes User Manager is running on the same router and accepts RADIUS requests through the loopback address.

If User Manager runs on a different device, replace `127.0.0.1` with the User Manager server's reachable IP address.

Enable PPP RADIUS authentication:

```routeros
/ppp aaa
set use-radius=yes accounting=yes interim-update=5m
```

The PPPoE server can now authenticate subscribers through User Manager.

# 🗁 Create a User Manager Subscriber

Create a subscriber account:

```routeros
/user-manager user
add name=customer001 password=StrongPassword001
```

The subscriber can use these credentials when connecting through a supported RADIUS-integrated service.

Inspect configured users:

```routeros
/user-manager user print
```

Display detailed user information:

```routeros
/user-manager user print detail
```

# 🗁 Create a User Manager Limitation

A limitation defines restrictions applied to a subscriber service.

For example, create a limitation for a basic subscriber package:

```routeros
/user-manager limitation
add name=basic-limitation \
    rate-limit-rx=10M \
    rate-limit-tx=5M
```

The receive and transmit directions are defined from the perspective of the network access server and the RADIUS service implementation.

Verify the effective direction and resulting queue behavior in a test environment before applying a production bandwidth policy.

Other limitation settings can include:

* Session duration
* Idle timeout
* Data usage limits
* Rate limits
* Simultaneous connection restrictions

The exact available properties depend on the installed User Manager release.

# 🗁 Create a User Manager Profile

Create a profile for a subscriber package:

```routeros
/user-manager profile
add name=basic-profile \
    name-for-users=Basic Package \
    validity=30d
```

A profile organizes service settings and can be associated with limitations.

Inspect profiles:

```routeros
/user-manager profile print
```

A profile is useful for grouping subscribers according to their service packages.

# 🗁 Associate a Limitation with a Profile

Create a profile limitation association:

```routeros
/user-manager profile limitation
add profile=basic-profile \
    limitation=basic-limitation
```

This associates the bandwidth limitation with the profile.

The profile can then be assigned to subscribers.

# 🗁 Assign a Profile to a Subscriber

Assign the profile to a subscriber:

```routeros
/user-manager user
set [find name=customer001] \
    actual-profile=basic-profile
```

Inspect the subscriber configuration:

```routeros
/user-manager user print detail
```

The profile determines the service policy associated with the subscriber.

Verify the resulting RADIUS authorization attributes and active session behavior on the network access device.

# 🗁 Understand Subscriber Package Management

An ISP can create multiple profiles for different service packages.

Example package structure:

| Package | Download | Upload | Validity |
| --- | ---: | ---: | --- |
| Basic | 10 Mbps | 5 Mbps | 30 days |
| Standard | 25 Mbps | 10 Mbps | 30 days |
| Premium | 50 Mbps | 25 Mbps | 30 days |
| Business | 100 Mbps | 50 Mbps | 30 days |

Each package should have an appropriate limitation and profile.

For a larger ISP deployment, use a consistent naming convention for profiles, limitations, and subscribers.

# 🗁 Understand User Manager Accounting

Accounting allows User Manager to record subscriber connection information.

For PPP services, enable accounting on the NAS:

```routeros
/ppp aaa
set accounting=yes interim-update=5m
```

Accounting records can include:

* Subscriber username
* Session start time
* Session end time
* Session duration
* Input traffic
* Output traffic
* Assigned IP address
* NAS information

Inspect User Manager sessions:

```routeros
/user-manager session print
```

The session records help administrators investigate subscriber activity and usage.

# 🗁 Manage Subscriber Sessions

View active sessions:

```routeros
/user-manager session print
```

Inspect detailed session information:

```routeros
/user-manager session print detail
```

Use session information to identify connected subscribers and their service activity.

To terminate a subscriber session, use the appropriate session management operation supported by the installed User Manager release or disconnect the subscriber from the NAS.

For a PPPoE session on the NAS:

```routeros
/ppp active
remove [find name=customer001]
```

Disconnecting the session does not automatically disable the subscriber account.

# 🗁 User Manager and Hotspot

User Manager can also authenticate Hotspot users through RADIUS.

Configure a RADIUS entry:

```routeros
/radius
add service=hotspot \
    address=127.0.0.1 \
    secret=StrongRadiusSecret
```

Enable RADIUS in the Hotspot profile:

```routeros
/ip hotspot profile
set [find name=hotspot-profile] use-radius=yes
```

The Hotspot server sends authentication requests to User Manager.

A centralized subscriber database can then be used for both PPPoE and Hotspot services, provided the required user profiles and service policies are configured correctly.

# 🗁 Configure User Manager Access Security

User Manager contains subscriber credentials and service information.

Protect access to its management interface.

Recommended practices:

* Restrict administrative access to trusted management networks.
* Use strong administrator credentials.
* Keep RouterOS updated.
* Restrict RADIUS traffic to authorized NAS devices.
* Use unique shared secrets for different network access devices.
* Back up User Manager data and configuration.
* Monitor authentication and accounting activity.
* Avoid exposing management services directly to the public internet.

# 🗁 Troubleshoot User Manager

| Problem | Possible cause | What to check |
| --- | --- | --- |
| User Manager commands unavailable | Package missing or incompatible | Package installation and architecture |
| Subscriber authentication fails | Incorrect credentials | User Manager subscriber record |
| RADIUS timeout | Incorrect server address or blocked traffic | RADIUS reachability and firewall |
| Invalid RADIUS client | Shared secret or source address mismatch | User Manager router definition |
| Subscriber connects without expected limits | Incorrect profile or limitation association | Profile and limitation configuration |
| Accounting records missing | Accounting disabled or blocked | NAS accounting settings |
| Hotspot authentication fails | Hotspot RADIUS not enabled | Hotspot profile |
| User Manager database unavailable | Storage or package problem | System logs and storage status |
| Package does not load | Version or architecture mismatch | Downloaded package compatibility |

Useful commands:

```routeros
/system package print
```

```routeros
/user-manager router print detail
```

```routeros
/user-manager user print detail
```

```routeros
/user-manager profile print detail
```

```routeros
/user-manager limitation print detail
```

```routeros
/user-manager session print detail
```

Inspect RADIUS logs:

```routeros
/log print where topics~"radius"
```

# 🗁 Practical Lab

Build a centralized subscriber authentication environment using MikroTik User Manager and a PPPoE server.

**Topology**

```text
                Internet
                   |
             +-----------+
             | MikroTik  |
             | Router    |
             +-----------+
                   |
             PPPoE Server
                   |
              Subscribers

             User Manager
             RADIUS Server
             on MikroTik
```

**Configuration requirements**

* Install the compatible User Manager package.
* Verify that User Manager is available.
* Register the PPPoE router as a RADIUS client.
* Configure the RADIUS client entry on RouterOS.
* Enable PPP RADIUS authentication.
* Enable accounting.
* Create a subscriber account.
* Create a limitation.
* Create a subscriber profile.
* Associate the limitation with the profile.
* Assign the profile to the subscriber.

**Verification tasks**

* Connect a PPPoE client using the User Manager subscriber credentials.
* Verify that authentication succeeds.
* Inspect the active PPP session.
* Confirm the assigned IP address.
* Verify the bandwidth policy.
* Inspect User Manager session records.
* Disconnect the subscriber.
* Verify that the session is terminated.
* Test an invalid password.
* Inspect RADIUS logs when authentication fails.

# 🗁 Key Takeaways

* User Manager provides centralized subscriber management through RADIUS.
* It can authenticate PPPoE and Hotspot users.
* The MikroTik network access device must be registered as a RADIUS client.
* Subscribers are maintained in a centralized User Manager database.
* Profiles and limitations organize service packages and restrictions.
* RADIUS accounting records subscriber session and usage information.
* User Manager package compatibility depends on RouterOS version and architecture.
* Centralized authentication simplifies subscriber management across multiple MikroTik routers.
* Proper access control, shared-secret management, backups, and monitoring are essential for reliable ISP operations.
