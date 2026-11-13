# VirtualBox and MikroTik CHR

VirtualBox and MikroTik Cloud Hosted Router (CHR) provide a practical environment for learning router configuration without requiring physical MikroTik hardware.

**VirtualBox** is a virtualization platform that runs virtual machines on your computer.

**MikroTik CHR** is a RouterOS version designed to run as a virtual machine on supported hypervisors.

Together, they allow you to build virtual networking labs and practice:

* Router configuration.
* IP addressing and subnetting.
* Ethernet interfaces and bridges.
* Static routing.
* DHCP server and client configuration.
* NAT and firewall rules.
* VLANs.
* VPN connections.
* Bandwidth management.
* Network troubleshooting.

# 🗁 Understanding MikroTik CHR

MikroTik CHR provides RouterOS functionality in a virtualized environment.

Unlike traditional MikroTik routers, CHR does not require a physical RouterBOARD device.

CHR supports multiple virtualized environments, including:

* VirtualBox.
* VMware.
* Hyper-V.
* KVM.
* Cloud virtualization platforms.

CHR uses virtual network interfaces that connect to virtual switches or network adapters provided by the hypervisor.

**Important CHR characteristics:**

* It runs RouterOS.
* It supports RouterOS configuration through CLI, WinBox, and WebFig.
* It can use multiple virtual Ethernet interfaces.
* It supports RouterOS v7 features.
* Its performance depends on the host hardware and virtualization platform.
* Its license level determines applicable throughput and usage restrictions.

For learning, the free CHR option is sufficient for many basic configuration exercises. Check MikroTik's current licensing documentation for the exact limitations of each license.

Official resources:

* CHR download: <https://mikrotik.com/download>
* CHR documentation: <https://help.mikrotik.com/docs/>
* CHR licensing: <https://help.mikrotik.com/docs/spaces/ROS/pages/18350234/CHR>

# 🗁 Download MikroTik CHR

Download the CHR disk image from the official MikroTik website.

Visit:

<https://mikrotik.com/download>

Find the **Cloud Hosted Router** section.

For VirtualBox, download the disk image in VDI format when available.

The downloaded archive may contain a virtual disk file such as:

```text
chr-7.x.vdi
```

The exact filename depends on the RouterOS version.

Extract the downloaded archive before attaching the virtual disk to VirtualBox.

# 🗁 Create a Virtual Machine in VirtualBox

Open VirtualBox and create a new virtual machine.

Suggested configuration:

| Setting | Value |
| --- | --- |
| Name | MikroTik-CHR |
| Type | Linux |
| Version | Other Linux (64-bit) |
| RAM | 256–512 MB |
| CPU | 1 vCPU |
| Disk | Use existing virtual disk |
| Network adapters | 2 or more |

The operating system type is primarily a VirtualBox configuration choice. CHR itself runs RouterOS, not a conventional Linux distribution.

# 🗁 Attach the CHR Virtual Disk

During virtual machine creation, select the option to use an existing virtual hard disk.

Choose the extracted CHR VDI file.

If the virtual machine has already been created:

* Select the virtual machine.
* Open Settings.
* Select Storage.
* Add a hard disk to the appropriate controller.
* Choose the existing CHR VDI file.
* Save the configuration.

The CHR virtual disk should now appear in the machine's storage configuration.

# 🗁 Configure CPU and Memory

Select the CHR virtual machine and open Settings.

Under System:

* Assign one virtual CPU initially.
* Allocate 256–512 MB RAM for a basic lab.
* Keep sufficient memory available for the host operating system.

For larger labs, increase resources according to the number of interfaces, enabled services, routing protocols, and traffic load.

Do not allocate more resources than the host can comfortably provide.

# 🗁 Configure Virtual Network Adapters

Virtual network adapters connect CHR to other virtual machines and external networks.

For a basic lab, configure two adapters.

| Adapter | VirtualBox Mode | Purpose |
| --- | --- | --- |
| Adapter 1 | NAT | Internet connectivity |
| Adapter 2 | Internal Network | Isolated lab network |

Set the Internal Network name to:

```text
network-lab
```

Use the same Internal Network name for client virtual machines that need to communicate with CHR.

VirtualBox treats internal network names as separate virtual network segments. A spelling difference can place machines on different isolated networks.

**Adapter 1: NAT**

Provides outbound connectivity through the host's network connection.

**Adapter 2: Internal Network**

Connects CHR to other virtual machines on the isolated lab network.

This separation allows you to practice routing and firewall configuration without directly exposing the lab network to your physical LAN.

# 🗁 Start the CHR Virtual Machine

Start the MikroTik CHR virtual machine.

The RouterOS console should appear after the system boots.

The default login credentials for a fresh CHR installation are generally:

```text
Username: admin
Password: (blank)
```

At the first login, follow the prompts to set a new password if requested.

Do not assume these credentials will remain unchanged if the disk image has previously been configured.

# 🗁 Understand the RouterOS CLI

RouterOS uses a command-line interface organized into configuration menus.

Examples of common menus:

| Menu | Purpose |
| --- | --- |
| `/system` | System configuration |
| `/interface` | Interface configuration |
| `/ip address` | IPv4 address configuration |
| `/ip route` | Routing configuration |
| `/ip firewall` | Firewall configuration |
| `/ip dhcp-server` | DHCP server configuration |
| `/ip dns` | DNS configuration |
| `/tool` | Diagnostic and utility tools |

Useful commands:

```routeros
/system identity print
/system resource print
/system package print
/interface print
/ip address print
/ip route print
```

These commands help you inspect the router's identity, resources, installed packages, interfaces, addresses, and routes.

# 🗁 Rename the Router

Assign a meaningful identity to the router.

```routeros
/system identity set name=LAB-CHR-01
```

Verify:

```routeros
/system identity print
```

Meaningful device names make it easier to identify routers when your lab grows.

# 🗁 Identify Network Interfaces

List all interfaces:

```routeros
/interface print
```

Example output:

```text
Flags: R - RUNNING
Columns: NAME, TYPE, ACTUAL-MTU
#   NAME     TYPE      ACTUAL-MTU
0 R ether1   ether          1500
1 R ether2   ether          1500
```

Your output may differ depending on the number of adapters and the CHR version.

For this lab, assume:

* `ether1` connects to the NAT adapter.
* `ether2` connects to the Internal Network.

Check interface details:

```routeros
/interface ethernet print detail
```

# 🗁 Configure Interface Names

Rename the interfaces to make their roles easier to understand.

```routeros
/interface set [find default-name=ether1] name=WAN
/interface set [find default-name=ether2] name=LAN
```

Verify:

```routeros
/interface print
```

Expected interface names:

```text
WAN
LAN
```

Interface naming is especially useful when managing routers with several WAN, LAN, and VLAN interfaces.

# 🗁 Configure the LAN IP Address

Assign an IPv4 address to the LAN interface.

Use the following addressing plan:

| Device | Interface | IP Address |
| --- | --- | --- |
| CHR | LAN | 192.168.10.1/24 |
| Ubuntu Client 1 | Ethernet | 192.168.10.10/24 |
| Ubuntu Client 2 | Ethernet | 192.168.10.20/24 |

Configure the CHR address:

```routeros
/ip address add address=192.168.10.1/24 interface=LAN comment="Lab LAN"
```

Verify:

```routeros
/ip address print
```

The LAN interface now acts as the default gateway for devices in the `192.168.10.0/24` subnet.

# 🗁 Configure the WAN Interface

The VirtualBox NAT adapter normally provides DHCP service to the connected virtual machine.

Configure the CHR WAN interface as a DHCP client:

```routeros
/ip dhcp-client add interface=WAN disabled=no
```

Check the DHCP client:

```routeros
/ip dhcp-client print detail
```

Check the assigned IP address:

```routeros
/ip address print
```

If the DHCP client receives an address successfully, CHR has obtained its WAN configuration from the VirtualBox NAT network.

# 🗁 Configure DNS

Configure public DNS resolvers:

```routeros
/ip dns set servers=1.1.1.1,8.8.8.8 allow-remote-requests=no
```

Verify:

```routeros
/ip dns print
```

The `allow-remote-requests=no` setting prevents CHR from accepting DNS queries from clients through its DNS service.

If you later want CHR to provide DNS caching for lab clients, configure that service deliberately and restrict access to trusted networks.

# 🗁 Configure NAT Masquerade

The LAN clients use private IPv4 addresses. To allow them to access external IPv4 networks through the CHR WAN interface, configure source NAT.

```routeros
/ip firewall nat add chain=srcnat out-interface=WAN action=masquerade comment="Lab LAN Internet Access"
```

This rule translates matching outgoing source addresses to the address of the WAN interface.

Verify:

```routeros
/ip firewall nat print
```

The NAT rule is useful when the WAN address is assigned dynamically.

# 🗁 Configure an Ubuntu Client

Create an Ubuntu virtual machine in VirtualBox.

Attach its network adapter to the same Internal Network:

```text
network-lab
```

Assign a static IPv4 address inside Ubuntu.

First, identify the interface:

```bash
ip -br link
```

Find the actual interface name, such as `enp0s3`.

For a temporary configuration, use:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
sudo ip link set enp0s3 up
sudo ip route add default via 192.168.10.1
```

Replace `enp0s3` with the actual interface name.

Configure DNS for testing:

```bash
sudo resolvectl dns enp0s3 1.1.1.1
```

These commands configure the current runtime network state. The settings may not survive a reboot or be compatible with an interface managed by another network configuration service.

Verify:

```bash
ip -br address
ip route
```

# 🗁 Test LAN Connectivity

From the Ubuntu client, ping the CHR LAN address:

```bash
ping -c 4 192.168.10.1
```

If successful, the client can communicate with the router over the internal virtual network.

Test the second client after configuring it:

```bash
ping -c 4 192.168.10.20
```

If both clients are connected to the same Internal Network and have compatible IP configurations, they should be able to communicate directly.

# 🗁 Test Internet Connectivity

From the CHR console, test an external IP address:

```routeros
/ping 1.1.1.1
```

Test DNS resolution:

```routeros
/ping google.com
```

From the Ubuntu client:

```bash
ping -c 4 1.1.1.1
```

Then test DNS:

```bash
ping -c 4 google.com
```

If the client can reach the CHR but cannot reach the Internet, check:

* WAN DHCP client status.
* CHR default route.
* Source NAT rule.
* CHR firewall filter rules.
* VirtualBox NAT adapter.
* Host Internet connectivity.

# 🗁 Inspect the Routing Table

On CHR:

```routeros
/ip route print
```

A DHCP-provided WAN configuration should normally install a default route when the DHCP client is configured to add one.

The default route allows CHR to forward traffic toward destinations that do not have a more specific route.

On Ubuntu:

```bash
ip route
```

Expected default gateway:

```text
default via 192.168.10.1
```

# 🗁 Access CHR Through WinBox

WinBox is MikroTik's graphical configuration utility.

Download it from the official MikroTik website:

<https://mikrotik.com/download>

Start WinBox and connect using:

* IP address: `192.168.10.1`
* Username: `admin`
* Password: Your configured password.

The host must have a network path to the CHR LAN address.

For a Host-only management connection, configure an additional CHR adapter and the corresponding host-only adapter in VirtualBox. Assign compatible addresses to both sides.

For example:

| Device | Interface | IP Address |
| --- | --- | --- |
| Host | Host-only adapter | 192.168.99.1/24 |
| CHR | Management adapter | 192.168.99.2/24 |

Use the actual CHR interface corresponding to the additional adapter.

Do not expose WinBox management services to untrusted networks.

# 🗁 Save the Router Configuration

RouterOS normally stores configuration changes automatically.

Export a readable configuration:

```routeros
/export file=lab-chr-config
```

Create a binary backup:

```routeros
/system backup save name=lab-chr-backup
```

The generated files are stored on the CHR filesystem.

Retrieve them through an available file-transfer method, such as WinBox Files or SFTP, and store them in your host's backup directory.

A text export is useful for reviewing and documenting configuration. A binary backup is useful for restoring a compatible RouterOS environment.

# 🗁 Troubleshooting

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| CHR does not boot | Incorrect or damaged disk image | Download and attach a valid CHR image |
| No network interfaces | Virtual adapters not enabled | Check VirtualBox network settings |
| WAN has no IP address | DHCP client disabled or NAT adapter unavailable | Check WAN DHCP client and adapter mode |
| LAN clients cannot reach CHR | Different Internal Network names | Use the same network name |
| Client cannot reach Internet | Missing default route or NAT | Inspect routing and NAT configuration |
| WinBox cannot connect | No host-to-CHR management path | Configure Host-only networking |
| Slow virtual router | Insufficient CPU or memory | Adjust virtual machine resources |
| IP address conflict | Duplicate addresses | Review the lab IP plan |

Useful diagnostic commands:

```routeros
/interface print
/ip address print
/ip dhcp-client print detail
/ip route print
/ip firewall nat print
/ip firewall filter print
```

# 🗁 Lab Exercise

Build a small routed network using VirtualBox and MikroTik CHR.

**Requirements**

* VirtualBox.
* One MikroTik CHR virtual machine.
* Two Ubuntu virtual machines.
* Two virtual network adapters on CHR.
* One isolated Internal Network.

**Tasks**

* Import the CHR virtual disk.
* Create the CHR virtual machine.
* Configure WAN and LAN adapters.
* Rename the router and its interfaces.
* Configure the LAN IP address.
* Configure the WAN DHCP client.
* Configure DNS.
* Add a source NAT masquerade rule.
* Configure static IP addresses on both clients.
* Test communication between clients.
* Test Internet connectivity.
* Access CHR through WinBox.
* Export and back up the configuration.

**Expected Results**

* CHR boots successfully.
* WAN receives an IP address.
* LAN clients can reach the CHR gateway.
* Both clients can communicate.
* Clients can access external IPv4 destinations when the WAN connection is available.
* DNS resolution works.
* Router configuration is saved and backed up.

# 🗁 Best Practices

* Use meaningful virtual machine names.
* Keep WAN and LAN interfaces clearly identified.
* Use separate virtual networks for management and lab traffic.
* Maintain a documented IP addressing plan.
* Avoid connecting experimental networks directly to production networks.
* Back up CHR configurations regularly.
* Take VirtualBox snapshots before major experiments.
* Use strong passwords for router management.
* Restrict management services to trusted interfaces.
* Monitor CPU and memory usage as the lab grows.

# 🗁 Summary

You have learned how to create a MikroTik CHR virtual router in VirtualBox and connect it to virtual client machines.

You should now understand:

* MikroTik CHR and its purpose.
* CHR virtual machine creation.
* VirtualBox network adapters.
* RouterOS CLI basics.
* WAN and LAN interface configuration.
* IP addressing and routing.
* DHCP client configuration.
* DNS and NAT configuration.
* WinBox management.
* Router configuration backup.

This virtual router will serve as the foundation for future networking exercises involving routing, switching, VLANs, firewall rules, VPNs, and ISP scenarios.
