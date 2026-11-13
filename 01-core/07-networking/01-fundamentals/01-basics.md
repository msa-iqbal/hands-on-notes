# Networking Basics

Networking is the process of connecting two or more computing devices so they can communicate, exchange data, and share resources.

A computer network allows devices such as computers, servers, smartphones, printers, routers, and switches to communicate through wired or wireless connections.

Networking is the foundation of the Internet, cloud computing, distributed systems, web applications, and modern IT infrastructure.

# 🗁 What Is Computer Networking?

Computer networking refers to the technologies, protocols, hardware, and software that enable devices to exchange information.

A network can be as simple as two computers connected directly or as complex as the global Internet connecting billions of devices.

**Example:**

Consider a small office with:

* Five desktop computers.
* One network printer.
* One router.
* One network switch.
* One Internet connection.

All computers can communicate through the local network, access the shared printer, and connect to the Internet through the router.

```text
                  Internet
                     |
                  [Router]
                     |
                  [Switch]
                /    |    \
               /     |     \
            [PC-1] [PC-2] [PC-3]
                     |
                 [Printer]
```

This is a basic example of a local area network.

# 🗁 Why Do We Need Networking?

Networking makes it possible for devices and applications to communicate without requiring direct physical interaction.

Common purposes include:

### 🖆 Resource Sharing

Devices can share resources such as:

* Printers.
* Storage devices.
* Files and folders.
* Internet connections.
* Applications and services.

### 🖆 Communication

Networks enable communication between users and systems through:

* Email.
* Instant messaging.
* Voice over IP (VoIP).
* Video conferencing.
* Collaboration platforms.

### 🖆 Data Exchange

Applications use networks to exchange information between clients, servers, databases, and external services.

For example, when a browser requests a webpage, it communicates with a web server over a network.

### 🖆 Centralized Management

Organizations can manage users, devices, applications, and resources through centralized network infrastructure.

### 🖆 Scalability

Networks allow organizations to add more devices, services, users, and locations without redesigning the entire infrastructure.

### 🖆 Remote Access

Users can access organizational resources from remote locations through technologies such as VPNs and secure remote-access services.

# 🗁 Basic Components of a Network

A computer network consists of several components that work together to provide communication.

### 🖆 Network Devices

Physical or virtual devices that connect networks and forward data.

Common examples:

| Device | Purpose |
| --- | --- |
| Router | Connects different IP networks |
| Switch | Connects devices within a local network |
| Hub | Repeats incoming signals to all ports |
| Bridge | Connects network segments at Layer 2 |
| Access Point | Provides wireless network connectivity |
| Modem | Converts signals for a particular access technology |
| Firewall | Controls network traffic according to security rules |
| Network Interface Card (NIC) | Provides a device with network connectivity |

### 🖆 Network Interfaces

A network interface is the point through which a device connects to a network.

Examples include:

* Ethernet interfaces.
* Wireless interfaces.
* Fiber-optic interfaces.
* Virtual network interfaces.

On Linux, you can inspect network interfaces using:

```bash
ip link show
```

To display IP addresses assigned to interfaces:

```bash
ip addr show
```

### 🖆 Transmission Media

Transmission media provide the path through which data travels.

Common types include:

* Twisted-pair copper cables.
* Coaxial cables.
* Fiber-optic cables.
* Wireless radio signals.

### 🖆 Network Protocols

Protocols define the rules that devices follow when communicating.

Examples include:

| Protocol | Purpose |
| --- | --- |
| Ethernet | Wired local network communication |
| IP | Logical addressing and packet delivery |
| TCP | Reliable, connection-oriented transport |
| UDP | Connectionless transport |
| HTTP | Web communication |
| HTTPS | Encrypted web communication |
| DNS | Domain-name resolution |
| DHCP | Automatic IP configuration |

### 🖆 Network Software

Network software enables devices to communicate and provides services to users and applications.

Examples include:

* Network operating systems.
* Network drivers.
* DNS and DHCP services.
* Web servers.
* Firewall software.
* Network monitoring tools.

# 🗁 How Does Network Communication Work?

Network communication involves sending data from a source device to a destination device.

Consider what happens when a computer accesses a website.

```text
       Client Computer
              |
              | HTTP/HTTPS Request
              |
          [Router]
              |
              | Internet
              |
        [Destination Server]
              |
              | HTTP/HTTPS Response
              |
          [Router]
              |
              |
       Client Computer
```

The communication process can be understood through these stages:

### 🖆 Source Generates Data

An application creates data that needs to be transmitted.

For example, a browser generates an HTTP request when a user opens a website.

### 🖆 Data Is Encapsulated

Networking protocols add the information required for communication.

Depending on the protocol, this information can include:

* Source and destination addresses.
* Source and destination ports.
* Protocol identifiers.
* Error-checking information.

### 🖆 Data Travels Through the Network

The operating system sends the data through an appropriate network interface.

Switches forward Ethernet frames within local networks, while routers forward IP packets between networks.

### 🖆 Destination Receives Data

The destination device receives the transmitted data and processes it according to the relevant network protocols.

### 🖆 Response Is Returned

The destination application may generate a response, which travels back through the network to the original device.

# 🗁 Types of Network Communication

Network communication can be classified according to how devices exchange information.

### 🖆 Unicast

Unicast is communication between one sender and one specific receiver.

Example:

A computer sends a request to a web server.

```text
[Sender] ----------> [Receiver]
```

### 🖆 Broadcast

Broadcast is communication in which one sender transmits data to all devices within the relevant broadcast domain.

Example:

An IPv4 ARP request is commonly sent as an Ethernet broadcast.

```text
                  [Device A]
                      |
        +-------------+-------------+
        |             |             |
     [Device B]   [Device C]   [Device D]
```

### 🖆 Multicast

Multicast allows one sender to transmit data to a specific group of interested receivers.

Example:

A multicast video stream can be delivered to multiple subscribed devices without sending an independent stream to every receiver.

```text
                  [Sender]
                      |
                [Multicast Group]
                  /    |    \
                 /     |     \
              [PC-1] [PC-2] [PC-3]
```

These communication methods are explored in detail in the dedicated networking fundamentals topics.

# 🗁 Network Addressing Fundamentals

Devices need addresses so that network communication can identify the intended destination.

Two important addressing concepts are MAC addresses and IP addresses.

### 🖆 MAC Address

A Media Access Control (MAC) address is a link-layer address associated with a network interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

MAC addresses are primarily used for communication within a local Layer 2 network.

### 🖆 IP Address

An Internet Protocol (IP) address identifies a network interface within an IP network.

IPv4 example:

```text
192.168.1.10
```

IPv6 example:

```text
2001:db8:1::10
```

IP addresses are used for logical addressing and packet delivery across interconnected networks.

### 🖆 Port Number

A port number identifies a communication endpoint associated with a transport-layer protocol.

Examples:

| Port | Common Service |
| --- | --- |
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |

A typical network connection can be described using:

* Source IP address.
* Destination IP address.
* Transport protocol.
* Source port.
* Destination port.

For TCP and UDP, these values help identify communication flows between applications.

# 🗁 Client-Server Architecture

Client-server architecture is a networking model in which clients request services and servers provide them.

### 🖆 Client

A client is a device or application that requests resources or services.

Examples:

* Web browsers.
* Mobile applications.
* Email clients.
* Database clients.

### 🖆 Server

A server is a system or application that provides resources or services to clients.

Examples:

* Web servers.
* Database servers.
* DNS servers.
* File servers.
* Mail servers.

**Example: Web Application**

```text
             +----------------+
             |  Web Browser   |
             |    Client      |
             +----------------+
                     |
                     | HTTPS
                     |
             +----------------+
             |   Web Server   |
             +----------------+
                     |
                     | Database Query
                     |
             +----------------+
             | Database Server|
             +----------------+
```

When a user opens a web application:

* The browser sends an HTTP request.
* The web server processes the request.
* The web server may communicate with a database.
* The server returns an HTTP response.
* The browser displays the result.

A single machine can act as both a client and a server depending on the communication taking place.

# 🗁 Peer-to-Peer Networking

Peer-to-peer (P2P) networking is a communication model in which devices can communicate and share resources directly without requiring a dedicated central server for every interaction.

Each participating device is called a peer.

```text
              [Peer A]
              /      \
             /        \
        [Peer B] ---- [Peer C]
             \        /
              \      /
              [Peer D]
```

Common examples include:

* Peer-to-peer file sharing.
* Distributed communication systems.
* Blockchain networks.
* Some multiplayer networking architectures.

### 🖆 Client-Server vs Peer-to-Peer

| Feature | Client-Server | Peer-to-Peer |
| --- | --- | --- |
| Architecture | Centralized service provider | Distributed peers |
| Communication | Clients request server services | Peers communicate directly |
| Management | Usually centrally managed | Often distributed |
| Scalability | Depends on server infrastructure | Depends on peer participation and design |
| Failure impact | Server failure may affect clients | Individual peer failure may have limited impact |
| Example | Web application | BitTorrent |

# 🗁 Network Topology Overview

Network topology describes how network devices and connections are arranged.

Two important categories are:

### 🖆 Physical Topology

Physical topology describes the actual physical arrangement of devices, cables, and network connections.

### 🖆 Logical Topology

Logical topology describes how data flows between devices, regardless of their physical arrangement.

Common topology types include:

| Topology | Description |
| --- | --- |
| Bus | Devices share a common communication medium |
| Star | Devices connect through a central device |
| Ring | Devices form a closed communication loop |
| Mesh | Devices have multiple interconnections |
| Tree | Devices are arranged in a hierarchical structure |
| Hybrid | Combines multiple topology types |

Detailed topology concepts are covered in the network topologies documentation.

# 🗁 LAN, WAN, and the Internet

Networks can be categorized according to their geographical coverage and purpose.

### 🖆 Local Area Network (LAN)

A LAN connects devices within a limited geographical area.

Examples:

* Home networks.
* Office networks.
* School networks.
* Laboratory networks.

### 🖆 Wide Area Network (WAN)

A WAN connects networks across larger geographical areas.

Examples:

* Networks connecting multiple company branches.
* ISP backbone networks.
* Enterprise networks spanning multiple regions.

### 🖆 Internet

The Internet is a global system of interconnected networks that communicate using the TCP/IP protocol suite.

It allows devices and services around the world to exchange information.

### 🖆 Intranet

An intranet is a private network or collection of services accessible within an organization.

### 🖆 Extranet

An extranet provides controlled access to selected internal resources for authorized external users or organizations.

# 🗁 Network Performance Fundamentals

Network performance describes how effectively a network transfers data and responds to communication requests.

Important performance metrics include:

### 🖆 Bandwidth

Bandwidth represents the maximum data-transfer capacity of a network connection under specified conditions.

It is commonly measured in:

* Kbps.
* Mbps.
* Gbps.
* Tbps.

### 🖆 Throughput

Throughput is the actual rate at which data is successfully transferred over a network.

### 🖆 Latency

Latency is the time required for data to travel between two points.

It is commonly measured in milliseconds (ms).

### 🖆 Jitter

Jitter describes variations in packet delay over time.

High jitter can negatively affect real-time applications such as voice and video communication.

### 🖆 Packet Loss

Packet loss occurs when transmitted packets fail to reach their intended destination.

It can result from congestion, transmission errors, faulty equipment, or other network problems.

These performance metrics are explored in greater detail in the QoS and bandwidth section.

# 🗁 Essential Networking Commands in Ubuntu

Ubuntu provides several command-line utilities for inspecting and troubleshooting network connectivity.

### 🖆 Display Network Interfaces

```bash
ip link show
```

Displays available network interfaces and their link-layer status.

### 🖆 Display IP Addresses

```bash
ip addr show
```

Displays IPv4 and IPv6 addresses assigned to interfaces.

### 🖆 Display Routing Table

```bash
ip route show
```

Displays the IPv4 routing table.

For IPv6:

```bash
ip -6 route show
```

### 🖆 Test Network Connectivity

```bash
ping -c 4 8.8.8.8
```

Sends four ICMP Echo Requests to the specified destination.

### 🖆 Trace the Network Path

Install traceroute if necessary:

```bash
sudo apt update
sudo apt install traceroute
```

Trace the path to a destination:

```bash
traceroute google.com
```

### 🖆 Inspect DNS Resolution

Install DNS utilities if necessary:

```bash
sudo apt install dnsutils
```

Query a DNS server:

```bash
dig google.com
```

### 🖆 Display Listening Ports

```bash
ss -tuln
```

Displays listening TCP and UDP sockets.

### 🖆 Display Network Connections

```bash
ss -tunap
```

Displays TCP and UDP sockets with process information where permissions allow.

### 🖆 Display the ARP and Neighbor Table

```bash
ip neigh show
```

Displays the kernel's neighbor table, including IPv4 ARP and IPv6 neighbor information.

# 🗁 Common Networking Terms

| Term | Meaning |
| --- | --- |
| Node | Any device or endpoint connected to a network |
| Host | A network-connected device that can send or receive data |
| Client | A device or application requesting a service |
| Server | A system providing services or resources |
| Packet | A unit of data at the network layer |
| Frame | A unit of data at the data-link layer |
| Segment | Commonly refers to a TCP transport-layer data unit |
| Datagram | A connectionless transport-layer data unit, commonly associated with UDP |
| Protocol | A set of rules governing communication |
| Interface | A connection point between a device and a network |
| Gateway | A device or system that provides a path to another network |
| Subnet | A logical subdivision of an IP network |
| Bandwidth | Available data-transfer capacity |
| Throughput | Actual achieved data-transfer rate |
| Latency | Communication delay |
| Packet Loss | Failure of transmitted packets to reach their destination |

# 🗁 Common Networking Mistakes

### 🖆 Confusing MAC Addresses with IP Addresses

A MAC address is used for link-layer communication, while an IP address provides logical addressing at the network layer.

They serve different purposes.

### 🖆 Assuming a Switch and Router Are the Same

A traditional Layer 2 switch forwards Ethernet frames using MAC addresses.

A router forwards IP packets between networks using routing information.

Some modern devices combine switching and routing functionality, but the underlying roles remain distinct.

### 🖆 Confusing Bandwidth with Throughput

Bandwidth represents capacity, while throughput represents the actual transfer rate achieved.

A connection with high bandwidth does not necessarily guarantee high throughput.

### 🖆 Assuming Successful Ping Means Everything Works

Ping tests ICMP reachability, not whether every application, port, or service is functioning correctly.

A server may respond to HTTPS requests while blocking ICMP Echo Requests.

### 🖆 Ignoring DNS

A network connection can have working IP connectivity but still fail to access websites because of DNS resolution problems.

Always distinguish between IP connectivity and name resolution.

### 🖆 Ignoring the Default Gateway

A device generally needs an appropriate default gateway to communicate with destinations outside its directly connected IP network.

Incorrect gateway configuration can prevent external connectivity.

# 🗁 Practical Exercises

Use Ubuntu Linux or a virtual machine to complete these exercises.

### 🖆 Exercise: Inspect Your Network Interfaces

Run:

```bash
ip link show
ip addr show
```

Identify:

* Your Ethernet interface, if present.
* Your wireless interface, if present.
* The loopback interface.
* The MAC address of each interface.
* The assigned IPv4 and IPv6 addresses.

### 🖆 Exercise: Inspect Your Routing Table

Run:

```bash
ip route show
```

Identify:

* The default route.
* The gateway address.
* Directly connected networks.
* The interface used for each route.

### 🖆 Exercise: Test Connectivity

Test an external IP address:

```bash
ping -c 4 8.8.8.8
```

Then test a domain name:

```bash
ping -c 4 google.com
```

Compare the results and determine whether IP connectivity and DNS resolution are working.

### 🖆 Exercise: Inspect Listening Services

Run:

```bash
ss -tuln
```

Identify:

* Listening TCP ports.
* Listening UDP ports.
* Services bound to `127.0.0.1`.
* Services listening on all IPv4 interfaces (`0.0.0.0`).

### 🖆 Exercise: Build a Small Virtual Network

Create a simple network using VirtualBox or another virtualization platform.

Requirements:

* Create two Ubuntu virtual machines.
* Connect both machines to the same virtual network.
* Assign IP addresses within the same subnet.
* Test communication using `ping`.
* Inspect their neighbor tables using `ip neigh show`.

Record the IP addresses, interfaces, and test results.

# 🗁 Interview Questions

**What is computer networking?**

Computer networking is the practice of connecting devices so they can communicate and exchange data through communication protocols and network infrastructure.

**Why do we need computer networks?**

Networks enable resource sharing, communication, data exchange, centralized management, remote access, and scalable infrastructure.

**What is the difference between a client and a server?**

A client requests services or resources, while a server provides them.

**What is the difference between a MAC address and an IP address?**

A MAC address is a link-layer address used for local network communication, whereas an IP address is a logical address used for communication across IP networks.

**What is the difference between a switch and a router?**

A switch primarily forwards Ethernet frames within a Layer 2 network, while a router forwards IP packets between different networks.

**What is the difference between LAN and WAN?**

A LAN connects devices within a limited geographical area, while a WAN connects networks across larger geographical areas.

**What is the Internet?**

The Internet is a global collection of interconnected networks that communicate using the TCP/IP protocol suite.

**What is a network protocol?**

A network protocol defines the rules, formats, and procedures that devices follow to communicate.

**What is the difference between bandwidth and throughput?**

Bandwidth describes the capacity of a connection, while throughput measures the actual data-transfer rate achieved.

**What is latency?**

Latency is the time required for data to travel between two points in a network.

**What is the difference between unicast, broadcast, and multicast?**

Unicast communicates with one receiver, broadcast communicates with all devices in a broadcast domain, and multicast communicates with a specific group of receivers.

**What is the purpose of a default gateway?**

A default gateway provides a path for sending traffic to destinations outside a device's directly connected networks.

# 🗁 References

* [Cisco Networking Basics](https://www.cisco.com/)
* [Cloudflare Learning Center](https://www.cloudflare.com/learning/)
* [IBM Networking Fundamentals](https://www.ibm.com/think/topics/networking)
* [Ubuntu Server Networking Documentation](https://documentation.ubuntu.com/server/how-to/networking/)
* [Linux iproute2 Documentation](https://www.kernel.org/doc/html/latest/networking/)
