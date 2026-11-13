# Network Topologies

Network topology describes how devices, links, and communication paths are arranged within a computer network.

It defines the structure of a network and influences how devices communicate, how data travels, how easily the network can be expanded, and how failures affect connectivity.

Network topology is generally classified into two categories:

* **Physical topology:** The physical arrangement of devices, cables, ports, and networking equipment.
* **Logical topology:** The way data flows between devices, regardless of their physical arrangement.

Understanding network topologies is important when designing, implementing, troubleshooting, and maintaining networks.

# 🗁 Physical and Logical Topology

### 🖆 Physical Topology

Physical topology describes the actual physical layout of a network.

It includes:

* The location of computers, switches, routers, and access points.
* The physical connections between networking devices.
* The arrangement of Ethernet cables, fiber-optic links, and wireless connections.
* The physical structure of network infrastructure.

For example, when multiple computers connect to a central Ethernet switch, the physical topology is typically a star.

### 🖆 Logical Topology

Logical topology describes how data moves through a network.

It focuses on:

* Data transmission paths.
* Communication between devices.
* How network devices forward traffic.
* The logical organization of network communication.

A network can have a physical star topology but use different logical forwarding mechanisms depending on its technology.

For example, traditional Ethernet hubs physically connect devices in a star arrangement, but all connected devices share the same collision domain and receive transmitted signals.

Modern Ethernet switches also commonly use a physical star arrangement, but each switch port provides a separate collision domain.

### 🖆 Physical vs Logical Topology

| Feature | Physical Topology | Logical Topology |
| --- | --- | --- |
| Meaning | Physical network arrangement | Data communication arrangement |
| Focus | Devices and connections | Data flow and forwarding |
| Components | Cables, switches, routers | Protocols and forwarding behavior |
| Example | Star, physical mesh | Ethernet switching behavior |
| Importance | Installation and maintenance | Communication and troubleshooting |

# 🗁 Bus Topology

### 🖆 What Is Bus Topology?

Bus topology is a network arrangement in which all devices connect to a single shared communication cable called the backbone.

Data transmitted by one device travels along the shared cable. All connected devices can receive the signal, but only the intended destination processes the frame.

<pre>
          Terminator
              |
================================
    |          |          |
  PC-01      PC-02      PC-03
</pre>

### 🖆 How Bus Topology Works

* All devices share one communication medium.
* A device transmits data onto the shared cable.
* The signal travels along the backbone.
* Connected devices receive the signal.
* The destination device accepts the data.

Traditional coaxial Ethernet networks used bus topology.

### 🖆 Characteristics

* Uses a shared backbone cable.
* Requires termination at both ends of the cable.
* Relatively simple physical structure.
* All devices share the communication medium.
* Performance can decrease as network traffic increases.
* A backbone cable failure can disrupt the entire network.

### 🖆 Advantages

* Requires less cable than some other physical arrangements.
* Simple to understand and implement in small legacy networks.
* Does not require a central switching device.

### 🖆 Disadvantages

* Backbone failure can bring down the entire network.
* Difficult to troubleshoot cable faults.
* Performance degrades under heavy traffic.
* Adding devices can affect network performance.
* Limited scalability.
* Cable length and device count are restricted by the underlying technology.

### 🖆 Where It Is Used

Bus topology is mostly associated with legacy networking technologies.

It is rarely used in modern enterprise Ethernet networks.

# 🗁 Star Topology

### 🖆 What Is Star Topology?

Star topology is a network arrangement in which all devices connect to a central networking device, such as a switch or hub.

All communication between connected devices passes through the central device.

<pre>
                 PC-01
                   |
                   |
PC-02 --------- SWITCH --------- PC-03
                   |
                   |
                 PC-04
</pre>

### 🖆 How Star Topology Works

* Each endpoint has a separate connection to the central device.
* A device sends data through its connected link.
* The central device receives the traffic.
* A switch forwards frames toward the appropriate destination port.
* The destination device receives the data.

Modern switched Ethernet LANs commonly use physical star topology.

### 🖆 Characteristics

* Requires a central networking device.
* Each endpoint has an individual connection to the central device.
* Easy to add or remove devices.
* A single endpoint cable failure normally affects only that endpoint.
* Central device failure can disrupt the connected network segment.

### 🖆 Advantages

* Easy to install and manage.
* Simple troubleshooting.
* Easy network expansion.
* Individual cable failures have limited impact.
* Good performance with Ethernet switches.
* Supports centralized network management.

### 🖆 Disadvantages

* Requires more cabling than some topologies.
* Depends on the central networking device.
* Switch failure can disconnect all devices attached to that switch.
* Network expansion may require additional switches and uplinks.

### 🖆 Where It Is Used

Star topology is widely used in:

* Home networks.
* Office LANs.
* School and university networks.
* Enterprise access networks.
* Small and medium-sized business networks.

# 🗁 Ring Topology

### 🖆 What Is Ring Topology?

Ring topology connects each device to two neighboring devices, forming a closed communication loop.

Data travels around the ring from one device to another until it reaches its destination.

<pre>
          PC-01 -------- PC-02
            |              |
            |              |
          PC-04 -------- PC-03
</pre>

### 🖆 How Ring Topology Works

In a basic ring network:

* Each device connects to two neighboring devices.
* A device receives data from one neighbor.
* The device forwards data to the next neighbor.
* Data continues around the ring until it reaches the destination.

Some ring technologies use token passing to control which device can transmit.

### 🖆 Characteristics

* Devices form a closed loop.
* Each device has connections to neighboring devices.
* Communication follows a defined ring path.
* Some implementations support unidirectional transmission.
* Other implementations support bidirectional or redundant paths.

### 🖆 Advantages

* Predictable communication behavior in certain implementations.
* Token-passing mechanisms can prevent transmission collisions.
* Can provide controlled access to a shared communication path.

### 🖆 Disadvantages

* A single link or device failure can interrupt a basic ring.
* Adding or removing devices may affect network operation.
* Troubleshooting can be complicated.
* Latency can increase as traffic passes through multiple devices.

### 🖆 Where It Is Used

Ring topology has been used in:

* Token Ring networks.
* Fiber Distributed Data Interface (FDDI).
* Metropolitan and industrial networking technologies.
* Resilient Ethernet ring implementations.

Modern resilient ring networks often use protection mechanisms to maintain connectivity when a link fails.

# 🗁 Mesh Topology

### 🖆 What Is Mesh Topology?

Mesh topology connects network devices through multiple communication paths.

Instead of depending on a single connection, devices can communicate through alternative paths when redundant links are available.

There are two major types:

* Full mesh topology.
* Partial mesh topology.

### 🖆 Full Mesh Topology

In a full mesh, every device has a direct connection to every other device.

<pre>
          Router-01
           /  |  \
          /   |   \
         /    |    \
 Router-02---Router-03
         \    |    /
          \   |   /
           \  |  /
          Router-04
</pre>

For a full mesh network containing `n` devices, the number of required point-to-point links is:

\[
L = \frac{n(n-1)}{2}
\]

Where:

* `L` = Total number of links.
* `n` = Number of devices.

For example, a full mesh containing four devices requires:

\[
L = \frac{4(4-1)}{2} = 6
\]

### 🖆 Partial Mesh Topology

In a partial mesh, only selected devices have multiple direct connections.

Other devices communicate through intermediate network devices.

<pre>
          Router-01
           /     \
          /       \
     Router-02---Router-03
          |
          |
       Router-04
</pre>

Partial mesh is generally more practical and economical than full mesh for larger networks.

### 🖆 Characteristics

* Provides multiple communication paths.
* Supports redundancy and fault tolerance.
* Can improve network availability.
* Requires more links and configuration.
* Often uses dynamic routing protocols to select paths.

### 🖆 Advantages

* High redundancy.
* Better fault tolerance.
* Alternative paths are available when links fail.
* Supports resilient network architectures.
* Can improve availability when designed correctly.

### 🖆 Disadvantages

* Higher infrastructure cost.
* More complex installation and maintenance.
* Requires additional interfaces, cables, or wireless capacity.
* Routing and network management can become more complicated.
* Full mesh becomes expensive as the number of devices increases.

### 🖆 Where It Is Used

Mesh topology is commonly used in:

* ISP backbone networks.
* Enterprise WANs.
* Data center networks.
* Wireless mesh networks.
* Critical infrastructure networks.
* Redundant router and firewall architectures.

# 🗁 Tree Topology

### 🖆 What Is Tree Topology?

Tree topology is a hierarchical network arrangement in which devices are organized into multiple levels.

It combines characteristics of star and bus topologies.

A typical tree network contains:

* Root or core devices.
* Distribution devices.
* Access devices.
* End-user devices.

<pre>
                 CORE SWITCH
                 /         \
                /           \
       DISTRIBUTION-01   DISTRIBUTION-02
          /      \          /      \
         /        \        /        \
      SW-01      SW-02   SW-03      SW-04
       / \        / \     / \        / \
     PC  PC     PC  PC  PC  PC     PC  PC
</pre>

### 🖆 How Tree Topology Works

* The core layer provides the central network backbone.
* Distribution devices connect different network segments.
* Access switches connect end-user devices.
* Traffic moves between network segments through the appropriate upstream devices.

### 🖆 Characteristics

* Hierarchical network structure.
* Multiple levels of connectivity.
* Supports network segmentation.
* Easy to expand by adding branches.
* Commonly used in enterprise network design.

### 🖆 Advantages

* Scalable network architecture.
* Supports hierarchical management.
* Easier network segmentation.
* Simplifies network expansion.
* Supports structured troubleshooting.

### 🖆 Disadvantages

* Failure of an important upstream device can affect multiple downstream devices.
* Requires careful planning.
* More cabling and networking equipment may be required.
* Incorrect configuration can affect large network segments.

### 🖆 Where It Is Used

Tree topology is commonly found in:

* Enterprise campus networks.
* University networks.
* Large office networks.
* Corporate LAN infrastructure.
* Hierarchical switched networks.

# 🗁 Hybrid Topology

### 🖆 What Is Hybrid Topology?

Hybrid topology combines two or more different network topologies into a single network architecture.

For example, an enterprise network might use:

* Star topology at the access layer.
* Partial mesh topology between core routers.
* Tree topology across campus buildings.

<pre>
              CORE ROUTER
               /       \
              /         \
         CORE-SW-01   CORE-SW-02
           /    \       /    \
         SW-01  SW-02  SW-03  SW-04
          / \    / \    / \    / \
        PCs PCs PCs PCs PCs PCs PCs PCs
</pre>

### 🖆 How Hybrid Topology Works

Different parts of the network use the topology that best meets their requirements.

For example:

* End-user devices connect to access switches using star topology.
* Distribution switches connect to core switches using redundant links.
* Core routers connect through a partial mesh.
* Remote branches connect through WAN links.

### 🖆 Characteristics

* Combines multiple topology types.
* Flexible network design.
* Supports large and complex infrastructures.
* Can provide redundancy and scalability.
* Requires careful planning and management.

### 🖆 Advantages

* Flexible architecture.
* Supports different network requirements.
* Scalable for large organizations.
* Can combine the strengths of multiple topologies.
* Allows redundancy in critical network segments.

### 🖆 Disadvantages

* More complex design.
* Higher implementation costs.
* Requires experienced network administrators.
* Troubleshooting can involve multiple network architectures.

### 🖆 Where It Is Used

Hybrid topology is commonly used in:

* Enterprise networks.
* ISP infrastructure.
* Large campus networks.
* Data centers.
* Multi-branch organizations.

# 🗁 Point-to-Point Topology

### 🖆 What Is Point-to-Point Topology?

Point-to-point topology provides a direct communication link between exactly two network devices.

<pre>
       Router-01 ================= Router-02
                 Direct Link
</pre>

The connection can use:

* Ethernet.
* Fiber-optic cable.
* Wireless radio links.
* Serial interfaces.
* Dedicated WAN circuits.

### 🖆 How Point-to-Point Communication Works

Two devices communicate directly over a dedicated link.

For example, two routers may connect through a dedicated fiber-optic connection and exchange IP traffic.

The connection does not require intermediate forwarding devices between the two endpoints.

### 🖆 Characteristics

* Connects two endpoints.
* Provides a direct communication path.
* Easy to understand and troubleshoot.
* Link bandwidth is dedicated to the connection, subject to the service and technology.
* Commonly used for router-to-router connectivity.

### 🖆 Advantages

* Simple architecture.
* Predictable communication path.
* Easy troubleshooting.
* No shared-medium contention on a dedicated link.
* Suitable for dedicated WAN connections.

### 🖆 Disadvantages

* Connects only two endpoints directly.
* Additional connections are required to connect more devices.
* Link failure disconnects the two endpoints unless an alternative path exists.
* Dedicated connections may be expensive.

### 🖆 Where It Is Used

Point-to-point topology is commonly used in:

* Router-to-router links.
* Dedicated WAN connections.
* Fiber-optic backbone links.
* Wireless backhaul.
* Data center interconnections.

# 🗁 Comparison of Network Topologies

| Topology | Structure | Scalability | Fault Tolerance | Complexity |
| --- | --- | --- | --- | --- |
| Bus | Shared backbone | Low | Low | Low |
| Star | Central device | High | Moderate | Low |
| Ring | Closed loop | Moderate | Low to high, depending on implementation | Moderate |
| Full Mesh | Every device connects to every other device | Limited by cost and complexity | Very high | High |
| Partial Mesh | Selected redundant connections | High | High | High |
| Tree | Hierarchical structure | High | Depends on redundancy | Moderate to high |
| Hybrid | Combination of topologies | Very high | Depends on design | High |
| Point-to-Point | Direct connection between two devices | Limited | Low without backup links | Low |

### 🖆 Important Considerations

Topology selection depends on several factors:

* Network size.
* Required availability.
* Infrastructure budget.
* Physical distance.
* Expected traffic.
* Scalability requirements.
* Maintenance complexity.
* Redundancy requirements.

No single topology is ideal for every network.

# 🗁 Choosing the Right Topology

### 🖆 Home Network

A typical home network uses a star arrangement.

Example:

* A router or wireless access point acts as the central device.
* Computers connect through Ethernet or Wi-Fi.
* Other devices connect through the same central network infrastructure.

### 🖆 Office Network

An office network commonly uses an extended star or hierarchical tree architecture.

Example:

* Access switches connect employee devices.
* Distribution switches aggregate access switches.
* Core switches provide high-speed connectivity.

### 🖆 Enterprise Network

A large enterprise may use a hybrid topology.

Example:

* Hierarchical switching across buildings.
* Redundant core connections.
* Multiple WAN links.
* Mesh connectivity between critical routers.

### 🖆 ISP Network

An ISP commonly uses partial mesh and hierarchical architectures.

Example:

* Access routers connect subscribers or aggregation networks.
* Aggregation routers collect customer traffic.
* Core routers provide high-capacity transport.
* Multiple paths provide redundancy.

### 🖆 Data Center Network

Modern data centers often use leaf-spine architecture.

<pre>
       SPINE-01       SPINE-02
          |  \       /  |
          |   \     /   |
          |    \   /    |
       LEAF-01 LEAF-02 LEAF-03
          |       |       |
        SERVERS SERVERS SERVERS
</pre>

Each leaf switch connects to every spine switch.

This creates multiple equal-cost paths between leaf switches and supports scalable east-west traffic.

Leaf-spine is a specific data center network architecture that uses a structured form of partial mesh connectivity.

# 🗁 Common Mistakes

### 🖆 Confusing Physical and Logical Topology

Physical topology describes how devices are physically connected.

Logical topology describes how communication and data forwarding occur.

They are related but not necessarily identical.

### 🖆 Assuming Star Topology Has No Single Point of Failure

A traditional star network depends on its central device.

If the central switch fails, all devices connected through that switch may lose connectivity.

Redundant switches and uplinks can reduce this risk.

### 🖆 Assuming Mesh Always Means Full Mesh

Mesh topology can be either full mesh or partial mesh.

Partial mesh is often more practical for large networks.

### 🖆 Ignoring Redundancy

A network may have a good topology but still contain critical single points of failure.

Redundancy should be considered for important links and devices.

### 🖆 Ignoring Network Growth

A topology should account for future expansion, additional devices, traffic growth, and infrastructure requirements.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify Network Topologies

Identify the topology represented by each scenario.

* Ten computers connect to one central Ethernet switch.
* Four routers each have a direct connection to every other router.
* Six devices connect through one shared coaxial backbone.
* Multiple devices connect in a closed communication loop.
* A company uses access, distribution, and core switching layers.

### 🖆 Exercise: Calculate Full Mesh Links

Calculate the number of point-to-point links required for:

* Three devices.
* Five devices.
* Eight devices.
* Ten devices.

Use the formula:

\[
L = \frac{n(n-1)}{2}
\]

### 🖆 Exercise: Design an Office Network

Design a network for an office containing:

* 30 employee computers.
* 5 network printers.
* 3 wireless access points.
* 2 servers.
* 1 router.
* 2 switches.

Determine:

* Which topology is appropriate?
* How should the devices connect?
* What happens if one access switch fails?
* How could you introduce redundancy?
* How would you organize the network into separate VLANs?

### 🖆 Exercise: Design an ISP Network

Create a basic ISP network architecture containing:

* Two core routers.
* Three aggregation routers.
* Four access routers.
* Multiple customer networks.
* Redundant core connections.

Identify where you would use star, tree, and partial mesh connectivity.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is network topology?**

Network topology describes the physical or logical arrangement of devices and communication links in a network.

**What are the two major categories of network topology?**

Physical topology and logical topology.

**What is bus topology?**

A topology in which all devices share a common communication backbone.

**What is star topology?**

A topology in which devices connect to a central networking device.

**What is mesh topology?**

A topology in which devices have multiple communication paths through direct connections.

**What is hybrid topology?**

A topology that combines two or more different network topology types.

### 🖆 Intermediate Questions

**Which topology is commonly used in modern Ethernet LANs?**

Physical star topology using Ethernet switches is commonly used.

**What is the difference between full mesh and partial mesh?**

In full mesh, every device connects directly to every other device. In partial mesh, only selected devices have direct connections to multiple devices.

**Why is mesh topology suitable for critical networks?**

It provides alternative communication paths, improving redundancy and fault tolerance.

**What is the main disadvantage of star topology?**

The central device can become a single point of failure if no redundancy is implemented.

**How many links are required for a full mesh containing six devices?**

\[
L = \frac{6(6-1)}{2} = 15
\]

### 🖆 Advanced Questions

**What is leaf-spine architecture?**

A data center network architecture in which leaf switches connect to all spine switches, providing multiple paths between network segments.

**Can a network have a physical star topology and a different logical topology?**

Yes. Physical and logical topology describe different aspects of network organization.

**Why do large enterprise networks use hierarchical topology?**

Hierarchical designs improve scalability, segmentation, manageability, and fault isolation.

**How does redundancy improve network availability?**

Redundant links and devices provide alternative communication paths when primary components fail.

# 🗁 References

* [Cisco Networking Basics](https://www.cisco.com/)
* [Cisco Enterprise Network Design](https://www.cisco.com/site/us/en/learn/topics/networking/what-is-network-design.html)
* [IEEE 802 Standards](https://standards.ieee.org/standard/802_3-2022.html)
* [CompTIA Network+](https://www.comptia.org/certifications/network)
* [Computer Networking: A Top-Down Approach](https://gaia.cs.umass.edu/kurose_ross/index.php)
