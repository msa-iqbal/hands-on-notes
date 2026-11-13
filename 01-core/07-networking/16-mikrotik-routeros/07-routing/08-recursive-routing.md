# Recursive Routing in MikroTik RouterOS

Recursive routing is a routing technique in which a route's gateway is resolved through another route instead of being directly connected to an interface or next-hop router.

In MikroTik RouterOS, recursive routing is commonly used for:

* Monitoring WAN connectivity beyond the ISP gateway.
* Detecting upstream connectivity failures.
* Implementing reliable multi-WAN failover.
* Creating more intelligent default-route selection.
* Checking whether an external destination is reachable before using a WAN connection.

A normal gateway check may confirm that the ISP gateway is reachable while the ISP's upstream network or Internet connection is unavailable.

Recursive routing addresses this limitation by monitoring a remote IP address through the intended WAN connection.

# 🗁 How Recursive Routing Works

Consider this routing arrangement:

```text
Router
  |
  +---- WAN1 ---- ISP Gateway 1 ---- Internet
  |
  +---- WAN2 ---- ISP Gateway 2 ---- Internet
```

Instead of using the ISP gateway directly as the default route's gateway, the router uses a remote monitoring IP address.

For example:

* WAN1 monitors `1.1.1.1`.
* WAN2 monitors `8.8.8.8`.

The router first resolves each monitoring IP through its corresponding ISP gateway. It then uses those monitoring IP addresses as recursive gateways for the default routes.

```text
Default Route
     |
     +---- Gateway: 1.1.1.1
                  |
                  +---- Host Route: 1.1.1.1/32
                                   |
                                   +---- ISP Gateway 1
```

If the monitoring destination becomes unreachable, the corresponding recursive default route can become inactive. Another eligible default route can then take over.

# 🗁 Understanding Route Scope and Target Scope

RouterOS uses `scope` and `target-scope` to control how route gateways are resolved.

**Scope**

The `scope` property defines the scope of a route. It helps determine whether that route can be used to resolve another route's gateway.

**Target Scope**

The `target-scope` property controls the maximum scope of routes that can be used to resolve a route's gateway.

In a common recursive routing configuration:

* The monitoring host route uses `scope=10`.
* The recursive default route uses `target-scope=11`.

This allows the default route to resolve its monitoring IP through the host route.

Example:

```routeros
/ip/route
add dst-address=1.1.1.1/32 gateway=192.0.2.1 scope=10

add dst-address=0.0.0.0/0 gateway=1.1.1.1 \
    target-scope=11 check-gateway=ping
```

Here:

* `1.1.1.1/32` is the monitoring host route.
* `192.0.2.1` is the ISP gateway.
* The default route uses `1.1.1.1` as its recursive gateway.
* `check-gateway=ping` enables reachability checking.

The example uses documentation IP addresses. Replace them with your actual WAN gateway and monitoring destination.

# 🗁 Single-WAN Recursive Routing

A single-WAN recursive route can monitor an external destination instead of checking only the ISP gateway.

Assume the following network:

| Parameter | Value |
| --- | --- |
| WAN interface | `ether1` |
| ISP gateway | `192.0.2.1` |
| Monitoring IP | `1.1.1.1` |

Add a host route to the monitoring destination:

```routeros
/ip/route
add dst-address=1.1.1.1/32 \
    gateway=192.0.2.1 \
    scope=10 \
    comment="WAN1 monitoring route"
```

Add the recursive default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=1.1.1.1 \
    target-scope=11 \
    check-gateway=ping \
    distance=1 \
    comment="Recursive WAN1 default route"
```

The router resolves `1.1.1.1` through the host route, which points to the ISP gateway.

When the monitoring destination cannot be reached, the recursive route can become inactive.

**Important:** This configuration monitors reachability to the selected destination. It does not guarantee that every Internet service or destination is reachable.

# 🗁 Dual-WAN Recursive Routing

Recursive routing is particularly useful when a router has two Internet connections.

Assume the following topology:

| Parameter | WAN1 | WAN2 |
| --- | --- | --- |
| Interface | `ether1` | `ether2` |
| ISP gateway | `192.0.2.1` | `198.51.100.1` |
| Monitoring IP | `1.1.1.1` | `8.8.8.8` |
| Default route distance | `1` | `2` |

WAN1 is the primary Internet connection, while WAN2 is the backup connection.

**Configure WAN1 monitoring route:**

```routeros
/ip/route
add dst-address=1.1.1.1/32 \
    gateway=192.0.2.1 \
    scope=10 \
    comment="WAN1 monitoring route"
```

**Configure WAN2 monitoring route:**

```routeros
/ip/route
add dst-address=8.8.8.8/32 \
    gateway=198.51.100.1 \
    scope=10 \
    comment="WAN2 monitoring route"
```

**Configure the primary default route:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=1.1.1.1 \
    target-scope=11 \
    check-gateway=ping \
    distance=1 \
    comment="Primary WAN1 recursive route"
```

**Configure the backup default route:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=8.8.8.8 \
    target-scope=11 \
    check-gateway=ping \
    distance=2 \
    comment="Backup WAN2 recursive route"
```

**Expected behavior:**

| WAN1 status | WAN2 status | Active default route |
| --- | --- | --- |
| Reachable | Reachable | WAN1 |
| Unreachable | Reachable | WAN2 |
| Reachable | Unreachable | WAN1 |
| Unreachable | Unreachable | No active recursive default route |

The lower route distance makes WAN1 preferred when both routes are eligible.

When WAN1 becomes unavailable, RouterOS can select WAN2. When WAN1 becomes reachable again, it can become the preferred route.

# 🗁 Recursive Routing with Multiple Monitoring Targets

Monitoring a single external IP address has a limitation: the monitoring destination itself might become unavailable even when the WAN connection is healthy.

For more reliable monitoring, you can use multiple monitoring destinations.

For example:

* `1.1.1.1`
* `8.8.8.8`

Each monitoring destination should have a route that resolves through the intended WAN.

However, adding multiple recursive routes requires careful route design. A monitoring target must not accidentally resolve through another WAN or through the default route being monitored.

A robust design should ensure:

* Each monitoring target is pinned to its intended WAN.
* Monitoring destinations are independently reachable.
* Recursive gateway resolution does not depend on the route being tested.
* Route distances and scopes are configured consistently.

Multiple probes improve monitoring resilience, but they do not automatically create an AND/OR health-check policy. That requires additional routing or monitoring logic.

# 🗁 Recursive Routing with PPPoE

When a WAN connection uses PPPoE, the next hop is commonly represented by the PPPoE interface rather than a fixed ISP gateway address.

Example:

```routeros
/interface/pppoe-client
add name=pppoe-out1 \
    interface=ether1 \
    user="your-username" \
    password="your-password" \
    add-default-route=no \
    disabled=no
```

Create the monitoring host route through the PPPoE interface:

```routeros
/ip/route
add dst-address=1.1.1.1/32 \
    gateway=pppoe-out1 \
    scope=10 \
    comment="PPPoE monitoring route"
```

Create the recursive default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=1.1.1.1 \
    target-scope=11 \
    check-gateway=ping \
    distance=1 \
    comment="PPPoE recursive default route"
```

Using `add-default-route=no` prevents the PPPoE client from automatically adding another default route.

This example assumes that the PPPoE connection is established and the interface is available. The monitoring route will not work while the PPPoE session is disconnected.

# 🗁 Verifying Recursive Routes

After configuring recursive routing, inspect the routing table.

```routeros
/ip/route/print detail
```

You can also inspect the active routing information:

```routeros
/routing/route/print detail
```

Check the following:

* The monitoring host route exists.
* The host route resolves through the intended ISP gateway or WAN interface.
* The recursive default route resolves through the monitoring IP.
* The route has the expected distance and scope properties.
* The route is active when its gateway is reachable.

Test the monitoring destinations:

```routeros
/ping 1.1.1.1 count=5
/ping 8.8.8.8 count=5
```

Inspect the route status after testing:

```routeros
/ip/route/print detail where dst-address=0.0.0.0/0
```

The exact route flags and status fields can vary with RouterOS versions and route state.

# 🗁 Troubleshooting Recursive Routing

**Recursive default route is inactive**

Possible causes:

* The monitoring host route does not exist.
* The ISP gateway is unreachable.
* The monitoring destination cannot be reached.
* The `target-scope` does not permit the required gateway resolution.
* The monitoring route resolves through an unintended path.

Check:

```routeros
/ip/route/print detail
/ping 192.0.2.1 count=5
/ping 1.1.1.1 count=5
```

**Monitoring traffic uses the wrong WAN**

Possible causes:

* The monitoring host route is missing.
* Another route provides a more specific or preferred path.
* The configured gateway is incorrect.
* The monitoring destination is being resolved through another routing table.

Inspect the selected route and verify that each monitoring destination is pinned to its intended WAN.

**WAN failover does not work**

Possible causes:

* The backup default route is not eligible.
* The backup monitoring route is unreachable.
* The route distances are incorrect.
* The primary route is still considered reachable.
* Firewall rules or routing policies interfere with monitoring traffic.

Inspect both recursive routes and test each monitoring destination independently.

**WAN failback does not work**

Possible causes:

* The primary monitoring destination remains unreachable.
* The primary host route is incorrect.
* The primary default route is not eligible.
* The primary route has a less preferred distance than another route.

Verify the primary monitoring path before changing route distances.

# 🗁 Best Practices

* Use reliable monitoring destinations.
* Use different monitoring targets for separate WAN connections.
* Pin each monitoring destination to its intended WAN.
* Configure route scope and target scope deliberately.
* Use route distances to define primary and backup preferences.
* Test failover and failback before deploying the configuration.
* Monitor routing status after WAN changes.
* Avoid relying on a single probe when the availability of that probe is critical.
* Consider source NAT and return-path consistency when implementing multi-WAN routing.
* Remember that recursive routing checks the selected monitoring destination, not the complete health of every Internet destination.

# 🗁 Summary

Recursive routing allows RouterOS to resolve a route's gateway through another route. It is especially useful for WAN monitoring and multi-WAN failover.

The main components are:

* **Monitoring host route:** Provides a specific path to the remote monitoring destination.
* **Recursive default route:** Uses the monitoring destination as its gateway.
* **Scope:** Defines the route's scope for gateway resolution.
* **Target scope:** Controls which routes can resolve the recursive gateway.
* **Gateway checking:** Monitors reachability using mechanisms such as ICMP ping.
* **Distance:** Determines preference between otherwise eligible routes.

By combining these features, MikroTik RouterOS can detect certain upstream connectivity failures and select an alternative WAN connection automatically.
