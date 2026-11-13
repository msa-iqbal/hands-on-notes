# WinBox

WinBox is MikroTik's graphical configuration and management utility for RouterOS devices.

It provides an interface for configuring, monitoring, and troubleshooting MikroTik routers without requiring administrators to enter every command manually.

WinBox is commonly used by network engineers, system administrators, and ISP technicians for daily router management.

Its main capabilities include:

* Managing Ethernet and wireless interfaces.
* Configuring IP addresses and routing.
* Creating bridges and VLANs.
* Configuring DHCP servers and clients.
* Managing firewall and NAT rules.
* Configuring VPN connections.
* Managing users and permissions.
* Monitoring CPU, memory, and network traffic.
* Viewing system logs.
* Performing backups and configuration exports.

WinBox communicates with RouterOS through the router's management services.

# 🗁 Downloading and Installing WinBox

Download WinBox from the official MikroTik website:

<https://mikrotik.com/download>

Select the WinBox version appropriate for your operating system.

MikroTik provides WinBox for supported Windows, macOS, and Linux environments.

**Installation on Windows**

1. Open the official MikroTik download page.
2. Download the Windows WinBox executable.
3. Open the downloaded application.
4. Allow the application to start.
5. Connect to your MikroTik router.

WinBox is distributed as a standalone application, so a traditional installation wizard is generally not required on Windows.

**Running WinBox on Linux**

Depending on the Linux distribution and available compatibility environment, WinBox can be run using the supported Linux package or through a compatibility layer.

Download the Linux version from the official MikroTik download page and follow its provided instructions.

# 🗁 Understanding the WinBox Interface

The WinBox interface provides access to RouterOS configuration menus and operational information.

Common interface areas include:

| Area | Purpose |
| --- | --- |
| Connect To | Enter the router IP address or MAC address |
| Login | Enter the RouterOS username |
| Password | Enter the account password |
| Neighbors | Discover MikroTik devices on the local network |
| Session | Manage the current connection |
| Main menu | Access RouterOS configuration sections |
| Terminal | Execute RouterOS CLI commands |
| Dashboard | View router status and resource information |

The exact appearance and available controls can vary between WinBox versions.

# 🗁 Connecting to a MikroTik Router

WinBox supports two common connection methods:

* IP-based connection.
* MAC-based connection.

The appropriate method depends on the network configuration and accessibility of the router.

**IP-based connection**

An IP-based connection uses the router's configured IP address.

Example:

```text
Address: 192.168.88.1
Username: admin
Password: your-password
```

Connection procedure:

1. Open WinBox.
2. Enter the router's IP address in Connect To.
3. Enter the username.
4. Enter the password.
5. Select Connect.

IP-based management can work across routed networks when routing, firewall rules, and management service restrictions permit access.

**MAC-based connection**

MAC-based access allows WinBox to connect to a MikroTik router using its Layer 2 MAC address.

It is particularly useful when:

* The router has no IP address configured.
* The IP address is unknown.
* The router's IP configuration is incorrect.
* Initial configuration is required.
* IP connectivity is unavailable but Layer 2 connectivity remains available.

Connection procedure:

1. Connect your computer to the router or the same Layer 2 network.
2. Open WinBox.
3. Open the Neighbors section.
4. Wait for neighbor discovery.
5. Select the router's MAC address.
6. Enter your login credentials.
7. Select Connect.

MAC-based access generally requires both devices to be within the same Layer 2 discovery domain. It does not provide normal routed access across separate IP networks.

**IP vs MAC connection**

| Feature | IP connection | MAC connection |
| --- | --- | --- |
| Address type | IPv4 or IPv6 address | MAC address |
| Network layer | Layer 3 | Layer 2 |
| Requires IP configuration | Yes | No |
| Works across routed networks | Yes, when permitted | Generally no |
| Common purpose | Regular administration | Initial setup and recovery |

# 🗁 Understanding the Neighbors List

The Neighbors section displays MikroTik devices discovered through supported neighbor discovery mechanisms.

It helps administrators identify routers and other MikroTik devices connected to the local network.

Common information displayed includes:

| Field | Description |
| --- | --- |
| Identity | Router system identity |
| Address | Discovered IP address |
| MAC Address | Hardware MAC address |
| Version | RouterOS version |
| Board | Device model or board information |
| Interface | Interface through which the neighbor was discovered |

**How neighbor discovery works**

RouterOS uses MikroTik Neighbor Discovery Protocol (MNDP), along with supported discovery mechanisms such as LLDP and CDP, to identify neighboring devices.

Discovery is controlled through RouterOS configuration and interface settings.

Display neighbor discovery configuration:

```routeros
/ip neighbor discovery-settings print
```

Display discovered neighbors:

```routeros
/ip neighbor print
```

Display neighbor discovery interfaces:

```routeros
/ip neighbor discovery-settings print
```

Discovery visibility depends on the enabled discovery protocols, interface configuration, and network topology.

**Restricting neighbor discovery**

For security and network organization, neighbor discovery should be enabled only on interfaces where it is needed.

Example:

```routeros
/ip neighbor discovery-settings
set discover-interface-list=LAN
```

This example assumes that an interface list named `LAN` already exists.

Display interface lists:

```routeros
/interface list print
```

# 🗁 WinBox Connection Settings

WinBox provides connection options that help administrators manage sessions.

Common settings include:

* Router address.
* Username.
* Password.
* Session configuration.
* Connection preferences.
* Saved connection entries, depending on WinBox version.

For regular administration, use a known management IP address instead of depending exclusively on neighbor discovery.

**Saving connection details**

You can maintain a list of frequently managed routers using descriptive connection entries.

Example naming convention:

| Router | Address | Purpose |
| --- | --- | --- |
| MAIN-GW-01 | `192.168.10.1` | Main gateway |
| CORE-RTR-01 | `192.168.20.1` | Core router |
| BRANCH-RTR-01 | `192.168.30.1` | Branch router |
| LAB-CHR-01 | `192.168.100.1` | Virtual lab router |

Avoid storing passwords in shared or unsecured environments.

# 🗁 Managing RouterOS Through WinBox

After successfully connecting, the main WinBox window provides access to RouterOS menus.

Some frequently used menus include:

| Menu | Purpose |
| --- | --- |
| Interfaces | Manage physical and virtual interfaces |
| Bridge | Configure Layer 2 bridges |
| PPP | Configure PPP, PPPoE, and related services |
| IP | Manage IP addressing, DHCP, DNS, routes, and firewall |
| IPv6 | Configure IPv6 addressing and routing |
| Routing | Manage routing tables and routing protocols |
| Queues | Configure bandwidth management |
| Wireless | Configure wireless features on supported devices |
| System | Manage identity, users, packages, resources, and services |
| Files | Manage router files and backups |
| Log | View system events and messages |
| Tools | Access diagnostics and troubleshooting utilities |
| New Terminal | Open the RouterOS command-line interface |

The available menus depend on RouterOS packages, device capabilities, and configuration.

# 🗁 Using WinBox Terminal

WinBox includes a built-in terminal for executing RouterOS CLI commands.

This is useful when a configuration is easier to perform through commands than through graphical menus.

Open Terminal from the WinBox main menu.

Example commands:

Display interfaces:

```routeros
/interface print
```

Display IP addresses:

```routeros
/ip address print
```

Display routing table:

```routeros
/ip route print
```

Display firewall rules:

```routeros
/ip firewall filter print
```

Set system identity:

```routeros
/system identity set name=CORE-RTR-01
```

The terminal uses the same RouterOS command environment available through SSH and other supported CLI access methods.

# 🗁 Monitoring Router Resources

WinBox provides access to system resource information.

Open the System menu and select Resources.

You can inspect:

* CPU usage.
* Available memory.
* Total memory.
* Free storage.
* RouterOS version.
* Architecture.
* Uptime.
* Board name.

The equivalent CLI command is:

```routeros
/system resource print
```

**Monitoring interface traffic**

Open Interfaces and select an interface to inspect its traffic statistics.

You can review:

* Receive traffic.
* Transmit traffic.
* Packet counters.
* Error counters.
* Link status.
* Interface properties.

For more detailed live traffic analysis, use Tools such as Torch.

# 🗁 Managing Files Through WinBox

WinBox provides a Files section for managing files stored on the router.

Common uses include:

* Uploading configuration scripts.
* Downloading backups.
* Uploading certificates.
* Managing RouterOS package files.
* Transferring files required by RouterOS services.

**Uploading a file**

1. Open WinBox.
2. Navigate to Files.
3. Drag a file into the Files window or use the available upload control.
4. Wait for the transfer to finish.
5. Verify that the file appears in the router's file list.

**Downloading a file**

1. Open Files.
2. Select the required file.
3. Use the download option or drag the file to your computer.
4. Verify the downloaded file.

Always protect backup files and configuration exports because they may contain sensitive network information.

# 🗁 Working With Safe Mode

WinBox supports RouterOS Safe Mode through its terminal.

Safe Mode helps protect against accidental loss of management access when making configuration changes.

For example, changing firewall rules or management IP addresses can interrupt an active WinBox session.

To activate Safe Mode in the terminal, press:

```text
Ctrl + X
```

When Safe Mode is active, RouterOS tracks changes made during that session.

If the session terminates unexpectedly, the changes made in Safe Mode are automatically reverted.

Press `Ctrl + X` again to leave Safe Mode and commit the changes.

Use Safe Mode when modifying configurations that could interrupt your current management connection.

# 🗁 Configuring WinBox Access Restrictions

WinBox management access is provided by the RouterOS WinBox service.

The conventional default TCP port is `8291`, although it can be changed.

Display the service configuration:

```routeros
/ip service print
```

Restrict WinBox access to a trusted management subnet:

```routeros
/ip service
set [find name=winbox] address=192.168.10.0/24
```

Restrict access to a single administration computer:

```routeros
/ip service
set [find name=winbox] address=192.168.10.10/32
```

Change the WinBox service port:

```routeros
/ip service
set [find name=winbox] port=8292
```

After changing the port, connect using the new port in the WinBox connection settings.

Service-level address restrictions should be combined with appropriate firewall rules.

**Firewall example**

Allow WinBox access from a trusted management subnet:

```routeros
/ip firewall filter
add chain=input action=accept protocol=tcp dst-port=8291 src-address=192.168.10.0/24
```

Place this rule before any rule that would otherwise drop the management connection.

Use the actual configured WinBox port and management subnet.

# 🗁 WinBox Security Best Practices

WinBox is a powerful management interface, so access should be controlled carefully.

Recommended practices:

* Download WinBox only from the official MikroTik website.
* Keep WinBox and RouterOS updated.
* Use strong administrative passwords.
* Create individual accounts for administrators.
* Restrict access to trusted management networks.
* Use VPN access for remote router administration.
* Disable unnecessary management services.
* Avoid exposing WinBox directly to the public Internet.
* Restrict MAC-based management to trusted interfaces.
* Disable unnecessary neighbor discovery.
* Review router logs regularly.
* Maintain secure configuration backups.

Changing the default WinBox port does not replace proper authentication and firewall protection.

# 🗁 Troubleshooting WinBox Connections

| Problem | Possible cause | Recommended solution |
| --- | --- | --- |
| Router does not appear in Neighbors | Discovery disabled or Layer 2 connectivity problem | Check cables, bridge membership, and discovery settings |
| IP connection fails | Incorrect IP address or routing problem | Verify IP configuration and reachability |
| MAC connection fails | MAC server disabled or interface restrictions | Check MAC server configuration and Layer 2 access |
| Login rejected | Incorrect username or password | Verify account credentials |
| Connection refused | WinBox service disabled or port changed | Check `/ip service print` |
| Connection times out | Firewall blocking management traffic | Review input-chain rules |
| Router is visible but inaccessible | Service restrictions or authentication issue | Check WinBox service address restrictions and user permissions |
| Connection drops after firewall changes | Management traffic is blocked | Review rule order and use a recovery method |
| Router responds slowly | High CPU usage or network congestion | Inspect system resources and interface traffic |

Useful diagnostic commands:

Display the WinBox service:

```routeros
/ip service print where name=winbox
```

Display discovered neighbors:

```routeros
/ip neighbor print
```

Display interface status:

```routeros
/interface print
```

Display management-related firewall rules:

```routeros
/ip firewall filter print
```

Display active connections:

```routeros
/ip firewall connection print
```

Display recent logs:

```routeros
/log print
```

# 🗁 Practical WinBox Administration Workflow

A consistent workflow helps reduce configuration mistakes.

**Routine administration**

1. Open WinBox.
2. Select the intended router.
3. Verify its identity and address.
4. Authenticate with an individual administrator account.
5. Review the current configuration.
6. Make the required changes.
7. Verify the affected services.
8. Check logs and interface status.
9. Save a backup when appropriate.
10. Disconnect when administration is complete.

**Before changing critical configuration**

1. Confirm that you are connected to the correct router.
2. Export the current configuration.
3. Ensure that an alternative management method is available.
4. Activate Safe Mode when appropriate.
5. Apply changes carefully.
6. Verify connectivity before committing the changes.

# 🗁 Official Documentation

* [MikroTik WinBox](https://help.mikrotik.com/docs/spaces/ROS/pages/328129/WinBox)
* [MikroTik Management Tools](https://help.mikrotik.com/docs/spaces/ROS/pages/328121/Management+tools)
* [MikroTik Services](https://help.mikrotik.com/docs/spaces/ROS/pages/103841820/Services)
* [MikroTik Neighbor Discovery](https://help.mikrotik.com/docs/spaces/ROS/pages/24805517/Neighbor+discovery)
* [MikroTik Firewall](https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall)
* [MikroTik Downloads](https://mikrotik.com/download)
