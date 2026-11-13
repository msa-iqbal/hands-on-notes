# Source NAT and Masquerade

Source Network Address Translation (Source NAT or SNAT) modifies the source IP address of packets as they pass through a router.

In MikroTik RouterOS, Source NAT is commonly used to allow devices on private LAN networks to access the internet through a public WAN address.

Masquerade is a special type of Source NAT designed primarily for interfaces whose public IP address may change dynamically.

# 🗁 What Is Source NAT?

Source NAT changes the source IP address of outgoing packets.

For example, consider a private LAN device accessing an internet server:

**Before Source NAT:**

```text
Source IP:      192.168.10.10
Destination IP: 8.8.8.8
```

The private source address is not normally routable across the public internet.

**After Source NAT:**

```text
Source IP:      203.0.113.10
Destination IP: 8.8.8.8
```

The router replaces the private source address with its public WAN address.

When the response returns, connection tracking allows RouterOS to reverse the translation and forward the response to the original LAN device.

# 🗁 How Source NAT Works

The following example illustrates a typical internet access scenario:

```text
LAN Device
192.168.10.10
      |
      |
      v
MikroTik Router
LAN: 192.168.10.1
WAN: 203.0.113.10
      |
      | Source NAT
      | 192.168.10.10 -> 203.0.113.10
      |
      v
Internet Server
8.8.8.8
```

The packet flow is:

* A LAN device sends a packet to an internet destination.
* The packet reaches the MikroTik router.
* The router checks the routing table and selects the outgoing WAN interface.
* The Source NAT rule changes the packet's source address.
* The translated packet is forwarded to the internet.
* The response returns to the router.
* Connection tracking reverses the translation.
* The response is forwarded to the original LAN device.

# 🗁 Source NAT vs Masquerade

RouterOS provides two commonly used actions for Source NAT:

| Feature | `src-nat` | `masquerade` |
| --- | --- | --- |
| Purpose | Replaces the source IP with a specified address | Uses the outgoing interface's current IP address |
| IP address | Manually configured | Automatically selected |
| Dynamic WAN | Requires additional management | Designed for dynamic addresses |
| Static WAN | Suitable | Also works |
| Common use | Static public IP addresses | Dynamic public IP addresses |

**Use `src-nat` when the public IP address is static.**

**Use `masquerade` when the WAN address is dynamic or may change.**

# 🗁 Configuring Masquerade

Assume the following network configuration:

| Component | Configuration |
| --- | --- |
| LAN interface | `ether2` |
| WAN interface | `ether1` |
| LAN network | `192.168.10.0/24` |
| Router LAN IP | `192.168.10.1` |
| WAN address | Dynamic |

Create a WAN interface list if it does not already exist:

```routeros
/interface/list
add name=WAN
```

Add the WAN interface to the list:

```routeros
/interface/list/member
add list=WAN interface=ether1
```

Configure the Masquerade rule:

```routeros
/ip/firewall/nat
add chain=srcnat out-interface-list=WAN action=masquerade comment="LAN Internet Access"
```

This rule performs the following operations:

* `chain=srcnat`: Processes packets in the source NAT chain.
* `out-interface-list=WAN`: Matches packets leaving through an interface belonging to the WAN list.
* `action=masquerade`: Replaces the source address with the current IP address of the outgoing interface.
* `comment`: Adds a descriptive label to the rule.

This is a common configuration for providing internet access to private LAN networks.

# 🗁 Masquerade for a Specific LAN Network

You can restrict the rule to a particular source network.

```routeros
/ip/firewall/nat
add chain=srcnat src-address=192.168.10.0/24 out-interface-list=WAN action=masquerade comment="Masquerade LAN Network"
```

This rule translates traffic originating from `192.168.10.0/24` when it leaves through a WAN interface.

This is useful when a router has multiple internal networks and you want to control which networks receive NAT.

# 🗁 Configuring Static Source NAT

When the router has a static public IP address, you can use the `src-nat` action.

Assume:

```text
LAN Network: 192.168.10.0/24
WAN Interface: ether1
Public IP: 203.0.113.10
```

Add the following rule:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=192.168.10.0/24 out-interface=ether1 action=src-nat to-addresses=203.0.113.10 comment="Static Source NAT"
```

The `to-addresses` parameter specifies the source address that RouterOS uses for translated packets.

For a single public IP address, RouterOS translates matching outgoing connections to that address.

# 🗁 Source NAT with Multiple Public IP Addresses

If an organization has multiple public IP addresses, RouterOS can use an address range for Source NAT.

Example public address range:

```text
203.0.113.10 - 203.0.113.20
```

Configuration:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=192.168.10.0/24 out-interface=ether1 action=src-nat to-addresses=203.0.113.10-203.0.113.20 comment="Public IP Pool for Source NAT"
```

RouterOS can select translated addresses from the configured range.

For more advanced public IP allocation, you can create separate NAT rules for different internal networks or customer groups.

# 🗁 NAT Rule Order

RouterOS evaluates NAT rules in order when processing the first packet of a connection.

The first matching NAT rule determines the translation for that connection.

Example:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=192.168.10.0/24 out-interface=ether1 action=src-nat to-addresses=203.0.113.10 comment="Static NAT"
add chain=srcnat out-interface-list=WAN action=masquerade comment="General Masquerade"
```

The specific rule is placed before the general Masquerade rule.

If the general rule matches first, the more specific rule may never be used for that connection.

**Important:** NAT decisions are tracked per connection. Changing or adding NAT rules does not automatically retranslate existing tracked connections. When testing a changed rule, use a new connection or remove the relevant connection-tracking entry.

# 🗁 Viewing NAT Rules

Display all configured NAT rules:

```routeros
/ip/firewall/nat/print
```

Display rules with additional details:

```routeros
/ip/firewall/nat/print detail
```

Display NAT rules with their counters:

```routeros
/ip/firewall/nat/print stats
```

The packet and byte counters help identify whether a NAT rule is matching traffic.

# 🗁 Monitoring Connection Tracking

RouterOS uses connection tracking to maintain information about active connections and their NAT translations.

Display tracked connections:

```routeros
/ip/firewall/connection/print
```

Filter connections by source address:

```routeros
/ip/firewall/connection/print where src-address~"192.168.10."
```

Connection tracking is important because NAT translations are associated with connection state.

# 🗁 Removing a NAT Rule

First, display the NAT rules:

```routeros
/ip/firewall/nat/print
```

Remove a rule using its current item number:

```routeros
/ip/firewall/nat/remove 0
```

The item number depends on the current rule list. Verify the correct entry before removing it.

# 🗁 Troubleshooting Source NAT

If LAN devices cannot access the internet, check the following areas.

**Verify the WAN interface**

```routeros
/interface/print
```

Make sure the WAN interface is running and has the expected IP configuration.

**Verify the default route**

```routeros
/ip/route/print
```

A valid default route should point toward the internet gateway.

**Verify the NAT configuration**

```routeros
/ip/firewall/nat/print detail
```

Check that the rule uses the correct source network and outgoing interface or interface list.

**Check NAT rule counters**

```routeros
/ip/firewall/nat/print stats
```

If the packet counter does not increase, the rule may not be matching the outgoing traffic.

**Check firewall filter rules**

```routeros
/ip/firewall/filter/print
```

A firewall filter rule may be blocking forwarded traffic even when the NAT configuration is correct.

**Check DNS resolution**

```routeros
/ping 8.8.8.8
:resolve example.com
```

If IP connectivity works but domain names do not resolve, investigate DNS configuration separately.

# 🗁 Important Considerations

* Source NAT is commonly used to provide internet access to private IPv4 networks.
* Masquerade automatically uses the current IP address of the outgoing interface.
* Static Source NAT is generally preferable when the public IP address is fixed.
* NAT does not replace firewall filtering or network security policies.
* NAT rules are evaluated for new connections, and connection tracking maintains established translations.
* A router can have multiple Source NAT rules for different networks, WAN links, or public IP addresses.
* Avoid applying Source NAT indiscriminately to traffic that should remain routed between internal networks.
* RouterOS NAT configuration applies to IPv4. IPv6 networks generally use routed addressing rather than traditional IPv4-style NAT.

# 🗁 Practical Example

The following configuration provides internet access to a LAN network using Masquerade.

**Network topology:**

```text
                  Internet
                     |
                     |
                  ether1
                     |
              MikroTik Router
                     |
                  ether2
                     |
              LAN: 192.168.10.0/24
                     |
              +------+------+
              |             |
           PC-01          PC-02
       192.168.10.10  192.168.10.20
```

**Configure the LAN IP address:**

```routeros
/ip/address
add address=192.168.10.1/24 interface=ether2 comment="LAN Gateway"
```

**Create the WAN interface list:**

```routeros
/interface/list
add name=WAN
```

**Add the WAN interface:**

```routeros
/interface/list/member
add list=WAN interface=ether1
```

**Configure the default route:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1
```

Replace `192.0.2.1` with the actual upstream gateway address.

**Configure Masquerade:**

```routeros
/ip/firewall/nat
add chain=srcnat out-interface-list=WAN action=masquerade comment="LAN Internet Access"
```

**Verify the configuration:**

```routeros
/ip/address/print
/ip/route/print
/interface/list/member/print
/ip/firewall/nat/print stats
```

The LAN devices should now be able to access the internet, provided that the upstream connection, routing, DNS, and firewall forwarding policies are correctly configured.

# 🗁 Summary

Source NAT changes the source IP address of outgoing packets. It is commonly used to allow private network devices to communicate with internet services through a public IP address.

RouterOS provides two primary Source NAT actions:

* `src-nat`: Translates traffic to a manually specified IP address.
* `masquerade`: Translates traffic using the current IP address of the outgoing interface.

Understanding Source NAT and Masquerade is essential for configuring internet access, multi-WAN environments, and ISP networks.
