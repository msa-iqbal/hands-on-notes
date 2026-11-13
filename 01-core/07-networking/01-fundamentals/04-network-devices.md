# Network Devices

Network devices are hardware components that connect computers, servers, and other equipment, enabling communication and data exchange across networks.

Each network device performs a specific role. Some devices connect endpoints within a local network, while others forward traffic between different networks, provide wireless connectivity, filter traffic, or translate network addresses.

Understanding network devices is essential for designing, configuring, securing, and troubleshooting computer networks.

Common network devices include:

* Network Interface Card (NIC)
* Hub
* Repeater
* Bridge
* Switch
* Router
* Gateway
* Modem
* Wireless Access Point (WAP)
* Firewall
* Proxy Server
* Load Balancer

# 🗁 Network Interface Card (NIC)

### 🖆 What Is a NIC?

A Network Interface Card (NIC) is a hardware component that allows a computer or another device to connect to a network.

A NIC can be integrated into a motherboard or installed as an expansion card.

It provides the physical or wireless interface through which a device sends and receives network traffic.

<pre>
       Computer
    +-------------+
    |             |
    |     CPU     |
    |             |
    |   +------+  |
    |   | NIC  |  |
    |   +------+  |
    +------|------+
           |
       Ethernet
         Cable
           |
         Switch
</pre>

### 🖆 Functions of a NIC

* Connects a device to a network.
* Sends and receives network frames.
* Provides a MAC address for network communication.
* Converts data between the host system and network interface.
* Supports Ethernet or wireless communication, depending on the NIC.
* May support hardware features such as checksum offloading and VLAN tagging.

### 🖆 Types of NIC

**Ethernet NIC**

Provides wired network connectivity through an Ethernet interface.

Common speeds include:

* 100 Mbps
* 1 Gbps
* 2.5 Gbps
* 10 Gbps
* 25 Gbps
* 40 Gbps
* 100 Gbps and higher

**Wireless NIC**

Provides wireless connectivity using Wi-Fi technologies.

It communicates with wireless access points or other supported wireless devices.

### 🖆 MAC Address

A NIC commonly has a MAC address used for communication at the Data Link layer.

Example:

```text
00:1A:2B:3C:4D:5E
```

A MAC address is generally represented as a 48-bit address in Ethernet networks.

### 🖆 Linux Commands for NIC Information

List network interfaces:

```bash
ip link
```

Display IP addresses:

```bash
ip addr
```

Display detailed interface information:

```bash
ip -details link show
```

Display a specific interface:

```bash
ip link show eth0
```

The interface name may differ depending on the Linux system.

# 🗁 Hub

### 🖆 What Is a Hub?

A hub is a basic networking device that connects multiple Ethernet devices within the same physical network segment.

It operates at the Physical layer (Layer 1) of the OSI model.

A hub does not understand MAC addresses or make forwarding decisions. It simply repeats incoming electrical signals to its other ports.

<pre>
             PC-01
               |
               |
       +---------------+
       |      HUB      |
       +---------------+
          /    |    \
         /     |     \
      PC-02  PC-03  PC-04
</pre>

### 🖆 How a Hub Works

Suppose PC-01 sends data through a hub.

* The hub receives the incoming electrical signal.
* It repeats the signal through its other active ports.
* All connected devices receive the signal.
* The intended destination processes the frame.
* Other devices ignore traffic that is not addressed to them.

A hub does not selectively forward traffic to a specific destination.

### 🖆 Types of Hub

**Passive Hub**

Connects devices without actively regenerating or amplifying signals.

**Active Hub**

Regenerates and retransmits incoming signals.

**Intelligent Hub**

Provides basic monitoring and management capabilities in addition to hub functionality.

### 🖆 Characteristics

* Operates at Layer 1.
* Does not maintain a MAC address table.
* Sends incoming signals to other ports.
* All devices share the same collision domain.
* Typically operates in half-duplex environments.
* Does not perform traffic filtering.

### 🖆 Disadvantages

* Unnecessary traffic reaches every connected device.
* Collisions can occur when multiple devices transmit simultaneously.
* Limited performance and scalability.
* No intelligent forwarding.
* Provides no traffic isolation.

### 🖆 Modern Usage

Hubs are largely obsolete in modern Ethernet networks.

Ethernet switches replaced hubs because switches provide more efficient traffic forwarding and separate collision domains per port.

# 🗁 Repeater

### 🖆 What Is a Repeater?

A repeater is a networking device that regenerates or reconstructs signals to extend the distance over which data can travel.

It operates at the Physical layer (Layer 1) of the OSI model.

Signals can become weaker or distorted as they travel through a communication medium. This phenomenon is known as attenuation.

A repeater helps extend the communication distance by regenerating the signal.

<pre>
   Network Segment
         |
         |
     +---------+
     | REPEATER|
     +---------+
         |
         |
   Extended Segment
</pre>

### 🖆 How a Repeater Works

* Receives an incoming signal.
* Processes or regenerates the signal.
* Transmits the regenerated signal through another segment.
* Extends the effective communication distance.

### 🖆 Types of Repeater

* Ethernet repeater.
* Wireless repeater.
* Optical repeater.

### 🖆 Characteristics

* Operates at Layer 1.
* Regenerates signals.
* Does not make routing decisions.
* Does not inspect IP addresses.
* Does not perform MAC-based forwarding.

### 🖆 Repeater vs Hub

A hub is essentially a multiport repeater.

A repeater generally extends a communication link, while a hub connects multiple devices by repeating signals across its ports.

### 🖆 Where It Is Used

* Extending wireless coverage.
* Extending communication over certain physical media.
* Optical communication systems.
* Specialized industrial networks.

# 🗁 Bridge

### 🖆 What Is a Bridge?

A network bridge connects multiple LAN segments and forwards Ethernet frames based on MAC addresses.

It operates at the Data Link layer (Layer 2) of the OSI model.

A bridge learns the MAC addresses of devices and uses that information to decide whether a frame should be forwarded or filtered.

<pre>
       Network Segment A
         PC-01   PC-02
            \     /
             \   /
          +---------+
          | BRIDGE  |
          +---------+
                |
                |
         Network Segment B
           PC-03   PC-04
</pre>

### 🖆 How a Bridge Works

A bridge performs several operations:

**MAC Address Learning**

The bridge examines the source MAC address of incoming frames and learns which segment contains that device.

**Frame Forwarding**

If the destination MAC address is known and belongs to another segment, the bridge forwards the frame toward that segment.

**Frame Filtering**

If the destination is known to exist on the same segment where the frame arrived, the bridge does not forward it to another segment.

**Unknown Unicast Flooding**

If the destination MAC address is unknown, the bridge generally forwards the frame through other eligible ports, excluding the incoming port.

Broadcast frames are also forwarded according to the bridge's forwarding rules.

### 🖆 Types of Bridge

* Transparent bridge.
* Source-route bridge.
* Translational bridge.

### 🖆 Characteristics

* Operates at Layer 2.
* Uses MAC addresses.
* Learns source MAC addresses.
* Forwards and filters Ethernet frames.
* Separates collision domains.
* Traditional bridges generally do not separate broadcast domains.

### 🖆 Bridge vs Switch

A switch is conceptually a multiport bridge.

Traditional bridges generally have fewer interfaces, while switches provide many ports and more advanced forwarding capabilities.

Modern Ethernet switches perform the same fundamental MAC-learning and frame-forwarding functions.

# 🗁 Switch

### 🖆 What Is a Switch?

A network switch connects multiple devices within a Local Area Network (LAN).

It operates primarily at the Data Link layer (Layer 2) and forwards Ethernet frames based on destination MAC addresses.

Some switches also support Layer 3 routing functionality.

<pre>
       PC-01        PC-02
          \         /
           \       /
        +-------------+
        |   SWITCH    |
        +-------------+
          /    |    \
         /     |     \
      PC-03  SERVER  PRINTER
</pre>

### 🖆 How a Switch Works

A switch uses a MAC address table, also called a forwarding database, to make forwarding decisions.

The basic process is:

* A frame arrives on a switch port.
* The switch reads the source MAC address.
* It learns the source MAC address and associates it with the incoming port and VLAN.
* It checks the destination MAC address.
* If the destination is known, it forwards the frame through the appropriate port.
* If the destination is unknown, it floods the frame within the relevant VLAN, excluding the incoming port.
* Broadcast frames are forwarded within the applicable VLAN.

### 🖆 MAC Address Table

A MAC address table stores information about learned MAC addresses.

Example:

| MAC Address | VLAN | Switch Port |
| --- | ---: | --- |
| AA:AA:AA:AA:AA:01 | 10 | Gi0/1 |
| BB:BB:BB:BB:BB:02 | 10 | Gi0/2 |
| CC:CC:CC:CC:CC:03 | 20 | Gi0/3 |
| DD:DD:DD:DD:DD:04 | 20 | Gi0/4 |

The switch uses this information to forward frames efficiently.

### 🖆 Types of Switch

**Unmanaged Switch**

A basic switch that operates with minimal configuration.

Commonly used in small home and office networks.

**Managed Switch**

A configurable switch that provides features such as:

* VLANs.
* Spanning Tree Protocol (STP).
* Link aggregation.
* Port security.
* SNMP monitoring.
* Quality of Service (QoS).

**Layer 2 Switch**

Forwards Ethernet frames based on MAC addresses.

**Layer 3 Switch**

Supports Layer 2 switching and Layer 3 routing between IP networks or VLANs.

**PoE Switch**

Provides Power over Ethernet, allowing supported devices to receive electrical power through Ethernet cables.

Common PoE-powered devices include:

* Wireless access points.
* IP cameras.
* VoIP phones.

### 🖆 Collision Domains and Broadcast Domains

A traditional Ethernet switch creates a separate collision domain for each switch port.

However, ports in the same VLAN belong to the same Layer 2 broadcast domain.

VLANs can separate a switch into multiple broadcast domains.

Example:

```text
Switch
├── VLAN 10
│   ├── PC-01
│   └── PC-02
│
└── VLAN 20
    ├── PC-03
    └── PC-04
```

VLAN 10 and VLAN 20 are separate broadcast domains.

Communication between them requires Layer 3 routing.

### 🖆 Where Switches Are Used

* Office LANs.
* Enterprise networks.
* Data centers.
* Campus networks.
* ISP access and aggregation networks.
* Server infrastructure.

# 🗁 Router

### 🖆 What Is a Router?

A router is a networking device that connects different IP networks and forwards packets between them.

It operates at the Network layer (Layer 3) of the OSI model.

Routers use destination IP addresses and routing information to determine the appropriate next hop for packets.

<pre>
       LAN-01                 LAN-02
    192.168.1.0/24          192.168.2.0/24
          |                       |
       SWITCH                  SWITCH
          |                       |
          |                       |
      +-----------------------------+
      |            ROUTER           |
      +-----------------------------+
</pre>

### 🖆 How a Router Works

When a router receives an IP packet:

* It receives the frame through an interface.
* It processes the incoming Layer 2 information.
* It examines the destination IP address.
* It checks the routing table.
* It selects the best matching route using longest-prefix matching.
* It determines the next hop and outgoing interface.
* It forwards the packet by creating an appropriate outgoing Layer 2 frame.

The router decrements the IPv4 TTL or IPv6 Hop Limit when forwarding a packet.

### 🖆 Routing Table

A routing table contains information about reachable IP networks and their next hops.

Example:

| Destination Network | Next Hop | Interface |
| --- | --- | --- |
| 192.168.1.0/24 | Directly connected | eth0 |
| 192.168.2.0/24 | Directly connected | eth1 |
| 10.10.0.0/16 | 192.168.1.254 | eth0 |
| 0.0.0.0/0 | 192.168.1.1 | eth0 |

The default route `0.0.0.0/0` matches IPv4 destinations that do not match a more specific route.

The IPv6 default route is `::/0`.

### 🖆 Types of Router

**Wired Router**

Connects networks using wired interfaces such as Ethernet or fiber.

**Wireless Router**

Combines routing functionality with wireless access point functionality.

**Core Router**

Forwards traffic within high-capacity backbone networks.

**Edge Router**

Connects an internal network to external networks, such as an ISP or another organization.

**Virtual Router**

A software-based routing implementation running on a virtual machine, server, or network operating system.

### 🖆 Routing Protocols

Routers may use static routes or dynamic routing protocols.

Common routing protocols include:

* RIP.
* OSPF.
* IS-IS.
* BGP.
* EIGRP.

Dynamic routing protocols allow routers to exchange routing information and adapt to network changes.

### 🖆 Router vs Switch

| Feature | Router | Switch |
| --- | --- | --- |
| Primary OSI Layer | Layer 3 | Layer 2 |
| Main Address | IP address | MAC address |
| Main Function | Connects IP networks | Connects devices within LANs |
| Forwarding Decision | Routing table | MAC address table |
| Broadcast Domains | Separates networks | VLANs separate broadcast domains |
| Typical Use | LAN-to-WAN and inter-network connectivity | LAN connectivity |

# 🗁 Gateway

### 🖆 What Is a Gateway?

A gateway is a device or software function that provides an entry or exit point between networks.

The term can describe different types of network functionality depending on the context.

In IP networking, a default gateway is the router or Layer 3 device that a host uses to reach destinations outside its local subnet.

<pre>
       PC-01
          |
          |
        SWITCH
          |
          |
      DEFAULT GATEWAY
          |
          |
      EXTERNAL NETWORK
</pre>

### 🖆 Default Gateway

A default gateway is the next-hop IP address used when a host needs to communicate with a destination outside its local network and has no more specific route.

Example:

```text
IP Address:      192.168.1.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.1.1
```

If the computer wants to communicate with `8.8.8.8`, it sends the traffic toward its default gateway.

The host uses ARP in IPv4 or Neighbor Discovery in IPv6 to resolve the next-hop link-layer address when required.

### 🖆 Types of Gateway

**Default Gateway**

Provides a next hop for traffic destined outside the local subnet.

**Application Gateway**

Processes or translates application-level communication between systems.

**Protocol Gateway**

Translates communication between different protocols or protocol environments.

**VoIP Gateway**

Connects voice-over-IP systems with other telephony networks.

### 🖆 Router vs Gateway

A router is a device that forwards IP packets between networks.

A gateway is a broader term that can refer to an entry point, an exit point, or a protocol/application translation function.

A router commonly serves as a network's default gateway.

# 🗁 Modem

### 🖆 What Is a Modem?

A modem is a device that performs modulation and demodulation to enable communication over a transmission medium.

The term modem comes from:

* Modulator.
* Demodulator.

A modem adapts digital information for transmission through a particular communication system and recovers information from received signals.

<pre>
      Home Network
           |
         Router
           |
         Modem
           |
       ISP Network
</pre>

### 🖆 How a Modem Works

* Receives digital data from connected equipment.
* Converts or encodes the data into signals suitable for the transmission medium.
* Sends signals through the provider's communication infrastructure.
* Receives incoming signals.
* Recovers the transmitted digital information.

The exact modulation and signaling techniques depend on the access technology.

### 🖆 Types of Modem

**DSL Modem**

Uses telephone copper lines to provide broadband connectivity.

**Cable Modem**

Uses cable television infrastructure, commonly based on DOCSIS technology.

**Fiber ONT**

An Optical Network Terminal converts optical signals from a fiber access network into electrical interfaces for customer equipment.

An ONT is not technically a conventional modem, although it is sometimes casually described as a fiber modem.

**Cellular Modem**

Uses cellular networks such as 4G LTE or 5G to provide connectivity.

### 🖆 Modem vs Router

| Feature | Modem | Router |
| --- | --- | --- |
| Main Function | Interfaces with access technology | Forwards traffic between networks |
| Main Responsibility | Signal adaptation and access connectivity | IP packet forwarding |
| Typical Connection | ISP access medium | LAN and WAN interfaces |
| Example | DSL or cable modem | Home or enterprise router |

Some consumer devices combine modem, router, firewall, and wireless access point functionality in a single unit.

# 🗁 Wireless Access Point

### 🖆 What Is a Wireless Access Point?

A Wireless Access Point (WAP or AP) allows wireless devices to connect to a wired network using Wi-Fi.

It operates primarily at the Data Link layer, providing wireless Layer 2 connectivity.

<pre>
       Laptop       Smartphone
          \           /
           \         /
          +-----------+
          | ACCESS    |
          | POINT     |
          +-----------+
                |
             Ethernet
                |
              SWITCH
                |
              ROUTER
                |
             INTERNET
</pre>

### 🖆 How a Wireless Access Point Works

* Broadcasts one or more wireless network names, known as SSIDs.
* Allows compatible wireless clients to discover the network.
* Authenticates clients according to the configured security method.
* Establishes wireless associations.
* Bridges wireless client traffic into the connected network.
* Transmits and receives wireless frames.

### 🖆 Common Wi-Fi Standards

| Standard | IEEE Specification | Frequency Bands |
| --- | --- | --- |
| Wi-Fi 4 | 802.11n | 2.4 GHz and 5 GHz |
| Wi-Fi 5 | 802.11ac | 5 GHz |
| Wi-Fi 6 | 802.11ax | 2.4 GHz and 5 GHz |
| Wi-Fi 6E | 802.11ax | 2.4 GHz, 5 GHz and 6 GHz |
| Wi-Fi 7 | 802.11be | 2.4 GHz, 5 GHz and 6 GHz |

Actual frequency availability depends on regulatory rules and device support.

### 🖆 Access Point vs Wireless Router

A wireless access point primarily provides wireless connectivity to an existing network.

A wireless router typically combines multiple functions:

* IP routing.
* Wireless access point.
* Ethernet switching.
* DHCP server.
* NAT.
* Firewall functionality.

### 🖆 Where Access Points Are Used

* Homes.
* Offices.
* Universities.
* Hotels.
* Shopping centers.
* Public Wi-Fi networks.
* Enterprise wireless infrastructure.

# 🗁 Firewall

### 🖆 What Is a Firewall?

A firewall is a network security system that monitors and controls incoming and outgoing traffic according to configured security policies.

It can be implemented as dedicated hardware, software, or a combination of both.

Firewalls commonly operate at multiple OSI layers depending on their capabilities.

<pre>
       INTERNAL NETWORK
              |
              |
         +----------+
         | FIREWALL |
         +----------+
              |
              |
        EXTERNAL NETWORK
</pre>

### 🖆 How a Firewall Works

* Receives network traffic.
* Evaluates traffic against configured rules.
* Checks relevant information such as IP addresses, ports, protocols, and connection state.
* Allows, rejects, or drops traffic according to policy.
* May inspect application-layer information or perform additional security functions.

### 🖆 Types of Firewall

**Packet-Filtering Firewall**

Filters packets based on information such as:

* Source IP address.
* Destination IP address.
* Protocol.
* Source port.
* Destination port.

**Stateful Firewall**

Tracks connection state and uses that information when evaluating traffic.

**Proxy Firewall**

Acts as an intermediary between clients and servers, potentially inspecting application-layer communication.

**Next-Generation Firewall (NGFW)**

May combine traditional firewall functions with capabilities such as:

* Application awareness.
* Intrusion prevention.
* Deep packet inspection.
* Threat intelligence integration.
* User-based policies.

### 🖆 Common Firewall Functions

* Traffic filtering.
* Network segmentation.
* Connection tracking.
* NAT and port forwarding.
* Application inspection.
* Logging and monitoring.
* Access control.

### 🖆 Where Firewalls Are Used

* Home networks.
* Enterprise networks.
* Data centers.
* ISP infrastructure.
* Cloud environments.
* Server infrastructure.

# 🗁 Proxy Server

### 🖆 What Is a Proxy Server?

A proxy server acts as an intermediary between a client and another server.

Instead of communicating directly with the destination server, the client sends its request through the proxy.

The proxy then processes or forwards the request according to its configuration.

<pre>
       CLIENT
          |
          |
      PROXY SERVER
          |
          |
     DESTINATION SERVER
</pre>

### 🖆 Forward Proxy

A forward proxy acts on behalf of clients.

Example:

```text
Employee Computer
       |
       v
Forward Proxy
       |
       v
Internet Website
```

Common uses include:

* Centralized internet access.
* Content filtering.
* Access control.
* Caching.
* Monitoring outbound requests.

### 🖆 Reverse Proxy

A reverse proxy acts on behalf of backend servers.

Example:

```text
      Internet Client
             |
             v
        Reverse Proxy
             |
        +----+----+
        |         |
        v         v
     Server-01 Server-02
```

Common uses include:

* Hiding backend server details.
* TLS termination.
* Load balancing.
* Request routing.
* Caching.
* Security controls.

### 🖆 Transparent Proxy

A transparent proxy intercepts or processes traffic without requiring clients to explicitly configure a conventional proxy setting.

Its behavior depends on network configuration and the protocols being handled.

### 🖆 Where Proxy Servers Are Used

* Enterprise networks.
* Web hosting infrastructure.
* Content delivery systems.
* Internet service providers.
* Security gateways.

# 🗁 Load Balancer

### 🖆 What Is a Load Balancer?

A load balancer distributes incoming traffic across multiple backend servers or service instances.

Its purpose is to improve scalability, availability, and resource utilization.

<pre>
          CLIENT REQUESTS
                 |
                 v
          +--------------+
          | LOAD BALANCER|
          +--------------+
             /       \
            /         \
           v           v
       SERVER-01    SERVER-02
            \         /
             \       /
              BACKEND
              SERVICES
</pre>

### 🖆 How a Load Balancer Works

* Receives incoming requests.
* Selects an available backend.
* Forwards the request to that backend.
* Monitors backend health when health checks are configured.
* Stops directing new requests to unhealthy backends according to its policy.

### 🖆 Types of Load Balancer

**Layer 4 Load Balancer**

Makes forwarding decisions using transport-layer information, commonly TCP or UDP addresses and ports.

**Layer 7 Load Balancer**

Makes decisions using application-layer information such as:

* HTTP hostnames.
* URL paths.
* HTTP headers.
* Cookies.

### 🖆 Common Load Balancing Algorithms

**Round Robin**

Distributes requests sequentially across available backends.

**Least Connections**

Selects a backend with fewer active connections, depending on the implementation.

**Weighted Round Robin**

Distributes traffic according to assigned backend weights.

**IP Hash**

Uses client IP information to select a backend.

### 🖆 Where Load Balancers Are Used

* Web applications.
* API infrastructure.
* Enterprise applications.
* Cloud platforms.
* High-availability services.
* Distributed application architectures.

# 🗁 Network Devices by OSI Layer

Different networking devices operate at different OSI layers.

| OSI Layer | Common Devices | Main Function |
| --- | --- | --- |
| Layer 1 - Physical | Hub, repeater, physical media converter | Signal transmission and regeneration |
| Layer 2 - Data Link | Bridge, Layer 2 switch, wireless AP | Frame forwarding using MAC addresses |
| Layer 3 - Network | Router, Layer 3 switch | IP packet forwarding |
| Layer 4 - Transport | Stateful firewall, Layer 4 load balancer | Transport-aware filtering and forwarding |
| Layer 7 - Application | Application proxy, Layer 7 load balancer, application gateway | Application-level processing |

Some devices operate across multiple OSI layers.

For example, a modern firewall may inspect Layer 3 and Layer 4 information while also analyzing Layer 7 application traffic.

The OSI model is a conceptual reference. Real networking devices often combine functions from several layers.

# 🗁 Comparison of Network Devices

| Device | Primary Function | Common OSI Layer |
| --- | --- | --- |
| NIC | Connects a host to a network | Layer 1 and Layer 2 |
| Hub | Repeats signals to connected ports | Layer 1 |
| Repeater | Regenerates signals | Layer 1 |
| Bridge | Connects LAN segments using MAC addresses | Layer 2 |
| Switch | Forwards frames using MAC addresses | Layer 2 |
| Router | Forwards packets between IP networks | Layer 3 |
| Gateway | Provides network entry or translation functionality | Depends on function |
| Modem | Adapts signals for a communication medium | Depends on technology |
| Wireless AP | Provides wireless LAN connectivity | Primarily Layer 2 |
| Firewall | Filters and controls network traffic | Multiple layers |
| Proxy Server | Intermediates client-server communication | Layer 7 in common application-proxy use |
| Load Balancer | Distributes traffic across backend systems | Layer 4 or Layer 7 |

# 🗁 How Network Devices Work Together

### 🖆 Example: A Typical Home Network

Consider a home network with:

* A laptop.
* A smartphone.
* A wireless access point.
* An Ethernet switch.
* A router.
* An ISP modem or ONT.

The network may look like this:

<pre>
          LAPTOP
             |
             | Wi-Fi
             |
        +----------+
        | ACCESS   |
        | POINT    |
        +----------+
             |
             | Ethernet
             |
          SWITCH
             |
             |
          ROUTER
             |
             |
        MODEM / ONT
             |
             |
          ISP NETWORK
             |
             |
          INTERNET
</pre>

### 🖆 Example: Accessing a Website

Suppose a laptop accesses a website hosted on a remote server.

The general communication process is:

* The laptop obtains an IP configuration through DHCP or manual configuration.
* It resolves the website hostname through DNS.
* It determines whether the destination IP address belongs to its local subnet.
* If the destination is remote, it sends the packet toward its default gateway.
* The access point or switch forwards the local network traffic.
* The router examines the destination IP address and selects a route.
* The firewall evaluates traffic according to its policies.
* The traffic passes through the ISP infrastructure.
* The remote server receives the request and sends a response.
* The response travels back through the network.

Actual packet paths depend on the network architecture, firewall placement, routing policies, and ISP infrastructure.

# 🗁 Choosing the Right Network Device

### 🖆 Connecting Multiple Computers

Use an Ethernet switch to connect multiple wired devices within a LAN.

### 🖆 Connecting Different IP Networks

Use a router or Layer 3 switch to forward traffic between IP networks.

### 🖆 Extending Wireless Connectivity

Use a wireless access point to provide Wi-Fi connectivity to an existing network.

### 🖆 Controlling Network Traffic

Use a firewall to enforce network security policies and control traffic.

### 🖆 Connecting to an ISP

Use the access equipment required by the ISP technology, such as a DSL modem, cable modem, or fiber ONT.

### 🖆 Distributing Application Traffic

Use a load balancer to distribute incoming requests across backend servers.

### 🖆 Managing Outbound Web Access

Use a forward proxy when centralized web access policies, filtering, or caching are required.

# 🗁 Common Mistakes

### 🖆 Confusing a Switch with a Router

A Layer 2 switch forwards Ethernet frames within a LAN.

A router forwards IP packets between networks.

A Layer 3 switch can perform both switching and routing functions.

### 🖆 Assuming Every Gateway Is a Router

A default gateway is commonly a router, but the term gateway also describes application and protocol translation systems.

### 🖆 Assuming a Wireless Router Is Only a Router

Many consumer wireless routers combine several functions, including:

* Router.
* Ethernet switch.
* Wireless access point.
* DHCP server.
* NAT.
* Firewall.

### 🖆 Assuming a Hub and Switch Work the Same Way

A hub repeats signals to its other ports.

A switch learns MAC addresses and selectively forwards Ethernet frames.

### 🖆 Assuming a Modem and ONT Are Identical

A modem and an Optical Network Terminal both provide access connectivity, but they use different technologies and perform different physical-layer functions.

### 🖆 Ignoring Device Capacity

When selecting networking equipment, consider:

* Interface speed.
* Forwarding capacity.
* Packet processing performance.
* Number of ports.
* VLAN support.
* Routing capabilities.
* PoE requirements.
* Security features.
* Redundancy and reliability.

# 🗁 Practical Exercises

### 🖆 Identify Network Devices

Identify the appropriate network device for each requirement:

* Connect ten computers through Ethernet.
* Connect two different IP subnets.
* Extend a wireless network into another office area.
* Filter traffic between an internal LAN and the internet.
* Distribute web requests across four backend servers.
* Regenerate signals over a communication link.
* Connect a fiber-optic subscriber connection to a customer router.

### 🖆 Inspect Network Interfaces in Ubuntu

List all interfaces:

```bash
ip link
```

Display interface IP addresses:

```bash
ip addr show
```

Display routing information:

```bash
ip route
```

Display the default gateway:

```bash
ip route show default
```

Display the ARP and Neighbor Discovery cache:

```bash
ip neigh
```

### 🖆 Build a Small LAN

Using a network simulator or available equipment:

* Connect three computers to a switch.
* Assign IP addresses within the same subnet.
* Test connectivity using `ping`.
* Inspect the MAC address table on the switch.
* Connect a router to the switch.
* Configure a second IP subnet.
* Test communication between the two subnets.

### 🖆 Build a Network with Redundancy

Create a topology containing:

* Two switches.
* Two routers.
* Four computers.
* Multiple connections between networking devices.

Investigate how redundant links behave and how routing or Spanning Tree Protocol can prevent connectivity problems or Layer 2 loops.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is a network device?**

A network device is hardware or a dedicated system that enables network connectivity, communication, forwarding, or traffic management.

**What is a NIC?**

A Network Interface Card provides a device with a network connection.

**What is the difference between a hub and a switch?**

A hub repeats signals to other ports, while a switch forwards Ethernet frames based on MAC addresses.

**What is a router?**

A router forwards IP packets between different networks using routing information.

**What is a wireless access point?**

A wireless access point connects Wi-Fi clients to a wired network.

### 🖆 Intermediate Questions

**Why does a switch maintain a MAC address table?**

To associate MAC addresses with switch ports and VLANs so that frames can be forwarded efficiently.

**What is the difference between a bridge and a switch?**

A switch is conceptually a multiport bridge with additional performance and management capabilities.

**What is a default gateway?**

It is the next-hop address a host uses to send traffic outside its local subnet when no more specific route exists.

**What is the difference between a modem and a router?**

A modem interfaces with a communication access technology, while a router forwards packets between IP networks.

**What is a Layer 3 switch?**

A switch that supports Layer 2 switching and Layer 3 routing functionality.

### 🖆 Advanced Questions

**Can a switch perform routing?**

Yes. A Layer 3 switch can route traffic between IP networks or VLANs.

**What happens when a switch receives a frame with an unknown destination MAC address?**

It generally floods the frame through eligible ports within the same VLAN, excluding the incoming port.

**What is the difference between a forward proxy and a reverse proxy?**

A forward proxy acts on behalf of clients, while a reverse proxy acts on behalf of backend servers.

**What is the difference between Layer 4 and Layer 7 load balancing?**

Layer 4 load balancing uses transport-layer information such as IP addresses and ports. Layer 7 load balancing can use application-level information such as HTTP paths, hostnames, and headers.

**Why is a router necessary for communication between different subnets?**

Hosts normally send traffic outside their local subnet to a router or another Layer 3 forwarding device, which selects the next hop.

# 🗁 References

* [Cisco Networking](https://www.cisco.com/)
* [Cisco Switching](https://www.cisco.com/site/us/en/learn/topics/networking/what-is-a-network-switch.html)
* [Cisco Routing](https://www.cisco.com/site/us/en/learn/topics/networking/what-is-a-router.html)
* [IEEE 802 Standards](https://standards.ieee.org/)
* [Wi-Fi Alliance](https://www.wi-fi.org/)
* [The Open Systems Interconnection Model - ITU-T X.200](https://www.itu.int/rec/T-REC-X.200)
* [Linux ip Command Documentation](https://man7.org/linux/man-pages/man8/ip.8.html)
