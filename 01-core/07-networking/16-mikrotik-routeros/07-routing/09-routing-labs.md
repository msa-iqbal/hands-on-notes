# MikroTik RouterOS Routing Labs

This document contains practical routing exercises for MikroTik RouterOS v7. The labs cover route configuration, route selection, policy-based routing, dynamic routing, and troubleshooting.

The exercises are designed to build practical skills by configuring routers, testing connectivity, inspecting routing tables, and diagnosing routing failures.

# 🗁 Lab Environment

You can perform these exercises using:

* MikroTik RouterOS CHR in VirtualBox.
* MikroTik CHR in GNS3.
* MikroTik CHR in EVE-NG.
* Physical MikroTik routers.

For multi-router labs, use at least two RouterOS instances.

**Recommended topology:**

```text
                    Internet Simulation
                           |
                         R1
                        /  \
                       /    \
                     R2      R3
```

For basic routing exercises, two routers and two simulated LAN networks are sufficient.

**Example IP addressing plan:**

| Device | Interface | IP Address |
| --- | --- | --- |
| R1 | ether1 | 10.0.12.1/30 |
| R2 | ether1 | 10.0.12.2/30 |
| R1 | ether2 | 192.168.10.1/24 |
| R2 | ether2 | 192.168.20.1/24 |

The inter-router network is `10.0.12.0/30`.

The LAN networks are:

* R1 LAN: `192.168.10.0/24`
* R2 LAN: `192.168.20.0/24`

# 🗁 Lab: Configure IP Addresses and Connected Routes

**Objective**

Configure router interfaces and understand how RouterOS automatically creates connected routes.

**Topology**

```text
LAN 1                     LAN 2
192.168.10.0/24           192.168.20.0/24
     |                         |
   ether2                    ether2
     R1 -------- ether1 ------- R2
       10.0.12.1/30   10.0.12.2/30
```

**Configure R1:**

```routeros
/ip/address
add address=10.0.12.1/30 interface=ether1
add address=192.168.10.1/24 interface=ether2
```

**Configure R2:**

```routeros
/ip/address
add address=10.0.12.2/30 interface=ether1
add address=192.168.20.1/24 interface=ether2
```

**Verify connected routes:**

On R1:

```routeros
/ip/route/print
```

On R2:

```routeros
/ip/route/print
```

**Expected result**

R1 should have connected routes for:

* `10.0.12.0/30`
* `192.168.10.0/24`

R2 should have connected routes for:

* `10.0.12.0/30`
* `192.168.20.0/24`

**Practice tasks**

* Remove an IP address and inspect the routing table.
* Disable an interface and observe the connected route.
* Re-enable the interface and verify that the route returns.

# 🗁 Lab: Configure Static Routing Between Two Routers

**Objective**

Configure static routes so that two LAN networks can communicate through separate routers.

**Prerequisites**

Complete the previous lab.

**Configure a route on R1:**

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=10.0.12.2
```

**Configure a route on R2:**

```routeros
/ip/route
add dst-address=192.168.10.0/24 gateway=10.0.12.1
```

**Verify routing tables:**

```routeros
/ip/route/print detail
```

**Test connectivity from R1:**

```routeros
/ping 192.168.20.1
```

**Test connectivity from R2:**

```routeros
/ping 192.168.10.1
```

**Expected result**

Both routers should be able to reach the opposite router's LAN interface.

For end-to-end testing, connect a host to each LAN and configure its default gateway:

| Host | IP Address | Default Gateway |
| --- | --- | --- |
| PC1 | 192.168.10.10/24 | 192.168.10.1 |
| PC2 | 192.168.20.10/24 | 192.168.20.1 |

Test connectivity between PC1 and PC2.

**Practice tasks**

* Remove the static route from R1.
* Observe which destinations become unreachable.
* Restore the route and test again.
* Configure the same route using the gateway interface where appropriate.

# 🗁 Lab: Configure a Default Route

**Objective**

Configure a default route to forward traffic destined for unknown networks to an upstream router.

**Topology**

```text
LAN
192.168.10.0/24
      |
      R1
      |
      | 10.0.12.1/30
      |
      | 10.0.12.2/30
      |
      R2
      |
   Upstream Network
```

Assume R2 provides access to an upstream network.

**Configure the default route on R1:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=10.0.12.2
```

**Verify the route:**

```routeros
/ip/route/print detail where dst-address=0.0.0.0/0
```

**Test the next hop:**

```routeros
/ping 10.0.12.2
```

**Practice tasks**

* Change the default route gateway.
* Disable the upstream interface.
* Observe the default route status.
* Restore connectivity and verify route activation.

# 🗁 Lab: Configure Route Distance and Failover

**Objective**

Configure primary and backup default routes using route distance.

**Topology**

```text
                   R1
                  /  \
                 /    \
              WAN1    WAN2
               |       |
              ISP1    ISP2
```

Assume:

| Parameter | WAN1 | WAN2 |
| --- | --- | --- |
| Interface | ether1 | ether2 |
| Gateway | 192.0.2.1 | 198.51.100.1 |
| Distance | 1 | 2 |

**Configure WAN1:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=192.0.2.1 \
    distance=1 \
    comment="Primary WAN"
```

**Configure WAN2:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=198.51.100.1 \
    distance=2 \
    comment="Backup WAN"
```

**Verify routes:**

```routeros
/ip/route/print detail where dst-address=0.0.0.0/0
```

**Expected result**

* WAN1 is preferred because it has a lower distance.
* WAN2 can become active when WAN1's route becomes unavailable.
* WAN1 becomes preferred again when it is available and eligible.

**Practice tasks**

* Disable WAN1 and inspect route selection.
* Re-enable WAN1.
* Change WAN2 distance to `1`.
* Observe how equal-distance routes behave when both are eligible.

# 🗁 Lab: Configure Recursive Routing

**Objective**

Monitor upstream connectivity using recursive routing.

**Topology**

```text
                  R1
                 /  \
              WAN1  WAN2
                |     |
              ISP1   ISP2
                |     |
             1.1.1.1 8.8.8.8
```

Assume:

| Parameter | WAN1 | WAN2 |
| --- | --- | --- |
| Gateway | 192.0.2.1 | 198.51.100.1 |
| Monitoring IP | 1.1.1.1 | 8.8.8.8 |
| Distance | 1 | 2 |

**Configure monitoring routes:**

```routeros
/ip/route
add dst-address=1.1.1.1/32 \
    gateway=192.0.2.1 \
    scope=10 \
    comment="WAN1 probe"

/ip/route
add dst-address=8.8.8.8/32 \
    gateway=198.51.100.1 \
    scope=10 \
    comment="WAN2 probe"
```

**Configure recursive default routes:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=1.1.1.1 \
    target-scope=11 \
    check-gateway=ping \
    distance=1 \
    comment="WAN1 recursive default"

/ip/route
add dst-address=0.0.0.0/0 \
    gateway=8.8.8.8 \
    target-scope=11 \
    check-gateway=ping \
    distance=2 \
    comment="WAN2 recursive default"
```

**Verify route status:**

```routeros
/ip/route/print detail
```

**Practice tasks**

* Test both monitoring destinations.
* Simulate WAN1 failure.
* Observe whether WAN2 becomes active.
* Restore WAN1 and observe failback.
* Test what happens when a monitoring destination is unreachable while its WAN remains connected.

# 🗁 Lab: Configure Routing Rules

**Objective**

Use routing rules to send traffic from a specific source network through a dedicated routing table.

**Topology**

```text
                 R1
                /  \
             WAN1  WAN2
               |     |
              ISP1  ISP2
```

Assume:

* WAN1 is the primary connection.
* WAN2 is dedicated to a particular LAN.
* LAN2 uses `192.168.20.0/24`.

**Create a custom routing table:**

```routeros
/routing/table
add name=to-wan2 fib
```

**Add a default route to the custom table:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=198.51.100.1 \
    routing-table=to-wan2 \
    comment="WAN2 policy route"
```

**Create a routing rule:**

```routeros
/routing/rule
add src-address=192.168.20.0/24 \
    action=lookup-only-in-table \
    table=to-wan2
```

**Verify routing rules:**

```routeros
/routing/rule/print
```

**Practice tasks**

* Send another source subnet through WAN2.
* Change the rule to `lookup`.
* Compare `lookup` with `lookup-only-in-table`.
* Test behavior when the custom table has no usable route.

Ensure that the WAN2 gateway is resolvable in the intended routing context before testing.

# 🗁 Lab: Configure Mangle-Based Policy Routing

**Objective**

Use firewall mangle rules to mark traffic and direct it through a custom routing table.

**Scenario**

Traffic originating from `192.168.20.0/24` should use WAN2.

**Create the routing table:**

```routeros
/routing/table
add name=wan2-table fib
```

**Add a WAN2 default route:**

```routeros
/ip/route
add dst-address=0.0.0.0/0 \
    gateway=198.51.100.1 \
    routing-table=wan2-table
```

**Create a routing mark:**

```routeros
/ip/firewall/mangle
add chain=prerouting \
    src-address=192.168.20.0/24 \
    dst-address-type=!local \
    action=mark-routing \
    new-routing-mark=wan2-table \
    passthrough=no \
    comment="Route LAN2 through WAN2"
```

**Verify mangle rules:**

```routeros
/ip/firewall/mangle/print stats
```

**Practice tasks**

* Create a second routing table for WAN1.
* Route a different source subnet through WAN1.
* Inspect packet and byte counters.
* Compare routing rules with mangle-based policy routing.
* Investigate FastTrack interactions with policy routing.

For a production configuration, also account for connection tracking, NAT, local destinations, and return-path symmetry.

# 🗁 Lab: Configure OSPF Between Two Routers

**Objective**

Enable OSPF so routers can dynamically exchange network routes.

**Topology**

```text
LAN 1                  LAN 2
192.168.10.0/24        192.168.20.0/24
     |                      |
     R1 ------------------- R2
       10.0.12.0/30
```

Use the IP addressing configuration from the first lab.

**Configure OSPF on R1:**

```routeros
/routing/ospf/instance
add name=ospf-v2 version=2 router-id=1.1.1.1

/routing/ospf/area
add name=backbone instance=ospf-v2 area-id=0.0.0.0

/routing/ospf/interface-template
add area=backbone networks=10.0.12.0/30
add area=backbone networks=192.168.10.0/24 passive
```

**Configure OSPF on R2:**

```routeros
/routing/ospf/instance
add name=ospf-v2 version=2 router-id=2.2.2.2

/routing/ospf/area
add name=backbone instance=ospf-v2 area-id=0.0.0.0

/routing/ospf/interface-template
add area=backbone networks=10.0.12.0/30
add area=backbone networks=192.168.20.0/24 passive
```

**Verify OSPF neighbors:**

```routeros
/routing/ospf/neighbor/print
```

**Inspect learned routes:**

```routeros
/ip/route/print detail
```

**Expected result**

* R1 and R2 establish an OSPF neighbor relationship.
* R1 learns the R2 LAN prefix.
* R2 learns the R1 LAN prefix.
* Both LANs can communicate through the routers.

**Practice tasks**

* Add a third router.
* Configure a different OSPF area.
* Change the OSPF interface cost.
* Disable the inter-router interface and inspect neighbor state.
* Investigate why an OSPF neighbor might remain stuck in an unexpected state.

# 🗁 Lab: Configure BGP Between Two Routers

**Objective**

Establish an eBGP session between two routers and exchange network prefixes.

**Topology**

```text
       R1                         R2
     AS 65001                   AS 65002
  10.0.12.1/30 --------------- 10.0.12.2/30

  LAN: 192.168.10.0/24         LAN: 192.168.20.0/24
```

This example uses the explicit BGP instance and connection model available in RouterOS v7.20 and later.

**Configure R1:**

```routeros
/routing/bgp/instance
add name=as65001 as=65001 router-id=1.1.1.1

/ip/firewall/address-list
add list=bgp-networks address=192.168.10.0/24

/routing/bgp/connection
add name=R1-to-R2 \
    instance=as65001 \
    local.role=ebgp \
    local.address=10.0.12.1 \
    remote.address=10.0.12.2 \
    remote.as=65002 \
    output.network=bgp-networks
```

**Configure R2:**

```routeros
/routing/bgp/instance
add name=as65002 as=65002 router-id=2.2.2.2

/ip/firewall/address-list
add list=bgp-networks address=192.168.20.0/24

/routing/bgp/connection
add name=R2-to-R1 \
    instance=as65002 \
    local.role=ebgp \
    local.address=10.0.12.2 \
    remote.address=10.0.12.1 \
    remote.as=65001 \
    output.network=bgp-networks
```

The advertised prefixes must exist as active routes in the local routing table for the configured BGP network advertisement mechanism.

**Verify BGP sessions:**

```routeros
/routing/bgp/session/print detail
```

**Inspect BGP routes:**

```routeros
/routing/route/print detail where protocol=bgp
```

**Practice tasks**

* Advertise an additional LAN prefix.
* Inspect received and advertised routes.
* Configure BGP input filters.
* Disable the BGP connection and inspect route withdrawal.
* Investigate the effect of an incorrect remote AS number.

# 🗁 Lab: Configure IPv6 Static Routing

**Objective**

Configure IPv6 addresses and static routes between two routers.

**Topology**

```text
LAN 1                       LAN 2
2001:db8:10::/64             2001:db8:20::/64
      |                            |
      R1 ------------------------- R2
          2001:db8:12::/64
```

**Configure R1:**

```routeros
/ipv6/address
add address=2001:db8:12::1/64 interface=ether1
add address=2001:db8:10::1/64 interface=ether2
```

**Configure R2:**

```routeros
/ipv6/address
add address=2001:db8:12::2/64 interface=ether1
add address=2001:db8:20::1/64 interface=ether2
```

**Configure the static route on R1:**

```routeros
/ipv6/route
add dst-address=2001:db8:20::/64 gateway=2001:db8:12::2
```

**Configure the static route on R2:**

```routeros
/ipv6/route
add dst-address=2001:db8:10::/64 gateway=2001:db8:12::1
```

**Verify IPv6 routes:**

```routeros
/ipv6/route/print detail
```

**Test connectivity:**

```routeros
/ping 2001:db8:12::2
```

From R2:

```routeros
/ping 2001:db8:12::1
```

**Practice tasks**

* Configure an IPv6 default route.
* Add another IPv6 LAN.
* Test communication between the LANs.
* Inspect IPv6 neighbor discovery entries.

# 🗁 Lab: Troubleshoot Routing Problems

**Objective**

Develop a systematic approach to diagnosing routing failures.

Use the two-router topology from the static routing lab.

**Scenario: Missing Static Route**

Remove the route to the opposite LAN from R1:

```routeros
/ip/route
remove [find dst-address=192.168.20.0/24]
```

Test connectivity:

```routeros
/ping 192.168.20.1
```

Inspect the routing table:

```routeros
/ip/route/print detail
```

**Expected finding**

R1 has no specific route to the R2 LAN. Restore the route:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=10.0.12.2
```

**Scenario: Incorrect Gateway**

Configure an incorrect gateway:

```routeros
/ip/route
add dst-address=192.168.20.0/24 gateway=10.0.12.10
```

Inspect the route and test connectivity.

Determine whether the gateway can be resolved through the connected network.

**Scenario: Interface Failure**

Disable the inter-router interface on R1:

```routeros
/interface
disable ether1
```

Inspect the connected and static routes:

```routeros
/ip/route/print detail
```

Restore the interface:

```routeros
/interface
enable ether1
```

**Scenario: Firewall Blocking Traffic**

Inspect firewall rules:

```routeros
/ip/firewall/filter/print stats
```

Check whether traffic is being dropped in the `input` or `forward` chain.

Remember:

* `input` handles traffic destined for the router.
* `forward` handles traffic routed through the router.
* `output` handles traffic generated by the router.

**Scenario: Routing Loop**

Create an isolated lab with multiple routers and inspect the next hop of each route.

Use traceroute to identify repeated hops:

```routeros
/tool/traceroute address=192.168.20.1
```

Identify and remove the incorrect static route responsible for the loop.

# 🗁 Routing Verification Commands

Use these commands regularly when configuring and troubleshooting RouterOS routing.

| Command | Purpose |
| --- | --- |
| `/ip/route/print` | Display IPv4 routes |
| `/ip/route/print detail` | Display detailed IPv4 route information |
| `/routing/route/print detail` | Inspect routing information |
| `/routing/table/print` | Display routing tables |
| `/routing/rule/print` | Display routing rules |
| `/ip/firewall/mangle/print stats` | Inspect mangle counters |
| `/routing/ospf/neighbor/print` | Inspect OSPF neighbors |
| `/routing/bgp/session/print detail` | Inspect BGP sessions |
| `/ping address=...` | Test reachability |
| `/tool/traceroute address=...` | Trace the forwarding path |

# 🗁 Final Practice Challenges

Complete these challenges without copying the previous configurations directly.

**Static Routing Challenge**

* Build a network with three routers.
* Connect three separate LAN networks.
* Configure static routes between all LANs.
* Verify end-to-end connectivity.

**Dynamic Routing Challenge**

* Build a network with three routers.
* Configure OSPF.
* Advertise all LAN networks.
* Verify route learning and neighbor relationships.
* Simulate a link failure.

**Multi-WAN Challenge**

* Configure two WAN connections.
* Make WAN1 the primary connection.
* Make WAN2 the backup connection.
* Implement recursive gateway monitoring.
* Test failover and failback.

**Policy Routing Challenge**

* Create two routing tables.
* Route one LAN through WAN1.
* Route another LAN through WAN2.
* Verify source-based routing.
* Test behavior when one WAN becomes unavailable.

**ISP Routing Challenge**

* Build a small simulated ISP network.
* Configure customer routers.
* Use OSPF for internal routing.
* Use BGP between autonomous systems.
* Advertise customer prefixes.
* Apply routing filters.
* Verify end-to-end reachability.

# 🗁 Lab Completion Checklist

* [ ] Understand connected and static routes.
* [ ] Configure static routes between routers.
* [ ] Configure default routes.
* [ ] Understand route distance and selection.
* [ ] Configure primary and backup routes.
* [ ] Implement recursive routing.
* [ ] Configure routing rules.
* [ ] Implement mangle-based policy routing.
* [ ] Establish OSPF neighbor relationships.
* [ ] Exchange routes using BGP.
* [ ] Configure IPv6 static routing.
* [ ] Troubleshoot missing routes and unreachable gateways.
* [ ] Verify routing using RouterOS diagnostic commands.
* [ ] Complete the multi-WAN and ISP routing challenges.
