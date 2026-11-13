# Basic Connectivity Labs

Basic connectivity labs teach you how network devices communicate with one another using Ethernet, IP addressing, ARP, ICMP, and routing.

Before working with advanced technologies such as VLANs, OSPF, BGP, NAT, and VPNs, you should understand how to establish and troubleshoot basic network connectivity.

These exercises use virtual routers and client machines to demonstrate fundamental networking concepts.

You will practice:

* Connecting network devices.
* Configuring Ethernet interfaces.
* Assigning IPv4 addresses.
* Configuring default gateways.
* Understanding ARP.
* Testing connectivity using ICMP.
* Inspecting routing tables.
* Using ping and traceroute.
* Capturing packets with tcpdump and Wireshark.
* Troubleshooting connectivity problems.

# 🗁 Lab Environment

You can perform these exercises using VirtualBox, GNS3, or EVE-NG.

Recommended devices:

| Device | Purpose |
| --- | --- |
| MikroTik CHR | Router configuration |
| Ubuntu Server | Client and server testing |
| Virtual Ethernet Network | Device connectivity |
| Wireshark | Packet analysis |
| tcpdump | Command-line packet capture |

For these exercises, assume the following interface names:

* MikroTik router interfaces: `ether1`, `ether2`.
* Ubuntu client interface: `enp0s3`.

Your actual interface names may differ.

# 🗁 Understand Basic Network Communication

A basic IPv4 network requires devices to have compatible IP addressing and a working Layer 2 connection.

For example:

```text
       Ubuntu Client
       192.168.10.10/24
              |
              |
         Ethernet
              |
              |
       MikroTik Router
       192.168.10.1/24
```

Both devices belong to the same subnet:

```text
192.168.10.0/24
```

The client can communicate directly with the router because they share the same IP network.

# 🗁 Lab: Configure a Basic Ethernet Connection

**Objective**

Connect two Ubuntu virtual machines through the same virtual network.

**Topology**

```text
+----------------+       +----------------+
|   Ubuntu-01    |       |   Ubuntu-02    |
| 192.168.10.10  |       | 192.168.10.20  |
+----------------+       +----------------+
        |                         |
        +-----------+-------------+
                    |
              Virtual Switch
```

**IP addressing plan**

| Device | IP Address | Subnet Mask |
| --- | --- | --- |
| Ubuntu-01 | 192.168.10.10 | 255.255.255.0 |
| Ubuntu-02 | 192.168.10.20 | 255.255.255.0 |

Connect both virtual machines to the same VirtualBox Internal Network, GNS3 Ethernet switch, or EVE-NG network segment.

Identify the network interface on Ubuntu-01:

```bash
ip -br link
```

Assign an IP address:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
sudo ip link set enp0s3 up
```

On Ubuntu-02:

```bash
sudo ip address add 192.168.10.20/24 dev enp0s3
sudo ip link set enp0s3 up
```

Verify both configurations:

```bash
ip -br address
```

Test connectivity from Ubuntu-01:

```bash
ping -c 4 192.168.10.20
```

**Expected Result**

Ubuntu-01 should receive ICMP Echo Replies from Ubuntu-02.

No default gateway is required for communication between these two devices because they belong to the same subnet.

# 🗁 Lab: Configure a MikroTik Router Interface

**Objective**

Configure a MikroTik CHR interface and establish connectivity between a router and an Ubuntu client.

**Topology**

```text
+------------------+
|   Ubuntu Client  |
| 192.168.10.10/24 |
+------------------+
         |
         |
    Virtual Switch
         |
         |
+------------------+
|  MikroTik CHR    |
| 192.168.10.1/24  |
+------------------+
```

Configure the MikroTik interface:

```routeros
/interface print
```

Identify the interface connected to the client network.

Assign an IP address:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2 comment="Lab LAN"
```

Verify:

```routeros
/ip address print
```

Configure the Ubuntu client:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
sudo ip link set enp0s3 up
```

Test the router:

```bash
ping -c 4 192.168.10.1
```

Test from MikroTik:

```routeros
/ping 192.168.10.10
```

**Expected Result**

Both devices should successfully exchange ICMP packets.

# 🗁 Lab: Configure a Default Gateway

**Objective**

Understand how a default gateway allows a device to communicate with destinations outside its local subnet.

**Topology**

```text
       Ubuntu Client
       192.168.10.10/24
              |
              |
       MikroTik Router
       192.168.10.1/24
              |
              |
         WAN Network
```

Configure the client IP address:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
```

Add a default route:

```bash
sudo ip route add default via 192.168.10.1
```

Inspect the routing table:

```bash
ip route
```

Expected route:

```text
default via 192.168.10.1 dev enp0s3
```

The default route tells Ubuntu to forward packets for destinations without a more specific matching route to `192.168.10.1`.

Verify the gateway:

```bash
ping -c 4 192.168.10.1
```

**Expected Result**

The client should be able to reach its default gateway.

External connectivity additionally requires the router to have a valid route and appropriate forwarding and firewall configuration.

# 🗁 Lab: Understand ARP

**Objective**

Observe how IPv4 devices discover the MAC address associated with an IP address on a local network.

ARP stands for Address Resolution Protocol.

When a device needs to send an Ethernet frame to another device on the same IPv4 subnet, it needs the destination MAC address.

If that address is not already cached, the device sends an ARP request.

**Topology**

```text
Ubuntu-01                     Ubuntu-02
192.168.10.10                 192.168.10.20
     |                             |
     +---------- Switch -----------+
```

Clear the ARP neighbor cache on Ubuntu-01:

```bash
sudo ip neigh flush all
```

Ping Ubuntu-02:

```bash
ping -c 1 192.168.10.20
```

Inspect the neighbor table:

```bash
ip neigh show
```

You should see an entry associating `192.168.10.20` with the MAC address of Ubuntu-02.

Capture ARP traffic:

```bash
sudo tcpdump -i enp0s3 -n -e arp
```

In Wireshark, use the display filter:

```text
arp
```

**Expected Result**

You should observe ARP request and reply messages.

An ARP request is generally broadcast on the local Layer 2 network. The device owning the requested IPv4 address normally responds with an ARP reply.

# 🗁 Lab: Understand ICMP and Ping

**Objective**

Use ICMP Echo Request and Echo Reply messages to test IP connectivity.

ICMP stands for Internet Control Message Protocol.

The `ping` utility commonly uses:

* ICMP Echo Request.
* ICMP Echo Reply.

From Ubuntu:

```bash
ping -c 4 192.168.10.1
```

Example output:

```text
64 bytes from 192.168.10.1: icmp_seq=1 ttl=64 time=0.8 ms
64 bytes from 192.168.10.1: icmp_seq=2 ttl=64 time=0.7 ms
64 bytes from 192.168.10.1: icmp_seq=3 ttl=64 time=0.9 ms
64 bytes from 192.168.10.1: icmp_seq=4 ttl=64 time=0.8 ms
```

Important fields:

| Field | Meaning |
| --- | --- |
| bytes | Size of the received ICMP payload |
| icmp_seq | Sequence number |
| ttl | Remaining IPv4 Time to Live |
| time | Round-trip time |

Check packet loss:

```bash
ping -c 10 192.168.10.1
```

Ping helps identify basic connectivity problems, but a failed ping does not always mean the destination is unavailable. Firewalls and network policies can block or deprioritize ICMP.

# 🗁 Lab: Understand the Routing Table

**Objective**

Inspect how a Linux system determines where to forward packets.

Display the routing table:

```bash
ip route
```

Example:

```text
default via 192.168.10.1 dev enp0s3
192.168.10.0/24 dev enp0s3 proto kernel scope link src 192.168.10.10
```

The routes represent:

| Route | Meaning |
| --- | --- |
| `default via 192.168.10.1` | Send unmatched destinations to the gateway |
| `192.168.10.0/24 dev enp0s3` | Local subnet connected through the interface |

Ask Linux which route it will use for a destination:

```bash
ip route get 8.8.8.8
```

The output identifies the selected interface and next hop.

On MikroTik:

```routeros
/ip route print
```

The routing table contains connected, static, dynamic, and other routes depending on the router configuration.

# 🗁 Lab: Configure Two Networks Through a Router

**Objective**

Connect two different IPv4 networks through a MikroTik router.

**Topology**

```text
 Ubuntu-01                              Ubuntu-02
192.168.10.10                          192.168.20.10
     |                                      |
     |                                      |
     +---- ether2      ether3 --------------+
              MikroTik CHR
```

**IP addressing plan**

| Device | Interface | IP Address |
| --- | --- | --- |
| CHR | ether2 | 192.168.10.1/24 |
| CHR | ether3 | 192.168.20.1/24 |
| Ubuntu-01 | Ethernet | 192.168.10.10/24 |
| Ubuntu-02 | Ethernet | 192.168.20.10/24 |

Configure CHR:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
/ip address add address=192.168.20.1/24 interface=ether3
```

Configure Ubuntu-01:

```bash
sudo ip address add 192.168.10.10/24 dev enp0s3
sudo ip route add default via 192.168.10.1
```

Configure Ubuntu-02:

```bash
sudo ip address add 192.168.20.10/24 dev enp0s3
sudo ip route add default via 192.168.20.1
```

Test the gateway from Ubuntu-01:

```bash
ping -c 4 192.168.10.1
```

Test the remote client:

```bash
ping -c 4 192.168.20.10
```

MikroTik automatically creates connected routes for the directly attached networks when their IP addresses are configured on active interfaces.

**Expected Result**

Ubuntu-01 should communicate with Ubuntu-02 through the MikroTik router.

No additional static routes are necessary for this simple topology because both networks are directly connected to the same router.

# 🗁 Lab: Understand Traceroute

**Objective**

Discover the Layer 3 path packets take toward a destination.

Traceroute works by sending probe packets with increasing TTL values.

Each router that decrements an IPv4 packet's TTL to zero may return an ICMP Time Exceeded message.

On Ubuntu:

```bash
traceroute 8.8.8.8
```

Alternative:

```bash
tracepath 8.8.8.8
```

On MikroTik:

```routeros
/tool traceroute 8.8.8.8
```

Traceroute helps identify:

* Intermediate routers.
* Routing path changes.
* Potential forwarding problems.
* Where responses stop arriving.

Some routers do not respond to traceroute probes. Asterisks do not necessarily indicate that the destination is unreachable.

# 🗁 Lab: Capture ICMP Traffic

**Objective**

Capture and inspect ICMP packets using tcpdump and Wireshark.

Start packet capture on Ubuntu:

```bash
sudo tcpdump -i enp0s3 -nn icmp
```

In another terminal, generate traffic:

```bash
ping -c 4 192.168.10.1
```

You should see ICMP Echo Request and Echo Reply packets.

Save the capture:

```bash
sudo tcpdump -i enp0s3 -nn icmp -w icmp-lab.pcap
```

Open the capture in Wireshark:

```bash
wireshark icmp-lab.pcap
```

Use this Wireshark display filter:

```text
icmp
```

Inspect:

* Source IP address.
* Destination IP address.
* ICMP message type.
* ICMP sequence number.
* TTL.
* Packet timing.

# 🗁 Lab: Troubleshoot Connectivity Problems

**Objective**

Identify common causes of failed network communication.

Use the following troubleshooting scenarios.

**Scenario: Incorrect IP Address**

Ubuntu-01:

```text
192.168.10.10/24
```

Ubuntu-02:

```text
192.168.20.20/24
```

Both clients are connected to the same Layer 2 segment, but neither has a router configured between the two subnets.

Test:

```bash
ping -c 4 192.168.20.20
```

Investigate the routing table:

```bash
ip route
```

Explain why the destination is not reachable through the current configuration.

**Scenario: Incorrect Default Gateway**

Configure an incorrect gateway:

```bash
sudo ip route replace default via 192.168.10.254
```

Test external connectivity and inspect the route:

```bash
ip route
```

Restore the correct gateway:

```bash
sudo ip route replace default via 192.168.10.1
```

**Scenario: Interface Down**

Inspect interface status:

```bash
ip -br link
```

Bring the interface up:

```bash
sudo ip link set enp0s3 up
```

**Scenario: Incorrect Virtual Network**

Check whether both virtual machines are connected to the same virtual network.

For VirtualBox, compare the Internal Network names.

For GNS3 and EVE-NG, inspect the links and network segments in the topology.

**Scenario: Firewall Blocking ICMP**

Inspect Ubuntu firewall status:

```bash
sudo ufw status verbose
```

Inspect MikroTik firewall filter rules:

```routeros
/ip firewall filter print
```

Determine whether an input or forwarding rule is dropping the relevant traffic.

Do not disable the entire firewall as the default troubleshooting method. Identify the affected traffic and correct the relevant rule.

# 🗁 Useful Connectivity Commands

| Command | Purpose |
| --- | --- |
| `ip address` | Display IP addresses |
| `ip link` | Display network interfaces |
| `ip route` | Display routing table |
| `ip neigh` | Display ARP and neighbor entries |
| `ping` | Test ICMP connectivity |
| `traceroute` | Discover network paths |
| `tracepath` | Inspect network paths and path MTU information |
| `ss -tulnp` | Display listening sockets |
| `tcpdump` | Capture packets |
| `ethtool` | Inspect supported Ethernet interface details |
| `dig` | Test DNS queries |
| `mtr` | Combine path discovery and repeated probes |

# 🗁 Best Practices

* Start with a simple topology.
* Verify physical or virtual links before troubleshooting IP configuration.
* Check interface status before testing connectivity.
* Verify IP addresses and subnet masks.
* Inspect the routing table.
* Check ARP resolution when local communication fails.
* Use ping to test reachability.
* Use traceroute to investigate forwarding paths.
* Capture packets when the cause is unclear.
* Document every configuration change.
* Change one variable at a time during troubleshooting.

# 🗁 Lab Exercises

Complete the following practical exercises independently.

**Exercise: Same-Subnet Communication**

* Create two Ubuntu virtual machines.
* Connect them to the same virtual network.
* Assign addresses from `192.168.10.0/24`.
* Test bidirectional communication.
* Inspect ARP entries.

**Exercise: Router Gateway**

* Create one MikroTik CHR.
* Connect one Ubuntu client.
* Configure the router's LAN address.
* Configure the client's default gateway.
* Test gateway connectivity.

**Exercise: Inter-Network Routing**

* Create two LAN segments.
* Connect both segments to one MikroTik router.
* Configure separate IP subnets.
* Configure client default gateways.
* Test communication between the networks.

**Exercise: Packet Analysis**

* Capture ARP traffic.
* Capture ICMP traffic.
* Inspect packet headers in Wireshark.
* Identify Echo Request and Echo Reply messages.

**Exercise: Connectivity Troubleshooting**

* Introduce an incorrect IP address.
* Introduce an incorrect default gateway.
* Disable a virtual interface.
* Inspect the resulting behavior.
* Restore the correct configuration.

# 🗁 Summary

Basic connectivity is the foundation of all networking operations.

After completing these labs, you should understand:

* Ethernet connectivity.
* IPv4 address configuration.
* Subnet-based communication.
* Default gateways.
* ARP resolution.
* ICMP and ping.
* Routing tables.
* Traceroute.
* Packet capture.
* Basic network troubleshooting.

These skills will help you understand more advanced networking concepts and diagnose problems systematically.
