# Access Control Lists (ACLs)

An Access Control List (ACL) is an ordered collection of rules used to control network traffic based on defined matching conditions.

ACLs determine whether traffic is permitted or denied according to attributes such as source IP address, destination IP address, protocol, and transport-layer ports.

They are commonly implemented on routers, switches, firewalls, and network security devices.

ACLs are essential for network access control, traffic filtering, network segmentation, and restricting communication between different network zones.

# 🗁 Overview

ACLs provide a way to define which network traffic is allowed or denied.

For example, consider an organization with three networks:

| Network | Subnet | Purpose |
| --- | --- | --- |
| Management | `192.168.10.0/24` | Network administration |
| Employees | `192.168.20.0/24` | Employee workstations |
| Guest | `192.168.30.0/24` | Guest devices |
| Servers | `192.168.40.0/24` | Internal application servers |

The organization may require the following policies:

* Management devices can access all internal networks.
* Employees can access application servers.
* Guest devices can access the internet but not internal networks.
* Only authorized management devices can access network equipment.

ACLs can implement these restrictions by matching packet attributes and applying permit or deny decisions.

# 🗁 How ACLs Work

An ACL evaluates network traffic against an ordered list of rules.

Each rule defines matching conditions and an action.

A simplified process:

```text
Incoming Packet
      |
      v
ACL Evaluation
      |
      v
Check First Rule
      |
      v
Does Packet Match?
      |
   +--+--+
   |     |
  Yes    No
   |     |
   v     v
Action  Check Next Rule
   |
   v
Permit or Deny
```

For example:

```text
Permit 192.168.10.0/24 to 192.168.40.0/24
Deny   192.168.30.0/24 to internal networks
Permit other traffic
```

The actual behavior depends on the ACL implementation, rule order, attachment point, and default policy.

# 🗁 ACL Components

An ACL rule can contain several matching fields.

| Component | Description |
| --- | --- |
| Source address | Identifies where traffic originates |
| Destination address | Identifies where traffic is going |
| Protocol | Matches TCP, UDP, ICMP, IP, or another protocol |
| Source port | Matches the originating transport-layer port |
| Destination port | Matches the destination service port |
| Interface | Restricts matching to a specific network interface |
| Direction | Determines whether traffic is evaluated inbound or outbound |
| Action | Permits, denies, or otherwise handles matching traffic |

Example policy:

```text
Source:      192.168.20.0/24
Destination: 192.168.40.10
Protocol:    TCP
Port:        443
Action:      Permit
```

This rule permits HTTPS traffic from the employee network to the specified server.

# 🗁 Types of ACLs

ACL implementations provide different matching capabilities.

Common Cisco IOS ACL categories include:

| ACL type | Matching capability |
| --- | --- |
| Standard ACL | Primarily matches source IPv4 addresses |
| Extended ACL | Matches source, destination, protocol, and transport ports |
| Named ACL | Uses a descriptive name instead of relying only on a numeric identifier |

Other network platforms may provide additional ACL types, such as:

* IPv6 ACLs.
* MAC ACLs.
* Time-based ACLs.
* Dynamic ACLs.
* Reflexive ACLs.
* Control-plane ACLs.
* VLAN ACLs.

The available features depend on the device and operating system.

# 🗁 Standard ACLs

A standard ACL primarily filters traffic based on its source IPv4 address.

It does not normally distinguish between destination addresses or TCP and UDP destination ports.

Example Cisco IOS configuration:

```cisco
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any
```

This ACL permits packets from the `192.168.10.0/24` source network and denies other sources.

Apply the ACL to an interface:

```cisco
interface GigabitEthernet0/0
 ip access-group 10 in
```

Inspect the ACL:

```cisco
show access-lists
```

Standard ACLs are useful when source-based filtering is sufficient.

Because they have limited matching capabilities, they are not appropriate when a policy must distinguish between destinations or application services.

# 🗁 Extended ACLs

Extended ACLs provide more granular traffic filtering.

They can match:

* Source IPv4 address.
* Destination IPv4 address.
* IP protocol.
* TCP and UDP ports.
* ICMP traffic and message types.

Example:

```cisco
access-list 110 permit tcp 192.168.20.0 0.0.0.255 192.168.40.10 0.0.0.0 eq 443
```

This permits TCP connections from the employee network to `192.168.40.10` on destination port `443`.

Another example:

```cisco
access-list 110 permit udp 192.168.20.0 0.0.0.255 any eq 53
```

This permits UDP DNS traffic from the employee network.

Deny traffic to a protected subnet:

```cisco
access-list 110 deny ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
```

Permit other traffic:

```cisco
access-list 110 permit ip any any
```

Apply the ACL:

```cisco
interface GigabitEthernet0/1
 ip access-group 110 in
```

Extended ACLs are useful when access decisions depend on both source and destination information.

# 🗁 Named and Numbered ACLs

Cisco IOS supports numbered and named ACLs.

**Numbered ACL**

A numbered ACL is identified by a numeric value.

Example:

```cisco
access-list 10 permit 192.168.10.0 0.0.0.255
```

**Named ACL**

A named ACL uses a descriptive identifier.

Example:

```cisco
ip access-list extended EMPLOYEE-ACCESS
 permit tcp 192.168.20.0 0.0.0.255 host 192.168.40.10 eq 443
 permit udp 192.168.20.0 0.0.0.255 any eq 53
 deny ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
 permit ip 192.168.20.0 0.0.0.255 any
```

Apply the named ACL:

```cisco
interface GigabitEthernet0/1
 ip access-group EMPLOYEE-ACCESS in
```

Named ACLs are generally easier to understand and maintain in larger configurations.

# 🗁 Inbound and Outbound ACLs

ACLs can be applied in different directions on a network interface.

**Inbound ACL**

An inbound ACL evaluates packets as they enter an interface.

```text
Incoming Traffic
       |
       v
Network Interface
       |
       v
Inbound ACL
       |
       v
Routing Decision
```

Example:

```cisco
interface GigabitEthernet0/0
 ip access-group LAN-IN in
```

**Outbound ACL**

An outbound ACL evaluates packets as they leave an interface.

```text
Routing Decision
       |
       v
Outbound ACL
       |
       v
Network Interface
       |
       v
Outgoing Traffic
```

Example:

```cisco
interface GigabitEthernet0/1
 ip access-group LAN-OUT out
```

The same ACL can produce different results depending on where and in which direction it is applied.

For example, an inbound ACL on a LAN interface evaluates traffic entering the router from that LAN.

An outbound ACL on a WAN interface evaluates traffic leaving the router through the WAN interface.

# 🗁 ACL Processing and Rule Order

ACL entries are evaluated in sequence.

The first matching entry determines the result for traditional Cisco IOS ACL processing.

Consider:

```cisco
ip access-list extended WEB-ACCESS
 deny tcp any host 192.168.40.10 eq 80
 permit tcp any host 192.168.40.10 eq 443
 permit ip any any
```

The policy:

* Denies HTTP traffic to the server.
* Permits HTTPS traffic to the server.
* Permits other IP traffic.

Rule order is important.

For example:

```cisco
ip access-list extended WEB-ACCESS
 permit ip any any
 deny tcp any host 192.168.40.10 eq 80
```

The deny entry is ineffective because the earlier permit rule already matches all IP traffic.

Always place specific restrictions before broader permit rules when using first-match ACL implementations.

# 🗁 Implicit Deny

Many ACL implementations use an implicit deny at the end of the rule list.

In Cisco IOS, if a packet does not match any explicit ACL entry, it is denied.

Example:

```cisco
access-list 10 permit 192.168.10.0 0.0.0.255
```

This permits the specified source network.

All other sources are implicitly denied.

Conceptually:

```text
permit 192.168.10.0/24
deny any
```

The final deny does not need to be explicitly configured in a traditional Cisco ACL.

An explicit deny can still be useful for logging or making the policy easier to understand.

Be careful when applying restrictive ACLs to interfaces. An implicit deny can unexpectedly block DNS, DHCP, routing protocols, management access, or other required traffic.

# 🗁 Wildcard Masks

Cisco IPv4 ACLs commonly use wildcard masks to identify address ranges.

A wildcard mask indicates which bits of an IPv4 address must match.

A wildcard mask works opposite to a subnet mask:

* `0` means the corresponding bit must match.
* `1` means the corresponding bit can differ.

For a `/24` network:

| Item | Value |
| --- | --- |
| Network address | `192.168.10.0` |
| Subnet mask | `255.255.255.0` |
| Wildcard mask | `0.0.0.255` |

Example:

```cisco
access-list 10 permit 192.168.10.0 0.0.0.255
```

Common wildcard masks:

| Prefix | Subnet mask | Wildcard mask |
| --- | --- | --- |
| `/32` | `255.255.255.255` | `0.0.0.0` |
| `/24` | `255.255.255.0` | `0.0.0.255` |
| `/16` | `255.255.0.0` | `0.0.255.255` |
| `/8` | `255.0.0.0` | `0.255.255.255` |
| `/30` | `255.255.255.252` | `0.0.0.3` |

Cisco also provides the `host` keyword for a single IPv4 address:

```cisco
access-list 110 permit ip host 192.168.10.10 any
```

The `any` keyword matches any IPv4 source or destination address.

# 🗁 ACLs in Cisco IOS

Cisco IOS ACLs are commonly applied to router and Layer 3 switch interfaces.

**Create an extended ACL**

```cisco
ip access-list extended OFFICE-IN
 permit tcp 192.168.20.0 0.0.0.255 host 192.168.40.10 eq 443
 permit udp 192.168.20.0 0.0.0.255 any eq 53
 deny ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
 permit ip 192.168.20.0 0.0.0.255 any
```

**Apply the ACL inbound**

```cisco
interface GigabitEthernet0/1
 ip access-group OFFICE-IN in
```

**Inspect ACLs**

```cisco
show access-lists
```

**Inspect interface ACL assignments**

```cisco
show ip interface GigabitEthernet0/1
```

**Remove an ACL from an interface**

```cisco
interface GigabitEthernet0/1
 no ip access-group OFFICE-IN in
```

**Delete the ACL**

```cisco
no ip access-list extended OFFICE-IN
```

Do not remove an ACL that is still required by an active security policy.

# 🗁 ACLs in Linux nftables

Linux nftables does not use Cisco-style ACL objects as its primary configuration model.

Instead, it provides ordered rules inside chains. These rules can implement ACL-like policies.

For example, create an IPv4 and IPv6 filtering table:

```bash
sudo nft add table inet acl
```

Create an input chain:

```bash
sudo nft add chain inet acl input \
    '{ type filter hook input priority filter; policy drop; }'
```

Allow loopback traffic:

```bash
sudo nft add rule inet acl input iifname "lo" accept
```

Allow established and related connections:

```bash
sudo nft add rule inet acl input ct state established,related accept
```

Allow SSH only from the management subnet:

```bash
sudo nft add rule inet acl input \
    ip saddr 192.168.10.0/24 tcp dport 22 accept
```

Allow HTTPS:

```bash
sudo nft add rule inet acl input tcp dport 443 accept
```

Inspect the configuration:

```bash
sudo nft list table inet acl
```

This is an ACL-like policy implemented through nftables rules.

The actual packet-processing model remains based on Netfilter hooks, chains, and verdicts.

# 🗁 ACLs in iptables

iptables implements packet filtering through ordered rules in chains.

Example: permit SSH from a management network.

```bash
sudo iptables -A INPUT \
    -s 192.168.10.0/24 \
    -p tcp \
    --dport 22 \
    -j ACCEPT
```

Deny SSH from a specific source:

```bash
sudo iptables -A INPUT \
    -s 198.51.100.25 \
    -p tcp \
    --dport 22 \
    -j DROP
```

Permit HTTPS:

```bash
sudo iptables -A INPUT \
    -p tcp \
    --dport 443 \
    -j ACCEPT
```

Inspect rules:

```bash
sudo iptables -L -n -v
```

iptables rules are evaluated sequentially. A matching terminal action such as `ACCEPT` or `DROP` determines the result within the current chain.

Unlike Cisco ACLs, iptables also provides connection-tracking integration, NAT tables, user-defined chains, and other packet-processing capabilities.

# 🗁 ACLs in MikroTik RouterOS

MikroTik RouterOS provides access-control functionality through its firewall filter rules.

Although RouterOS does not primarily use Cisco-style ACL objects, its ordered firewall rules can implement equivalent access policies.

Common matching conditions include:

* Source address.
* Destination address.
* Protocol.
* Source port.
* Destination port.
* Input interface.
* Output interface.
* Connection state.
* Address lists.
* Interface lists.

**Allow SSH from a management subnet**

```routeros
/ip firewall filter add \
    chain=input \
    src-address=192.168.10.0/24 \
    protocol=tcp \
    dst-port=22 \
    action=accept \
    comment="Allow SSH from management network"
```

**Block access from a specific source**

```routeros
/ip firewall filter add \
    chain=input \
    src-address=198.51.100.25 \
    action=drop \
    comment="Block unwanted source"
```

**Allow HTTPS access to the router**

```routeros
/ip firewall filter add \
    chain=input \
    protocol=tcp \
    dst-port=443 \
    action=accept \
    comment="Allow HTTPS management"
```

**Use an address list**

Create a management address list:

```routeros
/ip firewall address-list add \
    list=management-networks \
    address=192.168.10.0/24 \
    comment="Management subnet"
```

Reference the address list in a firewall rule:

```routeros
/ip firewall filter add \
    chain=input \
    src-address-list=management-networks \
    protocol=tcp \
    dst-port=22 \
    action=accept \
    comment="Allow management SSH"
```

Inspect firewall rules:

```routeros
/ip firewall filter print
```

Inspect rule counters:

```routeros
/ip firewall filter print stats
```

Inspect address lists:

```routeros
/ip firewall address-list print
```

RouterOS firewall rules are evaluated from top to bottom. Place specific restrictions and required permits in the correct order relative to broader rules.

# 🗁 ACLs and Network Segmentation

ACLs are useful for enforcing communication policies between network segments.

Consider this network:

```text
                 Router / Firewall
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
      VLAN 10        VLAN 20       VLAN 30
     Management      Employees       Guest
   192.168.10.0/24 192.168.20.0/24 192.168.30.0/24
          |
          |
          v
       VLAN 40
        Servers
     192.168.40.0/24
```

A sample security policy:

| Source network | Destination | Policy |
| --- | --- | --- |
| Management | All internal networks | Permit |
| Employees | Application server HTTPS | Permit |
| Employees | Management network | Deny |
| Guest | Internal networks | Deny |
| Guest | Internet | Permit |
| Servers | Required infrastructure services | Permit |

ACLs can enforce these policies at Layer 3 boundaries.

For example, an extended Cisco ACL can restrict employee access:

```cisco
ip access-list extended EMPLOYEE-SEGMENT
 permit tcp 192.168.20.0 0.0.0.255 host 192.168.40.10 eq 443
 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
 permit ip 192.168.20.0 0.0.0.255 any
```

Apply it to the appropriate Layer 3 interface or VLAN interface.

The policy must account for required services such as DNS, DHCP, authentication, monitoring, and other infrastructure dependencies.

# 🗁 ACLs vs Firewall Rules

ACLs and firewall rules have overlapping purposes, but they are not identical concepts.

| Feature | Traditional ACL | Stateful Firewall |
| --- | --- | --- |
| Basic filtering | Yes | Yes |
| Source address matching | Yes | Yes |
| Destination address matching | Depends on ACL type | Yes |
| Protocol matching | Depends on ACL type | Yes |
| Port matching | Extended ACLs | Yes |
| Rule ordering | Commonly first-match | Depends on implementation |
| Connection tracking | Not inherent to traditional ACLs | Commonly supported |
| NAT integration | Platform-dependent | Commonly supported |
| Interface direction | Common | Implementation-dependent |
| Application-layer inspection | Generally limited | May be supported |

A traditional stateless ACL evaluates packets using configured matching conditions without inherently maintaining connection state.

A stateful firewall can use connection tracking to recognize established and related traffic.

For example, a stateful firewall can allow response packets belonging to an already permitted connection without requiring a separate reverse-direction rule for every connection.

# 🗁 Best Practices

* Use descriptive names for named ACLs and address lists.
* Apply the principle of least privilege.
* Permit only required protocols and services.
* Place specific rules before broad matching rules.
* Understand implicit deny behavior.
* Apply ACLs at appropriate network boundaries.
* Restrict administrative access to trusted management networks.
* Document the purpose of each rule.
* Remove unused and obsolete entries.
* Review ACLs whenever network architecture changes.
* Include required infrastructure traffic in the policy.
* Monitor ACL counters and firewall logs.
* Test policy changes before deploying them to production.
* Keep IPv4 and IPv6 access-control policies consistent where appropriate.
* Use stateful filtering when connection-state awareness is required.

# 🗁 Common Mistakes

| Mistake | Result |
| --- | --- |
| Incorrect rule order | Traffic may be permitted or denied unexpectedly |
| Forgetting implicit deny | Legitimate traffic may be blocked |
| Incorrect wildcard mask | ACL matches the wrong address range |
| Wrong interface direction | Traffic is evaluated at an unintended point |
| Using standard ACLs for port filtering | Required matching information is unavailable |
| Overly broad permit rule | Unintended access is allowed |
| Missing DNS permission | Clients may be unable to resolve hostnames |
| Missing DHCP permission | Clients may fail to obtain addresses |
| Blocking required ICMP traffic | Troubleshooting and network functions may fail |
| Applying ACL to the wrong interface | The intended traffic is not filtered |
| Treating stateless ACLs as stateful firewalls | Return traffic may not behave as expected |
| Forgetting IPv6 policies | IPv6 traffic may bypass an IPv4-only security policy |

# 🗁 Troubleshooting

When an ACL blocks expected traffic, inspect the configuration and follow the packet path.

**Cisco IOS**

Display ACLs:

```cisco
show access-lists
```

Inspect interface configuration:

```cisco
show running-config interface GigabitEthernet0/0
```

Inspect interface IP and ACL information:

```cisco
show ip interface GigabitEthernet0/0
```

Check interface status:

```cisco
show ip interface brief
```

Inspect routing:

```cisco
show ip route
```

**Linux nftables**

Display the ruleset:

```bash
sudo nft list ruleset
```

Display rules with handles and counters:

```bash
sudo nft -a list ruleset
```

Capture packets:

```bash
sudo tcpdump -ni any
```

Check a specific TCP service:

```bash
nc -vz 192.168.40.10 443
```

**MikroTik RouterOS**

Inspect firewall rules:

```routeros
/ip firewall filter print
```

Display counters:

```routeros
/ip firewall filter print stats
```

Inspect address lists:

```routeros
/ip firewall address-list print
```

Inspect interface configuration:

```routeros
/interface print
```

**Troubleshooting checklist**

* Confirm the source and destination IP addresses.
* Verify the protocol and destination port.
* Identify the interface where the packet enters.
* Determine whether the ACL is applied inbound or outbound.
* Inspect rule order.
* Check wildcard masks and subnet definitions.
* Review implicit deny behavior.
* Inspect rule counters and logs.
* Verify the routing path.
* Confirm that required DNS, DHCP, ICMP, and infrastructure traffic is permitted.
* Test the policy from the actual source network.

# 🗁 Practical Example

Build an ACL policy for a small organization.

**Network topology**

```text
                 Router
                   |
        +----------+----------+
        |          |          |
        v          v          v
     VLAN 10    VLAN 20    VLAN 30
   Management  Employees    Guest
        |          |          |
        +----------+----------+
                   |
                   v
                VLAN 40
                 Servers
```

**Network addressing**

| Network | Subnet |
| --- | --- |
| Management | `192.168.10.0/24` |
| Employees | `192.168.20.0/24` |
| Guest | `192.168.30.0/24` |
| Servers | `192.168.40.0/24` |

Application server:

```text
192.168.40.10
```

**Security requirements**

* Employees can access the application server through HTTPS.
* Employees cannot access the management network.
* Guest devices cannot access internal networks.
* Management devices can access internal networks.
* Guest devices can access the internet.

**Cisco IOS example**

```cisco
ip access-list extended EMPLOYEE-ACL
 permit tcp 192.168.20.0 0.0.0.255 host 192.168.40.10 eq 443
 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 permit ip 192.168.20.0 0.0.0.255 any
```

Apply the ACL to the employee-facing Layer 3 interface:

```cisco
interface Vlan20
 ip access-group EMPLOYEE-ACL in
```

Guest network ACL:

```cisco
ip access-list extended GUEST-ACL
 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
 permit ip 192.168.30.0 0.0.0.255 any
```

Apply it inbound:

```cisco
interface Vlan30
 ip access-group GUEST-ACL in
```

These examples demonstrate the policy structure. A production configuration must also account for infrastructure services, routing behavior, IPv6, and the actual security boundaries.

**Expected behavior**

| Test | Expected result |
| --- | --- |
| Employee to application server HTTPS | Allowed |
| Employee to management network | Denied |
| Guest to employee network | Denied |
| Guest to management network | Denied |
| Guest to internal server network | Denied |
| Guest to external internet | Allowed, subject to routing and upstream firewall policy |

# 🗁 Summary

Access Control Lists are ordered collections of rules used to permit or deny network traffic.

They are commonly used on routers, switches, and firewalls to control communication between network segments and restrict access to network services.

Remember these key points:

* ACLs control traffic using matching conditions.
* Standard ACLs primarily match source IPv4 addresses.
* Extended ACLs can match source, destination, protocol, and ports.
* Named ACLs use descriptive identifiers.
* Inbound ACLs inspect traffic entering an interface.
* Outbound ACLs inspect traffic leaving an interface.
* Traditional Cisco ACLs use first-match processing.
* Cisco ACLs have an implicit deny at the end.
* Wildcard masks identify address ranges.
* Linux nftables and iptables can implement ACL-like policies through firewall rules.
* MikroTik RouterOS provides equivalent access control through firewall filter rules.
* ACLs are useful for network segmentation and least-privilege access.

# 🗁 References

* [Cisco — Configure IP Access Lists](https://www.cisco.com/c/en/us/support/docs/security/ios-firewall/23602-confaccesslists.html)
* [Cisco IOS IP Access List Commands](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/sec_data_acl/configuration/xe-17/sec-data-acl-xe-17-book.html)
* [nftables Documentation](https://netfilter.org/projects/nftables/manpage.html)
* [Netfilter Documentation](https://www.netfilter.org/documentation/)
* [iptables Manual](https://man7.org/linux/man-pages/man8/iptables.8.html)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [RFC 1918 — Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918)
