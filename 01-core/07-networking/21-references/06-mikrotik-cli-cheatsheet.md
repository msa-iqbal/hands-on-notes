# MikroTik CLI Cheatsheet

MikroTik RouterOS provides a command-line interface (CLI) for configuring, managing, monitoring, and troubleshooting MikroTik routers.

The CLI is available through:

* WinBox Terminal.
* WebFig Terminal.
* SSH.
* Serial console.
* Local terminal on supported hardware.

This cheatsheet focuses on **MikroTik RouterOS v7** and provides frequently used commands for network administrators, system engineers, and ISP technicians.

The examples use common interface names:

| Interface | Purpose |
| --- | --- |
| `ether1` | WAN connection |
| `ether2` | LAN connection |
| `bridge` | LAN bridge |
| `vlan10` | VLAN 10 |
| `vlan20` | VLAN 20 |

Actual interface names depend on the router model and configuration.

# 🗁 RouterOS CLI Fundamentals

### 🖆 RouterOS Command Structure

RouterOS commands are organized hierarchically.

Example:

```routeros
/ip address print
```

Command components:

* `/ip` — IP configuration menu.
* `address` — IP address submenu.
* `print` — Display configured addresses.

Another example:

```routeros
/interface ethernet monitor ether1 once
```

This displays Ethernet link information for `ether1` once.

### 🖆 Display Available Commands

Display commands in the current menu:

```routeros
?
```

Display available commands under a menu:

```routeros
/ip ?
```

Display available properties:

```routeros
/ip address add ?
```

### 🖆 Print Configuration

Display configured objects:

```routeros
/ip address print
```

Display detailed information:

```routeros
/ip address print detail
```

Display configuration without pagination:

```routeros
/ip address print without-paging
```

### 🖆 Add, Set, Remove, and Disable

Add a configuration item:

```routeros
/ip address add address=192.168.1.1/24 interface=bridge
```

Modify an existing item:

```routeros
/ip address set [find address="192.168.1.1/24"] comment="LAN Gateway"
```

Remove an item:

```routeros
/ip address remove [find address="192.168.1.1/24"]
```

Disable an item:

```routeros
/ip firewall filter disable [find comment="Block test traffic"]
```

Enable an item:

```routeros
/ip firewall filter enable [find comment="Block test traffic"]
```

Using `find` is generally more reliable than assuming an item's position in the configuration.

### 🖆 Comments

Add a comment when creating an item:

```routeros
/ip address add address=192.168.1.1/24 interface=bridge comment="LAN Gateway"
```

Modify a comment:

```routeros
/ip address set [find address="192.168.1.1/24"] comment="Office LAN Gateway"
```

Comments make configurations easier to understand and maintain.

# 🗁 System Information Commands

### 🖆 Display System Resources

```routeros
/system resource print
```

Displays:

* CPU architecture.
* CPU model.
* CPU utilization.
* Free and total memory.
* Free and total storage.
* RouterOS version.
* Uptime.

### 🖆 Display Router Identity

```routeros
/system identity print
```

Change the router identity:

```routeros
/system identity set name=Main-Router
```

### 🖆 Display RouterOS Version

```routeros
/system package print
```

### 🖆 Display System Clock

```routeros
/system clock print
```

Configure the time zone:

```routeros
/system clock set time-zone-name=Asia/Dhaka
```

### 🖆 Display System Health

```routeros
/system health print
```

Hardware support determines which health sensors are available.

### 🖆 Display Installed Packages

```routeros
/system package print
```

### 🖆 Reboot and Shutdown

Reboot the router:

```routeros
/system reboot
```

Shut down the router:

```routeros
/system shutdown
```

These commands interrupt network services. Use them only when the operational impact is understood.

# 🗁 Interface Management

### 🖆 Display Interfaces

Display all interfaces:

```routeros
/interface print
```

Display detailed information:

```routeros
/interface print detail
```

Display Ethernet interfaces:

```routeros
/interface ethernet print
```

### 🖆 Enable and Disable Interfaces

Enable an interface:

```routeros
/interface enable ether1
```

Disable an interface:

```routeros
/interface disable ether1
```

### 🖆 Rename an Interface

```routeros
/interface set ether1 name=WAN
```

Another example:

```routeros
/interface set ether2 name=LAN
```

### 🖆 Monitor Ethernet Link Status

Display Ethernet information once:

```routeros
/interface ethernet monitor ether1 once
```

Continuously monitor Ethernet information:

```routeros
/interface ethernet monitor ether1
```

### 🖆 Monitor Interface Traffic

```routeros
/interface monitor-traffic ether1
```

Monitor multiple interfaces:

```routeros
/interface monitor-traffic ether1,ether2
```

### 🖆 Display Interface Statistics

```routeros
/interface print stats
```

### 🖆 Configure Interface Comment

```routeros
/interface set ether1 comment="Primary Internet Connection"
```

# 🗁 Bridge Management

### 🖆 Create a Bridge

```routeros
/interface bridge add name=bridge
```

### 🖆 Add Ports to a Bridge

```routeros
/interface bridge port add bridge=bridge interface=ether2
/interface bridge port add bridge=bridge interface=ether3
```

### 🖆 Display Bridges

```routeros
/interface bridge print
```

### 🖆 Display Bridge Ports

```routeros
/interface bridge port print
```

### 🖆 Display Bridge Host Table

```routeros
/interface bridge host print
```

This table displays learned Layer 2 MAC addresses and their associated bridge ports.

### 🖆 Enable RSTP

```routeros
/interface bridge set bridge protocol-mode=rstp
```

### 🖆 Display Bridge Information

```routeros
/interface bridge monitor bridge once
```

# 🗁 VLAN Configuration

### 🖆 Create VLAN Interfaces

Create VLAN 10:

```routeros
/interface vlan add name=vlan10 vlan-id=10 interface=bridge
```

Create VLAN 20:

```routeros
/interface vlan add name=vlan20 vlan-id=20 interface=bridge
```

### 🖆 Assign IP Addresses to VLAN Interfaces

```routeros
/ip address add address=192.168.10.1/24 interface=vlan10
/ip address add address=192.168.20.1/24 interface=vlan20
```

### 🖆 Display VLAN Interfaces

```routeros
/interface vlan print
```

### 🖆 Configure Bridge VLAN Filtering

Enable VLAN filtering:

```routeros
/interface bridge set bridge vlan-filtering=yes
```

For a complete VLAN-aware bridge configuration, configure:

* Bridge VLAN table.
* Tagged ports.
* Untagged access ports.
* PVID values.
* Appropriate CPU-facing bridge membership.
* Management access.

Example VLAN membership:

```routeros
/interface bridge vlan
add bridge=bridge vlan-ids=10 tagged=bridge,ether2
add bridge=bridge vlan-ids=20 tagged=bridge,ether2
```

This example assumes `ether2` is a trunk carrying tagged VLAN 10 and VLAN 20. Access ports and PVIDs must be configured separately according to the topology.

### 🖆 Display Bridge VLAN Configuration

```routeros
/interface bridge vlan print
```

Display bridge port VLAN settings:

```routeros
/interface bridge port print detail
```

# 🗁 IP Address Management

### 🖆 Display IP Addresses

```routeros
/ip address print
```

Display detailed information:

```routeros
/ip address print detail
```

### 🖆 Add an IP Address

```routeros
/ip address add address=192.168.1.1/24 interface=bridge
```

### 🖆 Add a WAN Address

```routeros
/ip address add address=203.0.113.2/30 interface=ether1
```

`203.0.113.0/24` is a documentation-only IPv4 range. Replace it with the actual address provided by the ISP.

### 🖆 Remove an IP Address

```routeros
/ip address remove [find address="192.168.1.1/24"]
```

### 🖆 Display ARP Entries

```routeros
/ip arp print
```

### 🖆 Display IPv6 Addresses

```routeros
/ipv6 address print
```

# 🗁 DHCP Client and Server

### 🖆 Configure a DHCP Client

Add a DHCP client to the WAN interface:

```routeros
/ip dhcp-client add interface=ether1 disabled=no
```

Display DHCP clients:

```routeros
/ip dhcp-client print detail
```

Release and renew a DHCP lease by disabling and re-enabling the client:

```routeros
/ip dhcp-client disable [find interface=ether1]
/ip dhcp-client enable [find interface=ether1]
```

### 🖆 Create a DHCP Address Pool

```routeros
/ip pool add name=dhcp-pool ranges=192.168.10.10-192.168.10.254
```

### 🖆 Create a DHCP Server

For a basic DHCP server on a LAN bridge:

```routeros
/ip dhcp-server add name=dhcp-lan interface=bridge address-pool=dhcp-pool disabled=no
```

### 🖆 Configure the DHCP Network

```routeros
/ip dhcp-server network add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

### 🖆 Display DHCP Servers

```routeros
/ip dhcp-server print
```

### 🖆 Display DHCP Leases

```routeros
/ip dhcp-server lease print
```

Display detailed leases:

```routeros
/ip dhcp-server lease print detail
```

### 🖆 Make a DHCP Lease Static

```routeros
/ip dhcp-server lease make-static [find mac-address="AA:BB:CC:DD:EE:FF"]
```

### 🖆 Remove a DHCP Lease

```routeros
/ip dhcp-server lease remove [find mac-address="AA:BB:CC:DD:EE:FF"]
```

# 🗁 DNS Configuration

### 🖆 Display DNS Configuration

```routeros
/ip dns print
```

### 🖆 Configure DNS Servers

```routeros
/ip dns set servers=1.1.1.1,8.8.8.8
```

### 🖆 Enable DNS Service for LAN Clients

```routeros
/ip dns set allow-remote-requests=yes
```

Restrict DNS access to trusted LAN interfaces using firewall rules. Do not expose an unrestricted recursive DNS resolver to the public Internet.

### 🖆 Display DNS Cache

```routeros
/ip dns cache print
```

### 🖆 Flush DNS Cache

```routeros
/ip dns cache flush
```

### 🖆 Configure Static DNS Records

```routeros
/ip dns static add name=router.local address=192.168.10.1
```

Display static DNS records:

```routeros
/ip dns static print
```

# 🗁 Routing Commands

### 🖆 Display Routing Table

```routeros
/ip route print
```

Display detailed routing information:

```routeros
/ip route print detail
```

### 🖆 Add a Static Route

```routeros
/ip route add dst-address=192.168.20.0/24 gateway=192.168.10.2
```

### 🖆 Configure a Default Route

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.10.254
```

### 🖆 Configure Route Distance

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.10.254 distance=1
```

### 🖆 Remove a Route

```routeros
/ip route remove [find dst-address=192.168.20.0/24]
```

### 🖆 Display IPv6 Routes

```routeros
/ipv6 route print detail
```

### 🖆 Inspect OSPF Configuration

RouterOS v7 uses the routing OSPF configuration menus:

```routeros
/routing ospf instance print detail
/routing ospf area print detail
/routing ospf interface-template print detail
```

Display OSPF neighbors:

```routeros
/routing ospf neighbor print detail
```

### 🖆 Inspect BGP Configuration

```routeros
/routing bgp connection print detail
```

Display BGP sessions:

```routeros
/routing bgp session print detail
```

Display routing filters:

```routeros
/routing filter rule print detail
```

Routing configuration depends on the RouterOS v7 release and network design. Verify the supported properties on the router using `?`.

# 🗁 Firewall Filter Commands

### 🖆 Display Firewall Rules

Display IPv4 filter rules:

```routeros
/ip firewall filter print
```

Display detailed rules:

```routeros
/ip firewall filter print detail
```

### 🖆 Add an Input Firewall Rule

Allow established and related connections:

```routeros
/ip firewall filter add chain=input action=accept connection-state=established,related
```

Drop invalid connections:

```routeros
/ip firewall filter add chain=input action=drop connection-state=invalid
```

Allow management access from a trusted LAN:

```routeros
/ip firewall filter add chain=input action=accept in-interface=bridge src-address=192.168.10.0/24
```

Drop other input traffic:

```routeros
/ip firewall filter add chain=input action=drop
```

Rule order is important. These rules must be placed in an appropriate order, and required management, VPN, and service access must be explicitly considered before applying a final drop rule.

### 🖆 Add a Forward Firewall Rule

Allow established and related forwarded connections:

```routeros
/ip firewall filter add chain=forward action=accept connection-state=established,related
```

Drop invalid forwarded connections:

```routeros
/ip firewall filter add chain=forward action=drop connection-state=invalid
```

Drop traffic from a blocked subnet:

```routeros
/ip firewall filter add chain=forward action=drop src-address=192.168.50.0/24
```

### 🖆 Disable or Enable Firewall Rules

Disable a rule:

```routeros
/ip firewall filter disable [find comment="Block test traffic"]
```

Enable a rule:

```routeros
/ip firewall filter enable [find comment="Block test traffic"]
```

### 🖆 Display Firewall Rule Counters

```routeros
/ip firewall filter print stats
```

Counters help identify whether traffic matches a rule.

# 🗁 NAT Commands

### 🖆 Display NAT Rules

```routeros
/ip firewall nat print
```

Display detailed NAT configuration:

```routeros
/ip firewall nat print detail
```

### 🖆 Configure Masquerade

A common masquerade rule for an interface list named `WAN`:

```routeros
/ip firewall nat add chain=srcnat out-interface-list=WAN action=masquerade
```

The `WAN` interface list must exist and contain the appropriate WAN interfaces.

### 🖆 Configure Source NAT

```routeros
/ip firewall nat add chain=srcnat src-address=192.168.10.0/24 out-interface=ether1 action=src-nat to-addresses=203.0.113.2
```

This example uses a documentation-only public address. Replace it with the actual address assigned to the router.

### 🖆 Configure Destination NAT

Forward external TCP port 8080 to an internal web server:

```routeros
/ip firewall nat add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.100 to-ports=80
```

A corresponding forward-chain firewall policy must permit the forwarded traffic.

### 🖆 Display NAT Statistics

```routeros
/ip firewall nat print stats
```

# 🗁 Address Lists and Connection Tracking

### 🖆 Create an Address List

```routeros
/ip firewall address-list add list=blocked-users address=192.168.10.50
```

### 🖆 Display Address Lists

```routeros
/ip firewall address-list print
```

### 🖆 Remove an Address List Entry

```routeros
/ip firewall address-list remove [find list=blocked-users address=192.168.10.50]
```

### 🖆 Display Connection Tracking

```routeros
/ip firewall connection print
```

Display detailed connections:

```routeros
/ip firewall connection print detail
```

Display connection tracking statistics:

```routeros
/ip firewall connection tracking print
```

# 🗁 Wireless Commands

Wireless command availability depends on the installed RouterOS wireless package and device architecture.

### 🖆 Legacy Wireless Package

Display wireless interfaces:

```routeros
/interface wireless print
```

Display wireless registration table:

```routeros
/interface wireless registration-table print
```

Display wireless security profiles:

```routeros
/interface wireless security-profiles print
```

### 🖆 New WiFi Configuration

Display WiFi interfaces:

```routeros
/interface wifi print
```

Display WiFi registration table:

```routeros
/interface wifi registration-table print
```

Display WiFi configuration:

```routeros
/interface wifi configuration print detail
```

The newer WiFi menu is not available on every RouterOS device. Use the menu corresponding to the installed wireless package.

# 🗁 PPPoE Commands

### 🖆 Display PPP Profiles

```routeros
/ppp profile print
```

### 🖆 Create a PPP Profile

```routeros
/ppp profile add name=10Mbps rate-limit=10M/10M
```

The `rate-limit` format is upload/download from the perspective of the PPP client. Confirm the intended direction when designing asymmetric bandwidth plans.

### 🖆 Display PPP Secrets

```routeros
/ppp secret print
```

### 🖆 Create a PPPoE User

```routeros
/ppp secret add name=customer01 password=StrongPassword service=pppoe profile=10Mbps
```

### 🖆 Display Active PPP Sessions

```routeros
/ppp active print
```

### 🖆 Disconnect a PPP Session

```routeros
/ppp active remove [find name=customer01]
```

### 🖆 Display PPPoE Server Configuration

```routeros
/interface pppoe-server server print
```

# 🗁 Hotspot Commands

### 🖆 Display Hotspot Servers

```routeros
/ip hotspot print
```

### 🖆 Display Hotspot Users

```routeros
/ip hotspot user print
```

### 🖆 Create a Hotspot User

```routeros
/ip hotspot user add name=user01 password=StrongPassword
```

### 🖆 Display Active Hotspot Sessions

```routeros
/ip hotspot active print
```

### 🖆 Disconnect a Hotspot User

```routeros
/ip hotspot active remove [find user=user01]
```

### 🖆 Display Hotspot Profiles

```routeros
/ip hotspot user profile print
```

# 🗁 Queue and Bandwidth Management

### 🖆 Display Simple Queues

```routeros
/queue simple print
```

### 🖆 Create a Simple Queue

```routeros
/queue simple add name=customer01 target=192.168.10.50/32 max-limit=10M/10M
```

### 🖆 Configure Queue Limits

```routeros
/queue simple set [find name=customer01] max-limit=20M/10M
```

The `max-limit` format is upload/download.

### 🖆 Display Queue Statistics

```routeros
/queue simple print stats
```

### 🖆 Display Queue Trees

```routeros
/queue tree print
```

### 🖆 Display Queue Types

```routeros
/queue type print
```

# 🗁 VPN Commands

### 🖆 WireGuard

Display WireGuard interfaces:

```routeros
/interface wireguard print
```

Display WireGuard peers:

```routeros
/interface wireguard peers print detail
```

Display WireGuard interface details:

```routeros
/interface wireguard print detail
```

Create a WireGuard interface:

```routeros
/interface wireguard add name=wg-office listen-port=13231
```

Display WireGuard addresses:

```routeros
/ip address print where interface=wg-office
```

### 🖆 IPsec

Display IPsec peers:

```routeros
/ip ipsec peer print detail
```

Display IPsec identities:

```routeros
/ip ipsec identity print detail
```

Display IPsec policies:

```routeros
/ip ipsec policy print detail
```

Display active IPsec installed SAs:

```routeros
/ip ipsec installed-sa print
```

### 🖆 L2TP

Display L2TP client interfaces:

```routeros
/interface l2tp-client print
```

Display L2TP server configuration:

```routeros
/interface l2tp-server server print
```

### 🖆 OpenVPN

Display OpenVPN client interfaces:

```routeros
/interface ovpn-client print
```

Display OpenVPN server configuration:

```routeros
/interface ovpn-server server print
```

Available VPN features and properties depend on the RouterOS version and configuration.

# 🗁 Monitoring and Troubleshooting

### 🖆 Ping

```routeros
/ping 8.8.8.8
```

Specify a source address:

```routeros
/ping 8.8.8.8 src-address=192.168.10.1
```

### 🖆 Traceroute

```routeros
/tool traceroute 8.8.8.8
```

### 🖆 Torch

Monitor real-time traffic:

```routeros
/tool torch ether1
```

Filter by protocol:

```routeros
/tool torch ether1 protocol=tcp
```

### 🖆 Packet Sniffer

Configure the sniffer interface:

```routeros
/tool sniffer set interface=ether1
```

Start packet capture:

```routeros
/tool sniffer start
```

Stop packet capture:

```routeros
/tool sniffer stop
```

Display sniffer settings:

```routeros
/tool sniffer print
```

### 🖆 System Logs

Display logs:

```routeros
/log print
```

Display logging configuration:

```routeros
/system logging print
```

### 🖆 Monitor CPU Usage

```routeros
/system resource print
```

Display CPU profiling:

```routeros
/tool profile
```

### 🖆 Monitor Netwatch

Display Netwatch entries:

```routeros
/tool netwatch print detail
```

Netwatch can monitor the reachability of configured hosts and trigger actions when their status changes.

### 🖆 Display Neighbor Discovery

Display discovered neighbors:

```routeros
/ip neighbor print
```

Display IPv6 neighbors:

```routeros
/ipv6 neighbor print
```

# 🗁 Backup and Configuration Export

### 🖆 Create a Binary Backup

```routeros
/system backup save name=router-backup
```

Binary backups are intended primarily for restoring compatible RouterOS configurations. Store them securely.

### 🖆 Export Configuration

```routeros
/export file=router-config
```

### 🖆 Display Configuration in Terminal

```routeros
/export
```

### 🖆 Export Sensitive Configuration

```routeros
/export show-sensitive file=router-config-sensitive
```

Sensitive exports may contain credentials, keys, and other confidential values. Protect these files appropriately.

### 🖆 Restore a Binary Backup

```routeros
/system backup load name=router-backup.backup
```

Restoring a backup may reboot the router and replace its configuration. Verify device compatibility and ensure you have recovery access before restoring.

# 🗁 User and Access Management

### 🖆 Display Router Users

```routeros
/user print
```

### 🖆 Create a User

```routeros
/user add name=network-admin group=full password=StrongPassword
```

Use a strong, unique password and appropriate access privileges.

### 🖆 Change User Password

```routeros
/user set [find name=network-admin] password=NewStrongPassword
```

### 🖆 Disable a User

```routeros
/user disable [find name=old-admin]
```

### 🖆 Display Management Services

```routeros
/ip service print
```

### 🖆 Disable Unused Management Services

For example, disable Telnet:

```routeros
/ip service disable telnet
```

Management services should be restricted to trusted management networks. Disabling a service without verifying alternative access can lock out administrators.

# 🗁 Useful CLI Shortcuts

### 🖆 Tab Completion

Press `Tab` to complete a command or property name.

Example:

```routeros
/interface eth<Tab>
```

### 🖆 Command Help

Use `?` to inspect available commands and parameters:

```routeros
/ip route add ?
```

### 🖆 Previous Commands

Use the keyboard's Up and Down arrow keys to navigate command history.

### 🖆 Find Configuration Items

Find an interface:

```routeros
/interface find name=ether1
```

Find an IP address:

```routeros
/ip address find address="192.168.10.1/24"
```

Find a firewall rule by comment:

```routeros
/ip firewall filter find comment="Allow LAN"
```

### 🖆 Use Safe Mode

Safe Mode helps protect against accidental configuration changes that disconnect the management session.

Enter Safe Mode:

```text
Ctrl + X
```

Press `Ctrl + X` again to leave Safe Mode and commit the changes.

If the session terminates unexpectedly while Safe Mode is active, RouterOS can roll back changes made during that Safe Mode session.

# 🗁 Quick Reference

### 🖆 System and Interface

| Purpose | Command |
| --- | --- |
| System resources | `/system resource print` |
| Router identity | `/system identity print` |
| RouterOS packages | `/system package print` |
| System clock | `/system clock print` |
| Interfaces | `/interface print` |
| Interface statistics | `/interface print stats` |
| Monitor interface | `/interface monitor-traffic ether1` |
| Ethernet link | `/interface ethernet monitor ether1 once` |

### 🖆 IP and Routing

| Purpose | Command |
| --- | --- |
| IP addresses | `/ip address print` |
| ARP table | `/ip arp print` |
| IPv6 addresses | `/ipv6 address print` |
| IPv4 routes | `/ip route print detail` |
| IPv6 routes | `/ipv6 route print detail` |
| Ping | `/ping 8.8.8.8` |
| Traceroute | `/tool traceroute 8.8.8.8` |
| DNS settings | `/ip dns print` |
| DNS cache | `/ip dns cache print` |

### 🖆 DHCP and VLAN

| Purpose | Command |
| --- | --- |
| DHCP clients | `/ip dhcp-client print` |
| DHCP servers | `/ip dhcp-server print` |
| DHCP leases | `/ip dhcp-server lease print` |
| DHCP pools | `/ip pool print` |
| Bridge | `/interface bridge print` |
| Bridge ports | `/interface bridge port print` |
| VLAN interfaces | `/interface vlan print` |
| Bridge VLAN table | `/interface bridge vlan print` |

### 🖆 Firewall and NAT

| Purpose | Command |
| --- | --- |
| Firewall filters | `/ip firewall filter print` |
| NAT rules | `/ip firewall nat print` |
| Mangle rules | `/ip firewall mangle print` |
| Address lists | `/ip firewall address-list print` |
| Connection tracking | `/ip firewall connection print` |
| Firewall statistics | `/ip firewall filter print stats` |

### 🖆 ISP Services and Monitoring

| Purpose | Command |
| --- | --- |
| PPP profiles | `/ppp profile print` |
| PPP users | `/ppp secret print` |
| Active PPP sessions | `/ppp active print` |
| Hotspot users | `/ip hotspot user print` |
| Active Hotspot sessions | `/ip hotspot active print` |
| Simple queues | `/queue simple print` |
| Queue statistics | `/queue simple print stats` |
| Torch | `/tool torch ether1` |
| System logs | `/log print` |
| CPU profiling | `/tool profile` |
| Backup | `/system backup save name=router-backup` |
| Configuration export | `/export file=router-config` |

# 🗁 Best Practices

* Use RouterOS v7 documentation when configuring routing, wireless, VPN, and firewall features.
* Use descriptive interface names and configuration comments.
* Keep a backup before making significant changes.
* Use Safe Mode when remotely changing management or firewall configurations.
* Verify firewall rule order before applying restrictive policies.
* Restrict management services to trusted networks.
* Monitor CPU, memory, interface traffic, and connection tracking.
* Use `print detail` to inspect configuration properties.
* Prefer `find` expressions over hard-coded item numbers in scripts.
* Test changes in a lab before applying them to production routers.
* Keep sensitive exports and backups protected.
* Document network topology, IP addressing, VLAN IDs, and routing decisions.

The RouterOS CLI is a powerful administration interface. Understanding its command hierarchy and configuration model makes MikroTik network management faster, more consistent, and easier to automate.
