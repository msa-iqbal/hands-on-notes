# Firewall Fundamentals

A firewall is a network security system that monitors and controls incoming and outgoing network traffic according to predefined security rules.

Firewalls inspect network packets and determine whether traffic should be allowed, rejected, or dropped.

They are used to protect:

* Personal computers and home networks.
* Enterprise networks.
* Servers and data centers.
* Cloud infrastructure.
* Internet Service Provider (ISP) networks.
* Network devices and management interfaces.

A firewall can operate at different layers of the networking stack. Depending on its capabilities, it may inspect IP addresses, protocols, ports, connection states, application data, and other traffic attributes.

# 🗁 What Is a Firewall?

A firewall is a security mechanism that controls network traffic between devices, networks, or security zones.

It evaluates traffic against a set of rules called a **firewall rule set** or **firewall policy**.

For example, an organization may configure its firewall to:

* Allow employees to access the internet.
* Allow HTTPS traffic to public web servers.
* Block unsolicited inbound connections.
* Allow SSH access only from trusted administration networks.
* Prevent guest Wi-Fi devices from accessing internal servers.
* Allow DNS queries to approved DNS resolvers.

A firewall can be implemented as:

* Dedicated hardware.
* Software running on a server.
* A feature integrated into a router.
* A cloud-based network security service.
* A host-based security component.

**Basic firewall architecture**

```text
                Internet
                    |
                    |
              ┌─────▼─────┐
              │ Firewall  │
              │           │
              │ Rules:    │
              │ Allow     │
              │ Drop      │
              │ Reject    │
              └─────┬─────┘
                    |
             Internal Network
                    |
          ┌─────────┼─────────┐
          |         |         |
        PC-01     Server    Laptop
```

The firewall acts as a control point between networks and applies security policies to traffic passing through it.

# 🗁 Why Firewalls Are Important

Firewalls help enforce network security policies and reduce unauthorized access.

**Prevent unauthorized access**

Firewalls can block traffic from untrusted networks and restrict access to internal services.

**Protect servers**

Only required services and ports should be accessible to external clients.

For example, a public web server may allow:

| Protocol | Port | Purpose |
| --- | ---: | --- |
| TCP | 80 | HTTP |
| TCP | 443 | HTTPS |

SSH management access might be restricted to a trusted administration network instead of being exposed to the entire internet.

**Control internal communication**

Firewalls can restrict communication between different internal networks.

For example:

* Guest Wi-Fi cannot access corporate servers.
* IoT devices cannot access employee workstations.
* Application servers can access only required database services.

**Reduce attack exposure**

Blocking unnecessary ports and services reduces the number of network entry points available to attackers.

**Control outbound traffic**

Firewalls can also restrict outgoing connections.

For example, an organization may permit DNS queries only to approved resolvers or restrict servers from initiating unnecessary internet connections.

**Support network segmentation**

Firewalls enforce communication policies between network segments, helping contain security incidents and limit unauthorized lateral movement.

# 🗁 How a Firewall Works

A firewall receives network traffic and evaluates it against configured rules.

The exact processing behavior depends on the firewall implementation and its configuration.

A simplified process is:

* A packet arrives at the firewall.
* The firewall identifies relevant packet attributes.
* It evaluates applicable firewall rules.
* It determines the matching action.
* It allows, drops, or rejects the packet.
* It may record information about the event.

**Example**

Assume a firewall protects an internal network:

```text
Internal Network: 192.168.1.0/24
Firewall:         192.168.1.1
Web Server:       192.168.1.100
SSH Server:       192.168.1.200
```

Configured policy:

| Source | Destination | Protocol | Port | Action |
| --- | --- | --- | ---: | --- |
| Any | Web Server | TCP | 443 | Allow |
| Trusted Admin | SSH Server | TCP | 22 | Allow |
| Any | SSH Server | TCP | 22 | Drop |
| Any | Internal Network | Any | Any | Drop |

When an external client attempts to access the web server over HTTPS, the firewall checks the relevant rule and allows matching traffic.

When an untrusted client attempts SSH access, the firewall blocks the connection.

Rule order, chain processing, connection state, and platform-specific behavior determine which rule is evaluated and how the final decision is reached.

# 🗁 Firewall Components

A firewall commonly contains several logical components.

**Firewall Rules**

Rules define which traffic is permitted or denied.

A rule may match:

* Source IP address.
* Destination IP address.
* Source port.
* Destination port.
* Protocol.
* Incoming interface.
* Outgoing interface.
* Connection state.
* Packet or connection attributes.

**Firewall Policies**

A policy defines the overall security behavior of the firewall.

Examples:

* Default-deny inbound traffic.
* Permit established connections.
* Allow only approved management sources.
* Restrict communication between network zones.

**Rule Set**

A rule set is the collection of rules applied by the firewall.

Rules are evaluated according to the implementation's processing order and matching behavior.

**Connection Tracking**

Stateful firewalls maintain information about network connections.

This allows them to identify whether packets belong to new, established, related, or invalid connections.

**Logging and Monitoring**

Firewall logging records selected events such as:

* Blocked connections.
* Allowed administrative access.
* Invalid packets.
* Suspicious traffic.
* Policy violations.

Logging helps administrators investigate incidents and troubleshoot connectivity.

# 🗁 Types of Firewalls

Firewalls can be classified according to their architecture and inspection capabilities.

**Packet-Filtering Firewall**

A packet-filtering firewall evaluates individual packets using attributes such as:

* Source and destination IP addresses.
* Protocol.
* Source and destination ports.
* Network interfaces.

It does not necessarily maintain connection state.

**Stateful Firewall**

A stateful firewall tracks connection information and uses connection state when making filtering decisions.

For example, it can allow response packets belonging to an established outbound connection without requiring a separate inbound rule for every response packet.

**Application-Level Gateway**

An application-level gateway, sometimes called an application proxy, acts as an intermediary between clients and application servers.

It can inspect application-layer traffic and apply protocol-specific policies.

**Next-Generation Firewall (NGFW)**

An NGFW combines traditional firewall capabilities with additional security features.

Depending on the product, these may include:

* Deep Packet Inspection (DPI).
* Intrusion Prevention System (IPS).
* Application awareness.
* User identity integration.
* URL filtering.
* Threat intelligence.
* Malware detection.

**Host-Based Firewall**

A host-based firewall runs on an individual computer or server.

Examples include:

* Linux nftables.
* Windows Defender Firewall.
* macOS Application Firewall.

**Network-Based Firewall**

A network-based firewall protects communication between networks or security zones.

It is commonly deployed at network boundaries, data centers, and internal segmentation points.

# 🗁 Firewall Filtering Criteria

A firewall can inspect different packet and connection attributes to determine whether traffic matches a rule.

| Attribute | Description | Example |
| --- | --- | --- |
| Source IP | Originating IP address | `192.168.1.10` |
| Destination IP | Target IP address | `192.168.1.100` |
| Protocol | Network or transport protocol | TCP, UDP, ICMP |
| Source Port | Client-side transport port | `51500` |
| Destination Port | Service port | `443` |
| Interface | Network interface | `eth0` |
| Connection State | Tracked connection status | `established` |
| Direction | Traffic direction or processing path | Input, output, forward |

**Example: Allow HTTPS**

A firewall rule may allow TCP traffic to destination port 443:

```text
Protocol: TCP
Destination Port: 443
Action: Allow
```

**Example: Restrict SSH**

A firewall may permit SSH only from:

```text
Trusted Admin Network: 192.168.10.0/24
Destination Port:       22
Protocol:               TCP
Action:                 Allow
```

All other SSH traffic can be denied by a subsequent rule or the default policy.

# 🗁 Firewall Actions

A firewall rule specifies what happens when traffic matches its conditions.

Common actions include:

| Action | Description |
| --- | --- |
| Accept / Allow | Permits matching traffic |
| Drop | Silently discards matching traffic |
| Reject | Denies traffic and may return an error |
| Log | Records matching traffic |
| Jump | Transfers processing to another chain |
| Return | Returns processing to the calling chain |
| Continue | Continues evaluation where supported |

**Accept**

The firewall permits the matching traffic.

**Drop**

The firewall silently discards the packet.

The sender generally receives no immediate response from the firewall.

**Reject**

The firewall discards the packet and sends an appropriate rejection response when supported.

For example, it may send an ICMP unreachable message or a TCP reset.

**Log**

The firewall records information about matching traffic.

Logging may be combined with other actions, depending on the platform.

**Important distinction**

Dropping and rejecting traffic are not identical.

Drop silently discards packets, while reject explicitly informs the sender that communication was denied.

# 🗁 Firewall Placement

Firewalls can be deployed at different locations within a network.

**Network Perimeter Firewall**

A perimeter firewall controls traffic between an organization's internal network and external networks such as the internet.

```text
              Internet
                  |
                  |
            Perimeter Firewall
                  |
                  |
           Internal Network
```

**Internal Segmentation Firewall**

An internal firewall controls traffic between different internal networks.

```text
          Internal Network
                  |
          Segmentation Firewall
             /          \
            /            \
       User VLAN       Server VLAN
```

**Host-Based Firewall**

A host-based firewall protects an individual server or workstation.

```text
       Network
          |
          |
      Firewall
          |
          |
      Server
   ┌─────────────┐
   │ Host-Based  │
   │ Firewall    │
   └─────────────┘
```

**Cloud Firewall**

Cloud firewalls enforce network security policies for cloud workloads and virtual networks.

Examples include cloud network firewalls, security groups, and network access control mechanisms.

Their capabilities and processing behavior depend on the cloud provider.

# 🗁 Firewall Zones

A firewall zone represents a network or group of interfaces with a particular security classification.

Zones help organize firewall policies according to trust levels and business requirements.

Common zones include:

| Zone | Purpose | Typical Trust Level |
| --- | --- | --- |
| WAN | External network or internet | Untrusted |
| LAN | Internal user network | Relatively trusted |
| DMZ | Public-facing servers | Restricted |
| Guest | Guest devices | Untrusted |
| Management | Administrative interfaces | Highly restricted |
| IoT | Smart devices and sensors | Restricted |
| Server | Internal application infrastructure | Controlled |

**Example network architecture**

```text
                       Internet
                           |
                           |
                    ┌──────▼──────┐
                    │  Firewall   │
                    └──────┬──────┘
                           |
             ┌─────────────┼─────────────┐
             |             |             |
             ▼             ▼             ▼
            DMZ           LAN        Management
             |             |             |
         Web Server     User PCs     Admin Systems
```

The firewall applies different policies to traffic entering or leaving each zone.

For example:

* WAN to DMZ: Allow required public services.
* WAN to LAN: Deny unsolicited access.
* LAN to DMZ: Allow required application communication.
* Guest to LAN: Deny access.
* Management to Network Devices: Allow approved administration protocols.

Zone-based policies make complex network security rules easier to manage.

# 🗁 Firewall vs Router vs NAT

These technologies are often implemented on the same device, but they perform different functions.

| Feature | Router | Firewall | NAT |
| --- | --- | --- | --- |
| Primary purpose | Forward packets between networks | Control network traffic | Translate packet addresses |
| Main operation | Routing decisions | Traffic filtering | Address and sometimes port translation |
| Uses routing table | Yes | May use routing information | Usually operates with packet forwarding |
| Controls access | Not inherently | Yes | Not inherently |
| Changes IP addresses | Not normally | Not normally | Yes |
| Tracks connections | Not necessarily | Depends on firewall type | Commonly uses connection tracking for stateful NAT |

**Router**

A router forwards packets based on its routing table.

**Firewall**

A firewall evaluates traffic against security policies.

**NAT**

NAT modifies packet addressing information.

A single router may perform routing, firewall filtering, and NAT simultaneously.

# 🗁 Linux Firewall Fundamentals

Linux provides firewall functionality through the Netfilter framework.

Netfilter is part of the Linux kernel and provides hooks for processing network packets.

The main user-space tools include:

* `nftables`
* `iptables`
* `ufw`
* `firewalld`

**Netfilter**

Netfilter provides the kernel infrastructure for packet filtering, NAT, and other network packet-processing operations.

**nftables**

nftables is the modern Linux packet-filtering framework and is the recommended foundation for new Linux firewall configurations.

It supports:

* IPv4 and IPv6 filtering.
* TCP and UDP port matching.
* Stateful connection tracking.
* NAT.
* Sets and maps.
* Logging.
* Packet marking.
* Multiple tables and chains.

**iptables**

iptables is the older firewall management interface.

It remains available on many Linux systems, often through compatibility layers, but nftables is preferred for new configurations.

**UFW**

Uncomplicated Firewall (UFW) provides a simpler interface for managing firewall policies on Ubuntu.

Example commands:

Check firewall status:

```bash
sudo ufw status verbose
```

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

Deny a port:

```bash
sudo ufw deny 23/tcp
```

Enable UFW:

```bash
sudo ufw enable
```

**Inspect nftables rules**

```bash
sudo nft list ruleset
```

These commands introduce Linux firewall management. Detailed packet filtering, connection tracking, and firewall chains are covered in their dedicated documents.

# 🗁 MikroTik RouterOS Firewall Fundamentals

MikroTik RouterOS includes an integrated firewall used to filter, inspect, and manage network traffic.

RouterOS firewall functionality includes:

* Filter rules.
* NAT rules.
* Connection tracking.
* Raw rules.
* Mangle rules.
* Address lists.
* Layer 7 matching capabilities.
* Logging.

The main firewall filter chains are:

| Chain | Purpose |
| --- | --- |
| `input` | Traffic destined for the router itself |
| `forward` | Traffic passing through the router |
| `output` | Traffic generated by the router |

The main NAT chains are:

| Chain | Purpose |
| --- | --- |
| `srcnat` | Source address translation |
| `dstnat` | Destination address translation |

**Inspect firewall filter rules**

```routeros
/ip firewall filter print
```

**Inspect NAT rules**

```routeros
/ip firewall nat print
```

**Inspect connection tracking**

```routeros
/ip firewall connection print
```

**Inspect firewall address lists**

```routeros
/ip firewall address-list print
```

**Example: Allow established and related connections**

```routeros
/ip firewall filter add \
    chain=input \
    connection-state=established,related \
    action=accept
```

This rule permits packets belonging to established or related connections addressed to the router.

It is an illustrative rule, not a complete firewall configuration. Rule ordering and the rest of the firewall policy determine the final behavior.

# 🗁 Common Firewall Policies

A firewall policy defines the traffic that should be permitted or denied.

**Default-Deny Policy**

A default-deny policy blocks traffic unless an explicit rule permits it.

This is a widely used security principle.

Example:

```text
Default Policy: DROP

Allow established connections
Allow required management access
Allow approved application traffic
Allow required DNS traffic
Allow required ICMP traffic
Drop remaining traffic
```

**Least-Privilege Policy**

Allow only the traffic required for legitimate business operations.

For example, a database server should not accept connections from every network if it only needs to communicate with an application server.

**Management Access Restriction**

Administrative services such as SSH and WinBox should be accessible only from trusted management networks.

**Inter-Zone Filtering**

Different network zones should communicate only through explicitly permitted services.

**Outbound Filtering**

Control which destinations and services internal systems can access.

**Logging Policy**

Record security-relevant events while avoiding excessive logging that could consume resources or hide important events.

# 🗁 Best Practices

* Use a default-deny policy for untrusted inbound traffic.
* Permit only required services and protocols.
* Restrict management access to trusted networks.
* Keep firewall rules organized and documented.
* Use stateful filtering where appropriate.
* Allow legitimate established and related traffic.
* Handle invalid connection states appropriately.
* Apply filtering to both IPv4 and IPv6.
* Preserve essential ICMP and ICMPv6 traffic.
* Segment networks according to security requirements.
* Avoid exposing unnecessary services to the internet.
* Use logging for important security events.
* Rate-limit repetitive or potentially abusive logging.
* Regularly review and remove obsolete rules.
* Back up firewall configurations before major changes.
* Test firewall changes before deploying them in production.

# 🗁 Common Misconceptions

| Misconception | Correct Explanation |
| --- | --- |
| A firewall and router are the same thing. | A router forwards packets; a firewall enforces security policies. |
| NAT provides complete firewall protection. | NAT does not replace explicit traffic filtering. |
| Firewalls only block incoming traffic. | Firewalls can control both inbound and outbound traffic. |
| Every firewall is stateful. | Some firewalls perform stateless packet filtering. |
| Blocking all ICMP is a good security policy. | Essential ICMP and ICMPv6 messages support network operation and troubleshooting. |
| A firewall automatically prevents every attack. | Firewalls reduce exposure but cannot eliminate every security threat. |
| Internal networks are always trusted. | Internal devices and network segments also require security controls. |
| A firewall rule has the same behavior on every platform. | Processing order, chains, states, and actions vary by implementation. |

# 🗁 Summary

A firewall monitors and controls network traffic according to security policies.

Important concepts include:

* Firewalls control incoming, outgoing, and forwarded traffic.
* Rules determine whether matching traffic is accepted, dropped, or rejected.
* Packet-filtering firewalls inspect packet attributes.
* Stateful firewalls track connection information.
* Application proxies can inspect application-layer communication.
* Firewalls can protect network boundaries and internal segments.
* Network zones help organize security policies.
* Linux uses Netfilter and nftables for firewall functionality.
* MikroTik RouterOS provides integrated filtering, NAT, and connection tracking.
* NAT, routing, and firewall filtering are separate networking functions.

Understanding firewall fundamentals provides the foundation for packet filtering, connection tracking, firewall chains, access-control lists, and network segmentation.

# 🗁 References

* [Netfilter Project](https://www.netfilter.org/)
* [nftables Wiki](https://wiki.nftables.org/)
* [nftables Manual](https://netfilter.org/projects/nftables/manpage.html)
* [Ubuntu UFW Documentation](https://help.ubuntu.com/community/UFW)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [NIST SP 800-41 Rev. 1 – Guidelines on Firewalls and Firewall Policy](https://csrc.nist.gov/pubs/sp/800/41/r1/final)
* [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
