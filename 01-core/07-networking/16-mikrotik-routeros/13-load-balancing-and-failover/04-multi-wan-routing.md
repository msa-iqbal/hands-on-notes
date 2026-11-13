# Multi-WAN Routing

Multi-WAN routing allows a MikroTik router to use multiple internet connections simultaneously or selectively.

A router with multiple WAN connections can distribute traffic across providers, route selected networks through specific connections, maintain backup internet connectivity, and control how traffic enters and leaves the network.

RouterOS v7 provides routing tables, routing rules, firewall Mangle, connection tracking, and route selection mechanisms for implementing advanced Multi-WAN designs.

This document focuses on configuring and managing multiple WAN routing paths in RouterOS v7.

# 🗁 Understanding Multi-WAN Routing

Multi-WAN means connecting one router to two or more upstream networks.

A typical deployment may use:

| Interface | WAN connection | Gateway |
| --- | --- | --- |
| ether1 | ISP-A | 192.0.2.1 |
| ether2 | ISP-B | 198.51.100.1 |
| ether3 | ISP-C | 203.0.113.1 |
| bridge-LAN | Internal network | 192.168.10.0/24 |

The router can use these connections in different ways.

### 🖆 Load Distribution

Distributes outgoing connections across multiple WAN links.

For example:

* Some connections use ISP-A.
* Other connections use ISP-B.
* Additional connections use ISP-C.

### 🖆 Policy-Based Routing

Selects a WAN connection based on a policy.

Examples:

* Office computers use ISP-A.
* Guest Wi-Fi uses ISP-B.
* Servers use ISP-C.
* Specific destinations use a designated provider.

### 🖆 Failover

Uses another WAN connection when the preferred connection becomes unavailable.

### 🖆 Application-Specific Routing

Routes selected traffic through a particular WAN based on source address, destination address, protocol, ports, or other packet properties.

# 🗁 Routing Tables in RouterOS v7

RouterOS v7 supports multiple routing tables.

The `main` table is the default routing table. Additional tables can be created for independent routing decisions.

For example:

| Routing table | Purpose |
| --- | --- |
| main | Default router traffic and general routing |
| to_isp_a | Traffic routed through ISP-A |
| to_isp_b | Traffic routed through ISP-B |
| to_isp_c | Traffic routed through ISP-C |

### 🖆 Creating Routing Tables

Create custom routing tables using `/routing/table`.

```routeros
/routing/table
add name=to_isp_a fib
add name=to_isp_b fib
add name=to_isp_c fib
```

The `fib` property enables the table for forwarding lookups.

### 🖆 Viewing Routing Tables

```routeros
/routing/table/print
```

### 🖆 Understanding Route Lookup

A routing decision can use:

* The `main` routing table.
* A custom routing table selected by a routing mark.
* A routing rule that directs lookup to a specific table.
* Policy-based routing configured through firewall Mangle.

A custom routing table must have usable routes for the destinations it is expected to handle.

# 🗁 Creating WAN-Specific Routing Tables

Each custom table needs routes that point to its corresponding WAN gateway.

Assume the following topology:

| WAN | Router IP | Gateway |
| --- | --- | --- |
| ISP-A | 192.0.2.2/24 | 192.0.2.1 |
| ISP-B | 198.51.100.2/24 | 198.51.100.1 |

The WAN interfaces are:

* `ether1` for ISP-A.
* `ether2` for ISP-B.

### 🖆 Configuring WAN Addresses

```routeros
/ip/address
add address=192.0.2.2/24 interface=ether1 comment="ISP-A"
add address=198.51.100.2/24 interface=ether2 comment="ISP-B"
```

### 🖆 Creating Routing Tables

```routeros
/routing/table
add name=to_isp_a fib
add name=to_isp_b fib
```

### 🖆 Adding WAN-Specific Default Routes

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1@main routing-table=to_isp_a distance=1
add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to_isp_b distance=1
```

The `@main` suffix specifies that the gateway should be resolved using the main routing table.

This is useful when the custom table needs to use a gateway whose directly connected route exists in `main`.

### 🖆 Adding Routes to the Main Table

The main table also needs default routes if the router itself or unmarked traffic must use the WAN connections.

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1 distance=1
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2
```

With these distances, ISP-A is preferred and ISP-B is a backup for ordinary main-table traffic.

These routes do not automatically define the failover behavior of every custom routing table. Each custom table needs its own appropriate primary and backup routes if failover is required there.

# 🗁 Routing Traffic Through a Specific WAN

A routing rule can direct traffic from a particular source network into a selected routing table.

For example:

* `192.168.10.0/24` uses ISP-A.
* `192.168.20.0/24` uses ISP-B.

### 🖆 Creating Routing Rules

```routeros
/routing/rule
add src-address=192.168.10.0/24 action=lookup-only-in-table table=to_isp_a
add src-address=192.168.20.0/24 action=lookup-only-in-table table=to_isp_b
```

### 🖆 Understanding Routing Rule Actions

| Action | Behavior |
| --- | --- |
| lookup | Checks the selected table and can continue with subsequent lookup behavior when no route is found |
| lookup-only-in-table | Requires route lookup in the selected table; it does not fall back to another table |
| drop | Discards matching traffic |
| unreachable | Returns an unreachable result for matching traffic |

Use `lookup-only-in-table` when traffic must not escape through another WAN if its designated table has no usable route.

Use `lookup` when fallback to other routing lookup behavior is intentional.

### 🖆 Important Consideration

Routing rules are useful for straightforward source-based routing.

For more advanced policies involving connection marks, PCC, inbound connection symmetry, or protocol-specific classification, firewall Mangle provides greater control.

# 🗁 Policy-Based Routing

Policy-Based Routing (PBR) selects a routing path according to defined policies rather than relying only on the destination address.

RouterOS supports PBR through:

* Routing rules.
* Firewall Mangle routing marks.
* Multiple routing tables.

Mangle is particularly useful when routing decisions depend on connection state or packet classification.

### 🖆 Example: Route a LAN Subnet Through ISP-B

Assume:

* LAN: `192.168.10.0/24`
* ISP-A: `ether1`
* ISP-B: `ether2`
* Routing table: `to_isp_b`

Create an address list for internal destinations.

```routeros
/ip/firewall/address-list
add list=local-networks address=192.168.10.0/24
add list=local-networks address=192.168.20.0/24
```

Add a Mangle rule to mark connections originating from the selected LAN subnet.

```routeros
/ip/firewall/mangle
add chain=prerouting src-address=192.168.10.0/24 dst-address-list=!local-networks connection-mark=no-mark action=mark-connection new-connection-mark=isp_b_connection passthrough=yes
```

Mark packets belonging to those connections for routing.

```routeros
/ip/firewall/mangle
add chain=prerouting connection-mark=isp_b_connection action=mark-routing new-routing-mark=to_isp_b passthrough=no
```

The routing table must already contain a usable route through ISP-B.

### 🖆 Why Internal Destinations Must Be Excluded

Without excluding internal networks, traffic between LAN subnets may receive a WAN routing mark.

For example, traffic from `192.168.10.0/24` to `192.168.20.0/24` should normally be routed internally, not sent toward an internet provider.

The address list should include all internal networks that must remain reachable through internal routing.

### 🖆 Why Connection Marks Matter

A connection mark classifies a connection so that related packets can receive consistent routing treatment.

This is particularly important when implementing:

* PCC load balancing.
* Multiple WAN connections.
* Inbound connections.
* Symmetric routing.
* Protocol-specific routing policies.

# 🗁 Handling Router-Originated Traffic

Traffic generated by the MikroTik router itself does not enter through the LAN `prerouting` chain.

Examples include:

* DNS queries generated by the router.
* NTP synchronization.
* RouterOS package downloads.
* Router monitoring traffic.
* Ping and traceroute initiated from the router.

Router-originated traffic passes through the `output` Mangle chain.

### 🖆 Marking Router-Originated Connections

For example, mark router-originated connections that should use ISP-B.

```routeros
/ip/firewall/mangle
add chain=output connection-mark=no-mark action=mark-connection new-connection-mark=router_isp_b passthrough=yes
```

Apply the routing mark.

```routeros
/ip/firewall/mangle
add chain=output connection-mark=router_isp_b action=mark-routing new-routing-mark=to_isp_b passthrough=no
```

This example marks all eligible router-originated connections for ISP-B. In a real deployment, use more specific conditions when only selected router services should use that WAN.

### 🖆 Testing Router-Originated Traffic

```routeros
/ping 1.1.1.1 routing-table=to_isp_b
```

This tests reachability using the selected routing table.

# 🗁 Inbound Connection Routing

Inbound connections arrive through a particular WAN interface.

For example:

* ISP-A receives a connection to a public web server.
* ISP-B receives a connection to a VPN service.

The response traffic should normally leave through the same WAN connection that received the original connection.

This is known as symmetric routing.

### 🖆 Why Symmetric Routing Matters

Consider this connection:

```text
Internet Client
      |
      v
   ISP-A
      |
      v
 MikroTik Router
      |
      v
 Internal Server
```

If the server's response leaves through ISP-B, the remote client may reject the response or the connection may fail.

This can happen because:

* The source public IP changes.
* The upstream provider expects traffic from its own assigned address.
* Stateful firewalls track the original connection.
* NAT mappings are associated with a particular WAN path.

### 🖆 Marking Connections Received Through ISP-A

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=ether1 connection-state=new connection-mark=no-mark action=mark-connection new-connection-mark=isp_a_in passthrough=yes
```

### 🖆 Marking Connections Received Through ISP-B

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=ether2 connection-state=new connection-mark=no-mark action=mark-connection new-connection-mark=isp_b_in passthrough=yes
```

### 🖆 Applying Routing Marks to Return Traffic

```routeros
/ip/firewall/mangle
add chain=prerouting connection-mark=isp_a_in action=mark-routing new-routing-mark=to_isp_a passthrough=no
add chain=prerouting connection-mark=isp_b_in action=mark-routing new-routing-mark=to_isp_b passthrough=no
```

These rules are a basic illustration of inbound connection marking. In a complete policy, place them carefully relative to other Mangle rules and ensure that only the intended forwarded traffic receives these routing marks.

Traffic destined for the router itself requires appropriate `input` firewall handling. Do not treat router-local traffic as ordinary forwarded traffic.

# 🗁 Multi-WAN with PCC

Per Connection Classifier (PCC) distributes connections across multiple WAN links.

PCC classifies connections using selected address and port combinations.

For example, with two WAN links:

* ISP-A handles one group of connections.
* ISP-B handles another group of connections.

PCC is useful when both WAN links should carry outgoing traffic.

### 🖆 PCC Classification Example

For two equally weighted WAN connections, the classifier can use a divisor of `2`.

```routeros
/ip/firewall/mangle
add chain=prerouting src-address=192.168.10.0/24 dst-address-list=!local-networks connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:2/0 action=mark-connection new-connection-mark=isp_a_connection passthrough=yes

add chain=prerouting src-address=192.168.10.0/24 dst-address-list=!local-networks connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:2/1 action=mark-connection new-connection-mark=isp_b_connection passthrough=yes
```

### 🖆 Applying Routing Marks

```routeros
/ip/firewall/mangle
add chain=prerouting connection-mark=isp_a_connection action=mark-routing new-routing-mark=to_isp_a passthrough=no

add chain=prerouting connection-mark=isp_b_connection action=mark-routing new-routing-mark=to_isp_b passthrough=no
```

### 🖆 PCC Distribution Characteristics

PCC distributes connections, not individual packets or guaranteed bandwidth percentages.

A single connection normally uses one WAN path.

The actual traffic distribution depends on connection patterns, application behavior, and connection sizes.

A network with a few large downloads may not achieve an exact 50/50 bandwidth split even when PCC uses equal classification groups.

For complete PCC configuration, including weighted distribution, NAT, and FastTrack considerations, refer to `03-pcc-load-balancing.md`.

# 🗁 Multi-WAN with Failover

Multi-WAN routing can combine primary and backup connections.

For example:

* ISP-A is the primary connection.
* ISP-B is the backup connection.

A custom routing table can contain both routes.

### 🖆 Adding Primary and Backup Routes

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1@main routing-table=to_isp_a distance=1
add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to_isp_a distance=2
```

The first route is preferred while it is active. The second route becomes the alternative when the primary route is unavailable according to the configured route health mechanism.

### 🖆 Gateway Reachability Versus Internet Reachability

A gateway check can confirm that the directly connected ISP gateway responds.

However, an ISP gateway may remain reachable even when the provider has lost upstream internet connectivity.

For stronger failover detection, use recursive gateway checks with reliable external monitoring targets.

See `02-recursive-gateway-checks.md` for the detailed configuration.

### 🖆 Failover in Multiple Routing Tables

If `to_isp_a` and `to_isp_b` are used independently, each table must have the correct primary and backup routes.

A backup route in `main` does not automatically guarantee that a custom table will fail over.

# 🗁 NAT Configuration

Each WAN connection generally requires an appropriate source NAT rule when private LAN addresses access the internet.

For dynamically assigned or changing WAN addresses, masquerade is commonly used.

### 🖆 ISP-A NAT

```routeros
/ip/firewall/nat
add chain=srcnat out-interface=ether1 action=masquerade comment="NAT through ISP-A"
```

### 🖆 ISP-B NAT

```routeros
/ip/firewall/nat
add chain=srcnat out-interface=ether2 action=masquerade comment="NAT through ISP-B"
```

The outgoing interface determines which NAT rule applies.

### 🖆 Important NAT Considerations

* Routing determines the outgoing interface.
* NAT translates addresses according to the selected rule.
* NAT does not select the WAN connection.
* NAT does not replace firewall filter rules.
* Static public IP deployments may use `src-nat` with a specified public address instead of masquerade.

# 🗁 Monitoring and Verification

After configuring Multi-WAN routing, verify that routes, routing tables, Mangle rules, and NAT behave as expected.

### 🖆 View Routing Tables

```routeros
/routing/table/print
```

### 🖆 View Routes

```routeros
/ip/route/print detail
```

### 🖆 View Mangle Rules

```routeros
/ip/firewall/mangle/print stats
```

### 🖆 View NAT Rules

```routeros
/ip/firewall/nat/print stats
```

### 🖆 View Connection Tracking

```routeros
/ip/firewall/connection/print
```

### 🖆 Test ISP-A

```routeros
/ping 1.1.1.1 routing-table=to_isp_a
```

### 🖆 Test ISP-B

```routeros
/ping 1.1.1.1 routing-table=to_isp_b
```

### 🖆 Trace a Route Through a Specific Table

```routeros
/tool/traceroute 1.1.1.1 routing-table=to_isp_a
```

### 🖆 Monitor WAN Interfaces

```routeros
/interface/monitor-traffic ether1
/interface/monitor-traffic ether2
```

### 🖆 Verify Public IP Address

From a client routed through each WAN, check the externally visible public IP address using an external IP-checking service.

The observed public IP should correspond to the selected provider's NAT or public address configuration.

# 🗁 Troubleshooting

### 🖆 Traffic Uses the Wrong WAN

Possible causes:

* Incorrect routing rule.
* Incorrect routing mark.
* Wrong routing table.
* A more preferred route in another lookup path.
* Mangle rule ordering problems.

Useful commands:

```routeros
/routing/rule/print detail
/ip/firewall/mangle/print stats
/ip/route/print detail
```

### 🖆 Custom Routing Table Has No Usable Route

Possible causes:

* Default route is missing.
* Gateway cannot be resolved.
* Incorrect routing table name.
* Route is inactive.
* Gateway resolution through `main` is unavailable.

Check:

```routeros
/ip/route/print detail where routing-table=to_isp_a
```

### 🖆 Internet Works Through One WAN Only

Possible causes:

* Incorrect PCC classification.
* Missing routing table.
* Missing NAT rule.
* Incorrect gateway.
* Firewall policy blocking forwarded traffic.
* FastTrack interfering with policy routing.

Inspect the Mangle counters and connection tracking entries to determine whether connections are being classified correctly.

### 🖆 Inbound Connections Fail

Possible causes:

* Return traffic leaves through another WAN.
* Missing connection marks.
* Missing routing marks.
* Incorrect destination NAT.
* Firewall filter blocks forwarded traffic.
* The provider does not route the expected public address to the router.

Verify connection marking and return-path routing.

### 🖆 Failover Does Not Work

Possible causes:

* The primary route remains active despite an upstream outage.
* Only the main table has a backup route.
* Recursive monitoring targets are unreachable.
* The backup route has an incorrect distance.
* The custom routing table has no alternative route.

Check the active routes in every relevant routing table.

### 🖆 Traffic Stops After Enabling FastTrack

FastTrack can bypass parts of firewall processing, including Mangle processing required by policy routing.

Exclude connections that depend on policy routing from FastTrack, or disable FastTrack while testing the configuration.

# 🗁 Best Practices

* Use descriptive names for WAN interfaces and routing tables.
* Keep the main routing table understandable and predictable.
* Create a separate routing table for each distinct WAN policy.
* Ensure every policy table has the required routes.
* Exclude internal networks from internet-routing classification.
* Preserve symmetric routing for inbound connections.
* Configure NAT for every WAN that requires source translation.
* Use reliable monitoring targets for internet failover.
* Review FastTrack rules when using Mangle-based routing.
* Monitor route status, Mangle counters, and connection tracking.
* Test each WAN independently before enabling load balancing.
* Document the purpose of each routing rule and Mangle rule.

# 🗁 Summary

Multi-WAN routing provides flexible control over multiple internet connections.

RouterOS v7 supports this through custom routing tables, routing rules, firewall Mangle, connection tracking, and route selection.

The key concepts are:

* Routing tables define available forwarding paths.
* Routing rules select tables based on routing conditions.
* Mangle marks connections and packets for advanced policies.
* Connection marks help preserve consistent routing.
* NAT translates addresses after routing selects the outgoing path.
* Backup routes provide alternative paths when preferred routes become unavailable.

A reliable Multi-WAN deployment requires coordinated routing, firewall, NAT, connection tracking, and monitoring configurations.
