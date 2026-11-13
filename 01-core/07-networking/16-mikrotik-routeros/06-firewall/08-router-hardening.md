# Router Hardening

Router hardening is the process of reducing a router's attack surface, restricting unauthorized access, and protecting network infrastructure against common security threats.

A MikroTik router may provide internet access, routing, firewalling, VPN services, wireless connectivity, and ISP infrastructure. If its management services or firewall policies are poorly configured, attackers may gain unauthorized access or exploit exposed services.

Router hardening involves several security layers:

* Secure administrative accounts.
* Restrict management access.
* Disable unnecessary services.
* Configure firewall protection.
* Secure remote administration.
* Keep RouterOS and RouterBOARD firmware updated.
* Monitor suspicious activity.
* Maintain reliable backups.

The objective is to expose only the services required for normal network operation.

# 🗁 Secure Administrative Accounts

The default `admin` account is a common target during automated attacks.

Create a dedicated administrative account with a strong password.

```routeros
/user
add name=network-admin group=full password="Use-A-Strong-Unique-Password"
```

After confirming that the new account works, disable the default account:

```routeros
/user
disable admin
```

Verify configured users:

```routeros
/user/print
```

**Administrative account best practices**

* Use unique usernames instead of predictable account names.
* Use strong, unique passwords.
* Avoid sharing administrator credentials.
* Create separate accounts for different administrators.
* Assign the minimum required permissions.
* Disable unused accounts.
* Review account activity regularly.

For automated deployments, avoid embedding real passwords in scripts, exported configurations, or shared documentation.

# 🗁 User Groups and Permissions

RouterOS uses user groups to control administrative permissions.

Display available groups:

```routeros
/user/group/print
```

Common built-in groups include:

| Group | Purpose |
| --- | --- |
| `full` | Full administrative permissions |
| `read` | Read-only access |
| `write` | Configuration modification permissions with restrictions |
| `policy` | Policy-related permissions |

Create a limited read-only account:

```routeros
/user
add name=network-monitor group=read password="Use-A-Unique-Password"
```

Use the appropriate permission group for each administrator or monitoring account.

Avoid giving full administrative permissions to users who only need monitoring access.

# 🗁 Restrict Management Services

RouterOS provides several management services.

Display configured services:

```routeros
/ip/service/print
```

Common services include:

| Service | Default Port | Purpose |
| --- | ---: | --- |
| Telnet | 23 | Remote terminal access |
| FTP | 21 | File transfer |
| SSH | 22 | Secure remote terminal access |
| HTTP | 80 | Web management |
| HTTPS | 443 | Secure web management |
| WinBox | 8291 | Router management |
| API | 8728 | RouterOS API |
| API-SSL | 8729 | Secure RouterOS API |

Disable services that are not required:

```routeros
/ip/service
disable telnet
disable ftp
disable www
disable api
disable api-ssl
```

If HTTPS and WinBox are not needed, disable them as well.

```routeros
/ip/service
disable www-ssl
disable winbox
```

Only disable services after confirming that another management method is available.

# 🗁 Restrict Management Access by IP

Disabling unnecessary services is not enough. Required management services should also be restricted to trusted addresses.

For example, assume the management network is:

```text
192.168.50.0/24
```

Restrict WinBox access:

```routeros
/ip/service
set winbox address=192.168.50.0/24
```

Restrict SSH access:

```routeros
/ip/service
set ssh address=192.168.50.0/24
```

Restrict HTTPS access:

```routeros
/ip/service
set www-ssl address=192.168.50.0/24
```

The `address` property limits which source addresses can access the service.

It is also important to enforce management restrictions through firewall rules. Service-level restrictions and firewall filtering provide complementary protection.

# 🗁 Configure a Management Address List

Address lists make firewall policies easier to maintain.

Create a trusted management address list:

```routeros
/ip/firewall/address-list
add list=trusted-management address=192.168.50.0/24 comment="Trusted management network"
add list=trusted-management address=192.168.10.10 comment="Authorized administrator"
```

Allow management access from trusted addresses:

```routeros
/ip/firewall/filter
add chain=input src-address-list=trusted-management protocol=tcp dst-port=22,443,8291 action=accept comment="Allow trusted management"
```

This rule permits SSH, HTTPS, and WinBox from the listed addresses.

Place the rule appropriately in the input chain, before a broader rule that would drop the same traffic.

# 🗁 Secure the Input Chain

The input chain protects services running on the router itself.

A router should generally accept only the traffic required for its operation.

A simplified input-chain policy might look like this:

```routeros
/ip/firewall/filter
add chain=input connection-state=established,related action=accept comment="Allow established and related"
add chain=input connection-state=invalid action=drop comment="Drop invalid"
add chain=input protocol=icmp action=accept comment="Allow ICMP"
add chain=input src-address-list=trusted-management protocol=tcp dst-port=22,443,8291 action=accept comment="Allow trusted management"
add chain=input in-interface=bridge-LAN action=accept comment="Allow LAN access to router"
add chain=input action=drop comment="Drop other router access"
```

This is an illustrative policy, not a universal configuration.

Before deploying an input-chain policy, identify all required router services, including:

* DHCP.
* DNS, if the router provides DNS service.
* VPN services.
* Routing protocols.
* Monitoring services.
* Network management services.
* ICMP diagnostics.

For example, if the router provides DNS to LAN clients, appropriate UDP and TCP port 53 access must be allowed from the trusted LAN.

If DHCP is provided by the router, the DHCP server's required input traffic must also be permitted.

A broad LAN acceptance rule is convenient for a basic trusted LAN, but more restrictive deployments should permit only the required services.

# 🗁 Secure the Forward Chain

The forward chain controls traffic routed through the router.

It should prevent unauthorized traffic from crossing network boundaries.

Example:

```routeros
/ip/firewall/filter
add chain=forward connection-state=established,related action=accept comment="Allow established and related"
add chain=forward connection-state=invalid action=drop comment="Drop invalid"
add chain=forward in-interface=bridge-LAN out-interface=ether1 action=accept comment="Allow LAN to WAN"
add chain=forward action=drop comment="Drop other forwarded traffic"
```

This example assumes:

* `bridge-LAN` is the trusted LAN.
* `ether1` is the WAN.
* LAN clients should access the internet.
* Other forwarding traffic should be denied unless explicitly permitted.

If the router provides port forwarding, VPN access, inter-VLAN routing, or other services, add specific forwarding permissions before the final drop rule.

# 🗁 Secure Neighbor Discovery

MikroTik Neighbor Discovery helps devices identify other MikroTik devices on the network.

It can reveal device information to neighboring hosts.

Display the current discovery configuration:

```routeros
/ip/neighbor/discovery-settings/print
```

Restrict discovery to trusted interfaces:

```routeros
/ip/neighbor/discovery-settings
set discover-interface-list=LAN
```

The `LAN` interface list must exist and contain the intended interfaces.

Create it if necessary:

```routeros
/interface/list
add name=LAN comment="Trusted LAN interfaces"

/interface/list/member
add list=LAN interface=bridge-LAN
```

Neighbor discovery should generally not be enabled on untrusted WAN interfaces.

# 🗁 Restrict MAC-Based Management

MikroTik supports MAC-based management mechanisms that operate at Layer 2.

These include:

* MAC Telnet.
* MAC WinBox.
* MAC Ping.

They can be useful during local administration and recovery, but unrestricted access can expose management capabilities to unwanted devices on the same Layer 2 network.

Inspect MAC server settings:

```routeros
/tool/mac-server/print
/tool/mac-server/mac-winbox/print
```

Restrict MAC server access to a trusted interface list:

```routeros
/tool/mac-server
set allowed-interface-list=LAN

/tool/mac-server/mac-winbox
set allowed-interface-list=LAN
```

Disable MAC Telnet if it is not required:

```routeros
/tool/mac-server
set allowed-interface-list=none
```

Be aware that the MAC server and MAC WinBox settings are separate. Configure both according to the management requirements.

Restrict MAC Ping as well:

```routeros
/tool/mac-server/ping
set enabled=no
```

# 🗁 Disable Unnecessary Discovery and Management Features

Review services and features that are not required.

Examples include:

**Disable bandwidth-server access**

```routeros
/tool/bandwidth-server
set enabled=no
```

**Disable proxy services when unused**

```routeros
/ip/proxy
set enabled=no
```

**Disable SOCKS proxy when unused**

```routeros
/ip/socks
set enabled=no
```

**Disable the router's DNS cache service if it is not needed**

```routeros
/ip/dns
set allow-remote-requests=no
```

Only disable DNS remote requests if LAN clients are not supposed to use the router as their DNS resolver. If the router provides DNS service, restrict access to trusted networks through firewall rules.

# 🗁 Secure DNS Configuration

When RouterOS provides DNS resolution to LAN clients, remote DNS access should be restricted.

Example:

```routeros
/ip/dns
set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
```

Then allow DNS queries from the trusted LAN:

```routeros
/ip/firewall/filter
add chain=input in-interface=bridge-LAN protocol=udp dst-port=53 action=accept comment="Allow LAN DNS UDP"
add chain=input in-interface=bridge-LAN protocol=tcp dst-port=53 action=accept comment="Allow LAN DNS TCP"
```

Ensure these rules appear before the final input-chain drop rule.

Do not expose the router's DNS resolver to the public internet unless there is a deliberate, secured design for doing so.

# 🗁 Protect Against Brute-Force Attempts

Brute-force attacks repeatedly attempt to guess administrative credentials.

The strongest protections are:

* Do not expose management services publicly.
* Restrict access to trusted IP addresses.
* Use strong, unique passwords.
* Prefer VPN-based remote administration.
* Monitor authentication failures.
* Disable unused services.

Address lists can also be used to temporarily block suspicious sources.

Example of a manually maintained blocked-address list:

```routeros
/ip/firewall/address-list
add list=blocked-sources address=198.51.100.50 comment="Manually blocked source"
```

Drop matching packets early:

```routeros
/ip/firewall/raw
add chain=prerouting src-address-list=blocked-sources action=drop comment="Drop blocked sources"
```

The address is an example documentation address. Automated brute-force protection requires carefully designed detection rules, thresholds, and exclusions to avoid blocking legitimate administrators.

# 🗁 Secure Remote Administration with VPN

Remote management should preferably use a VPN rather than exposing WinBox, SSH, or WebFig directly to the internet.

Common VPN options include:

* WireGuard.
* IPsec.
* OpenVPN.
* SSTP.

A typical secure management architecture is:

```text
Remote Administrator
        |
        v
   VPN Connection
        |
        v
  MikroTik Router
        |
        v
 Management Network
        |
        v
 Router Administration
```

After connecting to the VPN, the administrator accesses the router through its private management address.

This approach reduces direct exposure of administrative services.

# 🗁 Keep RouterOS and Firmware Updated

Security vulnerabilities may be fixed through RouterOS and RouterBOARD firmware updates.

Check the installed RouterOS version:

```routeros
/system/resource/print
```

Check available RouterOS updates:

```routeros
/system/package/update/check-for-updates
```

Upgrade through the appropriate RouterOS update channel:

```routeros
/system/package/update
set channel=stable
check-for-updates
```

If an update is available, review the offered version and follow the appropriate upgrade procedure.

Check RouterBOARD firmware:

```routeros
/system/routerboard/print
```

Upgrade RouterBOARD firmware when an appropriate firmware version is available:

```routeros
/system/routerboard/upgrade
```

Reboot when required:

```routeros
/system/reboot
```

Always review release notes, hardware compatibility, backup availability, and maintenance requirements before upgrading production equipment.

# 🗁 Protect Configuration Backups

Configuration backups contain sensitive network information.

They may expose:

* IP addressing.
* Firewall policies.
* VPN configuration.
* User accounts.
* Network topology.
* Service configuration.
* Other operational details.

Create a binary backup:

```routeros
/system/backup/save name=pre-hardening-backup
```

Export the configuration:

```routeros
/export file=pre-hardening-export
```

Protect backup files with secure storage, restricted permissions, and encryption where appropriate.

Avoid uploading configuration backups to public repositories or sharing them through unsecured channels.

# 🗁 Monitor Router Activity

Regular monitoring helps identify suspicious behavior and operational problems.

**Review active users**

```routeros
/user/active/print
```

**Review system logs**

```routeros
/log/print
```

**Review current connections**

```routeros
/ip/firewall/connection/print
```

**Review listening services**

```routeros
/ip/service/print
```

**Review system resources**

```routeros
/system/resource/print
```

**Review interface traffic**

```routeros
/interface/monitor-traffic ether1
```

For larger networks, consider centralized monitoring and logging through appropriate monitoring systems and secure management networks.

# 🗁 Router Hardening Checklist

Use this checklist when preparing a MikroTik router for deployment.

* [ ] Create a dedicated administrative account.
* [ ] Disable or secure the default administrator account.
* [ ] Use strong, unique passwords.
* [ ] Assign appropriate user permissions.
* [ ] Disable unused management services.
* [ ] Restrict SSH, WinBox, and HTTPS access.
* [ ] Configure trusted management address lists.
* [ ] Secure the input chain.
* [ ] Secure the forward chain.
* [ ] Restrict Neighbor Discovery.
* [ ] Restrict MAC-based management.
* [ ] Disable unnecessary proxy and bandwidth-server services.
* [ ] Restrict DNS access to trusted networks.
* [ ] Prefer VPN for remote administration.
* [ ] Update RouterOS and RouterBOARD firmware.
* [ ] Create and securely store configuration backups.
* [ ] Review logs and active connections.
* [ ] Test administrative access after applying changes.

# 🗁 Best Practices

* Apply a default-deny approach to router management access.
* Keep management interfaces separate from untrusted networks.
* Use VPN for remote administration.
* Disable services that are not required.
* Use individual administrative accounts.
* Avoid exposing management ports directly to the internet.
* Keep RouterOS and firmware maintained.
* Store backups securely.
* Review firewall policies after every major configuration change.
* Maintain an emergency recovery and access procedure.
* Test security changes before deploying them to production routers.

# 🗁 Knowledge Check

* What is router hardening?
* Why should unnecessary management services be disabled?
* What is the difference between service-level restrictions and firewall restrictions?
* Why should MAC-based management be restricted?
* Why is exposing WinBox directly to the internet undesirable?
* How can a VPN improve remote administration security?
* Why should DNS access be restricted?
* What information can configuration backups expose?
* Why should RouterBOARD firmware be maintained alongside RouterOS?
* What is the purpose of a default-deny firewall policy?

# 🗁 References

* MikroTik RouterOS Documentation — Services: <https://help.mikrotik.com/docs/spaces/ROS/pages/103841820/Services>
* MikroTik RouterOS Documentation — Firewall: <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall>
* MikroTik RouterOS Documentation — User: <https://help.mikrotik.com/docs/spaces/ROS/pages/8978504/User>
* MikroTik RouterOS Documentation — Neighbor Discovery: <https://help.mikrotik.com/docs/spaces/ROS/pages/24805517/Neighbor+discovery>
* MikroTik RouterOS Documentation — RouterBOARD: <https://help.mikrotik.com/docs/spaces/ROS/pages/18295853/RouterBOARD>
