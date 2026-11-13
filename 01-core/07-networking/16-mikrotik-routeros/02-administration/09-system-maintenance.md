# MikroTik RouterOS System Maintenance

System maintenance is the process of monitoring, managing, and maintaining a MikroTik router to ensure stable operation, reliable network connectivity, security, and optimal performance.

Regular maintenance helps network administrators identify potential problems before they affect users or critical network services.

RouterOS provides several tools for system monitoring, resource management, file management, configuration inspection, and hardware maintenance.

This documentation covers essential maintenance operations for MikroTik RouterOS v7.

# 🗁 Checking System Information

Start maintenance by checking the router's basic system information.

```routeros
/system resource print
```

Important fields include:

| Field | Description |
| --- | --- |
| `uptime` | Time since the router last booted |
| `version` | Installed RouterOS version |
| `build-time` | RouterOS build date |
| `factory-software` | Original factory software version |
| `free-memory` | Available RAM |
| `total-memory` | Total installed RAM |
| `cpu` | CPU model |
| `cpu-count` | Number of CPU cores |
| `cpu-frequency` | CPU frequency |
| `cpu-load` | Current CPU utilization |
| `architecture-name` | Hardware architecture |
| `board-name` | Router model |

Check the router identity:

```routeros
/system identity print
```

Change the router identity:

```routeros
/system identity set name=CORE-ROUTER
```

A meaningful identity helps administrators distinguish devices in WinBox, monitoring systems, and network documentation.

# 🗁 Monitoring CPU Usage

CPU utilization indicates how much processing capacity the router is currently using.

Check CPU usage:

```routeros
/system resource print
```

For a continuously updating view:

```routeros
/system resource monitor
```

Monitor individual CPU cores:

```routeros
/system resource cpu print
```

High CPU utilization can result from:

* Heavy traffic processing.
* Large firewall rule sets.
* Complex routing operations.
* Excessive connection tracking.
* VPN encryption.
* Queue processing.
* Frequent management queries.
* Misconfigured scripts.
* Unexpected traffic or attacks.

To investigate CPU-intensive activity, inspect the router's active processes:

```routeros
/tool profile
```

This displays CPU usage by RouterOS subsystems and processes.

For example, unusually high firewall, networking, or management-related CPU usage may indicate where further investigation is required.

# 🗁 Monitoring Memory Usage

Memory is required for RouterOS services, connection tracking, routing tables, firewall rules, queues, and other system operations.

Check memory:

```routeros
/system resource print
```

Monitor memory continuously:

```routeros
/system resource monitor
```

Important memory fields include:

* `total-memory`
* `free-memory`

Calculate approximate memory utilization:

\[
\text{Memory Usage} =
\frac{\text{Total Memory}-\text{Free Memory}}
{\text{Total Memory}}\times100
\]

A router with consistently low free memory may experience instability or service interruptions.

Investigate:

* Excessive connection tracking entries.
* Large routing tables.
* Memory-intensive services.
* Unnecessary packages or processes.
* Scripts that create excessive data.
* Unexpected traffic patterns.

# 🗁 Monitoring Router Temperature and Hardware Health

Supported MikroTik devices expose hardware health information through RouterOS.

Run:

```routeros
/system health print
```

Depending on the hardware, available information may include:

* CPU temperature.
* Board temperature.
* Voltage.
* Fan speed.
* Power-related measurements.

Some devices do not provide all health metrics.

Check RouterBOARD information:

```routeros
/system routerboard print
```

Review:

* RouterBOARD model.
* Serial number.
* Current firmware.
* Available upgrade firmware.
* Factory firmware.

For devices deployed in network racks or outdoor installations, regularly inspect cooling, ventilation, power supplies, and physical connections.

# 🗁 Monitoring Uptime and Reboots

Uptime indicates how long a router has been operating without restarting.

Check uptime:

```routeros
/system resource print
```

A router that frequently reboots unexpectedly may have problems involving:

* Unstable power.
* Hardware faults.
* Watchdog configuration.
* Software crashes.
* Failed upgrades.
* Environmental conditions.

Check system logs:

```routeros
/log print
```

Review messages around the suspected reboot time.

Also inspect watchdog settings:

```routeros
/system watchdog print
```

Watchdog can automatically restart a router when configured conditions are met.

Do not disable watchdog functionality without understanding why it is enabled.

# 🗁 Monitoring Interface Status

Network interfaces are essential to router connectivity.

Display all interfaces:

```routeros
/interface print
```

Display detailed interface information:

```routeros
/interface print detail
```

Monitor an individual Ethernet interface:

```routeros
/interface ethernet monitor ether1
```

Replace `ether1` with the appropriate interface name.

Depending on the interface and hardware, monitoring information may include:

* Link status.
* Negotiated speed.
* Duplex mode.
* Auto-negotiation status.
* Link partner information.

Check interface traffic:

```routeros
/interface monitor-traffic ether1
```

This displays current receive and transmit traffic.

For detailed interface statistics:

```routeros
/interface print stats
```

Investigate:

* Interfaces that unexpectedly go down.
* Negotiated speeds below expected values.
* Excessive errors.
* Packet drops.
* Unstable physical links.
* Incorrect duplex or speed configuration.

# 🗁 Monitoring Disk and File Storage

RouterOS devices may use internal flash storage, NAND, SSD, USB storage, or other supported storage devices.

Check system resources:

```routeros
/system resource print
```

Inspect stored files:

```routeros
/file print
```

Inspect available storage devices:

```routeros
/disk print
```

The disk menu and available information depend on the device and storage hardware.

Remove unnecessary files only after confirming that important backups and configuration exports are safely stored elsewhere.

For production devices, avoid filling storage with excessive backups, logs, or temporary files.

# 🗁 Managing System Clock and Time Zone

Correct system time is important for logging, certificates, scheduled tasks, monitoring, and troubleshooting.

Display the current date and time:

```routeros
/system clock print
```

Set the time zone:

```routeros
/system clock set time-zone-name=Asia/Dhaka
```

Configure the clock manually when required:

```routeros
/system clock set date=nov/11/2026 time=10:30:00
```

For automatic time synchronization, configure an NTP client.

Check the available NTP configuration:

```routeros
/system ntp client print
```

Enable the NTP client and configure a server:

```routeros
/system ntp client set enabled=yes
/system ntp client servers add address=time.cloudflare.com
```

Verify synchronization:

```routeros
/system ntp client print
/system clock print
```

Use reliable NTP sources and ensure the router can reach them.

# 🗁 Managing System Identity and Notes

System identity helps identify a router within a network.

Set a meaningful identity:

```routeros
/system identity set name=EDGE-ROUTER-01
```

Display the identity:

```routeros
/system identity print
```

RouterOS also supports system notes, which can be used to document device information.

```routeros
/system note set note="Location: Main Office | Role: Edge Router"
```

Display the note:

```routeros
/system note print
```

System notes can help administrators record operational details such as:

* Router location.
* Network role.
* Maintenance contact.
* Management information.
* Device ownership.

Avoid storing passwords or other sensitive credentials in system notes.

# 🗁 Managing Installed Packages

RouterOS consists of packages that provide operating system functionality.

Display installed packages:

```routeros
/system package print
```

Depending on the RouterOS release and device, installed packages may include:

* `routeros`
* `wireless`
* `wifi-qcom`
* `wifi-qcom-ac`
* `container`
* Other supported packages.

Package availability depends on hardware architecture and RouterOS version.

Do not remove packages without understanding their dependencies and impact on network services.

Before changing packages, create a backup and review the official package documentation.

# 🗁 Managing Running Services

RouterOS provides services such as SSH, WinBox, WebFig, API, and other management interfaces.

Display available services:

```routeros
/ip service print
```

Common services include:

| Service | Default Port | Purpose |
| --- | ---: | --- |
| Telnet | 23 | Remote terminal access |
| FTP | 21 | File transfer |
| SSH | 22 | Secure terminal access |
| HTTP | 80 | WebFig access |
| HTTPS | 443 | Secure WebFig access |
| WinBox | 8291 | WinBox management |
| API | 8728 | RouterOS API |
| API-SSL | 8729 | Secure RouterOS API |

Disable an unused service:

```routeros
/ip service disable telnet
```

Change a service port when required:

```routeros
/ip service set winbox port=8292
```

Restrict management services to trusted management networks:

```routeros
/ip service set winbox address=192.168.88.0/24
```

Apply restrictions carefully to avoid losing management access.

Service-level restrictions should be combined with appropriate firewall rules.

# 🗁 Managing Automatic Reboot

RouterOS supports scheduled tasks through the Scheduler menu.

Although automatic rebooting can be useful in specific operational scenarios, it should not be used as a substitute for diagnosing system problems.

Inspect scheduled tasks:

```routeros
/system scheduler print
```

Create a scheduled reboot:

```routeros
/system scheduler add name=weekly-reboot \
    start-time=04:00:00 \
    interval=1w \
    on-event="/system reboot"
```

This schedules a weekly reboot at 04:00.

Use automatic rebooting only when there is a documented operational reason and an appropriate maintenance window.

For most production routers, continuous operation with proper monitoring is preferable to unnecessary scheduled reboots.

# 🗁 Managing RouterOS Logs

System logs provide information about events and activities occurring inside the router.

Display logs:

```routeros
/log print
```

Display the most recent entries:

```routeros
/log print without-paging
```

Clear the memory log:

```routeros
/log remove [find]
```

**Important:** Clearing logs removes useful diagnostic information. Export or review relevant entries before deleting them.

RouterOS supports different logging topics, including:

* `system`
* `critical`
* `error`
* `warning`
* `info`
* `firewall`
* `account`
* `interface`
* `wireless`
* `dhcp`
* `ppp`
* `routing`

The exact available topics depend on RouterOS version and enabled features.

Logging configuration and remote logging will be covered in more detail in the dedicated logging documentation.

# 🗁 Checking Active Connections

Connection tracking maintains information about active network connections.

Display tracked connections:

```routeros
/ip firewall connection print
```

Display the number of tracked connections:

```routeros
/ip firewall connection print count-only
```

Inspecting connection tracking can help diagnose:

* High connection counts.
* Unexpected connection activity.
* NAT problems.
* Firewall behavior.
* Excessive connection creation.

Do not remove all connection-tracking entries as a routine maintenance operation. Doing so can interrupt active network sessions.

# 🗁 Monitoring Active Users and Sessions

Administrators should regularly review active management sessions and connected users.

Display active RouterOS users:

```routeros
/user active print
```

Display configured user accounts:

```routeros
/user print
```

Review:

* Unexpected management sessions.
* Unused accounts.
* Excessive administrative privileges.
* Unknown login activity.

Disable unused accounts and use individual administrator accounts rather than sharing credentials.

# 🗁 Safe Mode for Configuration Maintenance

Safe Mode is useful when making changes that could interrupt remote management access.

It allows RouterOS to automatically revert changes if the management session is unexpectedly disconnected before the changes are committed.

Enter Safe Mode in the terminal using:

```routeros
Ctrl + X
```

RouterOS indicates when Safe Mode is active.

Make the required configuration changes.

If the session remains connected and the changes are correct, press `Ctrl + X` again to leave Safe Mode and commit the changes.

If the session unexpectedly terminates while Safe Mode is active, RouterOS attempts to undo the changes made during that Safe Mode session.

Safe Mode is especially useful when modifying:

* Firewall rules.
* IP addresses.
* Routing configuration.
* Bridge settings.
* VLAN configuration.
* Remote management access.

**Important:** Safe Mode has limitations and is not a replacement for configuration backups or a recovery plan.

# 🗁 Performing Routine Maintenance

A structured maintenance routine helps administrators keep routers healthy and secure.

**Daily checks**

* Check router availability.
* Review critical system logs.
* Monitor CPU and memory usage.
* Check interface status.
* Verify important network services.

**Weekly checks**

* Review system resource utilization.
* Inspect interface errors and traffic.
* Check available storage.
* Review active users and sessions.
* Verify backup creation.
* Check important network connectivity.

**Monthly checks**

* Review RouterOS versions.
* Check RouterBOARD firmware.
* Review firewall and management service configuration.
* Inspect hardware condition.
* Verify NTP synchronization.
* Test backup restoration in a lab.
* Review scheduled tasks and scripts.

**Before major configuration changes**

* Create a binary backup.
* Export the current configuration.
* Download backups to external storage.
* Review the change plan.
* Use Safe Mode where appropriate.
* Prepare a recovery method.

# 🗁 Practical Lab: RouterOS System Maintenance

This lab demonstrates basic system maintenance using a MikroTik CHR instance.

**Lab requirements:**

* MikroTik CHR.
* WinBox or SSH access.
* Basic knowledge of RouterOS CLI.

**Inspect system information**

```routeros
/system resource print
/system identity print
/system routerboard print
```

**Monitor CPU and memory**

```routeros
/system resource monitor
/tool profile
```

**Inspect interfaces**

```routeros
/interface print
/interface print stats
```

**Inspect storage**

```routeros
/file print
/disk print
```

**Check time synchronization**

```routeros
/system clock print
/system ntp client print
```

**Review management services**

```routeros
/ip service print
```

**Review active sessions**

```routeros
/user active print
```

**Create a maintenance backup**

```routeros
/system backup save name=maintenance-backup
/export file=maintenance-export
```

**Lab objectives:**

* Inspect RouterOS system information.
* Monitor CPU and memory usage.
* Check interface status and statistics.
* Inspect storage and installed packages.
* Verify system time.
* Review management services and active users.
* Create maintenance backups.

# 🗁 System Maintenance Best Practices

* Maintain an accurate inventory of routers and hardware.
* Use meaningful system identities.
* Monitor CPU, memory, temperature, and storage.
* Keep system clocks synchronized.
* Review logs regularly.
* Disable unused management services.
* Restrict administrative access to trusted networks.
* Maintain individual administrator accounts.
* Create regular configuration backups.
* Store backup copies outside the router.
* Test restoration procedures.
* Review scheduled scripts and tasks.
* Use Safe Mode for risky remote configuration changes.
* Keep RouterOS and firmware versions documented.
* Maintain a maintenance history for production devices.

# 🗁 Official Documentation

* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [MikroTik RouterOS Scripting](https://help.mikrotik.com/docs/)
* [MikroTik RouterOS System Resource Documentation](https://help.mikrotik.com/docs/)
