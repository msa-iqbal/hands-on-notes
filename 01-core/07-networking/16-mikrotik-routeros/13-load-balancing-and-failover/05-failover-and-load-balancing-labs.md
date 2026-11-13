# Failover and Load Balancing Labs

Failover and load balancing are important techniques for building reliable multi-WAN networks.

Failover provides an alternative internet connection when the primary connection becomes unavailable.

Load balancing distributes outgoing connections across multiple WAN connections to improve aggregate network utilization.

In these labs, you will configure and test both techniques using MikroTik RouterOS v7.

The exercises progress from basic WAN configuration to advanced multi-WAN routing.

# 🗁 Lab Environment

You can perform these labs using:

* MikroTik physical router.
* MikroTik CHR.
* GNS3.
* EVE-NG.
* VirtualBox with CHR.
* VMware with CHR.

### 🖆 Required Knowledge

Before starting, understand:

* IP addressing and subnetting.
* Static and default routing.
* Route distance and route selection.
* Firewall filter and NAT.
* Connection tracking.
* Firewall Mangle.
* Routing tables.
* PCC classification.
* Recursive gateway checking.

### 🖆 Required Devices

| Device | Purpose |
| --- | --- |
| MikroTik Router | Main router |
| ISP-A | Primary or first WAN |
| ISP-B | Secondary or second WAN |
| LAN client | Traffic generation and testing |
| Internet test destination | Connectivity verification |

# 🗁 Lab Topology

Use the following example topology.

```text
                 Internet
                    |
          +---------+---------+
          |                   |
       ISP-A                 ISP-B
     192.0.2.1           198.51.100.1
          |                   |
       ether1              ether2
          |                   |
          +---------+---------+
                    |
              MikroTik Router
                    |
                 ether3
                    |
                LAN Switch
                    |
             192.168.10.0/24
                    |
                LAN Clients
```

### 🖆 IP Addressing Plan

| Device | Interface | IP address |
| --- | --- | --- |
| ISP-A | Gateway | 192.0.2.1/24 |
| MikroTik | ether1 | 192.0.2.2/24 |
| ISP-B | Gateway | 198.51.100.1/24 |
| MikroTik | ether2 | 198.51.100.2/24 |
| MikroTik | ether3 | 192.168.10.1/24 |
| LAN | Network | 192.168.10.0/24 |

The addresses `192.0.2.0/24` and `198.51.100.0/24` are documentation ranges. Replace them with addresses appropriate for your lab.

### 🖆 Interface Naming

| Interface | Role |
| --- | --- |
| ether1 | WAN-ISP-A |
| ether2 | WAN-ISP-B |
| ether3 | LAN |

Use these interface names consistently throughout the labs.

# 🗁 Lab Preparation

Start with a clean or isolated lab router.

### 🖆 Rename Interfaces

```routeros
/interface
set [find default-name=ether1] name=WAN-ISP-A
set [find default-name=ether2] name=WAN-ISP-B
set [find default-name=ether3] name=LAN
```

### 🖆 Configure IP Addresses

```routeros
/ip/address
add address=192.0.2.2/24 interface=WAN-ISP-A comment="ISP-A"
add address=198.51.100.2/24 interface=WAN-ISP-B comment="ISP-B"
add address=192.168.10.1/24 interface=LAN comment="LAN"
```

### 🖆 Configure a LAN DHCP Server

```routeros
/ip/pool
add name=LAN-POOL ranges=192.168.10.100-192.168.10.200

/ip/dhcp-server
add name=LAN-DHCP interface=LAN address-pool=LAN-POOL disabled=no

/ip/dhcp-server/network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=1.1.1.1,8.8.8.8
```

### 🖆 Configure NAT

```routeros
/ip/firewall/nat
add chain=srcnat out-interface=WAN-ISP-A action=masquerade comment="NAT ISP-A"
add chain=srcnat out-interface=WAN-ISP-B action=masquerade comment="NAT ISP-B"
```

### 🖆 Create an Internal Network Address List

```routeros
/ip/firewall/address-list
add list=LOCAL-NETWORKS address=192.168.10.0/24
```

Add every other internal subnet used in your environment to this list.

# 🗁 Lab: Configure Multiple WAN Connections

**Objective:** Configure two internet connections and verify that both are independently reachable.

### 🖆 Configure Default Routes

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1 distance=1 comment="ISP-A Default"
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2 comment="ISP-B Backup"
```

ISP-A is preferred because it has the lower distance.

ISP-B is the backup route for traffic using the main routing table.

### 🖆 Verify WAN Gateways

```routeros
/ping 192.0.2.1
/ping 198.51.100.1
```

### 🖆 Verify Internet Connectivity

```routeros
/ping 1.1.1.1
```

### 🖆 Verify Routes

```routeros
/ip/route/print detail
```

### 🖆 Expected Result

* Both WAN gateways respond.
* ISP-A is the active preferred default route.
* ISP-B is available as an alternative route.
* LAN clients can access the internet through ISP-A.

# 🗁 Lab: Configure WAN Failover

**Objective:** Automatically switch to ISP-B when ISP-A becomes unavailable.

### 🖆 Configure Primary and Backup Routes

If the previous lab routes are already configured, inspect them before adding new routes.

```routeros
/ip/route
print detail
```

For a gateway-reachability-based test, configure the routes with gateway checking:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1 check-gateway=ping distance=1 comment="ISP-A Primary"
add dst-address=0.0.0.0/0 gateway=198.51.100.1 check-gateway=ping distance=2 comment="ISP-B Backup"
```

Avoid keeping duplicate default routes from the preparation lab when testing this configuration.

### 🖆 Test Failover

Disable the primary WAN interface:

```routeros
/interface
disable WAN-ISP-A
```

Check the route table:

```routeros
/ip/route/print detail
```

Test connectivity:

```routeros
/ping 1.1.1.1
```

### 🖆 Restore the Primary WAN

```routeros
/interface
enable WAN-ISP-A
```

Check the route table again.

### 🖆 Expected Result

* ISP-A is preferred when available.
* ISP-B becomes active when ISP-A's route becomes unavailable.
* ISP-A becomes preferred again after recovery.

**Important:** `check-gateway=ping` checks gateway reachability. It does not prove that the ISP has working upstream internet connectivity. Use recursive gateway monitoring for stronger failure detection.

# 🗁 Lab: Configure Recursive Gateway Monitoring

**Objective:** Detect upstream connectivity failures even when the directly connected ISP gateway remains reachable.

Use dedicated monitoring addresses for each ISP.

Example:

| WAN | Monitoring address | Gateway |
| --- | --- | --- |
| ISP-A | 1.1.1.1 | 192.0.2.1 |
| ISP-B | 8.8.8.8 | 198.51.100.1 |

### 🖆 Create Monitoring Routes

Each monitoring destination must resolve through its own ISP.

```routeros
/ip/route
add dst-address=1.1.1.1/32 gateway=192.0.2.1 scope=10 comment="ISP-A Monitor"
add dst-address=8.8.8.8/32 gateway=198.51.100.1 scope=10 comment="ISP-B Monitor"
```

### 🖆 Configure Recursive Default Routes

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=1.1.1.1 distance=1 target-scope=11 check-gateway=ping comment="ISP-A Recursive Default"

add dst-address=0.0.0.0/0 gateway=8.8.8.8 distance=2 target-scope=11 check-gateway=ping comment="ISP-B Recursive Default"
```

### 🖆 Verify Recursive Routes

```routeros
/ip/route/print detail
```

Check:

* Monitoring host routes are active.
* Recursive default routes resolve through the intended WAN.
* The primary route has the lower distance.
* Gateway checking is enabled on the recursive routes.

### 🖆 Test Upstream Failure

In an isolated lab, simulate an upstream failure while keeping the ISP gateway reachable.

Observe whether the monitoring address stops responding through the primary path.

### 🖆 Expected Result

When the ISP-A monitoring target becomes unreachable, the primary recursive route should become inactive and ISP-B should take over.

When ISP-A connectivity recovers, its lower-distance route should become preferred again.

### 🖆 Important Considerations

* Use reliable monitoring targets.
* Ensure monitoring destinations are routed through their intended WAN.
* Do not allow a monitoring target to be reached through another WAN accidentally.
* ICMP filtering or rate limiting can produce false failure detection.
* Choose monitoring targets that are independent of the ISP gateway.
* Test the exact behavior in your environment before deploying it in production.

# 🗁 Lab: Configure PCC Load Balancing

**Objective:** Distribute outgoing LAN connections across two WAN connections.

### 🖆 Create Routing Tables

```routeros
/routing/table
add name=to_ISP_A fib
add name=to_ISP_B fib
```

### 🖆 Add WAN-Specific Routes

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=192.0.2.1@main routing-table=to_ISP_A distance=1

add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to_ISP_B distance=1
```

### 🖆 Configure PCC Connection Classification

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=LAN src-address=192.168.10.0/24 dst-address-list=!LOCAL-NETWORKS connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:2/0 action=mark-connection new-connection-mark=ISP_A_CONN passthrough=yes comment="PCC ISP-A"

add chain=prerouting in-interface=LAN src-address=192.168.10.0/24 dst-address-list=!LOCAL-NETWORKS connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:2/1 action=mark-connection new-connection-mark=ISP_B_CONN passthrough=yes comment="PCC ISP-B"
```

### 🖆 Apply Routing Marks

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=LAN connection-mark=ISP_A_CONN action=mark-routing new-routing-mark=to_ISP_A passthrough=no

add chain=prerouting in-interface=LAN connection-mark=ISP_B_CONN action=mark-routing new-routing-mark=to_ISP_B passthrough=no
```

### 🖆 Verify PCC Classification

```routeros
/ip/firewall/mangle/print stats
/ip/firewall/connection/print
```

Generate multiple new connections from LAN clients.

### 🖆 Expected Result

* Some connections are assigned to ISP-A.
* Other connections are assigned to ISP-B.
* Each marked connection uses its assigned routing table.
* Internal LAN traffic remains outside the PCC classification.

PCC distributes connections, not individual packets. Existing connections should not be expected to move immediately when the rules are changed.

# 🗁 Lab: Configure Weighted PCC

**Objective:** Distribute connections unevenly according to WAN capacity.

Assume:

| WAN | Available bandwidth | Relative weight |
| --- | ---: | ---: |
| ISP-A | 100 Mbps | 2 |
| ISP-B | 50 Mbps | 1 |

The intended connection classification ratio is approximately 2:1.

### 🖆 Configure PCC Classification

Use a divisor of `3`.

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=LAN src-address=192.168.10.0/24 dst-address-list=!LOCAL-NETWORKS connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:3/0 action=mark-connection new-connection-mark=ISP_A_CONN passthrough=yes

add chain=prerouting in-interface=LAN src-address=192.168.10.0/24 dst-address-list=!LOCAL-NETWORKS connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:3/1 action=mark-connection new-connection-mark=ISP_A_CONN passthrough=yes

add chain=prerouting in-interface=LAN src-address=192.168.10.0/24 dst-address-list=!LOCAL-NETWORKS connection-mark=no-mark per-connection-classifier=both-addresses-and-ports:3/2 action=mark-connection new-connection-mark=ISP_B_CONN passthrough=yes
```

### 🖆 Apply Routing Marks

Use the routing-mark rules from the previous PCC lab.

### 🖆 Expected Result

* Two classification groups select ISP-A.
* One classification group selects ISP-B.
* The intended connection allocation is approximately 2:1.

This does not guarantee an exact 2:1 bandwidth ratio. Actual throughput depends on connection sizes, client behavior, and application traffic patterns.

# 🗁 Lab: Configure Policy-Based Routing

**Objective:** Route different LAN networks through different WAN providers.

Assume:

| Network | Routing policy |
| --- | --- |
| 192.168.10.0/24 | ISP-A |
| 192.168.20.0/24 | ISP-B |

### 🖆 Add the Second LAN Network

Configure the second LAN subnet on an appropriate interface or VLAN.

For a lab using a separate interface:

```routeros
/ip/address
add address=192.168.20.1/24 interface=ether4 comment="LAN-B"
```

Add it to the internal network address list:

```routeros
/ip/firewall/address-list
add list=LOCAL-NETWORKS address=192.168.20.0/24
```

### 🖆 Create Routing Rules

```routeros
/routing/rule
add src-address=192.168.10.0/24 action=lookup-only-in-table table=to_ISP_A

add src-address=192.168.20.0/24 action=lookup-only-in-table table=to_ISP_B
```

### 🖆 Test Both Networks

From a client in each subnet:

* Open an external IP-checking service.
* Confirm the public IP address.
* Confirm that the selected ISP matches the policy.

### 🖆 Expected Result

* LAN-A uses ISP-A.
* LAN-B uses ISP-B.
* Internal destinations remain reachable through internal routing.

# 🗁 Lab: Configure Router-Originated Traffic Routing

**Objective:** Send traffic generated by the router through a selected WAN.

Router-originated packets use the `output` chain, not the LAN `prerouting` chain.

### 🖆 Mark Router-Originated Connections

```routeros
/ip/firewall/mangle
add chain=output connection-mark=no-mark action=mark-connection new-connection-mark=ROUTER_ISP_B passthrough=yes comment="Router connections ISP-B"
```

### 🖆 Apply the Routing Mark

```routeros
/ip/firewall/mangle
add chain=output connection-mark=ROUTER_ISP_B action=mark-routing new-routing-mark=to_ISP_B passthrough=no
```

This example routes all eligible router-originated connections through ISP-B. A production policy should use more specific matching conditions when only selected router services need that WAN.

### 🖆 Test the Routing Table

```routeros
/ping 1.1.1.1 routing-table=to_ISP_B
```

### 🖆 Expected Result

The test uses the ISP-B routing table.

# 🗁 Lab: Preserve Symmetric Routing

**Objective:** Ensure that inbound connections return through the WAN that received them.

### 🖆 Mark Connections Received Through ISP-A

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=WAN-ISP-A connection-state=new connection-mark=no-mark action=mark-connection new-connection-mark=ISP_A_IN passthrough=yes
```

### 🖆 Mark Connections Received Through ISP-B

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=WAN-ISP-B connection-state=new connection-mark=no-mark action=mark-connection new-connection-mark=ISP_B_IN passthrough=yes
```

### 🖆 Apply Routing Marks to Return Traffic

```routeros
/ip/firewall/mangle
add chain=prerouting connection-mark=ISP_A_IN action=mark-routing new-routing-mark=to_ISP_A passthrough=no

add chain=prerouting connection-mark=ISP_B_IN action=mark-routing new-routing-mark=to_ISP_B passthrough=no
```

### 🖆 Test Inbound Connectivity

Use an externally reachable test service or server in your lab.

Check:

* The connection arrives through the intended WAN.
* Connection tracking records the expected connection mark.
* Response traffic uses the corresponding routing table.
* Firewall filter rules allow only the intended inbound services.

### 🖆 Expected Result

Return traffic follows the WAN associated with the inbound connection.

For production configurations, carefully distinguish forwarded traffic from traffic destined to the router itself. Ensure that routing marks do not accidentally redirect router-local or internal traffic.

# 🗁 Lab: Combine PCC and Failover

**Objective:** Distribute connections across two WANs and provide backup routes when either WAN becomes unavailable.

This lab combines:

* Custom routing tables.
* PCC connection classification.
* WAN-specific default routes.
* Recursive gateway monitoring.
* WAN-specific NAT.
* Connection tracking.

### 🖆 Create Routing Tables

```routeros
/routing/table
add name=to_ISP_A fib
add name=to_ISP_B fib
```

### 🖆 Configure ISP-A Monitoring

```routeros
/ip/route
add dst-address=1.1.1.1/32 gateway=192.0.2.1 scope=10 comment="ISP-A Monitor"

add dst-address=0.0.0.0/0 gateway=1.1.1.1 routing-table=to_ISP_A target-scope=11 check-gateway=ping distance=1 comment="ISP-A Primary"

add dst-address=0.0.0.0/0 gateway=198.51.100.1@main routing-table=to_ISP_A distance=2 comment="ISP-B Backup for ISP-A Table"
```

### 🖆 Configure ISP-B Monitoring

```routeros
/ip/route
add dst-address=8.8.8.8/32 gateway=198.51.100.1 scope=10 comment="ISP-B Monitor"

add dst-address=0.0.0.0/0 gateway=8.8.8.8 routing-table=to_ISP_B target-scope=11 check-gateway=ping distance=1 comment="ISP-B Primary"

add dst-address=0.0.0.0/0 gateway=192.0.2.1@main routing-table=to_ISP_B distance=2 comment="ISP-A Backup for ISP-B Table"
```

### 🖆 Configure PCC Classification

Use the PCC classification rules from the previous PCC lab.

### 🖆 Apply Routing Marks

Use the routing-mark rules from the previous PCC lab.

### 🖆 Configure NAT

```routeros
/ip/firewall/nat
add chain=srcnat out-interface=WAN-ISP-A action=masquerade comment="NAT ISP-A"

add chain=srcnat out-interface=WAN-ISP-B action=masquerade comment="NAT ISP-B"
```

### 🖆 Test Normal Operation

Generate traffic from multiple LAN clients.

Verify that:

* Connections are classified by PCC.
* Both WAN links carry traffic.
* Each routing table uses its preferred WAN.

### 🖆 Test ISP-A Failure

Simulate an upstream ISP-A failure.

Check:

```routeros
/ip/route/print detail
```

The ISP-A recursive route should become inactive when its monitoring target is unreachable through that path. The backup route in `to_ISP_A` should become available.

### 🖆 Test ISP-B Failure

Repeat the test for ISP-B.

### 🖆 Expected Result

* Normal operation distributes connections across both WANs.
* The affected routing table can use its backup route.
* New connections can use the remaining working WAN.
* Existing connections may be interrupted when their WAN path or NAT mapping changes.

### 🖆 Important Design Considerations

This is a lab design, not a universal production template.

When a WAN fails, connections originally translated through its public address may not survive a change to another WAN. Applications may need to reconnect.

Also consider:

* Router-originated traffic.
* Inbound connections.
* Internal network exclusions.
* FastTrack behavior.
* Monitoring target reachability.
* Firewall filter policy.
* NAT behavior after route changes.

# 🗁 Monitoring and Verification

Use these commands to inspect the router during each lab.

### 🖆 View Routing Tables

```routeros
/routing/table/print
```

### 🖆 View All Routes

```routeros
/ip/route/print detail
```

### 🖆 View Routing Rules

```routeros
/routing/rule/print detail
```

### 🖆 View Mangle Rules and Counters

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

### 🖆 Monitor WAN Traffic

```routeros
/interface/monitor-traffic WAN-ISP-A
/interface/monitor-traffic WAN-ISP-B
```

### 🖆 Test Each Routing Table

```routeros
/ping 1.1.1.1 routing-table=to_ISP_A
/ping 1.1.1.1 routing-table=to_ISP_B
```

### 🖆 Verify the Public IP Address

From clients assigned to each WAN policy, inspect the public IP address reported by an external IP-checking service.

# 🗁 Troubleshooting Exercises

Use these scenarios to practice identifying and fixing common Multi-WAN problems.

### 🖆 Exercise: ISP-B Never Receives Traffic

**Problem:** PCC is configured, but only ISP-A carries traffic.

Investigate:

* PCC rule counters.
* Connection marks.
* Routing marks.
* Custom routing table routes.
* ISP-B NAT rule.
* ISP-B gateway reachability.

### 🖆 Exercise: LAN Clients Lose Internet After PCC Configuration

**Problem:** Clients can reach the router but cannot access external destinations.

Investigate:

* Routing table names.
* Default routes.
* Gateway resolution.
* Mangle rule order.
* NAT rules.
* Firewall forward-chain policy.

### 🖆 Exercise: ISP Failover Does Not Activate

**Problem:** The primary route remains active after upstream connectivity fails.

Investigate:

* Recursive monitoring routes.
* Monitoring target reachability.
* Route scope and target-scope.
* Gateway checking.
* Backup route distance.
* Whether the monitoring target is accidentally reachable through another WAN.

### 🖆 Exercise: Inbound Connections Fail Through ISP-B

**Problem:** Connections arrive through ISP-B but responses leave through ISP-A.

Investigate:

* Connection marks.
* Routing marks.
* Routing table contents.
* Destination NAT.
* Firewall filter rules.
* FastTrack rules.

### 🖆 Exercise: Some Applications Work and Others Fail

**Problem:** Certain applications work while others experience intermittent connectivity.

Investigate:

* Connection classification.
* Return-path symmetry.
* MTU and path MTU.
* FastTrack.
* Connection tracking.
* Provider restrictions.
* Application behavior when public source IP addresses change.

# 🗁 Final Practical Project

Build a complete two-WAN network with load balancing, failover, and policy-based routing.

### 🖆 Project Requirements

Configure the following:

| Requirement | Expected configuration |
| --- | --- |
| WAN connections | Two independent ISPs |
| LAN network | 192.168.10.0/24 |
| Routing tables | Separate table for each WAN |
| Load balancing | PCC |
| Failover | Recursive gateway monitoring |
| NAT | WAN-specific masquerade |
| Policy routing | Selected traffic through a preferred WAN |
| Inbound routing | Preserve connection symmetry |
| Monitoring | Route, interface, and connection monitoring |

### 🖆 Suggested Implementation Order

* Configure interfaces and IP addresses.
* Verify both WAN gateways.
* Configure WAN-specific NAT.
* Create routing tables.
* Configure recursive gateway monitoring.
* Add primary and backup routes.
* Configure PCC connection classification.
* Apply routing marks.
* Exclude internal destinations.
* Configure policy-based routing.
* Configure inbound connection marking where required.
* Review FastTrack rules.
* Test normal operation.
* Test each WAN failure independently.
* Verify recovery after each failure.

### 🖆 Project Acceptance Criteria

The project is complete when:

* Both WAN connections work independently.
* PCC distributes new connections across both WANs.
* Each WAN-specific routing table has a valid backup path.
* Internal network traffic remains internal.
* Selected networks follow their assigned routing policies.
* Router-originated traffic follows the intended policy.
* Inbound connections use symmetric return paths.
* Monitoring identifies upstream failures.
* Recovery restores the preferred route.
* Firewall and NAT behavior remain correct during failover.

### 🖆 Optional Advanced Challenges

* Add a third WAN connection.
* Implement weighted PCC for unequal bandwidth links.
* Route selected applications through a dedicated ISP.
* Add a dedicated management network.
* Configure WAN-specific DNS behavior.
* Monitor WAN latency and packet loss.
* Create Netwatch-based notification and automation.
* Build a complete ISP edge router with subscriber networks.

# 🗁 Summary

These labs cover the practical configuration and troubleshooting of Multi-WAN routing in RouterOS v7.

You have worked with:

* Multiple WAN interfaces.
* Default routes and route distances.
* Recursive gateway monitoring.
* PCC load balancing.
* Weighted PCC.
* Policy-based routing.
* Router-originated traffic routing.
* Symmetric routing for inbound connections.
* NAT across multiple WANs.
* Combined load balancing and failover.

The final project combines these techniques into a practical multi-WAN network suitable for further expansion and testing.
