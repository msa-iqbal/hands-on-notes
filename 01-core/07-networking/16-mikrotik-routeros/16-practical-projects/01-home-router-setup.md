# Home Router Setup

A home router provides internet connectivity and network access to devices such as computers, smartphones, smart TVs, printers, and IoT devices.

MikroTik RouterOS offers advanced routing, firewall, DHCP, NAT, wireless, and bandwidth-management capabilities that can be used to build a secure and manageable home network.

This practical project demonstrates how to configure a MikroTik router for a typical home environment.

# 🗁 Project Objectives

By completing this project, you will learn how to:

* Configure router identity and administrative access.
* Configure WAN and LAN interfaces.
* Assign IP addresses.
* Configure a DHCP server.
* Configure DNS.
* Enable internet access through NAT.
* Configure firewall protection.
* Configure wireless networking where supported.
* Test internet connectivity.
* Monitor connected devices and network traffic.

# 🗁 Network Topology

The example network uses one WAN connection and one LAN network.

```text
                   Internet
                       |
                       |
                  ISP Modem
                       |
                       | WAN
                       |
                +---------------+
                | MikroTik      |
                | Router        |
                |               |
                | ether1 = WAN  |
                | ether2 = LAN  |
                +---------------+
                       |
                       |
                  LAN Switch
                       |
             +---------+---------+
             |         |         |
           PC        Laptop    Smart TV
             |
          Smartphone
        (Wi-Fi, if supported)

```

# 🗁 Network Addressing Plan

Use the following example addressing scheme:

| Component | Configuration |
| --- | --- |
| Router LAN IP | `192.168.10.1/24` |
| LAN network | `192.168.10.0/24` |
| DHCP range | `192.168.10.100-192.168.10.200` |
| WAN interface | `ether1` |
| LAN interface | `ether2` |
| DNS servers | `1.1.1.1`, `8.8.8.8` |
| NAT type | Masquerade |

The WAN interface receives its IP configuration from the ISP through DHCP in this example.

If your ISP uses PPPoE, a static IP address, or VLAN tagging, the WAN configuration must be adjusted accordingly.

# 🗁 Preparing the Router

Connect to the MikroTik router using WinBox or another supported management method.

Before configuring the router:

* Identify the WAN interface connected to the ISP.
* Identify the LAN interface connected to your local network.
* Confirm that you have administrative access.
* Save a backup of the existing configuration if the router has already been configured.
* Avoid removing the active management connection before confirming the new configuration works.

For a fresh installation, use a clean configuration or reset the router according to the device's deployment requirements.

# 🗁 Setting Router Identity

Router identity helps identify the device in management tools, logs, and monitoring systems.

```routeros
/system/identity/set name=Home-Router
```

Verify the identity:

```routeros
/system/identity/print
```

# 🗁 Configuring the WAN Interface

The WAN interface connects the router to the ISP modem or upstream network.

For an ISP connection that provides an address through DHCP:

```routeros
/ip/dhcp-client/add interface=ether1 add-default-route=yes use-peer-dns=no disabled=no
```

This configuration:

* Requests an IP address from the upstream DHCP server.
* Installs a default route through the received gateway.
* Does not automatically use the ISP-provided DNS servers.

Inspect the DHCP client:

```routeros
/ip/dhcp-client/print detail
```

Check the assigned address:

```routeros
/ip/address/print
```

Check the routing table:

```routeros
/ip/route/print
```

If your ISP requires PPPoE, configure a PPPoE client instead of a DHCP client on the WAN interface.

# 🗁 Configuring the LAN Interface

Assign a private IP address to the LAN interface.

```routeros
/ip/address/add address=192.168.10.1/24 interface=ether2 comment="Home LAN Gateway"
```

This address becomes the default gateway for devices connected to the LAN.

Verify the address:

```routeros
/ip/address/print
```

# 🗁 Configuring DHCP Server

DHCP automatically assigns network configuration to connected devices.

The DHCP configuration includes:

* IP address.
* Subnet information.
* Default gateway.
* DNS server addresses.
* Lease duration.

Create an IP pool:

```routeros
/ip/pool/add name=home-dhcp-pool ranges=192.168.10.100-192.168.10.200
```

Create the DHCP server:

```routeros
/ip/dhcp-server/add name=home-dhcp interface=ether2 address-pool=home-dhcp-pool lease-time=12h disabled=no
```

Configure the DHCP network:

```routeros
/ip/dhcp-server/network/add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

Verify the DHCP server:

```routeros
/ip/dhcp-server/print
/ip/dhcp-server/lease/print
```

# 🗁 Configuring DNS

Configure external DNS resolvers:

```routeros
/ip/dns/set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

This configuration allows the router to resolve DNS queries and provide DNS service to LAN clients.

Because the router is accepting DNS requests from clients, protect the DNS service with appropriate input firewall rules. Do not expose recursive DNS service to untrusted WAN networks.

Verify DNS settings:

```routeros
/ip/dns/print
```

# 🗁 Configuring NAT

Network Address Translation allows private LAN devices to access the internet through the WAN interface.

Create a masquerade rule:

```routeros
/ip/firewall/nat/add chain=srcnat action=masquerade out-interface=ether1 comment="Home LAN Internet NAT"
```

The rule translates LAN source addresses when traffic leaves through `ether1`.

Verify the NAT configuration:

```routeros
/ip/firewall/nat/print
```

NAT does not replace firewall filtering. The firewall must separately control which traffic is allowed to reach the router and pass through it.

# 🗁 Configuring Firewall Protection

The firewall should protect the router's management plane and prevent unsolicited WAN traffic from reaching the LAN.

The following example assumes:

* `ether1` is the WAN interface.
* `ether2` is the LAN interface.
* The router has the LAN address `192.168.10.1/24`.
* There are no existing rules that conflict with this policy.

Create an interface list for WAN:

```routeros
/interface/list/add name=WAN
/interface/list/member/add list=WAN interface=ether1
```

Create an interface list for LAN:

```routeros
/interface/list/add name=LAN
/interface/list/member/add list=LAN interface=ether2
```

Add input-chain protection:

```routeros
/ip/firewall/filter/add chain=input action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=input action=drop connection-state=invalid comment="Drop invalid connections"
/ip/firewall/filter/add chain=input action=accept in-interface-list=LAN comment="Allow LAN access to router"
/ip/firewall/filter/add chain=input action=drop in-interface-list=WAN comment="Drop unsolicited WAN access to router"
```

Add forward-chain protection:

```routeros
/ip/firewall/filter/add chain=forward action=accept connection-state=established,related,untracked comment="Accept established and related forwarding"
/ip/firewall/filter/add chain=forward action=drop connection-state=invalid comment="Drop invalid forwarded connections"
/ip/firewall/filter/add chain=forward action=accept in-interface-list=LAN out-interface-list=WAN comment="Allow LAN to WAN"
/ip/firewall/filter/add chain=forward action=drop in-interface-list=WAN connection-state=new connection-nat-state=!dstnat comment="Drop unsolicited WAN forwarding"
```

The input chain controls traffic destined for the router itself. The forward chain controls traffic passing through the router.

Review rule order carefully. These examples are a basic IPv4 policy and may need additional rules for VPN access, port forwarding, IPv6, inter-VLAN routing, and other services.

For an IPv6-enabled network, configure an appropriate IPv6 firewall policy separately.

# 🗁 Configuring Wireless Networking

Wireless configuration depends on the MikroTik hardware and installed wireless package.

Some devices use the newer WiFi configuration menu, while others use the legacy wireless menu.

For devices using the legacy wireless interface:

```routeros
/interface/wireless/set [find default-name=wlan1] mode=ap-bridge ssid=Home-WiFi disabled=no
```

Create a WPA2 security profile:

```routeros
/interface/wireless/security-profiles/add name=home-security authentication-types=wpa2-psk encryption=aes-ccm wpa2-pre-shared-key="StrongWirelessPassword"
```

Assign the security profile:

```routeros
/interface/wireless/set [find default-name=wlan1] security-profile=home-security
```

For a wireless interface connected to the same LAN, add it to a bridge with the LAN interface. The bridge then becomes the shared Layer 2 network.

Example bridge configuration:

```routeros
/interface/bridge/add name=home-bridge
/interface/bridge/port/add bridge=home-bridge interface=ether2
/interface/bridge/port/add bridge=home-bridge interface=wlan1
```

If you use this bridge configuration, assign the LAN IP address and DHCP server to `home-bridge` instead of `ether2`.

Do not apply this bridge example on top of the earlier standalone `ether2` LAN configuration without migrating the IP address and DHCP server interface. Avoid placing an interface in more than one bridge.

For newer WiFi interfaces, use the `/interface/wifi` configuration menu and its corresponding security settings.

# 🗁 Securing Router Management

Restrict management services to the LAN network.

Inspect available services:

```routeros
/ip/service/print
```

Disable unused services:

```routeros
/ip/service/disable telnet
/ip/service/disable ftp
/ip/service/disable www
```

Restrict WinBox access:

```routeros
/ip/service/set winbox address=192.168.10.0/24
```

Restrict SSH access:

```routeros
/ip/service/set ssh address=192.168.10.0/24
```

Use strong administrative credentials:

```routeros
/user/set [find name=admin] password="StrongUniqueAdminPassword"
```

If the default administrator account has a different name or has been replaced, select the correct account.

# 🗁 Testing Internet Connectivity

After completing the configuration, test connectivity in stages.

Check the router's WAN address:

```routeros
/ip/address/print
```

Test the upstream gateway:

```routeros
/ping 192.0.2.1
```

The gateway address above is illustrative. Replace it with the actual ISP gateway.

Test an external IP address:

```routeros
/ping 1.1.1.1
```

Test DNS resolution:

```routeros
/ping google.com
```

If external IP connectivity works but DNS resolution fails, investigate DNS configuration and upstream DNS reachability.

From a LAN client, verify:

* It receives an IP address in `192.168.10.100-192.168.10.200`.
* Its default gateway is `192.168.10.1`.
* Its DNS server is `192.168.10.1`.
* It can reach the router.
* It can access external IP addresses.
* It can resolve domain names.

# 🗁 Monitoring Connected Devices

View DHCP leases:

```routeros
/ip/dhcp-server/lease/print
```

View ARP entries:

```routeros
/ip/arp/print
```

Monitor interface traffic:

```routeros
/interface/monitor-traffic ether2
```

View active connections:

```routeros
/ip/firewall/connection/print
```

These commands help identify connected clients, traffic activity, and active network sessions.

# 🗁 Common Troubleshooting Scenarios

| Problem | Possible cause | Investigation |
| --- | --- | --- |
| No WAN address | ISP DHCP unavailable or incorrect interface | Check DHCP client |
| LAN clients receive no IP | DHCP server or pool misconfiguration | Check DHCP server and leases |
| Clients receive IP but no internet | Missing default route or NAT | Check routes and NAT |
| IP connectivity works but websites fail | DNS configuration problem | Check DNS settings |
| Router is unreachable | Incorrect LAN IP or firewall rule | Check IP and input chain |
| Wi-Fi is unavailable | Wireless interface disabled or unsupported configuration | Check wireless interface |
| Some websites fail | MTU, DNS, or upstream issue | Investigate path and packet behavior |

Useful diagnostic commands:

```routeros
/ip/dhcp-client/print detail
/ip/dhcp-server/print detail
/ip/route/print
/ip/firewall/filter/print
/ip/firewall/nat/print
/ip/dns/print
/interface/print
```

# 🗁 Final Configuration Checklist

* [ ] Router identity configured.
* [ ] Administrative credentials secured.
* [ ] WAN interface configured.
* [ ] WAN connectivity verified.
* [ ] LAN IP address configured.
* [ ] DHCP pool created.
* [ ] DHCP server configured.
* [ ] DNS configured.
* [ ] NAT rule created.
* [ ] Firewall input policy configured.
* [ ] Firewall forward policy configured.
* [ ] Wireless security configured, if applicable.
* [ ] Management services restricted.
* [ ] Client connectivity tested.
* [ ] Configuration backup created.

# 🗁 Expected Result

After completing this project, the MikroTik router should provide:

* Internet connectivity through the ISP.
* Private LAN connectivity.
* Automatic IP address assignment.
* DNS resolution for LAN clients.
* NAT-based internet access.
* Basic firewall protection.
* Secure router management.
* Wireless access where supported by the device.

This project establishes a foundation for more advanced network deployments involving VLAN segmentation, multi-WAN failover, VPN connectivity, and bandwidth management.
