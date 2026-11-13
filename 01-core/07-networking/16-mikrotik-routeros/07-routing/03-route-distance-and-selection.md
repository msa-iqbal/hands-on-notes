# Route Distance and Selection

MikroTik RouterOS uses route attributes and routing-table information to determine which route should be selected when multiple routes can reach the same destination.

**Route distance** is one of the most important attributes in this process. It represents the administrative preference of a route. When multiple eligible routes provide the same destination prefix, the route with the lower distance is generally preferred.

Understanding route selection is essential for:

* Configuring primary and backup WAN connections.
* Managing static and dynamic routes.
* Building reliable failover configurations.
* Understanding why a particular route is active.
* Troubleshooting unexpected routing behavior.

# 🗁 What Is Route Distance?

Route distance is an administrative preference value assigned to a route.

A lower distance indicates a more preferred route.

For example:

| Route | Destination | Gateway | Distance |
| --- | --- | --- | ---: |
| Primary WAN | `0.0.0.0/0` | `192.168.1.1` | 1 |
| Backup WAN | `0.0.0.0/0` | `192.168.2.1` | 2 |
| Secondary backup | `0.0.0.0/0` | `192.168.3.1` | 5 |

All three routes describe the same destination prefix: `0.0.0.0/0`.

If all three routes are eligible and otherwise comparable, RouterOS prefers the route with distance `1`.

The other routes can remain available as alternatives.

# 🗁 Route Distance vs. Routing Metric

Route distance and routing metrics are related concepts, but they are not identical.

**Route distance:**

* Represents administrative preference.
* Helps RouterOS choose between competing routes.
* Lower values are preferred.
* Can be configured on static routes and influenced by routing protocols.

**Routing metric:**

* Represents a protocol-specific measurement of route cost.
* Its calculation depends on the routing protocol.
* Examples include OSPF cost and BGP path attributes.
* Is used by a routing protocol to evaluate possible paths.

A routing protocol may calculate its preferred path using its own metrics before RouterOS evaluates the route's administrative preference.

Do not assume that every routing protocol uses the same metric calculation.

# 🗁 Default Route Distance Values

RouterOS assigns default distance values to different route types and routing protocols.

Common defaults include:

| Route Type | Typical Default Distance |
| --- | ---: |
| Connected | 0 |
| Static | 1 |
| OSPF | 110 |
| BGP | 20 (eBGP), 200 (iBGP) |
| RIP | 120 |

These are commonly used defaults, not universal values for every possible configuration. Route properties can be configured, and protocol-specific behavior can affect the final result.

# 🗁 Viewing Route Distance

Use the following command to display IPv4 routes:

```routeros
/ip/route/print
```

For a more detailed view:

```routeros
/ip/route/print detail
```

To display routes for a specific destination:

```routeros
/ip/route/print where dst-address=0.0.0.0/0
```

The output includes route information such as:

* Destination prefix.
* Gateway.
* Routing table.
* Distance.
* Route state and flags.
* Routing protocol information.

To inspect the active routing table:

```routeros
/routing/route/print
```

This is useful when investigating route selection and routing-table state.

# 🗁 Configuring Static Route Distance

You can explicitly configure the distance of a static route.

Example primary route:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.1.1 \
    distance=1 \
    comment="Primary WAN"
```

Example backup route:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.2.1 \
    distance=2 \
    comment="Backup WAN"
```

Both routes target the default destination.

When both are eligible, the route with distance `1` is preferred.

The backup route can become active if the preferred route becomes unavailable according to the configured route-resolution and reachability behavior.

# 🗁 Changing an Existing Route Distance

First, inspect the configured routes:

```routeros
/ip/route/print
```

Identify the route that you want to modify.

You can change its distance using its item number:

```routeros
/ip/route/set 0 distance=5
```

Replace `0` with the correct item number from your router.

You can also identify a route using its comment:

```routeros
/ip/route/set [find where comment="Backup WAN"] distance=2
```

Verify the updated configuration:

```routeros
/ip/route/print detail
```

# 🗁 Route Selection Process

RouterOS route selection involves more than simply comparing distance values.

A simplified selection process is:

* Determine which routes are eligible and usable.
* Consider the destination prefix length.
* Prefer the most specific matching destination prefix.
* When competing routes have the same prefix, evaluate route preference, including distance.
* Consider additional route attributes and protocol-specific selection behavior.
* Install eligible selected routes into the relevant routing table or tables.

The most important concept is **longest-prefix match**.

For example:

| Destination | Gateway | Distance |
| --- | --- | ---: |
| `0.0.0.0/0` | `192.168.1.1` | 1 |
| `10.10.0.0/16` | `192.168.2.1` | 10 |
| `10.10.20.0/24` | `192.168.3.1` | 20 |

Consider a packet destined for `10.10.20.50`.

The `/24` route is more specific than the `/16` route and the default route. Therefore, it is selected for that destination if it is eligible, even though its distance is higher.

Route distance does not override longest-prefix matching.

# 🗁 Primary and Backup WAN Example

A common use case for route distance is configuring two internet connections.

Assume the following network:

| Interface | Gateway | Purpose |
| --- | --- | --- |
| WAN1 | `192.168.1.1` | Primary internet |
| WAN2 | `192.168.2.1` | Backup internet |

Configure the primary default route:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.1.1 \
    distance=1 \
    comment="Primary WAN"
```

Configure the backup default route:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.2.1 \
    distance=2 \
    comment="Backup WAN"
```

When both routes are usable:

* WAN1 is preferred.
* WAN2 remains an alternative.
* Traffic normally uses WAN1 for destinations covered by the default route.

If WAN1 becomes unavailable and its route is no longer eligible, RouterOS can select WAN2.

**Important:** Route distance alone does not detect every internet outage. If the WAN gateway remains reachable while connectivity beyond it is broken, a route using only gateway reachability may remain active. End-to-end monitoring requires an appropriate reachability-checking design, such as recursive routing.

# 🗁 Equal-Cost Routes

When multiple eligible routes have the same destination prefix and equal preference, RouterOS may install multiple next hops for equal-cost multipath routing (ECMP), depending on route properties and configuration.

Example:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.1.1 \
    distance=1 \
    comment="WAN1"

/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.2.1 \
    distance=1 \
    comment="WAN2"
```

Both routes have the same destination and distance.

This can provide multiple next hops rather than a simple primary/backup arrangement.

However, equal distance does not automatically guarantee that every route will participate in ECMP. Gateway resolution, route attributes, routing-table configuration, and RouterOS route-selection behavior also matter.

For connection-based load balancing, additional configuration may be necessary to maintain consistent traffic paths.

# 🗁 Distance and Dynamic Routing Protocols

Route distance also affects the selection of routes learned through dynamic routing protocols.

For example:

* OSPF routes commonly have a default distance of `110`.
* BGP routes commonly use `20` for eBGP and `200` for iBGP.
* RIP routes commonly have a default distance of `120`.

Suppose a router has two eligible routes to the same destination:

| Route Source | Distance |
| --- | ---: |
| Static | 1 |
| OSPF | 110 |

The static route is generally preferred because it has the lower distance.

This allows administrators to configure a static route as the preferred path and a dynamic route as an alternative, provided both routes are otherwise eligible and compete for the same prefix in the same routing context.

# 🗁 Common Route Selection Problems

**Unexpected route is active**

Possible causes:

* Another route has a lower distance.
* The expected route is inactive or unreachable.
* The destination prefixes are different.
* A more specific route matches the destination.
* The routes belong to different routing tables.

Useful commands:

```routeros
/ip/route/print detail
/routing/route/print detail
```

**Backup route is not selected**

Possible causes:

* The primary route is still considered reachable.
* The backup gateway cannot be resolved.
* The backup route is disabled.
* The backup route is in a different routing table.
* The backup route does not compete for the same destination prefix.

Check:

```routeros
/ip/route/print detail
```

**Traffic uses an unexpected gateway**

Possible causes:

* A more specific route exists.
* Routing rules select a different routing table.
* Mangle rules mark traffic for policy routing.
* The selected routing table contains another preferred route.
* An existing connection continues using its established connection-tracking state.

Useful commands:

```routeros
/routing/rule/print
/ip/firewall/mangle/print
/ip/route/print detail
```

# 🗁 Best Practices

* Use clear distance values to distinguish primary and backup routes.
* Keep route comments descriptive.
* Verify route eligibility instead of relying only on configured distance.
* Understand longest-prefix matching before troubleshooting distance.
* Use separate routing tables when implementing policy-based routing.
* Avoid changing dynamic routing distances without understanding protocol behavior.
* Test failover in a controlled environment before applying it to production.
* Use recursive routing or another suitable monitoring mechanism when gateway reachability alone is insufficient.
* Document the purpose of every manually configured route.

# 🗁 Practical Lab

**Objective:** Configure two default routes and observe how route distance affects route selection.

**Topology:**

```text
                 Internet
                    |
             +-------------+
             |   MikroTik  |
             +-------------+
                |       |
              WAN1     WAN2
                |       |
          192.168.1.1 192.168.2.1
          Primary     Backup
```

**Step 1: Configure the primary route**

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.1.1 \
    distance=1 \
    comment="Primary WAN"
```

**Step 2: Configure the backup route**

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.2.1 \
    distance=2 \
    comment="Backup WAN"
```

**Step 3: Inspect the routes**

```routeros
/ip/route/print detail
```

Observe the route flags, gateways, and distances.

**Step 4: Test the primary route**

Check connectivity:

```routeros
/ping 8.8.8.8
```

Inspect the routing table and verify which default route is active.

**Step 5: Simulate primary gateway failure**

In an isolated lab, make the primary gateway unreachable or disable the relevant WAN connection.

Inspect the routes again:

```routeros
/ip/route/print detail
```

If the primary route becomes ineligible and the backup route is reachable, the backup route should be selected.

**Step 6: Restore the primary connection**

Restore the primary gateway or WAN interface.

Verify whether the lower-distance primary route becomes preferred again.

**Lab observations:**

* Which route was preferred initially?
* What happened when the primary route became unavailable?
* Did the backup route become active?
* What happened after restoring the primary connection?
* Did gateway reachability accurately represent internet connectivity?

# 🗁 Summary

Route distance controls administrative preference among competing eligible routes.

Key points:

* Lower distance means higher preference.
* Distance does not override longest-prefix matching.
* Static routes commonly have a default distance of `1`.
* Dynamic routing protocols have their own typical default distances.
* Equal-cost routes may support ECMP.
* Route distance can support primary and backup WAN configurations.
* Reliable failover also requires appropriate gateway or end-to-end reachability detection.

Understanding route distance and route selection provides the foundation for advanced RouterOS routing, policy routing, OSPF, BGP, and recursive routing configurations.
