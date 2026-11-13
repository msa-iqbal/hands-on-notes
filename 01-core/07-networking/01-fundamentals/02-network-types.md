# Network Types

Computer networks can be classified according to their geographical coverage, ownership, architecture, communication technology, and intended purpose.

Understanding different network types helps network engineers choose appropriate technologies, design network infrastructure, and determine how devices and locations should communicate.

This document covers the major network classifications, their characteristics, practical applications, and differences.

# 🗁 Classification of Networks

Networks are commonly classified based on geographical coverage.

The major network types include:

* Personal Area Network (PAN).
* Local Area Network (LAN).
* Wireless Local Area Network (WLAN).
* Campus Area Network (CAN).
* Metropolitan Area Network (MAN).
* Wide Area Network (WAN).
* Global Area Network (GAN).

Each network type serves different connectivity requirements.

# 🗁 Personal Area Network (PAN)

A Personal Area Network (PAN) connects personal devices within a relatively short distance around an individual.

PANs are commonly used to connect devices such as smartphones, laptops, headphones, smartwatches, and wireless peripherals.

### 🖆 Characteristics

* Covers a very small geographical area.
* Usually operates within a few meters.
* Supports communication between personal devices.
* Can use wired or wireless technologies.
* Typically requires relatively simple configuration.

### 🖆 Common Technologies

| Technology | Purpose |
| --- | --- |
| Bluetooth | Connects wireless headphones, keyboards, and other peripherals |
| USB | Provides wired communication between devices |
| NFC | Enables very short-range communication |
| Infrared | Supports short-range line-of-sight communication |

### 🖆 Example

A smartphone connected to wireless headphones through Bluetooth creates a simple PAN.

```text
             [Smartphone]
                  |
              Bluetooth
                  |
             [Headphones]
                  |
              Bluetooth
                  |
              [Smartwatch]
```

### 🖆 Practical Applications

* Connecting wireless headphones.
* Synchronizing smartwatches.
* Sharing files between nearby devices.
* Connecting wireless keyboards and mice.
* Communicating with personal IoT devices.

# 🗁 Local Area Network (LAN)

A Local Area Network (LAN) connects computers and other devices within a limited geographical area, such as a home, office, school, or laboratory.

LANs commonly use Ethernet switches, routers, and network cables to provide connectivity.

### 🖆 Characteristics

* Covers a limited geographical area.
* Usually owned and managed by an individual or organization.
* Supports high-speed communication.
* Provides local resource sharing.
* Typically has lower latency than long-distance networks.
* Can operate independently of an Internet connection.

### 🖆 Common Technologies

* Ethernet.
* IPv4 and IPv6.
* TCP and UDP.
* VLANs.
* Wi-Fi for wireless access.

### 🖆 Example

An office LAN connects computers, printers, servers, and other network devices.

```text
                  [Router]
                     |
                  [Switch]
              /      |      \
             /       |       \
          [PC-1]  [PC-2]  [Printer]
                     |
                  [Server]
```

All connected devices can communicate through the local network according to their addressing and security configurations.

### 🖆 Practical Applications

* Office networking.
* Home networking.
* Computer laboratories.
* File and printer sharing.
* Local database access.
* Internal application hosting.

# 🗁 Wireless Local Area Network (WLAN)

A Wireless Local Area Network (WLAN) provides local network connectivity through wireless communication rather than requiring a physical Ethernet connection for every client device.

WLANs commonly use Wi-Fi technologies based on the IEEE 802.11 family of standards.

### 🖆 Characteristics

* Provides wireless connectivity within a limited area.
* Allows devices to connect without Ethernet cables.
* Uses wireless access points.
* Supports mobile devices and roaming.
* Requires appropriate wireless security configuration.
* Can experience interference and signal degradation.

### 🖆 Common Technologies

* IEEE 802.11 standards.
* Wi-Fi 4, Wi-Fi 5, Wi-Fi 6, and newer generations.
* WPA2 and WPA3 security.
* 2.4 GHz, 5 GHz, and 6 GHz frequency bands where supported.

### 🖆 Example

A wireless router or access point connects laptops, smartphones, and tablets to the local network.

```text
                  [Router]
                     |
                [Access Point]
                     |
              Wireless Connection
                /     |      \
               /      |       \
           [Laptop] [Phone] [Tablet]
```

### 🖆 Practical Applications

* Home Wi-Fi networks.
* Office wireless connectivity.
* Public Wi-Fi hotspots.
* Educational institutions.
* Wireless IoT deployments.

### 🖆 LAN vs WLAN

| Feature | LAN | WLAN |
| --- | --- | --- |
| Connectivity | Commonly wired | Wireless |
| Medium | Ethernet cable | Radio frequency |
| Mobility | Limited by cable connection | Supports mobility within coverage |
| Interference | Generally less susceptible to radio interference | Susceptible to wireless interference |
| Security | Physical and network access controls | Requires wireless authentication and encryption |
| Typical technology | Ethernet | Wi-Fi |

A WLAN is a type of LAN. The distinction primarily concerns how devices connect to the local network.

# 🗁 Campus Area Network (CAN)

A Campus Area Network (CAN) connects multiple LANs across a geographically limited campus or organizational property.

It is commonly used by universities, hospitals, corporate campuses, and large educational institutions.

A CAN is larger than an individual building LAN but generally smaller than a metropolitan network.

### 🖆 Characteristics

* Connects multiple local networks.
* Covers a campus or a group of nearby buildings.
* Usually managed by a single organization.
* Commonly uses fiber-optic backbone connections.
* Supports centralized network management.
* Can use VLANs and routing to separate departments.

### 🖆 Example

A university connects its administration building, library, laboratories, and student accommodation through a campus backbone.

```text
                  [Core Router]
                       |
                 [Core Switch]
                       |
          +------------+------------+
          |            |            |
       [Building A] [Building B] [Building C]
          |            |            |
       [Switch]     [Switch]     [Switch]
          |            |            |
       [Clients]    [Clients]    [Clients]
```

Each building may have its own LAN, while the campus backbone provides communication between them.

### 🖆 Practical Applications

* University campuses.
* Hospital networks.
* Corporate office campuses.
* Industrial facilities.
* Research institutions.

# 🗁 Metropolitan Area Network (MAN)

A Metropolitan Area Network (MAN) connects multiple networks across a city or metropolitan region.

MANs are commonly operated by telecommunications companies, service providers, government organizations, and large enterprises.

### 🖆 Characteristics

* Covers a city or metropolitan region.
* Connects multiple LANs or campus networks.
* Often uses fiber-optic infrastructure.
* Can provide high-speed connectivity between geographically separated locations.
* May be operated by a telecommunications provider.
* Can support enterprise and service-provider services.

### 🖆 Common Technologies

* Metro Ethernet.
* Fiber-optic networks.
* Carrier Ethernet.
* MPLS-based services.
* Provider-managed optical transport.

### 🖆 Example

A company connects its offices across a city using a service-provider Metro Ethernet network.

```text
             [Head Office LAN]
                    |
                    |
              [Metro Network]
               /     |     \
              /      |      \
        [Branch A] [Branch B] [Branch C]
           LAN        LAN       LAN
```

The provider's metropolitan infrastructure carries traffic between the different office locations.

### 🖆 Practical Applications

* Connecting company branches across a city.
* Municipal government networks.
* University networks spanning multiple city locations.
* Telecommunications infrastructure.
* Metropolitan Internet services.

# 🗁 Wide Area Network (WAN)

A Wide Area Network (WAN) connects multiple networks across large geographical areas, such as countries, regions, or continents.

WANs allow organizations to connect geographically distributed offices, data centers, cloud environments, and remote users.

### 🖆 Characteristics

* Covers large geographical areas.
* Connects multiple LANs and other networks.
* Often depends on telecommunications service providers.
* Can use leased lines, fiber, MPLS, broadband, and wireless technologies.
* May involve higher latency than local networks.
* Requires routing between different networks.

### 🖆 Common Technologies

* MPLS.
* Leased lines.
* Metro Ethernet.
* Broadband Internet.
* IPsec VPN.
* SD-WAN.
* Satellite connectivity.
* Long-distance fiber-optic networks.

### 🖆 Example

A multinational company connects its offices in different countries through a WAN.

```text
               [Head Office]
                    |
                 [WAN]
              /     |     \
             /      |      \
       [Branch A] [Branch B] [Branch C]
          Asia      Europe    America
```

Each branch maintains its own local network, while WAN connectivity enables communication between locations.

### 🖆 Practical Applications

* Enterprise branch connectivity.
* Banking networks.
* International business infrastructure.
* Cloud connectivity.
* ISP backbone connectivity.
* Data center interconnection.

# 🗁 Global Area Network (GAN)

A Global Area Network (GAN) is a network architecture that provides connectivity across multiple geographical regions or worldwide locations.

The term describes global-scale connectivity rather than one universally standardized network technology.

GANs can involve interconnected WANs, satellite communication systems, undersea fiber cables, and international telecommunications infrastructure.

### 🖆 Characteristics

* Supports worldwide connectivity.
* Connects geographically distributed networks.
* Can use multiple telecommunications technologies.
* Requires coordination across network providers and regions.
* May involve complex routing and network management.

### 🖆 Example

A global organization operates data centers in Asia, Europe, and North America and connects them through international network infrastructure.

```text
             [Asia Data Center]
                    |
                    |
             [Global Network]
               /          \
              /            \
 [Europe Data Center]  [US Data Center]
```

### 🖆 Practical Applications

* Global enterprise networks.
* International telecommunications.
* Worldwide cloud infrastructure.
* Global content delivery networks.
* International satellite communication.

# 🗁 Other Important Network Types

Besides geographical classifications, networks can also be categorized by their architecture, purpose, or specialized technology.

### 🖆 Storage Area Network (SAN)

A Storage Area Network is a specialized high-speed network that provides servers with access to shared block-level storage.

SANs are commonly used in enterprise data centers.

Characteristics:

* Provides centralized storage connectivity.
* Supports block-level storage access.
* Separates storage traffic from ordinary user traffic.
* Can provide redundancy and high availability.

Common technologies include:

* Fibre Channel.
* iSCSI.
* Fibre Channel over Ethernet (FCoE).

Example:

```text
       [Server A]       [Server B]
            |                |
            +--------+-------+
                     |
                 [SAN Fabric]
                     |
              [Storage Array]
```

### 🖆 Virtual Private Network (VPN)

A Virtual Private Network creates a logical private communication path over an underlying network, commonly the public Internet.

VPN technologies can provide encryption, authentication, and secure connectivity between devices or networks.

Common VPN technologies include:

* WireGuard.
* IPsec.
* OpenVPN.
* SSL/TLS-based VPN solutions.

Example:

```text
       [Remote User]
             |
          Internet
             |
        [VPN Gateway]
             |
       [Private Network]
```

### 🖆 Virtual LAN (VLAN)

A Virtual LAN logically separates devices into different Layer 2 broadcast domains, even when they share the same physical switching infrastructure.

VLANs are commonly used to improve network segmentation, security, and traffic management.

Example:

```text
                [Managed Switch]
                  /          \
                 /            \
            VLAN 10          VLAN 20
             Sales            IT
           /      \         /    \
        [PC-1]  [PC-2]   [PC-3] [PC-4]
```

Although all devices connect to the same physical switch, VLAN membership separates their Layer 2 broadcast domains.

### 🖆 Software-Defined Wide Area Network (SD-WAN)

SD-WAN is an approach to managing WAN connectivity through centralized software-based policies and intelligent traffic steering.

It can use multiple underlying connections, such as broadband Internet, MPLS, and LTE or 5G.

Characteristics:

* Centralized policy management.
* Application-aware traffic steering.
* Multiple WAN transport options.
* Automated path selection.
* Improved visibility into WAN performance.

### 🖆 Content Delivery Network (CDN)

A Content Delivery Network is a geographically distributed system of servers that delivers content closer to end users.

CDNs reduce content delivery latency, improve availability, and reduce the load on origin servers.

Example:

A user in Bangladesh may receive website images and static assets from a nearby CDN edge location rather than retrieving every resource from a distant origin server.

# 🗁 Network Types Comparison

The following table summarizes the major network classifications.

| Network Type | Coverage | Typical Use |
| --- | --- | --- |
| PAN | Personal space | Connecting personal devices |
| LAN | Home, office, or building | Local device communication |
| WLAN | Local wireless coverage | Wi-Fi connectivity |
| CAN | Campus or organizational property | Connecting multiple building LANs |
| MAN | City or metropolitan region | Connecting networks across a city |
| WAN | Country, region, or continent | Connecting distant networks |
| GAN | Worldwide | Global network connectivity |
| SAN | Data center or storage infrastructure | Shared storage connectivity |
| VPN | Logical overlay across an underlying network | Secure remote or site-to-site connectivity |
| VLAN | Logical Layer 2 network | Network segmentation |
| SD-WAN | Multiple WAN locations | Software-managed WAN connectivity |
| CDN | Distributed global edge infrastructure | Efficient content delivery |

# 🗁 Choosing the Right Network Type

Network design depends on geographical coverage, performance requirements, security, cost, scalability, and operational requirements.

Consider the following scenarios.

| Scenario | Suitable Network Type |
| --- | --- |
| Connecting a smartphone to Bluetooth headphones | PAN |
| Connecting computers in a small office | LAN |
| Providing wireless connectivity in a home | WLAN |
| Connecting buildings within a university | CAN |
| Connecting branches across a city | MAN |
| Connecting offices across countries | WAN |
| Providing worldwide connectivity | GAN |
| Connecting servers to shared storage | SAN |
| Securely connecting remote employees | VPN |
| Separating departments on a switch | VLAN |
| Managing multiple enterprise WAN links | SD-WAN |
| Delivering website content globally | CDN |

A real-world infrastructure may combine several network types.

For example, an international company may use:

* LANs inside individual offices.
* WLANs for employee wireless access.
* VLANs for department segmentation.
* A CAN for its headquarters campus.
* A WAN or SD-WAN for branch connectivity.
* VPNs for remote employees.
* A SAN for data center storage.
* CDNs for delivering application content.

# 🗁 Practical Examples

### 🖆 Home Network

A typical home network contains:

* A router connected to an ISP.
* Ethernet-connected desktop computers.
* Wi-Fi-connected smartphones.
* Smart televisions and IoT devices.

This is primarily a LAN with a WLAN for wireless access.

### 🖆 University Network

A university may have:

* Separate LANs for administrative departments.
* WLAN coverage for students and employees.
* A campus backbone connecting buildings.
* VLANs separating students, staff, and servers.
* WAN connectivity to external networks.

This is a CAN built from multiple LANs and WLANs.

### 🖆 Internet Service Provider Network

An ISP may operate:

* Access networks for subscribers.
* Metropolitan fiber infrastructure.
* Regional aggregation networks.
* National backbone networks.
* International upstream connections.

These networks work together to deliver Internet connectivity to customers.

### 🖆 Multinational Enterprise Network

A multinational company may connect its global offices using WAN infrastructure.

Each office maintains a LAN, while centralized network services, VPNs, and SD-WAN policies provide secure communication between locations.

# 🗁 Common Misconceptions

### 🖆 LAN and WLAN Are Completely Different Network Categories

A WLAN is a wireless form of local area networking. Both provide local network connectivity, but they use different access media.

### 🖆 WAN Always Means the Internet

A WAN is not necessarily the public Internet. An organization can operate a private WAN using dedicated connectivity or provider-managed services.

### 🖆 MAN Is Always Owned by a Government

A metropolitan network may be owned or operated by telecommunications companies, enterprises, governments, or other organizations.

### 🖆 VPN Is a Physical Network Type

A VPN is generally a logical network overlay that operates over an underlying physical or virtual network.

### 🖆 VLAN Requires Separate Physical Switches

VLANs allow multiple logical Layer 2 networks to operate on shared physical switching infrastructure.

# 🗁 Practical Exercises

### 🖆 Exercise: Classify Network Scenarios

Identify the most appropriate network type for each scenario.

* A smartphone communicates with a smartwatch using Bluetooth.
* A company connects computers within one office.
* A university connects five buildings through fiber.
* A bank connects branches across different countries.
* A data center connects servers to shared storage.
* A remote employee securely accesses the company network.
* A switch separates HR and Finance devices into different broadcast domains.

### 🖆 Exercise: Design a Small Office Network

Design a network for an office with:

* Ten desktop computers.
* Five wireless laptops.
* Two network printers.
* One file server.
* One router.
* One managed switch.
* One wireless access point.

Identify:

* The network type.
* Required network devices.
* Wired and wireless connections.
* IP addressing requirements.
* Whether VLANs are necessary.
* How the network connects to the Internet.

### 🖆 Exercise: Design a University Campus Network

Create a conceptual design for a university with three buildings.

Requirements:

* Each building has its own LAN.
* All buildings connect through a fiber backbone.
* Students and staff use separate VLANs.
* Wireless access is available throughout the campus.
* Internet connectivity is provided through a central gateway.

Identify the network types and technologies involved.

# 🗁 Interview Questions

**What is a PAN?**

A Personal Area Network connects personal devices over a short distance, commonly using Bluetooth, USB, or NFC.

**What is a LAN?**

A Local Area Network connects devices within a limited geographical area, such as a home, office, or building.

**What is the difference between LAN and WLAN?**

A LAN provides local network connectivity, commonly through Ethernet, while a WLAN provides local connectivity through wireless technologies such as Wi-Fi.

**What is a CAN?**

A Campus Area Network connects multiple LANs across a campus or organizational property.

**What is a MAN?**

A Metropolitan Area Network connects networks across a city or metropolitan region.

**What is a WAN?**

A Wide Area Network connects multiple networks across large geographical areas, such as countries or continents.

**What is a GAN?**

A Global Area Network describes network infrastructure that provides connectivity across worldwide geographical regions.

**What is the difference between LAN and WAN?**

A LAN covers a limited geographical area, while a WAN connects networks across much larger distances.

**What is a SAN?**

A Storage Area Network is a specialized network that provides servers with access to shared block-level storage.

**What is a VPN?**

A Virtual Private Network provides a logical private communication path over an underlying network, often using encryption and authentication.

**What is a VLAN?**

A Virtual LAN logically separates devices into different Layer 2 broadcast domains on shared switching infrastructure.

**What is SD-WAN?**

SD-WAN uses software-defined policies to manage WAN connections and intelligently direct network traffic across available transport links.

**Can an organization use multiple network types simultaneously?**

Yes. Organizations commonly combine LAN, WLAN, VLAN, CAN, WAN, VPN, and other technologies to build their network infrastructure.

# 🗁 References

* [Cisco Networking](https://www.cisco.com/)
* [Cloudflare Learning Center](https://www.cloudflare.com/learning/)
* [IEEE 802 Standards](https://standards.ieee.org/)
* [Wi-Fi Alliance](https://www.wi-fi.org/)
* [Internet Engineering Task Force (IETF)](https://www.ietf.org/)
* [MikroTik Documentation](https://help.mikrotik.com/docs/)
