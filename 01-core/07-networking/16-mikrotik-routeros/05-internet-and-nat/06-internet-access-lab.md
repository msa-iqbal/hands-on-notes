# Internet Access Lab

This practical lab demonstrates how to configure a MikroTik RouterOS v7 router to provide internet access to devices on a private LAN network.

You will configure WAN and LAN interfaces, IP addressing, DHCP, DNS, default routing, Source NAT, and firewall rules.

By completing this lab, you will understand how the major components work together to provide internet connectivity.

# 🗁 Lab Objectives

After completing this lab, you should be able to:

* Configure WAN and LAN interfaces.
* Assign IP addresses to router interfaces.
* Configure a DHCP client for a dynamic WAN connection.
* Configure a default route.
* Configure DNS resolution.
* Create a DHCP server for LAN clients.
* Configure Source NAT using Masquerade.
* Configure firewall filter rules.
* Verify internet connectivity.
* Troubleshoot common internet access problems.

# 🗁 Network Topology

The lab uses a MikroTik router connected to an upstream internet gateway and an internal LAN.

```text
                 Internet
                     |
                     |
              ISP Gateway
               192.0.2.1
                     |
                     |
                 ether1
                     |
              MikroTik Router
                     |
                 ether2
                     |
              LAN Network
             192.168.10.0/24
                     |
          +----------+----------+
          |          |          |
        PC-01      PC-02      PC-03
```

# 🗁 Network Addressing Plan

| Component | Interface | IP Address |
| --- | --- | --- |
| ISP Gateway | Upstream | `192.0.2.1` |
| MikroTik WAN | `ether1` | DHCP assigned |
| MikroTik LAN | `ether2` | `192.168.10.1/24` |
| LAN Network | - | `192.168.10.0/24` |
| DHCP Pool | - | `192.168.10.100-192.168.10.200` |
| DNS Server | - | `1.1.1.1`, `8.8.8.8` |
| Default Gateway | - | `192.168.10.1` |

The ISP gateway address is an example documentation address. In a real network, use the actual upstream gateway and WAN configuration supplied by the ISP or lab environment.

# 🗁 Lab Requirements

You need:

* One MikroTik router running RouterOS v7.
* One WAN connection or simulated upstream network.
* One LAN interface connected to a switch or client device.
* One or more LAN client devices.
* WinBox, WebFig, or SSH access to the router.

You can also complete this lab using MikroTik CHR in VirtualBox, VMware, GNS3, or EVE-NG.

# 🗁 Initial Router Inspection

Before changing the configuration, inspect the router.

Display available interfaces:

```routeros
/interface/print
```

Display existing IP addresses:

```routeros
/ip/address/print
```

Display existing routes:

```routeros
/ip/route/print
```

Display existing DHCP clients:

```routeros
/ip/dhcp-client/print
```

Display existing NAT rules:

```routeros
/ip/firewall/nat/print
```

If you are using a router with an existing configuration, identify conflicting settings before applying the lab configuration.

# 🗁 Configure WAN Interface

The WAN interface connects the MikroTik router to the ISP or upstream gateway.

In this lab, the upstream network provides an IP address through DHCP.

Configure the DHCP client:

```routeros
/ip/dhcp-client
add interface=ether1 add-default-route=yes use-peer-dns=no disabled=no comment="WAN DHCP Client"
```

Explanation:

* `interface=ether1`: Uses `ether1` as the WAN interface.
* `add-default-route=yes`: Installs a default route received through DHCP.
* `use-peer-dns=no`: Does not automatically use DNS servers provided by the upstream DHCP server.
* `disabled=no`: Enables the DHCP client.

Verify the WAN DHCP client:

```routeros
/ip/dhcp-client/print detail
```

Check the assigned WAN address:

```routeros
/ip/address/print
```

Check the default route:

```routeros
/ip/route/print
```

The DHCP client should obtain a valid address and gateway from the upstream network.

# 🗁 Configure LAN Interface

The LAN interface connects internal devices to the MikroTik router.

Assign the LAN gateway address:

```routeros
/ip/address
add address=192.168.10.1/24 interface=ether2 comment="LAN Gateway"
```

This establishes the router's LAN address.

Display the configured address:

```routeros
/ip/address/print
```

LAN clients will use `192.168.10.1` as their default gateway.

# 🗁 Create Interface Lists

Interface lists make firewall and NAT configuration easier to maintain.

Create WAN and LAN interface lists:

```routeros
/interface/list
add name=WAN
add name=LAN
```

Add the WAN interface:

```routeros
/interface/list/member
add list=WAN interface=ether1
```

Add the LAN interface:

```routeros
/interface/list/member
add list=LAN interface=ether2
```

Verify interface list membership:

```routeros
/interface/list/member/print
```

These lists will be used in the firewall and NAT configuration.

# 🗁 Configure DNS

DNS translates domain names into IP addresses.

For example:

```text
example.com -> Server IP Address
```

Configure DNS servers:

```routeros
/ip/dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

Explanation:

* `servers=1.1.1.1,8.8.8.8`: Configures the upstream DNS resolvers.
* `allow-remote-requests=yes`: Allows LAN clients to use the MikroTik router as a DNS resolver.

LAN clients will use the router's LAN address as their DNS server.

Verify the DNS configuration:

```routeros
/ip/dns/print
```

# 🗁 Configure DHCP Pool

A DHCP pool defines the IP address range available for dynamic client assignments.

Create a pool:

```routeros
/ip/pool
add name=LAN-POOL ranges=192.168.10.100-192.168.10.200
```

The router can now assign addresses from this range to LAN clients.

Verify the pool:

```routeros
/ip/pool/print
```

# 🗁 Configure DHCP Server

Create a DHCP server on the LAN interface:

```routeros
/ip/dhcp-server
add name=LAN-DHCP interface=ether2 address-pool=LAN-POOL lease-time=1d disabled=no
```

Create the DHCP network configuration:

```routeros
/ip/dhcp-server/network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1 comment="LAN DHCP Network"
```

Explanation:

* `address=192.168.10.0/24`: Defines the LAN network.
* `gateway=192.168.10.1`: Specifies the default gateway.
* `dns-server=192.168.10.1`: Specifies the MikroTik router as the DNS server.

Verify the DHCP server:

```routeros
/ip/dhcp-server/print
```

Check active DHCP leases:

```routeros
/ip/dhcp-server/lease/print
```

LAN clients configured for automatic addressing should now receive their IP configuration from the MikroTik router.

# 🗁 Configure Source NAT

Source NAT allows private LAN devices to access the internet through the router's WAN connection.

Create a Masquerade rule:

```routeros
/ip/firewall/nat
add chain=srcnat out-interface-list=WAN action=masquerade comment="LAN Internet Access"
```

Explanation:

* `chain=srcnat`: Processes outgoing packets in the Source NAT chain.
* `out-interface-list=WAN`: Matches packets leaving through a WAN interface.
* `action=masquerade`: Translates the source IP address to the current address of the outgoing interface.

Verify the NAT rule:

```routeros
/ip/firewall/nat/print detail
```

Check its packet and byte counters:

```routeros
/ip/firewall/nat/print stats
```

# 🗁 Configure Firewall Filter Rules

Firewall filter rules control which packets are accepted or dropped.

The following example provides a basic stateful firewall policy for this lab.

**Allow established and related connections:**

```routeros
/ip/firewall/filter
add chain=input connection-state=established,related action=accept comment="Allow Established and Related Input"
add chain=forward connection-state=established,related action=accept comment="Allow Established and Related Forward"
```

**Drop invalid connections:**

```routeros
/ip/firewall/filter
add chain=input connection-state=invalid action=drop comment="Drop Invalid Input"
add chain=forward connection-state=invalid action=drop comment="Drop Invalid Forward"
```

**Allow ICMP traffic:**

```routeros
/ip/firewall/filter
add chain=input protocol=icmp action=accept comment="Allow ICMP to Router"
```

**Allow management access from LAN:**

```routeros
/ip/firewall/filter
add chain=input in-interface-list=LAN action=accept comment="Allow LAN Management"
```

**Allow LAN clients to access external networks:**

```routeros
/ip/firewall/filter
add chain=forward in-interface-list=LAN out-interface-list=WAN action=accept comment="Allow LAN to WAN"
```

**Drop unsolicited incoming WAN traffic to the router:**

```routeros
/ip/firewall/filter
add chain=input in-interface-list=WAN action=drop comment="Drop Unsolicited WAN Input"
```

**Drop other forwarded traffic:**

```routeros
/ip/firewall/filter
add chain=forward action=drop comment="Drop Other Forwarded Traffic"
```

These rules are intended for a simple lab with one LAN and one WAN. Their order is important. Place the established/related and invalid-state rules before the general allow and drop rules.

If the router has existing firewall rules, integrate the policy carefully instead of blindly appending duplicate or conflicting rules.

# 🗁 Verify WAN Connectivity

Test connectivity to the upstream gateway:

```routeros
/ping 192.0.2.1
```

Replace the example gateway with the actual gateway assigned by the upstream network.

Test connectivity to an external IP address:

```routeros
/ping 1.1.1.1
```

If the router can reach an external IP address, basic WAN routing is working.

# 🗁 Verify DNS Resolution

Test domain name resolution:

```routeros
:resolve example.com
```

You can also test DNS using:

```routeros
/ping example.com
```

If IP connectivity works but DNS resolution fails, investigate the DNS configuration and upstream DNS reachability.

# 🗁 Verify LAN Client Connectivity

Connect a client to the LAN interface or switch.

Configure the client to obtain its network settings automatically through DHCP.

Check the assigned configuration.

Expected configuration:

| Setting | Expected Value |
| --- | --- |
| IP Address | `192.168.10.100-192.168.10.200` |
| Subnet Mask | `255.255.255.0` |
| Default Gateway | `192.168.10.1` |
| DNS Server | `192.168.10.1` |

From a Linux client, inspect the assigned address:

```bash
ip address
```

Inspect the routing table:

```bash
ip route
```

Inspect DNS configuration:

```bash
resolvectl status
```

Test the router's LAN address:

```bash
ping 192.168.10.1
```

Test external IP connectivity:

```bash
ping 1.1.1.1
```

Test DNS resolution:

```bash
getent hosts example.com
```

If all tests succeed, the client has working internet connectivity.

# 🗁 Verify NAT and Firewall Counters

Check NAT statistics:

```routeros
/ip/firewall/nat/print stats
```

Check firewall filter statistics:

```routeros
/ip/firewall/filter/print stats
```

Increasing counters help identify which rules are processing traffic.

If LAN clients can reach the router but cannot access external IP addresses, inspect:

* WAN DHCP client status.
* Default route.
* Source NAT rule.
* Firewall forward-chain rules.
* Upstream gateway connectivity.

# 🗁 Troubleshooting Common Problems

**LAN clients do not receive IP addresses**

Check the DHCP server:

```routeros
/ip/dhcp-server/print detail
/ip/dhcp-server/lease/print
```

Check the DHCP pool:

```routeros
/ip/pool/print
```

Verify that the DHCP server is enabled, attached to the correct LAN interface, and using the correct address pool.

**Router cannot reach the internet**

Check the WAN DHCP client:

```routeros
/ip/dhcp-client/print detail
```

Check the default route:

```routeros
/ip/route/print
```

Verify the upstream gateway and WAN connectivity.

**LAN clients can ping the router but not external IP addresses**

Check:

```routeros
/ip/firewall/nat/print stats
/ip/firewall/filter/print stats
/ip/route/print
```

Verify that Source NAT is matching outgoing LAN traffic and that the forward chain allows LAN-to-WAN connections.

**Clients can access external IP addresses but cannot resolve domain names**

Check DNS configuration:

```routeros
/ip/dns/print
```

Verify that clients receive `192.168.10.1` as their DNS server and that the router can resolve external domain names.

**NAT counters remain at zero**

Check the WAN interface list:

```routeros
/interface/list/member/print
```

Verify that `ether1` belongs to the WAN list and that the outgoing traffic matches the NAT rule.

**Internet access works on the router but not on LAN clients**

Inspect the DHCP network settings, Source NAT configuration, firewall forward chain, and client default gateway.

# 🗁 Complete Configuration Script

The following script combines the main configuration steps into one sequence.

Use it on a clean lab router with `ether1` as WAN and `ether2` as LAN. Do not run it blindly on an existing production configuration.

```routeros
# Create WAN and LAN interface lists

/interface/list
add name=WAN
add name=LAN

# Add interfaces to lists

/interface/list/member
add list=WAN interface=ether1
add list=LAN interface=ether2

# Configure WAN DHCP client

/ip/dhcp-client
add interface=ether1 add-default-route=yes use-peer-dns=no disabled=no comment="WAN DHCP Client"

# Configure LAN IP address

/ip/address
add address=192.168.10.1/24 interface=ether2 comment="LAN Gateway"

# Configure DNS

/ip/dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes

# Configure DHCP pool

/ip/pool
add name=LAN-POOL ranges=192.168.10.100-192.168.10.200

# Configure DHCP server

/ip/dhcp-server
add name=LAN-DHCP interface=ether2 address-pool=LAN-POOL lease-time=1d disabled=no

# Configure DHCP network

/ip/dhcp-server/network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1

# Configure Source NAT

/ip/firewall/nat
add chain=srcnat out-interface-list=WAN action=masquerade comment="LAN Internet Access"

# Configure firewall filter rules

/ip/firewall/filter
add chain=input connection-state=established,related action=accept comment="Allow Established and Related Input"
add chain=input connection-state=invalid action=drop comment="Drop Invalid Input"
add chain=input protocol=icmp action=accept comment="Allow ICMP"
add chain=input in-interface-list=LAN action=accept comment="Allow LAN Management"
add chain=input in-interface-list=WAN action=drop comment="Drop WAN Input"

add chain=forward connection-state=established,related action=accept comment="Allow Established and Related Forward"
add chain=forward connection-state=invalid action=drop comment="Drop Invalid Forward"
add chain=forward in-interface-list=LAN out-interface-list=WAN action=accept comment="Allow LAN to WAN"
add chain=forward action=drop comment="Drop Other Forwarded Traffic"
```

The script assumes a clean firewall configuration and an upstream DHCP service. A real deployment may require additional rules for VPNs, inter-VLAN routing, management access, and other services.

# 🗁 Lab Exercises

Complete the following exercises to strengthen your understanding.

**Basic Configuration**

* Configure WAN using DHCP.
* Configure a static WAN address and gateway in a separate lab scenario.
* Configure a LAN IP address.
* Create a DHCP pool and DHCP server.
* Configure DNS resolution.

**Internet Connectivity**

* Configure a default route manually.
* Configure Source NAT using Masquerade.
* Replace Masquerade with static `src-nat` in a static public IP scenario.
* Verify connectivity from multiple LAN clients.

**Firewall**

* Allow established and related connections.
* Drop invalid connections.
* Allow LAN-to-WAN forwarding.
* Block unsolicited WAN access to the router.
* Test how firewall rule order affects connectivity.

**Troubleshooting**

* Disable the default route and observe the result.
* Disable the Masquerade rule and test internet access.
* Configure an incorrect DNS server and test domain resolution.
* Disable the DHCP server and observe client behavior.
* Inspect NAT and firewall counters during connectivity tests.

# 🗁 Expected Results

At the end of this lab:

* The WAN interface should obtain valid upstream network settings.
* The router should have a working default route.
* The LAN interface should use `192.168.10.1/24`.
* LAN clients should receive IP addresses through DHCP.
* LAN clients should use the MikroTik router as their default gateway and DNS server.
* DNS resolution should work.
* Source NAT should translate outgoing LAN traffic.
* Firewall rules should allow authorized LAN-to-WAN traffic.
* LAN clients should be able to access external internet services.

# 🗁 Summary

A working internet access configuration requires several RouterOS components to work together.

The WAN interface provides upstream connectivity, the LAN interface provides internal network access, DHCP assigns client settings, DNS resolves domain names, routing selects the outgoing path, Source NAT translates private source addresses, and firewall rules control traffic.

Understanding how these components interact provides a foundation for more advanced MikroTik configurations, including VLANs, multiple WAN connections, failover, load balancing, and ISP network design.
