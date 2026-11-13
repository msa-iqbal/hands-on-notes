# Routing Exercises

Routing exercises help you understand how routers forward packets between different networks. In these labs, you will configure connected routes, static routes, default routes, route priorities, and basic routing failover.

You will use MikroTik RouterOS v7 and Ubuntu/Linux clients to build practical routing topologies and troubleshoot connectivity.

### 🖆 Learning Objectives

By completing these exercises, you will be able to:

* Understand how routers select routes.
* Identify directly connected, static, and default routes.
* Configure static routes between multiple networks.
* Configure and verify default gateways.
* Understand longest-prefix match.
* Configure primary and backup routes using route distance.
* Test routing with `ping`, `traceroute`, and route inspection commands.
* Identify common routing problems.

### 🖆 Lab Requirements

| Component | Requirement |
| --- | --- |
| Router platform | MikroTik CHR |
| RouterOS version | RouterOS v7 |
| Client operating system | Ubuntu/Linux |
| Virtualization | VirtualBox, GNS3, or EVE-NG |
| Routers | Two or three |
| Clients | Two or three |
| Network configuration | Static IPv4 addressing |

# 🗁 Routing Fundamentals

### 🖆 What Is Routing?

Routing is the process of selecting a path through which packets travel from a source network to a destination network.

A router examines the destination IP address of an incoming packet and checks its routing table to determine the appropriate next hop or outgoing interface.

For example:

* PC-A belongs to `192.168.10.0/24`.
* PC-B belongs to `192.168.20.0/24`.
* Both networks are connected through a router.

The router must know how to reach both networks before communication can succeed.

### 🖆 Types of Routes

| Route Type | Description |
| --- | --- |
| Connected route | Automatically created when an IP address is assigned to an active interface. |
| Static route | Manually configured by an administrator. |
| Default route | Used when no more specific route matches the destination. |
| Dynamic route | Learned through routing protocols such as OSPF or BGP. |

### 🖆 Longest Prefix Match

When multiple routes match a destination IP address, a router generally selects the route with the longest matching prefix.

For example, consider these routes:

| Destination | Prefix Length |
| --- | ---: |
| `192.168.0.0/16` | 16 |
| `192.168.10.0/24` | 24 |
| `192.168.10.128/25` | 25 |

For destination `192.168.10.150`, the `/25` route is selected because it is the most specific matching route.

### 🖆 Administrative Route Distance

Route distance determines the preference between competing routes to the same destination prefix.

In MikroTik RouterOS, a lower distance is preferred when otherwise comparable routes are available.

Example:

```routeros
/ip route add dst-address=192.168.20.0/24 gateway=10.0.12.2 distance=1
/ip route add dst-address=192.168.20.0/24 gateway=10.0.13.2 distance=10
```

The first route is preferred. The second route can be used as an alternative when the preferred route becomes unavailable.

Route distance is not the same as OSPF cost or BGP path selection attributes.

# 🗁 Lab Topology

### 🖆 Two-Router Static Routing Lab

Build a topology with two routers and two client networks.

<escape>```text
       LAN-A                         LAN-B
   192.168.10.0/24               192.168.20.0/24
          |                             |
        PC-A                           PC-B
          |                             |
     192.168.10.10                 192.168.20.10
          |                             |
     192.168.10.1                 192.168.20.1
          R1 -------- R2
          | 10.0.12.1/30 10.0.12.2/30 |
          |                             |

```</escape>

### 🖆 IP Addressing Plan

| Device | Interface | IP Address |
|---|---|---|
| R1 | ether1 | `192.168.10.1/24` |
| R1 | ether2 | `10.0.12.1/30` |
| R2 | ether1 | `10.0.12.2/30` |
| R2 | ether2 | `192.168.20.1/24` |
| PC-A | Ethernet | `192.168.10.10/24` |
| PC-B | Ethernet | `192.168.20.10/24` |

Default gateways:

- PC-A: `192.168.10.1`
- PC-B: `192.168.20.1`

The point-to-point network is `10.0.12.0/30`.

# 🗁 Exercise: Configure Interface IP Addresses

### 🖆 Configure R1

Assign the IP addresses to R1:

```routeros
/ip address
add address=192.168.10.1/24 interface=ether1
add address=10.0.12.1/30 interface=ether2
```

### 🖆 Configure R2

Assign the IP addresses to R2:

```routeros
/ip address
add address=10.0.12.2/30 interface=ether1
add address=192.168.20.1/24 interface=ether2
```

### 🖆 Verify Interface Addresses

On both routers:

```routeros
/ip address print
/interface print
```

Check that:

* Each interface has the correct IP address.
* The interfaces are enabled.
* The connected links are physically or virtually connected.

### 🖆 Verify Connected Routes

On R1:

```routeros
/ip route print
```

You should see connected routes for:

```text
192.168.10.0/24
10.0.12.0/30
```

On R2, you should see:

```text
10.0.12.0/30
192.168.20.0/24
```

Connected routes are created automatically when the corresponding interface and address are operational.

# 🗁 Exercise: Configure Static Routing

### 🖆 Understand the Routing Requirement

R1 knows about:

* `192.168.10.0/24`
* `10.0.12.0/30`

But R1 does not automatically know how to reach `192.168.20.0/24`.

Similarly, R2 does not automatically know how to reach `192.168.10.0/24`.

You must configure static routes on both routers.

### 🖆 Configure R1

```routeros
/ip route
add dst-address=192.168.20.0/24 gateway=10.0.12.2
```

This tells R1 to forward packets destined for `192.168.20.0/24` to R2.

### 🖆 Configure R2

```routeros
/ip route
add dst-address=192.168.10.0/24 gateway=10.0.12.1
```

This tells R2 to forward packets destined for `192.168.10.0/24` to R1.

### 🖆 Verify Static Routes

On both routers:

```routeros
/ip route print detail
```

Look for the configured destination networks and their gateways.

### 🖆 Test Connectivity Between Routers

From R1:

```routeros
/ping 10.0.12.2
```

From R2:

```routeros
/ping 10.0.12.1
```

Both tests should succeed if the point-to-point link is operational.

### 🖆 Test Connectivity Between LANs

From PC-A:

```bash
ping 192.168.20.10
```

From PC-B:

```bash
ping 192.168.10.10
```

Successful communication confirms that both routers have the required forward and return routes.

# 🗁 Exercise: Configure Default Routes

### 🖆 Understand Default Routing

A default route is used when the routing table does not contain a more specific matching route.

Its IPv4 destination is:

```text
0.0.0.0/0
```

For example, a router connected to an upstream router can send unknown destinations to that upstream router.

### 🖆 Configure a Default Route on R1

Assume R2 is acting as R1's upstream router:

```routeros
/ip route
add dst-address=0.0.0.0/0 gateway=10.0.12.2
```

### 🖆 Verify the Default Route

```routeros
/ip route print detail
```

Check the destination:

```text
0.0.0.0/0
```

### 🖆 Test Default Routing

From R1, test a destination that is not part of its directly connected networks:

```routeros
/ping 192.168.20.10
```

The packet should use the default route if no more specific matching route exists.

**Important:** A default route does not automatically provide Internet connectivity. The upstream router must also have a valid route toward the destination and a working return path.

# 🗁 Exercise: Understand Longest Prefix Match

### 🖆 Configure Multiple Routes

On R1, configure the following example routes using reachable next hops in your lab:

```routeros
/ip route
add dst-address=192.168.0.0/16 gateway=10.0.12.2
add dst-address=192.168.10.0/24 gateway=10.0.12.2
add dst-address=192.168.10.128/25 gateway=10.0.12.2
```

These routes intentionally use the same next hop to demonstrate prefix selection.

### 🖆 Analyze Route Selection

Consider these destination addresses:

| Destination IP | Selected Route |
| --- | --- |
| `192.168.5.10` | `192.168.0.0/16` |
| `192.168.10.50` | `192.168.10.0/24` |
| `192.168.10.150` | `192.168.10.128/25` |

### 🖆 Inspect the Routing Table

```routeros
/ip route print detail
```

On Ubuntu, you can inspect routes using:

```bash
ip route
```

To check the route selected for a particular destination:

```bash
ip route get 192.168.10.150
```

The operating system displays the selected route and outgoing interface information.

# 🗁 Exercise: Route Distance and Backup Routing

### 🖆 Understand Primary and Backup Routes

A router can have multiple routes to the same destination network.

The route with the lower distance is preferred when both routes are otherwise eligible.

For this exercise, use a three-router topology:

```text
             R2
            /  \
           /    \
         R1------R3
          |
        LAN-A
```

Use these example transit networks:

| Link | Network | First Router | Second Router |
| --- | --- | --- | --- |
| R1–R2 | `10.0.12.0/30` | R1: `10.0.12.1` | R2: `10.0.12.2` |
| R1–R3 | `10.0.13.0/30` | R1: `10.0.13.1` | R3: `10.0.13.2` |
| R2–R3 | `10.0.23.0/30` | R2: `10.0.23.1` | R3: `10.0.23.2` |

Assume the destination LAN behind R3 is:

```text
192.168.30.0/24
```

### 🖆 Configure the Primary Route on R1

```routeros
/ip route
add dst-address=192.168.30.0/24 gateway=10.0.12.2 distance=1
```

### 🖆 Configure the Backup Route on R1

```routeros
/ip route
add dst-address=192.168.30.0/24 gateway=10.0.13.2 distance=10
```

The first route is preferred when both routes are active.

### 🖆 Configure Return Routing

R3 must also know how to reach the source LAN behind R1.

For example, configure a route on R3 through R2:

```routeros
/ip route
add dst-address=192.168.10.0/24 gateway=10.0.23.1
```

If R3 needs to reach R1's LAN directly through its R1 link, configure the corresponding route through `10.0.13.1` instead.

### 🖆 Test Route Preference

Inspect the routes on R1:

```routeros
/ip route print detail
```

Test reachability to the destination:

```routeros
/ping 192.168.30.1
```

Then disable the primary route:

```routeros
/ip route disable [find dst-address=192.168.30.0/24 gateway=10.0.12.2]
```

Inspect the routing table again and test connectivity.

Restore the primary route:

```routeros
/ip route enable [find dst-address=192.168.30.0/24 gateway=10.0.12.2]
```

**Important:** Route distance provides preference; it does not guarantee end-to-end failure detection. A route can remain active even if a remote destination beyond its immediate gateway becomes unreachable. Reliable failover requires appropriate gateway monitoring or a routing protocol that can detect the failure.

# 🗁 Exercise: Basic Dynamic Routing with OSPF

### 🖆 Understand OSPF

OSPF (Open Shortest Path First) is a link-state dynamic routing protocol.

Routers running OSPF exchange routing information and calculate paths using the Shortest Path First algorithm.

In this optional exercise, use the same three-router topology and configure OSPF on RouterOS v7.

### 🖆 Create an OSPF Instance

On each router, configure an OSPF instance:

```routeros
/routing ospf instance
add name=ospf-v2 version=2 router-id=1.1.1.1
```

Use a unique router ID on each router.

For example:

| Router | Router ID |
| --- | --- |
| R1 | `1.1.1.1` |
| R2 | `2.2.2.2` |
| R3 | `3.3.3.3` |

### 🖆 Create an OSPF Area

On each router:

```routeros
/routing ospf area
add name=backbone area-id=0.0.0.0 instance=ospf-v2
```

### 🖆 Configure OSPF Interface Templates

On R1, advertise its transit links and LAN:

```routeros
/routing ospf interface-template
add area=backbone networks=10.0.12.0/30
add area=backbone networks=10.0.13.0/30
add area=backbone networks=192.168.10.0/24
```

On R2:

```routeros
/routing ospf interface-template
add area=backbone networks=10.0.12.0/30
add area=backbone networks=10.0.23.0/30
add area=backbone networks=192.168.20.0/24
```

On R3:

```routeros
/routing ospf interface-template
add area=backbone networks=10.0.13.0/30
add area=backbone networks=10.0.23.0/30
add area=backbone networks=192.168.30.0/24
```

These templates enable OSPF on matching interfaces and advertise the specified networks. Ensure the interface addressing and OSPF area configuration are consistent across the routers.

### 🖆 Verify OSPF Neighbors

On each router:

```routeros
/routing ospf neighbor print
```

Check that expected neighboring routers reach the `Full` state.

### 🖆 Verify Learned Routes

```routeros
/ip route print detail
```

Look for OSPF-learned routes.

### 🖆 Test Dynamic Routing

From a client in LAN-A:

```bash
ping 192.168.30.10
```

If the topology and OSPF configuration are correct, the routers should learn the remote LAN routes dynamically.

# 🗁 Linux Routing Practice

### 🖆 Display the Routing Table

On Ubuntu:

```bash
ip route
```

Example output:

```text
default via 192.168.10.1 dev enp0s3
192.168.10.0/24 dev enp0s3 proto kernel scope link src 192.168.10.10
```

### 🖆 Display the Selected Route

```bash
ip route get 192.168.20.10
```

This command shows which route and next hop Linux will use to reach the destination.

### 🖆 Add a Temporary Static Route

```bash
sudo ip route add 192.168.30.0/24 via 192.168.10.1
```

### 🖆 Delete a Static Route

```bash
sudo ip route del 192.168.30.0/24
```

These commands modify the active routing table. Routes added this way are generally temporary and do not survive a reboot unless configured through the system's network configuration.

### 🖆 Inspect the Route to a Remote Host

```bash
tracepath 192.168.20.10
```

Alternatively:

```bash
traceroute 192.168.20.10
```

Install the required utility if it is unavailable:

```bash
sudo apt update
sudo apt install traceroute
```

# 🗁 Routing Troubleshooting Exercises

### 🖆 Problem: Missing Static Route

**Symptoms:**

* A router can reach its directly connected networks.
* Remote networks are unreachable.
* The routing table does not contain a route to the destination.

**Investigation:**

```routeros
/ip route print detail
```

**Solution:**

Add the missing route with the correct destination prefix and next-hop address.

### 🖆 Problem: Incorrect Next Hop

**Symptoms:**

* The route exists.
* The next-hop address is incorrect or unreachable.
* Packets cannot reach the destination network.

**Investigation:**

```routeros
/ping 10.0.12.2
```

**Solution:**

Verify the transit network, interface addresses, and next-hop IP address.

### 🖆 Problem: Overlapping Networks

**Symptoms:**

* Unexpected route selection.
* Devices cannot communicate as expected.
* Routing entries overlap.

**Investigation:**

```routeros
/ip address print
/ip route print detail
```

**Solution:**

Use a non-overlapping IP addressing plan and correct the subnet masks.

### 🖆 Problem: Missing Return Route

**Symptoms:**

* The source sends packets successfully.
* The destination receives the request.
* Replies cannot return to the source.

**Investigation:**

Inspect the destination router's routing table and verify that it has a route to the source network.

**Solution:**

Configure the required return route or correct the routing protocol advertisement.

### 🖆 Problem: Interface Down

**Symptoms:**

* Connected routes are missing or inactive.
* The router cannot reach its directly connected neighbor.

**Investigation:**

```routeros
/interface print
/ip address print
```

**Solution:**

Check the interface state, virtual network connection, cabling, and IP configuration.

### 🖆 Problem: Forwarding Firewall Blocks Traffic

**Symptoms:**

* The routing table contains the correct route.
* The next hop is reachable.
* Traffic between networks is still blocked.

**Investigation:**

Check the router's forwarding firewall rules and connection tracking behavior.

**Solution:**

Correct the forwarding policy to permit the intended traffic.

# 🗁 Packet Capture and TTL Observation

### 🖆 Understand TTL

TTL (Time To Live) is an IPv4 header field that limits how many router hops a packet can traverse.

Each router normally decrements TTL by one. When TTL reaches zero, the router discards the packet and may send an ICMP Time Exceeded message.

IPv6 uses Hop Limit for the equivalent purpose.

### 🖆 Run Traceroute

On Ubuntu:

```bash
traceroute 192.168.20.10
```

Traceroute helps identify the intermediate routers along a path.

Some routers may not respond to traceroute probes because of filtering or rate limiting.

### 🖆 Capture Routing Traffic

On Ubuntu, install Wireshark and capture traffic on the relevant interface:

```bash
sudo apt update
sudo apt install wireshark
```

Useful display filters include:

```text
icmp
```

```text
ospf
```

```text
ip.ttl
```

Observe ICMP Time Exceeded messages and OSPF packets during the exercises.

# 🗁 Practice Challenges

### 🖆 Challenge: Three-Router Static Routing

Build a topology with three routers connected in a line:

```text
LAN-A --- R1 --- R2 --- R3 --- LAN-C
```

Requirements:

* Configure a unique LAN on each router.
* Configure point-to-point transit networks.
* Add static routes for remote LANs.
* Configure return routes.
* Verify end-to-end connectivity.

### 🖆 Challenge: Primary and Backup Path

Build two independent paths between R1 and R3.

Requirements:

* Configure a primary route.
* Configure a backup route with a higher distance.
* Verify route preference.
* Disable the primary path.
* Observe whether traffic uses the backup path.
* Restore the primary path.

### 🖆 Challenge: OSPF Network

Build a three-router topology.

Requirements:

* Configure unique router IDs.
* Create the backbone area.
* Enable OSPF on transit links.
* Advertise LAN networks.
* Verify OSPF neighbor relationships.
* Inspect dynamically learned routes.
* Test connectivity between all LANs.

### 🖆 Challenge: Route Selection

Configure multiple matching routes.

Requirements:

* Add a `/16` route.
* Add a more specific `/24` route.
* Add a `/25` route.
* Test several destination IP addresses.
* Identify which route is selected for each destination.

# 🗁 Knowledge Check

### 🖆 Questions

* What is the main purpose of a routing table?
* What is the difference between a connected route and a static route?
* What is a default route?
* What does `0.0.0.0/0` represent?
* What is longest prefix match?
* How does route distance influence route selection?
* Why is a return route necessary?
* What is the difference between static routing and dynamic routing?
* What is OSPF?
* Why does a router decrement TTL?
* Which Linux command displays the routing table?
* Which RouterOS command displays detailed route information?

### 🖆 Practical Verification Checklist

* [ ] Router interfaces have correct IP addresses.
* [ ] Connected routes appear in the routing table.
* [ ] Routers can reach their directly connected neighbors.
* [ ] Static routes are configured correctly.
* [ ] Default routes work as expected.
* [ ] Longest prefix match is understood and tested.
* [ ] Route distance and backup routing are verified.
* [ ] Return routes are configured.
* [ ] OSPF neighbors reach the Full state, if using OSPF.
* [ ] End-to-end connectivity is tested.
* [ ] Routing failures are diagnosed and corrected.

# 🗁 Expected Learning Outcome

After completing these exercises, you should understand how routers select paths, exchange routing information, and forward packets between different IP networks.

You should also be able to configure static and dynamic routing, verify route selection, implement basic backup routing, and troubleshoot common routing failures.

These skills provide the foundation for advanced topics such as OSPF optimization, BGP, route redistribution, policy-based routing, and ISP network design.
