# EoIP (Ethernet over IP)

EoIP (Ethernet over IP) is a MikroTik tunneling protocol that encapsulates Ethernet frames inside IP packets.

It allows two geographically separated MikroTik routers to connect Layer 2 networks over an IP network.

Unlike GRE and IPIP, which are primarily used for Layer 3 packet tunneling, EoIP provides Layer 2 Ethernet connectivity.

EoIP is useful for:

* Extending a LAN between two locations.
* Connecting remote Ethernet segments.
* Carrying VLAN traffic between sites.
* Bridging networks across an IP network.
* Connecting remote devices within the same Layer 2 broadcast domain.
* Building MikroTik-based Layer 2 network extensions.

EoIP does not provide encryption or authentication by itself. For secure deployments, protect the tunnel using IPsec or another suitable encrypted transport.

# 🗁 What Is EoIP?

EoIP stands for Ethernet over IP.

It is a MikroTik proprietary tunneling implementation that transports Ethernet frames over an IP connection.

The Ethernet frame is encapsulated and transmitted between two MikroTik tunnel endpoints.

The receiving router decapsulates the packet and forwards the original Ethernet frame.

```text
Original Ethernet Frame
          |
          v
+-------------------------+
| Ethernet Header         |
| Original Payload        |
+-------------------------+
          |
          v
       EoIP
   Encapsulation
          |
          v
+-------------------------+
| Outer IP Header         |
| EoIP Header             |
| Original Ethernet Frame|
+-------------------------+
          |
          v
    IP Network
          |
          v
    Decapsulation
          |
          v
Original Ethernet Frame
```

EoIP uses GRE-based encapsulation with MikroTik-specific tunnel identification.

# 🗁 EoIP Architecture

An EoIP tunnel creates a virtual Ethernet interface between two MikroTik routers.

The tunnel interfaces can be added to bridges to extend Layer 2 connectivity.

```text
              IP Network
                  |
          +-------+-------+
          |               |
       Router A        Router B
          |               |
      EoIP Tunnel     EoIP Tunnel
          |               |
       Bridge A         Bridge B
          |               |
      Switch A          Switch B
          |               |
       LAN A             LAN B
```

Devices connected to the two LANs can communicate at Layer 2 when the bridge and VLAN configuration permits it.

# 🗁 EoIP Characteristics

| Feature | Description |
| --- | --- |
| Technology | Ethernet tunneling |
| Encapsulation | GRE-based |
| Network Layer | Layer 2 transport |
| Transport Network | IP network |
| Tunnel Endpoint | MikroTik router |
| Tunnel Identification | Tunnel ID |
| Encryption | Not built in |
| Authentication | Not built in |
| VLAN Transport | Supported through Ethernet frame transport |
| Bridging | Can be added to a bridge |
| Broadcast Traffic | Can cross the tunnel |

# 🗁 EoIP vs GRE vs IPIP

| Feature | EoIP | GRE | IPIP |
| --- | --- | --- | --- |
| Primary Purpose | Ethernet tunneling | General packet tunneling | IP packet tunneling |
| Layer | Layer 2 | Primarily Layer 3 | Layer 3 |
| Ethernet Frames | Yes | Not a transparent Ethernet bridge by default | No |
| VLAN Transport | Through Ethernet frames | Depends on encapsulated protocol and implementation | No |
| Bridge Integration | Yes | Not as a standard Ethernet interface in this use case | No |
| Encryption | No built-in encryption | No built-in encryption | No built-in encryption |
| MikroTik Compatibility | MikroTik EoIP peers | Broad GRE support | IPIP support |

Use EoIP when extending Ethernet connectivity is required. Use GRE or IPIP when routed Layer 3 connectivity is sufficient.

# 🗁 EoIP Network Topology

This example connects two remote LANs through an EoIP tunnel.

```text
                       Internet
                           |
             +-------------+-------------+
             |                           |
          Router A                    Router B
          Site A                      Site B
             |                           |
       WAN: 203.0.113.1           WAN: 198.51.100.1
             |                           |
         EoIP Tunnel ID: 100
             |                           |
       eoip-site-b                  eoip-site-a
             |                           |
          Bridge A                    Bridge B
             |                           |
       192.168.10.0/24             192.168.10.0/24
             |                           |
          Devices                     Devices
```

The WAN addresses are documentation addresses. Replace them with reachable endpoint addresses in your environment.

Both LAN segments are shown in the same IP subnet because the objective is to extend one Layer 2 network.

# 🗁 EoIP Tunnel Requirements

Before configuring EoIP:

* Both routers must have IP connectivity.
* Each router must know the remote tunnel endpoint address.
* Both tunnel interfaces must use the same Tunnel ID.
* The tunnel ID must be unique for the intended tunnel endpoints.
* The required GRE-based traffic must be permitted through firewalls.
* The EoIP interfaces must be added to the appropriate bridges.
* The bridge and VLAN configuration must be consistent across both sites.

EoIP uses GRE protocol number `47` for its outer IPv4 transport.

It does not use a TCP or UDP port.

# 🗁 Configuring EoIP on Router A

Create the EoIP interface:

```routeros
/interface eoip
add name=eoip-site-b local-address=203.0.113.1 remote-address=198.51.100.1 tunnel-id=100
```

The important parameters are:

| Parameter | Purpose |
| --- | --- |
| `name` | Local tunnel interface name |
| `local-address` | Local tunnel endpoint address |
| `remote-address` | Remote tunnel endpoint address |
| `tunnel-id` | Identifies the EoIP tunnel |

Verify the interface:

```routeros
/interface eoip print detail
```

# 🗁 Configuring EoIP on Router B

Create the corresponding tunnel:

```routeros
/interface eoip
add name=eoip-site-a local-address=198.51.100.1 remote-address=203.0.113.1 tunnel-id=100
```

Both routers must use the same Tunnel ID.

Verify the configuration:

```routeros
/interface eoip print detail
```

# 🗁 Creating a Bridge on Router A

Create a bridge:

```routeros
/interface bridge
add name=bridge-site-a
```

Add the local LAN interface and EoIP tunnel:

```routeros
/interface bridge port
add bridge=bridge-site-a interface=ether2
add bridge=bridge-site-a interface=eoip-site-b
```

The physical LAN interface and EoIP interface now belong to the same Layer 2 bridge.

Replace `ether2` with the actual interface connected to the local LAN.

# 🗁 Creating a Bridge on Router B

Create the corresponding bridge:

```routeros
/interface bridge
add name=bridge-site-b
```

Add the local LAN interface and EoIP tunnel:

```routeros
/interface bridge port
add bridge=bridge-site-b interface=ether2
add bridge=bridge-site-b interface=eoip-site-a
```

Both routers now bridge their local Ethernet networks through the EoIP tunnel.

If the router already has a production LAN bridge, use the existing bridge where appropriate rather than creating an overlapping bridge.

# 🗁 Assigning LAN IP Addresses

When both sites are extending the same Layer 2 subnet, the LAN gateway should normally remain a single logical gateway.

For example:

```text
LAN Network: 192.168.10.0/24
Gateway:     192.168.10.1
```

The gateway may be located at Site A.

Devices at Site B can use the same gateway through the EoIP extension.

Do not configure duplicate gateway IP addresses on both routers.

If the existing LAN IP address is assigned to a bridge, keep it on the correct bridge interface after adding the EoIP tunnel.

# 🗁 Allowing EoIP Through the Firewall

EoIP requires GRE protocol traffic between the tunnel endpoints.

On Router A:

```routeros
/ip firewall filter
add chain=input action=accept protocol=gre src-address=198.51.100.1 dst-address=203.0.113.1 comment="Allow EoIP from Site B"
```

On Router B:

```routeros
/ip firewall filter
add chain=input action=accept protocol=gre src-address=203.0.113.1 dst-address=198.51.100.1 comment="Allow EoIP from Site A"
```

Place these rules before applicable input-chain drop rules.

Restrict GRE traffic to known tunnel endpoints whenever possible.

# 🗁 Understanding EoIP and VLANs

EoIP transports Ethernet frames, including VLAN-tagged frames.

This makes it possible to extend VLAN connectivity between remote sites.

Example:

```text
                  EoIP Tunnel
                       |
          +------------+------------+
          |                         |
       Router A                  Router B
          |                         |
       VLAN 10                   VLAN 10
       VLAN 20                   VLAN 20
       VLAN 30                   VLAN 30
          |                         |
       Switch A                  Switch B
```

The VLAN configuration must be consistent across both sites.

When using bridge VLAN filtering:

* Configure the correct VLAN IDs.
* Define tagged and untagged bridge ports.
* Configure PVIDs for access ports.
* Ensure the EoIP interface is handled as a bridge port.
* Ensure the bridge CPU port is included where Layer 3 access to a VLAN is required.

Do not assume that adding an EoIP interface to a bridge automatically provides the correct VLAN membership.

# 🗁 EoIP and Bridge VLAN Filtering

For VLAN-aware bridging, EoIP can act as a tagged trunk between remote bridges.

A simplified RouterOS bridge VLAN table might look like:

```routeros
/interface bridge vlan
add bridge=bridge-site-a vlan-ids=10 tagged=bridge-site-a,eoip-site-b
add bridge=bridge-site-a vlan-ids=20 tagged=bridge-site-a,eoip-site-b
```

This is an illustrative VLAN table.

Add the actual local tagged and untagged ports according to the network design. Ensure that each VLAN has the required bridge membership and PVID configuration.

The equivalent VLAN configuration must be applied at the remote site.

# 🗁 Understanding Broadcast Traffic

Because EoIP extends Layer 2 connectivity, broadcast and unknown-unicast traffic can cross the tunnel.

Examples include:

* ARP broadcasts.
* DHCP discovery traffic.
* Some discovery protocols.
* Layer 2 control traffic, depending on bridge handling.

This can be useful when extending an existing LAN, but it can also increase broadcast-domain size.

Consider the consequences of carrying broadcast traffic over a WAN connection.

# 🗁 DHCP Across EoIP

EoIP can carry DHCP broadcast traffic between two bridged LAN segments.

For example:

```text
Site A:
DHCP Server
192.168.10.1

          |
          |
       Bridge
          |
       EoIP Tunnel
          |
       Bridge
          |
Site B:
DHCP Clients
```

A DHCP client at Site B can receive an address from the DHCP server at Site A if:

* Both LAN segments belong to the same Layer 2 bridge domain.
* The EoIP tunnel is operational.
* Bridge filtering permits the relevant traffic.
* No conflicting DHCP server responds unexpectedly.

Use one intended DHCP authority for the shared subnet.

# 🗁 EoIP and MTU

EoIP adds encapsulation overhead to Ethernet frames.

The underlying WAN path must accommodate the additional packet size.

Potential symptoms include:

* Large packets failing.
* Intermittent connectivity.
* Slow file transfers.
* Fragmentation.
* Problems with certain applications.

Consider:

* Physical interface MTU.
* EoIP interface MTU.
* Ethernet frame size.
* VLAN tagging overhead.
* IPsec overhead when encryption is enabled.
* The actual path MTU between tunnel endpoints.

Check interface settings:

```routeros
/interface print detail
```

Do not change MTU values without testing the complete path.

# 🗁 Securing EoIP with IPsec

EoIP does not encrypt Ethernet frames.

Traffic crossing an untrusted network should be protected.

RouterOS EoIP interfaces support IPsec configuration through their IPsec-related properties.

For example:

```routeros
/interface eoip
set [find name=eoip-site-b] ipsec-secret=REPLACE_WITH_A_STRONG_SECRET
```

This enables the EoIP interface's integrated IPsec protection using the configured secret.

Configure the corresponding EoIP interface with the same secret on the remote router.

For production use:

* Use a strong, unique secret.
* Verify that IPsec SAs are established.
* Confirm that both endpoints use compatible IPsec settings.
* Check that the WAN firewall permits the required IKE and IPsec traffic.
* Avoid exposing the EoIP tunnel without protection over untrusted networks.

Integrated IPsec behavior and supported options should be verified against the installed RouterOS release.

# 🗁 Monitoring EoIP Interfaces

Display EoIP interfaces:

```routeros
/interface eoip print detail
```

Display interface statistics:

```routeros
/interface print stats
```

Display bridge ports:

```routeros
/interface bridge port print detail
```

Display bridge host entries:

```routeros
/interface bridge host print
```

Display bridge VLAN configuration:

```routeros
/interface bridge vlan print detail
```

If IPsec protection is enabled, inspect active security associations:

```routeros
/ip ipsec active-peers print
/ip ipsec installed-sa print
```

These commands help verify tunnel state, bridge membership, learned MAC addresses, VLAN configuration, and IPsec protection.

# 🗁 EoIP Troubleshooting

| Problem | Possible Cause |
| --- | --- |
| EoIP tunnel is not operational | Incorrect local or remote endpoint |
| Tunnel ID mismatch | Different Tunnel IDs configured on peers |
| Remote LAN devices cannot communicate | Incorrect bridge membership |
| MAC addresses are not learned remotely | Tunnel or bridge problem |
| DHCP does not work across the tunnel | Broadcast filtering or bridge configuration |
| VLAN traffic fails | Incorrect VLAN table or tagging |
| Large transfers fail | MTU or fragmentation issue |
| Tunnel does not work behind NAT | NAT device does not correctly handle GRE |
| IPsec protection fails | Incorrect secret or IPsec negotiation problem |
| Broadcast traffic becomes excessive | Layer 2 domain is too large |

# 🗁 EoIP Troubleshooting Commands

Check EoIP interface configuration:

```routeros
/interface eoip print detail
```

Check bridge membership:

```routeros
/interface bridge port print detail
```

Check learned MAC addresses:

```routeros
/interface bridge host print
```

Check VLAN membership:

```routeros
/interface bridge vlan print detail
```

Check firewall counters:

```routeros
/ip firewall filter print stats
```

Check IPsec status when encryption is enabled:

```routeros
/ip ipsec active-peers print
/ip ipsec installed-sa print
```

# 🗁 EoIP Security Best Practices

* Use IPsec protection when EoIP crosses untrusted networks.
* Restrict GRE traffic to known remote endpoints.
* Use strong, unique IPsec secrets.
* Keep RouterOS updated.
* Avoid unnecessarily extending large broadcast domains.
* Monitor broadcast and unknown-unicast traffic.
* Apply VLAN filtering where segmentation is required.
* Restrict access between VLANs using appropriate Layer 3 firewall policies.
* Monitor tunnel and bridge interface statistics.
* Avoid connecting redundant Layer 2 paths without proper loop prevention.

# 🗁 Removing EoIP Configuration

Remove the EoIP interface from the bridge:

```routeros
/interface bridge port
remove [find interface=eoip-site-b]
```

Remove the EoIP interface:

```routeros
/interface eoip
remove [find name=eoip-site-b]
```

Remove the corresponding firewall rule:

```routeros
/ip firewall filter
remove [find comment="Allow EoIP from Site B"]
```

Remove any dedicated bridge created specifically for the lab, but only after removing its dependent ports and confirming that it is not used by other services.

# 🗁 Practical Exercises

**Basic EoIP Lab**

* Configure two MikroTik routers.
* Establish WAN connectivity.
* Create an EoIP interface on each router.
* Configure matching Tunnel IDs.
* Verify the tunnel configuration.

**Layer 2 Bridging Lab**

* Create a bridge on each router.
* Add the local Ethernet interface.
* Add the EoIP interface.
* Connect devices at both locations.
* Verify that MAC addresses are learned across the tunnel.

**DHCP Extension Lab**

* Configure a DHCP server at Site A.
* Connect a DHCP client at Site B.
* Verify that the client receives an address from Site A.
* Test gateway and DNS connectivity.

**VLAN Extension Lab**

* Create VLAN 10 and VLAN 20.
* Configure bridge VLAN filtering.
* Extend the VLANs through EoIP.
* Verify tagged traffic at both sites.
* Test communication within each VLAN.

**Security Lab**

* Establish an EoIP tunnel.
* Enable integrated IPsec protection.
* Verify active IPsec peers and SAs.
* Test LAN communication.
* Inspect firewall counters.

# 🗁 Lab Completion Checklist

* [ ] Understand Ethernet over IP.
* [ ] Understand Layer 2 tunneling.
* [ ] Understand EoIP encapsulation.
* [ ] Configure matching Tunnel IDs.
* [ ] Create EoIP interfaces on two routers.
* [ ] Add EoIP interfaces to bridges.
* [ ] Verify remote MAC address learning.
* [ ] Extend DHCP across the tunnel.
* [ ] Transport VLAN-tagged traffic.
* [ ] Understand broadcast-domain implications.
* [ ] Understand MTU overhead.
* [ ] Secure EoIP with IPsec.
* [ ] Monitor and troubleshoot EoIP.
* [ ] Remove lab configuration safely.

# 🗁 Summary

EoIP is a MikroTik tunneling mechanism that extends Ethernet connectivity over an IP network.

It is useful when remote locations must share Layer 2 connectivity.

Important concepts include:

* Ethernet encapsulation.
* GRE-based transport.
* Tunnel IDs.
* Bridge integration.
* VLAN transport.
* Broadcast-domain extension.
* DHCP across tunnels.
* MTU considerations.
* IPsec protection.
* Monitoring and troubleshooting.

EoIP should be used when Layer 2 extension is genuinely required. For most networks that only need communication between separate IP subnets, routed VPN connectivity is generally simpler to manage and scale.
