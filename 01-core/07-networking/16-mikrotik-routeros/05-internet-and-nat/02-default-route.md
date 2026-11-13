# Default Route

A default route is a routing entry used when a router does not have a more specific route for a destination network.

It provides a path for forwarding packets to destinations that are not explicitly listed in the routing table.

In most home, office, and small ISP networks, the default route points toward the ISP's upstream gateway.

For IPv4, the default route is represented by:

```text
0.0.0.0/0
```

For IPv6, it is represented by:

```text
::/0
```

A typical network looks like this:

```text
                  Internet
                     |
                 ISP Router
                203.0.113.1
                     |
                     |
              MikroTik Router
                ether1 (WAN)
              203.0.113.2/30
                     |
                bridge-LAN
               192.168.10.1/24
                     |
                 LAN Clients
```

When a LAN client accesses an external destination, MikroTik checks its routing table and forwards the packet through the default route if no more specific route matches.

# 🗁 How a Default Route Works

RouterOS uses its routing table to determine where packets should be forwarded.

The routing process generally follows these steps:

* A packet arrives at the router.
* RouterOS examines the destination IP address.
* It searches for matching routes in the routing table.
* If multiple routes match, the most specific prefix is preferred.
* If no more specific route exists, the default route may be selected.
* The packet is forwarded toward the selected next hop.

For example, suppose the routing table contains:

| Destination | Gateway | Purpose |
| --- | --- | --- |
| `192.168.10.0/24` | Connected | LAN network |
| `203.0.113.0/30` | Connected | WAN network |
| `0.0.0.0/0` | `203.0.113.1` | Internet destinations |

A packet destined for `192.168.10.50` uses the connected LAN route.

A packet destined for `8.8.8.8` uses the default route through `203.0.113.1`.

# 🗁 Understanding the Default Route in RouterOS

RouterOS manages IPv4 routes through:

```routeros
/ip route
```

Display the routing table:

```routeros
/ip route print
```

Display detailed route information:

```routeros
/ip route print detail
```

Display only default routes:

```routeros
/ip route print where dst-address=0.0.0.0/0
```

RouterOS v7 also provides the routing table through:

```routeros
/routing route
```

Inspect routes:

```routeros
/routing route print
```

The `/ip route` menu remains commonly used for configuring IPv4 static routes.

# 🗁 Configuring a Static Default Route

A static default route is manually configured by an administrator.

Assume the following WAN configuration:

| Property | Value |
| --- | --- |
| WAN Interface | `ether1` |
| MikroTik WAN IP | `203.0.113.2/30` |
| ISP Gateway | `203.0.113.1` |
| Default Destination | `0.0.0.0/0` |

Add the default route:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=203.0.113.1 comment="ISP Default Route"
```

Verify the route:

```routeros
/ip route print where dst-address=0.0.0.0/0
```

Display detailed information:

```routeros
/ip route print detail where dst-address=0.0.0.0/0
```

The gateway must be reachable through an appropriate connected network or valid next-hop resolution.

The example uses documentation addresses. Replace them with the actual addressing information supplied by your ISP.

# 🗁 Default Route Through an Interface

For certain point-to-point or directly connected WAN designs, a default route can reference an interface.

Example:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=pppoe-out1 comment="PPPoE Default Route"
```

This is commonly used with PPPoE interfaces because the remote peer is associated with the logical point-to-point interface.

For ordinary Ethernet networks, specifying the upstream gateway IP is generally preferable to specifying only the Ethernet interface.

An Ethernet interface can have multiple reachable devices, so a gateway address provides more precise next-hop information.

# 🗁 Default Route Through DHCP Client

When an ISP provides WAN configuration through DHCP, RouterOS can install a default route automatically.

Configure a DHCP client:

```routeros
/ip dhcp-client add interface=ether1 add-default-route=yes use-peer-dns=yes disabled=no
```

The important property is:

```routeros
add-default-route=yes
```

It instructs the DHCP client to install a default route using the gateway received from the DHCP server.

Inspect the DHCP client:

```routeros
/ip dhcp-client print detail
```

Inspect the installed default route:

```routeros
/ip route print where dst-address=0.0.0.0/0
```

If you want to prevent the DHCP client from installing a default route:

```routeros
/ip dhcp-client set [find where interface=ether1] add-default-route=no
```

This is useful when you want to manage default routing manually or through a more advanced routing design.

Avoid creating a second static default route without understanding how the dynamically installed route will interact with it.

# 🗁 Default Route Distance

Route distance is an administrative preference used when selecting between routes with the same destination prefix and otherwise comparable routing attributes.

A lower distance is generally preferred.

Example:

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 comment="Primary ISP"

/ip route add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2 comment="Backup ISP"
```

In this example:

| Route | Gateway | Distance | Role |
| --- | --- | ---: | --- |
| Primary | `203.0.113.1` | 1 | Preferred |
| Backup | `198.51.100.1` | 2 | Less preferred |

If both routes are eligible and have otherwise comparable selection properties, the route with distance `1` is preferred.

The second route can become active when the preferred route becomes unavailable.

However, route distance alone does not guarantee internet failover. A gateway may remain reachable locally while the upstream internet connection is unavailable.

Reliable failover requires appropriate gateway monitoring or reachability checks, which are covered in advanced routing and failover topics.

# 🗁 Understanding Active and Inactive Routes

RouterOS displays route flags that describe route state.

Common route flags include:

| Flag | Meaning |
| --- | --- |
| `A` | Active |
| `D` | Dynamic |
| `S` | Static |
| `C` | Connected |
| `X` | Disabled |
| `I` | Inactive |
| `U` | Unreachable |
| `B` | Blackhole |

The exact flags shown depend on the route type and RouterOS version.

Example:

```routeros
/ip route print
```

A default route with the active and static flags may appear similar to:

```text
Flags: D - DYNAMIC; X - DISABLED, I - INACTIVE; A - ACTIVE; c - CONNECT, s - STATIC
```

The route flags indicate whether the route is installed and usable. An inactive route may have an unreachable gateway, an unavailable interface, or another unresolved routing condition.

# 🗁 Removing a Default Route

Display existing default routes:

```routeros
/ip route print where dst-address=0.0.0.0/0
```

Remove a manually configured default route by its gateway:

```routeros
/ip route remove [find where dst-address=0.0.0.0/0 gateway=203.0.113.1]
```

If the route is dynamically created by a DHCP client, change the DHCP client's configuration instead of attempting to manage the dynamic route as a static entry.

For example:

```routeros
/ip dhcp-client set [find where interface=ether1] add-default-route=no
```

# 🗁 IPv6 Default Route

IPv6 uses a different default route notation.

The IPv6 default route is:

```text
::/0
```

A static IPv6 default route can be configured as follows:

```routeros
/ipv6 route add dst-address=::/0 gateway=2001:db8:10::1 comment="IPv6 Default Route"
```

Inspect IPv6 default routes:

```routeros
/ipv6 route print where dst-address=::/0
```

Display detailed information:

```routeros
/ipv6 route print detail where dst-address=::/0
```

The example gateway uses a documentation prefix. Replace it with the actual IPv6 next hop provided by the upstream network.

IPv6 default routes can also be learned through Router Advertisements, depending on the interface and Neighbor Discovery configuration.

# 🗁 Testing Default Route Connectivity

A configured default route does not automatically mean that internet connectivity is working.

Test connectivity at different points in the path.

**Check the routing table**

```routeros
/ip route print
```

Confirm that a usable IPv4 default route exists.

**Ping the ISP gateway**

```routeros
/ping 203.0.113.1
```

Replace the example address with your actual ISP gateway.

**Ping a public IP address**

```routeros
/ping 1.1.1.1
```

This tests IP connectivity beyond the local ISP gateway.

**Test DNS resolution**

```routeros
:put [:resolve example.com]
```

If IP connectivity works but DNS resolution fails, investigate DNS configuration separately.

**Trace the route**

```routeros
/tool traceroute 1.1.1.1
```

Traceroute helps identify where packets stop receiving responses along the path.

Some intermediate routers may not respond to traceroute probes, so missing responses do not always indicate a forwarding failure.

# 🗁 Common Default Route Problems

| Problem | Possible Cause | What to Check |
| --- | --- | --- |
| No default route | Static route missing or DHCP route disabled | `/ip route print` |
| Default route inactive | Gateway or interface unavailable | Route details and interface status |
| Router cannot reach ISP gateway | Incorrect WAN IP, subnet, or physical connection | WAN address and link |
| Router reaches gateway but not internet | ISP upstream issue or missing upstream routing | Traceroute and ISP connectivity |
| Multiple default routes behave unexpectedly | Route distance or routing attributes | Route details |
| Backup route never becomes active | Primary route remains installed despite upstream failure | Gateway monitoring and recursive checks |
| DHCP WAN has no default route | DHCP client route option disabled or no gateway received | DHCP client details |
| IPv6 destinations unreachable | Missing IPv6 default route or upstream configuration | `/ipv6 route print` |

# 🗁 Practical Lab: Configure a Static Default Route

**Lab objective**

Configure a static IPv4 default route and verify internet-bound routing.

**Network topology**

```text
                  Internet
                     |
                 ISP Gateway
                 203.0.113.1
                     |
                  ether1
               MikroTik Router
               203.0.113.2/30
                     |
                  bridge-LAN
               192.168.10.1/24
                     |
                  Client PC
               192.168.10.100
```

**Step A: Configure the WAN address**

```routeros
/ip address add address=203.0.113.2/30 interface=ether1 comment="WAN Address"
```

**Step B: Configure the LAN address**

```routeros
/ip address add address=192.168.10.1/24 interface=bridge-LAN comment="LAN Gateway"
```

**Step C: Add the default route**

```routeros
/ip route add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 comment="Primary Default Route"
```

**Step D: Verify the route**

```routeros
/ip route print where dst-address=0.0.0.0/0
```

**Step E: Test the gateway**

```routeros
/ping 203.0.113.1
```

**Step F: Test external connectivity**

```routeros
/ping 1.1.1.1
```

**Step G: Inspect the route path**

```routeros
/tool traceroute 1.1.1.1
```

The lab assumes that the upstream network is configured to provide connectivity beyond the ISP gateway. It also assumes that any required firewall and NAT configuration is in place.

# 🗁 Best Practices

* Use the correct default gateway supplied by the ISP.
* Verify WAN addressing before configuring a static default route.
* Use DHCP client default-route installation when the ISP provides gateway information dynamically.
* Avoid unnecessary duplicate default routes.
* Use route distance to establish primary and backup route preference.
* Do not rely on route distance alone for complete internet failover.
* Monitor gateway reachability and upstream connectivity.
* Inspect active and inactive route flags during troubleshooting.
* Configure IPv4 and IPv6 default routes independently.
* Document gateway addresses, route distances, and routing policies.

# 🗁 Official Documentation

* [MikroTik RouterOS — IP Routing](https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing)
* [MikroTik RouterOS — DHCP](https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP)
* [MikroTik RouterOS — IPv6](https://help.mikrotik.com/docs/spaces/ROS/pages/103841817/IPv6)
