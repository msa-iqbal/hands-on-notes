# MikroTik Overview

MikroTik is a technology company that develops networking hardware and software for building, managing, and maintaining computer networks. Its products are widely used in home networks, small and medium-sized businesses, wireless internet service providers (WISPs), and internet service providers (ISPs).

MikroTik is particularly known for **RouterOS**, its network operating system, and **RouterBOARD**, its networking hardware platform.

This document introduces the MikroTik ecosystem, RouterOS capabilities, common deployment scenarios, management methods, and essential terminology.

# 🗁 What Is MikroTik?

MikroTik is a networking equipment and software manufacturer headquartered in Latvia.

Its product ecosystem includes:

* Routers for home, business, and service-provider networks.
* Switches for Ethernet connectivity and network segmentation.
* Wireless access points and point-to-point wireless equipment.
* RouterOS, a feature-rich network operating system.
* Cloud Hosted Router (CHR), a virtualized RouterOS platform.
* Network management and monitoring capabilities.

MikroTik products are popular because they provide a broad range of networking features, flexible configuration options, and solutions for different network sizes and budgets.

# 🗁 What Is RouterOS?

RouterOS is MikroTik's proprietary network operating system. It provides the software features required to configure and operate MikroTik networking devices.

RouterOS can perform multiple networking roles, including:

* Routing between different IP networks.
* Switching and Ethernet bridging.
* VLAN configuration and network segmentation.
* Firewall filtering and connection tracking.
* Network Address Translation (NAT).
* DHCP server and client services.
* DNS caching and forwarding.
* VPN and tunneling services.
* Wireless network management.
* Quality of Service (QoS) and bandwidth management.
* PPPoE server and client services.
* Hotspot and captive portal services.
* RADIUS authentication and accounting.
* Network monitoring and troubleshooting.
* Automation through scripting and APIs.

A single RouterOS device can perform several of these roles simultaneously, depending on its hardware capabilities and configuration.

# 🗁 MikroTik Hardware and Software

MikroTik's ecosystem consists of hardware platforms and the software that operates on them.

| Component | Description |
| --- | --- |
| RouterBOARD | MikroTik's hardware platform for networking devices |
| RouterOS | Operating system that provides networking functionality |
| RouterBOOT | Bootloader responsible for initializing supported RouterBOARD hardware |
| WinBox | Graphical application for managing RouterOS |
| WebFig | Browser-based RouterOS management interface |
| CHR | Virtualized RouterOS platform |
| RouterOS CLI | Command-line interface for configuring and managing devices |

RouterOS is available on compatible MikroTik hardware and supported software-based platforms.

# 🗁 Common MikroTik Deployment Scenarios

MikroTik devices are used in different networking environments.

**Home Networks**

* Internet connectivity and gateway routing.
* DHCP and DNS services.
* NAT and firewall configuration.
* Wireless network management.
* Basic bandwidth management.

**Small and Medium-Sized Businesses**

* Internet gateway configuration.
* VLAN-based network segmentation.
* Inter-VLAN routing.
* Site-to-site and remote-access VPNs.
* Firewall policies and traffic management.
* Multiple WAN connections and failover.

**Enterprise Networks**

* Advanced routing and network segmentation.
* Dynamic routing protocols.
* Centralized network connectivity.
* VPN connectivity between locations.
* Traffic prioritization and monitoring.

**Wireless Internet Service Providers (WISPs)**

* Wireless point-to-point and point-to-multipoint connectivity.
* Subscriber management.
* PPPoE and Hotspot services.
* Bandwidth control.
* Routing and failover.

**Internet Service Providers (ISPs)**

* Broadband subscriber authentication.
* PPPoE server deployment.
* RADIUS integration.
* Customer bandwidth management.
* BGP routing and upstream connectivity.
* CGNAT and public IP management.
* Network monitoring and troubleshooting.

# 🗁 RouterOS Platforms

RouterOS can operate on different hardware and virtualization platforms.

**RouterBOARD**

RouterBOARD refers to MikroTik's hardware platform. MikroTik routers and other supported devices generally ship with RouterOS installed.

The hardware determines available interfaces, processing capacity, memory, wireless capabilities, and overall performance.

**Cloud Hosted Router (CHR)**

CHR is a RouterOS version designed to run as a virtual machine.

It can be deployed on supported virtualization platforms such as:

* VMware.
* VirtualBox.
* Hyper-V.
* KVM.
* Other compatible hypervisors.

CHR is useful for network labs, virtual routers, routing experiments, and certain production deployments.

**x86 Installation**

RouterOS has also been distributed for compatible x86-based hardware. Availability and support depend on the RouterOS release and licensing model.

For new deployments, consult MikroTik's official documentation for supported installation methods and platform requirements.

# 🗁 RouterOS Versions

RouterOS has two major version families commonly encountered in existing environments:

* RouterOS v6
* RouterOS v7

RouterOS v7 introduced significant changes and improvements, particularly in routing functionality and its underlying implementation.

New configurations and practical examples in this knowledge base will primarily target **RouterOS v7**.

Understanding both versions remains useful when maintaining older MikroTik devices or migrating existing networks.

# 🗁 RouterOS Licensing Overview

RouterOS licensing determines which features and capabilities are available on supported software platforms.

Licensing differs depending on the platform.

* RouterBOARD devices generally include a RouterOS license associated with the hardware.
* CHR uses a separate licensing model with different license levels.
* Older x86 installation methods have their own licensing considerations.

For CHR, licensing options include free and paid levels with different capabilities.

License availability, limitations, and pricing can change. Always consult the official MikroTik licensing documentation before selecting a platform for deployment.

# 🗁 MikroTik Management Interfaces

RouterOS provides several methods for device administration.

**WinBox**

WinBox is MikroTik's graphical management application.

It provides access to RouterOS configuration, monitoring, and troubleshooting features.

Common uses include:

* Managing interfaces and bridges.
* Configuring IP addresses.
* Creating firewall rules.
* Managing routes.
* Monitoring system resources.

**WebFig**

WebFig is the browser-based management interface built into RouterOS.

It allows administrators to manage supported RouterOS features through a web browser without installing WinBox.

**Command-Line Interface (CLI)**

The RouterOS CLI provides direct access to configuration and operational commands.

It is useful for:

* Writing and executing configuration commands.
* Inspecting system information.
* Troubleshooting network problems.
* Automating repetitive tasks.
* Managing devices remotely.

**SSH**

SSH provides encrypted remote CLI access to RouterOS.

It is commonly used for remote administration, scripting, and automation.

**API**

RouterOS provides an API that allows external applications and automation systems to communicate with a RouterOS device.

It can be used to retrieve information, execute commands, and integrate network management systems.

**Serial Console**

A serial console provides direct access to a device's console interface when supported by its hardware.

It is particularly useful for recovery, troubleshooting, and situations where normal network management access is unavailable.

# 🗁 Essential MikroTik Terminology

Understanding the following terms will make RouterOS documentation and configuration easier to follow.

| Term | Description |
| --- | --- |
| RouterBOARD | MikroTik hardware platform |
| RouterOS | MikroTik network operating system |
| RouterBOOT | Bootloader used by supported RouterBOARD devices |
| Package | A RouterOS software component that provides specific functionality |
| Interface | A physical or logical network connection |
| Bridge | A logical Layer 2 connection between interfaces |
| VLAN | A logical Layer 2 network segment |
| IP Address | An address assigned to a network interface |
| Routing Table | Information used to determine packet forwarding paths |
| Firewall Chain | A processing path used by RouterOS firewall rules |
| NAT | A mechanism for translating IP addresses and, optionally, ports |
| Safe Mode | A CLI feature that helps recover from potentially disruptive configuration changes |
| CHR | Cloud Hosted Router, the virtualized RouterOS platform |
| Router ID | An identifier used by certain routing protocols, such as OSPF and BGP |

# 🗁 First Access to a MikroTik Device

A MikroTik device can generally be accessed through WinBox, WebFig, SSH, or its available console interface.

For an initial connection:

1. Connect your computer to a suitable Ethernet interface.
2. Open WinBox or another supported management interface.
3. Discover the device or connect using its configured IP address.
4. Authenticate using the credentials provided for the device.
5. Inspect the current configuration before making changes.

The exact initial access procedure depends on the device model, RouterOS version, and whether the device has already been configured.

# 🗁 Basic RouterOS Inspection Commands

The following commands are useful when exploring a RouterOS v7 device.

Display system resource information:

```routeros
/system resource print
```

Display RouterBOARD hardware information:

```routeros
/system routerboard print
```

Display installed RouterOS packages:

```routeros
/system package print
```

Display the device identity:

```routeros
/system identity print
```

Display available interfaces:

```routeros
/interface print
```

Display configured IP addresses:

```routeros
/ip address print
```

Display system license information:

```routeros
/system license print
```

Display system clock information:

```routeros
/system clock print
```

Display RouterBOARD settings:

```routeros
/system routerboard settings print
```

These commands are primarily for inspection and do not intentionally change the device configuration.

# 🗁 Changing the Router Identity

The system identity is the name used to identify a RouterOS device.

For example, a router can be assigned the identity `R1-CORE`.

```routeros
/system identity set name=R1-CORE
```

Verify the configured identity:

```routeros
/system identity print
```

Using meaningful device names makes administration easier, especially when managing multiple routers.

Examples:

| Identity | Purpose |
| --- | --- |
| R1-CORE | Core router |
| R2-EDGE | Edge router |
| SW1-DIST | Distribution switch |
| AP1-OFFICE | Office access point |
| ISP-GATEWAY | ISP gateway router |
| LAB-CHR-01 | Virtual lab router |

# 🗁 Initial Configuration and Security Practices

Before deploying a MikroTik device in a production network, establish a secure management configuration.

Important practices include:

* Set a strong administrative password.
* Use named administrative accounts where appropriate.
* Restrict management access to trusted networks.
* Disable unused management services.
* Use SSH or other encrypted management methods.
* Configure firewall rules to protect the router itself.
* Keep RouterOS and RouterBOOT firmware maintained.
* Back up the configuration before significant changes.
* Monitor system resources and device logs.

Do not assume that every MikroTik device has the same default configuration. Inspect the existing configuration before applying changes.

# 🗁 Safe Mode and Configuration Recovery

RouterOS provides a Safe Mode feature in its CLI.

Safe Mode helps protect against configuration changes that accidentally interrupt the administrator's management connection.

In a RouterOS terminal, press:

```text
Ctrl + X
```

The terminal indicates when Safe Mode is active.

When changes are made in Safe Mode, RouterOS can automatically revert them if the management session terminates unexpectedly before the changes are confirmed.

Safe Mode is particularly useful when changing:

* Firewall rules.
* IP addresses.
* Routing configuration.
* VLAN settings.
* Remote management access.

For significant changes, also maintain configuration backups and ensure that a recovery method is available.

# 🗁 Official MikroTik Resources

The following resources provide official documentation, software, and community support.

| Resource                  | Link                              |
| ------------------------- | --------------------------------- |
| MikroTik Official Website | <https://mikrotik.com/>           |
| RouterOS Documentation    | <https://help.mikrotik.com/docs/> |
| RouterOS Downloads        | <https://mikrotik.com/download>   |
| MikroTik Support          | <https://mikrotik.com/support>    |
| MikroTik Community Forum  | <https://forum.mikrotik.com/>     |

Use the official RouterOS documentation as the primary reference when verifying command syntax, feature availability, and version-specific behavior.

# 🗁 Summary

MikroTik provides networking hardware and RouterOS software for building and managing networks of different sizes.

RouterOS combines routing, switching, firewall, NAT, VPN, wireless, bandwidth management, and ISP-related services in a single network operating system.

Understanding the MikroTik ecosystem, available platforms, management interfaces, and essential terminology provides the foundation for learning RouterOS configuration.

The next topic explores RouterBOARD hardware and RouterOS in more detail, including how the hardware and operating system work together.
