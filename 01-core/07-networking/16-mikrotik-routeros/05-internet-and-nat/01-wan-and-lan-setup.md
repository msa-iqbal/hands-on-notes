# WAN and LAN Setup

A MikroTik router commonly connects two different network environments:

* **WAN (Wide Area Network):** Connects the router to an upstream network, ISP, or the internet.
* **LAN (Local Area Network):** Connects local devices such as computers, servers, access points, and switches.

The router operates between these networks and provides connectivity through routing, firewall policies, and Network Address Translation (NAT), when required.

A typical network looks like this:

```text
                  Internet
                     |
                  ISP Router
                     |
                  WAN Link
                     |
               ether1 (WAN)
               MikroTik Router
               bridge-LAN
                     |
          +----------+----------+
          |          |          |
       ether2     ether3     ether4
          |          |          |
         PC       Switch      Access Point
                     |
                  Clients
```

# 🗁 WAN Interface

The WAN interface connects the MikroTik router to an upstream network.

Depending on the ISP connection, the WAN interface may receive its configuration through:

* DHCP client.
* Static IPv4 configuration.
* PPPoE client.
* VLAN-based ISP connection.
* IPv6 configuration.

Common WAN interface names include:

```text
ether1
sfp1
pppoe-out1
vlan100
```

The actual interface depends on the router hardware and ISP requirements.

# 🗁 LAN Interface

The LAN side provides connectivity to internal network devices.

Multiple physical interfaces can be grouped into a single bridge so that connected devices belong to the same Layer 2 network.

Example LAN configuration:

| Property | Value |
| --- | --- |
| LAN Bridge | `bridge-LAN` |
| LAN Network | `192.168.10.0/24` |
| Router IP | `192.168.10.1` |
| DHCP Range | `192.168.10.100-192.168.10.200` |
| Gateway | `192.168.10.1` |
| DNS | `192.168.10.1` |

# 🗁 Example Network Plan

Before configuring WAN and LAN, define the network interfaces and addressing plan.

| Component | Interface | Configuration |
| --- | --- | --- |
| ISP connection | `ether1` | DHCP client |
| LAN bridge | `bridge-LAN` | `192.168.10.1/24` |
| LAN port 1 | `ether2` | Bridge member |
| LAN port 2 | `ether3` | Bridge member |
| LAN port 3 | `ether4` | Bridge member |
| LAN port 4 | `ether5` | Bridge member |
| DHCP pool | `192.168.10.100-192.168.10.200` | Dynamic allocation |

This example assumes that the ISP provides an IPv4 address through DHCP.

If the ISP uses PPPoE, static addressing, or VLAN tagging, the WAN configuration must be adapted accordingly.

# 🗁 Inspecting Router Interfaces

Before making changes, inspect the available interfaces.

Display all interfaces:

```routeros
/interface print
```

Display detailed interface information:

```routeros
/interface print detail
```

Check Ethernet interfaces:

```routeros
/interface ethernet print
```

Check interface status:

```routeros
/interface print where running=yes
```

Important interface properties include:

| Property | Description |
| --- | --- |
| `name` | Interface name |
| `type` | Interface type |
| `mtu` | Maximum Transmission Unit |
| `mac-address` | Hardware MAC address |
| `running` | Indicates whether the interface is operational |
| `disabled` | Indicates whether the interface is disabled |

# 🗁 Creating the LAN Bridge

A bridge combines multiple interfaces into one Layer 2 network.

Create a LAN bridge:

```routeros
/interface bridge add name=bridge-LAN comment="Main LAN Bridge"
```

Verify the bridge:

```routeros
/interface bridge print
```

Add LAN Ethernet interfaces to the bridge:

```routeros
/interface bridge port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
add bridge=bridge-LAN interface=ether4
add bridge=bridge-LAN interface=ether5
```

Verify bridge ports:

```routeros
/interface bridge port print
```

The physical interfaces added to the bridge now participate in the same Layer 2 LAN, subject to the bridge and VLAN configuration.

Do not add the WAN interface to the LAN bridge unless the network design specifically requires it.

# 🗁 Assigning an IPv4 Address to the LAN

The router needs an IP address on the LAN so that clients can use it as their default gateway.

Assign an address to the bridge:

```routeros
/ip address add address=192.168.10.1/24 interface=bridge-LAN comment="LAN Gateway"
```

Verify the address:

```routeros
/ip address print
```

Display detailed information:

```routeros
/ip address print detail
```

The resulting network is:

| Property | Value |
| --- | --- |
| Network | `192.168.10.0/24` |
| Network Address | `192.168.10.0` |
| Router Address | `192.168.10.1` |
| Broadcast Address | `192.168.10.255` |
| Usable Host Range | `192.168.10.1-192.168.10.254` |

Assign the LAN address to the bridge rather than separately assigning the same gateway address to every physical bridge port.

# 🗁 Configuring the WAN with DHCP Client

Many ISPs provide WAN IPv4 configuration automatically through DHCP.

A DHCP client can obtain:

* WAN IPv4 address.
* Subnet information.
* Default gateway.
* DNS server information, depending on configuration.

Add a DHCP client to the WAN interface:

```routeros
/ip dhcp-client add interface=ether1 add-default-route=yes use-peer-dns=yes disabled=no comment="ISP WAN DHCP"
```

Verify the DHCP client:

```routeros
/ip dhcp-client print
```

Display detailed information:

```routeros
/ip dhcp-client print detail
```

A successfully bound DHCP client should show the assigned address and gateway information.

The `add-default-route=yes` setting allows the DHCP client to install a default route dynamically. Default route behavior is covered separately in `02-default-route.md`.

The `use-peer-dns=yes` setting allows RouterOS to use DNS servers provided by the ISP.

If you want to configure DNS manually, you can use:

```routeros
/ip dns set servers=1.1.1.1,8.8.8.8
```

# 🗁 WAN Configuration with a Static IP

Some ISPs provide a fixed IPv4 address and gateway instead of DHCP.

For example:

| Property | Value |
| --- | --- |
| WAN Address | `203.0.113.2/30` |
| Gateway | `203.0.113.1` |
| Interface | `ether1` |

The addresses in this example are documentation addresses, not usable public production addresses.

Assign the WAN address:

```routeros
/ip address add address=203.0.113.2/30 interface=ether1 comment="Static WAN IP"
```

The default route can be configured separately:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=203.0.113.1 comment="ISP Default Route"
```

Verify the address:

```routeros
/ip address print where interface=ether1
```

Verify the route:

```routeros
/ip route print
```

Do not configure both a DHCP client and a static address on the same WAN interface unless the ISP and network design explicitly require that arrangement.

# 🗁 Configuring LAN DHCP

A DHCP server automatically provides IP configuration to LAN clients.

The usual DHCP parameters include:

* IP address.
* Subnet mask.
* Default gateway.
* DNS server.
* Lease time.

Create an IP pool:

```routeros
/ip pool add name=pool-LAN ranges=192.168.10.100-192.168.10.200
```

Create the DHCP server:

```routeros
/ip dhcp-server add name=dhcp-LAN interface=bridge-LAN address-pool=pool-LAN lease-time=1d disabled=no
```

Configure the DHCP network:

```routeros
/ip dhcp-server network add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

Verify the DHCP server:

```routeros
/ip dhcp-server print
```

Verify the DHCP pool:

```routeros
/ip pool print
```

Inspect active DHCP leases:

```routeros
/ip dhcp-server lease print
```

The DHCP server now provides clients with addresses from the configured pool.

# 🗁 Configuring Router DNS

RouterOS can provide DNS forwarding for LAN clients.

Enable DNS requests from LAN clients:

```routeros
/ip dns set allow-remote-requests=yes
```

Configure upstream DNS servers:

```routeros
/ip dns set servers=1.1.1.1,8.8.8.8
```

Inspect DNS settings:

```routeros
/ip dns print
```

When the DHCP network advertises `192.168.10.1` as its DNS server, clients send DNS queries to the MikroTik router.

RouterOS then forwards those queries to its configured upstream DNS servers.

**Important:** Allow DNS requests from trusted LAN interfaces only. Do not expose the router's DNS forwarding service to untrusted WAN networks.

# 🗁 Basic WAN and LAN Firewall Protection

A router connected to the internet should have appropriate firewall rules.

RouterOS distinguishes between three main filter chains:

| Chain | Purpose |
| --- | --- |
| `input` | Traffic destined for the router itself |
| `forward` | Traffic passing through the router |
| `output` | Traffic generated by the router |

The input chain should protect router management services from unauthorized WAN access.

The forward chain should control traffic between LAN, WAN, and other routed networks.

Inspect existing firewall rules:

```routeros
/ip firewall filter print
```

A basic stateful firewall policy for a simple IPv4 WAN/LAN router can use the following rules, provided they are added in the correct order and existing rules are reviewed first:

```routeros
/ip firewall filter
add chain=input action=accept connection-state=established,related,untracked comment="Accept established and related input"
add chain=input action=drop connection-state=invalid comment="Drop invalid input"
add chain=input action=accept in-interface=bridge-LAN comment="Allow LAN access to router"
add chain=input action=drop in-interface=ether1 comment="Block unsolicited WAN access to router"

add chain=forward action=accept connection-state=established,related,untracked comment="Accept established and related forwarding"
add chain=forward action=drop connection-state=invalid comment="Drop invalid forwarding"
add chain=forward action=accept in-interface=bridge-LAN out-interface=ether1 comment="Allow LAN to WAN"
add chain=forward action=drop in-interface=ether1 connection-state=new connection-nat-state=!dstnat comment="Block unsolicited WAN forwarding"
```

These rules are an illustrative baseline for a simple topology. They are not a universal replacement for a complete firewall policy.

Before applying them to an existing router, inspect rule order and existing protections. Ensure that required management, VPN, IPv6, and inter-VLAN traffic is handled by the intended policy.

# 🗁 Understanding the Complete Traffic Flow

After configuring WAN and LAN, a typical client internet connection follows this sequence:

* The client receives an IPv4 address through DHCP.
* The client receives the MikroTik LAN address as its default gateway.
* The client sends traffic for external destinations to the gateway.
* RouterOS checks its routing table.
* The router forwards the packet through the WAN interface.
* Source NAT or masquerade translates the private source address when required.
* The upstream network forwards the traffic toward the destination.
* Return traffic is processed through connection tracking and NAT.

The overall flow:

```text
LAN Client
192.168.10.100
      |
      v
MikroTik LAN Gateway
192.168.10.1
      |
      v
Routing Table
      |
      v
Source NAT / Masquerade
      |
      v
MikroTik WAN Interface
      |
      v
ISP Gateway
      |
      v
Internet
```

WAN connectivity, default routing, and NAT are separate configuration elements. A working WAN interface alone does not guarantee that LAN clients can access the internet.

# 🗁 Verification and Troubleshooting

**Check interface status**

```routeros
/interface print
```

Confirm that the WAN and LAN interfaces are enabled and operational.

**Check WAN address**

```routeros
/ip address print
```

For DHCP-based WAN connections:

```routeros
/ip dhcp-client print detail
```

Confirm that the DHCP client is bound and has received an address.

**Check LAN address**

```routeros
/ip address print where interface=bridge-LAN
```

Confirm that the LAN gateway address is configured correctly.

**Check bridge ports**

```routeros
/interface bridge port print
```

Confirm that the intended LAN interfaces belong to the bridge.

**Check DHCP leases**

```routeros
/ip dhcp-server lease print
```

Confirm that LAN clients receive valid addresses from the configured pool.

**Check DNS configuration**

```routeros
/ip dns print
```

**Check default route**

```routeros
/ip route print
```

A usable IPv4 default route should exist for internet-bound traffic.

**Test the ISP gateway**

```routeros
/ping 203.0.113.1
```

Replace the example gateway with the actual ISP gateway.

**Test external IP connectivity**

```routeros
/ping 1.1.1.1
```

**Test DNS resolution**

```routeros
:put [:resolve example.com]
```

These tests help isolate whether the problem is related to the physical connection, WAN configuration, routing, NAT, firewall, or DNS.

# 🗁 Common WAN and LAN Problems

| Problem | Possible Cause | What to Check |
| --- | --- | --- |
| WAN has no IP address | ISP DHCP unavailable or incorrect WAN configuration | DHCP client and physical link |
| LAN clients receive no IP | DHCP server or pool misconfiguration | DHCP server, pool, bridge |
| LAN clients cannot reach router | Incorrect gateway or bridge configuration | LAN IP and bridge ports |
| Router cannot reach internet | Missing default route or upstream connectivity | Routing table and WAN |
| Router reaches internet but clients cannot | Missing NAT or forwarding policy | NAT and forward chain |
| IP connectivity works but domains fail | DNS configuration problem | DNS servers and DHCP network |
| WAN access works intermittently | Link instability or ISP issues | Interface status and logs |

# 🗁 Best Practices

* Separate WAN and LAN interfaces clearly.
* Use a bridge for LAN ports that belong to the same Layer 2 network.
* Assign the LAN gateway address to the bridge.
* Keep WAN and LAN addressing separate.
* Use DHCP client, static addressing, or PPPoE according to ISP requirements.
* Configure DHCP pools that do not overlap with reserved static addresses.
* Protect router management services from WAN access.
* Apply appropriate input and forward firewall policies.
* Configure DNS forwarding only for trusted networks.
* Document interface names, IP addresses, gateways, and ISP connection details.
* Review existing configuration before applying new commands to a production router.

# 🗁 Official Documentation

* [MikroTik RouterOS — IP Addressing](https://help.mikrotik.com/docs/spaces/ROS/pages/328247/IP+Addressing)
* [MikroTik RouterOS — DHCP](https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP)
* [MikroTik RouterOS — DNS](https://help.mikrotik.com/docs/spaces/ROS/pages/37748767/DNS)
* [MikroTik RouterOS — NAT](https://help.mikrotik.com/docs/spaces/ROS/pages/3211299/NAT)
* [MikroTik RouterOS — Building Advanced Firewall](https://help.mikrotik.com/docs/spaces/ROS/pages/328513/Building+Advanced+Firewall)
