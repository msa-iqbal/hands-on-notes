# Connected Routes

A connected route is a routing table entry automatically created by a router or host when an IP address and subnet prefix are configured on an operational network interface.

Connected routes allow a device to communicate with destinations located directly on its attached networks without requiring a manually configured next hop.

They are among the most fundamental route types in IP networking.

For example, when a Linux interface receives the address `192.168.1.10/24`, the system normally installs a connected route for `192.168.1.0/24` through that interface.

The device can then determine that destinations within this subnet are directly reachable.

Connected routes are important for:

* Understanding how routers build routing tables.
* Configuring IP addresses on network interfaces.
* Communicating with devices on local networks.
* Resolving next-hop addresses using ARP or NDP.
* Understanding static and dynamic route resolution.
* Troubleshooting routing and interface connectivity.

# 🗁 What Is a Connected Route?

A connected route is a route automatically installed into a routing table for a network directly attached to an IP-enabled interface.

When an interface receives an IP address and prefix, the operating system determines the corresponding network prefix and normally creates a route for that network.

### 🖆 Example

Suppose a router interface has this configuration:

```text
Interface: eth0
IP Address: 192.168.1.10/24
```

The subnet is:

```text
Network: 192.168.1.0/24
```

The routing table receives a connected route similar to:

```text
192.168.1.0/24 dev eth0 proto kernel scope link
```

This indicates that the network `192.168.1.0/24` is directly reachable through `eth0`.

The router does not need another router as a next hop to reach hosts in this subnet.

### 🖆 Characteristics of Connected Routes

* Created automatically from interface addressing.
* Represent directly attached IP networks.
* Associated with an outgoing interface.
* Normally have no separate gateway next-hop address.
* Depend on the interface and address configuration.
* Usually disappear or become inactive when the associated interface or address is no longer usable.
* Take part in normal route selection through Longest Prefix Match.

# 🗁 How Connected Routes Are Created

Connected routes are normally created when an IP address and prefix are assigned to an interface.

Consider:

```text
Interface: eth0
Address:   192.168.10.1/24
```

The system calculates:

```text
IP Address:     192.168.10.1
Subnet Mask:    255.255.255.0
Network Prefix: 192.168.10.0/24
```

The connected route is then installed:

```text
192.168.10.0/24 dev eth0
```

### 🖆 Network Calculation

Given:

```text
IP Address: 192.168.10.1
Prefix:     /24
```

The subnet mask is:

```text
255.255.255.0
```

The network address is:

```text
192.168.10.0
```

Therefore, the connected route represents:

```text
192.168.10.0/24
```

### 🖆 Another Example

Interface configuration:

```text
Interface: eth1
IP Address: 10.0.0.1/30
```

Network information:

| Property | Value |
| --- | --- |
| IP address | `10.0.0.1` |
| Subnet mask | `255.255.255.252` |
| Network address | `10.0.0.0` |
| Prefix | `/30` |
| Connected route | `10.0.0.0/30` |

The router installs a route for the directly attached `/30` network.

# 🗁 How Connected Routing Works

Consider a router connected to a local network:

```text
                Router
              192.168.1.1
                   |
                   |
             192.168.1.0/24
                   |
          +--------+--------+
          |                 |
        PC A              PC B
     192.168.1.10     192.168.1.20
```

The router interface has:

```text
192.168.1.1/24
```

Its routing table contains:

```text
192.168.1.0/24 dev eth0
```

Suppose the router receives a packet destined for:

```text
192.168.1.20
```

### 🖆 Packet Forwarding Process

* The router receives the packet.
* It examines the destination IP address.
* It searches its routing table.
* The destination matches `192.168.1.0/24`.
* The connected route is selected.
* The router identifies `eth0` as the outgoing interface.
* It resolves the destination's MAC address if necessary.
* It forwards the packet through `eth0`.

The router does not forward the packet to another router because the destination belongs to a directly connected network.

### 🖆 Communication With a Remote Network

Now suppose the destination is:

```text
8.8.8.8
```

This address does not belong to `192.168.1.0/24`.

The router must find another matching route, such as a static route or default route.

Connected routes only describe networks directly attached to the device. They do not automatically provide reachability to every remote network.

# 🗁 Connected Routes and Local Routes

Connected routes and local routes are related, but they represent different types of reachability.

When an IP address is configured on a Linux interface, the system commonly creates:

* A connected network route.
* A local host route for the assigned IP address.

### 🖆 Connected Route

A connected route represents the subnet attached to an interface.

Example:

```text
192.168.1.0/24 dev eth0
```

It allows the system to reach other addresses in that directly attached subnet.

### 🖆 Local Route

A local route represents an IP address assigned to the device itself.

Example:

```text
local 192.168.1.10 dev eth0
```

It identifies `192.168.1.10` as a local destination.

Packets destined for that address are delivered to the local system rather than forwarded to another host.

### 🖆 Comparison

| Connected Route | Local Route |
| --- | --- |
| Represents an attached network | Represents an address assigned to the device |
| Usually has a network prefix | Commonly uses `/32` for IPv4 or `/128` for IPv6 |
| Used to reach other hosts on the subnet | Used to identify traffic destined for the local device |
| Associated with an interface | Associated with the interface holding the address |

### 🖆 Linux Example

Suppose:

```text
Interface: eth0
Address:   192.168.1.10/24
```

Linux may display:

```text
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10

local 192.168.1.10 dev eth0 proto kernel scope host
```

The first route represents the connected network.

The second route represents the device's own IP address.

# 🗁 Connected Routes in the Routing Table

Connected routes are installed in the routing table alongside static, dynamic, and default routes.

Example:

| Destination | Next Hop | Interface | Route Type |
| --- | --- | --- | --- |
| `192.168.1.0/24` | Direct | `eth0` | Connected |
| `10.0.0.0/30` | Direct | `eth1` | Connected |
| `192.168.20.0/24` | `10.0.0.2` | `eth1` | Static |
| `0.0.0.0/0` | `10.0.0.2` | `eth1` | Default |

### 🖆 Connected Route Selection

Connected routes participate in Longest Prefix Match just like other routes.

For example:

```text
192.168.0.0/16 via 10.0.0.2
192.168.1.0/24 dev eth0
0.0.0.0/0 via 10.0.0.2
```

For destination:

```text
192.168.1.50
```

The `/24` connected route is selected because it is more specific than the `/16` route and the default route.

### 🖆 Route Preference

A connected route is not selected simply because it is connected.

Route selection first considers the most-specific matching prefix. When competing routes have the same prefix, the routing system applies its route preference, administrative distance, metric, and other platform-specific rules.

Connected routes are commonly given a high preference by router operating systems.

# 🗁 Directly Connected Networks

A directly connected network is an IP network attached to a device through one of its interfaces.

A router can have multiple directly connected networks.

### 🖆 Example Topology

```text
                  Internet
                     |
                  Router
               /     |     \
              /      |      \
           LAN A    LAN B    LAN C
              |      |        |
          eth0      eth1     eth2
```

Router interfaces:

| Interface | IP Address | Connected Network |
| --- | --- | --- |
| `eth0` | `192.168.10.1/24` | `192.168.10.0/24` |
| `eth1` | `192.168.20.1/24` | `192.168.20.0/24` |
| `eth2` | `10.0.0.1/30` | `10.0.0.0/30` |

The router automatically installs routes for these networks when the interfaces and addresses are operational.

### 🖆 Why Directly Connected Networks Matter

They allow routers to:

* Communicate with devices on attached networks.
* Resolve local next-hop addresses.
* Forward packets between directly attached networks.
* Resolve the next hop of static routes.
* Establish routing protocol adjacencies on connected links.

# 🗁 ARP and NDP in Connected Routing

A router often needs a destination MAC address before it can transmit an IPv4 or IPv6 packet over an Ethernet network.

The address-resolution mechanism depends on the IP version.

### 🖆 ARP for IPv4

Address Resolution Protocol maps IPv4 addresses to MAC addresses on a local link.

Suppose a router needs to forward a packet to:

```text
192.168.1.20
```

It checks its ARP neighbor cache.

If the mapping is unavailable, the router sends an ARP request to discover the destination MAC address.

The resulting mapping is used to construct the Ethernet frame.

### 🖆 NDP for IPv6

Neighbor Discovery Protocol performs IPv6 neighbor resolution using ICMPv6.

It uses Neighbor Solicitation and Neighbor Advertisement messages to discover and maintain link-layer address information.

### 🖆 Important Difference

| IPv4 | IPv6 |
| --- | --- |
| Uses ARP | Uses NDP |
| ARP requests use Ethernet broadcast | NDP uses multicast |
| ARP resolves IPv4-to-MAC mappings | NDP resolves IPv6-to-link-layer mappings |

A connected route identifies the network and outgoing interface. ARP or NDP resolves the link-layer address required to transmit to a neighbor.

# 🗁 Connected Routes vs Static Routes

Connected routes and static routes are both commonly found in routing tables, but they are created differently.

### 🖆 Connected Routes

Connected routes are automatically created from interface addressing.

Example:

```text
192.168.1.0/24 dev eth0
```

The network is directly attached to the device.

### 🖆 Static Routes

Static routes are manually configured by an administrator.

Example:

```text
192.168.20.0/24 via 192.168.1.2
```

The destination network is remote and can be reached through the next-hop router.

### 🖆 Comparison

| Feature | Connected Route | Static Route |
| --- | --- | --- |
| Creation | Automatic | Manual |
| Destination | Directly attached network | Usually a remote network |
| Next hop | Directly attached interface | Configured next hop or interface |
| Configuration | IP address and prefix | Route configuration |
| Dependency | Interface and address state | Route configuration and next-hop resolution |
| Typical use | Local network reachability | Remote network reachability |

A static route may also specify an outgoing interface or an on-link next hop, depending on the routing platform.

# 🗁 Connected Routes in Linux

Linux automatically manages connected routes through its networking stack.

The `ip` command from the `iproute2` package is used to inspect and manage network interfaces and routes.

### 🖆 Display Network Interfaces

```bash
ip link show
```

Display assigned IP addresses:

```bash
ip addr show
```

### 🖆 Display Connected Routes

```bash
ip route show
```

Example output:

```text
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
10.0.0.0/30 dev eth1 proto kernel scope link src 10.0.0.1
default via 10.0.0.2 dev eth1
```

The first two entries represent directly connected networks.

The final entry represents a default route.

### 🖆 Display Only Connected Routes

Linux does not have one universal route type named `connected` in its routing table output. Connected routes can commonly be identified by their directly attached interface and `scope link`.

Example:

```bash
ip route show scope link
```

### 🖆 Display Local Routes

```bash
ip route show table local
```

This displays local and broadcast routes associated with the device's configured addresses.

### 🖆 Add an IP Address to an Interface

```bash
sudo ip addr add 192.168.1.10/24 dev eth0
```

The kernel normally creates the associated connected and local routes automatically when appropriate.

Verify:

```bash
ip route show
```

### 🖆 Remove an IP Address

```bash
sudo ip addr del 192.168.1.10/24 dev eth0
```

The associated automatically generated routes are normally removed when no remaining address requires them.

### 🖆 Inspect the Selected Route

```bash
ip route get 192.168.1.20
```

This displays how Linux would route traffic toward the destination.

### 🖆 Inspect Neighbor Information

IPv4 and IPv6 neighbor entries:

```bash
ip neigh show
```

IPv6-specific neighbor entries:

```bash
ip -6 neigh show
```

# 🗁 Connected Routes in MikroTik RouterOS

MikroTik RouterOS automatically creates connected routes when IP addresses are assigned to operational interfaces.

### 🖆 Configure an IPv4 Address

```routeros
/ip address add address=192.168.1.1/24 interface=ether2
```

This creates a connected route for:

```text
192.168.1.0/24
```

### 🖆 Display IPv4 Routes

```routeros
/ip route print
```

Display detailed route information:

```routeros
/ip route print detail
```

### 🖆 Identify Connected Routes

RouterOS route output uses flags to identify route properties.

A dynamically installed connected route commonly has the `DAc` flags:

| Flag | Meaning |
| --- | --- |
| `D` | Dynamic |
| `A` | Active |
| `c` | Connected |

The exact flags shown depend on the route and RouterOS version.

### 🖆 Configure Another Connected Network

```routeros
/ip address add address=10.0.0.1/30 interface=ether3
```

The router now has another directly connected network:

```text
10.0.0.0/30
```

### 🖆 Display Connected Routes

```routeros
/ip route print where dst-address=192.168.1.0/24
```

### 🖆 IPv6 Connected Routes

Assign an IPv6 address:

```routeros
/ipv6 address add address=2001:db8:1::1/64 interface=ether2
```

Display IPv6 routes:

```routeros
/ipv6 route print
```

RouterOS automatically creates the corresponding connected IPv6 route when the address and interface are operational.

# 🗁 Multiple Connected Networks

A router can have multiple interfaces, each connected to a different network.

Example:

```text
                 Router
              /     |     \
             /      |      \
          LAN A    LAN B    LAN C
```

Configuration:

| Interface | IP Address | Network |
| --- | --- | --- |
| `eth0` | `192.168.10.1/24` | `192.168.10.0/24` |
| `eth1` | `192.168.20.1/24` | `192.168.20.0/24` |
| `eth2` | `192.168.30.1/24` | `192.168.30.0/24` |

Routing table:

```text
192.168.10.0/24 dev eth0
192.168.20.0/24 dev eth1
192.168.30.0/24 dev eth2
```

The router can forward packets between these networks if IP forwarding is enabled and firewall policies permit the traffic.

### 🖆 Linux IP Forwarding

Check IPv4 forwarding:

```bash
sysctl net.ipv4.ip_forward
```

Enable temporarily:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Check IPv6 forwarding:

```bash
sysctl net.ipv6.conf.all.forwarding
```

Enable temporarily:

```bash
sudo sysctl -w net.ipv6.conf.all.forwarding=1
```

These settings enable forwarding at the kernel level. Firewall policies and return routing must still allow communication.

# 🗁 Interface Failure and Connected Routes

Connected routes depend on the associated interface and address configuration.

If an interface goes down, its connected routes may become inactive or disappear from the usable routing table.

### 🖆 Example

Initial state:

```text
192.168.1.0/24 dev eth0
```

Interface status:

```text
eth0: UP
```

If the interface goes down:

```bash
sudo ip link set eth0 down
```

The connected route may no longer be usable.

Check:

```bash
ip route show
```

Bring the interface back up:

```bash
sudo ip link set eth0 up
```

### 🖆 Why This Matters

Routing protocols and static routes may depend on connected routes for next-hop reachability.

If a directly connected network becomes unavailable:

* Neighbor resolution may fail.
* Static routes using that network may become unusable.
* Dynamic routing adjacencies may go down.
* Remote routes may be withdrawn.
* Traffic may be redirected through an alternative path if one exists.

The exact behavior depends on the operating system, interface state, address configuration, and routing protocol.

# 🗁 Best Practices

### 🖆 Use Correct IP Prefixes

Assign the correct subnet prefix to each interface.

An incorrect prefix can create an incorrect connected route and cause unexpected forwarding behavior.

### 🖆 Avoid Overlapping Connected Networks

Avoid assigning overlapping network prefixes to interfaces unless the design specifically requires such behavior.

Overlapping subnets can create ambiguous routing and neighbor-resolution problems.

### 🖆 Verify Interface State

A configured IP address does not guarantee that the interface is operational.

Check both the interface state and the installed routes.

### 🖆 Understand Local and Connected Routes

Do not confuse the route for an attached network with the local route for the device's own address.

### 🖆 Verify Neighbor Resolution

A connected route can exist even when a particular host is unreachable.

Check ARP or NDP neighbor information when investigating local connectivity problems.

### 🖆 Use Consistent Network Addressing

Maintain a clear IP addressing plan across LANs, WAN links, VLANs, and point-to-point connections.

### 🖆 Document Interface Assignments

Record interface names, IP addresses, subnet prefixes, VLANs, connected networks, and intended routing behavior.

# 🗁 Common Problems and Troubleshooting

### 🖆 Connected Route Is Missing

**Possible causes:**

* IP address not configured.
* Incorrect subnet prefix.
* Interface administratively down.
* Interface operationally down.
* Address configuration failure.
* Network configuration not applied.

Check:

```bash
ip addr show
ip link show
ip route show
```

### 🖆 Incorrect Connected Network

Suppose the intended address is:

```text
192.168.10.1/24
```

But the interface is configured with:

```text
192.168.10.1/16
```

The connected route becomes:

```text
192.168.0.0/16
```

This may cause traffic to be treated as directly connected when it should be routed elsewhere.

Verify the address and prefix:

```bash
ip addr show dev eth0
```

### 🖆 Destination Host Is Unreachable

A connected route may exist while the destination host is offline or unreachable.

Check:

```bash
ip route get 192.168.1.20
ip neigh show
```

Test:

```bash
ping -c 4 192.168.1.20
```

### 🖆 ARP Resolution Failure

For IPv4, inspect neighbor information:

```bash
ip neigh show
```

Capture ARP traffic:

```bash
sudo tcpdump -ni eth0 arp
```

Possible causes include:

* Incorrect IP address.
* VLAN mismatch.
* Physical connectivity problem.
* Destination host offline.
* Duplicate IP address.
* Firewall or Layer 2 filtering.

### 🖆 IPv6 Neighbor Resolution Failure

Inspect:

```bash
ip -6 neigh show
```

Capture NDP messages:

```bash
sudo tcpdump -ni eth0 icmp6
```

Check interface addressing and ensure necessary ICMPv6 neighbor-discovery traffic is permitted.

### 🖆 Connected Route Exists but Inter-Network Communication Fails

Connected routes alone do not guarantee that traffic can pass between different networks.

Check:

* IP forwarding.
* Firewall rules.
* Source and destination addressing.
* Return routes.
* VLAN configuration.
* Interface state.

### 🖆 MikroTik Connected Route Is Inactive

Check:

```routeros
/ip address print
/interface print
/ip route print detail
```

Verify that the interface is operational and that the IP address is assigned to the correct interface.

# 🗁 Practical Labs

### 🖆 Lab: Create a Connected Route in Linux

Assign an address:

```bash
sudo ip addr add 192.168.10.1/24 dev eth0
```

Inspect:

```bash
ip addr show dev eth0
ip route show
```

Identify the automatically created connected route.

### 🖆 Lab: Create Multiple Connected Routes

Configure:

```bash
sudo ip addr add 192.168.10.1/24 dev eth0
sudo ip addr add 192.168.20.1/24 dev eth1
sudo ip addr add 10.0.0.1/30 dev eth2
```

Inspect:

```bash
ip route show
```

Identify all directly connected networks.

### 🖆 Lab: Compare Connected and Local Routes

Configure:

```bash
sudo ip addr add 192.168.10.1/24 dev eth0
```

Display the main routing table:

```bash
ip route show
```

Display the local routing table:

```bash
ip route show table local
```

Identify the network route and the host route for the interface address.

### 🖆 Lab: Observe Interface Failure

Check the initial routing table:

```bash
ip route show
```

Disable the interface:

```bash
sudo ip link set eth0 down
```

Inspect the routing table again:

```bash
ip route show
```

Restore the interface:

```bash
sudo ip link set eth0 up
```

Observe how the connected route changes with interface state.

### 🖆 Lab: Configure Connected Networks in MikroTik

Assign addresses:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
/ip address add address=192.168.20.1/24 interface=ether3
/ip address add address=10.0.0.1/30 interface=ether4
```

Inspect:

```routeros
/ip address print
/ip route print
```

Identify the connected routes and their associated interfaces.

### 🖆 Lab: Test Directly Connected Communication

Build this topology:

```text
PC A -------- Router -------- PC B
```

Use separate IP subnets for the two router-facing interfaces.

Objectives:

* Configure interface addresses.
* Verify connected routes.
* Enable IP forwarding.
* Test communication between the two LANs.
* Inspect ARP entries.
* Disable one interface and observe routing changes.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

**What is a connected route?**

A connected route is a routing table entry automatically created for a network directly attached to an IP-enabled interface.

**How is a connected route created?**

It is normally created when an IP address and subnet prefix are configured on an operational interface.

**Does a connected route require a next-hop router?**

No. The destination network is directly attached, so the route identifies the outgoing interface rather than requiring a separate router as its next hop.

**What is a directly connected network?**

A network attached directly to a router or host through one of its network interfaces.

### 🖆 Intermediate Questions

**What is the difference between a connected route and a local route?**

A connected route represents an attached network. A local route represents an IP address assigned to the device itself.

**What happens when a connected interface goes down?**

The associated connected route may become inactive or disappear from the usable routing table, depending on the platform and interface state.

**Can a router have multiple connected routes?**

Yes. A router can have multiple interfaces connected to different IP networks.

**Do connected routes have higher priority than static routes?**

Not universally. Route selection first uses Longest Prefix Match. If candidate routes have the same prefix, platform-specific route preferences and metrics determine the selected route.

**What is the relationship between connected routes and ARP?**

The connected route identifies the directly attached network and outgoing interface. ARP resolves an IPv4 neighbor's MAC address when necessary.

### 🖆 Advanced Questions

**Can a router forward traffic between two connected networks without static routes?**

Yes. If both networks are directly connected, IP forwarding is enabled, and firewall policies permit the traffic, the router can forward packets between them without additional routes.

**Why might a connected route exist while a destination host is unreachable?**

The route only establishes that the destination belongs to an attached network. The host may be offline, incorrectly addressed, blocked by Layer 2 controls, or unreachable because of ARP or NDP failure.

**What is the purpose of the Linux local routing table?**

It contains routes for local addresses and other local delivery destinations. It helps the kernel distinguish traffic destined for the local device from traffic that should be forwarded.

**Can a static route use a connected network to resolve its next hop?**

Yes. A next-hop address must generally be reachable through an appropriate route. A connected route commonly provides that reachability.

**Why is an incorrect subnet mask dangerous?**

It can create an incorrect connected route, causing a device to treat remote destinations as directly connected or local destinations as remote.

# 🗁 Summary

Connected routes are automatically created for IP networks directly attached to network interfaces.

Key concepts:

* Connected routes are generated from interface IP addressing and prefix configuration.
* They represent directly attached networks.
* They identify the outgoing interface for local-network destinations.
* They do not normally require a separate next-hop gateway.
* Connected routes participate in Longest Prefix Match.
* Local routes represent addresses assigned to the device itself.
* ARP resolves IPv4 neighbors, while NDP resolves IPv6 neighbors.
* Linux displays connected routes using the `ip route` command.
* MikroTik RouterOS automatically installs connected routes when appropriate.
* Interface state and address configuration affect connected-route availability.
* Connected routes provide the foundation for static and dynamic route resolution.

Understanding connected routes makes it easier to understand how routing tables are built and how routers forward packets between directly attached and remote networks.

# 🗁 References

* [RFC 1122 — Requirements for Internet Hosts: Communication Layers](https://www.rfc-editor.org/rfc/rfc1122)
* [RFC 1812 — Requirements for IP Version 4 Routers](https://www.rfc-editor.org/rfc/rfc1812)
* [RFC 8200 — Internet Protocol, Version 6 (IPv6) Specification](https://www.rfc-editor.org/rfc/rfc8200)
* [RFC 4861 — Neighbor Discovery for IP Version 6](https://www.rfc-editor.org/rfc/rfc4861)
* [Linux iproute2 Documentation](https://www.kernel.org/doc/html/latest/networking/)
* [MikroTik RouterOS IP Addressing](https://help.mikrotik.com/docs/)
