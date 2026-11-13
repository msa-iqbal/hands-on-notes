# RouterBOARD and RouterOS

MikroTik networking devices combine hardware and software to provide routing, switching, wireless connectivity, and network management capabilities.

Two fundamental components of this ecosystem are **RouterBOARD** and **RouterOS**.

* **RouterBOARD** provides the physical hardware.
* **RouterOS** provides the operating system and networking functionality.

Understanding how these components work together helps administrators select suitable MikroTik devices, configure networks, troubleshoot hardware-related problems, and maintain RouterOS installations.

# 🗁 What Is RouterBOARD?

RouterBOARD is MikroTik's hardware platform for networking equipment.

It includes the physical components required to operate a network device, such as:

* CPU and processing units.
* RAM and storage.
* Ethernet interfaces.
* Wireless radios.
* Switching chips.
* Power connectors.
* Expansion interfaces.
* Cooling components.
* Console interfaces on supported models.

RouterBOARD devices are designed for different networking requirements, ranging from basic home connectivity to enterprise networking and ISP infrastructure.

# 🗁 RouterBOARD Product Categories

MikroTik offers several categories of networking hardware.

| Category | Purpose | Typical Deployment |
| --- | --- | --- |
| Routers | Routing, firewall, NAT, and network services | Home, office, ISP |
| Switches | Ethernet switching and VLAN segmentation | LAN and distribution networks |
| Wireless devices | Wireless connectivity and bridging | WISP and outdoor links |
| Access points | Wireless access for client devices | Homes and offices |
| Cloud Core Routers | High-performance routing | Enterprise and ISP |
| Cloud Router Switches | Switching with RouterOS capabilities | Distribution and aggregation |
| LTE and 5G devices | Cellular WAN connectivity | Remote sites and backup links |

The available features and performance depend on the specific hardware model.

# 🗁 RouterBOARD Hardware Components

Understanding the main hardware components helps when selecting and troubleshooting MikroTik devices.

**CPU**

The CPU processes RouterOS operations and network traffic.

Its performance affects tasks such as:

* Routing.
* Firewall processing.
* NAT and connection tracking.
* VPN encryption.
* Queues and traffic management.
* Routing protocol calculations.

A device with a more powerful CPU can generally handle more demanding workloads, although actual performance also depends on configuration and packet characteristics.

**RAM**

RAM stores temporary data required by RouterOS and its running services.

It is used for:

* Operating system processes.
* Routing tables.
* Firewall connection tracking.
* Packet buffers.
* Wireless management.
* Other active services.

Insufficient memory can affect device stability and limit the number of services or network features that can operate efficiently.

**Storage**

Storage holds RouterOS files, configuration data, packages, and other system information.

The storage technology and capacity vary between devices.

**Ethernet Interfaces**

Ethernet interfaces provide wired network connectivity.

Common interface speeds include:

* 10 Mbps.
* 100 Mbps.
* 1 Gbps.
* 2.5 Gbps.
* 10 Gbps.
* Higher speeds on supported hardware.

The number and speed of available interfaces are important when designing LAN, WAN, and backbone connectivity.

**Switching Chip**

Some RouterBOARD devices include dedicated switching hardware.

A switching chip can handle certain Layer 2 operations in hardware, reducing CPU involvement and improving forwarding performance.

Hardware offloading capabilities depend on the model, switch chip, RouterOS version, and configuration.

**Wireless Radio**

Wireless-capable RouterBOARD devices include radio hardware for Wi-Fi or other supported wireless technologies.

Wireless performance depends on several factors:

* Supported wireless standards.
* Frequency bands.
* Channel width.
* Antenna design.
* Transmit power.
* Interference.
* Client capabilities.

**Power Supply**

RouterBOARD devices use different power input methods, including:

* DC power adapters.
* Passive PoE.
* Standardized PoE input.
* PoE output on supported models.

Always verify the device's voltage, polarity, power requirements, and PoE compatibility before connecting a power source.

# 🗁 What Is RouterOS?

RouterOS is MikroTik's network operating system.

It provides the software environment that controls networking hardware and implements network services.

RouterOS allows administrators to configure a device as a:

* Router.
* Firewall.
* NAT gateway.
* Ethernet bridge.
* VLAN-aware switch.
* Wireless access point.
* VPN endpoint.
* PPPoE server.
* Hotspot gateway.
* Bandwidth management system.
* Dynamic routing platform.

Unlike a basic home router interface that exposes only a limited set of settings, RouterOS provides detailed control over networking operations.

# 🗁 How RouterBOARD and RouterOS Work Together

RouterBOARD and RouterOS perform different but complementary responsibilities.

RouterBOARD provides the hardware resources, while RouterOS manages those resources and implements networking functionality.

For example, consider a MikroTik router connected to an ISP and an internal LAN.

The hardware provides:

* CPU for processing.
* RAM for temporary system data.
* Ethernet interfaces for WAN and LAN connectivity.
* Storage for system files and configuration.

RouterOS provides:

* WAN IP configuration.
* LAN IP configuration.
* Routing between networks.
* Firewall filtering.
* NAT for internet access.
* DHCP services for LAN clients.

The result is a complete networking device.

# 🗁 RouterOS Boot Process

Understanding the boot process helps administrators troubleshoot startup and recovery problems.

A simplified RouterBOARD startup sequence is:

```text
Power On
   |
   v
RouterBOOT Initialization
   |
   v
Hardware Initialization
   |
   v
RouterOS Boot
   |
   v
System Services Start
   |
   v
Network Interfaces Become Available
   |
   v
Normal Operation
```

**RouterBOOT** is the bootloader used by supported RouterBOARD hardware.

It initializes the device and helps load the operating system.

The exact startup behavior depends on the hardware model, firmware, boot configuration, and RouterOS platform.

# 🗁 RouterBOOT and RouterBOARD Firmware

RouterBOOT is separate from RouterOS.

It handles early hardware initialization and the process of starting the operating system.

RouterOS can display RouterBOARD firmware information on supported devices.

Inspect the firmware information:

```routeros
/system routerboard print
```

The output can include:

* RouterBOARD model.
* Hardware revision.
* Serial number.
* Current firmware.
* Available upgrade firmware.

Upgrade RouterBOARD firmware using the appropriate RouterOS command when an update is required:

```routeros
/system routerboard upgrade
```

After the upgrade, reboot the device when appropriate:

```routeros
/system reboot
```

Check the firmware information again after the device restarts:

```routeros
/system routerboard print
```

RouterBOOT firmware and RouterOS are different software components. Updating one does not mean the other has also been updated.

# 🗁 RouterOS Packages

RouterOS functionality is organized into software components and packages.

Depending on the platform and RouterOS release, packages provide features required by different networking services.

Examples of functionality include:

* Routing.
* Wireless.
* Security.
* PPP services.
* Advanced networking protocols.
* Monitoring and management.

Inspect installed packages:

```routeros
/system package print
```

The output shows package information, including installed versions and package states where applicable.

Package availability and organization can differ between RouterOS releases and platforms.

Before installing, removing, or updating packages, check the official documentation for the relevant RouterOS version.

# 🗁 RouterBOARD Models and Product Selection

MikroTik devices have different hardware capabilities and are designed for different workloads.

When selecting a device, consider the following specifications:

| Specification | Why It Matters |
| --- | --- |
| CPU architecture | Determines platform capabilities and processing characteristics |
| CPU performance | Affects routing, firewall, VPN, and other workloads |
| RAM | Determines available memory for RouterOS and services |
| Storage | Provides space for system files and configuration |
| Ethernet ports | Determines physical connectivity options |
| Port speed | Determines interface bandwidth capacity |
| Switching chip | Influences Layer 2 hardware forwarding capabilities |
| Wireless support | Determines available wireless functionality |
| PoE support | Determines power input or output options |
| Expansion options | Provides additional connectivity where supported |

Do not select a router based only on the number of Ethernet ports or its advertised interface speed.

Real-world performance depends on packet size, enabled features, traffic patterns, hardware acceleration, and configuration complexity.

# 🗁 RouterBOARD and CHR Differences

RouterBOARD and Cloud Hosted Router are two different ways to run RouterOS.

| Feature | RouterBOARD | CHR |
| --- | --- | --- |
| Platform | Physical MikroTik hardware | Virtual machine |
| CPU | Physical processor | Virtual CPU |
| Memory | Physical RAM | Allocated virtual RAM |
| Interfaces | Physical Ethernet and other hardware interfaces | Virtual network interfaces |
| Storage | Device storage | Virtual disk |
| Deployment | Physical networking environment | Virtualized environment |
| Typical use | Production routers, switches, wireless devices | Labs, virtual networks, and supported production deployments |

CHR is useful when a physical router is unnecessary or when a virtualized network environment is preferred.

# 🗁 Hardware Offloading

Hardware offloading allows supported networking operations to be processed by dedicated hardware instead of relying entirely on the CPU.

For example, a supported switching chip may forward Layer 2 traffic directly in hardware.

Benefits can include:

* Higher forwarding performance.
* Lower CPU utilization.
* Improved scalability.
* More efficient packet processing.

However, not every RouterOS feature can be offloaded.

Some configurations or unsupported combinations may cause traffic to be processed by the CPU instead.

When troubleshooting switching performance, inspect the device documentation and verify whether the configured bridge and interfaces support hardware offloading.

# 🗁 Inspecting RouterBOARD and RouterOS Information

The following commands help identify the hardware and software environment.

Display the RouterBOARD model and firmware information:

```routeros
/system routerboard print
```

Display system resource information:

```routeros
/system resource print
```

Display RouterOS version and architecture information:

```routeros
/system resource print
```

Display installed packages:

```routeros
/system package print
```

Display device identity:

```routeros
/system identity print
```

Display available interfaces:

```routeros
/interface print
```

Display storage information:

```routeros
/disk print
```

The `/disk` menu is available only on platforms and RouterOS configurations that expose disk information through that menu.

# 🗁 Practical Example: Identifying a MikroTik Device

When receiving an unfamiliar MikroTik device, collect its basic hardware and software information before making configuration changes.

Run:

```routeros
/system identity print
/system resource print
/system routerboard print
/system package print
/interface print
/ip address print
```

Review the output to identify:

* Device identity.
* RouterOS version.
* CPU architecture.
* Processor and memory resources.
* RouterBOARD model.
* Firmware information.
* Installed packages.
* Available network interfaces.
* Existing IP configuration.

This information provides a useful baseline for documentation, troubleshooting, and hardware planning.

# 🗁 Maintenance Considerations

RouterBOARD and RouterOS should be maintained together as part of the device lifecycle.

Recommended maintenance practices include:

* Use RouterOS releases appropriate for the device and deployment.
* Review release notes before upgrading.
* Verify available storage and memory.
* Maintain configuration backups.
* Keep RouterBOARD firmware updated when appropriate.
* Monitor CPU and memory utilization.
* Inspect hardware temperature and voltage where supported.
* Verify power supply compatibility.
* Document device models and firmware versions.
* Test upgrades in a lab before applying them to critical production equipment.

# 🗁 Summary

RouterBOARD provides MikroTik's physical networking hardware, while RouterOS provides the operating system and networking features.

RouterBOOT initializes supported RouterBOARD hardware and helps start RouterOS. RouterOS then manages the device's interfaces, routing, firewall, wireless services, and other networking functions.

Understanding the relationship between hardware, firmware, and operating system software is essential for selecting devices, maintaining installations, and troubleshooting MikroTik networks.

The next topic covers RouterOS architecture, its configuration structure, and how administrators interact with its menus and configuration system.
