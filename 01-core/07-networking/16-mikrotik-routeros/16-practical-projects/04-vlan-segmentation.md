# VLAN Segmentation

VLAN segmentation is a network design technique that divides a physical network into multiple logical networks. Each VLAN creates a separate Layer 2 broadcast domain, allowing devices to be grouped according to their function, department, security requirements, or operational purpose.

MikroTik RouterOS v7 supports VLAN segmentation through bridge VLAN filtering, VLAN interfaces, firewall rules, and routing.

This project demonstrates how to build a segmented office network using a MikroTik router and a managed switch.

# 🗁 Project Objectives

By completing this project, you will learn how to:

* Design a VLAN-based office network.
* Configure a VLAN-aware bridge.
* Configure access and trunk ports.
* Assign VLAN IDs to different departments.
* Configure inter-VLAN routing.
* Configure DHCP services for individual VLANs.
* Restrict communication between VLANs.
* Configure a dedicated management VLAN.
* Verify VLAN isolation and connectivity.

# 🗁 Understanding the Network Design

This project uses five VLANs.

| VLAN ID | Name | Network | Purpose |
| ---: | --- | --- | --- |
| 10 | ADMIN | `192.168.10.0/24` | Administration |
| 20 | STAFF | `192.168.20.0/24` | Employees |
| 30 | GUEST | `192.168.30.0/24` | Guest devices |
| 40 | SERVERS | `192.168.40.0/24` | Internal servers |
| 99 | MANAGEMENT | `192.168.99.0/24` | Network management |

Each VLAN has its own IP subnet and broadcast domain.

# 🗁 Network Topology

The MikroTik router connects to a managed switch through a trunk link. The switch distributes VLAN traffic to access ports according to the connected devices.

```text
                         Internet
                            |
                         WAN Port
                            |
                    +----------------+
                    | MikroTik Router|
                    |                |
                    | VLAN Routing   |
                    | DHCP Services  |
                    | Firewall       |
                    +----------------+
                            |
                     Trunk: VLANs
                    10,20,30,40,99
                            |
                    +----------------+
                    | Managed Switch |
                    +----------------+
                      |   |   |   |   |
                      |   |   |   |   |
                   Admin Staff Guest Server Management
                    VLAN  VLAN  VLAN  VLAN    VLAN
                     10    20    30    40      99

```

# 🗁 Port Allocation Plan

The following example assumes:

* `ether1` is the WAN interface.
* `ether2` connects to the managed switch.
* `ether3` is an optional local management or testing port.
* VLAN interfaces are created on the bridge.

| MikroTik interface | Role |
| --- | --- |
| `ether1` | WAN |
| `ether2` | Trunk to managed switch |
| `ether3` | Optional access port for VLAN 99 |
| `bridge1` | VLAN-aware bridge |

The managed switch must also be configured with matching VLAN IDs and tagged/untagged membership.

# 🗁 VLAN IP Addressing Plan

The router will act as the default gateway for each VLAN.

| VLAN | Gateway | DHCP range |
| --- | --- | --- |
| ADMIN | `192.168.10.1/24` | `192.168.10.100-192.168.10.200` |
| STAFF | `192.168.20.1/24` | `192.168.20.100-192.168.20.200` |
| GUEST | `192.168.30.1/24` | `192.168.30.100-192.168.30.200` |
| SERVERS | `192.168.40.1/24` | `192.168.40.100-192.168.40.200` |
| MANAGEMENT | `192.168.99.1/24` | `192.168.99.100-192.168.99.150` |

For production networks, servers and infrastructure devices will often use statically assigned addresses or DHCP reservations rather than unrestricted dynamic addressing.

# 🗁 Creating the VLAN-Aware Bridge

Create the bridge:

```routeros
/interface/bridge/add name=bridge1 vlan-filtering=no protocol-mode=rstp
```

Add the trunk interface:

```routeros
/interface/bridge/port/add bridge=bridge1 interface=ether2
```

Add the optional management access port:

```routeros
/interface/bridge/port/add bridge=bridge1 interface=ether3 pvid=99
```

VLAN filtering is temporarily disabled so the bridge and VLAN membership can be configured before enforcement is enabled.

# 🗁 Configuring Bridge VLAN Membership

The bridge VLAN table determines which VLANs are tagged or untagged when frames leave bridge ports.

Add VLAN 10:

```routeros
/interface/bridge/vlan/add bridge=bridge1 vlan-ids=10 tagged=bridge1,ether2
```

Add VLAN 20:

```routeros
/interface/bridge/vlan/add bridge=bridge1 vlan-ids=20 tagged=bridge1,ether2
```

Add VLAN 30:

```routeros
/interface/bridge/vlan/add bridge=bridge1 vlan-ids=30 tagged=bridge1,ether2
```

Add VLAN 40:

```routeros
/interface/bridge/vlan/add bridge=bridge1 vlan-ids=40 tagged=bridge1,ether2
```

Add VLAN 99:

```routeros
/interface/bridge/vlan/add bridge=bridge1 vlan-ids=99 tagged=bridge1,ether2 untagged=ether3
```

Here:

* `bridge1` is the bridge CPU port.
* `ether2` is the trunk to the managed switch.
* `ether3` is an access port for VLAN 99.

The bridge CPU port must be a tagged member of VLANs that require router-level Layer 3 interfaces.

# 🗁 Configuring Port VLAN Behavior

Configure the trunk port:

```routeros
/interface/bridge/port/set [find interface=ether2] ingress-filtering=yes frame-types=admit-only-vlan-tagged
```

Configure the management access port:

```routeros
/interface/bridge/port/set [find interface=ether3] ingress-filtering=yes frame-types=admit-only-untagged-and-priority-tagged pvid=99
```

The trunk accepts tagged frames only.

The management access port accepts untagged or priority-tagged frames and classifies untagged ingress traffic into VLAN 99 through its PVID.

# 🗁 Enabling VLAN Filtering

Before enabling VLAN filtering, verify the bridge VLAN table and port settings.

```routeros
/interface/bridge/vlan/print
/interface/bridge/port/print detail
```

Enable VLAN filtering:

```routeros
/interface/bridge/set bridge1 vlan-filtering=yes
```

Enabling VLAN filtering changes how the bridge forwards traffic. Ensure that the management VLAN membership and access path are correct before applying this command, especially when configuring the router remotely.

# 🗁 Creating VLAN Interfaces

Create VLAN interfaces on the bridge, not directly on the trunk Ethernet interface.

```routeros
/interface/vlan/add name=vlan10-admin interface=bridge1 vlan-id=10
/interface/vlan/add name=vlan20-staff interface=bridge1 vlan-id=20
/interface/vlan/add name=vlan30-guest interface=bridge1 vlan-id=30
/interface/vlan/add name=vlan40-servers interface=bridge1 vlan-id=40
/interface/vlan/add name=vlan99-management interface=bridge1 vlan-id=99
```

These interfaces provide Layer 3 access to their corresponding VLANs.

# 🗁 Assigning Gateway IP Addresses

Assign the gateway address for each VLAN:

```routeros
/ip/address/add address=192.168.10.1/24 interface=vlan10-admin comment="ADMIN Gateway"
/ip/address/add address=192.168.20.1/24 interface=vlan20-staff comment="STAFF Gateway"
/ip/address/add address=192.168.30.1/24 interface=vlan30-guest comment="GUEST Gateway"
/ip/address/add address=192.168.40.1/24 interface=vlan40-servers comment="SERVERS Gateway"
/ip/address/add address=192.168.99.1/24 interface=vlan99-management comment="MANAGEMENT Gateway"
```

RouterOS automatically creates connected routes for these networks.

Verify:

```routeros
/ip/address/print
/ip/route/print
```

# 🗁 Configuring DHCP Pools

Create a separate DHCP pool for each VLAN:

```routeros
/ip/pool/add name=pool-admin ranges=192.168.10.100-192.168.10.200
/ip/pool/add name=pool-staff ranges=192.168.20.100-192.168.20.200
/ip/pool/add name=pool-guest ranges=192.168.30.100-192.168.30.200
/ip/pool/add name=pool-servers ranges=192.168.40.100-192.168.40.200
/ip/pool/add name=pool-management ranges=192.168.99.100-192.168.99.150
```

# 🗁 Configuring DHCP Servers

Create a DHCP server for each VLAN:

```routeros
/ip/dhcp-server/add name=dhcp-admin interface=vlan10-admin address-pool=pool-admin lease-time=12h disabled=no
/ip/dhcp-server/add name=dhcp-staff interface=vlan20-staff address-pool=pool-staff lease-time=12h disabled=no
/ip/dhcp-server/add name=dhcp-guest interface=vlan30-guest address-pool=pool-guest lease-time=4h disabled=no
/ip/dhcp-server/add name=dhcp-servers interface=vlan40-servers address-pool=pool-servers lease-time=12h disabled=no
/ip/dhcp-server/add name=dhcp-management interface=vlan99-management address-pool=pool-management lease-time=12h disabled=no
```

Create DHCP network definitions:

```routeros
/ip/dhcp-server/network/add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
/ip/dhcp-server/network/add address=192.168.20.0/24 gateway=192.168.20.1 dns-server=192.168.20.1
/ip/dhcp-server/network/add address=192.168.30.0/24 gateway=192.168.30.1 dns-server=192.168.30.1
/ip/dhcp-server/network/add address=192.168.40.0/24 gateway=192.168.40.1 dns-server=192.168.40.1
/ip/dhcp-server/network/add address=192.168.99.0/24 gateway=192.168.99.1 dns-server=192.168.99.1
```

Configure DNS:

```routeros
/ip/dns/set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

Restrict DNS access to trusted internal networks through the router's input firewall policy.

For a production server VLAN, consider static addressing or DHCP reservations instead of automatically assigning addresses to every server.

# 🗁 Configuring Internet NAT

Add the WAN interface to an interface list:

```routeros
/interface/list/add name=WAN
/interface/list/member/add list=WAN interface=ether1
```

Configure source NAT:

```routeros
/ip/firewall/nat/add chain=srcnat action=masquerade out-interface-list=WAN comment="VLAN Internet NAT"
```

This allows eligible internal VLAN traffic to use the WAN connection.

NAT does not provide VLAN isolation. Isolation must be enforced through firewall policies.

# 🗁 Understanding Inter-VLAN Routing

Inter-VLAN routing allows devices in different VLANs to communicate through a Layer 3 router.

For example:

```text
ADMIN VLAN 10
      |
      |
      v
MikroTik Router
      |
      +------ STAFF VLAN 20
      |
      +------ GUEST VLAN 30
      |
      +------ SERVERS VLAN 40
      |
      +------ MANAGEMENT VLAN 99
```

Because the MikroTik router has an IP address in each VLAN, it can route traffic between those networks.

By default, connected networks are routable through the router unless firewall policies restrict the traffic.

This means VLAN separation at Layer 2 does not automatically prevent communication at Layer 3.

# 🗁 Configuring Firewall Address Lists

Create an address list containing the internal networks:

```routeros
/ip/firewall/address-list/add list=INTERNAL-NETWORKS address=192.168.10.0/24
/ip/firewall/address-list/add list=INTERNAL-NETWORKS address=192.168.20.0/24
/ip/firewall/address-list/add list=INTERNAL-NETWORKS address=192.168.30.0/24
/ip/firewall/address-list/add list=INTERNAL-NETWORKS address=192.168.40.0/24
/ip/firewall/address-list/add list=INTERNAL-NETWORKS address=192.168.99.0/24
```

These entries will be used to control communication between VLANs.

# 🗁 Configuring Inter-VLAN Isolation

The following policy implements these access requirements:

| Source VLAN | Destination | Policy |
| --- | --- | --- |
| ADMIN | Other internal VLANs | Allow |
| STAFF | SERVERS | Allow |
| STAFF | Other internal VLANs | Deny |
| GUEST | Internal VLANs | Deny |
| GUEST | Internet | Allow |
| SERVERS | Internal VLANs | Deny by default |
| MANAGEMENT | Router and infrastructure | Allow according to policy |

The policy is intentionally restrictive for guest and staff networks.

Add the rules in the forward chain:

```routeros
/ip/firewall/filter/add chain=forward action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=forward action=drop connection-state=invalid comment="Drop invalid"

/ip/firewall/filter/add chain=forward action=accept src-address=192.168.10.0/24 dst-address-list=INTERNAL-NETWORKS comment="ADMIN to internal networks"

/ip/firewall/filter/add chain=forward action=accept src-address=192.168.20.0/24 dst-address=192.168.40.0/24 comment="STAFF to SERVERS"

/ip/firewall/filter/add chain=forward action=drop src-address=192.168.20.0/24 dst-address-list=INTERNAL-NETWORKS comment="Isolate STAFF from other internal networks"

/ip/firewall/filter/add chain=forward action=drop src-address=192.168.30.0/24 dst-address-list=INTERNAL-NETWORKS comment="Isolate GUEST from internal networks"

/ip/firewall/filter/add chain=forward action=drop src-address=192.168.40.0/24 dst-address-list=INTERNAL-NETWORKS comment="Restrict SERVERS to internal networks"

/ip/firewall/filter/add chain=forward action=accept src-address=192.168.99.0/24 dst-address-list=INTERNAL-NETWORKS comment="MANAGEMENT to internal networks"

/ip/firewall/filter/add chain=forward action=accept in-interface-list=LAN out-interface-list=WAN comment="Allow LAN VLANs to Internet"

/ip/firewall/filter/add chain=forward action=drop comment="Drop remaining forwarded traffic"
```

**Important:** This example assumes the router has an appropriately configured interface list named `LAN` containing all VLAN interfaces.

Create that list and add the VLAN interfaces:

```routeros
/interface/list/add name=LAN
/interface/list/member/add list=LAN interface=vlan10-admin
/interface/list/member/add list=LAN interface=vlan20-staff
/interface/list/member/add list=LAN interface=vlan30-guest
/interface/list/member/add list=LAN interface=vlan40-servers
/interface/list/member/add list=LAN interface=vlan99-management
```

If the list already exists, do not create it again.

The firewall rules must be integrated into the existing policy in the correct order. The final drop rule is a catch-all and should only be used after all required forwarding permissions have been defined.

The example allows ADMIN and MANAGEMENT to initiate connections toward internal networks. It does not automatically permit every possible return or new connection in both directions. Established and related traffic is accepted through connection tracking.

For a real server environment, replace broad network access with specific server addresses and service ports.

# 🗁 Protecting the Router Management Plane

The input chain controls traffic destined for the MikroTik router itself.

Permit established connections and trusted management access, then deny untrusted access.

Example:

```routeros
/ip/firewall/filter/add chain=input action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=input action=drop connection-state=invalid comment="Drop invalid"
/ip/firewall/filter/add chain=input action=accept in-interface=vlan99-management src-address=192.168.99.0/24 comment="Allow management VLAN"
/ip/firewall/filter/add chain=input action=accept in-interface=vlan10-admin src-address=192.168.10.0/24 comment="Allow ADMIN management"
/ip/firewall/filter/add chain=input action=accept protocol=icmp comment="Allow ICMP"
/ip/firewall/filter/add chain=input action=drop comment="Drop remaining router access"
```

This is an example policy. Add any required DHCP, DNS, VPN, or other router services before the final drop rule.

Restrict RouterOS management services to trusted networks as an additional control:

```routeros
/ip/service/set winbox address=192.168.99.0/24,192.168.10.0/24
/ip/service/set ssh address=192.168.99.0/24,192.168.10.0/24
/ip/service/set www disabled=yes
/ip/service/set www-ssl disabled=yes
/ip/service/set telnet disabled=yes
/ip/service/set ftp disabled=yes
```

Use secure management access and avoid exposing administrative services to the internet.

# 🗁 Configuring the Managed Switch

The managed switch must have a matching VLAN configuration.

Configure the port connected to MikroTik as a tagged trunk for the required VLANs:

| VLAN | Router uplink |
| --- | --- |
| 10 | Tagged |
| 20 | Tagged |
| 30 | Tagged |
| 40 | Tagged |
| 99 | Tagged |

Configure the endpoint ports as access ports:

| Switch port | VLAN | Device |
| --- | ---: | --- |
| `ether2` | 10 | Administration PC |
| `ether3` | 20 | Employee PC |
| `ether4` | 30 | Guest device |
| `ether5` | 40 | Server |
| `ether6` | 99 | Management workstation |

The exact switch commands depend on the manufacturer and operating system.

Ensure the switch management interface is assigned to VLAN 99 and that its uplink carries VLAN 99 tagged.

Do not configure an untagged native VLAN on the trunk unless both devices are deliberately configured to use the same native VLAN behavior.

# 🗁 Verifying VLAN Configuration

Inspect bridge ports:

```routeros
/interface/bridge/port/print detail
```

Inspect bridge VLAN membership:

```routeros
/interface/bridge/vlan/print detail
```

Inspect VLAN interfaces:

```routeros
/interface/vlan/print detail
```

Inspect assigned addresses:

```routeros
/ip/address/print
```

Inspect DHCP servers:

```routeros
/ip/dhcp-server/print detail
```

Inspect DHCP leases:

```routeros
/ip/dhcp-server/lease/print
```

Inspect firewall rules:

```routeros
/ip/firewall/filter/print
```

# 🗁 Testing VLAN Connectivity

Connect test devices to the appropriate switch ports.

For each device, verify:

* It receives an IP address from the correct subnet.
* Its default gateway matches its VLAN gateway.
* It can reach its own gateway.
* It can access permitted internal services.
* It cannot access prohibited internal networks.
* It can access the internet when permitted.

Example expected results:

| Test | Expected result |
| --- | --- |
| ADMIN to STAFF | Allowed |
| ADMIN to SERVERS | Allowed |
| STAFF to SERVERS | Allowed |
| STAFF to ADMIN | Blocked |
| GUEST to ADMIN | Blocked |
| GUEST to STAFF | Blocked |
| GUEST to SERVERS | Blocked |
| GUEST to Internet | Allowed |
| MANAGEMENT to router | Allowed |

The results depend on the firewall policy and the direction in which connections are initiated.

# 🗁 Common Troubleshooting Scenarios

| Problem | Possible cause | Investigation |
| --- | --- | --- |
| Device receives no DHCP address | Incorrect access VLAN or DHCP configuration | Check switch PVID and DHCP server |
| VLAN cannot reach its gateway | Missing bridge VLAN membership | Inspect bridge VLAN table |
| Trunk does not carry a VLAN | VLAN not allowed on trunk | Check tagged membership on both devices |
| Router management becomes inaccessible | Management VLAN missing from bridge CPU membership | Check VLAN 99 configuration |
| VLANs communicate unexpectedly | Missing or incorrectly ordered firewall rules | Inspect forward chain |
| Guest devices cannot access internet | Missing NAT or incorrect forward policy | Check NAT and firewall |
| VLAN works intermittently | Incorrect bridge filtering or switch configuration | Inspect VLAN membership and ingress filtering |
| Server is unreachable from STAFF | Missing allow rule or wrong server subnet | Check firewall and addressing |

# 🗁 Final Configuration Checklist

* [ ] VLAN architecture designed.
* [ ] VLAN IDs and subnets documented.
* [ ] VLAN-aware bridge created.
* [ ] Trunk port configured.
* [ ] Access port PVIDs configured.
* [ ] Bridge VLAN table populated.
* [ ] Bridge CPU port tagged for routed VLANs.
* [ ] VLAN filtering enabled.
* [ ] VLAN interfaces created on the bridge.
* [ ] Gateway IP addresses assigned.
* [ ] DHCP pools and servers configured.
* [ ] NAT configured.
* [ ] Inter-VLAN routing verified.
* [ ] Firewall isolation rules configured.
* [ ] Management access restricted.
* [ ] Managed switch VLANs configured.
* [ ] Connectivity and isolation tests completed.
* [ ] Router configuration backup created.

# 🗁 Expected Result

After completing this project, the office network should have five logically separated VLANs with independent IP networks and DHCP services.

The MikroTik router provides inter-VLAN routing, internet connectivity, and firewall enforcement.

The managed switch handles VLAN-aware Layer 2 forwarding, while the router controls which networks and services are allowed to communicate.

This architecture provides a foundation for scalable office networks, enterprise segmentation, and ISP customer network isolation.
