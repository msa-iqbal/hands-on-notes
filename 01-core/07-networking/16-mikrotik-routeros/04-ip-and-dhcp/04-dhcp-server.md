# DHCP Server in MikroTik RouterOS

A DHCP server automatically assigns IP configuration to devices connected to a network.

In MikroTik RouterOS, the DHCP server feature allows a router to provide network configuration to computers, smartphones, printers, access points, and other network devices.

A DHCP server can provide:

* IPv4 addresses.
* Subnet masks and prefixes.
* Default gateways.
* DNS server addresses.
* Lease durations.
* Additional DHCP options.

DHCP servers are commonly used on LAN networks, office networks, wireless networks, VLANs, and ISP customer networks.

# 🗁 Understanding DHCP Server

DHCP stands for Dynamic Host Configuration Protocol.

A DHCP server manages a pool of available IP addresses and leases them to DHCP clients.

Example network:

```text
                 Internet
                    |
                 ether1
                    |
             MikroTik Router
              DHCP Server
                    |
                bridge-LAN
                    |
             +------+------+
             |      |      |
            PC   Laptop  Phone
```

The MikroTik router assigns IP addresses to devices connected to the LAN.

Example:

| Device | Assigned IP |
| --- | --- |
| MikroTik LAN | 192.168.10.1 |
| PC | 192.168.10.10 |
| Laptop | 192.168.10.11 |
| Smartphone | 192.168.10.12 |

The assigned addresses are managed by the DHCP server.

# 🗁 DHCP Server vs DHCP Client

| Feature | DHCP Server | DHCP Client |
| --- | --- | --- |
| Main purpose | Provides network configuration | Receives network configuration |
| Address management | Manages address pools and leases | Requests an address |
| Typical interface | LAN | WAN |
| RouterOS menu | `/ip dhcp-server` | `/ip dhcp-client` |
| Example | Assigning addresses to LAN devices | Receiving an address from an ISP |

A MikroTik router can operate as a DHCP client on its WAN interface and a DHCP server on its LAN interface simultaneously.

# 🗁 DHCP Server Components

A functional RouterOS DHCP server commonly uses several components.

| Component | RouterOS Menu | Purpose |
| --- | --- | --- |
| IP Address | `/ip address` | Assigns an address to the router's interface |
| IP Pool | `/ip pool` | Defines the available client address range |
| DHCP Server | `/ip dhcp-server` | Manages DHCP leases |
| DHCP Network | `/ip dhcp-server network` | Defines gateway, DNS, and network options |
| DHCP Lease | `/ip dhcp-server lease` | Displays and manages assigned addresses |

These components work together to provide automatic IP configuration.

# 🗁 Prepare the LAN Interface

Before configuring a DHCP server, create or identify the LAN interface.

Example topology:

```text
                 MikroTik Router
                  bridge-LAN
                  192.168.10.1/24
                       |
                 +-----+-----+
                 |     |     |
                PC  Laptop  Phone
```

Create a bridge:

```routeros
/interface bridge
add name=bridge-LAN
```

Add Ethernet interfaces to the bridge:

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

Assign an IP address to the bridge:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

The router will use `192.168.10.1` as its LAN address and default gateway for connected devices.

If the bridge and LAN IP address already exist, do not create duplicates.

# 🗁 Create an IP Pool

An IP pool defines the address range from which the DHCP server assigns addresses.

Example:

```text
Network:       192.168.10.0/24
Gateway:       192.168.10.1
DHCP Pool:     192.168.10.10-192.168.10.200
Broadcast:     192.168.10.255
```

Create the pool:

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.10-192.168.10.200
```

Verify the pool:

```routeros
/ip pool print
```

The pool contains 191 available addresses.

The router's gateway address, `192.168.10.1`, is outside the pool, preventing the DHCP server from assigning that address to a client.

# 🗁 Configure DHCP Network

The DHCP network configuration defines the network parameters provided to clients.

Configure the LAN network:

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

Explanation:

| Property | Description |
| --- | --- |
| `address` | Network address and prefix |
| `gateway` | Default gateway provided to clients |
| `dns-server` | DNS server address provided to clients |

The router is configured as the DNS server for LAN clients.

For this arrangement, configure RouterOS DNS:

```routeros
/ip dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

The router can now resolve DNS queries for LAN clients.

Ensure firewall rules permit DNS requests from trusted LAN interfaces.

# 🗁 Add a DHCP Server

After preparing the LAN interface, IP pool, and DHCP network, create the DHCP server.

```routeros
/ip dhcp-server
add name=dhcp-LAN interface=bridge-LAN address-pool=pool-LAN lease-time=1d disabled=no
```

Explanation:

| Property | Description |
| --- | --- |
| `name=dhcp-LAN` | DHCP server name |
| `interface=bridge-LAN` | Interface where DHCP requests are received |
| `address-pool=pool-LAN` | Address pool used for dynamic leases |
| `lease-time=1d` | Lease duration of one day |
| `disabled=no` | Enables the DHCP server |

Verify the DHCP server:

```routeros
/ip dhcp-server print detail
```

A correctly configured DHCP server should show an enabled entry associated with the intended LAN interface.

# 🗁 Complete DHCP Server Configuration

The following configuration combines the previous steps.

```routeros
/interface bridge
add name=bridge-LAN

/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4

/ip address
add address=192.168.10.1/24 interface=bridge-LAN

/ip pool
add name=pool-LAN ranges=192.168.10.10-192.168.10.200

/ip dhcp-server
add name=dhcp-LAN interface=bridge-LAN address-pool=pool-LAN lease-time=1d disabled=no

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1

/ip dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

This configuration creates a basic DHCP-enabled LAN.

# 🗁 DHCP Setup Using the Setup Command

RouterOS provides a DHCP setup wizard that can configure a DHCP server interactively.

Start the wizard:

```routeros
/ip dhcp-server setup
```

The wizard asks for configuration information such as:

* DHCP server interface.
* Gateway address.
* Address pool.
* DNS servers.
* Lease duration.

Example selections:

```text
DHCP Server Interface: bridge-LAN
DHCP Address Space:    192.168.10.0/24
Gateway for DHCP:      192.168.10.1
Addresses to Give Out: 192.168.10.10-192.168.10.200
DNS Servers:           192.168.10.1
Lease Time:            1d
```

The wizard creates the necessary DHCP configuration.

Review the generated configuration afterward:

```routeros
/ip address print
/ip pool print
/ip dhcp-server print detail
/ip dhcp-server network print detail
```

Do not run the setup wizard blindly on an already configured production router because it may create conflicting or duplicate configuration.

# 🗁 Configure DHCP Lease Time

A DHCP lease determines how long a client can use an assigned IP address before renewing its lease.

Example:

```routeros
/ip dhcp-server
set [find name=dhcp-LAN] lease-time=12h
```

Common lease durations:

| Duration | Example Use |
| --- | --- |
| `10m` | Short-term testing |
| `1h` | Temporary networks |
| `12h` | Office networks |
| `1d` | Home and small business networks |
| `7d` | Networks with relatively stable clients |

A shorter lease duration can make addresses available again sooner after clients disconnect, but it also increases DHCP renewal traffic.

# 🗁 View DHCP Leases

A DHCP lease represents an address assignment managed by the DHCP server.

Display all leases:

```routeros
/ip dhcp-server lease print
```

Display detailed lease information:

```routeros
/ip dhcp-server lease print detail
```

Example:

```text
Flags: D - DYNAMIC, X - DISABLED, R - RADIUS
Columns: ADDRESS, MAC-ADDRESS, HOST-NAME, SERVER, STATUS
#   ADDRESS          MAC-ADDRESS        HOST-NAME  SERVER    STATUS
0 D 192.168.10.10    AA:BB:CC:DD:EE:01  desktop    dhcp-LAN  bound
1 D 192.168.10.11    AA:BB:CC:DD:EE:02  laptop     dhcp-LAN  bound
2 D 192.168.10.12    AA:BB:CC:DD:EE:03  phone      dhcp-LAN  bound
```

Lease information can include:

* IP address.
* MAC address.
* Hostname.
* DHCP server.
* Lease status.
* Remaining lease time.
* Client identifier.

# 🗁 Understanding Dynamic and Static Leases

RouterOS supports dynamic and static DHCP leases.

**Dynamic lease**

A dynamic lease is automatically assigned from the configured IP pool.

Example:

```text
IP Address: 192.168.10.10
MAC Address: AA:BB:CC:DD:EE:01
Lease Type: Dynamic
```

The address is assigned according to DHCP server availability and lease management.

**Static lease**

A static lease reserves a particular IP address for a specific client identity, usually its MAC address.

Example:

```routeros
/ip dhcp-server lease
add address=192.168.10.50 mac-address=AA:BB:CC:DD:EE:50 server=dhcp-LAN comment="Office Printer"
```

The DHCP server will provide the reserved address to the matching client.

Static leases are useful for:

* Network printers.
* Servers.
* NAS devices.
* CCTV systems.
* Network management devices.
* Infrastructure equipment.

Choose static lease addresses carefully so they do not conflict with other statically configured devices.

# 🗁 Convert a Dynamic Lease into a Static Lease

RouterOS can convert an existing dynamic lease into a static lease.

Find the client:

```routeros
/ip dhcp-server lease print
```

Make the lease static:

```routeros
/ip dhcp-server lease
make-static [find mac-address=AA:BB:CC:DD:EE:01]
```

Verify:

```routeros
/ip dhcp-server lease print detail
```

The lease is now retained as a static entry.

# 🗁 Configure a Client Comment

Comments make DHCP lease management easier.

Example:

```routeros
/ip dhcp-server lease
set [find mac-address=AA:BB:CC:DD:EE:50] comment="Office Printer"
```

Comments can identify devices, departments, customers, or locations.

# 🗁 Configure a Static Lease by MAC Address

A static lease associates a client identity with a predefined IP address.

Example:

```routeros
/ip dhcp-server lease
add server=dhcp-LAN mac-address=AA:BB:CC:DD:EE:10 address=192.168.10.20 comment="Development PC"
```

The DHCP client should renew its lease to obtain the reserved address.

The reserved address must belong to the correct network and must not conflict with another device.

# 🗁 Configure DHCP Server on a VLAN

A DHCP server can operate on a VLAN interface.

This is useful when different departments or network segments require separate IP networks.

Example topology:

```text
                 MikroTik Router
                       |
                  VLAN-Aware
                    Bridge
                       |
              +--------+--------+
              |                 |
           VLAN 10            VLAN 20
              |                 |
          192.168.10.0/24   192.168.20.0/24
              |                 |
          DHCP Server        DHCP Server
```

Create VLAN interfaces:

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10

add name=vlan20 interface=bridge-LAN vlan-id=20
```

Assign gateway addresses:

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10

add address=192.168.20.1/24 interface=vlan20
```

Create separate pools:

```routeros
/ip pool
add name=pool-VLAN10 ranges=192.168.10.10-192.168.10.200

add name=pool-VLAN20 ranges=192.168.20.10-192.168.20.200
```

Create DHCP servers:

```routeros
/ip dhcp-server
add name=dhcp-VLAN10 interface=vlan10 address-pool=pool-VLAN10 lease-time=1d disabled=no

add name=dhcp-VLAN20 interface=vlan20 address-pool=pool-VLAN20 lease-time=1d disabled=no
```

Configure DHCP network options:

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1

add address=192.168.20.0/24 gateway=192.168.20.1 dns-server=192.168.20.1
```

This example demonstrates the DHCP configuration for two VLANs.

The bridge VLAN table, tagged and untagged ports, and hardware-specific VLAN configuration must also be configured correctly. Creating VLAN interfaces alone does not establish complete VLAN isolation.

# 🗁 DHCP Server on a Wireless Network

A DHCP server can assign addresses to wireless clients through a wireless interface or a bridge containing the wireless interface.

Example:

```text
                 MikroTik Router
                       |
                   bridge-LAN
                       |
                Wireless Access Point
                       |
                 Wireless Clients
```

If the wireless interface is part of `bridge-LAN`, the DHCP server should normally run on that bridge.

This allows wired and wireless devices in the same Layer 2 network to receive addresses from the same DHCP server.

Separate wireless VLANs should use separate DHCP server configurations when network segmentation is required.

# 🗁 DHCP Server Options

DHCP options provide additional configuration information to clients.

Common DHCP options include:

| Option | Name | Purpose |
| --- | --- | --- |
| 1 | Subnet Mask | Defines the client's IPv4 subnet |
| 3 | Router | Provides the default gateway |
| 6 | DNS Server | Provides DNS server addresses |
| 12 | Host Name | Identifies the client hostname |
| 15 | Domain Name | Provides the DNS domain |
| 42 | NTP Servers | Provides time server addresses |
| 66 | TFTP Server Name | Provides a TFTP server name |
| 67 | Bootfile Name | Provides a boot filename |

RouterOS supports DHCP option configuration through its DHCP option menus.

Inspect available options:

```routeros
/ip dhcp-server option print
```

Create a custom option:

```routeros
/ip dhcp-server option
add name=ntp-server code=42 value="'192.168.10.1'"
```

Assign the option to the DHCP server network:

```routeros
/ip dhcp-server network
set [find address=192.168.10.0/24] dhcp-option=ntp-server
```

DHCP option values must follow the RouterOS value encoding rules for their respective data types. Verify the expected encoding for the option before deploying custom options.

# 🗁 Configure DHCP Server DNS Settings

A DHCP server can provide DNS information to clients.

Example:

```routeros
/ip dhcp-server network
set [find address=192.168.10.0/24] dns-server=1.1.1.1,8.8.8.8
```

Clients will receive these DNS server addresses through DHCP.

Alternatively, provide the MikroTik router's LAN address:

```routeros
/ip dhcp-server network
set [find address=192.168.10.0/24] dns-server=192.168.10.1
```

For the second configuration, RouterOS must be configured to resolve DNS queries and accept requests from trusted LAN clients.

# 🗁 Disable and Enable DHCP Server

Disable a DHCP server:

```routeros
/ip dhcp-server
disable [find name=dhcp-LAN]
```

Enable it again:

```routeros
/ip dhcp-server
enable [find name=dhcp-LAN]
```

Disabling the server prevents it from providing new DHCP responses on the configured interface.

# 🗁 Remove DHCP Server

Remove a DHCP server:

```routeros
/ip dhcp-server
remove [find name=dhcp-LAN]
```

Verify:

```routeros
/ip dhcp-server print
```

Removing the DHCP server does not automatically remove its associated IP pool or DHCP network configuration.

Remove unused supporting configuration separately when appropriate.

# 🗁 DHCP Server Troubleshooting

**Clients do not receive IP addresses**

Possible causes:

* DHCP server is disabled.
* Incorrect DHCP server interface.
* IP pool is exhausted.
* Bridge ports are not configured correctly.
* VLAN configuration is incorrect.
* Another DHCP server is interfering.
* Firewall rules are blocking DHCP traffic.

Check:

```routeros
/ip dhcp-server print detail
/ip pool print detail
/ip dhcp-server lease print detail
/interface bridge port print
```

**DHCP server reports an invalid configuration**

Possible causes:

* Incorrect interface assignment.
* Missing IP pool.
* Invalid address pool.
* Incorrect network configuration.

Check:

```routeros
/ip dhcp-server print detail
/ip pool print detail
/ip dhcp-server network print detail
```

**Clients receive addresses but cannot access the Internet**

Possible causes:

* Incorrect default gateway.
* Missing NAT configuration.
* Missing default route.
* Upstream connectivity failure.
* Firewall restrictions.

Check:

```routeros
/ip dhcp-server network print detail
/ip route print detail
/ip firewall nat print
```

**Clients receive incorrect DNS servers**

Possible causes:

* Incorrect DHCP network configuration.
* Multiple DHCP servers.
* Unexpected DHCP options.
* Clients retaining an older lease.

Check:

```routeros
/ip dhcp-server network print detail
/ip dhcp-server lease print detail
/ip dns print
```

Renew the client lease after correcting the configuration.

**Some clients cannot obtain addresses**

Possible causes:

* DHCP pool exhaustion.
* Duplicate static assignments.
* Client identity mismatch.
* Wireless or VLAN isolation.
* DHCP server interface mismatch.

Inspect active leases and available pool addresses.

# 🗁 DHCP Security Best Practices

* Run DHCP servers only on intended LAN or client-facing interfaces.
* Do not expose a LAN DHCP server directly to an untrusted WAN.
* Use VLAN segmentation for separate network groups.
* Maintain appropriate DHCP pool ranges.
* Use static leases for important infrastructure devices.
* Monitor unexpected DHCP servers on the network.
* Configure firewall rules to protect router management services.
* Document address assignments and lease reservations.
* Use DHCP snooping on supported switching infrastructure where appropriate.
* Avoid overlapping address pools and static IP assignments.

# 🗁 Practical DHCP Server Lab

**Lab topology**

```text
                 Upstream Router
                       |
                    ether1
                       |
                MikroTik Router
                 DHCP Server
                       |
                   bridge-LAN
                       |
                 +-----+-----+
                 |           |
               ether2      ether3
                 |           |
                PC         Laptop
```

**Network requirements**

| Parameter | Value |
| --- | --- |
| LAN interface | bridge-LAN |
| LAN gateway | 192.168.10.1 |
| Network | 192.168.10.0/24 |
| DHCP pool | 192.168.10.10-192.168.10.200 |
| Lease duration | 1 day |
| DNS server | 192.168.10.1 |

**Configure the bridge**

```routeros
/interface bridge
add name=bridge-LAN

/interface bridge port
add bridge=bridge-LAN interface=ether2

add bridge=bridge-LAN interface=ether3
```

**Assign the gateway address**

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

**Create the DHCP pool**

```routeros
/ip pool
add name=pool-LAN ranges=192.168.10.10-192.168.10.200
```

**Configure the DHCP network**

```routeros
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

**Create the DHCP server**

```routeros
/ip dhcp-server
add name=dhcp-LAN interface=bridge-LAN address-pool=pool-LAN lease-time=1d disabled=no
```

**Configure router DNS**

```routeros
/ip dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

**Verify the DHCP server**

```routeros
/ip dhcp-server print detail
/ip dhcp-server network print detail
/ip pool print detail
```

**Connect client devices**

Connect a PC or laptop to `ether2` or `ether3`.

Configure the client network adapter to obtain an IP address automatically.

Verify that the client receives an address within:

```text
192.168.10.10 - 192.168.10.200
```

**Verify client leases**

```routeros
/ip dhcp-server lease print detail
```

**Test connectivity**

From the client device, test:

```bash
ping 192.168.10.1
ping 8.8.8.8
ping google.com
```

The first test verifies connectivity to the MikroTik gateway. The remaining tests verify upstream IP connectivity and DNS resolution when the WAN and Internet configuration are available.

# 🗁 Official Documentation

* MikroTik DHCP: <https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP>
* MikroTik IP Addressing: <https://help.mikrotik.com/docs/spaces/ROS/pages/328247/IP+Addressing>
* MikroTik Firewall: <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Common+Firewall+Matchers+and+Actions>
