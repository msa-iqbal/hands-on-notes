# Recursive Gateway Checks

# 🗁 What Is Recursive Gateway Checking?

Recursive gateway checking is a RouterOS routing technique used to monitor the reachability of a destination beyond the directly connected ISP gateway.

A normal gateway check verifies whether the next-hop gateway responds. However, an ISP gateway may remain reachable even when its upstream Internet connection is unavailable.

Recursive gateway checking addresses this limitation by using a remote IP address as the effective gateway for a default route.

RouterOS resolves that remote IP address through another route pointing to the actual ISP gateway.

This allows the router to monitor connectivity beyond the local gateway.

# 🗁 Why Recursive Gateway Checks Are Needed

Consider a router connected to two ISPs:

```text
                  Internet
                 /        \
                /          \
             ISP-1         ISP-2
                |             |
          Gateway-1      Gateway-2
                |             |
                +------+------+
                       |
                 MikroTik Router
                       |
                    LAN Network
```

Suppose ISP-1 experiences an upstream failure.

The MikroTik router can still ping Gateway-1 because the local connection between the router and ISP gateway remains operational.

A simple gateway check may therefore consider WAN-1 available even though Internet connectivity through that ISP is broken.

Recursive gateway checking monitors a remote destination instead.

# 🗁 How Recursive Gateway Checking Works

The process involves two routes:

* A host route that reaches a remote monitoring address through a specific ISP gateway.
* A default route that uses the remote monitoring address as its recursive gateway.

Example:

```text
Remote monitoring address: 1.1.1.1
ISP gateway:               192.0.2.1

Host route:
1.1.1.1/32 -> 192.0.2.1

Recursive default route:
0.0.0.0/0 -> 1.1.1.1
```

The routing process works as follows:

* RouterOS looks up the recursive gateway address `1.1.1.1`.
* It finds the host route for `1.1.1.1/32`.
* That host route points to the actual ISP gateway `192.0.2.1`.
* RouterOS sends reachability checks toward `1.1.1.1` through that ISP.
* If the monitoring destination becomes unreachable, the recursive default route can become inactive.
* Another eligible default route can then take over.

# 🗁 Understanding Scope and Target Scope

RouterOS uses `scope` and `target-scope` to control route resolution.

These properties are particularly important in recursive routing.

**Scope**

The scope value determines which routes can be used to resolve a gateway.

A directly connected route commonly has a scope of `10`.

A manually configured host route used for recursive gateway resolution can also use scope `10`.

**Target Scope**

The target-scope value controls the scope of routes that can be used to resolve a route's gateway.

For a recursive default route, a common configuration is:

| Route | Scope | Target Scope |
| --- | ---: | ---: |
| Remote monitoring host route | 10 | Default |
| Recursive default route | Default | 11 |

The recursive default route can resolve its gateway through the host route because the host route's scope is within the allowed target-scope.

The relationship between these values must be correct for recursive resolution to work.

# 🗁 Example Network Design

The following example uses two WAN connections.

| Component | WAN-1 | WAN-2 |
| --- | --- | --- |
| Interface | `ether1` | `ether2` |
| ISP gateway | `192.0.2.1` | `198.51.100.1` |
| Monitoring IP | `1.1.1.1` | `8.8.8.8` |
| Route distance | 1 | 2 |
| Role | Primary | Backup |

These addresses belong to documentation ranges and are examples only.

Replace them with the actual addresses and gateways provided by the ISPs.

# 🗁 Configure WAN Interfaces

Assign the WAN addresses:

```routeros
/ip/address
add address=192.0.2.2/24 interface=ether1 comment="WAN-1"
add address=198.51.100.2/24 interface=ether2 comment="WAN-2"
```

The ISP gateways are:

```text
WAN-1 gateway: 192.0.2.1
WAN-2 gateway: 198.51.100.1
```

Verify the interface addresses:

```routeros
/ip/address/print
```

Confirm that both ISP gateways are reachable before configuring recursive routes.

# 🗁 Configure WAN-1 Recursive Routing

Create a host route for the WAN-1 monitoring address:

```routeros
/ip/route
add dst-address=1.1.1.1/32 \
    gateway=192.0.2.1 \
    scope=10 \
    comment="WAN-1 monitoring route"
```

Create the recursive default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=1.1.1.1 \
    target-scope=11 \
    check-gateway=ping \
    distance=1 \
    comment="Primary WAN-1 recursive default"
```

The default route uses `1.1.1.1` as its recursive gateway.

RouterOS resolves that address through the host route pointing to `192.0.2.1`.

# 🗁 Configure WAN-2 Recursive Routing

Create a host route for the WAN-2 monitoring address:

```routeros
/ip/route
add dst-address=8.8.8.8/32 \
    gateway=198.51.100.1 \
    scope=10 \
    comment="WAN-2 monitoring route"
```

Create the backup recursive default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=8.8.8.8 \
    target-scope=11 \
    check-gateway=ping \
    distance=2 \
    comment="Backup WAN-2 recursive default"
```

WAN-2 has a higher route distance, so it is preferred only when the primary default route is unavailable.

# 🗁 Understanding the Complete Configuration

The resulting routing configuration contains four important routes:

| Destination | Gateway | Purpose |
| --- | --- | --- |
| `1.1.1.1/32` | `192.0.2.1` | WAN-1 monitoring path |
| `0.0.0.0/0` | `1.1.1.1` | Primary default route |
| `8.8.8.8/32` | `198.51.100.1` | WAN-2 monitoring path |
| `0.0.0.0/0` | `8.8.8.8` | Backup default route |

The routing behavior is:

```text
                  Internet
                 /        \
                /          \
             ISP-1         ISP-2
                |             |
           192.0.2.1     198.51.100.1
                |             |
                |             |
             1.1.1.1       8.8.8.8
                |             |
                +------+------+
                       |
                 MikroTik Router
                       |
                  Default Route
                  Distance 1 / 2
```

WAN-1 is preferred because its default route has distance `1`.

WAN-2 is the backup because its default route has distance `2`.

If the WAN-1 monitoring destination becomes unreachable, RouterOS can deactivate the primary recursive default route and use WAN-2.

# 🗁 Understanding Failure Detection

Recursive gateway checking detects failures by monitoring a remote destination.

Consider these situations:

| Situation | WAN-1 gateway | WAN-1 monitoring IP | Expected result |
| --- | --- | --- | --- |
| Normal operation | Reachable | Reachable | WAN-1 active |
| ISP upstream failure | Reachable | Unreachable | WAN-1 default route inactive |
| Local link failure | Unreachable | Unreachable | WAN-1 default route inactive |
| Monitoring destination failure | Reachable | Unreachable | Possible false failover |

The last situation is important.

If the monitoring destination itself becomes unavailable while the ISP connection is healthy, the router may incorrectly switch to another WAN.

For production networks, use carefully selected monitoring destinations and consider independent probes.

# 🗁 Selecting Monitoring Addresses

Choose monitoring addresses that are:

* Reliably reachable through the corresponding ISP.
* Outside the local ISP gateway network.
* Stable and consistently available.
* Different for each WAN where possible.
* Suitable for ICMP reachability checks.

Public DNS resolver addresses are often used for connectivity monitoring.

Examples include:

| Provider | Monitoring address |
| --- | --- |
| Cloudflare DNS | `1.1.1.1` |
| Google DNS | `8.8.8.8` |
| Quad9 DNS | `9.9.9.9` |

Do not assume that a public monitoring address will always respond to ICMP. Verify reachability through each ISP before using it.

# 🗁 Preventing Monitoring Traffic from Using the Wrong WAN

Each monitoring address should have a host route through its intended ISP gateway.

For example:

```routeros
/ip/route
add dst-address=1.1.1.1/32 \
    gateway=192.0.2.1 \
    scope=10

add dst-address=8.8.8.8/32 \
    gateway=198.51.100.1 \
    scope=10
```

These host routes ensure that the monitoring destinations resolve through their respective WAN gateways.

Without correctly designed host routes, a monitoring address might resolve through another available default route, producing misleading health-check results.

# 🗁 Understanding Recursive Routing with More Than Two WAN Links

The same design can be extended to additional WAN connections.

Example:

| WAN | Monitoring IP | Route distance |
| --- | --- | ---: |
| WAN-1 | `1.1.1.1` | 1 |
| WAN-2 | `8.8.8.8` | 2 |
| WAN-3 | `9.9.9.9` | 3 |

Each WAN requires:

* A host route through its ISP gateway.
* A recursive default route.
* A suitable route distance.
* A reachable monitoring destination.

This creates a primary, secondary, and tertiary failover arrangement.

# 🗁 Understanding Recursive Routing and PCC

Recursive gateway checking can be combined with PCC load balancing.

In a PCC design:

* New connections are classified.
* Connections are assigned to WAN-specific routing paths.
* Each WAN has its own recursive reachability checks.
* Unavailable WAN paths must not continue receiving new connections.

However, configuring recursive default routes alone does not automatically remove failed WANs from every PCC routing table.

Each routing table and connection-marking policy must have appropriate route availability and failover behavior.

The detailed implementation is covered in the PCC and multi-WAN routing documents.

# 🗁 Verification Commands

Inspect the routing table:

```routeros
/ip/route/print detail
```

Display active routes:

```routeros
/ip/route/print where active
```

Inspect route status:

```routeros
/routing/route/print detail
```

Ping the WAN-1 monitoring destination:

```routeros
/ping 1.1.1.1
```

Ping the WAN-2 monitoring destination:

```routeros
/ping 8.8.8.8
```

Test the ISP gateways:

```routeros
/ping 192.0.2.1
/ping 198.51.100.1
```

Monitor WAN interfaces:

```routeros
/interface/monitor-traffic ether1
/interface/monitor-traffic ether2
```

# 🗁 Testing Failover

Perform the following tests in a controlled lab environment.

**Normal operation**

* Confirm that both WAN gateways are reachable.
* Confirm that both monitoring destinations respond.
* Verify that WAN-1 is the active default route.
* Verify that WAN-2 remains available as the backup.

**WAN-1 upstream failure**

* Keep the physical WAN-1 link connected.
* Simulate an upstream failure beyond the ISP gateway.
* Confirm that the WAN-1 monitoring destination becomes unreachable.
* Verify that the WAN-1 recursive default route becomes inactive.
* Confirm that WAN-2 becomes the active default route.
* Test new client connections.

**WAN-1 recovery**

* Restore upstream connectivity.
* Confirm that the WAN-1 monitoring destination responds again.
* Verify that the primary default route becomes active.
* Confirm that the router returns to WAN-1 according to route preference.

# 🗁 Troubleshooting Recursive Gateway Checks

**Recursive default route is inactive**

Check:

* Whether the monitoring host route exists.
* Whether the ISP gateway is reachable.
* Whether the monitoring destination responds.
* Whether the host route has an appropriate scope.
* Whether the recursive default route has a suitable target-scope.

Useful commands:

```routeros
/ip/route/print detail
/ping 1.1.1.1
```

**Monitoring traffic leaves through the wrong WAN**

Inspect the host routes and confirm that each monitoring destination resolves through the intended ISP gateway.

```routeros
/ip/route/print detail
```

**WAN-1 does not fail over**

Check whether the recursive default route is actually becoming inactive.

If the monitoring destination remains reachable through the failed ISP path, investigate routing resolution and the monitoring target.

**WAN-1 does not recover**

Check:

* Physical interface status.
* ISP gateway reachability.
* Monitoring destination reachability.
* Host route status.
* Recursive route status.
* Route distance and routing table.

**Intermittent failover**

Investigate packet loss, unstable upstream connectivity, monitoring destination reliability, and the gateway-check behavior.

# 🗁 Best Practices

* Use a remote monitoring address beyond the ISP gateway.
* Configure a dedicated host route for each WAN monitoring destination.
* Use different monitoring destinations for independent WAN paths.
* Configure route scope and target-scope correctly.
* Use appropriate route distances for primary and backup paths.
* Verify that monitoring traffic follows the intended WAN.
* Test upstream failures without disconnecting the physical interface.
* Avoid relying on a single potentially unreliable monitoring destination in critical networks.
* Check PCC and policy-routing tables separately when combining recursive routing with load balancing.
* Document each WAN gateway, monitoring destination, and route relationship.
