# Default Gateway

A **default gateway** is a network device that forwards traffic from a local network to destinations outside that network.

In most networks, the default gateway is a router or a Layer 3 switch.

When a device needs to communicate with another device, it first determines whether the destination belongs to its own local subnet or a different network.

* If the destination is on the local subnet, the device communicates directly with it.
* If the destination is outside the local subnet, the device forwards the traffic to its default gateway.

The default gateway provides a path to other networks, including the internet.

# 🗁 Understanding a Default Gateway

A default gateway is the next-hop router a host uses when it needs to send traffic to a destination for which it has no more specific route.

Consider the following network:

```text
                 Internet
                    |
                    |
              [ Router ]
             192.168.1.1
                    |
             Local Network
                    |
          ---------------------
          |                   |
      [PC 1]               [PC 2]
   192.168.1.10          192.168.1.20
   Gateway:              Gateway:
   192.168.1.1           192.168.1.1
```

Network configuration:

| Device | IP Address | Subnet Mask | Default Gateway |
| --- | --- | --- | --- |
| Router | `192.168.1.1` | `255.255.255.0` | Not required for this LAN interface |
| PC 1 | `192.168.1.10` | `255.255.255.0` | `192.168.1.1` |
| PC 2 | `192.168.1.20` | `255.255.255.0` | `192.168.1.1` |

Both computers use the router's LAN interface address as their default gateway.

The gateway must be reachable through the host's local network. It is normally configured with an IP address belonging to the same subnet as the host.

# 🗁 How a Default Gateway Works

A host uses its IP address and subnet mask to determine whether a destination is local or remote.

Consider this configuration:

```text
Host IP:          192.168.1.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1
```

The host belongs to this network:

```text
192.168.1.0/24
```

### 🖆 Communication Within the Same Network

Suppose the host wants to communicate with:

```text
Destination IP: 192.168.1.20
```

Both addresses belong to `192.168.1.0/24`.

Therefore, the host communicates directly with the destination.

The process:

* The host determines that the destination is on the local subnet.
* It uses ARP to discover the destination's MAC address if that address is not already cached.
* It sends the Ethernet frame directly to the destination device.

The default gateway is not involved in this communication.

```text
[PC 1]
192.168.1.10
     |
     | Direct communication
     |
[PC 2]
192.168.1.20
```

### 🖆 Communication With Another Network

Now suppose the host wants to communicate with:

```text
Destination IP: 8.8.8.8
```

The destination does not belong to `192.168.1.0/24`.

The host therefore sends the packet to its default gateway.

The process:

* The host determines that the destination is outside its local subnet.
* It checks its routing table for the appropriate route.
* It selects the default route if no more specific route matches.
* It resolves the gateway's MAC address using ARP if necessary.
* It sends an Ethernet frame to the gateway's MAC address.
* The router receives the frame, examines the IP packet, and forwards it according to its routing table.

```text
[PC]
192.168.1.10
     |
     | Ethernet frame addressed to gateway MAC
     |
[Router]
192.168.1.1
     |
     | Routing and forwarding
     |
[Internet]
8.8.8.8
```

The destination IP address remains `8.8.8.8` during ordinary routing. The router changes the Layer 2 encapsulation when forwarding the packet onto another network.

# 🗁 Default Gateway and Routing

A default gateway is closely related to the default route in a routing table.

A default route tells a device where to forward packets when no more specific route matches the destination.

### 🖆 Default Route in IPv4

The IPv4 default route is represented by:

```text
0.0.0.0/0
```

It matches every IPv4 destination address.

Example:

```text
Destination     Gateway         Interface
192.168.1.0/24  Directly connected  eth0
10.0.0.0/8      192.168.1.254      eth0
0.0.0.0/0       192.168.1.1        eth0
```

If a packet does not match a more specific route, the device uses:

```text
0.0.0.0/0 via 192.168.1.1
```

### 🖆 Default Route in IPv6

The IPv6 default route is represented by:

```text
::/0
```

Example:

```text
::/0 via fe80::1 dev eth0
```

Here:

* `::/0` represents the IPv6 default route.
* `fe80::1` is the next-hop link-local address.
* `eth0` is the outgoing network interface.

### 🖆 Default Gateway vs Default Route

| Concept | Description |
| --- | --- |
| Default Gateway | The next-hop router used to reach destinations outside the local network |
| Default Route | A routing-table entry used when no more specific route matches |
| Next Hop | The next router or destination to which a packet is forwarded |
| Routing Table | Contains routes that determine where packets should be sent |

A default gateway is commonly configured through a default route, but the two terms describe different aspects of network forwarding.

# 🗁 Default Gateway Configuration

A default gateway can be configured manually or automatically.

### 🖆 Manual Configuration

An administrator can manually configure the host's IP address, subnet mask, and default gateway.

Example:

```text
IP Address:       192.168.1.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.1.1
DNS Server:       8.8.8.8
```

### 🖆 Automatic Configuration Using DHCP

In IPv4 networks, DHCP can automatically provide a default gateway to clients.

The DHCP server communicates this information through:

```text
DHCP Option 3: Router
```

Example DHCP configuration:

```text
Subnet:          192.168.1.0/24
Gateway:         192.168.1.1
Address Pool:    192.168.1.100 - 192.168.1.200
DNS Server:      8.8.8.8
```

When a client receives its DHCP configuration, it can automatically install a default route using the supplied gateway.

# 🗁 Finding the Default Gateway

Different operating systems provide commands to inspect the configured default gateway.

### 🖆 Linux (Ubuntu)

Display the routing table:

```bash
ip route
```

Example output:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
```

The default gateway is:

```text
192.168.1.1
```

Display IPv6 routes:

```bash
ip -6 route
```

Display the routing table with more details:

```bash
ip route show
```

### 🖆 Windows

Display network configuration:

```powershell
ipconfig
```

Look for:

```text
Default Gateway . . . . . . . . . : 192.168.1.1
```

Display the routing table:

```powershell
route print
```

### 🖆 macOS

Display the IPv4 default gateway:

```bash
route -n get default
```

Display the IPv6 default gateway:

```bash
route -n get -inet6 default
```

# 🗁 Default Gateway in IPv6

IPv6 uses a different mechanism for discovering and configuring the default router.

IPv6 hosts commonly learn their default router through **Router Advertisements (RA)**, which are part of Neighbor Discovery Protocol (NDP).

Router Advertisements are ICMPv6 messages sent by routers to advertise their presence and network configuration.

Important details:

* IPv6 routers advertise themselves through Router Advertisements.
* Hosts can learn the default router automatically.
* The router's link-local IPv6 address is commonly used as the next hop.
* Router Advertisements can also provide information about network prefixes and other configuration options.

Example:

```text
Default Route: ::/0
Next Hop:      fe80::1
Interface:     eth0
```

Unlike IPv4, IPv6 does not use ARP. It uses Neighbor Discovery Protocol, which operates through ICMPv6.

# 🗁 Common Problems

### 🖆 Incorrect Default Gateway

If a host has an incorrect gateway address, it may communicate with local devices but fail to reach external networks.

Example:

```text
Host IP:          192.168.1.10
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.2.1
```

If `192.168.2.1` is not reachable through another valid route, the host cannot use it as its gateway.

### 🖆 Gateway Is Unreachable

Possible causes:

* Router is powered off.
* Incorrect VLAN configuration.
* Network cable or Wi-Fi connection problem.
* Incorrect IP address or subnet mask.
* Gateway interface is down.
* ARP resolution is failing.

### 🖆 Missing Default Route

A host may have a valid IP address and communicate with devices on its local subnet but fail to reach external networks because it has no suitable default route.

Inspect the routing table:

```bash
ip route
```

If no default route exists, check the DHCP configuration or network interface configuration.

### 🖆 Gateway Responds but Internet Is Unavailable

If the gateway is reachable but internet access is unavailable, possible causes include:

* Router has no upstream connectivity.
* Router has an incorrect default route.
* NAT or firewall rules are misconfigured.
* DNS resolution is failing.
* ISP connectivity is unavailable.

Useful troubleshooting commands:

```bash
ping -c 4 192.168.1.1
ping -c 4 8.8.8.8
ping -c 4 google.com
```

Interpretation:

| Test | What It Checks |
| --- | --- |
| Ping gateway | Local connectivity to the router |
| Ping `8.8.8.8` | IP connectivity to an external destination |
| Ping `google.com` | IP connectivity and DNS resolution |

# 🗁 Best Practices

* Configure a valid gateway address that is reachable from the local subnet.
* Use DHCP to distribute gateway configuration consistently when appropriate.
* Maintain correct routing-table entries.
* Avoid configuring multiple default gateways without a deliberate routing or failover design.
* Verify gateway reachability during network troubleshooting.
* Use appropriate firewall rules to control traffic passing through the gateway.
* Monitor gateway availability and upstream connectivity.
* In IPv6 networks, ensure Router Advertisements and default-router discovery work correctly.

# 🗁 Summary

* A default gateway provides a path from a local network to other networks.
* It is commonly a router or Layer 3 switch.
* Hosts communicate directly with devices on their local subnet.
* Hosts forward traffic for remote destinations through an appropriate router.
* IPv4 commonly uses `0.0.0.0/0` as the default route.
* IPv6 uses `::/0` as the default route.
* DHCP can provide the IPv4 default gateway through Option 3.
* IPv6 hosts commonly discover default routers through Router Advertisements.
* A missing or incorrect default route can prevent communication with external networks.

# 🗁 References

* [RFC 1122 – Requirements for Internet Hosts](https://www.rfc-editor.org/rfc/rfc1122)
* [RFC 2132 – DHCP Options and BOOTP Vendor Extensions](https://www.rfc-editor.org/rfc/rfc2132)
* [RFC 4861 – Neighbor Discovery for IP Version 6 (IPv6)](https://www.rfc-editor.org/rfc/rfc4861)
* [Linux `ip-route` Manual](https://man7.org/linux/man-pages/man8/ip-route.8.html)
* [Microsoft – ipconfig Documentation](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig)
