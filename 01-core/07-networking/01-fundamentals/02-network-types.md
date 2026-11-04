# Network Types

Networks can be classified according to geographic coverage, ownership, communication technology, access model, and purpose.

Understanding these classifications helps network engineers select appropriate architectures, technologies, security controls, and connectivity solutions.

# 🗁 Personal Area Network (PAN)

A Personal Area Network connects devices around an individual over a short distance.

**Examples:**

* A smartphone connected to wireless earbuds using Bluetooth.
* A laptop connected to a phone through a personal hotspot.
* A smartwatch communicating with a smartphone.

**Characteristics:**

* Short-range connectivity.
* Usually limited to personal devices.
* Commonly uses Bluetooth, USB, or other short-range technologies.

# 🗁 Local Area Network (LAN)

A Local Area Network connects devices within a limited area, such as a home, office, laboratory, or building.

**Examples:**

* Computers connected to an office switch.
* A home Ethernet network.
* A computer laboratory network.

**Characteristics:**

* Usually privately administered.
* Commonly uses Ethernet and Wi-Fi.
* Typically offers high capacity within the local environment.
* Can use VLANs to create separate logical broadcast domains.

A LAN can contain multiple IP subnets and VLANs. The terms LAN, IP subnet, and broadcast domain are related but not interchangeable.

## Difference between a LAN and VLAN

The difference between **LAN** and **VLAN**:

#### What is a LAN (Local Area Network)?

* It is a **physical** network.
* **Simple Example:** Suppose you have 5 computers in a room at your home or office. You connect them together using a network switch and cables—that is a LAN. Everything is physically connected with real wires right in front of you.

#### What is a VLAN (Virtual Local Area Network)?

* It is a **virtual or logical** network (created through software).
* **Simple Example:** Suppose you have 10 computers in the same room at your office. You want 5 of them to be for the 'Accounts' department and the other 5 for the 'IT' department. Instead of buying separate cables or switches, you use software inside the switch to divide them into two separate groups. This division is a VLAN.

**Main Differences at a Glance:**

| Feature                    | LAN (Local Area Network)                                                                    | VLAN (Virtual Local Area Network)                                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Structure**              | It is a real or **physical** connection (built with wires and switches).                    | It is a **virtual or logical** connection created through software.                                                               |
| **Security**               | Relatively lower, because everyone on the same network can access or see each other's data. | Much higher, because specific groups or departments can be kept isolated from one another (e.g., HR cannot access Finance files). |
| **Flexibility & Mobility** | If a computer is moved from one place to another, cabling often needs to be changed.        | No matter where the computer is moved, it can be kept in its original group simply through software configuration.                |

# 🗁Wireless Local Area Network (WLAN)

A WLAN is a LAN that uses wireless communication, commonly IEEE 802.11 Wi-Fi.

**Components:**

* Wireless clients.
* Access points (APs).
* Ethernet switching infrastructure.
* Optional routers, controllers, and authentication servers.

A WLAN can connect clients to a wired LAN through an access point.

Wi-Fi does not automatically eliminate the need for switching, routing, addressing, or security controls.

# 🗁 Metropolitan Area Network (MAN)

A Metropolitan Area Network interconnects networks across a city or metropolitan region.

**Examples:**

* A municipal fiber network.
* A service provider's metropolitan Ethernet infrastructure.
* Interconnected campuses across a city.

MAN is primarily a geographic classification, not a specific protocol.

# 🗁 Wide Area Network (WAN)

A Wide Area Network connects networks across large geographic areas.

**Examples:**

* A company connecting offices in multiple countries.
* An ISP connecting regional points of presence.
* A branch office connected to headquarters through a private WAN or VPN.

WAN technologies may include:

* Leased lines.
* MPLS-based services.
* Carrier Ethernet.
* Fiber-optic transport.
* Microwave links.
* Cellular connectivity.
* Internet-based VPNs.

A WAN can be privately operated, provider-managed, or built using a combination of services.

# 🗁 Global Area Network (GAN)

A Global Area Network describes connectivity spanning a global scale.

The term is less standardized in everyday network engineering than LAN and WAN. Global connectivity is commonly implemented through interconnected carrier networks, Internet backbones, satellite systems, and international transport infrastructure.

# 🗁 Storage Area Network (SAN)

A Storage Area Network provides specialized network connectivity between servers and shared storage systems.

Common technologies include:

* Fibre Channel.
* Fibre Channel over Ethernet (FCoE).
* Internet Small Computer Systems Interface (iSCSI) over Ethernet/IP.

SANs are designed for storage access, availability, and performance requirements. They are distinct from ordinary file-sharing networks, although both may use Ethernet/IP technologies.

# 🗁 Virtual Private Network (VPN)

A VPN creates a logical private communication path over an underlying network, often the public Internet.

Common applications:

* Remote access for employees.
* Site-to-site connectivity between offices.
* Secure administrative access.
* Interconnecting cloud and on-premises networks.

VPN technologies include IPsec, WireGuard, and OpenVPN.

A VPN is a logical connectivity solution rather than a geographic network category.

# 🗁 Intranet, Extranet, and Internet

### Intranet

A private organizational network or set of internal services accessible to authorized members.

### Extranet

A controlled extension of an organization's resources to external parties, such as suppliers, business partners, or customers.

### Internet

The global system of interconnected IP networks that exchange traffic using common protocols and routing arrangements.

The Internet is not a single network owned by one organization.

# 🗁 Client-Server and Peer-to-Peer Networks

These describe how services and resources are organized rather than how large a network is.

**Client-server:** Clients request services from servers.

**Peer-to-peer:** Participants exchange resources or communicate as peers, potentially without a single central application server.

A peer-to-peer application can still run over a LAN or the Internet.

# 🖆 Review Questions

#### How does a LAN differ from a WAN?

A **LAN (Local Area Network)** connects devices within a limited geographic area such as a home, office, or building.

A **WAN (Wide Area Network)** connects networks across large geographic areas such as multiple cities, countries, or regional points of presence.

#### Is every LAN wired?

No. While many LANs use wired Ethernet, a **WLAN (Wireless Local Area Network)** is a type of LAN that uses wireless communication (such as Wi-Fi) instead of physical wires.

#### What is the purpose of a SAN?

A **SAN (Storage Area Network)** provides specialized network connectivity between servers and shared storage systems to meet specific storage access, availability, and performance requirements.

#### Why is a VPN not simply another geographic network type?

A **VPN (Virtual Private Network)** is a logical connectivity solution that creates a private communication path over an underlying network (like the public Internet), rather than being defined by a specific physical or geographic boundary.

#### Can an organization use LAN, WAN, WLAN, and VPN technologies together?

Yes. Multiple network classifications and technologies can apply to the same organization simultaneously (e.g., using a LAN/WLAN locally, a WAN for branch offices, and a VPN for remote employees).

#### How do an intranet and an extranet differ?

An **intranet** is a private organizational network or internal service accessible only to authorized members of that organization.

An **extranet** is a controlled extension of an organization's resources shared with external parties, such as business partners, suppliers, or customers.
