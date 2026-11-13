# Small Office Network

A small office network connects employees, computers, printers, servers, wireless devices, and other business equipment through a centralized network infrastructure.

MikroTik RouterOS can provide routing, DHCP, DNS forwarding, firewall protection, VLAN segmentation, wireless connectivity, and bandwidth management for a small office.

This practical project demonstrates how to build a segmented small office network using a MikroTik router and managed switch.

# 🗁 Project Objectives

By completing this project, you will learn how to:

* Design a small office network topology.
* Configure WAN and LAN connectivity.
* Create VLANs for different departments.
* Configure inter-VLAN routing.
* Configure DHCP services for multiple networks.
* Implement firewall isolation between departments.
* Configure internet access using NAT.
* Configure management access.
* Monitor network devices and traffic.
* Create and verify configuration backups.

# 🗁 Network Requirements

Consider an office with the following departments:

| Department | VLAN ID | Network | Gateway |
| --- | ---: | --- | --- |
| Management | 10 | `192.168.10.0/24` | `192.168.10.1` |
| Administration | 20 | `192.168.20.0/24` | `192.168.20.1` |
| Development | 30 | `192.168.30.0/24` | `192.168.30.1` |
| Guest Wi-Fi | 40 | `192.168.40.0/24` | `192.168.40.1` |
| Network Management | 99 | `192.168.99.0/24` | `192.168.99.1` |

Additional requirements:

* One WAN connection from the ISP.
* One MikroTik router.
* One VLAN-aware managed switch.
* Separate networks for office departments.
* Guest users must not access internal office networks.
* Network management must be restricted to authorized administrators.
* All departments should have internet access.

# 🗁 Network Topology

```text
                         Internet
                            |
                            |
                        ISP Modem
                            |
                            |
                       ether1 (WAN)
                     +--------------+
                     | MikroTik     |
                     | Router       |
                     |              |
                     | VLAN Routing |
                     | Firewall     |
                     | DHCP         |
                     | NAT          |
                     +--------------+
                            |
                    ether2 (802.1Q Trunk)
                            |
                     +--------------+
                     | Managed      |
                     | Switch       |
                     +--------------+
                       |    |    |
              ----------    |    ----------
              |             |             |
          VLAN 10        VLAN 20       VLAN 30
          Management     Admin         Development

                     VLAN 40
                     Guest Wi-Fi

                     VLAN 99
                     Network Management

```

The MikroTik router performs Layer 3 routing between VLANs. The managed switch forwards tagged VLAN traffic to the appropriate access ports.

# 🗁 Interface and VLAN Plan

The example uses:

| MikroTik interface | Purpose |
| --- | --- |
| `ether1` | WAN connection |
| `ether2` | Trunk to managed switch |
| `bridge-office` | VLAN-aware bridge |
| `vlan10-management` | Management department |
| `vlan20-admin` | Administration department |
| `vlan30-development` | Development department |
| `vlan40-guest` | Guest network |
| `vlan99-network-management` | Network management |

The switch port connected to `ether2` must support 802.1Q VLAN tagging.

The following configuration assumes the switch is configured to carry all five VLANs on that trunk.

# 🗁 Preparing the MikroTik Router

Connect through WinBox or SSH.

Before applying the configuration:

* Identify the WAN and trunk interfaces.
* Confirm that the switch supports VLAN tagging.
* Save an existing configuration backup.
* Verify that the RouterOS version supports bridge VLAN filtering.
* Plan a management access method before enabling VLAN filtering.
* Prefer applying VLAN changes through a local or out-of-band management connection.

A mistake in bridge VLAN filtering can interrupt router management access.

# 🗁 Configuring Router Identity

Set a descriptive router identity:

```routeros
/system/identity/set name=Office-Router
```

Verify:

```routeros
/system/identity/print
```

# 🗁 Creating the Office Bridge

Create a VLAN-aware bridge:

```routeros
/interface/bridge/add name=bridge-office vlan-filtering=no protocol-mode=rstp
```

Add the trunk interface:

```routeros
/interface/bridge/port/add bridge=bridge-office interface=ether2
```

The MikroTik bridge acts as the Layer 2 interface connecting the VLAN networks.

The bridge itself represents the router's CPU-facing bridge port in the VLAN configuration.

# 🗁 Creating VLAN Interfaces

Create VLAN interfaces on the bridge:

```routeros
/interface/vlan/add name=vlan10-management interface=bridge-office vlan-id=10
/interface/vlan/add name=vlan20-admin interface=bridge-office vlan-id=20
/interface/vlan/add name=vlan30-development interface=bridge-office vlan-id=30
/interface/vlan/add name=vlan40-guest interface=bridge-office vlan-id=40
/interface/vlan/add name=vlan99-network-management interface=bridge-office vlan-id=99
```

Creating VLAN interfaces on the bridge allows RouterOS to route traffic between VLANs.

# 🗁 Configuring Bridge VLAN Membership

Configure the VLAN table:

```routeros
/interface/bridge/vlan/add bridge=bridge-office vlan-ids=10 tagged=bridge-office,ether2
/interface/bridge/vlan/add bridge=bridge-office vlan-ids=20 tagged=bridge-office,ether2
/interface/bridge/vlan/add bridge=bridge-office vlan-ids=30 tagged=bridge-office,ether2
/interface/bridge/vlan/add bridge=bridge-office vlan-ids=40 tagged=bridge-office,ether2
/interface/bridge/vlan/add bridge=bridge-office vlan-ids=99 tagged=bridge-office,ether2
```

The bridge CPU port is included as a tagged member because the router must receive the VLAN traffic to provide Layer 3 services.

Enable VLAN filtering after confirming the VLAN membership configuration:

```routeros
/interface/bridge/set bridge-office vlan-filtering=yes
```

For a production deployment, configure access ports on the managed switch with the appropriate untagged VLAN and PVID. Do not assume that every VLAN should be allowed on every switch port.

# 🗁 Assigning IP Addresses to VLAN Interfaces

Assign the gateway address for each department:

```routeros
/ip/address/add address=192.168.10.1/24 interface=vlan10-management comment="Management Gateway"
/ip/address/add address=192.168.20.1/24 interface=vlan20-admin comment="Administration Gateway"
/ip/address/add address=192.168.30.1/24 interface=vlan30-development comment="Development Gateway"
/ip/address/add address=192.168.40.1/24 interface=vlan40-guest comment="Guest Gateway"
/ip/address/add address=192.168.99.1/24 interface=vlan99-network-management comment="Network Management Gateway"
```

Verify:

```routeros
/ip/address/print
```

Each VLAN interface now acts as the default gateway for its corresponding network.

# 🗁 Configuring WAN Connectivity

For an ISP connection using DHCP:

```routeros
/ip/dhcp-client/add interface=ether1 add-default-route=yes use-peer-dns=no disabled=no
```

Verify the DHCP client:

```routeros
/ip/dhcp-client/print detail
```

Verify the default route:

```routeros
/ip/route/print
```

If the ISP uses PPPoE or static addressing, configure the appropriate WAN connection instead.

# 🗁 Configuring DHCP for Each VLAN

Create separate address pools:

```routeros
/ip/pool/add name=pool-management ranges=192.168.10.100-192.168.10.200
/ip/pool/add name=pool-admin ranges=192.168.20.100-192.168.20.200
/ip/pool/add name=pool-development ranges=192.168.30.100-192.168.30.200
/ip/pool/add name=pool-guest ranges=192.168.40.100-192.168.40.220
/ip/pool/add name=pool-network-management ranges=192.168.99.100-192.168.99.150
```

Create DHCP servers:

```routeros
/ip/dhcp-server/add name=dhcp-management interface=vlan10-management address-pool=pool-management lease-time=12h disabled=no
/ip/dhcp-server/add name=dhcp-admin interface=vlan20-admin address-pool=pool-admin lease-time=12h disabled=no
/ip/dhcp-server/add name=dhcp-development interface=vlan30-development address-pool=pool-development lease-time=12h disabled=no
/ip/dhcp-server/add name=dhcp-guest interface=vlan40-guest address-pool=pool-guest lease-time=4h disabled=no
/ip/dhcp-server/add name=dhcp-network-management interface=vlan99-network-management address-pool=pool-network-management lease-time=12h disabled=no
```

Create DHCP network definitions:

```routeros
/ip/dhcp-server/network/add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
/ip/dhcp-server/network/add address=192.168.20.0/24 gateway=192.168.20.1 dns-server=192.168.20.1
/ip/dhcp-server/network/add address=192.168.30.0/24 gateway=192.168.30.1 dns-server=192.168.30.1
/ip/dhcp-server/network/add address=192.168.40.0/24 gateway=192.168.40.1 dns-server=192.168.40.1
/ip/dhcp-server/network/add address=192.168.99.0/24 gateway=192.168.99.1 dns-server=192.168.99.1
```

Verify the DHCP servers:

```routeros
/ip/dhcp-server/print detail
/ip/dhcp-server/lease/print
```

# 🗁 Configuring DNS

Configure external DNS resolvers:

```routeros
/ip/dns/set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

The VLAN clients use their respective router gateway addresses as DNS servers.

Protect the router's DNS service with input firewall rules. Only trusted internal networks should be allowed to use the router as a recursive DNS resolver.

# 🗁 Configuring WAN and LAN Interface Lists

Create interface lists:

```routeros
/interface/list/add name=WAN
/interface/list/add name=LAN
```

Add the WAN interface:

```routeros
/interface/list/member/add list=WAN interface=ether1
```

Add the office VLAN interfaces:

```routeros
/interface/list/member/add list=LAN interface=vlan10-management
/interface/list/member/add list=LAN interface=vlan20-admin
/interface/list/member/add list=LAN interface=vlan30-development
/interface/list/member/add list=LAN interface=vlan40-guest
/interface/list/member/add list=LAN interface=vlan99-network-management
```

These lists simplify firewall rules that apply to multiple VLANs.

# 🗁 Configuring NAT

Configure masquerade for internet access:

```routeros
/ip/firewall/nat/add chain=srcnat action=masquerade out-interface-list=WAN comment="Office VLAN Internet NAT"
```

This allows private VLAN addresses to access the internet through the WAN interface.

NAT does not control inter-VLAN access. Inter-VLAN traffic must be controlled through firewall filtering.

# 🗁 Configuring Inter-VLAN Firewall Isolation

The firewall should enforce the office network's security policy.

The example policy will:

* Allow established and related traffic.
* Drop invalid connections.
* Allow authorized LAN devices to access the router.
* Block guest access to internal office networks.
* Allow management devices to administer the router.
* Allow office networks to access the internet.
* Block unsolicited WAN forwarding.

Create an address list for internal office networks:

```routeros
/ip/firewall/address-list/add list=office-networks address=192.168.10.0/24
/ip/firewall/address-list/add list=office-networks address=192.168.20.0/24
/ip/firewall/address-list/add list=office-networks address=192.168.30.0/24
/ip/firewall/address-list/add list=office-networks address=192.168.40.0/24
/ip/firewall/address-list/add list=office-networks address=192.168.99.0/24
```

Create an address list for administrative networks:

```routeros
/ip/firewall/address-list/add list=admin-networks address=192.168.10.0/24
/ip/firewall/address-list/add list=admin-networks address=192.168.99.0/24
```

Configure the input chain:

```routeros
/ip/firewall/filter/add chain=input action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=input action=drop connection-state=invalid comment="Drop invalid"
/ip/firewall/filter/add chain=input action=accept in-interface-list=LAN src-address-list=admin-networks comment="Allow management networks to router"
/ip/firewall/filter/add chain=input action=accept in-interface-list=LAN protocol=udp dst-port=53 comment="Allow internal DNS UDP"
/ip/firewall/filter/add chain=input action=accept in-interface-list=LAN protocol=tcp dst-port=53 comment="Allow internal DNS TCP"
/ip/firewall/filter/add chain=input action=accept protocol=icmp comment="Allow ICMP"
/ip/firewall/filter/add chain=input action=drop comment="Drop other router input"
```

The management networks are allowed to access router services. Guest clients are not included in the administrative address list.

Configure the forward chain:

```routeros
/ip/firewall/filter/add chain=forward action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=forward action=drop connection-state=invalid comment="Drop invalid"
/ip/firewall/filter/add chain=forward action=drop src-address=192.168.40.0/24 dst-address-list=office-networks comment="Block guest access to office networks"
/ip/firewall/filter/add chain=forward action=accept src-address-list=office-networks out-interface-list=WAN comment="Allow office networks to internet"
/ip/firewall/filter/add chain=forward action=drop in-interface-list=WAN connection-state=new connection-nat-state=!dstnat comment="Drop unsolicited WAN forwarding"
/ip/firewall/filter/add chain=forward action=drop comment="Drop other forwarding"
```

This is a restrictive example policy. It does not provide unrestricted communication between office departments.

For example, administration and development cannot initiate connections to each other under this policy. Add explicit inter-VLAN allow rules before the final drop rule when business applications require access between departments.

If port forwarding or VPN access is required, define the corresponding permitted traffic explicitly.

The firewall policy is IPv4-specific. Deploy an equivalent IPv6 security policy if IPv6 is enabled.

# 🗁 Configuring Managed Switch Access Ports

The managed switch must assign endpoint ports to the appropriate VLANs.

An example switch port plan:

| Switch port | Connected device | VLAN mode | VLAN |
| --- | --- | --- | ---: |
| Port 1 | MikroTik trunk | Tagged trunk | 10, 20, 30, 40, 99 |
| Port 2 | Management PC | Access | 10 |
| Port 3 | Administration PC | Access | 20 |
| Port 4 | Development PC | Access | 30 |
| Port 5 | Guest access point | Access or trunk | 40 |
| Port 6 | Network administrator | Access | 99 |

The access point configuration depends on whether it supports multiple SSIDs mapped to VLANs.

For an access port:

* Assign the intended VLAN as the untagged VLAN.
* Configure the corresponding PVID.
* Prevent unintended VLANs from reaching the endpoint.

For a trunk port:

* Allow only the VLANs required by the connected device.
* Configure tagged membership consistently at both ends.
* Avoid unnecessary native VLAN usage.

The switch configuration is vendor-specific and must be adapted to the actual managed switch.

# 🗁 Configuring Guest Network Isolation

Guest Wi-Fi should provide internet access without exposing internal office resources.

The VLAN 40 firewall rule blocks guest-initiated traffic to the office networks.

The guest access point should:

* Map the guest SSID to VLAN 40.
* Use WPA2 or WPA3 security as supported.
* Prevent guest clients from accessing access-point management interfaces.
* Avoid bridging guest clients into an internal office VLAN.

If guest clients must communicate only with the internet, keep the guest network isolated from all internal networks.

# 🗁 Testing VLAN Connectivity

After configuring the router and switch, verify VLAN connectivity.

Inspect bridge VLAN membership:

```routeros
/interface/bridge/vlan/print detail
```

Inspect bridge ports:

```routeros
/interface/bridge/port/print detail
```

Inspect VLAN interfaces:

```routeros
/interface/vlan/print
```

Verify IP addresses:

```routeros
/ip/address/print
```

Verify DHCP leases:

```routeros
/ip/dhcp-server/lease/print
```

Test gateway connectivity from each department.

For example, a development workstation should receive:

```text
IP Address:      192.168.30.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.30.1
DNS Server:      192.168.30.1
```

Test these conditions:

* Management devices can reach the router.
* Administration devices receive addresses from VLAN 20.
* Development devices receive addresses from VLAN 30.
* Guest devices receive addresses from VLAN 40.
* Guest devices cannot access internal office networks.
* Authorized office devices can access the internet.
* Network management is accessible only from authorized devices.

# 🗁 Monitoring Office Network Activity

View connected DHCP clients:

```routeros
/ip/dhcp-server/lease/print
```

View interface traffic:

```routeros
/interface/monitor-traffic ether2
```

Inspect firewall rules and counters:

```routeros
/ip/firewall/filter/print stats
```

Inspect active connections:

```routeros
/ip/firewall/connection/print
```

Inspect router resources:

```routeros
/system/resource/print
```

These commands help identify connectivity problems, unusual traffic patterns, and resource constraints.

# 🗁 Saving the Configuration

Create a binary backup:

```routeros
/system/backup/save name=small-office-backup
```

Create a text export:

```routeros
/export file=small-office-config
```

Download both files and store them in a secure external location.

A backup should be created after the network has been tested and the configuration has been validated.

# 🗁 Common Troubleshooting Scenarios

| Problem | Possible cause | Investigation |
| --- | --- | --- |
| VLAN clients receive no IP | Incorrect trunk tagging or DHCP configuration | Check bridge VLAN table and DHCP server |
| Router cannot reach a VLAN | Missing bridge CPU tagged membership | Check bridge VLAN configuration |
| One department cannot access internet | Incorrect gateway, route, or NAT | Check IP addresses, routes, and NAT |
| Guest clients access internal resources | Missing or incorrectly ordered firewall rule | Check forward-chain policy |
| Management access is unavailable | VLAN membership or input firewall issue | Check management VLAN and input chain |
| Devices receive addresses from the wrong subnet | Incorrect access-port PVID | Check managed switch VLAN configuration |
| Some traffic does not pass between devices | VLAN membership or switch filtering issue | Inspect trunk and access-port configuration |

# 🗁 Final Configuration Checklist

* [ ] WAN connectivity configured and tested.
* [ ] Office bridge created.
* [ ] Trunk interface added.
* [ ] VLAN interfaces created on the bridge.
* [ ] Bridge VLAN membership configured.
* [ ] Bridge CPU port tagged for routed VLANs.
* [ ] VLAN filtering enabled.
* [ ] Gateway addresses assigned.
* [ ] DHCP pools and servers configured.
* [ ] DNS configured.
* [ ] WAN and LAN interface lists created.
* [ ] NAT configured.
* [ ] Inter-VLAN firewall policy implemented.
* [ ] Guest network isolated.
* [ ] Managed switch access ports configured.
* [ ] Management access restricted.
* [ ] Connectivity tested.
* [ ] Configuration backup created.

# 🗁 Expected Result

After completing this project, the office network should provide:

* Separate networks for management, administration, development, guests, and network administration.
* Centralized Layer 3 routing through MikroTik.
* Automatic IP address assignment for each VLAN.
* Internet access through a shared WAN connection.
* Firewall-based guest isolation.
* Controlled inter-VLAN communication.
* Centralized router management.
* Basic network monitoring and backup capability.

This project provides a foundation for more advanced deployments involving dual-WAN failover, site-to-site VPNs, centralized monitoring, and enterprise network security.
