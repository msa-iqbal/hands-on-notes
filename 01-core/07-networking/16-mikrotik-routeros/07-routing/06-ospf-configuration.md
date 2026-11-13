# OSPF Configuration

OSPF (Open Shortest Path First) is a dynamic Interior Gateway Protocol (IGP) used to exchange routing information between routers within an autonomous system.

MikroTik RouterOS v7 supports OSPF for automatically discovering and maintaining routes between routers.

Unlike static routing, OSPF dynamically calculates routes based on network topology and link costs.

OSPF is commonly used for:

* Routing between multiple routers.
* Connecting different LAN networks.
* Building scalable enterprise networks.
* Dynamic route discovery.
* Redundant network paths.
* ISP and service-provider infrastructure.

This documentation focuses on configuring OSPFv2 in MikroTik RouterOS v7.

# 🗁 OSPF Fundamentals

OSPF is a link-state routing protocol.

Each participating router discovers its OSPF neighbors and exchanges link-state information. Routers use this information to build a link-state database (LSDB) representing the network topology.

They then calculate the shortest paths using the Shortest Path First (SPF) algorithm, based on Dijkstra's algorithm.

Important OSPF characteristics:

| Feature | Description |
| --- | --- |
| Protocol type | Link-state IGP |
| Algorithm | Dijkstra SPF |
| Administrative distance | Commonly 110 |
| Metric | Cost |
| Protocol number | 89 |
| Multicast IPv4 addresses | `224.0.0.5`, `224.0.0.6` |
| Standard | OSPFv2: RFC 2328 |
| IPv6 version | OSPFv3: RFC 5340 |

OSPF does not use TCP or UDP ports. It operates directly over IP using protocol number `89`.

# 🗁 OSPF Areas

OSPF divides a routing domain into areas to improve scalability and reduce routing overhead.

An area is a logical grouping of OSPF routers and networks.

The most important area is the **Backbone Area**, identified by:

```text
0.0.0.0
```

The backbone connects other OSPF areas.

Common OSPF area types include:

| Area Type | Description |
| --- | --- |
| Backbone | Main OSPF area, Area 0 |
| Normal | Supports standard OSPF routing information |
| Stub | Restricts certain external route advertisements |
| Totally Stubby | Vendor-specific extension that further restricts route information |
| NSSA | Supports limited external route redistribution through an ASBR |

For a basic OSPF deployment, start with a single backbone area.

# 🗁 OSPF Router Roles

OSPF routers can perform different roles depending on their position in the topology.

| Router Role | Description |
| --- | --- |
| Internal Router | All OSPF interfaces belong to the same area |
| Backbone Router | Has at least one interface in Area 0 |
| Area Border Router (ABR) | Connects multiple OSPF areas |
| Autonomous System Boundary Router (ASBR) | Redistributes routes from other routing sources into OSPF |

A router can perform more than one role.

# 🗁 OSPF Router ID

Every OSPF router needs a unique Router ID.

The Router ID is a 32-bit identifier represented in IPv4 dotted-decimal notation.

Example:

```text
1.1.1.1
```

Router IDs identify routers within the OSPF domain. They do not need to be reachable IP addresses, but they should be unique and consistently managed.

Configure the Router ID through the OSPF instance:

```routeros
/routing/ospf/instance/set [find where name="default-v2"] router-id=1.1.1.1
```

If the instance does not exist, create it with the desired Router ID instead.

Avoid changing Router IDs unnecessarily because doing so can affect OSPF neighbor relationships and route calculations.

# 🗁 OSPF Instances

An OSPF instance defines the OSPF routing process and its general configuration.

RouterOS v7 uses OSPF instances to organize OSPF operation.

Create an OSPFv2 instance:

```routeros
/routing/ospf/instance/add \
    name=default-v2 \
    version=2 \
    router-id=1.1.1.1
```

The `version=2` property selects OSPFv2 for IPv4 routing.

Inspect configured instances:

```routeros
/routing/ospf/instance/print detail
```

# 🗁 OSPF Areas Configuration

After creating an OSPF instance, create an area.

Create the backbone area:

```routeros
/routing/ospf/area/add \
    name=backbone-v2 \
    instance=default-v2 \
    area-id=0.0.0.0
```

The `area-id` identifies the OSPF area.

For a basic single-area configuration, use Area 0.

Inspect configured areas:

```routeros
/routing/ospf/area/print detail
```

# 🗁 OSPF Interface Templates

RouterOS v7 uses OSPF interface templates to determine which interfaces and networks participate in OSPF.

An interface template can specify:

* The OSPF area.
* Matching interfaces or networks.
* Interface type.
* Authentication settings.
* Cost.
* Passive behavior.
* Other OSPF interface parameters.

Example using a network prefix:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=10.0.12.0/30
```

This enables OSPF participation for matching interfaces and addresses.

You can also configure an interface explicitly:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    interfaces=ether1
```

Use the interface or network matching method that fits the topology. Avoid overlapping templates that unintentionally apply conflicting settings.

# 🗁 Passive Interfaces

A passive OSPF interface advertises its connected network into OSPF without forming OSPF neighbor adjacencies on that interface.

This is useful for LAN interfaces where client devices do not run OSPF.

Example:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=192.168.10.0/24 \
    passive=yes
```

The LAN prefix can be advertised to other OSPF routers, but OSPF neighbor discovery does not occur on that passive interface.

Passive interfaces are useful for:

* User LANs.
* Server networks.
* Management networks.
* Networks containing devices that do not participate in OSPF.

# 🗁 OSPF Network Advertisement

OSPF advertises network reachability through its link-state database.

In RouterOS v7, interface templates determine which interfaces and networks participate in OSPF.

For example:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=192.168.10.0/24 \
    passive=yes
```

This advertises the LAN network without attempting to establish OSPF neighbors with LAN clients.

The network must already be configured and available on an appropriate router interface.

# 🗁 OSPF Neighbor Discovery

OSPF routers discover neighboring routers using Hello packets.

Neighbor discovery requires compatible settings and working IP connectivity between the routers.

Important parameters include:

* Area ID.
* Hello interval.
* Dead interval.
* Authentication settings.
* Network type.
* Interface subnet compatibility.
* Router ID uniqueness.

OSPF uses the following multicast addresses for IPv4:

| Multicast Address | Purpose |
| --- | --- |
| `224.0.0.5` | All OSPF routers |
| `224.0.0.6` | All designated routers |

Inspect OSPF neighbors:

```routeros
/routing/ospf/neighbor/print
```

For more details:

```routeros
/routing/ospf/neighbor/print detail
```

A successfully established adjacency is generally displayed with the state `Full`.

# 🗁 OSPF Neighbor States

OSPF neighbors pass through several states while establishing an adjacency.

| State | Description |
| --- | --- |
| Down | No Hello packets have been received |
| Attempt | Unicast Hello packets are being sent, primarily for NBMA networks |
| Init | A Hello packet has been received, but bidirectional communication is not confirmed |
| 2-Way | Bidirectional Hello communication is established |
| ExStart | Routers negotiate master/slave roles for database exchange |
| Exchange | Database Description packets are exchanged |
| Loading | Required link-state advertisements are requested |
| Full | Link-state databases are synchronized |

Not every neighbor must reach `Full` in every network type.

For example, on broadcast networks, some routers may remain in the `2-Way` state with each other when they are not required to form a full adjacency.

# 🗁 OSPF Cost

OSPF uses cost as its routing metric.

The SPF algorithm calculates the total cost of a path by adding the costs of the links along that path.

A lower total cost is generally preferred.

Example:

```text
             Link Cost 10
        R1 ---------------- R2
         \
          \ Link Cost 20
           \
            R3
```

If R1 has two possible paths to a destination:

* Path through R2 has total cost 10.
* Path through R3 has total cost 20.

OSPF prefers the path through R2, assuming both paths are otherwise eligible.

You can configure interface cost through an OSPF interface template.

Example:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    interfaces=ether1 \
    cost=10
```

A carefully designed cost plan helps control path selection in larger networks.

# 🗁 Configuring OSPF Authentication

OSPF authentication helps protect routing communication against unauthorized participation and certain routing attacks.

RouterOS v7 supports OSPF authentication configuration through interface templates.

Authentication must be configured consistently between neighboring routers.

For example, a template can be configured with an authentication method and key:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    interfaces=ether1 \
    auth=md5 \
    auth-id=1 \
    auth-key="Replace-With-A-Strong-Key"
```

The exact authentication method must be supported by the selected RouterOS version and compatible with the neighboring router.

Do not enable authentication on only one side of an adjacency.

# 🗁 Viewing OSPF Routes

OSPF-learned routes can be inspected through the routing menu.

Display OSPF-related routes:

```routeros
/routing/route/print where protocol=ospf
```

Display detailed route information:

```routeros
/routing/route/print detail where protocol=ospf
```

You can also inspect the IPv4 routing table:

```routeros
/ip/route/print detail
```

OSPF routes commonly appear with the `o` route flag in the IPv4 route display.

# 🗁 OSPF and Static Routes

OSPF can coexist with static routes.

For example:

* Static routes can provide manually configured reachability.
* OSPF can dynamically advertise internal network routes.
* Static routes can be redistributed into OSPF when required.
* OSPF-learned routes can provide alternatives to other route sources.

Route selection depends on destination prefix specificity, route eligibility, and administrative preference.

A static route commonly has a lower default distance than an OSPF route, so it is generally preferred when both provide the same prefix and are otherwise comparable.

# 🗁 OSPF Troubleshooting

When OSPF does not establish an adjacency or advertise routes correctly, check the following areas.

**OSPF instance**

```routeros
/routing/ospf/instance/print detail
```

Verify:

* The instance exists.
* The correct OSPF version is configured.
* The Router ID is unique.

**OSPF areas**

```routeros
/routing/ospf/area/print detail
```

Verify:

* The area exists.
* The interface template references the correct area.
* Neighboring routers use compatible area settings.

**OSPF interface templates**

```routeros
/routing/ospf/interface-template/print detail
```

Verify:

* The correct interfaces or networks are matched.
* The correct area is selected.
* Interface settings are compatible.
* The interface is not unintentionally passive.

**OSPF neighbors**

```routeros
/routing/ospf/neighbor/print detail
```

Verify:

* Hello packets are exchanged.
* Neighbor states progress correctly.
* Router IDs are unique.
* Authentication settings match.
* Hello and dead intervals are compatible.

**Routing information**

```routeros
/routing/route/print detail
```

Verify:

* Expected OSPF routes are present.
* Routes are active and eligible.
* The destination prefix is correct.
* Route selection is not favoring another route.

# 🗁 Common OSPF Problems

| Problem | Possible Cause |
| --- | --- |
| No neighbor discovered | Incorrect interface template, IP connectivity, or interface state |
| Neighbor stuck in Init | Bidirectional Hello communication is not established |
| Neighbor stuck in ExStart or Exchange | MTU mismatch or database exchange problems |
| Authentication failure | Incompatible authentication settings or keys |
| No OSPF routes received | Network not advertised, passive configuration issues, or area problems |
| Wrong path selected | OSPF cost or route preference |
| Duplicate Router ID | Router ID conflict |
| Adjacency repeatedly drops | Link instability, timers, packet loss, or authentication issues |

# 🗁 Practical Lab: Configure OSPF Between Two MikroTik Routers

**Objective:** Establish an OSPF adjacency between two MikroTik routers and exchange LAN routes dynamically.

**Topology:**

```text
       LAN-A                                      LAN-B
   192.168.10.0/24                            192.168.20.0/24
         |                                           |
   +-----------+       10.0.12.0/30            +-----------+
   |  Router 1 |-------------------------------|  Router 2 |
   +-----------+                               +-----------+
         |                                           |
   ether1: 10.0.12.1/30                  ether1: 10.0.12.2/30
```

**IP addressing plan:**

| Device | Interface | IP Address |
| --- | --- | --- |
| Router 1 | ether1 | `10.0.12.1/30` |
| Router 1 | bridge-LAN | `192.168.10.1/24` |
| Router 2 | ether1 | `10.0.12.2/30` |
| Router 2 | bridge-LAN | `192.168.20.1/24` |

Assume the LAN bridges and client addressing are already configured.

**Router 1 Configuration**

Create the OSPF instance:

```routeros
/routing/ospf/instance/add \
    name=default-v2 \
    version=2 \
    router-id=1.1.1.1
```

Create the backbone area:

```routeros
/routing/ospf/area/add \
    name=backbone-v2 \
    instance=default-v2 \
    area-id=0.0.0.0
```

Enable OSPF on the transit network:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=10.0.12.0/30
```

Advertise the LAN network passively:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=192.168.10.0/24 \
    passive=yes
```

**Router 2 Configuration**

Create the OSPF instance:

```routeros
/routing/ospf/instance/add \
    name=default-v2 \
    version=2 \
    router-id=2.2.2.2
```

Create the backbone area:

```routeros
/routing/ospf/area/add \
    name=backbone-v2 \
    instance=default-v2 \
    area-id=0.0.0.0
```

Enable OSPF on the transit network:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=10.0.12.0/30
```

Advertise the LAN network passively:

```routeros
/routing/ospf/interface-template/add \
    area=backbone-v2 \
    networks=192.168.20.0/24 \
    passive=yes
```

**Verify OSPF neighbor adjacency**

On Router 1:

```routeros
/routing/ospf/neighbor/print detail
```

On Router 2:

```routeros
/routing/ospf/neighbor/print detail
```

Both routers should discover each other and establish a full adjacency on the transit link.

**Verify learned routes**

On Router 1:

```routeros
/routing/route/print where protocol=ospf
```

Router 1 should learn the LAN-B network:

```text
192.168.20.0/24
```

On Router 2:

```routeros
/routing/route/print where protocol=ospf
```

Router 2 should learn the LAN-A network:

```text
192.168.10.0/24
```

**Test connectivity**

From Router 1:

```routeros
/ping 192.168.20.1
```

From Router 2:

```routeros
/ping 192.168.10.1
```

You can also test communication between client devices on both LANs.

**Firewall considerations**

OSPF uses IP protocol number `89`, not TCP or UDP ports.

If firewall rules restrict traffic to the router, permit OSPF packets on the transit interface from trusted neighboring routers.

For example, on Router 1:

```routeros
/ip/firewall/filter/add \
    chain=input \
    action=accept \
    protocol=ospf \
    src-address=10.0.12.2 \
    in-interface=ether1 \
    comment="Allow OSPF from Router 2"
```

On Router 2:

```routeros
/ip/firewall/filter/add \
    chain=input \
    action=accept \
    protocol=ospf \
    src-address=10.0.12.1 \
    in-interface=ether1 \
    comment="Allow OSPF from Router 1"
```

Place these rules before any input-chain rule that would otherwise drop the OSPF packets.

Also ensure that the forward-chain firewall policy permits the intended traffic between LAN networks.

# 🗁 Best Practices

* Assign a unique and stable Router ID to every OSPF router.
* Use Area 0 for a basic single-area deployment.
* Use passive interfaces for LAN networks that do not contain OSPF routers.
* Keep OSPF interface templates organized.
* Use consistent authentication settings between neighbors.
* Design OSPF costs intentionally.
* Avoid advertising unnecessary networks.
* Permit OSPF protocol traffic only from trusted network segments.
* Monitor neighbor states and learned routes.
* Test route convergence after link failures.
* Document area assignments and Router IDs.

# 🗁 Summary

OSPF is a link-state dynamic routing protocol that automatically discovers network topology and calculates routes.

Key points:

* OSPF uses the SPF algorithm to calculate shortest paths.
* OSPFv2 is used for IPv4 routing.
* Router IDs uniquely identify OSPF routers.
* Areas organize OSPF networks.
* Area 0 is the backbone area.
* RouterOS v7 uses OSPF instances, areas, and interface templates.
* Passive interfaces advertise networks without forming neighbor adjacencies.
* OSPF uses cost to select preferred paths.
* OSPF operates directly over IP using protocol number `89`.

Understanding OSPF provides the foundation for building dynamic, redundant, and scalable MikroTik routing networks.
