# Lab Environment Setup

A networking lab environment provides a controlled space to learn, configure, test, and troubleshoot network technologies without affecting production infrastructure.

Instead of purchasing multiple physical routers, switches, and servers, you can use virtualization and network simulation tools to build complex network topologies on a single computer.

A properly configured lab environment allows you to practice:

* IP addressing and subnetting.
* Ethernet and switching.
* VLAN configuration.
* Routing protocols.
* NAT and firewall rules.
* DHCP and DNS services.
* VPN and tunneling.
* Network monitoring.
* Packet capture and analysis.
* MikroTik RouterOS configuration.

# 🗁 Types of Networking Labs

**Physical Lab**

Uses real networking equipment.

Examples:

* MikroTik routers.
* Cisco routers and switches.
* Managed Ethernet switches.
* Wireless access points.
* Ethernet cables.
* Servers and client computers.

Advantages:

* Real hardware behavior.
* Physical interface experience.
* Practical cabling and connectivity.
* Hardware-specific troubleshooting.

Limitations:

* Higher equipment cost.
* Physical space requirements.
* Power consumption.
* Limited flexibility when building large topologies.

**Virtual Lab**

Uses virtual machines and virtual networking interfaces.

Examples:

* VirtualBox.
* VMware Workstation.
* MikroTik CHR.
* Ubuntu Server.
* Windows virtual machines.

Advantages:

* Lower equipment cost.
* Flexible network design.
* Easy snapshots and restoration.
* Multiple virtual devices on one computer.
* Suitable for repeatable experiments.

**Network Simulation Lab**

Uses specialized software to create and simulate network topologies.

Examples:

* GNS3.
* EVE-NG.

Advantages:

* Large network topologies.
* Multiple virtual networking devices.
* Centralized lab management.
* Useful for routing and switching practice.

# 🗁 Hardware Requirements

The required hardware depends on the size and complexity of your labs.

| Component | Basic Lab | Recommended Lab |
| --- | --- | --- |
| CPU | 4 cores | 8 or more cores |
| RAM | 8 GB | 16–32 GB |
| Storage | 100 GB free | 250 GB or more SSD |
| Virtualization | Hardware virtualization support | VT-x or AMD-V enabled |
| Network | Ethernet or Wi-Fi | Ethernet preferred for stable lab connectivity |

These are practical guidelines, not strict requirements. Large GNS3 or EVE-NG environments may require significantly more memory and CPU resources.

# 🗁 Software Requirements

Prepare the following software:

| Software | Purpose |
| --- | --- |
| VirtualBox | Virtual machine management |
| MikroTik CHR | RouterOS virtual router |
| Ubuntu Server | Linux networking practice |
| Wireshark | Packet capture and analysis |
| GNS3 | Network topology simulation |
| EVE-NG | Network emulation environment |
| SSH Client | Remote device management |
| Web Browser | Access web-based management interfaces |

You do not need to install every tool immediately. Start with VirtualBox and a small number of virtual machines, then expand your environment as you progress.

# 🗁 Enable Hardware Virtualization

Hardware virtualization allows a computer to run virtual machines efficiently.

Common technologies:

* Intel VT-x.
* AMD-V (SVM).

Check CPU virtualization support on Linux:

```bash
lscpu | grep -i virtualization
```

Possible output:

```text
Virtualization: VT-x
```

or:

```text
Virtualization: AMD-V
```

Check virtualization flags:

```bash
grep -Eoc '(vmx|svm)' /proc/cpuinfo
```

A result greater than zero indicates that the CPU exposes virtualization-related flags to the operating system.

If virtualization is unavailable:

* Check whether the processor supports it.
* Enable virtualization in BIOS or UEFI.
* Verify that the operating system can access the feature.

# 🗁 Prepare the Host Operating System

The host operating system is the physical computer's operating system that runs your virtualization software.

For a Linux-based lab, Ubuntu is a practical choice.

Update Ubuntu:

```bash
sudo apt update
sudo apt upgrade -y
```

Install useful networking tools:

```bash
sudo apt install -y \
  iproute2 \
  iputils-ping \
  traceroute \
  tcpdump \
  dnsutils \
  net-tools \
  curl \
  wget \
  openssh-client \
  openssh-server \
  nmap \
  iperf3 \
  wireshark
```

Verify important commands:

```bash
ip --version
ping -V
traceroute --version
tcpdump --version
nmap --version
```

# 🗁 Create a Lab Directory

Organize your lab files so that virtual machines, configurations, diagrams, and packet captures remain easy to manage.

Example directory structure:

```text
networking-lab/
│
├── virtual-machines/
│
├── iso-images/
│
├── router-configurations/
│
├── switch-configurations/
│
├── network-topologies/
│
├── packet-captures/
│
├── documentation/
│
└── backups/
```

Create the directories:

```bash
mkdir -p ~/networking-lab/{virtual-machines,iso-images,router-configurations,switch-configurations,network-topologies,packet-captures,documentation,backups}
```

Verify:

```bash
tree ~/networking-lab
```

If `tree` is not installed:

```bash
sudo apt install tree
```

# 🗁 Install VirtualBox

VirtualBox is a virtualization platform that allows you to create and manage virtual machines.

It is useful for running:

* MikroTik CHR.
* Ubuntu Server.
* Windows.
* Other supported operating systems.

Download VirtualBox from its official website:

<https://www.virtualbox.org/>

On Ubuntu, installation can also be performed through the distribution's package manager:

```bash
sudo apt update
sudo apt install virtualbox
```

Verify installation:

```bash
VBoxManage --version
```

VirtualBox package versions and extension-pack availability depend on the distribution repositories and installed release.

# 🗁 Understand Virtual Machine Networking

VirtualBox provides several networking modes.

| Mode | Description |
| --- | --- |
| NAT | VM accesses external networks through the host's NAT |
| NAT Network | Multiple VMs can communicate through a shared NAT network |
| Bridged Adapter | VM connects to the physical network through a host interface |
| Internal Network | VMs communicate with each other on an isolated virtual network |
| Host-only Adapter | VMs communicate with the host and other host-only VMs |
| Generic Driver | Uses specialized networking backends |

**NAT**

Useful when a virtual machine needs Internet access without being directly exposed to the physical LAN.

**Bridged Adapter**

Useful when a VM needs to appear as a separate device on the physical network.

**Internal Network**

Useful for isolated networking exercises where only selected virtual machines should communicate.

**Host-only Adapter**

Useful for management access between the host computer and virtual machines.

# 🗁 Design a Basic Lab Topology

Start with a simple topology containing one router and two client machines.

```text
             Host Computer
                   |
              VirtualBox
                   |
             MikroTik CHR
               ether1
                   |
             Virtual Network
               /       \
              /         \
          Client 1    Client 2
          Ubuntu      Ubuntu
```

Suggested IP addressing:

| Device | Interface | IP Address |
| --- | --- | --- |
| MikroTik CHR | LAN | 192.168.10.1/24 |
| Client 1 | Ethernet | 192.168.10.10/24 |
| Client 2 | Ethernet | 192.168.10.20/24 |

Default gateway for both clients:

```text
192.168.10.1
```

This topology can be expanded later with additional routers, VLANs, switches, and external network connections.

# 🗁 Configure Virtual Network Isolation

Isolated networks are important because they prevent experimental configurations from affecting your production network.

For basic labs:

* Use VirtualBox Internal Network for isolated device-to-device communication.
* Use Host-only Adapter when host access is required.
* Use NAT when Internet access is required.
* Use Bridged Adapter only when direct physical-network connectivity is necessary.

Example network separation:

```text
Physical Network
       |
      Host
       |
   VirtualBox
       |
  Internal Network
       |
  MikroTik CHR
       |
  Lab Clients
```

Keep experimental DHCP servers, firewall rules, and routing configurations inside isolated lab networks.

# 🗁 Prepare Virtual Machine Resources

For a small lab, you can start with the following resource allocation.

| Virtual Machine | RAM | CPU |
| --- | ---: | ---: |
| MikroTik CHR | 256–512 MB | 1 vCPU |
| Ubuntu Server | 1–2 GB | 1–2 vCPU |
| Additional Ubuntu Client | 1–2 GB | 1–2 vCPU |
| Windows Client | 4 GB or more | 2 vCPU |

These are starting points. Actual resource requirements depend on RouterOS services, operating-system versions, workload, and the number of devices running simultaneously.

Avoid allocating all host memory and CPU resources to virtual machines. The host operating system also needs sufficient resources.

# 🗁 Verify the Lab Environment

After creating the initial virtual network, verify connectivity.

From Client 1:

```bash
ip address
```

Check the routing table:

```bash
ip route
```

Test the gateway:

```bash
ping -c 4 192.168.10.1
```

Test Client 2:

```bash
ping -c 4 192.168.10.20
```

Check DNS resolution:

```bash
dig example.com
```

Check Internet connectivity when Internet access has been configured:

```bash
ping -c 4 1.1.1.1
```

A successful ping to the gateway confirms basic IP reachability, but does not prove that DNS or Internet connectivity is working.

# 🗁 Configure SSH Management

SSH provides secure command-line access to Linux servers and supported network devices.

On Ubuntu Server, install the SSH server:

```bash
sudo apt install openssh-server
```

Check service status:

```bash
sudo systemctl status ssh
```

Find the VM's IP address:

```bash
ip address show
```

Connect from the host:

```bash
ssh username@192.168.10.10
```

Replace `username` and the IP address with the actual values of your virtual machine.

# 🗁 Use Virtual Machine Snapshots

Snapshots preserve a virtual machine's state at a particular point in time.

They are useful before:

* Changing IP addressing.
* Configuring firewall rules.
* Installing networking software.
* Changing routing protocols.
* Testing potentially disruptive configurations.

Recommended snapshot workflow:

```text
Create Virtual Machine
        |
        v
Install Operating System
        |
        v
Configure Initial Network
        |
        v
Create Snapshot
        |
        v
Perform Lab Exercise
        |
        v
Verify Configuration
        |
        v
Keep or Restore Snapshot
```

A snapshot is not a replacement for a proper backup. Maintain separate backups of important virtual disks and configuration files.

# 🗁 Common Lab Setup Problems

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| Virtual machine cannot start | Virtualization disabled | Enable VT-x or AMD-V |
| VM has no Internet access | Incorrect virtual network mode | Check NAT or bridged configuration |
| VMs cannot communicate | Different virtual networks | Connect them to the same network |
| Host cannot access VM | Incorrect host-only configuration | Verify host-only adapter settings |
| VM cannot obtain DHCP address | DHCP service unavailable | Check virtual network and DHCP configuration |
| Poor VM performance | Insufficient host resources | Adjust CPU and memory allocation |
| SSH connection fails | SSH service stopped or blocked | Check service and firewall |
| Duplicate IP address | Address conflict | Assign unique IP addresses |

# 🗁 Lab Exercise

Build a basic virtual networking environment.

**Requirements**

* VirtualBox installed.
* One MikroTik CHR virtual machine.
* Two Ubuntu virtual machines.
* One isolated virtual network.

**Tasks**

* Create the virtual machines.
* Configure the virtual network.
* Assign IP addresses to the clients.
* Configure MikroTik CHR as the default gateway.
* Test communication between both clients.
* Test connectivity to the gateway.
* Configure SSH access to an Ubuntu VM.
* Capture ICMP packets using tcpdump.
* Save a snapshot after completing the configuration.

**Expected Results**

* All virtual machines start successfully.
* Both clients have unique IP addresses.
* Both clients can reach the MikroTik gateway.
* Both clients can communicate with each other.
* SSH management works from the host where configured.
* Packet captures show the ICMP communication.

# 🗁 Best Practices

* Keep lab networks isolated from production networks.
* Maintain organized virtual machine names.
* Document IP addressing and interface assignments.
* Take snapshots before major configuration changes.
* Keep important configurations backed up.
* Monitor host CPU, RAM, and storage usage.
* Use consistent interface and device naming.
* Build simple topologies before attempting complex designs.
* Verify every configuration step.
* Record troubleshooting findings.

# 🗁 Summary

A properly prepared lab environment is the foundation of practical networking education.

You should now understand:

* Physical, virtual, and simulated networking labs.
* Hardware and software requirements.
* Hardware virtualization.
* VirtualBox networking modes.
* Network isolation.
* Basic IP addressing and connectivity.
* SSH-based management.
* Snapshots and backup practices.

The next step is to build virtual routers using MikroTik Cloud Hosted Router and connect them to virtual machines.
