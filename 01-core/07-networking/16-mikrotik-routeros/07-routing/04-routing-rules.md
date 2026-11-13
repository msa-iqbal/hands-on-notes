# Routing Rules

Routing rules in MikroTik RouterOS v7 allow administrators to control which routing table the router uses when looking up a route for a packet.

By default, RouterOS performs route lookup in the `main` routing table. However, different traffic may need different routing paths.

For example:

* Traffic from one LAN subnet should use WAN1.
* Traffic from another LAN subnet should use WAN2.
* Traffic destined for a particular network should use a dedicated routing table.
* Specific traffic should use the main routing table as a fallback.

Routing rules provide a way to implement this behavior based on packet attributes such as source address, destination address, and incoming interface.

# 🗁 Routing Rules vs. Routing Tables

These two features work together but serve different purposes.

**Routing tables:**

* Store routes and their next-hop information.
* Determine where packets can be forwarded.
* Can contain default routes, static routes, and dynamic routes.
* Include the built-in `main` table.

**Routing rules:**

* Determine which routing table should be consulted.
* Match traffic using configured conditions.
* Can direct matching traffic to a custom routing table.
* Control routing-table lookup without requiring packet marks.

Example:

```text
LAN-A traffic ──> Routing Rule ──> WAN1 Table ──> WAN1
LAN-B traffic ──> Routing Rule ──> WAN2 Table ──> WAN2
```

# 🗁 Main Routing Table

The `main` routing table is the default routing table in RouterOS.

Display its routes:

```routeros
/ip/route/print where routing-table=main
```

You can also inspect routing tables:

```routeros
/routing/table/print
```

If no policy-routing configuration changes the lookup process, packets normally use the `main` routing table.

# 🗁 Creating Custom Routing Tables

Before creating routing rules for custom tables, create the required routing tables.

In RouterOS v7, custom routing tables used for forwarding need the `fib` property.

Create a table for WAN1:

```routeros
/routing/table/add name=to_WAN1 fib
```

Create a table for WAN2:

```routeros
/routing/table/add name=to_WAN2 fib
```

Verify the tables:

```routeros
/routing/table/print
```

The `fib` property enables the table to participate in forwarding lookups.

# 🗁 Adding Routes to Custom Tables

A routing rule is useful only when the selected routing table contains suitable routes.

Assume this network:

| Network | Interface | Gateway |
| --- | --- | --- |
| WAN1 | ether1 | `192.168.1.1` |
| WAN2 | ether2 | `192.168.2.1` |
| LAN | bridge-LAN | `192.168.10.0/24` |

Add a default route to the WAN1 table:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.1.1 \
    routing-table=to_WAN1 \
    distance=1 \
    comment="WAN1 default route"
```

Add a default route to the WAN2 table:

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.2.1 \
    routing-table=to_WAN2 \
    distance=1 \
    comment="WAN2 default route"
```

The gateways must be resolvable in the routing context. If necessary, configure gateway resolution appropriately, for example by referencing the main routing table with `@main`.

Inspect the routes:

```routeros
/ip/route/print detail
```

# 🗁 Routing Rule Properties

Routing rules support several matching properties.

| Property | Purpose |
| --- | --- |
| `src-address` | Matches the packet's source IP address or prefix |
| `dst-address` | Matches the packet's destination IP address or prefix |
| `interface` | Matches the incoming interface |
| `action` | Defines what happens when the rule matches |
| `table` | Specifies the routing table for a lookup action |
| `min-prefix` | Specifies the minimum prefix length considered for lookup |
| `disabled` | Enables or disables a rule |

The exact combination of properties depends on the routing design.

# 🗁 Understanding Routing Rule Actions

RouterOS routing rules support different actions.

**lookup**

Attempts to find a route in the specified routing table. If a suitable route is not found, lookup processing can continue with subsequent rules or the normal lookup process.

**lookup-only-in-table**

Performs route lookup only in the specified table. If no suitable route is found, it does not fall back to another routing table.

**drop**

Drops matching packets instead of forwarding them.

**unreachable**

Returns an unreachable result for matching traffic.

**prohibit**

Rejects matching traffic with a prohibited result.

For most policy-routing configurations, `lookup` and `lookup-only-in-table` are the most commonly used actions.

# 🗁 Routing Traffic by Source Address

One common use case is sending traffic from a particular LAN subnet through a specific WAN connection.

Assume:

* LAN subnet: `192.168.10.0/24`
* WAN1 routing table: `to_WAN1`
* WAN2 routing table: `to_WAN2`

Create a rule for WAN1:

```routeros
/routing/rule/add \
    src-address=192.168.10.0/24 \
    action=lookup-only-in-table \
    table=to_WAN1 \
    comment="LAN subnet through WAN1"
```

Traffic originating from `192.168.10.0/24` will use the `to_WAN1` table for routing lookup.

If the table has no suitable route, the packet will not fall back to the main table.

For a fallback-capable rule, use:

```routeros
/routing/rule/add \
    src-address=192.168.10.0/24 \
    action=lookup \
    table=to_WAN1 \
    comment="LAN subnet prefer WAN1"
```

With `lookup`, RouterOS can continue with subsequent lookup processing if the selected table does not provide a suitable route.

# 🗁 Routing Traffic by Destination Address

Routing rules can also match destination addresses.

For example, traffic destined for a specific remote network can be directed to a dedicated routing table.

Create a rule:

```routeros
/routing/rule/add \
    dst-address=10.20.0.0/16 \
    action=lookup-only-in-table \
    table=to_WAN2 \
    comment="Remote network through WAN2"
```

This rule directs matching destination traffic to the `to_WAN2` routing table.

Make sure the table contains an appropriate route to the destination network.

# 🗁 Routing Traffic by Incoming Interface

Routing rules can also match the interface on which a packet arrives.

Example:

```routeros
/routing/rule/add \
    interface=bridge-LAN \
    action=lookup-only-in-table \
    table=to_WAN1 \
    comment="LAN interface through WAN1"
```

This can be useful when different LAN interfaces or VLAN interfaces need different routing policies.

For example:

```text
VLAN 10 ──> Routing Rule ──> to_WAN1
VLAN 20 ──> Routing Rule ──> to_WAN2
```

Use the actual incoming Layer 3 interface in the rule. For VLAN-based networks, this is commonly the relevant VLAN interface rather than a physical switch port.

# 🗁 Rule Order and Matching

Routing rules are evaluated according to their order.

When multiple rules can match a packet, rule order can affect the selected routing table.

Display the configured rules:

```routeros
/routing/rule/print
```

Consider the following example:

```routeros
/routing/rule/add \
    src-address=192.168.10.0/24 \
    action=lookup-only-in-table \
    table=to_WAN1 \
    comment="LAN-A through WAN1"

/routing/rule/add \
    src-address=192.168.20.0/24 \
    action=lookup-only-in-table \
    table=to_WAN2 \
    comment="LAN-B through WAN2"
```

Each rule matches a different source subnet.

When building more complex policies, place specific matching rules before broader rules where their conditions could overlap.

# 🗁 Routing Rules and the Main Table

The `main` table is important because it commonly contains connected routes and the router's ordinary default route.

A routing rule can explicitly direct matching traffic to the main table.

Example:

```routeros
/routing/rule/add \
    src-address=192.168.30.0/24 \
    action=lookup-only-in-table \
    table=main \
    comment="Use main routing table"
```

This can be useful when custom routing policies exist but selected traffic must continue using the normal routing table.

# 🗁 Routing Rules vs. Mangle-Based Policy Routing

RouterOS supports policy routing through both routing rules and firewall mangle rules.

They serve related purposes but provide different matching and classification capabilities.

| Feature | Routing Rules | Mangle |
| --- | --- | --- |
| Source address matching | Yes | Yes |
| Destination address matching | Yes | Yes |
| Incoming interface matching | Yes | Yes |
| Connection marking | No | Yes |
| Packet marking | No | Yes |
| Routing-mark support | No | Yes |
| Complex traffic classification | Limited | More flexible |
| Typical use | Straightforward policy routing | Advanced policy routing |

Use routing rules when the policy can be expressed through their supported matching properties.

Use mangle when you need connection marks, packet marks, or more advanced traffic classification.

Avoid applying overlapping routing policies without understanding their interaction.

# 🗁 Important Considerations

**Router-originated traffic**

Traffic generated by the router itself does not enter through a LAN interface in the same way as forwarded traffic.

An interface-based routing rule intended for incoming LAN packets should not be assumed to control router-originated connections.

For router-originated traffic, consider the available source-address and destination-address matching conditions or other suitable routing mechanisms.

**Return traffic**

Policy routing must account for both directions of a connection.

If outbound traffic leaves through WAN2 but the return path uses WAN1, the connection may fail because of asymmetric routing, upstream filtering, or connection-tracking behavior.

**NAT configuration**

When using multiple WAN connections, source NAT must match the actual egress interface or address.

Example:

```routeros
/ip/firewall/nat/add \
    chain=srcnat \
    out-interface=ether1 \
    action=masquerade \
    comment="NAT through WAN1"

/ip/firewall/nat/add \
    chain=srcnat \
    out-interface=ether2 \
    action=masquerade \
    comment="NAT through WAN2"
```

**Connected routes**

Custom routing tables must have a valid path to their gateways.

A default route referencing a gateway that cannot be resolved will not provide a usable forwarding path.

**RouterOS version**

This documentation uses RouterOS v7 routing-table syntax. RouterOS v6 policy-routing configurations use different mechanisms and should not be copied directly into v7.

# 🗁 Troubleshooting Routing Rules

Display routing rules:

```routeros
/routing/rule/print detail
```

Display routing tables:

```routeros
/routing/table/print
```

Display routes:

```routeros
/ip/route/print detail
```

Inspect route lookup results:

```routeros
/routing/route/print detail
```

Inspect firewall mangle rules:

```routeros
/ip/firewall/mangle/print
```

Inspect NAT rules:

```routeros
/ip/firewall/nat/print
```

Common problems include:

* The custom routing table does not exist.
* The custom table was created without `fib`.
* No suitable route exists in the selected table.
* The gateway cannot be resolved.
* A previous routing rule matches the traffic first.
* Mangle rules apply a routing mark that changes the lookup behavior.
* Source NAT is missing or incorrect.
* Return traffic follows a different path.

# 🗁 Practical Lab: Route Different LAN Subnets Through Different WANs

**Objective:** Configure two LAN subnets and direct each subnet through a different WAN connection.

**Topology:**

```text
                       Internet
                          |
                 +----------------+
                 |    MikroTik    |
                 +----------------+
                    |          |
                   WAN1       WAN2
                    |          |
              192.168.1.1 192.168.2.1
                    |
             +------+------+
             |             |
           LAN-A         LAN-B
        192.168.10.0  192.168.20.0
             |             |
           WAN1          WAN2
```

Assume the WAN interfaces and gateway addresses are already configured and reachable.

**Step 1: Create routing tables**

```routeros
/routing/table/add name=to_WAN1 fib
/routing/table/add name=to_WAN2 fib
```

**Step 2: Add default routes**

```routeros
/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.1.1 \
    routing-table=to_WAN1 \
    comment="LAN-A default route"

/ip/route/add \
    dst-address=0.0.0.0/0 \
    gateway=192.168.2.1 \
    routing-table=to_WAN2 \
    comment="LAN-B default route"
```

**Step 3: Add routing rules**

```routeros
/routing/rule/add \
    src-address=192.168.10.0/24 \
    action=lookup-only-in-table \
    table=to_WAN1 \
    comment="LAN-A through WAN1"

/routing/rule/add \
    src-address=192.168.20.0/24 \
    action=lookup-only-in-table \
    table=to_WAN2 \
    comment="LAN-B through WAN2"
```

**Step 4: Configure source NAT**

```routeros
/ip/firewall/nat/add \
    chain=srcnat \
    out-interface=ether1 \
    action=masquerade \
    comment="NAT WAN1"

/ip/firewall/nat/add \
    chain=srcnat \
    out-interface=ether2 \
    action=masquerade \
    comment="NAT WAN2"
```

**Step 5: Verify the configuration**

```routeros
/routing/table/print
/routing/rule/print detail
/ip/route/print detail
/ip/firewall/nat/print
```

**Step 6: Test traffic**

From a client in LAN-A, check the public IP address. Repeat from a client in LAN-B.

Verify that:

* LAN-A traffic exits through WAN1.
* LAN-B traffic exits through WAN2.
* Both routing tables contain valid default routes.
* NAT is applied through the correct WAN interface.
* Return traffic follows a valid path.

**Lab notes:**

This lab demonstrates basic source-based policy routing. A production configuration should also account for router-originated traffic, WAN failure detection, gateway reachability, firewall policies, and return-path consistency.

# 🗁 Summary

Routing rules control which routing table RouterOS uses for matching traffic.

Key points:

* Routing tables store routes; routing rules select lookup tables.
* RouterOS v7 custom forwarding tables should be created with `fib`.
* Rules can match source addresses, destination addresses, and incoming interfaces.
* `lookup` allows subsequent lookup processing when no suitable route is found in the selected table.
* `lookup-only-in-table` restricts lookup to the selected table.
* Rule order matters when matching conditions overlap.
* NAT and return-path behavior must be considered in multi-WAN configurations.
* Mangle provides additional classification capabilities for advanced policy routing.

Routing rules are a foundation for multi-WAN routing, VLAN-based internet policies, and more advanced network designs.
