# IP Address Configuration in MikroTik RouterOS

IP address configuration is one of the fundamental tasks in MikroTik RouterOS administration.

An IP address identifies a device or interface within an IP network and enables communication between devices across local and remote networks.

In MikroTik RouterOS, IP addresses can be assigned to:

* Ethernet interfaces.
* Bridge interfaces.
* VLAN interfaces.
* Loopback interfaces.
* VPN interfaces.
* Tunnel interfaces.
* Other supported Layer 3 interfaces.

RouterOS supports both IPv4 and IPv6 addressing.

This document focuses primarily on IPv4 address configuration using RouterOS v7.

# 🗁 Understanding IPv4 Addressing

An IPv4 address is a 32-bit logical address represented as four decimal octets separated by dots.

Example:

```text
192.168.10.1
```

Each octet represents 8 bits and has a decimal range of 0–255.

An IPv4 address contains two logical components:

* Network portion: Identifies the IP network.
* Host portion: Identifies an interface within that network.

The subnet mask or CIDR prefix determines how many bits belong to the network portion.

Example:

```text
IP Address: 192.168.10.1
Subnet Mask: 255.255.255.0
CIDR: /24
Network: 192.168.10.0
Broadcast: 192.168.10.255
Usable Hosts: 192.168.10.1 - 192.168.10.254
```

# 🗁 Private and Public IP Addresses

IPv4 addresses are commonly classified as private or public.

**Private IP addresses**

Private addresses are reserved for internal networks and are not globally routed across the public Internet.

| Private range | CIDR | Address range |
| --- | --- | --- |
| 10.0.0.0/8 | /8 | 10.0.0.0 – 10.255.255.255 |
| 172.16.0.0/12 | /12 | 172.16.0.0 – 172.31.255.255 |
| 192.168.0.0/16 | /16 | 192.168.0.0 – 192.168.255.255 |

**Public IP addresses**

Public IP addresses are globally routable addresses assigned through Internet registries and service providers.

They are commonly used for:

* Internet-facing routers.
* Public servers.
* ISP infrastructure.
* Publicly accessible network services.

A MikroTik router may use private IP addresses on its LAN interfaces and a public IP address on its WAN interface.

# 🗁 Understanding CIDR and Subnet Masks

CIDR (Classless Inter-Domain Routing) represents the network prefix using a slash followed by the number of network bits.

Examples:

| CIDR | Subnet mask | Total addresses |
| --- | --- | ---: |
| /8 | 255.0.0.0 | 16,777,216 |
| /16 | 255.255.0.0 | 65,536 |
| /24 | 255.255.255.0 | 256 |
| /25 | 255.255.255.128 | 128 |
| /26 | 255.255.255.192 | 64 |
| /27 | 255.255.255.224 | 32 |
| /28 | 255.255.255.240 | 16 |
| /30 | 255.255.255.252 | 4 |
| /32 | 255.255.255.255 | 1 |

For a conventional IPv4 subnet, the network and broadcast addresses are not assigned to ordinary hosts.

For example, a `/24` network normally provides 254 usable host addresses.

# 🗁 IP Address Configuration in RouterOS

RouterOS manages IPv4 addresses through:

```routeros
/ip address
```

The main command for assigning an IP address is:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
```

This assigns `192.168.10.1/24` to `ether2`.

The address is associated with the interface and creates a connected route for the network when the address is active.

# 🗁 Assign an IP Address to an Ethernet Interface

Suppose `ether2` connects to a local network.

Network configuration:

| Parameter | Value |
| --- | --- |
| Interface | ether2 |
| IP address | 192.168.10.1 |
| CIDR | /24 |
| Network | 192.168.10.0/24 |
| Subnet mask | 255.255.255.0 |
| Broadcast | 192.168.10.255 |

Assign the IP address:

```routeros
/ip address
add address=192.168.10.1/24 interface=ether2 comment="LAN Gateway"
```

Verify the configuration:

```routeros
/ip address print
```

Display detailed information:

```routeros
/ip address print detail
```

# 🗁 Assign an IP Address to a Bridge Interface

When multiple physical interfaces are combined into a Layer 2 bridge, the router's LAN IP address is commonly assigned to the bridge rather than an individual bridge port.

Example:

```routeros
/interface bridge
add name=bridge-LAN

/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
```

Assign the gateway IP address:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN comment="LAN Gateway"
```

All devices connected through the bridge can communicate within the same Layer 2 network, subject to the bridge and firewall configuration.

**Important:** Do not assign the same subnet's gateway address to both the bridge and one of its physical bridge ports. Use the bridge interface as the Layer 3 interface for that bridged network.

# 🗁 Assign an IP Address to a VLAN Interface

In a VLAN-aware network, the gateway IP address should normally be assigned to the corresponding VLAN interface.

Example:

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10

/ip address
add address=192.168.10.1/24 interface=vlan10 comment="VLAN 10 Gateway"
```

This makes the router available as the Layer 3 gateway for VLAN 10.

For multiple VLANs:

```routeros
/interface vlan
add name=vlan10 interface=bridge-LAN vlan-id=10
add name=vlan20 interface=bridge-LAN vlan-id=20

/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
```

The bridge VLAN table must also permit the router CPU to access these VLANs. Configure the bridge interface as a tagged member of each VLAN when required.

# 🗁 Assign Multiple IP Addresses to One Interface

RouterOS allows multiple IP addresses to be assigned to the same interface.

This can be useful when:

* Migrating between IP subnets.
* Supporting multiple logical networks.
* Maintaining compatibility with legacy devices.
* Configuring additional service addresses.

Example:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN comment="Primary LAN"
add address=192.168.20.1/24 interface=bridge-LAN comment="Secondary LAN"
```

Both addresses are associated with `bridge-LAN`.

However, assigning multiple subnets to one interface does not automatically provide Layer 2 isolation between those networks. Use VLANs when separate broadcast domains are required.

# 🗁 Configure a Secondary IP Address

A secondary IP address is an additional address assigned to an interface alongside its primary address.

Example:

```routeros
/ip address
add address=10.10.10.1/24 interface=ether2 comment="Additional Network"
```

If `ether2` already has `192.168.10.1/24`, it can also have `10.10.10.1/24`.

Inspect all configured addresses:

```routeros
/ip address print detail
```

RouterOS does not require a separate secondary-address command. Each address is configured as an independent entry.

# 🗁 Configure a Point-to-Point Address

Point-to-point links connect two Layer 3 endpoints.

A `/30` subnet is traditionally used for IPv4 point-to-point addressing because it provides two usable host addresses.

Example:

```text
Network: 10.0.0.0/30
Router A: 10.0.0.1
Router B: 10.0.0.2
Broadcast: 10.0.0.3
```

On Router A:

```routeros
/ip address
add address=10.0.0.1/30 interface=ether1
```

On Router B:

```routeros
/ip address
add address=10.0.0.2/30 interface=ether1
```

Both routers can communicate over the directly connected network.

A `/31` subnet can also be used for point-to-point links when both endpoints support RFC 3021 behavior.

# 🗁 Configure a Loopback Address

A loopback interface is a logical interface that does not depend on a physical Ethernet link.

It is useful for:

* Router identification.
* Routing protocol router IDs.
* Management addressing.
* Monitoring.
* Stable endpoint addressing.

RouterOS can create a loopback-style interface using an empty bridge.

Example:

```routeros
/interface bridge
add name=loopback protocol-mode=none

/ip address
add address=10.255.255.1/32 interface=loopback comment="Router Loopback"
```

A `/32` address represents a single IPv4 host route.

Loopback addresses are commonly used in OSPF and BGP designs because they can remain stable even when physical interfaces change state.

# 🗁 Static and Dynamic IP Addresses

RouterOS can have both manually configured and dynamically obtained IP addresses.

**Static IP address**

A static IP address is manually configured by an administrator.

Example:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

Static addresses are commonly used for:

* LAN gateways.
* Server networks.
* Router management.
* Point-to-point links.
* Infrastructure interfaces.

**Dynamic IP address**

A dynamic IP address is obtained automatically through a protocol or service, such as DHCP.

Example DHCP client configuration:

```routeros
/ip dhcp-client
add interface=ether1 add-default-route=yes use-peer-dns=yes disabled=no
```

The DHCP server may provide:

* IP address.
* Subnet information.
* Gateway.
* DNS servers.
* Lease duration.

Inspect dynamically obtained addresses:

```routeros
/ip address print
/ip dhcp-client print detail
```

Dynamic addresses are commonly marked with `D` in the IP address listing.

# 🗁 Add, Remove, Enable, and Disable IP Addresses

**Add an IP address**

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

**Display IP addresses**

```routeros
/ip address print
```

**Display detailed information**

```routeros
/ip address print detail
```

**Find an IP address by value**

```routeros
/ip address
print where address="192.168.10.1/24"
```

**Disable an IP address**

```routeros
/ip address
disable [find address="192.168.10.1/24"]
```

**Enable an IP address**

```routeros
/ip address
enable [find address="192.168.10.1/24"]
```

**Remove an IP address**

```routeros
/ip address
remove [find address="192.168.10.1/24"]
```

Be careful when modifying the IP address used for your current management connection. Removing or disabling it can disconnect your session.

# 🗁 Modify an Existing IP Address

Use the `set` command to modify an existing address.

First, inspect the address list:

```routeros
/ip address print
```

Example output:

```text
Columns: ADDRESS, NETWORK, INTERFACE
# ADDRESS             NETWORK       INTERFACE
0 192.168.10.1/24     192.168.10.0  bridge-LAN
```

Change the address:

```routeros
/ip address
set [find address="192.168.10.1/24"] address=192.168.20.1/24
```

Change the interface:

```routeros
/ip address
set [find address="192.168.20.1/24"] interface=ether3
```

Use the correct address selector when modifying existing entries. If multiple entries match a selector, inspect the result before applying changes.

# 🗁 Understanding the Network Property

RouterOS calculates the network associated with an IP address and prefix.

Example:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

The resulting network is:

```text
192.168.10.0
```

Inspect the calculated network:

```routeros
/ip address print detail
```

The `network` property represents the network address associated with the configured IP prefix.

For point-to-point addressing, the network calculation and route behavior can differ from conventional multi-access Ethernet subnet examples.

# 🗁 Connected Routes

When an IP address is assigned to an active interface, RouterOS normally creates a connected route for the associated network.

Example:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

Inspect the routing table:

```routeros
/ip route print
```

A connected route is commonly identified by the `DAc` flags:

* `D` — Dynamic.
* `A` — Active.
* `c` — Connected.

Example:

```text
DAc 192.168.10.0/24 bridge-LAN
```

This route allows the router to reach destinations within the directly connected subnet without requiring a manually configured static route.

# 🗁 IP Address Configuration Through WinBox

IP addresses can also be configured through WinBox.

Steps:

* Open WinBox and connect to the MikroTik router.
* Navigate to **IP → Addresses**.
* Click **Add New**.
* Enter the IP address with its CIDR prefix.
* Select the appropriate interface.
* Add a comment if necessary.
* Click **Apply** and **OK**.

<escape>Example:</escape>

```text
Address:   192.168.10.1/24
Interface: bridge-LAN
Comment:   LAN Gateway
```

The new address should appear in the address list.

# 🗁 Verify IP Connectivity

After configuring an IP address, verify connectivity.

**Inspect configured addresses**

```routeros
/ip address print
```

**Inspect connected routes**

```routeros
/ip route print
```

**Ping a local gateway**

```routeros
/ping 192.168.10.254
```

**Ping a remote IP address**

```routeros
/ping 8.8.8.8
```

**Specify the source address**

```routeros
/ping 8.8.8.8 src-address=192.168.10.1
```

Specifying the source address helps test connectivity using a particular router address.

**Inspect the ARP table**

```routeros
/ip arp print
```

**Inspect interface status**

```routeros
/interface print
```

A configured IP address alone does not guarantee connectivity. The interface must be operational, and routing, firewall, and upstream network configuration must permit the traffic.

# 🗁 Common IP Address Configuration Problems

**IP address cannot be assigned**

Possible causes:

* Incorrect IP address format.
* Invalid CIDR prefix.
* Address already configured on the same interface.
* Incorrect interface name.
* Address conflicts with the existing network design.

Check:

```routeros
/ip address print detail
/interface print
```

**Connected route is missing**

Possible causes:

* Interface is inactive.
* IP address is disabled.
* Address configuration is invalid.
* The interface is not operational.

Check:

```routeros
/ip address print detail
/ip route print detail
/interface print detail
```

**Router cannot ping another device in the same subnet**

Possible causes:

* Incorrect subnet mask.
* Incorrect IP address.
* Interface is down.
* VLAN membership is incorrect.
* ARP resolution is failing.
* Remote device firewall blocks ICMP.

Check:

```routeros
/ip address print
/ip arp print
/ping 192.168.10.254
```

**Router can ping an IP address but cannot resolve domain names**

Possible causes:

* DNS servers are not configured.
* DNS requests are blocked.
* The DNS service is unavailable.

Check:

```routeros
/ip dns print
/ping google.com
```

**IP address is configured on the wrong interface**

This commonly happens when an IP address is assigned to a physical port that belongs to a bridge.

For a bridged LAN, assign the gateway address to the bridge interface. For a VLAN network, assign it to the corresponding VLAN interface.

# 🗁 Practical IP Address Configuration Lab

**Lab topology**

```text
                 MikroTik Router
                       |
                    ether2
                       |
                   LAN Switch
                  /          \
              PC-1            PC-2
```

**Network requirements**

| Parameter | Value |
| --- | --- |
| Router interface | bridge-LAN |
| LAN network | 192.168.10.0/24 |
| Router IP | 192.168.10.1/24 |
| PC-1 IP | 192.168.10.10/24 |
| PC-2 IP | 192.168.10.20/24 |
| Default gateway | 192.168.10.1 |

**Create the bridge**

```routeros
/interface bridge
add name=bridge-LAN
```

**Add the Ethernet interface**

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
```

**Assign the LAN gateway**

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN comment="LAN Gateway"
```

**Configure PC-1**

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
DNS Server:      1.1.1.1
```

**Configure PC-2**

```text
IP Address:      192.168.10.20
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
DNS Server:      1.1.1.1
```

**Test connectivity**

From the MikroTik router:

```routeros
/ping 192.168.10.10
/ping 192.168.10.20
```

From PC-1:

```bash
ping 192.168.10.1
ping 192.168.10.20
```

Expected results:

* Both PCs communicate with the router.
* Both PCs communicate with each other.
* The router has a connected route for `192.168.10.0/24`.

# 🗁 Best Practices

* Use a consistent IP addressing plan.
* Document network prefixes and gateway addresses.
* Assign static IP addresses to infrastructure interfaces.
* Use DHCP for ordinary client devices when appropriate.
* Assign IP addresses to bridge interfaces instead of their member ports.
* Assign VLAN gateway addresses to the corresponding VLAN interfaces.
* Avoid overlapping IPv4 subnets on unintended interfaces.
* Use `/32` addresses for loopback identification where appropriate.
* Verify connected routes after assigning addresses.
* Keep management access available when changing router addresses.
* Use meaningful comments for IP address entries.
* Back up the router configuration before major network changes.

# 🗁 Official Documentation

* MikroTik IP Addressing: <https://help.mikrotik.com/docs/>
* MikroTik IP Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing>
* MikroTik DHCP: <https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP>
