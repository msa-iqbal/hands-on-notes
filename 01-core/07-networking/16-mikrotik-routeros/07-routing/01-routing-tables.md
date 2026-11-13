# Routing Tables

A routing table is a collection of routes that RouterOS uses to determine where network packets should be forwarded.

When a router receives a packet that must be routed, it examines the destination IP address and searches for the most appropriate route in the selected routing table.

Routing tables are essential for:

* Connecting different networks.
* Selecting the correct gateway.
* Configuring static and default routes.
* Implementing multiple WAN connections.
* Building policy-based routing.
* Separating routing decisions for different traffic groups.

RouterOS v7 supports multiple routing tables, allowing a router to maintain different routing decisions for different traffic.

# 🗁 What Is a Routing Table?

A routing table contains routes that describe how destinations can be reached.

Each route can contain information such as:

| Property | Description |
| --- | --- |
| Destination | Network or host that the route reaches |
| Gateway | Next-hop router or gateway |
| Routing table | Table containing the route |
| Distance | Administrative preference of the route |
| Scope | Route scope used in route resolution |
| Target scope | Scope used when resolving a gateway |
| Active status | Whether the route is currently usable |
| Immediate gateway | Resolved next hop used for forwarding |

For example, a router might have these routes:

| Destination | Gateway | Routing Table |
| --- | --- | --- |
| `192.168.10.0/24` | Connected | `main` |
| `192.168.20.0/24` | `10.0.0.2` | `main` |
| `0.0.0.0/0` | `203.0.113.1` | `main` |

The router uses these routes to determine how to forward packets.

# 🗁 Main Routing Table

RouterOS automatically provides a default routing table named `main`.

The `main` table is used for ordinary routing when no other routing policy selects a different table.

Display the main routing table:

```routeros
/ip/route/print
```

Display detailed route information:

```routeros
/ip/route/print detail
```

Display routes from the main table explicitly:

```routeros
/ip/route/print where routing-table=main
```

The main table normally contains connected routes, static routes, dynamic routes, and the default route when configured.

# 🗁 Routing Information Base and Forwarding Information Base

RouterOS uses routing information to determine the best paths and install usable routes for packet forwarding.

Two important concepts are:

**Routing Information Base (RIB)**

The RIB contains routing information learned from different sources, such as:

* Connected networks.
* Static routes.
* OSPF.
* BGP.
* Other supported routing protocols.

The RIB can contain multiple candidate routes to the same destination.

**Forwarding Information Base (FIB)**

The FIB contains the routes used for actual packet forwarding.

RouterOS uses the selected routing information to build forwarding entries.

A simplified relationship is:

```text
Routing Information Sources
          |
          v
     Routing RIB
          |
          v
   Route Selection
          |
          v
      Routing FIB
          |
          v
    Packet Forwarding
```

A route appearing in routing information does not necessarily mean it is the active forwarding route.

# 🗁 Routing Tables in RouterOS v7

RouterOS v7 supports multiple routing tables.

The default `main` table is created automatically. Additional tables must be created before routes can be assigned to them.

Create a routing table:

```routeros
/routing/table
add name=to-wan2 fib
```

The `fib` property enables the table to be used for forwarding.

Display configured routing tables:

```routeros
/routing/table/print
```

Example output conceptually:

```text
Flags: D - dynamic, X - disabled, I - invalid, U - used

0 name="main" fib
1 name="to-wan2" fib
```

The exact output and flags depend on the RouterOS version and configuration.

# 🗁 Creating Multiple Routing Tables

Multiple routing tables allow different traffic groups to use different routes.

For example, consider a router connected to two internet providers:

| Component | WAN 1 | WAN 2 |
| --- | --- | --- |
| Interface | `ether1` | `ether2` |
| Gateway | `203.0.113.1` | `198.51.100.1` |
| Purpose | Primary internet | Secondary internet |

The router can use:

* `main` for ordinary traffic.
* `to-wan2` for traffic that should use WAN 2.

Create the additional table:

```routeros
/routing/table
add name=to-wan2 fib
```

Create a default route in the additional table:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to-wan2
```

The `@main` notation specifies that the gateway should be resolved using the `main` routing table.

This is useful when the gateway is reachable through a route in another table.

The gateway must still be reachable through the actual network topology. Creating a routing table does not automatically establish connectivity to its gateway.

# 🗁 Assigning Routes to Routing Tables

A route can be assigned to a specific routing table using the `routing-table` property.

Example main-table default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 routing-table=main
```

Example secondary-table default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to-wan2
```

These routes provide different default gateways depending on the selected routing table.

Display routes from the secondary table:

```routeros
/ip/route/print where routing-table=to-wan2
```

# 🗁 Connected Routes

When an IP address is assigned to an interface, RouterOS normally creates a connected route for the associated network.

For example:

```routeros
/ip/address
add address=192.168.10.1/24 interface=bridge-LAN
```

This creates a connected route for:

```text
192.168.10.0/24
```

The router can directly reach hosts within this network through `bridge-LAN`, provided the interface and Layer 2 connectivity are operational.

Inspect connected routes:

```routeros
/ip/route/print where dst-address=192.168.10.0/24
```

Connected routes are important because gateways and next hops must be reachable through valid routing information.

# 🗁 Route Selection and Longest Prefix Match

When multiple routes match a destination, RouterOS uses the most specific matching destination prefix.

This is called **Longest Prefix Match (LPM)**.

Consider these routes:

| Destination | Gateway |
| --- | --- |
| `0.0.0.0/0` | Gateway A |
| `10.0.0.0/8` | Gateway B |
| `10.10.0.0/16` | Gateway C |
| `10.10.20.0/24` | Gateway D |

For a packet destined for `10.10.20.50`, the router selects:

```text
10.10.20.0/24
```

Because `/24` is more specific than `/16`, `/8`, or `/0`.

The default route `/0` matches destinations that do not have a more specific matching route.

When multiple routes have the same destination prefix, additional route-selection criteria determine which route becomes active.

# 🗁 Routing Tables and Policy-Based Routing

Policy-based routing allows traffic to use a routing table based on criteria other than the destination address alone.

For example:

* Users in one LAN subnet use WAN 1.
* Users in another LAN subnet use WAN 2.
* Selected services use a dedicated internet connection.
* Traffic from a particular VPN uses a separate routing path.

RouterOS provides routing rules and firewall mangle marks to influence routing-table selection.

A routing table alone does not select traffic automatically. A routing rule or appropriate routing mark must direct the traffic to that table.

Policy-based routing is covered in detail in the upcoming routing documents.

# 🗁 Routing Lookup

A routing lookup helps determine which route RouterOS would use for a destination.

Use the routing lookup command:

```routeros
/routing/route/print
```

For a more focused inspection, filter by routing table:

```routeros
/routing/route/print where routing-table=main
```

You can also inspect the IP route configuration:

```routeros
/ip/route/print detail
```

When troubleshooting a route, examine:

* Destination prefix.
* Routing table.
* Gateway.
* Route status.
* Distance.
* Immediate gateway.
* Whether the selected table has a usable route.

# 🗁 Practical Lab: Create and Inspect Routing Tables

**Objective:** Create two routing tables and configure different default routes.

Assumed topology:

```text
                 WAN 1
              203.0.113.1
                   |
                ether1
                   |
             +-----------+
             | MikroTik  |
             |    R1     |
             +-----------+
                   |
                ether2
                   |
                 WAN 2
              198.51.100.1
```

Assumptions:

| Component | Configuration |
| --- | --- |
| WAN 1 interface | `ether1` |
| WAN 1 gateway | `203.0.113.1` |
| WAN 2 interface | `ether2` |
| WAN 2 gateway | `198.51.100.1` |

The gateway addresses are documentation examples. The lab requires both gateways to be reachable through their respective interfaces.

**Create the secondary routing table**

```routeros
/routing/table
add name=to-wan2 fib
```

**Configure the primary default route**

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 routing-table=main
```

**Configure the secondary default route**

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to-wan2
```

**Inspect the routing tables**

```routeros
/routing/table/print
```

**Inspect the main routing table**

```routeros
/ip/route/print where routing-table=main
```

**Inspect the secondary routing table**

```routeros
/ip/route/print where routing-table=to-wan2
```

**Expected result**

* The `main` table contains its default route through WAN 1.
* The `to-wan2` table contains its default route through WAN 2.
* Both tables are configured for forwarding.
* The gateways are resolvable and the routes become active when their paths are reachable.

This lab creates routing tables and routes. It does not yet configure traffic selection between them.

# 🗁 Troubleshooting Routing Tables

**A routing table does not exist**

Inspect configured tables:

```routeros
/routing/table/print
```

Create the required table if it is missing:

```routeros
/routing/table
add name=to-wan2 fib
```

**A route is inactive**

Inspect the route:

```routeros
/ip/route/print detail
```

Check:

* Whether the gateway is reachable.
* Whether the interface is operational.
* Whether the gateway can be resolved through the specified routing table.
* Whether the route is disabled.
* Whether a more preferred route exists.

**Traffic does not use the expected routing table**

Check:

* Whether the table exists.
* Whether the table has a valid route.
* Whether a routing rule selects the table.
* Whether a mangle routing mark is applied.
* Whether router-originated traffic requires separate handling.

**The gateway cannot be resolved**

Verify the connected network and gateway address:

```routeros
/ip/address/print
/ip/route/print detail
```

A gateway must be reachable through valid routing information. Referencing `@main` specifies where gateway resolution should occur; it does not make an unreachable gateway reachable.

# 🗁 Best Practices

* Use `main` for ordinary routing unless a separate routing policy is required.
* Give additional routing tables meaningful names.
* Create tables with `fib` when they are intended for packet forwarding.
* Verify gateway reachability before relying on a route.
* Keep routing policies documented.
* Inspect route status before troubleshooting higher-level connectivity.
* Understand longest prefix match before changing route preferences.
* Use routing rules or mangle marks when traffic must select a non-main table.
* Test multiple routing tables in an isolated lab before deploying them to production.

# 🗁 Knowledge Check

* What is a routing table?
* What is the purpose of the `main` routing table?
* What is the difference between RIB and FIB?
* Why must an additional RouterOS v7 routing table use `fib` for forwarding?
* How do you create a routing table?
* What does the `routing-table` property specify?
* What is a connected route?
* What is Longest Prefix Match?
* What does the `@main` notation mean when specifying a gateway?
* Does creating multiple routing tables automatically distribute traffic between WAN connections?
* What is the relationship between routing tables and policy-based routing?

# 🗁 References

* MikroTik RouterOS Documentation — IP Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing>
* MikroTik RouterOS Documentation — Policy Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/59965508/Policy+Routing>
* MikroTik RouterOS Documentation — Route Selection: <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing>
