# Networking Fundamentals

## 🗁 Overview

Networking fundamentals introduce the core concepts required to understand, design, configure, operate, and troubleshoot computer networks.

This section explains how devices communicate, how networks are classified, how network topologies are organized, how networking devices operate, and how Ethernet and MAC addressing enable communication across local networks.

These concepts provide the foundation for studying network models, IP addressing, transport protocols, switching, routing, network security, and MikroTik RouterOS.

## 🗁 Learning Objectives

By completing this section, you will be able to:

- Explain what a computer network is and how network communication works.
- Identify the major components of a network.
- Differentiate LAN, WLAN, WAN, PAN, MAN, and other network types.
- Explain physical and logical network topologies.
- Identify routers, switches, bridges, access points, modems, and other networking devices.
- Compare copper, fiber-optic, and wireless transmission media.
- Explain Ethernet frames and their role in local network communication.
- Understand MAC addresses and how switches learn them.
- Differentiate unicast, broadcast, and multicast communication.
- Use fundamental networking terminology accurately.
- Perform basic network inspection and connectivity tests.

## 🗁 Prerequisites

- Basic computer literacy.
- Familiarity with operating systems and command-line interfaces is helpful but not required.
- No previous networking experience is necessary.

## 🗁 Topics

| File | Topic |
| --- | --- |
| `01-basics.md` | Computer networking fundamentals |
| `02-network-types.md` | Network classifications and use cases |
| `03-network-topologies.md` | Physical and logical network layouts |
| `04-network-devices.md` | Networking hardware and device roles |
| `05-network-media.md` | Copper, fiber-optic, and wireless media |
| `06-ethernet.md` | Ethernet standards, frames, and communication |
| `07-mac-address.md` | MAC address structure and operation |
| `08-unicast-broadcast-multicast.md` | Communication delivery models |
| `09-network-terminologies.md` | Essential networking vocabulary |

## 🗁 Recommended Learning Order

Study the files in numerical order. Begin with basic communication concepts, then learn how networks are classified and built. Next, study network devices and transmission media before examining Ethernet frames, MAC addresses, and delivery types.

Finish by reviewing the terminology reference and completing the practical exercises.

## 🗁 Practical Labs

By the end of this section, you should be able to:

1. Identify the network interfaces available on your computer.
2. Display your MAC address and current IP configuration.
3. Inspect your default gateway and local network.
4. Test connectivity to another device.
5. Observe Ethernet and IP traffic with Wireshark.
6. Identify the source and destination MAC addresses in a captured Ethernet frame.
7. Explain why communication within a local network differs from communication with a remote network.

## 🗁 Useful Tools

- `ping` — test IP reachability.
- `ipconfig` — inspect network configuration on Windows.
- `ip addr` — inspect IP addresses on Linux.
- `ip link` — inspect Linux network interfaces and link-layer addresses.
- `ip route` — inspect Linux routing information.
- `getmac` — display MAC addresses on Windows.
- `arp -a` — inspect the IPv4 neighbor cache on Windows.
- `ip neigh` — inspect the neighbor cache on Linux.
- Wireshark — capture and analyze network traffic.
- MikroTik WinBox and RouterOS CLI — inspect and configure MikroTik devices.

## 🗁 Key Takeaway

Computer networking is the foundation of modern communication. Understanding devices, transmission media, Ethernet, MAC addressing, and delivery methods makes the higher-level topics of IP networking, routing, security, and network engineering much easier to learn.
