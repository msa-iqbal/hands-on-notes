# GNS3 and EVE-NG

GNS3 and EVE-NG are network emulation platforms used to build, configure, and troubleshoot virtual network topologies.

They allow you to connect virtual routers, switches, firewalls, servers, and client machines to create realistic networking environments.

Unlike a basic VirtualBox lab, these platforms provide specialized tools for designing complex network topologies and connecting multiple network devices.

They are useful for practicing:

* Routing protocols such as OSPF and BGP.
* VLANs and switching.
* NAT and firewall configuration.
* Network segmentation.
* VPN and tunneling.
* Multi-router network architectures.
* ISP network scenarios.
* Network troubleshooting.
* Packet capture and traffic analysis.

# 🗁 Understanding GNS3

<text>**GNS3 (Graphical Network Simulator-3)**</text> is a network emulation platform that allows you to design and run virtual network topologies.

It supports different types of devices and execution environments.

Common components include:

* GNS3 GUI: The graphical interface for designing topologies.
* GNS3 Server: Manages network projects and device instances.
* GNS3 VM: An optional virtual machine that provides a dedicated environment for running supported appliances.
* Dynamips: Emulates certain Cisco router platforms.
* QEMU: Runs virtual machine appliances.
* Docker: Runs supported container-based network services.
* VirtualBox integration: Connects supported VirtualBox virtual machines to GNS3 topologies.

Official website:

<https://www.gns3.com/>

Official documentation:

<https://docs.gns3.com/>

# 🗁 Understanding EVE-NG

<text>**EVE-NG (Emulated Virtual Environment Next Generation)**</text> is a network emulation platform designed to run virtual networking appliances in interconnected lab topologies.

It provides a web-based interface for creating and managing network labs.

Common capabilities include:

* Browser-based lab management.
* Virtual router and switch integration.
* Firewall appliance support.
* Linux and Windows virtual machines.
* Multi-vendor network environments.
* Network topology documentation.
* Packet capture integration.

EVE-NG is particularly useful for building larger enterprise and service-provider networking labs.

Official website:

<https://www.eve-ng.net/>

Official documentation:

<https://www.eve-ng.net/index.php/documentation/>

# 🗁 GNS3 vs EVE-NG

| Feature | GNS3 | EVE-NG |
| --- | --- | --- |
| Interface | Desktop application | Web-based interface |
| Topology design | Graphical workspace | Browser-based workspace |
| Virtual appliances | Supported | Supported |
| QEMU integration | Yes | Yes |
| Docker integration | Supported | Depends on edition and configuration |
| Multi-vendor labs | Yes | Yes |
| Remote lab access | Supported through server architecture | Supported through server architecture |
| Learning curve | Moderate | Moderate |
| Typical use | Flexible network simulation and emulation | Centralized network emulation |

Both platforms are useful. The choice depends on your hardware, preferred workflow, appliance requirements, and lab size.

# 🗁 Hardware Requirements

GNS3 and EVE-NG can consume significant system resources, particularly when running multiple virtual routers and servers.

Recommended starting specifications:

| Component | Basic Environment | Recommended Environment |
| --- | --- | --- |
| CPU | 4 cores | 8 or more cores |
| RAM | 16 GB | 32 GB or more |
| Storage | 100 GB free | 250 GB or more SSD |
| Virtualization | VT-x or AMD-V | Hardware virtualization enabled |
| Network | Ethernet or Wi-Fi | Stable Ethernet connection |

These specifications are guidelines. Actual requirements depend on the number of running appliances, their memory allocation, and the complexity of the topology.

A large lab with multiple firewalls, Windows servers, and virtual routers may require substantially more resources.

# 🗁 Understand the Virtualization Architecture

GNS3 and EVE-NG use virtualization technologies to execute network appliances.

A simplified architecture looks like this:

```text
             Physical Computer
                    |
             Host Operating System
                    |
             Virtualization Layer
                    |
          GNS3 Server / EVE-NG VM
                    |
             Virtual Network
                    |
        +-----------+-----------+
        |           |           |
      Router     Firewall     Server
        |           |           |
        +-----------+-----------+
                    |
                 Clients
```

The virtualization layer provides CPU, memory, storage, and network resources to the lab environment.

Depending on the platform and deployment model, appliances may run directly on the host, inside a dedicated VM, or on a remote server.

# 🗁 Understand QEMU

QEMU is a machine emulator and virtualizer used by GNS3 and EVE-NG to run supported virtual appliances.

It can execute virtual machines containing:

* MikroTik CHR.
* Linux routers.
* Firewall appliances.
* Supported vendor network operating systems.
* Other QEMU-compatible virtual machines.

QEMU allows networking devices to run as virtual machines and connect to virtual network segments.

# 🗁 Understand Dynamips

Dynamips is an emulator for certain Cisco router hardware platforms.

It is mainly associated with older Cisco router images and learning environments.

Modern network labs often use QEMU-based appliances instead.

# 🗁 Install GNS3 on Ubuntu

GNS3 can be installed on Ubuntu using supported packages or the official installation instructions.

Start by updating the system:

```bash
sudo apt update
sudo apt upgrade -y
```

Install commonly required dependencies:

```bash
sudo apt install -y \
  python3 \
  python3-pip \
  python3-venv \
  pipx \
  qemu-system-x86 \
  qemu-utils \
  libvirt-daemon-system \
  libvirt-clients \
  bridge-utils \
  wireshark \
  ubridge \
  dynamips
```

Package names and availability may differ between Ubuntu releases.

For the GNS3 GUI and server, follow the installation instructions for your specific Ubuntu version:

<https://docs.gns3.com/docs/getting-started/installation/linux/>

Do not assume that installing the GUI alone provides a complete environment for every appliance. Configure the GNS3 server and virtualization backend required by your selected lab devices.

# 🗁 Configure GNS3 Server

After installing GNS3, start the application and complete the initial setup wizard.

Choose the server architecture that matches your environment.

Common options include:

* Local server.
* GNS3 VM.
* Remote GNS3 server.

A local server is convenient for small labs when the host has sufficient resources.

The GNS3 VM is useful when you need a dedicated virtualization environment for supported appliances.

For remote labs, the server must be reachable from the client and properly configured.

# 🗁 Install EVE-NG

EVE-NG is commonly deployed as a dedicated virtual machine.

The general installation workflow is:

* Download the official EVE-NG installation image.
* Create a virtual machine using a supported hypervisor.
* Allocate sufficient CPU, memory, and storage.
* Enable hardware virtualization support where required.
* Configure the management network.
* Install EVE-NG.
* Access the management interface through a web browser.

Official download and installation instructions:

<https://www.eve-ng.net/index.php/download/>

Follow the requirements for the selected EVE-NG edition and hypervisor.

# 🗁 Configure EVE-NG Management Access

After installation, determine the management IP address assigned to the EVE-NG virtual machine.

From your host computer, open:

```text
https://EVE-NG-IP-ADDRESS
```

Replace `EVE-NG-IP-ADDRESS` with the actual address of your installation.

Log in using the credentials configured during installation.

For a management network, use an IP addressing plan that does not conflict with your existing LAN, VPN, or other virtual networks.

Keep management access restricted to trusted devices.

# 🗁 Understand Network Appliances

A network appliance is a virtual machine or container that provides a particular networking function.

Examples:

| Appliance | Purpose |
| --- | --- |
| MikroTik CHR | RouterOS routing and firewall |
| Cisco IOSv | Cisco virtual router |
| Cisco IOSvL2 | Virtual Layer 2 switching |
| Linux | Routing, firewall, and network services |
| pfSense | Firewall and routing |
| OPNsense | Firewall and routing |
| FortiGate VM | Enterprise firewall |
| Ubuntu Server | DNS, DHCP, web, and other services |

Appliance availability depends on platform support, image format, licensing, and vendor distribution policies.

Some commercial appliances require an authorized license or account.

Only use images you are legally authorized to use.

# 🗁 Import MikroTik CHR into GNS3

MikroTik CHR can run as a QEMU virtual machine.

Download the CHR disk image from:

<https://mikrotik.com/download>

Select the appropriate virtual disk format for your workflow.

If your image is in VDI format, convert it to QCOW2 when required by your QEMU template:

```bash
qemu-img convert -f vdi -O qcow2 chr-7.x.vdi chr-7.x.qcow2
```

Replace the filenames with the actual image names.

Verify the resulting image:

```bash
qemu-img info chr-7.x.qcow2
```

In GNS3, create a QEMU virtual machine template and configure:

* Template name: MikroTik CHR.
* Disk image: Converted CHR image.
* RAM: 256–512 MB initially.
* CPU: 1 vCPU initially.
* Network adapters: As required by the topology.
* Console type: Compatible with the appliance.

The exact template fields depend on your GNS3 version and appliance configuration.

# 🗁 Import MikroTik CHR into EVE-NG

EVE-NG supports QEMU-based network appliances.

The process generally involves:

* Downloading the CHR image.
* Converting the image to the required disk format if necessary.
* Creating the appropriate EVE-NG image directory.
* Placing the disk image in the correct directory.
* Applying the required ownership and permissions.
* Creating a node template or using a supported template.
* Adding the CHR node to a lab.

The exact directory name and disk filename must follow the template requirements for your EVE-NG version.

Refer to the official EVE-NG documentation for the supported MikroTik appliance setup procedure.

# 🗁 Create Your First GNS3 Topology

Build a simple network with two routers and two client machines.

```text
          Client-1
             |
             |
           R1
             |
             |
           R2
             |
             |
          Client-2
```

Suggested addressing plan:

| Device | Interface | IP Address |
| --- | --- | --- |
| Client-1 | Ethernet | 192.168.10.10/24 |
| R1 | LAN | 192.168.10.1/24 |
| R1 | WAN | 10.0.0.1/30 |
| R2 | WAN | 10.0.0.2/30 |
| R2 | LAN | 192.168.20.1/24 |
| Client-2 | Ethernet | 192.168.20.10/24 |

This topology provides an environment for practicing:

* Interface configuration.
* Static routing.
* Default gateways.
* Inter-network communication.
* Packet forwarding.
* Routing troubleshooting.

# 🗁 Create Your First EVE-NG Lab

Create a new lab through the EVE-NG web interface.

Use a meaningful name:

```text
basic-routing-lab
```

Add the following nodes:

* Two MikroTik CHR routers.
* Two Linux clients.
* One network segment for each LAN.
* One transit network between routers.

Connect the nodes according to the topology.

Start the nodes and open their consoles.

Configure IP addresses and routes using the CLI of each appliance.

The configuration commands depend on the operating system running on each node.

# 🗁 Understand Virtual Network Connections

GNS3 and EVE-NG provide virtual network connections between appliances.

A network connection represents a virtual link between interfaces.

For example:

```text
Router-1 ether1
       |
       |
 Virtual Link
       |
       |
Router-2 ether1
```

A virtual link can represent an Ethernet connection between two routers.

When designing topologies, distinguish between:

* Management networks.
* Transit networks.
* LAN segments.
* WAN connections.
* Isolated testing networks.

This separation makes larger labs easier to understand and troubleshoot.

# 🗁 Connect a Lab to an External Network

Both platforms can connect virtual topologies to external networks through supported network objects or adapters.

Possible use cases include:

* Internet access for virtual machines.
* Accessing a router from the host.
* Connecting to physical networking equipment.
* Testing services from an external client.

For initial exercises, prefer NAT or an isolated management connection.

Use bridged connections only when direct physical-network connectivity is necessary and the topology is properly controlled.

# 🗁 Capture Network Traffic

Packet capture is useful for examining communication between virtual devices.

GNS3 and EVE-NG provide packet capture capabilities for supported links.

You can analyze captured traffic using Wireshark.

Common protocols to inspect include:

* ARP.
* ICMP.
* DHCP.
* DNS.
* TCP.
* UDP.
* OSPF.
* BGP.

For example, an ICMP ping between two routers can be inspected to understand:

* Source and destination IP addresses.
* ICMP request and reply messages.
* Ethernet source and destination MAC addresses.
* TTL values.
* Packet timing.

Packet capture analysis helps connect theoretical protocol knowledge with actual network behavior.

# 🗁 Common GNS3 and EVE-NG Problems

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| Appliance does not start | Invalid image or template | Verify image format and template |
| QEMU fails | Insufficient resources or incorrect configuration | Check CPU, memory, and QEMU settings |
| GNS3 VM unavailable | Hypervisor integration issue | Verify GNS3 VM configuration |
| EVE-NG inaccessible | Management network problem | Check IP address and virtual adapter |
| Nodes cannot communicate | Incorrect link or interface configuration | Inspect topology and IP addressing |
| High CPU usage | Too many active appliances | Stop unused nodes and optimize resources |
| Image permission errors | Incorrect ownership or permissions | Follow platform-specific image installation instructions |
| Packet capture unavailable | Incorrect link selection or capture configuration | Select a supported link and start capture |

# 🗁 GNS3 and EVE-NG Best Practices

* Start with small topologies.
* Use consistent device names.
* Maintain an IP addressing plan.
* Organize labs by topic.
* Save configurations regularly.
* Back up important lab projects.
* Use authorized appliance images.
* Monitor CPU, memory, and storage usage.
* Separate management and lab networks.
* Document topology diagrams.
* Capture traffic when troubleshooting.
* Avoid connecting experimental appliances directly to production networks.

# 🗁 Lab Exercise

Build a small routed network using GNS3 or EVE-NG.

**Requirements**

* GNS3 or EVE-NG installed.
* Two virtual routers.
* Two Linux client machines.
* Three virtual network segments.
* A documented IP addressing plan.

**Tasks**

* Create a new network lab.
* Add two virtual routers.
* Add two Linux clients.
* Connect the devices.
* Configure router interfaces.
* Assign IP addresses to clients.
* Configure static routes.
* Test communication between both LANs.
* Capture ICMP traffic.
* Save the lab and export its topology documentation.

**Expected Results**

* All virtual devices start successfully.
* Router interfaces have the correct addresses.
* Both LANs can communicate through the routers.
* Static routes work correctly.
* Packet captures show the expected traffic.
* The lab can be reopened and continued.

# 🗁 Summary

GNS3 and EVE-NG provide flexible environments for building and testing complex network topologies.

You should now understand:

* GNS3 architecture and components.
* EVE-NG architecture and management.
* QEMU and Dynamips.
* Virtual network appliances.
* MikroTik CHR integration.
* Virtual network connections.
* External network connectivity.
* Packet capture.
* Basic topology design.

These platforms will be useful for the upcoming connectivity, subnetting, routing, firewall, VLAN, VPN, and ISP networking exercises.
