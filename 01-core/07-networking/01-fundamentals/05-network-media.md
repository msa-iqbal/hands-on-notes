# Network Media

Network media refers to the physical or wireless communication channels through which data travels between network devices.

It provides the transmission path that connects computers, switches, routers, servers, access points, and other networking equipment.

Network media is generally classified into two major categories:

* **Guided Media:** Data travels through a physical transmission medium, such as copper cables or fiber-optic cables.
* **Unguided Media:** Data travels through electromagnetic waves without requiring a physical cable, such as Wi-Fi, radio, and satellite communication.

Choosing the appropriate network media affects:

* Transmission speed.
* Available bandwidth.
* Maximum transmission distance.
* Signal quality.
* Latency.
* Interference resistance.
* Installation cost.
* Network reliability.
* Security considerations.

# 🗁 Types of Network Media

### 🖆 Guided Media

Guided media uses a physical medium to carry signals between devices.

Common examples include:

* Twisted pair cable.
* Coaxial cable.
* Fiber-optic cable.

The physical medium provides a defined transmission path.

### 🖆 Unguided Media

Unguided media transmits data through electromagnetic waves without a physical cable connecting the communicating devices.

Common examples include:

* Radio waves.
* Wi-Fi.
* Microwave communication.
* Infrared communication.
* Satellite communication.
* Cellular networks.

### 🖆 Network Media Classification

<pre>
                  NETWORK MEDIA
                       |
             +---------+---------+
             |                   |
        GUIDED MEDIA        UNGUIDED MEDIA
             |                   |
       +-----+-----+       +-----+------+
       |     |     |       |     |      |
     UTP   Coax  Fiber   Radio Wi-Fi Satellite
     STP          Optic
</pre>

# 🗁 Twisted Pair Cable

### 🖆 What Is Twisted Pair Cable?

Twisted pair cable is a copper-based network transmission medium consisting of pairs of insulated copper wires twisted around each other.

The twisting reduces electromagnetic interference and crosstalk between adjacent wire pairs.

Twisted pair cable is widely used in Ethernet networks.

<AsyncImage query="twisted pair Ethernet cable cutaway showing four color coded copper twisted pairs RJ45 connector" aspectRatio="16:9" width="100%" />

### 🖆 Cable Structure

A typical Ethernet twisted pair cable contains:

* Eight insulated copper conductors.
* Four twisted wire pairs.
* An outer protective jacket.
* Optional shielding, depending on cable type.

Each pair uses different wire colors to help identify the conductors.

Common color combinations include:

* Orange and white-orange.
* Green and white-green.
* Blue and white-blue.
* Brown and white-brown.

### 🖆 Why Are Wires Twisted?

Twisting helps reduce electromagnetic interference.

When two conductors carry balanced signals, external interference tends to affect both conductors similarly.

The receiver can reject much of this common-mode noise, improving signal quality.

Twisting also reduces crosstalk between neighboring wire pairs.

### 🖆 Types of Twisted Pair Cable

**UTP - Unshielded Twisted Pair**

UTP cable does not use additional metallic shielding around the wire pairs.

Characteristics:

* Commonly used in Ethernet LANs.
* Relatively inexpensive.
* Flexible and easy to install.
* Relies on balanced transmission and pair twisting to reduce interference.

**STP - Shielded Twisted Pair**

STP is a commonly used general term for shielded twisted-pair cabling.

Shielding can reduce susceptibility to external electromagnetic interference when correctly selected, installed, and grounded.

Characteristics:

* Uses metallic shielding.
* Suitable for environments with significant electromagnetic interference.
* Usually more expensive than unshielded alternatives.
* Requires proper installation and grounding practices.

### 🖆 Common Shielding Designations

| Designation | Meaning |
| --- | --- |
| U/UTP | No overall shield and no individual pair shields |
| F/UTP | Overall foil shield, unshielded pairs |
| S/UTP | Overall braided shield, unshielded pairs |
| U/FTP | No overall shield, individually foil-shielded pairs |
| F/FTP | Overall foil shield and individually foil-shielded pairs |
| S/FTP | Overall braided shield and individually foil-shielded pairs |

These designations describe the shielding arrangement of the cable.

### 🖆 Ethernet Cable Categories

Twisted pair Ethernet cables are classified into categories that define performance requirements.

| Category | Typical Supported Ethernet Application | Bandwidth |
| --- | --- | --- |
| Cat 5e | 1000BASE-T | 100 MHz |
| Cat 6 | 1000BASE-T and 10GBASE-T over shorter distances | 250 MHz |
| Cat 6A | 10GBASE-T up to 100 meters | 500 MHz |
| Cat 7 | Class F cabling systems | 600 MHz |
| Cat 7A | Class FA cabling systems | 1000 MHz |
| Cat 8 | 25GBASE-T and 40GBASE-T up to 30 meters | 2000 MHz |

Cat 7 and Cat 7A are ISO/IEC cabling classes and are not standard TIA-recognized Ethernet categories in the same way as Cat 5e, Cat 6, and Cat 6A.

Actual supported Ethernet speeds depend on the cable, installation quality, link length, and connected equipment.

### 🖆 Ethernet Cable Distance

For common balanced twisted-pair Ethernet installations, the structured cabling channel is generally limited to 100 meters.

A typical channel consists of:

* Up to 90 meters of permanent horizontal cable.
* Up to 10 meters of patch cords combined.

The actual limits depend on the Ethernet standard and cabling specifications.

### 🖆 RJ45 Connector

Ethernet twisted-pair cables commonly use an 8-position modular connector, often called RJ45 in everyday networking terminology.

<AsyncImage query="RJ45 8P8C Ethernet connector transparent view with eight copper contacts" aspectRatio="16:9" width="100%" />

The connector provides eight contact positions for the cable conductors.

### 🖆 T568A and T568B Wiring Standards

T568A and T568B define the conductor pin assignments used in structured Ethernet cabling.

**T568A**

| Pin | Wire Color |
| ---: | --- |
| 1 | White/Green |
| 2 | Green |
| 3 | White/Orange |
| 4 | Blue |
| 5 | White/Blue |
| 6 | Orange |
| 7 | White/Brown |
| 8 | Brown |

**T568B**

| Pin | Wire Color |
| ---: | --- |
| 1 | White/Orange |
| 2 | Orange |
| 3 | White/Green |
| 4 | Blue |
| 5 | White/Blue |
| 6 | Green |
| 7 | White/Brown |
| 8 | Brown |

Both standards are valid. Consistency throughout an installation is important.

### 🖆 Straight-Through Cable

A straight-through cable uses the same wiring standard at both ends.

Examples:

* T568A to T568A.
* T568B to T568B.

Traditionally, straight-through cables connected unlike devices, such as a computer to a switch.

Modern Ethernet equipment commonly supports Auto MDI-X, which automatically handles transmit and receive pair assignments.

### 🖆 Crossover Cable

A crossover cable uses different wiring standards at opposite ends.

Example:

* T568A on one end.
* T568B on the other end.

Traditionally, crossover cables connected similar Ethernet interface types, such as switch-to-switch connections.

Auto MDI-X has made dedicated crossover cables unnecessary for most modern Ethernet installations.

# 🗁 Coaxial Cable

### 🖆 What Is Coaxial Cable?

Coaxial cable is a copper-based transmission medium consisting of a central conductor surrounded by insulating material, metallic shielding, and an outer protective jacket.

The central conductor and surrounding shield share a common axis, which gives coaxial cable its name.

<AsyncImage query="coaxial cable cutaway cross section center conductor dielectric insulation braided shielding outer jacket" aspectRatio="16:9" width="100%" />

### 🖆 Structure of Coaxial Cable

A typical coaxial cable contains:

* **Central Conductor:** Carries the electrical signal.
* **Dielectric Insulator:** Separates the central conductor from the shield.
* **Metallic Shield:** Protects the signal from external electromagnetic interference.
* **Outer Jacket:** Provides mechanical protection.

### 🖆 How Coaxial Cable Works

Electrical signals travel through the central conductor, with the surrounding shield providing the return path and electromagnetic shielding.

The cable's geometry helps maintain controlled electrical characteristics.

### 🖆 Types of Coaxial Cable

**RG-6**

Commonly used in:

* Cable television.
* Cable broadband.
* Satellite television installations.

**RG-59**

Commonly used in:

* Some video installations.
* CCTV systems.
* Short-distance signal distribution.

**RG-11**

Used in applications requiring lower signal attenuation over longer coaxial cable runs.

### 🖆 Common Applications

* Cable internet.
* Cable television.
* Satellite communication systems.
* CCTV installations.
* Radio-frequency signal distribution.

### 🖆 Advantages

* Good shielding against external interference.
* Suitable for radio-frequency transmission.
* Supports relatively long transmission distances depending on the application.
* Widely used in cable broadband infrastructure.

### 🖆 Disadvantages

* Bulkier than twisted-pair Ethernet cable.
* More difficult to install and terminate in some environments.
* Signal attenuation increases with cable length and frequency.
* Not commonly used for modern office Ethernet LAN connections.

# 🗁 Fiber Optic Cable

### 🖆 What Is Fiber Optic Cable?

Fiber-optic cable is a transmission medium that carries data using pulses of light rather than electrical signals.

It consists of thin optical fibers made from glass or plastic.

Fiber-optic technology is widely used in high-speed networking, ISP infrastructure, telecommunications, and data centers.

<AsyncImage query="fiber optic cable cutaway showing optical core cladding buffer strength members outer jacket" aspectRatio="16:9" width="100%" />

### 🖆 Structure of Fiber Optic Cable

A typical fiber-optic cable contains:

* **Core:** The central region through which light travels.
* **Cladding:** Surrounds the core and helps confine light within it.
* **Coating:** Protects the glass fiber from physical damage.
* **Buffer:** Provides additional mechanical protection.
* **Strength Members:** Protect the cable against pulling forces.
* **Outer Jacket:** Protects the cable from environmental conditions.

### 🖆 How Fiber Optic Communication Works

Fiber-optic communication uses light signals to transmit information.

The general process is:

* A transmitter converts electrical data into optical signals.
* A light source, such as a laser or LED, generates light pulses.
* The light travels through the optical fiber.
* A receiver detects the incoming light.
* The receiver converts the optical signal back into electrical data.

The fiber guides light through the core using total internal reflection.

### 🖆 Types of Fiber Optic Cable

Fiber-optic cable is commonly classified into two main categories:

* Single-Mode Fiber (SMF).
* Multi-Mode Fiber (MMF).

### 🖆 Single-Mode Fiber

Single-mode fiber has a very small core, typically around 8–10 micrometers.

It supports one primary propagation mode.

Characteristics:

* Suitable for long-distance communication.
* Supports high bandwidth over long distances.
* Commonly uses laser-based optical transmitters.
* Widely used in ISP and telecommunications infrastructure.

<AsyncImage query="single mode fiber optic cable yellow jacket close up" aspectRatio="16:9" width="100%" />

Common applications:

* ISP backbone networks.
* FTTH deployments.
* Metropolitan networks.
* Long-distance telecommunications.
* Data center interconnections.

### 🖆 Multi-Mode Fiber

Multi-mode fiber has a larger core, commonly 50 or 62.5 micrometers.

It supports multiple light propagation modes.

Characteristics:

* Commonly used for shorter-distance connections.
* Often used in enterprise and data center environments.
* Typically uses VCSEL-based optical transceivers for many Ethernet applications.
* Modal dispersion limits its effective distance and bandwidth.

<AsyncImage query="multimode fiber optic cable aqua OM3 jacket close up" aspectRatio="16:9" width="100%" />

Common applications:

* Data center connections.
* Server-to-switch links.
* Campus networks.
* Short-distance building connections.

### 🖆 Single-Mode vs Multi-Mode Fiber

| Feature | Single-Mode Fiber | Multi-Mode Fiber |
| --- | --- | --- |
| Core Diameter | Approximately 8–10 µm | Commonly 50 or 62.5 µm |
| Light Propagation | One primary mode | Multiple modes |
| Typical Distance | Longer distances | Shorter distances |
| Dispersion | Low modal dispersion | Modal dispersion is a factor |
| Transmitter | Commonly laser-based | Commonly VCSEL for Ethernet applications |
| Applications | ISP and telecom networks | Data centers and campus networks |

### 🖆 Fiber Categories

Common multimode fiber classifications include:

| Category | Core Diameter | Common Application |
| --- | ---: | --- |
| OM1 | 62.5 µm | Legacy multimode installations |
| OM2 | 50 µm | Legacy and shorter-distance links |
| OM3 | 50 µm | 10G and higher-speed short-reach links |
| OM4 | 50 µm | Higher-performance multimode links |
| OM5 | 50 µm | Wideband multimode applications |

Single-mode fiber commonly uses these classifications:

| Category | Description |
| --- | --- |
| OS1 | Single-mode fiber designed primarily for indoor structured cabling |
| OS2 | Low-attenuation single-mode fiber commonly used for longer-distance outdoor and campus links |

Exact performance depends on the applicable fiber specifications, optical transceivers, and link design.

### 🖆 Fiber Optic Connectors

Common fiber-optic connector types include:

**LC - Lucent Connector**

A small-form-factor connector widely used in switches, routers, and transceivers.

**SC - Subscriber Connector**

A push-pull connector commonly used in telecommunications and fiber distribution systems.

**ST - Straight Tip**

A bayonet-style connector used in some legacy installations.

**FC - Ferrule Connector**

A threaded connector used in certain optical communication and measurement systems.

<AsyncImage query="LC SC ST FC fiber optic connector types comparison" aspectRatio="16:9" width="100%" />

### 🖆 Fiber Transceivers

Optical transceivers convert electrical signals into optical signals and optical signals back into electrical signals.

Common transceiver form factors include:

* SFP.
* SFP+.
* SFP28.
* QSFP+.
* QSFP28.
* QSFP-DD.

Common optical Ethernet transceiver types include:

| Transceiver | Typical Ethernet Speed | Common Application |
| --- | ---: | --- |
| 1000BASE-SX | 1 Gbps | Multimode fiber |
| 1000BASE-LX | 1 Gbps | Single-mode fiber and specified multimode applications |
| 10GBASE-SR | 10 Gbps | Multimode fiber |
| 10GBASE-LR | 10 Gbps | Single-mode fiber |
| 25GBASE-SR | 25 Gbps | Multimode fiber |
| 25GBASE-LR | 25 Gbps | Single-mode fiber |
| 100GBASE-SR4 | 100 Gbps | Multimode parallel optics |
| 100GBASE-LR4 | 100 Gbps | Single-mode fiber |

Always verify the transceiver's wavelength, connector, fiber type, optical budget, and supported distance.

### 🖆 Advantages of Fiber Optic Cable

* Very high bandwidth potential.
* Supports long-distance communication.
* Immune to electromagnetic interference.
* Provides electrical isolation between connected systems.
* Low signal attenuation compared with many copper alternatives.
* Difficult to tap without specialized equipment.

### 🖆 Disadvantages of Fiber Optic Cable

* Optical equipment can be expensive.
* Requires specialized termination and testing equipment.
* Fiber can be damaged by excessive bending or pulling.
* Troubleshooting may require optical measurement tools.
* Some installations require trained technicians.

# 🗁 Wireless Media

### 🖆 What Is Wireless Media?

Wireless media transmits information through electromagnetic waves instead of using a physical communication cable.

Wireless communication is widely used in mobile networks, Wi-Fi, satellite systems, and point-to-point radio links.

<AsyncImage query="wireless networking radio waves WiFi access point communication illustration" aspectRatio="16:9" width="100%" />

### 🖆 Radio Waves

Radio waves are electromagnetic signals used for wireless communication.

They support technologies such as:

* Wi-Fi.
* Bluetooth.
* Cellular networks.
* Radio communication.
* Wireless broadband.

Radio waves can travel through many common building materials, although walls, obstacles, interference, and frequency affect signal strength and coverage.

### 🖆 Wi-Fi

Wi-Fi is a wireless networking technology based on IEEE 802.11 standards.

It allows devices to connect to local networks without requiring Ethernet cables.

Common Wi-Fi frequency bands include:

* 2.4 GHz.
* 5 GHz.
* 6 GHz.

Each band has different characteristics involving coverage, available channels, interference, and supported data rates.

### 🖆 Microwave Communication

Microwave communication uses high-frequency electromagnetic waves.

It is commonly used for point-to-point wireless links and telecommunications backhaul.

Characteristics:

* Often uses directional antennas.
* Commonly requires line of sight.
* Supports high-capacity links.
* Can provide connectivity where installing fiber is difficult.

### 🖆 Infrared Communication

Infrared communication uses infrared electromagnetic radiation.

Characteristics:

* Commonly used over short distances.
* Often requires direct or reflected optical paths.
* Does not generally pass through walls.
* Used in remote controls and specialized communication systems.

### 🖆 Satellite Communication

Satellite communication uses satellites to relay signals between geographically separated locations.

Common applications:

* Remote internet connectivity.
* Maritime communication.
* Aviation communication.
* Broadcasting.
* Global telecommunications.

Satellite systems may use geostationary, medium Earth orbit, or low Earth orbit satellites.

### 🖆 Cellular Networks

Cellular networks provide wireless connectivity through geographically distributed cell sites.

Common generations include:

* 2G.
* 3G.
* 4G LTE.
* 5G.

Cellular technologies support mobile voice and data communication.

# 🗁 Transmission Characteristics

### 🖆 Bandwidth

Bandwidth describes the frequency range or capacity of a communication channel.

For a digital network, bandwidth is commonly discussed in terms of the maximum data transmission rate, measured in bits per second.

Examples:

* 100 Mbps.
* 1 Gbps.
* 10 Gbps.
* 100 Gbps.

In strict technical terminology, frequency bandwidth is measured in hertz, while digital transmission capacity is measured in bits per second.

### 🖆 Throughput

Throughput is the actual amount of data successfully transferred over a network during a given period.

For example, a 1 Gbps Ethernet link may deliver less than 1 Gbps of application-level throughput because of protocol overhead, traffic conditions, and other limitations.

### 🖆 Latency

Latency is the time required for data to travel from one point to another.

It is commonly measured in milliseconds.

Factors affecting latency include:

* Physical distance.
* Propagation speed.
* Transmission delay.
* Queueing delay.
* Processing delay.
* Network congestion.

### 🖆 Jitter

Jitter refers to variation in packet delay.

It is particularly important for real-time applications such as:

* Voice over IP.
* Video conferencing.
* Online gaming.
* Live streaming.

### 🖆 Attenuation

Attenuation is the reduction in signal strength as a signal travels through a transmission medium.

It is affected by:

* Cable length.
* Signal frequency.
* Cable quality.
* Connectors.
* Environmental conditions.

### 🖆 Crosstalk

Crosstalk occurs when signals in one communication channel interfere with signals in another channel.

It can occur between adjacent copper wire pairs.

Cable twisting and shielding help reduce crosstalk.

### 🖆 Electromagnetic Interference (EMI)

EMI is unwanted electromagnetic energy that affects signal transmission.

Common sources include:

* Electric motors.
* Power cables.
* Transformers.
* Industrial machinery.
* Radio transmitters.

Fiber-optic cables are immune to electromagnetic interference because they transmit light rather than electrical signals.

### 🖆 Signal-to-Noise Ratio (SNR)

SNR compares the strength of a desired signal with the level of background noise.

A higher SNR generally indicates better signal quality.

It is commonly expressed in decibels (dB).

### 🖆 Propagation Delay

Propagation delay is the time required for a signal to travel through a communication medium from one point to another.

It depends primarily on:

* Physical distance.
* Signal propagation speed in the medium.

Propagation delay is different from transmission delay, which depends on the amount of data and the transmission rate.

# 🗁 Cable Categories and Standards

### 🖆 Ethernet Cable Standards

Ethernet physical-layer standards define how signals are transmitted over supported media.

Examples include:

| Ethernet Standard | Speed | Medium |
| --- | ---: | --- |
| 10BASE-T | 10 Mbps | Twisted pair |
| 100BASE-TX | 100 Mbps | Twisted pair |
| 1000BASE-T | 1 Gbps | Twisted pair |
| 2.5GBASE-T | 2.5 Gbps | Twisted pair |
| 5GBASE-T | 5 Gbps | Twisted pair |
| 10GBASE-T | 10 Gbps | Twisted pair |
| 10GBASE-SR | 10 Gbps | Multimode fiber |
| 10GBASE-LR | 10 Gbps | Single-mode fiber |
| 100GBASE-SR4 | 100 Gbps | Multimode fiber |
| 100GBASE-LR4 | 100 Gbps | Single-mode fiber |

The supported maximum distance depends on the Ethernet standard and transmission medium.

### 🖆 Common Fiber Wavelengths

Common optical wavelengths used in Ethernet and telecommunications include:

| Wavelength | Common Applications |
| --- | --- |
| 850 nm | Multimode optical links |
| 1310 nm | Single-mode Ethernet and telecommunications |
| 1490 nm | GPON downstream transmission |
| 1550 nm | Telecommunications and optical distribution |

Wavelength assignments depend on the specific optical standard and equipment.

### 🖆 Structured Cabling

Structured cabling is a standardized approach to designing and installing telecommunications cabling infrastructure within buildings and campuses.

A structured cabling system commonly includes:

* Entrance facilities.
* Equipment rooms.
* Telecommunications rooms.
* Backbone cabling.
* Horizontal cabling.
* Work-area outlets.
* Patch panels.

It helps make network installations organized, scalable, and easier to maintain.

# 🗁 Copper vs Fiber Optic

### 🖆 Comparison

| Feature | Copper | Fiber Optic |
| --- | --- | --- |
| Signal Type | Electrical | Optical |
| Transmission Medium | Copper conductors | Glass or plastic fiber |
| EMI Resistance | Depends on shielding and installation | Immune to EMI |
| Typical LAN Distance | Common Ethernet channels up to 100 m | Depends on fiber and transceiver |
| Bandwidth Potential | High, depending on category and standard | Very high |
| Electrical Isolation | No | Yes |
| Installation | Generally simpler | Requires specialized handling and termination |
| Cost | Often lower for short links | Varies by equipment and deployment |
| Common Usage | Desktop and access connections | Backbone, ISP, and data center links |

### 🖆 When to Use Copper

Copper Ethernet is appropriate when:

* Devices are relatively close together.
* Standard Ethernet connectivity is required.
* PoE is needed.
* Existing structured copper cabling is available.
* Installation cost and simplicity are important.

### 🖆 When to Use Fiber

Fiber-optic cabling is appropriate when:

* Longer transmission distances are required.
* High bandwidth is needed.
* Electromagnetic interference is a concern.
* Electrical isolation is required.
* Connecting buildings or network distribution points.
* High-capacity backbone connectivity is needed.

# 🗁 Choosing the Right Network Media

### 🖆 Small Office Network

A small office may use:

* Cat 6 or Cat 6A Ethernet cable for endpoint connections.
* Fiber-optic uplinks between network switches when required.
* Wi-Fi for laptops and mobile devices.

### 🖆 Enterprise Network

An enterprise may use:

* Cat 6A for high-performance horizontal cabling.
* Single-mode fiber for building-to-building connections.
* Multimode fiber for supported short-distance data center links.
* Wi-Fi for mobile endpoints.

### 🖆 ISP Network

An ISP may use:

* Fiber-optic backbone connections.
* Single-mode fiber for long-distance transport.
* Coaxial cable in cable broadband networks.
* Twisted pair for certain legacy access services.
* Microwave links for wireless backhaul.
* GPON or XGS-PON for fiber access networks.

### 🖆 Wireless Network

A wireless network may use:

* Wi-Fi for local wireless connectivity.
* Point-to-point microwave for backhaul.
* Cellular networks for mobile connectivity.
* Satellite links for remote connectivity.

# 🗁 Common Network Media Problems

### 🖆 Damaged Ethernet Cable

Symptoms:

* Link does not come up.
* Connection repeatedly disconnects.
* Negotiated speed is lower than expected.
* Packet errors increase.

Possible causes:

* Broken conductor.
* Damaged connector.
* Poor termination.
* Excessive cable length.

### 🖆 Incorrect Cable Termination

Incorrect T568A or T568B wiring can cause:

* Open pairs.
* Short circuits.
* Split pairs.
* Failed Ethernet links.
* Reduced performance.

Use a suitable cable tester to identify wiring faults.

### 🖆 Excessive Cable Length

Ethernet links that exceed the supported cable distance may experience:

* Link instability.
* Increased errors.
* Failure to establish connectivity.

Use appropriate intermediate switches or fiber links when longer distances are required.

### 🖆 Fiber Connector Contamination

Dust and contamination on fiber connectors can increase optical loss.

Proper inspection and cleaning procedures are important before connecting optical interfaces.

### 🖆 Excessive Fiber Bending

Fiber cables can suffer increased attenuation or physical damage when bent beyond their specified bend radius.

Follow the cable manufacturer's bend-radius requirements.

### 🖆 Wireless Interference

Wireless networks may experience interference from:

* Nearby Wi-Fi networks.
* Bluetooth devices.
* Microwave ovens.
* Other radio-frequency sources.

Channel planning, appropriate access point placement, and suitable frequency selection can improve performance.

### 🖆 Weak Wireless Signal

Possible causes include:

* Long distance from the access point.
* Thick walls.
* Metal obstacles.
* Antenna placement.
* Transmit-power limitations.

Improve access point placement or deploy additional access points where necessary.

# 🗁 Practical Exercises

### 🖆 Exercise: Identify Cable Types

Identify the following network media:

* Cat 6 UTP cable.
* Cat 6A F/UTP cable.
* RG-6 coaxial cable.
* Single-mode OS2 fiber.
* OM4 multimode fiber.

For each one, identify its construction, common application, and transmission characteristics.

### 🖆 Exercise: Create an Ethernet Cable

Required equipment:

* Cat 6 UTP cable.
* RJ45-compatible 8P8C connectors.
* Crimping tool.
* Cable stripper.
* Ethernet cable tester.

Tasks:

* Strip the cable jacket.
* Arrange conductors using T568B.
* Insert conductors into the connector.
* Crimp the connector.
* Repeat at the opposite end.
* Test all eight conductors.

### 🖆 Exercise: Inspect Network Interfaces

In Ubuntu, identify network interfaces:

```bash
ip link
```

Check interface statistics:

```bash
ip -s link
```

Display negotiated link information when supported:

```bash
sudo ethtool eth0
```

Replace `eth0` with the actual interface name.

Install `ethtool` if it is not already available:

```bash
sudo apt update
sudo apt install ethtool
```

### 🖆 Exercise: Inspect Fiber Optic Equipment

Identify:

* Single-mode fiber cable.
* Multimode fiber cable.
* LC connector.
* SC connector.
* SFP transceiver.
* Optical patch panel.

Record the wavelength and supported distance of each available transceiver.

### 🖆 Exercise: Compare Wireless Signal Quality

Use the following Ubuntu command to inspect wireless interfaces:

```bash
iw dev
```

Display nearby wireless networks:

```bash
nmcli device wifi list
```

Observe:

* Signal strength.
* Frequency.
* Channel.
* Security type.

Compare wireless performance at different locations.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is network media?**

Network media is the physical or wireless transmission channel used to carry data between network devices.

**What are the two major types of network media?**

Guided media and unguided media.

**What is twisted pair cable?**

A copper cable containing insulated wire pairs twisted together to reduce interference and crosstalk.

**What is coaxial cable?**

A copper transmission medium with a central conductor, dielectric insulation, and surrounding metallic shielding.

**What is fiber-optic cable?**

A transmission medium that carries information using pulses of light through optical fibers.

### 🖆 Intermediate Questions

**What is the difference between UTP and STP?**

UTP does not use additional metallic shielding, while STP uses shielding to reduce electromagnetic interference.

**What is the difference between single-mode and multimode fiber?**

Single-mode fiber supports one primary propagation mode and is suitable for longer distances. Multimode fiber supports multiple modes and is commonly used for shorter links.

**What is attenuation?**

Attenuation is the reduction in signal strength as a signal travels through a transmission medium.

**Why are Ethernet wire pairs twisted?**

Twisting helps reduce electromagnetic interference and crosstalk.

**What is the maximum typical Ethernet channel length over twisted pair?**

For common structured cabling installations, the channel length is generally limited to 100 meters.

**What is the difference between T568A and T568B?**

They define different conductor pin assignments for Ethernet cabling.

### 🖆 Advanced Questions

**Why is fiber-optic cable immune to electromagnetic interference?**

It transmits information using light rather than electrical signals, so external electromagnetic fields do not induce interference in the optical transmission medium.

**What is the difference between bandwidth and throughput?**

Bandwidth describes the available capacity or frequency range of a communication channel, while throughput measures the actual successful data transfer rate.

**What is the difference between propagation delay and transmission delay?**

Propagation delay is the time required for a signal to travel through a medium. Transmission delay is the time required to place all bits of a packet onto the communication link.

**Why is single-mode fiber preferred for long-distance communication?**

Its small core and low modal dispersion make it suitable for long-distance, high-capacity optical communication.

**What is SNR?**

Signal-to-Noise Ratio measures the strength of a desired signal relative to background noise.

**Why should fiber connectors be cleaned before installation?**

Contamination can increase optical insertion loss and negatively affect link performance.

# 🗁 References

* [IEEE 802 Standards](https://standards.ieee.org/)
* [TIA Standards](https://tiaonline.org/)
* [ISO/IEC Standards](https://www.iso.org/)
* [ITU-T Recommendations](https://www.itu.int/ITU-T/recommendations/)
* [Fiber Optic Association](https://www.thefoa.org/)
* [Cisco Ethernet Technology](https://www.cisco.com/)
* [Wi-Fi Alliance](https://www.wi-fi.org/)
* [Linux ethtool Manual](https://man7.org/linux/man-pages/man8/ethtool.8.html)
