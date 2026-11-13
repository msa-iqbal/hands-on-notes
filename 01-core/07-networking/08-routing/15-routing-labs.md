# Routing Labs

Routing labs provide practical experience with configuring, testing, and troubleshooting routers and routing protocols.

This document contains hands-on exercises covering static routing, default routing, dynamic routing, OSPF, BGP, ECMP, route redistribution, IPv6 routing, and routing troubleshooting.

The labs use Ubuntu/Linux, MikroTik RouterOS v7, and Cisco IOS-style configurations.

You can perform these exercises using:

* Physical routers and switches.
* Virtual machines running Ubuntu Server.
* MikroTik Cloud Hosted Router (CHR).
* GNS3.
* EVE-NG.
* Cisco Packet Tracer for supported Cisco routing exercises.

### 🖆 Lab Objectives

By completing these exercises, you should be able to:

* Configure IP addresses on router interfaces.
* Understand connected routes.
* Configure static and default routes.
* Configure routing between multiple networks.
* Understand route selection and administrative distance.
* Configure dynamic routing protocols.
* Establish OSPF and BGP neighbor relationships.
* Configure ECMP.
* Understand route redistribution.
* Configure IPv6 routing.
* Troubleshoot routing problems using command-line tools.
* Analyze routing tables and packet paths.

### 🖆 Lab Environment

| Tool | Purpose |
| --- | --- |
| Ubuntu Server | Linux routing and testing |
| MikroTik CHR | RouterOS routing configuration |
| GNS3 | Network topology simulation |
| EVE-NG | Multi-vendor network labs |
| FRRouting (FRR) | Dynamic routing on Linux |
| Wireshark | Packet inspection |
| tcpdump | Command-line packet capture |
| Cisco Packet Tracer | Cisco routing practice |

Use documentation-only IPv4 and IPv6 prefixes in isolated labs.

IPv4 documentation networks:

```text
192.0.2.0/24
198.51.100.0/24
203.0.113.0/24
```

IPv6 documentation prefix:

```text
2001:db8::/32
```

---

# 🗁 Lab Topology Overview

The following topologies will be used throughout the exercises.

### 🖆 Two-Router Topology

This topology is used for connected routes, static routing, and default routing.

```text
       LAN A                                  LAN B
   192.168.10.0/24                        192.168.20.0/24
        |                                      |
     +------+                              +------+
     | PC-A |                              | PC-B |
     +------+                              +------+
        |                                      |
     +------+       192.0.2.0/30           +------+
     | R1   |------------------------------| R2   |
     +------+                              +------+
   192.168.10.1                         192.168.20.1
   192.0.2.1                            192.0.2.2
```

### 🖆 Three-Router Topology

This topology is used for OSPF, route selection, ECMP, and redistribution.

```text
                     +------+
                     | R1   |
                     +------+
                       /  \
                      /    \
             +------+      +------+
             | R2   |------| R3   |
             +------+      +------+
                \             /
                 \           /
                  +---------+
                  | LANs    |
                  +---------+
```

### 🖆 Multi-Autonomous-System Topology

This topology is used for BGP.

```text
             AS 65001                    AS 65002
          +------------+              +------------+
          |            |              |            |
          |     R1     |--------------|     R2     |
          |            |              |            |
          +------------+              +------------+
               |                            |
          Internal LAN                 Internal LAN
```

---

# 🗁 Lab: Configure Router Interfaces

<badge color="success">Beginner</badge>

### 🖆 Objective

Configure IPv4 addresses on router interfaces and verify connected routes.

### 🖆 Topology

```text
 PC-A                     Router R1
  |                           |
  +---------------------------+
       192.168.10.0/24
```

### 🖆 Addressing Plan

| Device | Interface | IPv4 Address |
| --- | --- | --- |
| PC-A | eth0 | `192.168.10.10/24` |
| R1 | eth0 | `192.168.10.1/24` |

### 🖆 Ubuntu Configuration

Configure the router interface:

```bash
sudo ip addr add 192.168.10.1/24 dev eth0
sudo ip link set eth0 up
```

Configure the host:

```bash
sudo ip addr add 192.168.10.10/24 dev eth0
sudo ip link set eth0 up
```

Configure the host's default gateway:

```bash
sudo ip route add default via 192.168.10.1
```

### 🖆 Verify Configuration

Display interface addresses:

```bash
ip addr show
```

Display routing table:

```bash
ip route show
```

Test connectivity:

```bash
ping -c 4 192.168.10.1
```

### 🖆 Expected Result

* Both devices have the correct IP addresses.
* The router has a connected route for `192.168.10.0/24`.
* PC-A can communicate with R1.
* PC-A uses R1 as its default gateway.

---

# 🗁 Lab: Configure Connected Routes

<badge color="success">Beginner</badge>

### 🖆 Objective

Understand how routers automatically create connected routes when interface addresses are configured.

### 🖆 Topology

```text
             Router R1
          +--------------+
          |              |
          | eth0         | eth1
          +--+-----------+--+
             |           |
             |           |
       192.168.10.0/24  192.168.20.0/24
```

### 🖆 Addressing Plan

| Interface | IPv4 Address |
|---|---|
| eth0 | `192.168.10.1/24` |
| eth1 | `192.168.20.1/24` |

### 🖆 Configure Interfaces

```bash
sudo ip addr add 192.168.10.1/24 dev eth0
sudo ip addr add 192.168.20.1/24 dev eth1

sudo ip link set eth0 up
sudo ip link set eth1 up
```

### 🖆 Inspect Connected Routes

```bash
ip route show
```

Expected routes include:

```text
192.168.10.0/24 dev eth0 proto kernel scope link
192.168.20.0/24 dev eth1 proto kernel scope link
```

The exact metrics and output formatting may differ.

### 🖆 Inspect Local Routes

```bash
ip route show table local
```

Linux also creates local routes for the router's own assigned IP addresses.

### 🖆 Practical Tasks

* Assign another IPv4 address to `eth0`.
* Observe the routing table.
* Remove an interface address.
* Observe how the connected route changes.
* Compare connected routes with local host routes.

### 🖆 Expected Result

You should understand that connected routes are automatically installed based on configured interface addresses and prefixes.

---

# 🗁 Lab: Configure Static Routing

<badge color="success">Beginner</badge>

### 🖆 Objective

Configure static routes between two routers to enable communication between separate networks.

### 🖆 Topology

```text
 LAN A                 R1                  R2                 LAN B
192.168.10.0/24    192.0.2.1/30      192.0.2.2/30      192.168.20.0/24
    |                   |                  |                  |
  PC-A ---------------- R1 ---------------- R2 --------------- PC-B
```

### 🖆 Addressing Plan

| Device | Interface | IPv4 Address |
| --- | --- | --- |
| R1 | eth0 | `192.168.10.1/24` |
| R1 | eth1 | `192.0.2.1/30` |
| R2 | eth0 | `192.0.2.2/30` |
| R2 | eth1 | `192.168.20.1/24` |
| PC-A | eth0 | `192.168.10.10/24` |
| PC-B | eth0 | `192.168.20.10/24` |

### 🖆 Configure R1

Assign interface addresses:

```bash
sudo ip addr add 192.168.10.1/24 dev eth0
sudo ip addr add 192.0.2.1/30 dev eth1
```

Add a route to LAN B:

```bash
sudo ip route add 192.168.20.0/24 via 192.0.2.2
```

### 🖆 Configure R2

Assign interface addresses:

```bash
sudo ip addr add 192.0.2.2/30 dev eth0
sudo ip addr add 192.168.20.1/24 dev eth1
```

Add a route to LAN A:

```bash
sudo ip route add 192.168.10.0/24 via 192.0.2.1
```

### 🖆 Enable IPv4 Forwarding

On both routers:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

### 🖆 Configure End Devices

On PC-A:

```bash
sudo ip addr add 192.168.10.10/24 dev eth0
sudo ip route add default via 192.168.10.1
```

On PC-B:

```bash
sudo ip addr add 192.168.20.10/24 dev eth0
sudo ip route add default via 192.168.20.1
```

### 🖆 Verify Routing Tables

On R1:

```bash
ip route show
```

On R2:

```bash
ip route show
```

### 🖆 Test Connectivity

From PC-A:

```bash
ping -c 4 192.168.20.10
```

Trace the route:

```bash
traceroute -n 192.168.20.10
```

### 🖆 Expected Result

PC-A should successfully communicate with PC-B through R1 and R2.

### 🖆 Troubleshooting Tasks

Intentionally remove the route from R2:

```bash
sudo ip route del 192.168.10.0/24
```

Test connectivity again.

Observe the failure and restore the route:

```bash
sudo ip route add 192.168.10.0/24 via 192.0.2.1
```

---

# 🗁 Lab: Configure Default Routing

<badge color="success">Beginner</badge>

### 🖆 Objective

Configure a default route to forward unknown destinations toward an upstream router.

### 🖆 Topology

```text
         Internal Network
                |
             Router R1
                |
          192.0.2.0/30
                |
             Router R2
                |
           External Network
```

### 🖆 Addressing Plan

| Device | Interface | IPv4 Address |
| --- | --- | --- |
| R1 | eth0 | `192.168.10.1/24` |
| R1 | eth1 | `192.0.2.1/30` |
| R2 | eth0 | `192.0.2.2/30` |
| R2 | eth1 | `198.51.100.1/24` |

### 🖆 Configure R1

Configure its upstream default route:

```bash
sudo ip route add default via 192.0.2.2
```

### 🖆 Configure R2

Configure a return route toward the internal network:

```bash
sudo ip route add 192.168.10.0/24 via 192.0.2.1
```

### 🖆 Verify Default Route

```bash
ip route show default
```

Expected output:

```text
default via 192.0.2.2 dev eth1
```

### 🖆 Practical Tasks

* Remove the default route.
* Test an unknown destination.
* Restore the default route.
* Add a more specific route.
* Observe which route Linux selects for each destination.

### 🖆 Expected Result

R1 should forward destinations without a more specific route through R2.

---

# 🗁 Lab: Understand Longest Prefix Match

<badge color="success">Beginner</badge>

### 🖆 Objective

Understand how routers select the most specific matching route.

### 🖆 Configure Routes

On a Linux router:

```bash
sudo ip route add 10.0.0.0/8 via 192.0.2.1
sudo ip route add 10.10.0.0/16 via 192.0.2.2
sudo ip route add 10.10.10.0/24 via 192.0.2.3
```

These example next hops must be reachable for successful forwarding.

### 🖆 Test Route Selection

Check a destination inside the `/24`:

```bash
ip route get 10.10.10.50
```

Check a destination inside the `/16` but outside the `/24`:

```bash
ip route get 10.10.20.50
```

Check a destination matching only the `/8`:

```bash
ip route get 10.20.10.50
```

### 🖆 Expected Result

| Destination | Selected prefix |
| --- | --- |
| `10.10.10.50` | `10.10.10.0/24` |
| `10.10.20.50` | `10.10.0.0/16` |
| `10.20.10.50` | `10.0.0.0/8` |

The most specific matching prefix wins.

---

# 🗁 Lab: Configure Floating Static Routes

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure a primary static route and a backup route with a less-preferred administrative distance.

This lab uses MikroTik RouterOS v7.

### 🖆 Topology

```text
                 +----------------+
                 |                |
                 |    Router R1   |
                 |                |
                 +---+--------+---+
                     |        |
                     |        |
                  Primary   Backup
                   Link      Link
                     |        |
                  +--+--+  +--+--+
                  | R2  |  | R3  |
                  +-----+  +-----+
```

### 🖆 Configure Primary Route

```routeros
/ip route add dst-address=203.0.113.0/24 gateway=192.0.2.2 distance=1
```

### 🖆 Configure Backup Route

```routeros
/ip route add dst-address=203.0.113.0/24 gateway=198.51.100.2 distance=10
```

### 🖆 Verify Routes

```routeros
/ip route print detail
```

The primary route should be preferred when both paths are usable and the routes are otherwise comparable.

### 🖆 Test Failover

* Disable the primary path.
* Check the routing table.
* Observe whether the backup route becomes active.
* Restore the primary path.
* Observe route selection again.

### 🖆 Important Note

A higher distance alone does not guarantee failover if the primary route remains considered active while its remote path is actually broken.

Reliable failover may require recursive next-hop resolution, route monitoring, Netwatch, BFD, or another suitable reachability mechanism.

---

# 🗁 Lab: Configure OSPF Routing with FRRouting

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure OSPF on Linux routers using FRRouting (FRR).

This lab demonstrates dynamic route exchange between two routers.

### 🖆 Topology

```text
 LAN A                  R1                 R2                  LAN B
10.10.1.0/24       192.0.2.1/30      192.0.2.2/30        10.10.2.0/24
    |                   |                  |                    |
    +-------------------R1------------------R2-------------------+
```

### 🖆 Addressing Plan

| Device | Interface | IPv4 Address |
| --- | --- | --- |
| R1 | eth0 | `10.10.1.1/24` |
| R1 | eth1 | `192.0.2.1/30` |
| R2 | eth0 | `192.0.2.2/30` |
| R2 | eth1 | `10.10.2.1/24` |

### 🖆 Install FRRouting

On both Ubuntu routers:

```bash
sudo apt update
sudo apt install frr frr-pythontools
```

Enable the OSPF daemon.

Edit:

```bash
sudo nano /etc/frr/daemons
```

Set:

```ini
ospfd=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

### 🖆 Configure R1

Enter the FRR configuration shell:

```bash
sudo vtysh
```

Configure OSPF:

```text
configure terminal

router ospf
 ospf router-id 1.1.1.1
 network 10.10.1.0/24 area 0
 network 192.0.2.0/30 area 0

end
write memory
```

### 🖆 Configure R2

```bash
sudo vtysh
```

Configure:

```text
configure terminal

router ospf
 ospf router-id 2.2.2.2
 network 10.10.2.0/24 area 0
 network 192.0.2.0/30 area 0

end
write memory
```

### 🖆 Verify OSPF Neighbors

On either router:

```text
show ip ospf neighbor
```

Expected state:

```text
Full
```

The exact neighbor output depends on the topology and FRR version.

### 🖆 Verify OSPF Routes

```text
show ip route ospf
```

R1 should learn the remote LAN route:

```text
10.10.2.0/24
```

R2 should learn:

```text
10.10.1.0/24
```

### 🖆 Test Connectivity

From R1:

```bash
ping -c 4 10.10.2.1
```

### 🖆 Practical Tasks

* Change the OSPF router ID.
* Change the OSPF interface cost.
* Configure a passive interface for a LAN.
* Disable the router-to-router link.
* Observe neighbor-state changes.
* Restore the link and verify convergence.

### 🖆 Expected Result

Both routers should establish an OSPF adjacency and exchange their connected LAN prefixes.

---

# 🗁 Lab: Configure OSPF on MikroTik RouterOS v7

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure OSPFv2 between two MikroTik routers.

### 🖆 Topology

```text
             R1 ---------------- R2
          192.0.2.1          192.0.2.2
             |                   |
        10.10.1.0/24         10.10.2.0/24
```

### 🖆 Configure R1

Configure interface addresses:

```routeros
/ip address add address=192.0.2.1/30 interface=ether1
/ip address add address=10.10.1.1/24 interface=ether2
```

Create an OSPF instance:

```routeros
/routing ospf instance add name=ospf-v2 version=2 router-id=1.1.1.1
```

Create an area:

```routeros
/routing ospf area add name=backbone area-id=0.0.0.0 instance=ospf-v2
```

Create an interface template:

```routeros
/routing ospf interface-template add area=backbone networks=192.0.2.0/30
```

Advertise the LAN:

```routeros
/routing ospf interface-template add area=backbone networks=10.10.1.0/24 passive
```

### 🖆 Configure R2

Configure interface addresses:

```routeros
/ip address add address=192.0.2.2/30 interface=ether1
/ip address add address=10.10.2.1/24 interface=ether2
```

Create the OSPF instance:

```routeros
/routing ospf instance add name=ospf-v2 version=2 router-id=2.2.2.2
```

Create the backbone area:

```routeros
/routing ospf area add name=backbone area-id=0.0.0.0 instance=ospf-v2
```

Create interface templates:

```routeros
/routing ospf interface-template add area=backbone networks=192.0.2.0/30
/routing ospf interface-template add area=backbone networks=10.10.2.0/24 passive
```

### 🖆 Verify OSPF

Check OSPF neighbors:

```routeros
/routing ospf neighbor print
```

Check OSPF routes:

```routeros
/ip route print where ospf
```

### 🖆 Practical Tasks

* Verify the neighbor relationship.
* Change the router ID.
* Change interface costs.
* Disable the OSPF interface template.
* Restore the configuration.
* Verify learned routes.

The exact RouterOS v7 menu behavior and available properties can vary by release. Verify configuration against the installed RouterOS version.

---

# 🗁 Lab: Configure OSPF on Cisco IOS

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure OSPFv2 between Cisco routers.

### 🖆 Topology

```text
               192.0.2.0/30
         +----------------------+
         |                      |
      +------+                +------+
      | R1   |----------------| R2   |
      +------+                +------+
         |                      |
   10.10.1.0/24           10.10.2.0/24
```

### 🖆 Configure R1

```cisco
configure terminal

interface GigabitEthernet0/0
 ip address 192.0.2.1 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 ip address 10.10.1.1 255.255.255.0
 no shutdown

router ospf 1
 router-id 1.1.1.1
 network 192.0.2.0 0.0.0.3 area 0
 network 10.10.1.0 0.0.0.255 area 0
```

### 🖆 Configure R2

```cisco
configure terminal

interface GigabitEthernet0/0
 ip address 192.0.2.2 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 ip address 10.10.2.1 255.255.255.0
 no shutdown

router ospf 1
 router-id 2.2.2.2
 network 192.0.2.0 0.0.0.3 area 0
 network 10.10.2.0 0.0.0.255 area 0
```

### 🖆 Verify OSPF

```cisco
show ip ospf neighbor
show ip route ospf
show ip ospf interface brief
show ip protocols
```

### 🖆 Expected Result

* R1 and R2 establish an OSPF adjacency.
* R1 learns `10.10.2.0/24`.
* R2 learns `10.10.1.0/24`.

---

# 🗁 Lab: Configure BGP Routing with FRRouting

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure an eBGP session between two autonomous systems using FRRouting.

### 🖆 Topology

```text
          AS 65001                     AS 65002

       +----------+                 +----------+
       |    R1    |-----------------|    R2    |
       +----------+                 +----------+
          |                              |
    10.10.1.0/24                   10.10.2.0/24

       192.0.2.1/30                192.0.2.2/30
```

### 🖆 Addressing Plan

| Device | Interface | Address |
| --- | --- | --- |
| R1 | eth0 | `192.0.2.1/30` |
| R1 | eth1 | `10.10.1.1/24` |
| R2 | eth0 | `192.0.2.2/30` |
| R2 | eth1 | `10.10.2.1/24` |

### 🖆 Install FRRouting

On both routers:

```bash
sudo apt update
sudo apt install frr frr-pythontools
```

Enable the BGP daemon:

```bash
sudo nano /etc/frr/daemons
```

Set:

```ini
bgpd=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

### 🖆 Configure R1

```bash
sudo vtysh
```

Configure:

```text
configure terminal

router bgp 65001
 bgp router-id 1.1.1.1
 neighbor 192.0.2.2 remote-as 65002
 network 10.10.1.0/24

end
write memory
```

### 🖆 Configure R2

```bash
sudo vtysh
```

Configure:

```text
configure terminal

router bgp 65002
 bgp router-id 2.2.2.2
 neighbor 192.0.2.1 remote-as 65001
 network 10.10.2.0/24

end
write memory
```

### 🖆 Verify BGP Neighbors

On R1:

```text
show bgp ipv4 unicast summary
```

On R2:

```text
show bgp ipv4 unicast summary
```

The BGP session should reach the `Established` state.

### 🖆 Verify BGP Routes

```text
show bgp ipv4 unicast
```

Inspect the routing table:

```text
show ip route bgp
```

### 🖆 Test Connectivity

From R1:

```bash
ping -c 4 10.10.2.1
```

For successful end-to-end communication, ensure that forwarding, firewall policies, and return routing are configured correctly.

### 🖆 Practical Tasks

* Change the local AS number.
* Configure a BGP neighbor password.
* Advertise another network.
* Withdraw a network advertisement.
* Inspect the AS_PATH attribute.
* Configure a prefix filter.
* Observe the effect of an incorrect remote AS.

### 🖆 Expected Result

Both routers should establish an eBGP session and exchange their advertised IPv4 prefixes.

---

# 🗁 Lab: Configure BGP on MikroTik RouterOS v7

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure an eBGP session between two MikroTik routers.

### 🖆 Topology

```text
        AS 65001                     AS 65002

       +------+                     +------+
       | R1   |---------------------| R2   |
       +------+                     +------+
       192.0.2.1                   192.0.2.2
```

### 🖆 Configure R1

Assign the interface address:

```routeros
/ip address add address=192.0.2.1/30 interface=ether1
```

Create a routing filter:

```routeros
/routing filter rule add chain=bgp-out rule="if (dst == 10.10.1.0/24) { accept }"
```

Create the BGP connection:

```routeros
/routing bgp connection add name=to-R2 local.role=ebgp local.as=65001 remote.address=192.0.2.2 remote.as=65002 output.filter-chain=bgp-out
```

Configure the route to be advertised:

```routeros
/ip route add dst-address=10.10.1.0/24 blackhole
```

This blackhole route is a lab technique to make the exact prefix present in the routing table. In a real deployment, advertise a legitimately reachable network and use appropriate filtering.

### 🖆 Configure R2

Assign the interface address:

```routeros
/ip address add address=192.0.2.2/30 interface=ether1
```

Create the BGP connection:

```routeros
/routing bgp connection add name=to-R1 local.role=ebgp local.as=65002 remote.address=192.0.2.1 remote.as=65001
```

### 🖆 Verify BGP

Inspect connections:

```routeros
/routing bgp connection print detail
```

Inspect BGP sessions:

```routeros
/routing bgp session print
```

Inspect received routes:

```routeros
/routing route print
```

### 🖆 Practical Tasks

* Verify that the BGP session is established.
* Advertise another prefix.
* Configure an output filter.
* Inspect received BGP routes.
* Disable the peer and observe route withdrawal.

The exact RouterOS v7 BGP properties and filter syntax should be checked against the installed release.

---

# 🗁 Lab: Configure ECMP Routing

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure equal-cost multipath routing with two next hops.

### 🖆 Topology

```text
                 +----------+
                 |   R1     |
                 +----------+
                    /    \
                   /      \
              +------+  +------+
              | R2   |  | R3   |
              +------+  +------+
                   \      /
                    \    /
                 Destination
                  Network
```

### 🖆 Linux ECMP Configuration

Configure two equal-cost paths:

```bash
sudo ip route add 203.0.113.0/24 \
  nexthop via 192.0.2.2 dev eth0 \
  nexthop via 198.51.100.2 dev eth1
```

Inspect the route:

```bash
ip route show 203.0.113.0/24
```

Inspect route selection for a destination:

```bash
ip route get 203.0.113.10
```

### 🖆 Test Traffic Distribution

Generate multiple flows toward different destination or source combinations.

Observe interface traffic:

```bash
ip -s link show eth0
ip -s link show eth1
```

Capture packets:

```bash
sudo tcpdump -ni eth0
```

```bash
sudo tcpdump -ni eth1
```

### 🖆 Practical Tasks

* Remove one next hop.
* Observe traffic behavior.
* Restore the next hop.
* Compare per-flow and per-packet load distribution conceptually.
* Investigate why equal-cost paths may not carry equal traffic volumes.

### 🖆 Expected Result

Linux should install an eligible multipath route and distribute flows according to its configured routing and hashing behavior.

A single flow commonly remains on one path to avoid packet reordering.

---

# 🗁 Lab: Configure Route Redistribution

<badge color="danger">Advanced</badge>

### 🖆 Objective

Understand how routes learned from one routing protocol can be advertised through another routing protocol.

### 🖆 Topology

```text
        OSPF Domain                  RIP Domain

       +----------+                +----------+
       |   R1     |----------------|   R2     |
       +----------+                +----------+
            |                           |
        OSPF LAN                    RIP LAN

             Redistribution
             between protocols
```

### 🖆 Lab Requirements

* Configure OSPF on one side.
* Configure RIP on the other side.
* Enable redistribution on the boundary router.
* Assign appropriate metrics.
* Filter redistributed prefixes.
* Prevent routing feedback.

### 🖆 Example Cisco IOS Configuration

On the boundary router, redistribute OSPF into RIP:

```cisco
router rip
 version 2
 redistribute ospf 1 metric 2
```

Redistribute RIP into OSPF:

```cisco
router ospf 1
 redistribute rip subnets metric 20 metric-type 2
```

These examples illustrate redistribution commands. A production design must also account for route filtering, metric assignment, and loop prevention.

### 🖆 Verify Redistribution

```cisco
show ip route
show ip protocols
show ip ospf database external
```

### 🖆 Practical Tasks

* Advertise an OSPF route into RIP.
* Advertise a RIP route into OSPF.
* Remove the redistribution metric and observe the behavior.
* Add route filters.
* Configure route tags to help prevent feedback.
* Verify that routes do not circulate indefinitely.

### 🖆 Expected Result

Routes from one routing domain should become available in the other domain according to the redistribution policy.

---

# 🗁 Lab: Configure IPv6 Routing on Linux

<badge color="success">Beginner</badge>

### 🖆 Objective

Configure IPv6 addresses, connected routes, static routes, and default routing.

### 🖆 Topology

```text
             Router R1
                  |
           2001:db8:1::/64
                  |
             Router R2
                  |
           2001:db8:2::/64
```

### 🖆 Configure IPv6 Addresses

On R1:

```bash
sudo ip -6 addr add 2001:db8:1::1/64 dev eth0
```

On R2:

```bash
sudo ip -6 addr add 2001:db8:1::2/64 dev eth0
sudo ip -6 addr add 2001:db8:2::1/64 dev eth1
```

### 🖆 Enable IPv6 Forwarding

On R2:

```bash
sudo sysctl -w net.ipv6.conf.all.forwarding=1
```

### 🖆 Configure Static Routing

On R1:

```bash
sudo ip -6 route add 2001:db8:2::/64 via 2001:db8:1::2
```

### 🖆 Verify Routing

```bash
ip -6 route show
```

Test connectivity:

```bash
ping -6 2001:db8:2::1
```

### 🖆 Practical Tasks

* Configure a default IPv6 route.
* Configure a link-local next hop.
* Inspect the NDP neighbor table.
* Disable forwarding and test again.
* Restore forwarding and verify connectivity.

---

# 🗁 Lab: Configure OSPFv3 with FRRouting

<badge color="danger">Advanced</badge>

### 🖆 Objective

Configure OSPFv3 between two Linux routers using FRRouting.

### 🖆 Topology

```text
             R1 ---------------- R2
          2001:db8:1::1      2001:db8:1::2
                |                 |
         2001:db8:10::/64   2001:db8:20::/64
```

### 🖆 Configure IPv6 Addresses

On R1:

```bash
sudo ip -6 addr add 2001:db8:1::1/64 dev eth0
sudo ip -6 addr add 2001:db8:10::1/64 dev eth1
```

On R2:

```bash
sudo ip -6 addr add 2001:db8:1::2/64 dev eth0
sudo ip -6 addr add 2001:db8:20::1/64 dev eth1
```

### 🖆 Enable OSPFv3 in FRR

Edit the FRR daemon configuration:

```bash
sudo nano /etc/frr/daemons
```

Enable:

```ini
ospf6d=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

### 🖆 Configure R1

Enter FRR:

```bash
sudo vtysh
```

Configure:

```text
configure terminal

router ospf6
 ospf6 router-id 1.1.1.1
 interface eth0 area 0.0.0.0
 interface eth1 area 0.0.0.0

end
write memory
```

### 🖆 Configure R2

```bash
sudo vtysh
```

Configure:

```text
configure terminal

router ospf6
 ospf6 router-id 2.2.2.2
 interface eth0 area 0.0.0.0
 interface eth1 area 0.0.0.0

end
write memory
```

### 🖆 Verify OSPFv3

Inspect neighbors:

```text
show ipv6 ospf6 neighbor
```

Inspect IPv6 routes:

```text
show ipv6 route ospf6
```

Inspect the OSPFv3 database:

```text
show ipv6 ospf6 database
```

### 🖆 Expected Result

R1 and R2 should establish an OSPFv3 adjacency over the inter-router link and exchange their IPv6 LAN prefixes.

The exact FRR OSPFv3 command syntax can vary by release. Consult the installed FRR documentation if a command is unavailable.

---

# 🗁 Lab: Configure IPv6 Static Routing on MikroTik

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Configure IPv6 addresses and static routes on two MikroTik RouterOS v7 routers.

### 🖆 Topology

```text
            R1 ---------------- R2
       2001:db8:1::1       2001:db8:1::2
            |                   |
       2001:db8:10::/64    2001:db8:20::/64
```

### 🖆 Configure R1

```routeros
/ipv6 address add address=2001:db8:1::1/64 interface=ether1
/ipv6 address add address=2001:db8:10::1/64 interface=ether2
```

Add a route to the remote LAN:

```routeros
/ipv6 route add dst-address=2001:db8:20::/64 gateway=2001:db8:1::2
```

### 🖆 Configure R2

```routeros
/ipv6 address add address=2001:db8:1::2/64 interface=ether1
/ipv6 address add address=2001:db8:20::1/64 interface=ether2
```

Add the return route:

```routeros
/ipv6 route add dst-address=2001:db8:10::/64 gateway=2001:db8:1::1
```

### 🖆 Verify Configuration

```routeros
/ipv6 address print
/ipv6 route print
/ipv6 neighbor print
```

### 🖆 Test Connectivity

```routeros
/ping 2001:db8:1::2
```

Test communication with a host on the remote LAN.

---

# 🗁 Lab: Troubleshoot Routing Problems

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Practice identifying common routing problems using Linux troubleshooting tools.

### 🖆 Scenario

Two networks cannot communicate:

```text
 LAN A                  R1                 R2                 LAN B
192.168.10.0/24    192.0.2.1/30      192.0.2.2/30      192.168.20.0/24
```

### 🖆 Troubleshooting Checklist

Start with interface configuration:

```bash
ip addr show
```

Check interface state:

```bash
ip link show
```

Check connected routes:

```bash
ip route show
```

Check the route to the destination:

```bash
ip route get 192.168.20.10
```

Check neighbor resolution:

```bash
ip neigh show
```

Check forwarding:

```bash
sysctl net.ipv4.ip_forward
```

Test the next hop:

```bash
ping -c 4 192.0.2.2
```

Trace the route:

```bash
traceroute -n 192.168.20.10
```

Inspect firewall rules:

```bash
sudo nft list ruleset
```

Capture packets:

```bash
sudo tcpdump -ni any 'arp or icmp'
```

### 🖆 Troubleshooting Exercises

Intentionally introduce the following problems:

| Problem | Expected symptom | Investigation |
| --- | --- | --- |
| Incorrect interface IP | Local communication fails | Inspect `ip addr` |
| Wrong subnet mask | Incorrect connected route | Inspect interface prefixes |
| Missing static route | Remote network unreachable | Inspect routing table |
| Missing return route | One-way communication | Inspect reverse path |
| Disabled forwarding | Router does not forward packets | Check forwarding sysctl |
| Failed ARP resolution | Neighbor unreachable | Inspect `ip neigh` |
| Incorrect default gateway | External networks unreachable | Inspect default route |
| Firewall blocks forwarding | Route exists but traffic fails | Inspect nftables |
| Incorrect next hop | Route cannot forward traffic | Check next-hop reachability |
| Routing loop | TTL expires | Use traceroute |

### 🖆 Expected Result

You should be able to identify whether a routing failure originates from addressing, interface state, route selection, neighbor resolution, forwarding, or firewall policy.

---

# 🗁 Lab: Analyze Routing with Packet Capture

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Understand how packets move through routers and observe routing-related network traffic.

### 🖆 Capture ICMP Traffic

```bash
sudo tcpdump -ni any icmp
```

Generate traffic:

```bash
ping -c 4 192.0.2.2
```

### 🖆 Capture ARP Traffic

```bash
sudo tcpdump -ni eth0 arp
```

Observe ARP requests and replies.

### 🖆 Capture OSPF Traffic

```bash
sudo tcpdump -ni eth0 'ip proto 89'
```

Observe OSPF packets such as Hello messages.

### 🖆 Capture BGP Traffic

```bash
sudo tcpdump -ni eth0 'tcp port 179'
```

Observe the BGP TCP session.

### 🖆 Capture IPv6 Routing Traffic

Capture ICMPv6:

```bash
sudo tcpdump -ni eth0 icmp6
```

Capture OSPFv3:

```bash
sudo tcpdump -ni eth0 'ip6 proto 89'
```

### 🖆 Practical Tasks

* Capture a ping between two networks.
* Identify source and destination IP addresses.
* Observe TTL changes across router hops.
* Capture OSPF Hello packets.
* Capture BGP session establishment.
* Identify ICMP Destination Unreachable messages.
* Observe IPv6 Neighbor Solicitation and Advertisement messages.

### 🖆 Expected Result

You should be able to relate routing-table decisions to the actual packets observed on network interfaces.

---

# 🗁 Lab: Build a Multi-Router Network

<badge color="danger">Advanced</badge>

### 🖆 Objective

Build a network containing multiple routers, multiple LANs, and dynamic routing.

### 🖆 Topology

```text
                 +---------+
                 |   R1    |
                 +---------+
                   /     \
                  /       \
             +------+   +------+
             | R2   |   | R3   |
             +------+   +------+
                  \       /
                   \     /
                 +---------+
                 |   R4    |
                 +---------+
                      |
                  Server LAN
```

### 🖆 Addressing Plan

| Network | Prefix |
| --- | --- |
| R1–R2 | `192.0.2.0/30` |
| R1–R3 | `192.0.2.4/30` |
| R2–R4 | `192.0.2.8/30` |
| R3–R4 | `192.0.2.12/30` |
| LAN A | `10.10.1.0/24` |
| LAN B | `10.10.2.0/24` |
| LAN C | `10.10.3.0/24` |
| Server LAN | `10.10.4.0/24` |

### 🖆 Configuration Requirements

* Configure all router interfaces.
* Configure LAN gateway addresses.
* Enable IP forwarding.
* Configure OSPF area 0.
* Advertise all connected networks.
* Configure router IDs.
* Verify neighbor relationships.
* Verify routing tables.
* Test connectivity between all LANs.

### 🖆 Advanced Tasks

* Change OSPF interface costs.
* Create an alternate path.
* Disable a router-to-router link.
* Observe OSPF convergence.
* Configure ECMP where paths have equal cost.
* Configure a default route from R1.
* Advertise the default route through OSPF.
* Capture OSPF packets.
* Investigate route selection after a link failure.

### 🖆 Expected Result

All LANs should communicate through dynamically learned routes.

When a link fails, OSPF should recalculate paths where an alternate route exists.

---

# 🗁 Lab: Configure Routing Between VLANs

<badge color="warning">Intermediate</badge>

### 🖆 Objective

Understand how routing enables communication between different VLANs.

### 🖆 Topology

```text
                   +-------------+
                   |   Router    |
                   +-------------+
                          |
                       Trunk
                          |
                   +-------------+
                   |   Switch    |
                   +-------------+
                    /           \
                   /             \
              VLAN 10           VLAN 20
             192.168.10.0/24   192.168.20.0/24
                  |                 |
                PC-A              PC-B
```

### 🖆 VLAN Addressing Plan

| VLAN | Network | Gateway |
| --- | --- | --- |
| VLAN 10 | `192.168.10.0/24` | `192.168.10.1` |
| VLAN 20 | `192.168.20.0/24` | `192.168.20.1` |

### 🖆 Configuration Requirements

* Create VLAN 10 and VLAN 20.
* Assign access ports to the correct VLANs.
* Configure a trunk between the switch and router.
* Configure router subinterfaces or Layer 3 VLAN interfaces.
* Assign gateway addresses.
* Configure firewall policies if required.
* Test communication between VLANs.

### 🖆 Expected Result

Devices in different VLANs should communicate through the router when routing and security policies permit the traffic.

---

# 🗁 Lab: Configure Routing Failover

<badge color="danger">Advanced</badge>

### 🖆 Objective

Build a network with two upstream paths and configure backup routing.

### 🖆 Topology

```text
                  Internal Network
                         |
                      Router R1
                       /     \
                      /       \
                 Primary     Backup
                   R2          R3
                    |           |
                    +-----+-----+
                          |
                     Upstream
```

### 🖆 Configuration Requirements

* Configure a primary route.
* Configure a backup route.
* Assign appropriate route distances.
* Configure a reachability-monitoring mechanism.
* Simulate a primary link failure.
* Verify traffic moves to the backup path.
* Restore the primary link.
* Verify route preference returns to the primary path.

### 🖆 Linux Monitoring Example

A simple connectivity check can be performed with:

```bash
ping -c 3 192.0.2.2
```

For production-grade monitoring, use appropriate routing mechanisms such as BFD, dynamic routing, or a controlled monitoring and route-management system.

### 🖆 Expected Result

Traffic should use the backup path when the primary route becomes unavailable according to the configured failure-detection mechanism.

---

# 🗁 Lab: Routing Verification and Documentation

<badge color="success">Beginner</badge>

### 🖆 Objective

Develop a consistent workflow for documenting and verifying routing configurations.

### 🖆 Verification Commands

| Task | Linux | MikroTik RouterOS |
| --- | --- | --- |
| Display IPv4 addresses | `ip addr` | `/ip address print` |
| Display IPv4 routes | `ip route` | `/ip route print` |
| Display IPv6 addresses | `ip -6 addr` | `/ipv6 address print` |
| Display IPv6 routes | `ip -6 route` | `/ipv6 route print` |
| Display neighbors | `ip neigh` | `/ip arp print` |
| Test connectivity | `ping` | `/ping` |
| Trace route | `traceroute` | `/tool traceroute` |
| Inspect interfaces | `ip link` | `/interface print` |

### 🖆 Routing Documentation Template

For each router, record:

| Field | Description |
| --- | --- |
| Router name | Unique device name |
| Router ID | Routing protocol identifier |
| Interface | Interface name |
| IP address | Interface address and prefix |
| Connected networks | Directly attached prefixes |
| Static routes | Manually configured routes |
| Default route | Upstream gateway |
| Routing protocols | OSPF, BGP, RIP, IS-IS |
| Neighbors | Established routing peers |
| Route preference | Distance or administrative preference |
| Metrics | Protocol or route metrics |
| Firewall policy | Permitted forwarding traffic |

### 🖆 Practical Tasks

* Document the complete multi-router topology.
* Record all interface addresses.
* Export routing tables.
* Record routing protocol neighbors.
* Document primary and backup paths.
* Save configuration backups.
* Record troubleshooting findings.

---

# 🗁 Practical Exercises

Complete these exercises after finishing the individual labs.

### 🖆 Static Routing Exercises

* [ ] Configure two routers connected through a point-to-point network.
* [ ] Connect two LANs through static routes.
* [ ] Configure a default route.
* [ ] Configure a backup static route.
* [ ] Verify route selection.
* [ ] Troubleshoot a missing return route.

### 🖆 Dynamic Routing Exercises

* [ ] Configure OSPF between two routers.
* [ ] Configure OSPF across three routers.
* [ ] Configure OSPF areas.
* [ ] Configure passive interfaces.
* [ ] Change OSPF costs.
* [ ] Configure eBGP between two autonomous systems.
* [ ] Advertise and withdraw BGP prefixes.
* [ ] Configure routing filters.
* [ ] Configure RIP or RIPng.
* [ ] Configure route redistribution.

### 🖆 Advanced Routing Exercises

* [ ] Configure ECMP.
* [ ] Configure floating static routes.
* [ ] Configure IPv6 static routing.
* [ ] Configure OSPFv3.
* [ ] Configure IPv6 default routing.
* [ ] Configure inter-VLAN routing.
* [ ] Build a multi-router network.
* [ ] Configure upstream failover.
* [ ] Capture and analyze routing protocol packets.
* [ ] Troubleshoot routing loops and black holes.

---

# 🗁 Common Lab Mistakes

### 🖆 Incorrect IP Addressing

Assigning an incorrect IP address or prefix can create incorrect connected routes.

Always verify interface addresses and subnet boundaries.

### 🖆 Missing Return Routes

A router may know how to reach the destination but not how to return traffic to the source.

Verify both forward and reverse paths.

### 🖆 Disabled IP Forwarding

A Linux system with IP forwarding disabled cannot normally route packets between interfaces.

Verify:

```bash
sysctl net.ipv4.ip_forward
```

For IPv6:

```bash
sysctl net.ipv6.conf.all.forwarding
```

### 🖆 Incorrect Next Hop

A static route may point to a next-hop address that is unreachable.

Verify next-hop connectivity and interface selection.

### 🖆 Routing Protocol Mismatch

OSPF or BGP neighbors may fail to establish because of incorrect configuration.

Check:

* Router IDs.
* Area IDs.
* Autonomous system numbers.
* Authentication.
* Interface addresses.
* Network reachability.
* Protocol filtering.

### 🖆 Incorrect Route Advertisement

A routing protocol may advertise prefixes that are not intended to be reachable.

Use prefix filters, route maps, routing policies, and explicit advertisement controls.

### 🖆 Firewall Blocking Routing Traffic

A firewall can block forwarding traffic or routing protocol messages.

For example:

* OSPF uses IP protocol 89.
* BGP uses TCP port 179.
* RIP uses UDP port 520.
* RIPng uses UDP port 521.

Allow the required traffic according to the intended topology and security policy.

---

# 🗁 Routing Lab Completion Checklist

Use this checklist to evaluate your practical progress.

### 🖆 Routing Fundamentals

* [ ] Understand connected routes.
* [ ] Understand static routes.
* [ ] Understand default routes.
* [ ] Understand longest prefix match.
* [ ] Understand route preference and metrics.
* [ ] Understand administrative distance.

### 🖆 Dynamic Routing

* [ ] Configure RIP.
* [ ] Configure OSPF.
* [ ] Configure BGP.
* [ ] Understand OSPF areas.
* [ ] Understand BGP autonomous systems.
* [ ] Configure route redistribution.
* [ ] Understand route filtering.

### 🖆 Advanced Routing

* [ ] Configure ECMP.
* [ ] Configure route failover.
* [ ] Understand policy-based routing.
* [ ] Understand route aggregation.
* [ ] Configure IPv6 routing.
* [ ] Configure OSPFv3.
* [ ] Troubleshoot routing loops.
* [ ] Analyze routing protocol packets.

### 🖆 Platform Skills

* [ ] Configure Linux routing.
* [ ] Configure MikroTik RouterOS v7 routing.
* [ ] Configure Cisco IOS routing.
* [ ] Use FRRouting for dynamic routing.
* [ ] Use GNS3 or EVE-NG for network simulations.
* [ ] Use Wireshark and tcpdump for packet analysis.

---

# 🗁 Summary

These routing labs provide hands-on experience with routing concepts and practical configurations.

You have practiced:

* Interface addressing and connected routes.
* Static and default routing.
* Longest Prefix Match.
* Floating static routes.
* OSPF and OSPFv3.
* BGP and eBGP.
* RIP and RIPng concepts.
* ECMP.
* Route redistribution.
* IPv6 routing.
* Inter-VLAN routing.
* Routing failover.
* Packet capture and troubleshooting.

The most important skill is not simply configuring routes. It is understanding how a router selects a path, forwards packets, learns new routes, handles failures, and maintains connectivity across multiple networks.
