# Open Shortest Path First (OSPF)

Open Shortest Path First (OSPF) is a link-state Interior Gateway Protocol (IGP) used to exchange routing information within an autonomous system.

OSPF enables routers to dynamically discover network topology, calculate optimal paths, and update routing information when network conditions change.

It uses the Shortest Path First (SPF) algorithm, also known as Dijkstra's algorithm, to calculate the shortest paths to destination networks.

OSPF is commonly used in enterprise networks, data centers, campus networks, and service-provider infrastructure.

Key characteristics:

* Link-state routing protocol.
* Interior Gateway Protocol.
* Uses Dijkstra's SPF algorithm.
* Uses cost as its routing metric.
* Supports hierarchical network design through areas.
* Supports IPv4 and IPv6 through different OSPF versions.
* Supports authentication.
* Uses multicast for neighbor discovery and adjacency maintenance.
* Supports equal-cost multipath routing.

# 🗁 What Is OSPF?

OSPF is a dynamic routing protocol that allows routers to exchange information about network topology.

Unlike distance-vector protocols, OSPF routers maintain a link-state database describing the topology of their OSPF area.

Each router uses this information to calculate the shortest paths to destination networks.

OSPF belongs to the Interior Gateway Protocol family because it operates within an autonomous system.

### 🖆 Example Network

```text
                    Area 0
              +----------------+
              |                |
           Router A -------- Router B
              |                |
              |                |
           Router C -------- Router D
              |                |
              +----------------+
```

Routers exchange link-state information and independently calculate their routing paths.

If a link fails, OSPF advertises the topology change, allowing affected routers to recalculate their routes.

# 🗁 OSPF Characteristics

| Characteristic | Description |
| --- | --- |
| Protocol type | Link-state |
| Routing category | Interior Gateway Protocol |
| Algorithm | Dijkstra SPF |
| Metric | Cost |
| Administrative distance | 110 by default on Cisco IOS |
| IP protocol number | 89 |
| IPv4 multicast | `224.0.0.5` and `224.0.0.6` |
| IPv6 multicast | `ff02::5` and `ff02::6` |
| Authentication | Supported |
| Hierarchical design | Areas |
| Classless routing | Supported |
| VLSM | Supported |
| Convergence | Generally fast, depending on topology and configuration |

OSPF does not use TCP or UDP ports. It is carried directly inside IP using IP protocol number 89.

# 🗁 Link-State Routing

OSPF uses a link-state routing approach.

A link-state router builds a representation of the network topology using information received from other OSPF routers.

Each router maintains a Link-State Database (LSDB) containing Link-State Advertisements (LSAs).

Routers within the same area maintain synchronized area LSDBs, subject to the OSPF topology and protocol rules.

### 🖆 Link-State Routing Process

```text
             Discover Neighbors
                     |
                     v
             Establish Adjacencies
                     |
                     v
              Exchange LSAs
                     |
                     v
             Synchronize LSDB
                     |
                     v
              Run SPF Algorithm
                     |
                     v
              Calculate Routes
                     |
                     v
             Install Best Routes
```

### 🖆 Link-State vs Distance-Vector Routing

| Characteristic | Link-State | Distance-Vector |
| --- | --- | --- |
| Network knowledge | Topology database | Neighbor-provided route information |
| Example | OSPF | RIP |
| Algorithm | SPF | Bellman-Ford family |
| Updates | Link-state flooding and synchronization | Periodic and/or triggered route updates |
| Convergence | Generally fast | Depends on protocol and topology |
| Resource usage | More memory and CPU for topology processing | Often simpler |
| Network design | Hierarchical areas in OSPF | Depends on protocol |

# 🗁 How OSPF Works

OSPF performs several operations to build and maintain routing information.

### 🖆 Neighbor Discovery

Routers send OSPF Hello packets to discover other OSPF routers on connected networks.

Routers must have compatible OSPF settings to become neighbors.

### 🖆 Adjacency Formation

OSPF routers establish adjacencies when required by the network type and OSPF rules.

Adjacent routers synchronize their link-state databases.

### 🖆 Link-State Advertisement Exchange

Routers exchange LSAs describing network topology and reachability information.

LSAs are flooded through the appropriate OSPF scope.

### 🖆 Link-State Database Synchronization

Routers maintain synchronized LSDB information within their respective areas.

### 🖆 SPF Calculation

Each router runs the SPF algorithm against its area topology database.

The algorithm calculates the shortest paths from the router to reachable destinations.

### 🖆 Routing Table Installation

The best OSPF routes are submitted to the routing table.

The router then uses its forwarding information to forward packets.

# 🗁 OSPF Areas

OSPF uses areas to divide a large routing domain into smaller logical sections.

This hierarchical design helps control link-state information flooding and SPF calculation scope.

Every normal multi-area OSPF design must have an Area 0 backbone.

### 🖆 Area 0

Area 0 is the OSPF backbone area.

It provides the logical backbone connecting other OSPF areas.

### 🖆 Non-Backbone Areas

Non-backbone areas connect to the backbone through Area Border Routers.

Example:

```text
                 Area 1
                    |
                    |
                  ABR
                    |
                    |
              +-----------+
              |  Area 0   |
              | Backbone  |
              +-----------+
                    |
                    |
                  ABR
                    |
                    |
                 Area 2
```

### 🖆 OSPF Router Roles

| Router Role | Description |
| --- | --- |
| Internal Router | All OSPF interfaces belong to the same area |
| Backbone Router | Has at least one OSPF interface in Area 0 |
| Area Border Router (ABR) | Connects multiple OSPF areas |
| Autonomous System Boundary Router (ASBR) | Introduces external routes into OSPF |

A router can perform more than one role. For example, an ABR can also be an ASBR.

### 🖆 Why Use Areas?

* Reduce the scope of topology flooding.
* Limit SPF recalculation to affected areas where possible.
* Improve scalability.
* Support route summarization at area boundaries.
* Organize network topology logically.

# 🗁 OSPF Area Types

OSPF supports several area types that control which LSA types are allowed and how external routing information is handled.

### 🖆 Normal Area

A normal area supports standard intra-area, inter-area, and external routing information according to OSPF rules.

### 🖆 Stub Area

A stub area restricts external Type 5 LSAs.

An ABR can inject a default route into the stub area so routers can reach destinations outside the area.

### 🖆 Totally Stubby Area

Totally stubby is a common vendor-specific extension.

It restricts additional summary information, generally providing a default route for destinations outside the area.

Its exact behavior depends on the implementation.

### 🖆 Not-So-Stubby Area (NSSA)

An NSSA restricts Type 5 external LSAs but allows redistribution of external routes into the area using Type 7 LSAs.

An ABR can translate Type 7 LSAs into Type 5 LSAs when advertising external routes into other areas, subject to OSPF rules.

### 🖆 Totally NSSA

Totally NSSA is a vendor-specific extension that further restricts summary information while retaining NSSA external-route behavior.

### 🖆 Area Type Comparison

| Area Type | Type 3 Summary LSAs | Type 5 External LSAs | Type 7 External LSAs |
| --- | --- | --- | --- |
| Normal | Allowed | Allowed | Not normally used |
| Stub | Allowed | Blocked | Not allowed |
| Totally Stubby | Restricted | Blocked | Not allowed |
| NSSA | Allowed | Blocked | Allowed |
| Totally NSSA | Restricted | Blocked | Allowed |

Area behavior and implementation-specific extensions should be checked against the router vendor's documentation.

# 🗁 OSPF Router ID

An OSPF Router ID is a 32-bit identifier represented in IPv4 dotted-decimal format.

Example:

```text
1.1.1.1
```

The Router ID uniquely identifies an OSPF router within the routing domain.

It is an identifier, not necessarily an IP address assigned to an interface.

### 🖆 Router ID Selection

Selection behavior depends on the platform.

On Cisco IOS, if a Router ID is not manually configured, the router generally selects:

* The highest IP address on an active loopback interface.
* Otherwise, the highest IP address on an active physical interface.

On other implementations, selection and stability rules may differ.

### 🖆 Example Cisco Configuration

```cisco
router ospf 1
 router-id 1.1.1.1
```

### 🖆 Important Considerations

* Router IDs should be unique within the OSPF routing domain.
* Manually configuring Router IDs improves predictability.
* Changing a Router ID may require restarting or resetting the OSPF process.
* A Router ID does not have to be reachable as a normal interface address.

# 🗁 OSPF Packet Types

OSPF uses five main packet types.

| Packet Type | Name | Purpose |
| --- | --- | --- |
| 1 | Hello | Discover and maintain neighbors |
| 2 | Database Description (DBD) | Summarize LSDB contents |
| 3 | Link-State Request (LSR) | Request specific LSAs |
| 4 | Link-State Update (LSU) | Carry LSAs |
| 5 | Link-State Acknowledgment (LSAck) | Acknowledge received LSAs |

### 🖆 Hello Packet

Hello packets discover neighbors and maintain neighbor relationships.

They also carry information such as:

* Router ID.
* Area ID.
* Hello and Dead intervals.
* Network mask for relevant network types.
* Router priority.
* Designated Router and Backup Designated Router information.
* Neighbor Router IDs.
* Authentication-related information where configured.

### 🖆 Database Description Packet

DBD packets summarize the contents of the sender's LSDB.

They help adjacent routers determine which LSAs they need.

### 🖆 Link-State Request

An LSR requests specific LSAs from a neighbor.

### 🖆 Link-State Update

An LSU carries one or more LSAs.

### 🖆 Link-State Acknowledgment

An LSAck acknowledges received LSAs.

# 🗁 OSPF Neighbor Discovery

OSPF routers use Hello packets to discover neighbors.

On IPv4 broadcast and point-to-point networks, OSPF commonly uses multicast address `224.0.0.5` for AllSPFRouters.

On broadcast and non-broadcast multi-access networks, designated routers use `224.0.0.6` for AllDRouters.

For IPv6:

| Multicast Address | Purpose |
| --- | --- |
| `ff02::5` | AllSPFRouters |
| `ff02::6` | AllDRouters |

OSPF multicast addresses are link-local in scope.

### 🖆 Important Neighbor Requirements

Common parameters that must be compatible include:

* Area ID.
* Hello interval.
* Dead interval.
* Authentication configuration.
* Area type and relevant area options.
* Network type.
* IP subnet compatibility where applicable.
* OSPF instance and protocol compatibility.

MTU mismatches can also prevent database exchange from completing successfully on some implementations.

A mismatch in some parameters can prevent neighbor formation entirely, while others can cause adjacency or database synchronization problems.

# 🗁 OSPF Neighbor States

OSPF neighbors progress through a series of states as their relationship develops.

| State | Description |
| --- | --- |
| Down | No recent Hello packets have been received |
| Attempt | Router is attempting to contact a neighbor on an NBMA network |
| Init | A Hello packet has been received, but the local Router ID is not listed in the neighbor's Hello |
| 2-Way | Bidirectional communication has been established |
| ExStart | Routers negotiate master/slave roles and initial database exchange parameters |
| Exchange | Database Description packets are exchanged |
| Loading | Missing LSAs are requested and received |
| Full | Relevant LSDB synchronization is complete |

### 🖆 Neighbor State Progression

```text
Down
  |
  v
Init
  |
  v
2-Way
  |
  v
ExStart
  |
  v
Exchange
  |
  v
Loading
  |
  v
Full
```

The Attempt state is relevant to certain NBMA configurations.

### 🖆 2-Way vs Full

A 2-Way relationship means two routers can communicate bidirectionally using Hello packets.

A Full relationship means the required database synchronization has completed.

On broadcast networks, DROther routers commonly remain in 2-Way state with one another because they do not need to form full adjacencies with every other DROther router.

# 🗁 OSPF Designated Router and Backup Designated Router

On broadcast and certain non-broadcast multi-access networks, OSPF elects:

* Designated Router (DR).
* Backup Designated Router (BDR).

The DR reduces the number of adjacencies and organizes LSA exchange on the multi-access network.

The BDR is prepared to take over if the DR becomes unavailable.

### 🖆 DR Election

OSPF DR election considers:

* OSPF interface priority.
* Router ID as a tie-breaker.

A priority of zero makes a router ineligible to become DR or BDR.

The election is non-preemptive in normal operation: a newly arriving router with a higher priority does not automatically replace an existing DR.

### 🖆 Example

```text
              Shared Ethernet Network
                         |
          +--------------+--------------+
          |              |              |
       Router A       Router B       Router C
         DR             BDR          DROther
          |              |              |
          +--------------+--------------+
```

### 🖆 Important Notes

* DR/BDR elections are per multi-access network segment.
* Point-to-point networks do not require DR/BDR elections.
* The DR and BDR help reduce the number of required full adjacencies.
* A router's priority can influence its eligibility and election preference.

# 🗁 OSPF Link-State Database

The Link-State Database contains the topology information used by OSPF routers to calculate routes.

LSAs describe routers, networks, reachable prefixes, and external routing information.

Routers within the same area maintain synchronized copies of the relevant area LSDB.

### 🖆 LSDB vs Routing Table

| LSDB | Routing Table |
| --- | --- |
| Contains topology and link-state information | Contains selected forwarding routes |
| Stores LSAs | Stores destination prefixes and next hops |
| Used by SPF calculation | Used for packet forwarding |
| May contain multiple topology details | Contains routes selected by the routing process |

The LSDB is not the same as the routing table.

Not every LSA becomes an installed route.

# 🗁 OSPF LSA Types

Link-State Advertisements carry information about OSPF topology and reachability.

Common LSA types include:

| Type | Name | Purpose |
| --- | --- | --- |
| 1 | Router LSA | Describes a router's links within an area |
| 2 | Network LSA | Describes a multi-access network and attached routers |
| 3 | Summary LSA | Advertises inter-area network information |
| 4 | ASBR Summary LSA | Provides reachability information to an ASBR |
| 5 | AS External LSA | Advertises external routes |
| 7 | NSSA External LSA | Advertises external routes within an NSSA |

### 🖆 Type 1 — Router LSA

Generated by every OSPF router for each area in which it participates.

It describes the router's OSPF links and associated topology information.

### 🖆 Type 2 — Network LSA

Generated by the DR for a broadcast or NBMA network.

It describes the network and the routers attached to it.

### 🖆 Type 3 — Summary LSA

Generated by an ABR to advertise network reachability information between areas.

### 🖆 Type 4 — ASBR Summary LSA

Advertises information about how to reach an ASBR from another area.

### 🖆 Type 5 — AS External LSA

Advertises external routing information redistributed into the OSPF domain.

### 🖆 Type 7 — NSSA External LSA

Used within an NSSA to advertise external routes.

An NSSA ABR can translate Type 7 LSAs into Type 5 LSAs for propagation outside the NSSA.

# 🗁 OSPF SPF Algorithm

OSPF uses Dijkstra's Shortest Path First algorithm to calculate the shortest paths through the topology represented by the LSDB.

Each router calculates a shortest-path tree with itself as the root.

### 🖆 Example Topology

```text
             Cost 10
       Router A -------- Router B
          |                 |
          |                 |
       Cost 20           Cost 15
          |                 |
          |                 |
       Router C -------- Router D
             Cost 5
```

Suppose Router A needs to reach Router D.

Possible paths:

| Path | Total Cost |
| --- | ---: |
| A → B → D | 25 |
| A → C → D | 25 |

Both paths have equal cost.

Depending on configuration and platform capabilities, OSPF may install both paths using ECMP.

### 🖆 SPF Calculation

The simplified process is:

* Build the topology graph from the LSDB.
* Set the local router as the root.
* Calculate the lowest-cost paths to reachable nodes.
* Determine next hops.
* Install eligible routes in the routing table.

OSPF performs SPF calculations when topology changes or when relevant information changes according to implementation and protocol behavior.

# 🗁 OSPF Cost and Metrics

OSPF uses cost as its routing metric.

The total path cost is calculated by adding the costs of the relevant outgoing interfaces along the path.

Lower total cost is preferred.

### 🖆 Reference Bandwidth Formula

A common Cisco-style formula is:

```text
OSPF Cost = Reference Bandwidth / Interface Bandwidth
```

For example:

```text
Reference Bandwidth = 100 Mbps
Interface Bandwidth = 10 Mbps

OSPF Cost = 100 / 10

OSPF Cost = 10
```

This is a common reference-bandwidth calculation, not a universal requirement for every OSPF implementation.

### 🖆 Why Reference Bandwidth Matters

If the reference bandwidth is too low, faster interfaces may all receive the same minimum cost.

For example, with a 100 Mbps reference bandwidth, multiple interfaces operating at 1 Gbps or 10 Gbps may receive a cost of 1 on implementations that use integer costs and this formula.

Modern networks should configure an appropriate reference bandwidth consistently across the OSPF domain.

### 🖆 Manually Configure OSPF Cost

Cisco IOS example:

```cisco
interface GigabitEthernet0/0
 ip ospf cost 10
```

Manually configured costs can be useful for traffic engineering.

# 🗁 OSPF Route Types

OSPF routes are classified according to where they originate and how they enter the routing domain.

| Route Type | Description |
| --- | --- |
| Intra-area | Learned within the same OSPF area |
| Inter-area | Learned from another OSPF area |
| External Type 1 (E1) | External cost plus internal OSPF cost to the ASBR |
| External Type 2 (E2) | External metric is primary; internal cost to the ASBR is considered as a tie-breaker |
| NSSA Type 1 (N1) | NSSA external route using Type 1-style metric behavior |
| NSSA Type 2 (N2) | NSSA external route using Type 2-style metric behavior |

### 🖆 Intra-Area Routes

Routes advertised within the same area.

### 🖆 Inter-Area Routes

Routes advertised between areas through ABRs.

### 🖆 External Type 1

The external metric is combined with the internal OSPF cost to reach the ASBR.

This can make the total cost reflect both external and internal path costs.

### 🖆 External Type 2

The external metric is the primary comparison value.

The internal cost to reach the ASBR is considered when comparing routes with the same external metric.

### 🖆 E1 vs E2 Example

Suppose two ASBRs advertise the same external destination.

| ASBR | External Metric | Internal Cost | E1 Total |
| --- | ---: | ---: | ---: |
| ASBR A | 20 | 10 | 30 |
| ASBR B | 20 | 30 | 50 |

For E1, the route through ASBR A has the lower total cost.

For E2, both have an external metric of 20, so internal cost can be used as a tie-breaker.

# 🗁 OSPF Authentication

OSPF authentication helps prevent unauthorized routers from participating in routing exchanges.

Authentication options depend on the OSPF version and implementation.

For OSPFv2, authentication mechanisms include:

* No authentication.
* Simple password authentication.
* Cryptographic authentication, such as keyed MD5 in legacy implementations.

Modern OSPF deployments should use supported cryptographic authentication mechanisms rather than relying on cleartext passwords.

OSPFv3 authentication and security are handled differently, including support for IPsec-based protection in its original standardized security model. Implementations may also support additional mechanisms.

### 🖆 Security Considerations

* Use authentication where supported and appropriate.
* Keep authentication settings consistent among neighbors.
* Use strong secrets.
* Avoid exposing routing infrastructure to untrusted networks.
* Filter unauthorized OSPF traffic at network boundaries.
* Monitor unexpected neighbor relationships.
* Follow current vendor security recommendations.

# 🗁 OSPF for IPv4 and IPv6

OSPF supports both IPv4 and IPv6.

The versions have different protocol behavior and configuration models.

| Feature | OSPFv2 | OSPFv3 |
| --- | --- | --- |
| Primary specification | RFC 2328 | RFC 5340 |
| Common use | IPv4 routing | IPv6 routing |
| IP protocol number | 89 | 89 |
| Router ID | 32-bit identifier | 32-bit identifier |
| IPv4 multicast | `224.0.0.5`, `224.0.0.6` | Not applicable |
| IPv6 multicast | Not applicable | `ff02::5`, `ff02::6` |

OSPFv3 was originally specified for IPv6 but has been extended to support additional address families.

OSPFv2 and OSPFv3 can run simultaneously in the same network to support IPv4 and IPv6 routing.

# 🗁 OSPF Configuration on Linux

Linux does not provide a complete OSPF routing daemon as part of the basic `iproute2` utilities.

A routing software package such as FRRouting (FRR) can provide OSPF functionality.

FRR supports several routing protocols, including OSPF, OSPFv3, BGP, and RIP.

### 🖆 Install FRRouting on Ubuntu

```bash
sudo apt update
sudo apt install frr frr-pythontools
```

### 🖆 Enable OSPF Daemon

Edit the FRR daemon configuration:

```bash
sudo nano /etc/frr/daemons
```

Enable the OSPF daemon:

```text
ospfd=yes
```

For IPv6 OSPF:

```text
ospf6d=yes
```

Restart FRR:

```bash
sudo systemctl restart frr
```

### 🖆 Configure OSPF Using vtysh

Open the FRR command-line interface:

```bash
sudo vtysh
```

Enter configuration mode:

```text
configure terminal
```

Configure an OSPF process:

```text
router ospf
 ospf router-id 1.1.1.1
 network 10.0.12.0/24 area 0.0.0.0
 network 192.168.10.0/24 area 0.0.0.0
```

The `network` statements enable OSPF on interfaces whose addresses match the specified networks and assign them to the configured area.

Save the configuration:

```text
write memory
```

### 🖆 Verify OSPF

```text
show ip ospf
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route ospf
```

### 🖆 Inspect Linux Routes

```bash
ip route show
ip route show proto ospf
```

The exact route protocol label can vary according to the FRR and Linux integration configuration.

# 🗁 OSPF Configuration on MikroTik RouterOS

MikroTik RouterOS v7 supports OSPFv2 and OSPFv3.

RouterOS v7 uses an instance-based OSPF configuration model.

The main configuration areas include:

* OSPF instances.
* OSPF areas.
* Interface templates.
* Interface configuration.
* OSPF routing information.

### 🖆 Example Topology

```text
Router A                          Router B
10.0.12.1/30                      10.0.12.2/30
192.168.10.1/24                   192.168.20.1/24
       |                                |
       +----------- OSPF ---------------+
                  Area 0
```

Assume Router A and Router B are directly connected through a transit network.

### 🖆 Configure Router A

Assign addresses:

```routeros
/ip address
add address=10.0.12.1/30 interface=ether1
add address=192.168.10.1/24 interface=ether2
```

Create an OSPF instance:

```routeros
/routing ospf instance
add name=ospf-v2 version=2 router-id=1.1.1.1
```

Create Area 0:

```routeros
/routing ospf area
add name=backbone instance=ospf-v2 area-id=0.0.0.0
```

Create interface templates:

```routeros
/routing ospf interface-template
add area=backbone networks=10.0.12.0/30
add area=backbone networks=192.168.10.0/24 passive
```

The passive LAN template advertises the LAN prefix without forming OSPF neighbor relationships with devices on that network.

### 🖆 Configure Router B

Assign addresses:

```routeros
/ip address
add address=10.0.12.2/30 interface=ether1
add address=192.168.20.1/24 interface=ether2
```

Create an OSPF instance:

```routeros
/routing ospf instance
add name=ospf-v2 version=2 router-id=2.2.2.2
```

Create Area 0:

```routeros
/routing ospf area
add name=backbone instance=ospf-v2 area-id=0.0.0.0
```

Create interface templates:

```routeros
/routing ospf interface-template
add area=backbone networks=10.0.12.0/30
add area=backbone networks=192.168.20.0/24 passive
```

### 🖆 Verify OSPF Neighbors

On both routers:

```routeros
/routing ospf neighbor print detail
```

### 🖆 View OSPF Routes

```routeros
/ip route print where ospf
```

### 🖆 Important Considerations

* Router IDs must be unique.
* Both routers must use compatible OSPF settings.
* The transit interfaces must have compatible network configuration.
* The LAN networks should be advertised without forming unwanted adjacencies with end devices.
* Ensure firewall rules allow OSPF protocol traffic.
* RouterOS v6 uses a different OSPF configuration structure.

RouterOS v7 syntax can vary with minor releases, so verify commands against the installed version's documentation.

# 🗁 OSPF Configuration on Cisco IOS

Cisco IOS commonly configures OSPF using a process ID and network statements.

### 🖆 Example Topology

```text
Router A                          Router B
10.0.12.1/30                      10.0.12.2/30
192.168.10.1/24                   192.168.20.1/24
       |                                |
       +----------- OSPF ---------------+
                  Area 0
```

### 🖆 Configure Router A

Assign interface addresses:

```cisco
interface GigabitEthernet0/0
 ip address 10.0.12.1 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
```

Configure OSPF:

```cisco
router ospf 1
 router-id 1.1.1.1
 network 10.0.12.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
```

### 🖆 Configure Router B

Assign interface addresses:

```cisco
interface GigabitEthernet0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
```

Configure OSPF:

```cisco
router ospf 1
 router-id 2.2.2.2
 network 10.0.12.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0
```

The process ID is locally significant and does not have to match between routers.

### 🖆 Verify OSPF

```cisco
show ip ospf
show ip ospf neighbor
show ip ospf interface brief
show ip ospf database
show ip route ospf
```

### 🖆 Configure a Passive Interface

To advertise a LAN network without sending OSPF Hello packets on the LAN interface:

```cisco
router ospf 1
 passive-interface GigabitEthernet0/1
```

This is commonly used on user-facing LAN interfaces.

# 🗁 OSPF Troubleshooting

OSPF problems commonly involve neighbor formation, adjacency synchronization, route calculation, or route installation.

### 🖆 Check Neighbor State

Cisco IOS:

```cisco
show ip ospf neighbor
```

MikroTik RouterOS v7:

```routeros
/routing ospf neighbor print detail
```

FRR:

```text
show ip ospf neighbor
```

### 🖆 Check OSPF Interfaces

Cisco IOS:

```cisco
show ip ospf interface brief
```

FRR:

```text
show ip ospf interface
```

MikroTik:

```routeros
/routing ospf interface print detail
```

### 🖆 Check Routing Information

```bash
ip route show
```

Cisco IOS:

```cisco
show ip route ospf
```

MikroTik:

```routeros
/ip route print where ospf
```

### 🖆 Common OSPF Problems

| Problem | Possible Cause |
| --- | --- |
| No neighbors discovered | Incorrect interface, subnet, multicast, or firewall configuration |
| Neighbor stuck in Init | One-way Hello communication |
| Neighbor stuck in ExStart or Exchange | MTU mismatch, duplicate Router ID, or database exchange problem |
| Neighbor stuck in Loading | LSA request or database synchronization issue |
| Routes missing | Incorrect area, missing advertisements, filtering, or route-selection issue |
| Adjacency repeatedly resets | Unstable link, timer mismatch, authentication failure, or packet loss |
| DR election unexpected | Interface priority or Router ID |
| External routes missing | Redistribution, ASBR, area type, or LSA filtering issue |

### 🖆 Verify OSPF Network Parameters

Check:

* Area ID.
* Hello interval.
* Dead interval.
* Authentication.
* Network type.
* Interface subnet and mask where applicable.
* Router ID uniqueness.
* OSPF interface state.
* MTU compatibility.
* Firewall rules.
* Multicast reachability.

### 🖆 Capture OSPF Packets

Linux:

```bash
sudo tcpdump -ni eth0 ip proto 89
```

Wireshark display filter:

```text
ospf
```

For IPv6 OSPF:

```text
ospf
```

Wireshark identifies OSPF packets according to their protocol decoding.

### 🖆 Troubleshooting Workflow

```text
        Check Physical Interface
                  |
                  v
          Check IP Addressing
                  |
                  v
          Check OSPF Enabled
                  |
                  v
       Check Area and Parameters
                  |
                  v
          Check Neighbor State
                  |
                  v
          Check LSDB Contents
                  |
                  v
         Check Calculated Routes
                  |
                  v
         Check Forwarding Path
```

# 🗁 OSPF Best Practices

* Use a clear and hierarchical area design.
* Keep Area 0 as the backbone.
* Configure unique and stable Router IDs.
* Use consistent OSPF cost planning.
* Configure reference bandwidth consistently across the routing domain.
* Use passive interfaces on networks where no OSPF neighbors are expected.
* Enable authentication where appropriate.
* Avoid unnecessary redistribution between routing protocols.
* Summarize routes at area boundaries when the topology and address plan permit.
* Monitor neighbor state changes and routing convergence.
* Avoid oversized or unnecessarily complex OSPF areas.
* Document area assignments, router roles, and transit links.
* Use appropriate route filtering and redistribution policies.

# 🗁 Common OSPF Mistakes

| Mistake | Correct Understanding |
| --- | --- |
| OSPF is a distance-vector protocol. | OSPF is a link-state routing protocol. |
| OSPF uses TCP port 89. | OSPF uses IP protocol number 89 directly. |
| Every OSPF router must have the same process ID. | Process IDs are locally significant on Cisco IOS. |
| Every OSPF neighbor must reach Full state. | DROther routers on broadcast networks commonly remain 2-Way with one another. |
| Area 0 is optional in a normal multi-area design. | The OSPF backbone is required for normal inter-area routing. |
| OSPF uses hop count. | OSPF uses cost. |
| A lower interface bandwidth always means lower OSPF cost. | Cost calculation depends on configured reference bandwidth and implementation. |
| The LSDB is the routing table. | The LSDB stores topology information; the routing table stores selected routes. |
| DR election is always preemptive. | Normal OSPF DR election is non-preemptive. |
| OSPFv3 is only for IPv6 in every possible deployment. | OSPFv3 was designed for IPv6 and has extensions for other address families. |
| A Full adjacency guarantees every destination is reachable. | Route advertisement, area design, filtering, and forwarding must also be correct. |

# 🗁 Interview Questions

### 🖆 What is OSPF?

OSPF is a link-state Interior Gateway Protocol that uses the SPF algorithm to calculate routes within an autonomous system.

### 🖆 What algorithm does OSPF use?

OSPF uses Dijkstra's Shortest Path First algorithm.

### 🖆 What metric does OSPF use?

OSPF uses cost. The path with the lowest total cost is normally preferred.

### 🖆 What is an OSPF area?

An OSPF area is a logical grouping of routers and networks that limits the scope of link-state information and SPF calculations.

### 🖆 What is Area 0?

Area 0 is the OSPF backbone area. Other areas normally connect to it through Area Border Routers.

### 🖆 What is an ABR?

An Area Border Router connects multiple OSPF areas.

### 🖆 What is an ASBR?

An Autonomous System Boundary Router introduces external routing information into the OSPF domain, commonly through redistribution.

### 🖆 What is an OSPF Router ID?

A Router ID is a 32-bit identifier used to identify an OSPF router.

### 🖆 What are the five OSPF packet types?

* Hello.
* Database Description.
* Link-State Request.
* Link-State Update.
* Link-State Acknowledgment.

### 🖆 What is the difference between a neighbor and an adjacency?

A neighbor relationship is established through Hello packet exchange. An adjacency is a relationship in which routers synchronize their relevant link-state databases.

### 🖆 What is the difference between 2-Way and Full?

2-Way indicates bidirectional Hello communication. Full indicates successful synchronization of the relevant LSDB information.

### 🖆 What is a DR and BDR?

The Designated Router and Backup Designated Router reduce adjacency complexity on multi-access OSPF networks.

### 🖆 What is an LSA?

A Link-State Advertisement carries topology or reachability information within OSPF.

### 🖆 What is the difference between Type 5 and Type 7 LSAs?

Type 5 advertises external routes in regular OSPF areas. Type 7 advertises external routes within an NSSA.

### 🖆 What is the difference between E1 and E2 external routes?

E1 adds internal OSPF cost to the external metric. E2 primarily compares the external metric and uses internal cost to the ASBR as a tie-breaker.

### 🖆 Why might an OSPF neighbor remain stuck in ExStart?

Possible causes include MTU mismatch, duplicate Router IDs, or database exchange negotiation problems.

### 🖆 Does OSPF use TCP or UDP?

No. OSPF is carried directly over IP using protocol number 89.

### 🖆 What multicast addresses does OSPFv2 use?

* `224.0.0.5` — AllSPFRouters.
* `224.0.0.6` — AllDRouters.

### 🖆 What is the difference between OSPFv2 and OSPFv3?

OSPFv2 is commonly used for IPv4 routing, while OSPFv3 was designed for IPv6 and has been extended to support additional address families.

# 🗁 Summary

OSPF is a widely used link-state routing protocol for exchanging routing information within an autonomous system.

Key concepts:

* OSPF is an Interior Gateway Protocol.
* It uses Dijkstra's SPF algorithm.
* Its routing metric is cost.
* It organizes networks into areas.
* Area 0 is the backbone area.
* Routers discover neighbors using Hello packets.
* LSAs describe network topology and reachability.
* The LSDB stores link-state information.
* OSPF supports DR and BDR elections on applicable multi-access networks.
* OSPF supports IPv4 through OSPFv2 and IPv6 through OSPFv3.
* Linux can run OSPF using FRRouting.
* MikroTik RouterOS v7 and Cisco IOS provide native OSPF implementations.

OSPF is especially useful in medium and large networks where dynamic routing, fast topology adaptation, and hierarchical network design are important.

# 🗁 References

* [RFC 2328 — OSPF Version 2](https://www.rfc-editor.org/rfc/rfc2328)
* [RFC 5340 — OSPF for IPv6](https://www.rfc-editor.org/rfc/rfc5340)
* [RFC 3101 — The OSPF Not-So-Stubby Area (NSSA) Option](https://www.rfc-editor.org/rfc/rfc3101)
* [RFC 5709 — OSPFv2 HMAC-SHA Cryptographic Authentication](https://www.rfc-editor.org/rfc/rfc5709)
* [RFC 7166 — Supporting Authentication Trailer for OSPFv3](https://www.rfc-editor.org/rfc/rfc7166)
* [FRRouting Documentation](https://docs.frrouting.org/)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [Cisco OSPF Configuration Documentation](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/7039-1.html)
