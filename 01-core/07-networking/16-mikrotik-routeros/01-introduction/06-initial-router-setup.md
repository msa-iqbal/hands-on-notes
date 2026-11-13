# Initial Router Setup

Initial router setup is the process of preparing a MikroTik router for basic network operation and administration.

A newly installed RouterOS device needs an initial configuration before it can safely operate in a network.

The initial setup typically includes:

* Accessing the router for the first time.
* Configuring administrative credentials.
* Identifying physical interfaces.
* Configuring LAN and WAN interfaces.
* Assigning IP addresses.
* Configuring a default gateway.
* Setting up DNS.
* Configuring a DHCP server for LAN clients.
* Configuring NAT for Internet access.
* Securing management access.
* Verifying network connectivity.

This document uses RouterOS v7 CLI commands. The examples assume a simple router with one WAN interface and one LAN interface.

# 🗁 Default Router Access

MikroTik devices may have a factory-default configuration depending on the model and RouterOS version.

Common default configuration details include:

| Setting | Typical value |
| --- | --- |
| Default LAN IP | `192.168.88.1/24` |
| Default LAN interface | Often `ether2` or a LAN bridge |
| Default username | `admin` |
| Default password | May be empty on older devices; newer devices may require the password printed on the device or supplied during initial setup |
| Management method | WinBox, WebFig, or CLI |

These values are not universal. Always check the device label, model documentation, or current configuration.

**Connecting for the first time**

You can connect to a MikroTik router using:

* WinBox through an IP address.
* WinBox through a MAC address.
* WebFig through a browser.
* SSH if IP connectivity and SSH service are available.
* Serial console on supported devices.

For initial setup, WinBox MAC-based access is useful when the router does not yet have a working IP configuration.

# 🗁 Inspecting the Default Configuration

Before making changes, inspect the existing configuration.

This helps you understand what is already configured and avoid duplicating or conflicting settings.

Display the system identity:

```routeros
/system identity print
```

Display RouterOS version and system information:

```routeros
/system resource print
```

Display available interfaces:

```routeros
/interface print
```

Display IP addresses:

```routeros
/ip address print
```

Display configured DHCP servers:

```routeros
/ip dhcp-server print
```

Display DHCP clients:

```routeros
/ip dhcp-client print
```

Display NAT rules:

```routeros
/ip firewall nat print
```

Display firewall filter rules:

```routeros
/ip firewall filter print
```

Display the routing table:

```routeros
/ip route print
```

Display management services:

```routeros
/ip service print
```

**Exporting the existing configuration**

Before making major changes, save a readable configuration export:

```routeros
/export file=before-initial-setup
```

The file can be viewed or downloaded through WinBox or another supported file-management method.

An export is useful for reviewing and reproducing configuration, but it is not a complete substitute for a binary backup.

# 🗁 Setting the Router Identity

The system identity is the name assigned to a RouterOS device.

A meaningful identity makes routers easier to identify in WinBox, monitoring systems, logs, and network documentation.

Set the identity:

```routeros
/system identity set name=CORE-RTR-01
```

Display the current identity:

```routeros
/system identity print
```

Examples of useful naming conventions:

| Device role | Example identity |
| --- | --- |
| Main gateway | `MAIN-GW-01` |
| Core router | `CORE-RTR-01` |
| Distribution router | `DIST-RTR-01` |
| Branch router | `BRANCH-01` |
| Lab router | `LAB-CHR-01` |
| Customer router | `CUST-RTR-001` |

Use consistent naming across your network.

# 🗁 Configuring the Administrative Password

A strong administrative password is one of the first security requirements for a new router.

Set the password for the current administrative account:

```routeros
/user set [find name=admin] password="Your-Strong-Password"
```

Use a unique password that is difficult to guess.

Display configured users:

```routeros
/user print
```

For production environments, consider creating a separate named administrator account and testing it before disabling or removing access to the original account.

Example:

```routeros
/user add name=netadmin group=full password="Another-Strong-Password"
```

After creating the new account:

1. Open a separate management session.
2. Log in using the new account.
3. Verify that the required administrative commands work.
4. Only then decide whether to disable or remove the default account.

Do not remove your only working administrative account.

# 🗁 Identifying Network Interfaces

RouterOS interfaces represent physical or virtual network connections.

A typical MikroTik router may have interfaces such as:

| Interface | Typical purpose |
| --- | --- |
| `ether1` | WAN connection |
| `ether2` | LAN connection |
| `ether3` | Additional LAN or network segment |
| `wlan1` | Wireless interface, if supported |
| `bridge` | Logical Layer 2 interface |
| `vlan10` | VLAN interface |

The actual interface names and available hardware depend on the device.

Display interfaces:

```routeros
/interface print
```

Display detailed interface information:

```routeros
/interface ethernet print detail
```

**Renaming interfaces**

Descriptive interface names make configurations easier to understand.

For example:

```routeros
/interface ethernet
set [find default-name=ether1] name=WAN
set [find default-name=ether2] name=LAN-PORT
```

Verify the result:

```routeros
/interface print
```

Interface renaming does not change the physical port. It changes the name used to reference that interface in RouterOS configuration.

# 🗁 Basic Network Design

Before configuring IP addresses, define the intended network topology.

Consider a simple network:

```text
                  Internet
                     |
                ISP Gateway
                     |
                  ether1
                     |
              MikroTik Router
                     |
                  ether2
                     |
                 LAN Switch
                     |
              ----------------
              |              |
           Client 1       Client 2
```

In this example:

* `ether1` connects to the ISP.
* `ether2` connects to the internal LAN.
* The router provides gateway, DHCP, DNS forwarding, and NAT services.
* LAN clients use the MikroTik router as their default gateway.

**Example IP addressing plan**

| Component | Interface | IP address |
| --- | --- | --- |
| WAN | `WAN` | Obtained from ISP through DHCP |
| Router LAN | `LAN-PORT` | `192.168.10.1/24` |
| DHCP pool | LAN clients | `192.168.10.100-192.168.10.200` |
| Default gateway for clients | LAN | `192.168.10.1` |
| DNS for clients | LAN | `192.168.10.1` |

The addressing plan should avoid conflicts with upstream networks and other local subnets.

# 🗁 Configuring the WAN Interface

The WAN interface connects the router to an upstream network or Internet service provider.

The WAN configuration depends on how the ISP provides connectivity.

Common WAN connection types include:

* DHCP client.
* Static IP address.
* PPPoE client.
* VLAN-based Internet connection.
* Other provider-specific configurations.

**WAN using DHCP**

If the ISP provides an IP address automatically through DHCP, configure a DHCP client on the WAN interface.

```routeros
/ip dhcp-client
add interface=WAN disabled=no add-default-route=yes use-peer-dns=no
```

Check the DHCP client status:

```routeros
/ip dhcp-client print detail
```

A successful DHCP client should obtain an IP address and usually a default route, depending on the configuration.

Check the WAN address:

```routeros
/ip address print
```

Check the default route:

```routeros
/ip route print
```

The ISP may require a specific VLAN, MAC address, or other settings before DHCP works.

**WAN using a static IP address**

If the ISP provides static addressing, configure the assigned IP address and gateway.

Example addressing information:

| Parameter | Example |
| --- | --- |
| WAN IP | `203.0.113.2/30` |
| Gateway | `203.0.113.1` |
| DNS | `1.1.1.1` |

The addresses in this example are documentation-only addresses and are not usable public Internet addresses.

Assign the WAN address:

```routeros
/ip address
add address=203.0.113.2/30 interface=WAN
```

Configure the default route:

```routeros
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1
```

Use the exact IP address, prefix length, and gateway supplied by your ISP.

**WAN using PPPoE**

Some ISPs provide Internet access through PPPoE authentication.

A basic PPPoE client configuration:

```routeros
/interface pppoe-client
add name=pppoe-out1 interface=WAN user="ISP-USERNAME" password="ISP-PASSWORD" add-default-route=yes use-peer-dns=no disabled=no
```

Check the PPPoE connection:

```routeros
/interface pppoe-client print detail
```

The PPPoE interface should establish a session and obtain the assigned connection details.

The ISP may require VLAN tagging or other provider-specific settings before PPPoE authentication can succeed.

# 🗁 Configuring the LAN Interface

The LAN interface connects internal devices to the router.

For a simple setup, assign a private IP address to the LAN interface.

Example:

```routeros
/ip address
add address=192.168.10.1/24 interface=LAN-PORT
```

Verify the address:

```routeros
/ip address print
```

The router's LAN address becomes the default gateway for devices in the `192.168.10.0/24` network.

**Using a bridge for multiple LAN ports**

If several Ethernet ports should belong to the same Layer 2 LAN, create a bridge and add the required ports.

Create a bridge:

```routeros
/interface bridge
add name=LAN-BRIDGE
```

Add LAN ports:

```routeros
/interface bridge port
add bridge=LAN-BRIDGE interface=LAN-PORT
add bridge=LAN-BRIDGE interface=ether3
add bridge=LAN-BRIDGE interface=ether4
```

Assign the LAN IP address to the bridge:

```routeros
/ip address
add address=192.168.10.1/24 interface=LAN-BRIDGE
```

When ports are members of a bridge, the Layer 3 LAN address normally belongs on the bridge rather than on an individual bridged physical port.

Avoid adding an interface to multiple bridges or assigning conflicting IP configurations.

# 🗁 Configuring the DHCP Server

DHCP (Dynamic Host Configuration Protocol) automatically provides network configuration to clients.

A DHCP server can distribute:

* IP addresses.
* Subnet masks.
* Default gateways.
* DNS server addresses.
* Lease durations.

For the example LAN, use this address pool:

```text
192.168.10.100-192.168.10.200
```

**Create an IP address pool**

```routeros
/ip pool
add name=LAN-POOL ranges=192.168.10.100-192.168.10.200
```

**Create the DHCP server**

```routeros
/ip dhcp-server
add name=LAN-DHCP interface=LAN-BRIDGE address-pool=LAN-POOL lease-time=1d disabled=no
```

If you are not using a bridge, replace `LAN-BRIDGE` with the actual LAN interface.

**Configure the DHCP network**

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

**Verify the DHCP server**

```routeros
/ip dhcp-server print
```

Display active DHCP leases:

```routeros
/ip dhcp-server lease print
```

A connected client should receive an address from the configured pool.

**Important:** Do not configure a DHCP server on a WAN interface unless your network design specifically requires it. A DHCP server should normally serve the intended internal network.

# 🗁 Configuring DNS

DNS (Domain Name System) translates domain names into IP addresses.

RouterOS can act as a DNS cache and resolver for LAN clients.

Enable remote DNS requests:

```routeros
/ip dns
set allow-remote-requests=yes
```

Configure upstream DNS servers:

```routeros
/ip dns
set servers=1.1.1.1,8.8.8.8
```

Display DNS configuration:

```routeros
/ip dns print
```

Because LAN clients use the router as their DNS server, the firewall must allow DNS requests from trusted LAN interfaces or subnets.

Allow DNS over UDP and TCP from the LAN:

```routeros
/ip firewall filter
add chain=input action=accept in-interface=LAN-BRIDGE protocol=udp dst-port=53
add chain=input action=accept in-interface=LAN-BRIDGE protocol=tcp dst-port=53
```

Place these rules appropriately in the input chain, before any rule that would block the requests.

Do not expose the router's DNS resolver to untrusted networks.

# 🗁 Configuring NAT for Internet Access

NAT (Network Address Translation) allows private LAN addresses to communicate through an upstream connection that uses a different address.

For a typical home or small office gateway, source NAT with masquerade is commonly used.

Example:

```routeros
/ip firewall nat
add chain=srcnat action=masquerade out-interface=WAN
```

This rule translates traffic leaving through the WAN interface using the appropriate source address of that interface.

Verify NAT configuration:

```routeros
/ip firewall nat print
```

**When to use masquerade**

Masquerade is generally useful when the WAN IP address is dynamic, such as a DHCP or PPPoE connection.

For a stable static WAN address, a specific `src-nat` rule may also be appropriate:

```routeros
/ip firewall nat
add chain=srcnat action=src-nat to-addresses=203.0.113.2 out-interface=WAN
```

Use the actual static address assigned by the ISP.

# 🗁 Basic Firewall Protection

A router's input chain controls traffic destined for the router itself.

Its forward chain controls traffic passing through the router.

These chains have different purposes and should be configured deliberately.

A basic stateful firewall policy commonly includes:

* Accept established and related connections.
* Drop invalid connections.
* Allow required services from trusted networks.
* Allow required ICMP traffic.
* Permit necessary forwarding traffic.
* Drop unsolicited traffic according to the network policy.

Example building blocks:

```routeros
/ip firewall filter
add chain=input action=accept connection-state=established,related,untracked
add chain=input action=drop connection-state=invalid
add chain=input action=accept protocol=icmp
add chain=input action=accept in-interface=LAN-BRIDGE
add chain=input action=drop
```

A basic forwarding policy can include:

```routeros
/ip firewall filter
add chain=forward action=accept connection-state=established,related,untracked
add chain=forward action=drop connection-state=invalid
```

These examples are not a complete production firewall policy. The forward chain still needs explicit rules for permitted LAN-to-WAN traffic and any other required routed services.

**Important:** RouterOS rule order matters. A final input-chain drop rule can block management access, DHCP, DNS, routing protocols, VPN services, and other traffic if the required exceptions are not placed before it.

Review the existing configuration and maintain a recovery method before applying restrictive firewall rules.

# 🗁 Securing Management Access

Management services should be available only to trusted administrators and management networks.

Display management services:

```routeros
/ip service print
```

Restrict WinBox access to the management subnet:

```routeros
/ip service
set [find name=winbox] address=192.168.10.0/24
```

Restrict SSH access:

```routeros
/ip service
set [find name=ssh] address=192.168.10.0/24
```

Disable unused services:

```routeros
/ip service
set [find name=telnet] disabled=yes
set [find name=ftp] disabled=yes
set [find name=www] disabled=yes
```

Use HTTPS instead of HTTP when WebFig is required.

Additional recommendations:

* Use strong administrative passwords.
* Create individual accounts for administrators.
* Restrict management access through firewall rules.
* Use VPN access for remote administration.
* Disable unnecessary services.
* Keep RouterOS updated.
* Review logs for unexpected login attempts.

# 🗁 Configuring the Time Zone and Clock

Correct system time is important for logs, troubleshooting, certificates, monitoring, and scheduled tasks.

Set the time zone:

```routeros
/system clock
set time-zone-name=Asia/Dhaka
```

Configure an NTP client using reachable time servers:

```routeros
/system ntp client
set enabled=yes
```

Add NTP servers:

```routeros
/system ntp client servers
add address=time.cloudflare.com
add address=pool.ntp.org
```

Verify the system clock:

```routeros
/system clock print
```

Verify NTP client status:

```routeros
/system ntp client print
```

NTP synchronization requires network connectivity and permission for the relevant NTP traffic.

# 🗁 Complete Basic Router Configuration Example

The following example combines the main configuration steps for a simple gateway router.

**Assumptions**

| Parameter | Value |
| --- | --- |
| WAN interface | `ether1` |
| LAN interface | `ether2` |
| LAN subnet | `192.168.10.0/24` |
| Router LAN IP | `192.168.10.1` |
| DHCP pool | `192.168.10.100-192.168.10.200` |
| DNS servers | `1.1.1.1`, `8.8.8.8` |
| WAN addressing | DHCP |

This example assumes a clean configuration with no conflicting default configuration. Review the device before applying it.

**Set the router identity**

```routeros
/system identity
set name=MAIN-GW-01
```

**Configure the administrative password**

```routeros
/user set [find name=admin] password="Your-Strong-Password"
```

**Rename interfaces**

```routeros
/interface ethernet
set [find default-name=ether1] name=WAN
set [find default-name=ether2] name=LAN-PORT
```

**Configure the LAN bridge**

```routeros
/interface bridge
add name=LAN-BRIDGE

/interface bridge port
add bridge=LAN-BRIDGE interface=LAN-PORT
```

**Configure WAN DHCP client**

```routeros
/ip dhcp-client
add interface=WAN disabled=no add-default-route=yes use-peer-dns=no
```

**Assign LAN IP address**

```routeros
/ip address
add address=192.168.10.1/24 interface=LAN-BRIDGE
```

**Create DHCP pool**

```routeros
/ip pool
add name=LAN-POOL ranges=192.168.10.100-192.168.10.200
```

**Create DHCP server**

```routeros
/ip dhcp-server
add name=LAN-DHCP interface=LAN-BRIDGE address-pool=LAN-POOL lease-time=1d disabled=no
```

**Configure DHCP network**

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

**Configure DNS**

```routeros
/ip dns
set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
```

**Configure NAT**

```routeros
/ip firewall nat
add chain=srcnat action=masquerade out-interface=WAN
```

**Configure basic input firewall rules**

```routeros
/ip firewall filter
add chain=input action=accept connection-state=established,related,untracked
add chain=input action=drop connection-state=invalid
add chain=input action=accept protocol=icmp
add chain=input action=accept in-interface=LAN-BRIDGE
add chain=input action=drop
```

The example input policy permits traffic from the LAN and blocks other traffic destined for the router after the established, related, invalid, and ICMP rules. Add any required WAN-side services or management exceptions before the final drop rule.

The example is a learning configuration, not a universal production template. Adapt it to the actual WAN connection, LAN topology, firewall requirements, and existing device configuration.

# 🗁 Verifying the Initial Configuration

After configuration, verify that each component is operating correctly.

**Check interfaces**

```routeros
/interface print
```

**Check IP addresses**

```routeros
/ip address print
```

**Check DHCP client**

```routeros
/ip dhcp-client print detail
```

**Check DHCP server**

```routeros
/ip dhcp-server print
```

**Check DHCP leases**

```routeros
/ip dhcp-server lease print
```

**Check DNS**

```routeros
/ip dns print
```

**Check default route**

```routeros
/ip route print
```

**Check NAT**

```routeros
/ip firewall nat print
```

**Check firewall**

```routeros
/ip firewall filter print
```

**Test upstream connectivity**

```routeros
/ping 1.1.1.1
```

**Test DNS resolution**

```routeros
/ping google.com
```

If the IP address ping succeeds but the domain-name ping fails, investigate DNS configuration and connectivity.

**Test client connectivity**

From a LAN client, verify:

* It receives an IP address from the DHCP server.
* Its default gateway is `192.168.10.1`.
* It can ping the router.
* It can reach an external IP address.
* It can resolve domain names.
* It can access the Internet.

# 🗁 Saving and Backing Up the Configuration

After completing the initial setup, create a backup.

RouterOS provides two common ways to preserve configuration.

**Binary backup**

```routeros
/system backup save name=initial-setup
```

A binary backup preserves device configuration in a format intended for restoring to the same device or a compatible environment.

Treat backup files as sensitive because they can contain confidential configuration data.

**Text configuration export**

```routeros
/export file=initial-setup
```

A text export provides readable RouterOS commands that can be reviewed and reused.

For sensitive configuration values, understand the export options before sharing or storing the file.

**Download backup files**

You can download generated files using WinBox's Files section or another supported file-transfer method.

Store backups securely and maintain copies outside the router.

# 🗁 Troubleshooting Initial Setup

| Problem | Possible cause | Recommended check |
| --- | --- | --- |
| Cannot access router | Incorrect connection or management configuration | Check physical connectivity and try WinBox MAC discovery |
| WAN does not receive an IP | ISP DHCP unavailable, wrong interface, or VLAN requirement | Check DHCP client status and ISP requirements |
| Static WAN cannot reach gateway | Incorrect IP, prefix, or gateway | Verify ISP addressing information |
| LAN clients do not receive IP addresses | DHCP server or bridge misconfiguration | Check DHCP server, pool, network, and bridge ports |
| Clients receive IP but cannot access Internet | Missing default route or NAT | Check routing table and NAT rules |
| Router can ping public IP but clients cannot | Forwarding or NAT configuration problem | Inspect forward-chain rules and source NAT |
| IP connectivity works but domain names fail | DNS configuration problem | Check DNS servers and UDP/TCP port 53 access |
| WinBox access stops after firewall changes | Input-chain rules block management traffic | Review rule order and use a recovery connection |
| Interface remains disconnected | Cable, port, negotiation, or upstream issue | Check link status and physical connectivity |

Useful diagnostic commands:

```routeros
/interface print detail
/ip address print
/ip route print detail
/ip dhcp-client print detail
/ip dhcp-server print detail
/ip dns print
/ip firewall filter print
/ip firewall nat print
/log print
```

# 🗁 Initial Setup Checklist

Use this checklist when preparing a new MikroTik router:

* [ ] Connect to the router.
* [ ] Inspect the default configuration.
* [ ] Set a meaningful system identity.
* [ ] Configure a strong administrative password.
* [ ] Identify and rename network interfaces.
* [ ] Define the WAN and LAN topology.
* [ ] Configure the WAN connection.
* [ ] Configure the LAN IP address.
* [ ] Configure a bridge if multiple LAN ports are required.
* [ ] Create the DHCP address pool.
* [ ] Configure the DHCP server.
* [ ] Configure DNS.
* [ ] Configure source NAT if required.
* [ ] Apply an appropriate firewall policy.
* [ ] Restrict management access.
* [ ] Configure the time zone and NTP.
* [ ] Verify routing and connectivity.
* [ ] Test from a LAN client.
* [ ] Save a backup and configuration export.
* [ ] Document the final configuration.

# 🗁 Official Documentation

* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [MikroTik First Time Configuration](https://help.mikrotik.com/docs/)
* [MikroTik IP Addressing](https://help.mikrotik.com/docs/spaces/ROS/pages/328247/IP+Addressing)
* [MikroTik DHCP](https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP)
* [MikroTik DNS](https://help.mikrotik.com/docs/spaces/ROS/pages/37748767/DNS)
* [MikroTik NAT](https://help.mikrotik.com/docs/spaces/ROS/pages/3211299/NAT)
* [MikroTik Firewall](https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall)
