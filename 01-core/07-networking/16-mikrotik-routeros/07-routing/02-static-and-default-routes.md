# Static and Default Routes

Static routing is a method of manually configuring routes on a router. A network administrator specifies the destination network and the gateway or outgoing interface used to reach it.

A default route is a special route used when no more specific route matches the destination address.

In MikroTik RouterOS v7, static and default routes are commonly used for:

* Connecting remote networks.
* Providing internet connectivity.
* Configuring primary and backup gateways.
* Controlling traffic paths.
* Building small and medium-sized network infrastructures.
* Establishing routing policies.

# 🗁 What Is a Static Route?

A static route is a manually configured entry in a routing table.

It tells the router how to reach a particular destination network.

For example:

| Property | Value |
| --- | --- |
| Destination network | `192.168.20.0/24` |
| Gateway | `192.168.10.2` |
| Routing table | `main` |
| Distance | `1` |

This route tells the router to forward packets destined for `192.168.20.0/24` through the next-hop router at `192.168.10.2`.

# 🗁 Static Route Syntax

The RouterOS v7 command structure is:

```routeros
/ip/route
add dst-address=<destination-network> gateway=<gateway-address>
```

Example:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=192.168.10.2
```

The route is added to the `main` routing table by default.

Specify a routing table explicitly when necessary:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=192.168.10.2 routing-table=main
```

# 🗁 Understanding Route Properties

RouterOS static routes support several important properties.

| Property | Description |
| --- | --- |
| `dst-address` | Destination network or host |
| `gateway` | Next-hop gateway or outgoing interface |
| `routing-table` | Routing table containing the route |
| `distance` | Administrative preference of the route |
| `check-gateway` | Gateway reachability checking method |
| `scope` | Route scope used during gateway resolution |
| `target-scope` | Maximum scope considered for gateway resolution |
| `comment` | Description of the route |
| `disabled` | Controls whether the route is disabled |

Example:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=192.168.10.2 distance=1 comment="Route to branch network"
```

# 🗁 What Is a Default Route?

A default route is used when the routing table has no more specific matching route for a destination.

For IPv4, the default destination is:

```text
0.0.0.0/0
```

For IPv6, the default destination is:

```text
::/0
```

A typical router uses a default route to forward internet-bound traffic toward an upstream gateway.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1
```

This tells the router to send traffic without a more specific matching route through `203.0.113.1`.

# 🗁 How Default Routing Works

Consider the following routing table:

| Destination | Gateway |
| --- | --- |
| `192.168.10.0/24` | Connected |
| `192.168.20.0/24` | `192.168.10.2` |
| `0.0.0.0/0` | `203.0.113.1` |

When the router receives a packet:

* Destination `192.168.10.50` matches the connected route.
* Destination `192.168.20.50` matches the static route.
* Destination `8.8.8.8` matches the default route.

The router uses the most specific matching destination prefix.

# 🗁 Configure a Static Route

Consider this topology:

```text
LAN-A                  MikroTik R1                  LAN-B
192.168.10.0/24        192.168.10.1                192.168.20.0/24
       |                      |                            |
       +----------------------+----------------------------+
                              |
                        192.168.10.2
                         MikroTik R2
```

A more practical two-router arrangement is:

```text
192.168.10.0/24
       |
       |
  MikroTik R1
       |
       | 10.0.0.1/30
       |
       | 10.0.0.2/30
       |
  MikroTik R2
       |
       |
192.168.20.0/24
```

Network configuration:

| Device | Interface | IP Address |
| --- | --- | --- |
| R1 | LAN | `192.168.10.1/24` |
| R1 | Link to R2 | `10.0.0.1/30` |
| R2 | Link to R1 | `10.0.0.2/30` |
| R2 | LAN | `192.168.20.1/24` |

**Configure R1**

Add a route to LAN-B:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=10.0.0.2 comment="Route to LAN-B"
```

**Configure R2**

Add a route to LAN-A:

```routeros
/ip/route
add dst-address=192.168.10.0/24 gateway=10.0.0.1 comment="Route to LAN-A"
```

Both routers now have routes to the remote LAN.

The interfaces and IP addresses must also be configured, and firewall policies must permit the required traffic.

# 🗁 Configure a Default Route

Consider a router connected to an ISP.

| Component | Value |
| --- | --- |
| WAN interface | `ether1` |
| Router WAN address | `203.0.113.10/24` |
| ISP gateway | `203.0.113.1` |
| LAN subnet | `192.168.10.0/24` |

Configure the default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 comment="Default route through ISP"
```

Verify the route:

```routeros
/ip/route/print
```

Test gateway connectivity:

```routeros
/ping 203.0.113.1
```

Test an external IP address:

```routeros
/ping 1.1.1.1
```

A successful ping to an external IP confirms IP-level reachability for that test, but does not by itself verify DNS resolution or general internet service availability.

# 🗁 Route Distance

Route distance determines administrative preference when multiple eligible routes have the same destination prefix.

A lower distance is preferred over a higher distance.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 comment="Primary ISP"
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2 comment="Backup ISP"
```

Conceptually:

| Route | Gateway | Distance | Preference |
| --- | --- | ---: | --- |
| Primary | `203.0.113.1` | 1 | Higher |
| Backup | `198.51.100.1` | 2 | Lower |

When both routes are eligible, the route with the lower distance is preferred.

If the primary route becomes inactive and the backup route remains eligible, the backup route can become active.

Route distance is not the same as network path length or the number of routers between two destinations.

# 🗁 Gateway Reachability Checking

RouterOS can check whether a gateway is reachable using the `check-gateway` property.

Supported checking methods include:

* `ping`
* `arp`

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 check-gateway=ping distance=1 comment="Primary ISP"
```

When gateway checking is enabled, RouterOS periodically tests the gateway according to its configured checking method.

If the gateway is considered unreachable, the route can become inactive.

A backup route can then be selected if it is eligible.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2 comment="Backup ISP"
```

Gateway checking only tests the configured gateway. It does not guarantee that the entire upstream internet path is working.

For end-to-end WAN monitoring, more advanced mechanisms may be needed.

# 🗁 Primary and Backup Default Routes

A router can use multiple default routes with different distances.

Example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 check-gateway=ping comment="Primary WAN"
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2 check-gateway=ping comment="Backup WAN"
```

Expected behavior:

```text
                 Internet
                    |
              +-----------+
              | MikroTik  |
              |  Router   |
              +-----------+
                 /     \
                /       \
          Primary      Backup
          Gateway      Gateway
          Distance 1   Distance 2
```

* Primary route is preferred while active.
* Backup route remains available.
* If the primary gateway becomes unreachable according to the configured check, the backup route may become active.

Both gateways must be independently reachable through their respective WAN interfaces. The routing design must also ensure that return traffic and source NAT use the correct WAN path.

# 🗁 Equal-Cost Default Routes

RouterOS can install multiple equally preferred routes to the same destination.

For example:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=1
```

When both routes are eligible and have equal preference, RouterOS can use ECMP (Equal-Cost Multi-Path) forwarding.

ECMP distributes eligible traffic across multiple next hops according to the forwarding implementation.

It is not the same as a primary/backup configuration.

A complete multi-WAN design must also account for:

* Connection tracking.
* Source NAT.
* Return-path consistency.
* Gateway reachability.
* Policy-based routing requirements.

# 🗁 IPv6 Static and Default Routes

IPv6 uses different address notation from IPv4.

The IPv6 default route is:

```text
::/0
```

Example:

```routeros
/ipv6/route
add dst-address=::/0 gateway=2001:db8:1::1
```

This example assumes the next-hop gateway is reachable through the IPv6 routing configuration.

Inspect IPv6 routes:

```routeros
/ipv6/route/print
```

IPv6 static routes work similarly to IPv4 static routes, but use IPv6 prefixes and gateways.

# 🗁 Managing Static Routes

**Display routes**

```routeros
/ip/route/print
```

**Display detailed route information**

```routeros
/ip/route/print detail
```

**Display routes for a destination**

```routeros
/ip/route/print where dst-address=192.168.20.0/24
```

**Disable a route**

```routeros
/ip/route/disable [find where comment="Route to LAN-B"]
```

**Enable a route**

```routeros
/ip/route/enable [find where comment="Route to LAN-B"]
```

**Remove a route**

```routeros
/ip/route/remove [find where comment="Route to LAN-B"]
```

Use unique comments when managing routes through scripts or `find` expressions. This helps avoid accidentally modifying multiple entries.

# 🗁 Practical Lab: Static Routing Between Two Routers

**Objective:** Establish communication between two separate LAN networks using static routes.

Topology:

```text
PC-A                  R1              R2                  PC-B
192.168.10.10         |               |               192.168.20.10
                      |               |
                10.0.0.1/30     10.0.0.2/30
                      |---------------|
```

**R1 configuration**

Configure the inter-router address:

```routeros
/ip/address
add address=10.0.0.1/30 interface=ether2
```

Configure the LAN address:

```routeros
/ip/address
add address=192.168.10.1/24 interface=ether3
```

Add the remote LAN route:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=10.0.0.2
```

**R2 configuration**

Configure the inter-router address:

```routeros
/ip/address
add address=10.0.0.2/30 interface=ether2
```

Configure the LAN address:

```routeros
/ip/address
add address=192.168.20.1/24 interface=ether3
```

Add the remote LAN route:

```routeros
/ip/route
add dst-address=192.168.10.0/24 gateway=10.0.0.1
```

**Configure the PCs**

PC-A:

```text
IP address:      192.168.10.10
Subnet mask:     255.255.255.0
Default gateway: 192.168.10.1
```

PC-B:

```text
IP address:      192.168.20.10
Subnet mask:     255.255.255.0
Default gateway: 192.168.20.1
```

**Test connectivity**

From R1:

```routeros
/ping 10.0.0.2
/ping 192.168.20.1
```

From PC-A:

```bash
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.20.10
```

Expected result:

* Both routers can communicate across the inter-router link.
* Each router can reach the remote LAN.
* PC-A can communicate with PC-B.

If communication fails, check interface status, addressing, route status, firewall forwarding policies, and the hosts' default gateways.

# 🗁 Troubleshooting Static and Default Routes

**Destination network is unreachable**

Inspect the routing table:

```routeros
/ip/route/print detail
```

Check whether a matching route exists and is active.

**Gateway is unreachable**

Test the gateway:

```routeros
/ping 203.0.113.1
```

Verify the connected interface and address:

```routeros
/interface/print
/ip/address/print
```

**Backup route does not become active**

Check:

* Primary route status.
* Backup route status.
* Route distances.
* Gateway reachability.
* Whether both routes belong to the intended routing table.

**Static route exists but traffic does not reach the destination**

Check:

* Whether the next-hop gateway is correct.
* Whether the remote router has a return route.
* Whether firewall rules permit forwarding.
* Whether NAT is modifying traffic unexpectedly.
* Whether the selected routing table contains the intended route.

**Internet access works by IP but not by domain name**

Inspect DNS configuration:

```routeros
/ip/dns/print
```

Routing and DNS resolution are separate functions. A working default route does not automatically guarantee working DNS.

# 🗁 Best Practices

* Use static routes for networks with predictable paths.
* Use default routes for destinations without more specific routes.
* Give routes descriptive comments.
* Use route distances deliberately.
* Configure gateway checking where appropriate.
* Verify both forward and return routing paths.
* Keep backup routes in the correct routing table.
* Avoid confusing route distance with physical path length.
* Test failover behavior before deploying it.
* Document route ownership and intended behavior.

# 🗁 Knowledge Check

* What is a static route?
* What is a default route?
* What does `0.0.0.0/0` represent?
* What is the difference between a static route and a default route?
* How does RouterOS select between matching destination prefixes?
* What does route distance control?
* Why would you configure `check-gateway=ping`?
* What is the difference between a backup route and an ECMP route?
* Why does a static route require a reachable next hop?
* Why must a remote network have a return route?
* What is the IPv6 default route?
* How can you verify whether a route is active?

# 🗁 References

* MikroTik RouterOS Documentation — IP Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing>
* MikroTik RouterOS Documentation — Policy Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/59965508/Policy+Routing>
* MikroTik RouterOS Documentation — IPv6: <https://help.mikrotik.com/docs/spaces/ROS/pages/328247/IPv6>
