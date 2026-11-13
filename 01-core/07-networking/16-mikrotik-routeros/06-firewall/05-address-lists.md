# Firewall Address Lists

Firewall address lists allow you to group IP addresses and networks under reusable names. Instead of specifying the same IP addresses in multiple firewall rules, you can reference an address list.

Address lists make firewall configurations easier to organize, maintain, and scale.

Common use cases include:

* Allowing management access from trusted IP addresses.
* Blocking malicious or unwanted IP addresses.
* Restricting access to selected servers.
* Grouping customer networks.
* Managing temporary IP blocks.
* Implementing automated security policies.
* Creating reusable firewall rules.

RouterOS provides separate address-list menus for IPv4 and IPv6:

```routeros
/ip/firewall/address-list
```

```routeros
/ipv6/firewall/address-list
```

# 🗁 Address List Structure

An address-list entry contains several properties.

| Property | Description |
|---|---|
| `list` | Name of the address list |
| `address` | IP address, subnet, or supported address range |
| `comment` | Description of the entry |
| `timeout` | Optional lifetime for a dynamic entry |

Example:

```routeros
/ip/firewall/address-list
add list=TRUSTED address=192.168.10.0/24 comment="Trusted LAN"
add list=TRUSTED address=203.0.113.10 comment="Trusted administrator"
add list=BLOCKED address=198.51.100.25 comment="Unwanted source"
```

This creates two lists:

* `TRUSTED` contains a LAN subnet and an administrator's public IP address.
* `BLOCKED` contains an IP address intended to be blocked.

Address-list names are user-defined. Use descriptive names that clearly communicate their purpose.

# 🗁 Adding IP Addresses to an Address List

**Add a single IPv4 address**

```routeros
/ip/firewall/address-list
add list=BLOCKED address=203.0.113.50 comment="Blocked host"
```

**Add an IPv4 subnet**

```routeros
/ip/firewall/address-list
add list=TRUSTED address=192.168.10.0/24 comment="Trusted LAN subnet"
```

**Add multiple addresses**

```routeros
/ip/firewall/address-list
add list=TRUSTED address=192.168.10.10 comment="Administrator workstation"
add list=TRUSTED address=192.168.10.20 comment="Management server"
add list=TRUSTED address=192.168.10.30 comment="Monitoring server"
```

**Add an IPv6 network**

```routeros
/ipv6/firewall/address-list
add list=IPV6-TRUSTED address=2001:db8:10::/64 comment="Trusted IPv6 subnet"
```

IPv4 and IPv6 address lists are managed separately. Firewall policies for both address families should be configured and reviewed independently.

# 🗁 Using Address Lists in Firewall Rules

Address lists become useful when referenced by firewall filter rules.

Two important matching properties are:

| Property | Purpose |
|---|---|
| `src-address-list` | Matches the packet's source address against a list |
| `dst-address-list` | Matches the packet's destination address against a list |

**Block sources in an address list**

Create a list of blocked IP addresses:

```routeros
/ip/firewall/address-list
add list=BLOCKED address=203.0.113.50
add list=BLOCKED address=198.51.100.25
```

Drop traffic from those addresses:

```routeros
/ip/firewall/filter
add chain=input src-address-list=BLOCKED action=drop comment="Drop blocked input sources"
add chain=forward src-address-list=BLOCKED action=drop comment="Drop blocked forwarding sources"
```

The `input` rule protects the router itself. The `forward` rule controls traffic routed through the router.

**Allow management access from trusted addresses**

Create a trusted address list:

```routeros
/ip/firewall/address-list
add list=TRUSTED-MANAGEMENT address=192.168.10.0/24
add list=TRUSTED-MANAGEMENT address=203.0.113.10
```

Allow management access from those addresses:

```routeros
/ip/firewall/filter
add chain=input src-address-list=TRUSTED-MANAGEMENT protocol=tcp dst-port=22,8291 action=accept comment="Allow trusted SSH and WinBox"
```

Place this rule before any broader rule that drops the same management traffic.

**Restrict access to selected destinations**

Create a destination list:

```routeros
/ip/firewall/address-list
add list=APPROVED-SERVERS address=192.168.10.100
add list=APPROVED-SERVERS address=192.168.10.101
```

Match traffic going to those servers:

```routeros
/ip/firewall/filter
add chain=forward dst-address-list=APPROVED-SERVERS action=accept comment="Allow approved server destinations"
```

This rule permits matching traffic only if no earlier rule prevents it. A complete access policy must also define how other traffic is handled.

# 🗁 Static Address List Entries

Static entries are manually configured and normally remain until they are removed or changed.

Example:

```routeros
/ip/firewall/address-list
add list=TRUSTED address=192.168.10.0/24 comment="Trusted LAN"
add list=BLOCKED address=203.0.113.50 comment="Manually blocked host"
```

Static entries are suitable for:

* Trusted administrator addresses.
* Internal network subnets.
* Known unwanted IP addresses.
* Server groups.
* Permanent security policies.

# 🗁 Dynamic Address List Entries

Dynamic entries are created automatically by RouterOS or by firewall rules.

They can have a timeout, allowing RouterOS to remove them automatically after a specified period.

For example:

```routeros
/ip/firewall/address-list
add list=TEMP-BLOCK address=203.0.113.50 timeout=10m
```

Dynamic entries are useful for:

* Temporary blocking.
* Brute-force protection.
* Rate-based security policies.
* Automated source tracking.
* Temporary access permissions.

# 🗁 Adding Addresses Dynamically Through Firewall Rules

RouterOS provides the `add-src-to-address-list` action.

This action adds a packet's source IP address to a specified address list.

Example:

```routeros
/ip/firewall/filter
add chain=input protocol=tcp dst-port=8291 connection-state=new action=add-src-to-address-list address-list=WINBOX-ATTEMPTS address-list-timeout=1h comment="Track WinBox attempts"
```

This rule tracks matching new TCP connection attempts to WinBox and adds their source addresses to `WINBOX-ATTEMPTS` for one hour.

Important: `add-src-to-address-list` does not automatically drop the packet. It adds the address to a list and then firewall processing continues.

To block sources already in the list, add a separate rule before the tracking rule:

```routeros
/ip/firewall/filter
add chain=input src-address-list=WINBOX-ATTEMPTS action=drop comment="Drop tracked WinBox sources"
```

A complete policy should also define which sources are trusted and which management traffic is permitted.

# 🗁 Address List Timeouts

The `timeout` property determines how long a dynamic entry remains active.

Examples:

```routeros
/ip/firewall/address-list
add list=TEMP-BLOCK address=203.0.113.50 timeout=30s
add list=TEMP-BLOCK address=198.51.100.25 timeout=5m
add list=TEMP-BLOCK address=192.0.2.50 timeout=1h
add list=TEMP-BLOCK address=192.0.2.60 timeout=1d
```

Common timeout formats:

| Value | Duration |
|---|---|
| `30s` | 30 seconds |
| `5m` | 5 minutes |
| `1h` | 1 hour |
| `12h` | 12 hours |
| `1d` | 1 day |
| `7d` | 7 days |

When a dynamic entry expires, RouterOS automatically removes it from the active address list.

# 🗁 Building a Temporary Blocking Policy

A common use case is temporarily blocking sources that repeatedly attempt to access a management service.

Assumptions:

| Component | Configuration |
|---|---|
| WAN interface | `ether1` |
| Trusted management subnet | `192.168.10.0/24` |
| Management service | WinBox |
| WinBox TCP port | `8291` |
| Temporary block duration | 1 hour |

**Create a trusted management list**

```routeros
/ip/firewall/address-list
add list=MGMT-ALLOWED address=192.168.10.0/24 comment="Trusted management LAN"
```

**Drop sources already tracked as unauthorized**

```routeros
/ip/firewall/filter
add chain=input in-interface=ether1 src-address-list=MGMT-BLOCKED action=drop comment="Drop temporarily blocked sources"
```

**Track unauthorized new WinBox connection attempts**

```routeros
/ip/firewall/filter
add chain=input in-interface=ether1 protocol=tcp dst-port=8291 connection-state=new src-address-list=!MGMT-ALLOWED action=add-src-to-address-list address-list=MGMT-BLOCKED address-list-timeout=1h comment="Track unauthorized WinBox attempts"
```

The behavior is:

* Trusted management addresses are excluded from the tracking rule.
* A new matching WinBox connection attempt from another source adds that source to `MGMT-BLOCKED`.
* Subsequent packets from an address in `MGMT-BLOCKED` are dropped by the preceding rule.
* The dynamic entry expires after one hour.

This example demonstrates basic source tracking. It does not distinguish between legitimate and malicious users, and it should not replace a properly restricted management-access policy.

# 🗁 Managing Address Lists

**Display all entries**

```routeros
/ip/firewall/address-list/print
```

**Display detailed information**

```routeros
/ip/firewall/address-list/print detail
```

**Display entries belonging to a specific list**

```routeros
/ip/firewall/address-list/print where list="TRUSTED"
```

**Display blocked entries**

```routeros
/ip/firewall/address-list/print where list="BLOCKED"
```

**Add an address**

```routeros
/ip/firewall/address-list/add list=BLOCKED address=203.0.113.75 comment="Manual block"
```

**Remove a specific address**

```routeros
/ip/firewall/address-list/remove [find list="BLOCKED" address="203.0.113.75"]
```

**Remove all entries from a specific list**

```routeros
/ip/firewall/address-list/remove [find list="BLOCKED"]
```

The last command removes every matching entry, including dynamic entries in that list. Use it carefully.

# 🗁 Address Lists and IPv6

RouterOS provides IPv6 address lists separately from IPv4 address lists.

Example:

```routeros
/ipv6/firewall/address-list
add list=IPV6-TRUSTED address=2001:db8:10::/64 comment="Trusted IPv6 LAN"
add list=IPV6-BLOCKED address=2001:db8:20::50 comment="Blocked IPv6 host"
```

Use the corresponding IPv6 firewall rules to reference those lists:

```routeros
/ipv6/firewall/filter
add chain=input src-address-list=IPV6-BLOCKED action=drop comment="Drop blocked IPv6 sources"
```

IPv6 firewall policies should account for ICMPv6, neighbor discovery, router advertisements, and other essential IPv6 functions. Avoid copying an IPv4 policy into IPv6 without considering the protocol differences.

# 🗁 Address List Design Practices

* Use descriptive names such as `TRUSTED`, `BLOCKED`, `MGMT-ALLOWED`, and `APPROVED-SERVERS`.
* Keep separate lists for different security purposes.
* Use comments to explain manually configured entries.
* Apply timeouts to temporary entries.
* Place blocking rules before rules that would otherwise accept the same traffic.
* Scope address-list rules to the correct chain and interface.
* Avoid overly broad lists that unintentionally block legitimate networks.
* Review dynamic entries regularly.
* Be careful when removing all entries from a list.
* Maintain separate IPv4 and IPv6 policies.

# 🗁 Troubleshooting Address Lists

**An address is not being blocked**

Check whether the address exists in the intended list:

```routeros
/ip/firewall/address-list/print where list="BLOCKED"
```

Check whether the relevant filter rule references that list:

```routeros
/ip/firewall/filter/print
```

Also verify the chain, rule order, and source address actually observed by the router.

**A dynamic entry disappears**

Check whether the entry has a timeout. Dynamic entries are automatically removed when their lifetime expires.

**A trusted address is still blocked**

Verify the source address seen by the router. NAT, VPNs, and upstream gateways can change the source address visible to the firewall.

**An address-list rule does not match**

Check:

* The spelling of the list name.
* The source or destination address.
* The firewall chain.
* The rule's position.
* Whether the traffic uses IPv4 or IPv6.

**Too many dynamic entries are created**

Review the rule that creates the entries. Ensure that it matches only the intended traffic and uses a reasonable timeout.

# 🗁 Practical Lab

**Objective:** Create trusted and blocked address lists and use them in firewall rules.

Network assumptions:

| Component | Configuration |
|---|---|
| WAN interface | `ether1` |
| LAN interface | `bridge-LAN` |
| LAN subnet | `192.168.10.0/24` |
| Trusted administrator | `203.0.113.10` |
| Test blocked address | `198.51.100.50` |

**Create the address lists**

```routeros
/ip/firewall/address-list
add list=TRUSTED-MANAGEMENT address=192.168.10.0/24 comment="Trusted LAN"
add list=TRUSTED-MANAGEMENT address=203.0.113.10 comment="Remote administrator"
add list=BLOCKED address=198.51.100.50 comment="Test blocked address"
```

**Allow management from trusted addresses**

```routeros
/ip/firewall/filter
add chain=input src-address-list=TRUSTED-MANAGEMENT protocol=tcp dst-port=22,8291 action=accept comment="Allow trusted management"
```

**Block the test address**

```routeros
/ip/firewall/filter
add chain=input src-address-list=BLOCKED action=drop comment="Block unwanted input sources"
```

Place the rules appropriately within the existing input policy so that the intended blocking and access controls take effect.

**Verify the configuration**

```routeros
/ip/firewall/address-list/print
/ip/firewall/filter/print
/ip/firewall/filter/print stats
```

Test the policy from the appropriate source networks and verify that the intended rules receive matching packets.

# 🗁 Knowledge Check

* What is a firewall address list?
* What is the difference between `src-address-list` and `dst-address-list`?
* How do static and dynamic address-list entries differ?
* Why are timeouts useful for dynamic entries?
* How can a firewall rule add an address to a list?
* Does `add-src-to-address-list` automatically drop a packet?
* Why should blocking rules appear before broad allow rules?
* How can address lists simplify a large firewall configuration?
* Why should IPv4 and IPv6 address lists be managed separately?

# 🗁 References

* MikroTik RouterOS Documentation — Firewall: <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall>
* MikroTik RouterOS Documentation — Packet Flow: <https://help.mikrotik.com/docs/spaces/ROS/pages/328227/Packet+Flow>
