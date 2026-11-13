# Default Routing

Default routing is a routing mechanism that allows a router to forward packets when no more-specific matching route exists in its routing table.

A default route acts as a fallback path for destinations that are not explicitly listed in the routing table.

It is commonly used in:

* Home networks.
* Small office networks.
* Enterprise branch networks.
* ISP customer networks.
* Internet gateway routers.
* Multi-WAN environments.

Instead of configuring individual routes for every possible destination, a router can use one default route to forward unknown destinations toward an upstream router.

Default routing is an essential part of internet connectivity because most networks use a default gateway to reach destinations outside their local routing domain.

# 🗁 What Is Default Routing?

Default routing is the process of forwarding packets to a predefined next hop when no more-specific route matches the destination address.

A default route is a special route with a prefix length of zero.

IPv4 default route:

```text
0.0.0.0/0
```

IPv6 default route:

```text
::/0
```

A `/0` prefix contains no fixed network bits, so it matches every destination address in its address family.

However, a router uses the default route only when no more-specific matching route is selected.

### 🖆 Example

Suppose a router has the following routing table:

| Destination | Next Hop |
| --- | --- |
| `192.168.1.0/24` | Directly connected |
| `10.0.0.0/8` | `192.168.1.2` |
| `0.0.0.0/0` | `192.168.1.1` |

If the router receives packets destined for:

| Destination IP | Selected Route |
| --- | --- |
| `192.168.1.10` | `192.168.1.0/24` |
| `10.10.10.10` | `10.0.0.0/8` |
| `8.8.8.8` | `0.0.0.0/0` |
| `172.16.1.10` | `0.0.0.0/0` |

The default route handles destinations that do not match the more-specific routes.

# 🗁 Why Default Routing Is Important

Without a default route, a router needs an explicit route for every destination network it must reach.

This is impractical for networks that communicate with many external destinations.

### 🖆 Simplifies Routing Configuration

A single default route can provide access to many external networks.

For example, a home router does not need individual routes for every internet destination.

It can forward unknown destinations to its ISP gateway.

### 🖆 Connects Networks to the Internet

Most home, office, and branch networks use a default route pointing toward an ISP or upstream router.

```text
Local Network
      |
    Router
      |
 Default Route
      |
  ISP Gateway
      |
   Internet
```

### 🖆 Reduces Routing Table Complexity

Default routing reduces the number of manually configured routes required on simple networks.

### 🖆 Supports Stub Networks

A stub network has a limited number of external paths, often a single connection to another network.

A default route is frequently sufficient for forwarding traffic outside the local network.

### 🖆 Supports Backup Connectivity

Multiple default routes can provide primary and backup forwarding paths.

This is commonly used in multi-WAN and branch-office designs.

# 🗁 Default Route Formats

The default route uses a zero-length network prefix.

### 🖆 IPv4 Default Route

```text
0.0.0.0/0
```

This route matches every IPv4 destination.

Example:

```text
0.0.0.0/0 via 192.168.1.1
```

Meaning:

Forward IPv4 traffic without a more-specific matching route toward `192.168.1.1`.

### 🖆 IPv6 Default Route

```text
::/0
```

This route matches every IPv6 destination.

Example:

```text
::/0 via 2001:db8:1::1
```

Meaning:

Forward IPv6 traffic without a more-specific matching route toward `2001:db8:1::1`.

### 🖆 Comparison

| Property | IPv4 | IPv6 |
| --- | --- | --- |
| Default prefix | `0.0.0.0/0` | `::/0` |
| Address size | 32 bits | 128 bits |
| Next-hop example | `192.168.1.1` | `2001:db8::1` |
| Route command | `ip route` | `ip -6 route` |
| Neighbor resolution | ARP | NDP |

# 🗁 How Default Routing Works

Consider a network with a LAN router connected to an ISP gateway.

```text
                 Internet
                    |
                ISP Router
               203.0.113.1
                    |
                    |
              203.0.113.2
                LAN Router
                    |
             192.168.1.0/24
                    |
                  PCs
```

The LAN router has:

* A directly connected LAN network.
* A directly connected ISP-facing network.
* A default route pointing to the ISP gateway.

Example routing table:

| Destination | Next Hop |
| --- | --- |
| `192.168.1.0/24` | Directly connected |
| `203.0.113.0/24` | Directly connected |
| `0.0.0.0/0` | `203.0.113.1` |

### 🖆 Packet Forwarding Process

Suppose a PC sends a packet to `8.8.8.8`.

* The PC determines that `8.8.8.8` is outside its local subnet.
* The PC forwards the packet to its default gateway.
* The LAN router receives the packet.
* The router checks its routing table.
* No more-specific route matches `8.8.8.8`.
* The router selects the default route.
* The router forwards the packet toward `203.0.113.1`.
* The ISP router continues forwarding the packet toward its destination.

### 🖆 Important Routing Principle

Default routing does not mean that every packet uses the default route.

The router first performs a route lookup and selects the most-specific matching prefix.

This behavior is known as Longest Prefix Match.

For example:

| Route | Prefix Length |
| --- | ---: |
| `0.0.0.0/0` | 0 |
| `10.0.0.0/8` | 8 |
| `10.10.0.0/16` | 16 |
| `10.10.10.0/24` | 24 |

A packet destined for `10.10.10.50` matches all four prefixes, but the `/24` route is selected because it is the most specific.

# 🗁 Default Route and Default Gateway

The terms default route and default gateway are closely related, but they describe different concepts.

### 🖆 Default Route

A default route is an entry in a routing table that defines where packets should be forwarded when no more-specific route matches.

Example:

```text
0.0.0.0/0 via 192.168.1.1
```

### 🖆 Default Gateway

A default gateway is the next-hop router used by a host or network device to reach destinations outside its directly connected networks.

Example:

```text
Default Gateway: 192.168.1.1
```

### 🖆 Difference

| Default Route | Default Gateway |
| --- | --- |
| Routing table entry | Next-hop router |
| Defines fallback forwarding | Receives traffic forwarded outside local networks |
| Includes a destination prefix | Identifies a gateway address |
| Used by routers and hosts | Commonly configured on hosts and routers |

A host's default gateway is typically represented by its default route.

A router's default route points toward another router or forwarding destination.

# 🗁 Default Route Selection

When a router receives a packet, it performs a route lookup to identify the best forwarding path.

Default routes participate in this process like other routes, but their prefix length is zero.

### 🖆 Longest Prefix Match

The router first considers matching destination prefixes and selects the most-specific one.

Example:

```text
192.168.10.0/24 via 10.0.0.2
192.168.0.0/16 via 10.0.0.3
0.0.0.0/0 via 10.0.0.1
```

For destination `192.168.10.50`, the `/24` route wins.

For destination `192.168.20.50`, the `/16` route wins.

For destination `8.8.8.8`, the default route wins.

### 🖆 Route Preference

If multiple candidate routes have the same destination prefix, the router uses route-selection rules to determine which route is preferred.

Depending on the platform, these rules can involve:

* Route source or administrative distance.
* Route preference.
* Metric.
* Protocol-specific selection.
* Equal-cost multipath rules.
* Route eligibility and next-hop resolution.

A lower metric is preferred in many systems, including the Linux routing model for otherwise comparable routes. MikroTik RouterOS uses route distance as an important route-source preference.

Route-selection behavior is platform-dependent. Do not assume that every vendor uses the same terminology or comparison order.

# 🗁 Multiple Default Routes

A router can have multiple default routes.

Multiple defaults may be configured for:

* Different upstream providers.
* Primary and backup internet connections.
* Equal-cost multipath forwarding.
* Separate routing tables.
* Policy-based routing.

Example:

```text
Default Route A:
0.0.0.0/0 via 192.168.1.1

Default Route B:
0.0.0.0/0 via 10.0.0.1
```

The router must determine whether one route is preferred or whether both should participate in forwarding.

### 🖆 Multiple Routes With Different Metrics

Linux example:

```bash
sudo ip route add default via 192.168.1.1 metric 100
sudo ip route add default via 10.0.0.1 metric 200
```

When both routes are otherwise comparable and valid, Linux prefers the route with the lower metric.

### 🖆 Equal-Cost Default Routes

Multiple equal-cost default routes may be used for ECMP forwarding.

Linux example:

```bash
sudo ip route add default \
  nexthop via 192.168.1.1 dev eth0 \
  nexthop via 10.0.0.1 dev eth1
```

The kernel can distribute traffic across the available next hops according to its multipath forwarding and hashing behavior.

This is not necessarily per-packet round-robin forwarding. Many implementations use flow-based hashing to preserve packet ordering within flows.

# 🗁 Backup Default Routing

Backup default routing provides an alternative forwarding path when the primary default route becomes unavailable.

A common design uses two internet connections.

```text
                    ISP 1
                      |
                 Primary WAN
                      |
                 +---------+
LAN --------------| Router  |
                 +---------+
                      |
                 Backup WAN
                      |
                    ISP 2
```

### 🖆 Primary and Backup Route Configuration

Example:

| Route | Gateway | Preference |
| --- | --- | ---: |
| Primary | `192.168.1.1` | 1 |
| Backup | `10.0.0.1` | 10 |

The primary route is preferred while it is eligible.

When it is withdrawn or becomes inactive, the backup route can be selected.

### 🖆 Linux Example

```bash
sudo ip route add default via 192.168.1.1 metric 100
sudo ip route add default via 10.0.0.1 metric 200
```

Verify:

```bash
ip route show default
```

Check the selected path:

```bash
ip route get 8.8.8.8
```

### 🖆 MikroTik RouterOS Example

Primary route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1 distance=1
```

Backup route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=10.0.0.1 distance=10
```

### 🖆 Gateway Monitoring

A route can remain active even when the upstream internet connection has failed.

For example:

```text
Router → ISP Gateway → ISP Core → Internet
```

The local ISP gateway may continue responding even if connectivity beyond it is broken.

Reliable failover may require monitoring a remote target or tracking a recursive next hop.

Monitoring should also ensure that probes use the intended WAN connection. Otherwise, a backup path may accidentally satisfy a primary-path health check.

# 🗁 IPv4 Default Routing

IPv4 default routing uses the destination prefix:

```text
0.0.0.0/0
```

It can be configured on hosts, routers, firewalls, and other IPv4 forwarding devices.

### 🖆 Example

```text
Destination: 0.0.0.0/0
Next Hop:    192.168.1.1
```

Linux:

```bash
sudo ip route add default via 192.168.1.1
```

MikroTik RouterOS:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1
```

### 🖆 IPv4 Default Route Through an Interface

On an appropriate point-to-point interface, a default route may specify the outgoing interface.

Linux:

```bash
sudo ip route add default dev ppp0
```

For Ethernet networks, specifying a reachable next-hop address is generally more appropriate.

# 🗁 IPv6 Default Routing

IPv6 default routing uses:

```text
::/0
```

An IPv6 router forwards packets toward the configured next hop when no more-specific IPv6 route matches.

### 🖆 Example

```text
Destination: ::/0
Next Hop:    2001:db8:1::1
```

Linux:

```bash
sudo ip -6 route add default via 2001:db8:1::1
```

MikroTik RouterOS:

```routeros
/ipv6 route add dst-address=::/0 gateway=2001:db8:1::1
```

### 🖆 Link-Local IPv6 Default Gateway

IPv6 routers frequently advertise themselves as default routers using Router Advertisements.

The router's link-local address can be used as the next hop.

Example:

```text
fe80::1
```

Linux:

```bash
sudo ip -6 route add default via fe80::1 dev eth0
```

The outgoing interface is required to identify the link on which the link-local address is valid.

### 🖆 IPv6 Router Advertisements

IPv6 hosts can learn default-router information through ICMPv6 Router Advertisements.

Important concepts:

* Router Advertisements identify available default routers.
* Router lifetime indicates how long the router should be considered a default router.
* Prefix Information Options advertise IPv6 prefixes.
* The M and O flags provide information about DHCPv6 usage.
* DHCPv6 does not normally provide the default-router information; that function is handled by Router Advertisements.

### 🖆 Verify IPv6 Default Routing

```bash
ip -6 route show default
```

Inspect the selected route:

```bash
ip -6 route get 2606:4700:4700::1111
```

# 🗁 Default Routing in Linux

Linux uses the `iproute2` package to manage IPv4 and IPv6 routing.

### 🖆 Display the Default Route

IPv4:

```bash
ip route show default
```

IPv6:

```bash
ip -6 route show default
```

Display the complete routing tables:

```bash
ip route show
ip -6 route show
```

### 🖆 Add an IPv4 Default Route

```bash
sudo ip route add default via 192.168.1.1
```

### 🖆 Add an IPv6 Default Route

```bash
sudo ip -6 route add default via 2001:db8:1::1
```

### 🖆 Replace the Default Route

IPv4:

```bash
sudo ip route replace default via 192.168.1.254
```

IPv6:

```bash
sudo ip -6 route replace default via 2001:db8:1::254
```

### 🖆 Delete the Default Route

IPv4:

```bash
sudo ip route del default
```

IPv6:

```bash
sudo ip -6 route del default
```

### 🖆 Inspect the Selected Route

```bash
ip route get 8.8.8.8
```

For IPv6:

```bash
ip -6 route get 2606:4700:4700::1111
```

### 🖆 Configure a Default Route With a Metric

```bash
sudo ip route add default via 192.168.1.1 metric 100
```

A second route can use a higher metric:

```bash
sudo ip route add default via 10.0.0.1 metric 200
```

### 🖆 Useful Linux Commands

| Command | Purpose |
| --- | --- |
| `ip route show default` | Display IPv4 default routes |
| `ip -6 route show default` | Display IPv6 default routes |
| `ip route add default` | Add an IPv4 default route |
| `ip -6 route add default` | Add an IPv6 default route |
| `ip route replace default` | Replace the IPv4 default route |
| `ip route del default` | Delete the IPv4 default route |
| `ip route get <IP>` | Check the selected IPv4 route |
| `ip -6 route get <IP>` | Check the selected IPv6 route |
| `ip rule show` | Display IPv4 policy-routing rules |
| `ip -6 rule show` | Display IPv6 policy-routing rules |

# 🗁 Persistent Default Routing in Ubuntu

Runtime routes created with `ip route` normally disappear after reboot.

Ubuntu commonly uses Netplan to define persistent network configuration.

### 🖆 Configure an IPv4 Default Route

Edit a Netplan configuration file:

```bash
sudo nano /etc/netplan/01-network.yaml
```

Example:

```yaml
network:
  version: 2
  ethernets:
    enp3s0:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
```

Validate:

```bash
sudo netplan generate
```

Apply:

```bash
sudo netplan apply
```

Verify:

```bash
ip route show default
```

### 🖆 Configure an IPv6 Default Route

Example:

```yaml
network:
  version: 2
  ethernets:
    enp3s0:
      addresses:
        - 2001:db8:1::10/64
      routes:
        - to: "::/0"
          via: 2001:db8:1::1
```

Apply:

```bash
sudo netplan generate
sudo netplan apply
```

### 🖆 Configure Multiple Default Routes

Example:

```yaml
network:
  version: 2
  ethernets:
    enp3s0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
          metric: 100

    enp4s0:
      addresses:
        - 10.0.0.2/24
      routes:
        - to: default
          via: 10.0.0.1
          metric: 200
```

When both routes are valid and otherwise comparable, the lower metric is preferred.

This configuration is an example of route preference, not a complete WAN failover design. A production setup should also consider gateway monitoring, source-address selection, policy routing, and return-path behavior.

# 🗁 Default Routing in MikroTik RouterOS

MikroTik RouterOS v7 uses the `/ip route` and `/ipv6 route` menus to configure default routes.

### 🖆 Display IPv4 Default Routes

```routeros
/ip route print where dst-address=0.0.0.0/0
```

Display detailed route information:

```routeros
/ip route print detail where dst-address=0.0.0.0/0
```

### 🖆 Configure an IPv4 Default Route

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1
```

This directs IPv4 traffic without a more-specific matching route toward `192.168.1.1`.

### 🖆 Configure an IPv6 Default Route

```routeros
/ipv6 route add dst-address=::/0 gateway=2001:db8:1::1
```

### 🖆 Configure a Primary Default Route

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1 distance=1
```

### 🖆 Configure a Backup Default Route

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=10.0.0.1 distance=10
```

The lower-distance route is preferred when both routes are valid and otherwise comparable.

### 🖆 Verify the Active Route

```routeros
/ip route print detail where dst-address=0.0.0.0/0
```

Check the route flags and gateway status to understand whether the route is active.

### 🖆 Remove a Default Route

Inspect the entries:

```routeros
/ip route print
```

Remove the correct route using its internal ID:

```routeros
/ip route remove numbers=0
```

The route number is only an example. Always inspect the current route list before removing an entry.

# 🗁 Default Routing in Multi-WAN Networks

Multi-WAN routing uses multiple upstream internet connections to provide redundancy, load distribution, or policy-based traffic forwarding.

A router may have:

* WAN 1 connected to ISP 1.
* WAN 2 connected to ISP 2.
* A LAN interface serving internal clients.

### 🖆 Multi-WAN Architecture

```text
                 ISP 1
                   |
                 WAN 1
                   |
              +---------+
LAN -----------| Router  |
              +---------+
                   |
                 WAN 2
                   |
                 ISP 2
```

### 🖆 Primary and Backup WAN

The primary WAN carries normal traffic.

The backup WAN is selected when the primary path becomes unavailable.

This is commonly implemented with different route preferences and connectivity monitoring.

### 🖆 ECMP Load Sharing

Equal-cost default routes may distribute traffic across multiple WAN gateways.

This is useful when both internet connections should carry traffic simultaneously.

The actual distribution depends on the routing implementation and hashing configuration.

### 🖆 Policy-Based Default Routing

Policy-based routing selects a routing table or forwarding path based on conditions such as:

* Source IP address.
* Incoming interface.
* Firewall mark.
* Destination prefix.
* Other supported packet or flow attributes.

Example:

```text
192.168.10.0/24 → WAN 1
192.168.20.0/24 → WAN 2
```

This is different from ordinary default-route selection because the routing decision depends on additional policy rules.

### 🖆 Return-Path Considerations

Multi-WAN environments must consider both forward and return paths.

If traffic leaves through WAN 1 but its reply returns through WAN 2, stateful firewall or NAT configurations may reject the response.

A well-designed multi-WAN system should maintain appropriate flow affinity, connection tracking, NAT behavior, and routing symmetry where required.

# 🗁 Best Practices

### 🖆 Use the Correct Default Gateway

Verify that the configured gateway is reachable through the intended interface.

### 🖆 Prefer More-Specific Routes When Required

Do not assume that a default route overrides all other routes.

More-specific matching routes normally take precedence through Longest Prefix Match.

### 🖆 Configure Backup Routes Carefully

Use route preference or metric to define the intended primary and backup paths.

Add health monitoring when failover must detect upstream failures.

### 🖆 Avoid Unnecessary Default Routes

Multiple competing default routes can create unexpected forwarding behavior if their preferences and routing contexts are not understood.

### 🖆 Verify IPv4 and IPv6 Separately

A working IPv4 default route does not guarantee that IPv6 connectivity is available.

Inspect both routing tables.

### 🖆 Consider Policy Routing

Use separate routing tables and policy rules when different sources or interfaces require different upstream paths.

### 🖆 Document Upstream Connections

Record:

* ISP name.
* WAN interface.
* Gateway address.
* Public or assigned address.
* Route preference.
* Monitoring target.
* Backup behavior.
* NAT and firewall dependencies.

# 🗁 Common Problems and Troubleshooting

### 🖆 Missing Default Route

**Symptoms:**

* Local network communication works.
* External destinations are unreachable.
* No suitable route exists for internet-bound traffic.

Check:

```bash
ip route show default
```

Add the appropriate route:

```bash
sudo ip route add default via 192.168.1.1
```

### 🖆 Incorrect Default Gateway

**Symptoms:**

* Packets are forwarded to the wrong router.
* The gateway is unreachable.
* Internet connectivity fails.

Check:

```bash
ip route get 8.8.8.8
```

Verify that the gateway address belongs to a reachable network.

### 🖆 Gateway Unreachable

Check the local interface:

```bash
ip link show
```

Check interface addresses:

```bash
ip addr show
```

Test the gateway:

```bash
ping -c 4 192.168.1.1
```

Inspect neighbor resolution:

```bash
ip neigh show
```

### 🖆 More-Specific Route Overrides the Default Route

A packet may follow a more-specific route instead of the default route.

Inspect:

```bash
ip route get 8.8.8.8
```

Review all matching destination prefixes and verify the selected next hop.

### 🖆 Incorrect Route Metric

A higher-preference route may not be selected as expected because its metric or route preference is configured incorrectly.

Inspect:

```bash
ip route show
```

Compare the candidate routes and their metrics.

### 🖆 Default Route Exists but Internet Access Fails

A valid default route does not guarantee complete internet connectivity.

Investigate:

* Gateway reachability.
* Upstream routing.
* Firewall policies.
* NAT configuration.
* DNS resolution.
* IPv4 or IPv6 connectivity.
* Return routing.

Test IP connectivity:

```bash
ping -c 4 1.1.1.1
```

Test DNS:

```bash
dig example.com
```

Test an HTTP connection:

```bash
curl -I https://example.com
```

### 🖆 IPv6 Default Route Missing

Inspect:

```bash
ip -6 route show default
```

Check whether Router Advertisements are being received:

```bash
sudo tcpdump -ni eth0 icmp6
```

Inspect IPv6 addresses and neighbor information:

```bash
ip -6 addr show
ip -6 neigh show
```

### 🖆 Backup Route Does Not Activate

A primary route may remain active even when the upstream connection has failed.

Check:

* Route eligibility.
* Gateway monitoring.
* Recursive next-hop resolution.
* Probe routing.
* Backup gateway reachability.
* Policy-routing rules.

# 🗁 Practical Labs

### 🖆 Lab: Configure an IPv4 Default Route

**Topology**

```text
PC
 |
LAN: 192.168.1.0/24
 |
Router
 |
WAN: 203.0.113.2/24
 |
ISP Gateway: 203.0.113.1
 |
Internet
```

Configure the default route:

```bash
sudo ip route add default via 203.0.113.1
```

Verify:

```bash
ip route show default
```

Check forwarding:

```bash
ip route get 8.8.8.8
```

### 🖆 Lab: Configure an IPv6 Default Route

Configure an IPv6 gateway:

```text
2001:db8:1::1
```

Add the default route:

```bash
sudo ip -6 route add default via 2001:db8:1::1
```

Verify:

```bash
ip -6 route show default
```

### 🖆 Lab: Configure Primary and Backup Routes

Assume two gateways:

```text
Primary: 192.168.1.1
Backup:  10.0.0.1
```

Configure:

```bash
sudo ip route add default via 192.168.1.1 metric 100
sudo ip route add default via 10.0.0.1 metric 200
```

Verify:

```bash
ip route show default
```

In a controlled environment, disable the primary path and observe whether the backup becomes selected.

### 🖆 Lab: Compare Default and Specific Routes

Configure:

```bash
sudo ip route add 10.0.0.0/8 via 192.168.1.2
sudo ip route add default via 192.168.1.1
```

Check:

```bash
ip route get 10.10.10.10
ip route get 8.8.8.8
```

Expected behavior:

* `10.10.10.10` follows the `/8` route.
* `8.8.8.8` follows the default route.

### 🖆 Lab: Configure Default Routing in MikroTik

Configure a primary route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1 distance=1
```

Configure a backup route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=10.0.0.1 distance=10
```

Inspect:

```routeros
/ip route print detail where dst-address=0.0.0.0/0
```

Test primary and backup route behavior in a controlled environment.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

**What is default routing?**

Default routing is the process of forwarding packets toward a predefined next hop when no more-specific matching route exists.

**What is the IPv4 default route?**

`0.0.0.0/0`.

**What is the IPv6 default route?**

`::/0`.

**Why is default routing important?**

It allows routers to forward traffic to external networks without requiring an individual route for every possible destination.

**What is a default gateway?**

A router that receives traffic from a host or network device when the destination is outside its directly connected networks.

### 🖆 Intermediate Questions

**Does every packet use the default route?**

No. A more-specific matching route is selected instead of the default route.

**Can a router have multiple default routes?**

Yes. Multiple default routes can provide backup paths, ECMP forwarding, or separate routing policies.

**What is the difference between a default route and a static route?**

A static route is any manually configured route. A default route is a route with a zero-length prefix. A default route can itself be statically configured or learned through a routing protocol.

**What happens if a default gateway becomes unreachable?**

The route may become unusable if the routing system detects the failure. The actual behavior depends on gateway monitoring and route tracking.

**What is the difference between a primary and backup default route?**

The primary route is preferred under normal conditions. The backup route has a lower preference and is selected when the primary route is no longer eligible.

### 🖆 Advanced Questions

**How does Longest Prefix Match affect default routing?**

A router selects the most-specific matching route. Since a default route has a prefix length of zero, it is selected only when no more-specific applicable route wins.

**Can a default route be learned dynamically?**

Yes. Routing protocols such as OSPF and BGP can distribute or advertise default routes.

**What is the difference between ECMP and backup default routing?**

ECMP can use multiple equal-cost next hops simultaneously. Backup routing prefers one path and uses another when the preferred path becomes unavailable.

**Why might a router have a default route but still fail to access the internet?**

Possible causes include an unreachable gateway, upstream routing failure, firewall restrictions, missing NAT, DNS failure, or an invalid return path.

**Why is gateway monitoring important in multi-WAN networks?**

It helps detect failures beyond the immediate gateway and allows the router to select an alternative path when the primary internet connection is no longer usable.

# 🗁 Summary

Default routing provides a fallback forwarding path for destinations without a more-specific matching route.

Key concepts:

* IPv4 uses `0.0.0.0/0` as its default route.
* IPv6 uses `::/0` as its default route.
* A default gateway is the next-hop router used to reach external networks.
* Default routes participate in normal route selection.
* Longest Prefix Match gives priority to more-specific routes.
* Multiple default routes can support backup connectivity or ECMP.
* Route metrics and preferences influence selection between competing routes.
* Ubuntu/Linux manages default routes using `ip route` and `ip -6 route`.
* Ubuntu can maintain persistent default routes through Netplan.
* MikroTik RouterOS v7 uses route distance and routing tables to manage default forwarding.
* Multi-WAN environments may require policy routing, connection tracking, NAT, and gateway monitoring.

Default routing is a fundamental building block for internet gateways, enterprise networks, and ISP infrastructure.

# 🗁 References

* [RFC 1812 — Requirements for IP Version 4 Routers](https://www.rfc-editor.org/rfc/rfc1812)
* [RFC 8200 — Internet Protocol, Version 6 (IPv6) Specification](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 4861 — Neighbor Discovery for IP Version 6](https://www.rfc-editor.org/rfc/rfc4861)
* [Linux iproute2 Documentation](https://www.kernel.org/doc/html/latest/networking/)
* [Netplan Documentation](https://netplan.readthedocs.io/)
* [MikroTik RouterOS IP Routing](https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
