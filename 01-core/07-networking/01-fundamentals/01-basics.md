# Computer Networking Basics

A computer network is a collection of interconnected devices that communicate and exchange data through wired or wireless communication links.

These devices may include computers, smartphones, servers, printers, switches, routers, wireless access points, and Internet of Things (IoT) devices.

Networks enable users and applications to share information, access services, communicate remotely, and use centralized resources.

#### Real-World Example

When you open a website:

1. Your computer or smartphone connects to a local network.
2. The device determines how to reach the destination.
3. A router forwards traffic toward the Internet when the destination is remote.
4. Network infrastructure transports the traffic to the destination server.
5. The server processes the request and sends a response.
6. Your browser receives the response and displays the website.

This process involves several networking technologies working together.

# 🗁 Why Computer Networks Are Important

Computer networks provide:

* **Resource sharing:** Share printers, storage, applications, and other resources.
* **Communication:** Support email, messaging, voice calls, and video conferencing.
* **Centralized management:** Manage users, devices, and services centrally.
* **Remote access:** Access systems and services from different locations.
* **Scalability:** Connect additional users, devices, and services.
* **Reliability:** Use redundant links and devices to maintain connectivity.
* **Security controls:** Enforce access policies, authentication, segmentation, and traffic filtering.
* **Cost efficiency:** Share infrastructure and centralized services.

# 🗁 Basic Components of a Network

### End Devices

End devices are the systems that originate or consume network communication.

Examples:

* Desktop computers
* Laptops
* Smartphones
* Servers
* Network printers
* IP cameras
* Smart sensors

An end device commonly uses a network interface to connect to a network.

### Network Interface

A network interface provides connectivity between a device and a network.

Examples include:

* Ethernet network interface cards (NICs)
* Wi-Fi adapters
* Virtual network interfaces
* Fiber-optic network interfaces

![He](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fm.media-amazon.com%2Fimages%2FI%2F612II75jdSL._AC_SL1500_.jpg&f=1&nofb=1&ipt=564d21ff9d8d9ffe470ab9a500096b6daa269919dfd26a6dea050eb6dbb5cf54&ipo=images)

A network interface can have a link-layer address, such as a MAC address, and may be configured with one or more IP addresses.

### Transmission Media

Transmission media carry signals between network interfaces.

Examples:

* Twisted-pair copper cable
* Fiber-optic cable
* Radio waves used by Wi-Fi
* Microwave links

<figure>    <img src="https://res.cloudinary.com/du7tjwbyi/image/upload/v1793415818/Radio_Wave_yk384c.jpg" alt="Alt text" />    <figcaption>Figure: How Wi-Fi Networks Distribute Data via Radio Waves</figcaption></figure>

<figure>    <img src="https://res.cloudinary.com/du7tjwbyi/image/upload/v1793419203/Microwave_links1_auzqwu.jpg" alt="Alt text" />    <figcaption>Figure: Microwave Links and Two-Way Communication in Live Broadcasting</figcaption></figure>

### Intermediary Devices

Intermediary devices connect network segments, forward traffic, or enforce network policies.

Examples:

* Switches
* Routers
* Wireless access points
* Firewalls
* Network bridges

Different devices operate at different protocol layers and perform different functions.

### Network Services

Network services provide capabilities required by clients and applications.

Examples:

* DNS resolves domain names to IP addresses.
* DHCP automatically provides IP configuration.
* NTP synchronizes system clocks.
* Authentication services verify user or device identities.
* Web servers deliver web content.

# 🗁 How Network Communication Works

Consider a laptop accessing a web server.

```text
[LAPTOP]
    🠟
[SWITCH or WI-FI ACCESS POINT]
    🠟
[ROUTER]
    🠟
[ISP NETWORK] 🠊 [INTERNET] 🠊 [WEB SERVER]
```

A simplified communication sequence is:

1. The application generates data.
2. The operating system prepares the data for transmission using the appropriate protocols.
3. The device determines whether the destination is local or remote.
4. The network interface transmits the data.
5. Network devices forward the traffic toward the destination.
6. The receiving device processes the incoming data.
7. The application receives the data and responds as required.

The exact process depends on the protocols, network topology, and destination.

# 🗁 Fundamental Communication Concepts

### Sender and Receiver

* **Sender:** The device or application that transmits data.
* **Receiver:** The device or application that receives data.

The same device can act as both sender and receiver during a two-way communication session.

### Protocol

A protocol is a defined set of rules that governs communication between systems.

Examples:

| Protocol | Purpose                                                      |
| -------- | ------------------------------------------------------------ |
| Ethernet | Local wired network communication                            |
| IP       | Addressing and forwarding packets between networks           |
| TCP      | Reliable, ordered byte-stream transport                      |
| UDP      | Connectionless datagram transport                            |
| DNS      | Domain-name resolution                                       |
| HTTP     | Web application communication                                |
| TLS      | Cryptographic protection for supported application protocols |

### Addressing

Addresses identify interfaces, hosts, or services within particular communication systems.

Important address types include:

* **MAC address:** Used for link-layer delivery on technologies such as Ethernet.
* **IP address:** Used for logical addressing and routing.
* **Port number:** Identifies a transport-layer endpoint associated with an application or service.
* **Domain name:** A human-readable name that can be resolved through DNS.

These identifiers serve different purposes and are not interchangeable.

### Data Encapsulation

As application data moves through a protocol stack, each layer may add its own control information.

A simplified example is:

```text
Application data 🠊 TCP segment 🠊 IP packet 🠊 Ethernet frame 🠊 Physical signals
```

The receiving system processes the information in the reverse direction. This process is explored in detail in the Network Models section.

### Bandwidth and Throughput

* **Bandwidth:** A link's nominal capacity or a communication channel's specified capacity, depending on context.
* **Throughput:** The actual rate at which data is successfully transferred.
* **Goodput:** The rate of useful application data delivered, excluding protocol overhead and retransmitted data.

A 100 Mbps link does not guarantee 100 Mbps of application goodput.

### Latency

Latency is the time required for data to travel between endpoints or for a specified communication operation to complete.

Network delay can include:

* Processing delay
* Queuing delay
* Transmission delay
* Propagation delay

Round-trip time (RTT) measures the time for a signal or packet to travel to a destination and for a response to return.

### Packet Loss

Packet loss occurs when packets fail to reach their intended destination or are discarded before delivery.

Common causes include congestion, physical-layer errors, faulty equipment, routing problems, and intentional filtering.

### Jitter

Jitter describes variation in packet delay. It matters for real-time applications such as voice calls, video conferencing, and interactive media.

# 🗁 Local and Remote Communication

A device commonly communicates with two broad destination categories:

* **Local destination:** Reachable directly on the same logical IP subnet, subject to the network configuration.
* **Remote destination:** Requires forwarding through a router or another Layer 3 forwarding mechanism.

For example:

```text
Laptop IP:      192.168.10.20/24
Server IP:      192.168.10.50/24
Gateway:        192.168.10.1
Remote server:  203.0.113.10
```

The laptop can normally reach `192.168.10.50` directly at Layer 2 if both devices are in the same VLAN and no isolation policy prevents communication.

To reach `203.0.113.10`, the laptop normally sends the traffic to its default gateway.

The addresses in this example are documentation addresses, not recommended public destinations for real connectivity tests.

# 🗁 Client-Server and Peer-to-Peer Models

### Client-Server

A client requests a service from a server.

Examples:

* A browser requests a web page from a web server.
* A DNS client queries a DNS resolver.
* A computer requests an address from a DHCP server.

### Peer-to-Peer (P2P)

Peers communicate or share resources without every exchange depending on a single central application server.

Examples include certain file-sharing systems and distributed applications.

Client-server and peer-to-peer describe application or resource-sharing architectures; they do not define the physical network topology.

# 🗁 Network Performance Metrics

| Metric | Meaning | Typical unit |
| --- | --- | --- |
| Bandwidth | Specified or nominal capacity | bit/s |
| Throughput | Actual transfer rate | bit/s |
| Goodput | Useful application-data rate | bit/s |
| Latency | Delay | ms |
| RTT | Round-trip delay | ms |
| Packet loss | Missing or discarded packets | % |
| Jitter | Variation in packet delay | ms |
| Availability | Time a service is operational | % |

Always identify what a measurement represents. For example, bandwidth and throughput are not synonyms.

# 🗁 Basic Network Inspection

### Windows

Display IP configuration:

```powershell
ipconfig /all
```

Display MAC addresses:

```powershell
getmac /v
```

Inspect the route table:

```powershell
route print
```

Test reachability:

```powershell
ping 192.168.10.1
```

Inspect the IPv4 neighbor cache:

```powershell
arp -a
```

### Linux

Display IP addresses:

```bash
ip addr
```

Display network interfaces:

```bash
ip link
```

Display routes:

```bash
ip route
```

Inspect the neighbor cache:

```bash
ip neigh
```

Test reachability:

```bash
ping -c 4 192.168.10.1
```

Replace example addresses with the appropriate addresses for your own network.

# 🖆 Review Questions

#### What is a computer network?

A computer network is a collection of interconnected devices that communicate and exchange data through wired or wireless communication links.

#### What roles do end devices and intermediary devices play?

 **End devices** originate or consume network communication (e.g., computers, smartphones, servers).

**Intermediary devices** connect network segments, forward traffic, or enforce network policies (e.g., switches, routers, firewalls).

#### What is a network protocol?

A network protocol is a defined set of rules that governs communication between systems.

#### How do MAC addresses differ from IP addresses?

* A **MAC address** is a link-layer address used for local delivery (such as on Ethernet).
* An **IP address** is used for logical addressing and routing across networks.

#### What is the difference between bandwidth, throughput, and goodput?

* **Bandwidth:** A link's nominal capacity or a communication channel's specified capacity.
* **Throughput:** The actual rate at which data is successfully transferred.
* **Goodput:** The rate of useful application data delivered, excluding protocol overhead and retransmitted data.

#### What is latency, and how does it differ from jitter?

* **Latency** is the time required for data to travel between endpoints or for a specified communication operation to complete (delay).
* **Jitter** describes the variation in packet delay over time.

#### Why does a device use a default gateway?

A device uses a default gateway to forward traffic to remote destinations outside its own local IP subnet.

#### What is the difference between client-server and peer-to-peer architectures?

* **Client-server:** Clients request services from a centralized server.
* **Peer-to-peer (P2P):** Participants exchange resources or communicate as peers without depending on a single central application server.

#### Why does a successful ping not guarantee that a web service is working?

A successful ping only proves basic reachability (ICMP response); it does not guarantee that the specific application service or web server process is running and listening.

#### Which commands display network configuration on your operating system?

* **Windows:** `ipconfig /all`
* **Linux:** `ip addr` (or `ip link`, `ip route`)
