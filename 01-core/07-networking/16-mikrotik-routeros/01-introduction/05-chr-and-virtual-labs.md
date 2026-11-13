# CHR and Virtual Labs

**Introduction to MikroTik CHR**

MikroTik Cloud Hosted Router (CHR) is a RouterOS version designed to run as a virtual machine on a hypervisor or cloud computing platform.

Unlike physical MikroTik RouterBOARD devices, CHR runs on virtualized hardware. It provides RouterOS networking features without requiring a dedicated physical router.

CHR is useful for learning, testing, development, demonstrations, and network infrastructure deployment.

Common use cases include:

* Learning RouterOS configuration.
* Practicing routing protocols such as OSPF and BGP.
* Building virtual ISP network environments.
* Testing VLANs, bridges, and switching concepts.
* Practicing firewall, NAT, and VPN configurations.
* Simulating multiple routers and network topologies.
* Developing and testing RouterOS automation scripts.
* Building network labs before deploying configurations to production devices.

# 🗁 CHR vs Physical RouterBOARD

CHR and RouterBOARD devices run RouterOS, but they operate on different hardware platforms.

| Feature | CHR | RouterBOARD |
| --- | --- | --- |
| Platform | Virtual machine | Physical hardware |
| Operating system | RouterOS | RouterOS |
| Hardware | Virtual CPU, memory, and interfaces | Physical CPU, memory, and interfaces |
| Network interfaces | Virtual network adapters | Physical Ethernet, wireless, and other interfaces |
| Wireless hardware | Depends on virtualized capabilities | Available on supported models |
| Hardware acceleration | Depends on hypervisor and virtual hardware | Depends on device hardware |
| Main use | Virtual labs and virtualized routing | Physical network deployment |
| Licensing | CHR licensing | RouterBOARD licensing model |

CHR provides a practical environment for learning RouterOS, but it does not reproduce every hardware-specific feature of physical MikroTik devices.

For example, physical wireless radios, hardware switching capabilities, and device-specific interfaces may not be available in a CHR environment.

# 🗁 CHR Licensing

MikroTik CHR uses a separate licensing model from physical RouterBOARD devices.

CHR provides several license levels with different performance and feature limitations.

The available license levels include:

| License | Description |
| --- | --- |
| Free | Free license with a 1 Mbps upload limit per interface |
| P1 | Paid license with a 1 Gbps upload limit per interface |
| P10 | Paid license with a 10 Gbps upload limit per interface |
| P-Unlimited | Paid license without the CHR license upload-speed limitation |

The license limitation applies to upload traffic per interface, not to the total traffic of the entire virtual router.

The Free license is sufficient for many learning and configuration exercises.

For high-throughput virtual routing environments, a paid license may be appropriate.

Always check MikroTik's official licensing documentation for current pricing, terms, and limitations.

**Checking the CHR license**

Use the following command:

```routeros
/system license print
```

This displays information about the installed CHR license.

**CHR licensing documentation:**

<https://help.mikrotik.com/docs/spaces/ROS/pages/18350234/Cloud+Hosted+Router+CHR>

# 🗁 Virtualization Platforms

CHR can run on different virtualization platforms.

A virtualization platform, also called a hypervisor, allows multiple virtual machines to share the resources of a physical computer.

Popular platforms for building MikroTik labs include:

| Platform | Description | Typical use |
| --- | --- | --- |
| VirtualBox | Desktop virtualization software | Individual learning and small labs |
| VMware Workstation | Desktop virtualization software | RouterOS testing and network labs |
| Proxmox VE | Type-1 virtualization platform | Advanced labs and virtual infrastructure |
| GNS3 | Network simulation and emulation platform | Multi-vendor network topologies |
| EVE-NG | Network emulation platform | Complex networking and ISP labs |
| Hyper-V | Microsoft's virtualization platform | Windows-based virtual environments |
| Cloud platforms | Infrastructure hosted by cloud providers | Remote CHR deployment |

The best platform depends on available hardware, lab complexity, and the type of networking exercises you want to perform.

# 🗁 VirtualBox

VirtualBox is a desktop virtualization platform that allows you to run CHR as a virtual machine on a personal computer.

It is a good starting point for learning RouterOS because it is relatively straightforward to install and configure.

<text></text>

**Main components**

* Host machine: Your physical computer.
* Hypervisor: VirtualBox.
* Guest machine: MikroTik CHR virtual machine.
* Virtual network adapters: Connections between CHR and other virtual machines or physical networks.

**Typical lab topology**

```text
                  Host Computer
                       |
                   VirtualBox
                       |
                MikroTik CHR
                 /         \
                /           \
          ether1             ether2
             |                  |
        Linux Client       Windows Client
```

This topology allows you to practice:

* IP addressing.
* DHCP server configuration.
* Routing.
* NAT.
* Firewall rules.
* Network connectivity testing.

**Basic VirtualBox setup**

Download and install VirtualBox:

<https://www.virtualbox.org/wiki/Downloads>

Download the CHR image from MikroTik:

<https://mikrotik.com/download>

Select the appropriate CHR image format supported by your virtualization platform.

For VirtualBox, the VDI disk image is commonly used.

Create a virtual machine with settings similar to these:

| Setting | Suggested configuration |
| --- | --- |
| Name | MikroTik-CHR |
| Type | Linux |
| Version | Other Linux (64-bit) |
| Memory | 256 MB or more |
| CPU | 1 or more virtual CPUs |
| Disk | Use an existing CHR virtual disk |
| Network adapters | 2 or more |

The required CPU and memory resources depend on the number of interfaces, enabled services, routing protocols, and traffic generated by the lab.

**Network adapter configuration**

VirtualBox provides several network attachment modes.

| Mode | Description | Typical use |
| --- | --- | --- |
| NAT | Provides outbound connectivity through the host | Internet access for a simple lab |
| NAT Network | Allows multiple VMs to communicate through a shared NAT network | Multi-VM labs with outbound connectivity |
| Bridged Adapter | Connects the VM to the physical network through the host interface | Access from the physical LAN |
| Internal Network | Connects VMs using an isolated virtual network | Router-to-router and client-to-router labs |
| Host-only Adapter | Creates a network between the host and virtual machines | Management and isolated testing |

For a basic routing lab, you can use an Internal Network for the LAN side and NAT or Bridged Adapter for the WAN side.

# 🗁 VMware Workstation

VMware Workstation is another desktop virtualization platform that can run MikroTik CHR.

It provides virtual hardware and networking features suitable for building isolated networking environments.

**Basic requirements**

* VMware Workstation installed.
* MikroTik CHR image.
* Sufficient CPU and memory resources.
* Virtual network adapters.

**Basic setup workflow**

1. Download the CHR image from the official MikroTik website.
2. Prepare a virtual disk supported by VMware.
3. Create a new virtual machine.
4. Select the appropriate Linux guest configuration.
5. Attach the CHR virtual disk.
6. Assign CPU and memory resources.
7. Configure one or more virtual network adapters.
8. Start the virtual machine.
9. Connect to CHR through its console or network management interface.

**VMware network modes**

| Network mode | Description |
| --- | --- |
| Bridged | Connects the VM to the physical network |
| NAT | Provides network access through the host |
| Host-only | Creates an isolated network between the host and VMs |
| Custom | Connects the VM to a specific virtual network |

VMware can be useful when you need multiple virtual routers and client machines running on the same physical computer.

# 🗁 Proxmox VE

Proxmox Virtual Environment (Proxmox VE) is a virtualization platform commonly used to manage virtual machines and containers.

It is suitable for larger networking labs and persistent virtual infrastructure.

CHR can run as a virtual machine using a supported virtual disk format and virtual network interfaces.

**CHR on Proxmox architecture**

```text
                 Physical Server
                       |
                   Proxmox VE
                       |
                  Virtual Bridge
                       |
                  MikroTik CHR
                 /     |      \
                /      |       \
            vmbr0    vmbr1    vmbr2
              |        |        |
             WAN      LAN      DMZ
```

**Basic deployment workflow**

1. Install Proxmox VE on the physical server.
2. Download the CHR image from MikroTik.
3. Prepare or convert the virtual disk into a format supported by Proxmox.
4. Create a virtual machine.
5. Configure CPU and memory resources.
6. Attach the CHR virtual disk.
7. Add virtual network interfaces.
8. Connect the interfaces to the appropriate Proxmox bridges.
9. Start the CHR virtual machine.
10. Access RouterOS through the console or network.

**Proxmox networking concepts**

| Component | Purpose |
| --- | --- |
| Bridge (`vmbr`) | Connects virtual network interfaces |
| Physical NIC | Connects the Proxmox host to a physical network |
| Virtual NIC | Provides network connectivity to a VM |
| VLAN-aware bridge | Supports VLAN-aware virtual networking |
| Linux bridge | Provides Layer 2 connectivity between interfaces |

A Proxmox bridge can connect multiple CHR instances and client virtual machines into a larger virtual network.

For example, separate bridges can represent WAN, LAN, and DMZ network segments.

# 🗁 GNS3

GNS3 is a network simulation and emulation platform used to build and test network topologies.

It supports different network devices and virtual machines, including MikroTik CHR.

GNS3 is useful for practicing multi-router environments and understanding how different network components interact.

**Example topology**

```text
                    Internet
                       |
                    CHR-1
                  ISP Router
                       |
                    ether2
                       |
                    CHR-2
                 Core Router
                  /       \
                 /         \
              CHR-3       CHR-4
             Branch-1    Branch-2
                |           |
              Client      Client
```

This topology can be used to practice:

* Static routing.
* OSPF.
* BGP.
* NAT.
* Firewall filtering.
* Site-to-site VPNs.
* WAN failover.
* ISP routing scenarios.

**Basic GNS3 workflow**

1. Install GNS3.
2. Configure the GNS3 server or local virtualization backend.
3. Download a MikroTik CHR image.
4. Prepare the CHR image for the supported GNS3 virtualization backend.
5. Create a CHR template.
6. Add CHR instances to the workspace.
7. Connect virtual interfaces.
8. Start the devices.
9. Configure and test the network.

GNS3 integration details depend on the selected GNS3 version and virtualization backend.

Official website:

<https://www.gns3.com/>

# 🗁 EVE-NG

EVE-NG (Emulated Virtual Environment Next Generation) is a network emulation platform designed for building complex virtual networking labs.

It supports multi-vendor network environments and can be used to run MikroTik CHR instances alongside other virtual network devices.

**Example ISP lab topology**

```text
                  Upstream ISP
                       |
                     CHR-1
                  Edge Router
                       |
                     CHR-2
                  Core Router
                   /       \
                  /         \
               CHR-3       CHR-4
              BNG-1        BNG-2
                |            |
             Clients       Clients
```

This environment can be used to practice:

* ISP network architecture.
* PPPoE server and client configurations.
* RADIUS integration.
* OSPF and BGP.
* VLAN-based customer separation.
* CGNAT.
* Multi-router routing.
* Network troubleshooting.

**Basic EVE-NG workflow**

1. Install EVE-NG on a supported virtualization platform.
2. Download the MikroTik CHR image.
3. Prepare the image according to EVE-NG's image directory and naming requirements.
4. Create a MikroTik CHR node template.
5. Add CHR nodes to a lab.
6. Connect interfaces.
7. Start the nodes.
8. Access the RouterOS consoles.
9. Configure and test the topology.

EVE-NG image preparation and permissions must follow the documentation for the installed version.

Official website:

<https://www.eve-ng.net/>

# 🗁 Choosing a Virtual Lab Platform

Different platforms are suitable for different learning objectives.

| Requirement | Recommended platform |
| --- | --- |
| Basic RouterOS learning | VirtualBox |
| Desktop-based networking labs | VMware Workstation |
| Persistent virtual infrastructure | Proxmox VE |
| Multi-vendor routing simulations | GNS3 |
| Complex network and ISP labs | EVE-NG |
| Remote virtual router deployment | Cloud platform |

For learning RouterOS from the beginning, VirtualBox is a convenient starting point.

For advanced routing, ISP architecture, and multi-router scenarios, GNS3 or EVE-NG provides more flexible network topologies.

# 🗁 Virtual Lab Resource Planning

A virtual lab shares the physical computer's hardware resources among its virtual machines.

Before creating a large lab, plan the available resources.

**Example resource allocation**

| Component | Suggested resources |
| --- | --- |
| Basic CHR router | 1 vCPU, 256 MB RAM |
| Multiple routing protocols | 1–2 vCPU, 512 MB RAM |
| Advanced CHR router | 2 or more vCPU, 1 GB RAM or more |
| Linux client | 1 vCPU, 512 MB RAM or more |
| Windows client | 2 vCPU, 2 GB RAM or more |

These are starting points for lab planning, not fixed requirements. Actual resource consumption depends on the RouterOS configuration, traffic load, number of virtual interfaces, and host platform.

**Important resource considerations**

* CPU allocation affects routing and packet-processing performance.
* RAM allocation affects available memory for RouterOS and enabled services.
* Disk space is needed for virtual machine images and configuration files.
* Network adapter count determines how many virtual network connections are available.
* Host CPU and memory must also support the hypervisor and other running applications.

Avoid allocating more virtual resources than the physical host can reasonably provide.

# 🗁 Building an Isolated Virtual Network

A virtual lab should be designed so that experimental configurations do not accidentally affect a production network.

Use isolated virtual networks for testing.

Example:

```text
                   Host Computer
                        |
                  Virtual Network
                        |
                     CHR-1
                   /       \
                  /         \
               ether1      ether2
                 |            |
              WAN-LAB       LAN-LAB
                 |            |
              CHR-2        Linux VM
```

In this example:

* `WAN-LAB` represents the upstream network.
* `LAN-LAB` represents the internal test network.
* CHR-1 acts as the edge router.
* CHR-2 can act as another router in the topology.
* Linux VM acts as a test client.

For isolated testing, use internal or host-only virtual networks instead of bridging experimental interfaces directly into a production LAN.

# 🗁 Accessing CHR in a Virtual Lab

After starting the CHR virtual machine, you can access RouterOS through its console or network interfaces.

**Console access**

The hypervisor normally provides a console window for the virtual machine.

Use this console for initial configuration or recovery when network access is unavailable.

**WinBox access**

After configuring an IP address on a CHR interface, connect through WinBox.

Example:

```text
Router IP: 192.168.10.1
Username: admin
```

**SSH access**

From a Linux terminal:

```bash
ssh admin@192.168.10.1
```

**Verify interface configuration**

```routeros
/interface print
```

**Check IP addresses**

```routeros
/ip address print
```

**Check connectivity**

```routeros
/ping 192.168.10.10
```

Replace the example addresses with the addresses configured in your lab.

# 🗁 Practical Virtual Lab Projects

Once CHR is running, you can build practical networking projects to develop RouterOS skills.

| Lab | Project | Main concepts |
| --- | --- | --- |
| Basic router | Configure a router with LAN and WAN interfaces | Interfaces, IP addressing |
| DHCP network | Provide automatic IP configuration to clients | DHCP server and pools |
| NAT gateway | Allow LAN clients to access an upstream network | Source NAT and masquerade |
| Firewall | Restrict traffic between network segments | Firewall filter rules |
| VLAN network | Separate departments using VLANs | Bridges, VLAN tagging |
| Multi-router network | Connect multiple routers | Static routing |
| Dynamic routing | Exchange routes between routers | OSPF |
| ISP simulation | Connect multiple customer networks | PPPoE, RADIUS |
| Dual-WAN router | Configure two upstream connections | Routing, failover |
| Load balancing | Distribute traffic across multiple WAN connections | PCC and policy routing |
| VPN network | Connect two remote networks | WireGuard or IPsec |
| Monitoring system | Monitor router performance and traffic | Logging and monitoring |

These projects can be expanded as you progress through the MikroTik RouterOS curriculum.

# 🗁 Best Practices for Virtual Labs

Follow these practices to keep virtual labs organized and reliable:

* Use descriptive names for virtual machines and network interfaces.
* Keep a documented network topology.
* Use separate virtual networks for WAN, LAN, and management.
* Save clean virtual machine snapshots before major experiments.
* Export RouterOS configurations before making significant changes.
* Use separate RouterOS accounts for different administrative purposes.
* Avoid exposing management services to untrusted networks.
* Keep CHR images and virtualization software updated.
* Monitor host CPU, memory, and disk usage.
* Test configurations in an isolated lab before applying them to physical routers.
* Document the purpose and result of each lab exercise.

# 🗁 Official Documentation and Resources

* [MikroTik CHR](https://help.mikrotik.com/docs/spaces/ROS/pages/18350234/Cloud+Hosted+Router+CHR)
* [MikroTik Downloads](https://mikrotik.com/download)
* [VirtualBox](https://www.virtualbox.org/)
* [VMware](https://www.vmware.com/)
* [Proxmox VE](https://www.proxmox.com/en/products/proxmox-virtual-environment/overview)
* [GNS3](https://www.gns3.com/)
* [EVE-NG](https://www.eve-ng.net/)
