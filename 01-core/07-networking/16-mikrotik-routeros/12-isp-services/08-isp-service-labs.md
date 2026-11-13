# ISP Service Labs

This section contains practical MikroTik RouterOS v7 labs covering common ISP services, including PPPoE, DHCP, Hotspot, RADIUS, User Manager, and CGNAT.

The labs combine previously covered concepts into realistic ISP network scenarios.

# 🗁 Lab Environment

You can build these labs using:

* MikroTik RouterOS v7 physical routers.
* MikroTik CHR (Cloud Hosted Router).
* VirtualBox.
* VMware Workstation.
* GNS3.
* EVE-NG.
* Linux virtual machines as subscriber clients.

For a virtual lab, use multiple RouterOS CHR instances and Linux clients.

# 🗁 Lab Network Topology

The following topology represents a small ISP network.

```text
                         Internet
                            |
                       Upstream Router
                            |
                       ether1 (WAN)
                            |
                      Core Router
                            |
                     ISP Core Network
                            |
                    Access / BNG Router
                            |
             +--------------+--------------+
             |              |              |
          VLAN 100        VLAN 200       VLAN 300
          PPPoE           Hotspot        DHCP
             |              |              |
        Subscriber 1   Subscriber 2   Subscriber 3
             |              |              |
            CPE            Client          Client
```

For larger labs, deploy separate routers for core routing, subscriber access, and CGNAT.

# 🗁 Lab Addressing Plan

Use the following example addressing plan:

| Component | Address |
| --- | --- |
| ISP core transit | `10.10.10.0/30` |
| Access router transit | `10.10.10.2/30` |
| Core router transit | `10.10.10.1/30` |
| PPPoE subscriber pool | `10.20.0.0/24` |
| Hotspot network | `192.168.20.0/24` |
| DHCP subscriber network | `192.168.30.0/24` |
| CGNAT subscriber range | `100.64.0.0/10` |
| Documentation public address | `198.51.100.10` |

The public address shown is reserved for documentation. Use a real upstream-assigned address when testing Internet connectivity.

# 🗁 Lab: Configure PPPoE Server

**Objective**

Create a PPPoE access service for subscribers.

**Network plan**

| Setting | Value |
| --- | --- |
| PPPoE interface | `vlan100-pppoe` |
| Local gateway | `10.20.0.1` |
| Subscriber pool | `10.20.0.10-10.20.0.200` |
| PPP profile | `pppoe-basic` |
| Example username | `subscriber01` |
| Example password | `StrongLabPassword123!` |

**Create an IP pool**

```routeros
/ip/pool
add name=pppoe-pool ranges=10.20.0.10-10.20.0.200
```

**Create a PPP profile**

```routeros
/ppp/profile
add name=pppoe-basic \
    local-address=10.20.0.1 \
    remote-address=pppoe-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    rate-limit=10M/20M
```

**Create a PPPoE server**

```routeros
/interface/pppoe-server/server
add interface=vlan100-pppoe \
    service-name=ISP-PPPoE \
    default-profile=pppoe-basic \
    authentication=chap,mschap2 \
    one-session-per-host=yes \
    disabled=no
```

**Create a subscriber account**

```routeros
/ppp/secret
add name=subscriber01 \
    password=StrongLabPassword123! \
    service=pppoe \
    profile=pppoe-basic
```

**Verification**

```routeros
/interface/pppoe-server/server/print
/ppp/active/print
/ppp/secret/print
```

Connect a PPPoE client and verify that it receives an address from the configured pool.

# 🗁 Lab: Configure PPPoE Client

**Objective**

Connect a customer router to the ISP's PPPoE service.

**Configure the PPPoE client**

```routeros
/interface/pppoe-client
add name=pppoe-out1 \
    interface=ether1 \
    user=subscriber01 \
    password=StrongLabPassword123! \
    add-default-route=yes \
    use-peer-dns=yes \
    disabled=no
```

**Configure LAN addressing**

```routeros
/ip/address
add address=192.168.50.1/24 interface=bridge-lan
```

**Configure NAT on the customer router**

```routeros
/ip/firewall/nat
add chain=srcnat \
    src-address=192.168.50.0/24 \
    out-interface=pppoe-out1 \
    action=masquerade
```

**Verification**

```routeros
/interface/pppoe-client/print detail
/ip/address/print
/ip/route/print
/ping 1.1.1.1
```

Confirm that the PPPoE session is connected and the customer LAN can reach the Internet through the upstream service.

# 🗁 Lab: Configure Subscriber Profiles

**Objective**

Create multiple service packages for different subscriber bandwidth requirements.

**Create IP pools**

```routeros
/ip/pool
add name=basic-pool ranges=10.20.1.10-10.20.1.100
add name=standard-pool ranges=10.20.2.10-10.20.2.100
add name=premium-pool ranges=10.20.3.10-10.20.3.100
```

**Create service profiles**

```routeros
/ppp/profile
add name=basic \
    local-address=10.20.1.1 \
    remote-address=basic-pool \
    rate-limit=5M/10M

add name=standard \
    local-address=10.20.2.1 \
    remote-address=standard-pool \
    rate-limit=10M/20M

add name=premium \
    local-address=10.20.3.1 \
    remote-address=premium-pool \
    rate-limit=20M/50M
```

**Create subscriber accounts**

```routeros
/ppp/secret
add name=user-basic password=BasicLab123! service=pppoe profile=basic

add name=user-standard password=StandardLab123! service=pppoe profile=standard

add name=user-premium password=PremiumLab123! service=pppoe profile=premium
```

**Verification**

```routeros
/ppp/profile/print detail
/ppp/secret/print detail
/ppp/active/print detail
```

Connect each subscriber and verify the assigned address and bandwidth profile.

# 🗁 Lab: Configure Hotspot Service

**Objective**

Create a captive portal for customers connecting through a shared access network.

**Network plan**

| Setting | Value |
| --- | --- |
| Hotspot interface | `vlan200-hotspot` |
| Gateway | `192.168.20.1/24` |
| Address pool | `192.168.20.10-192.168.20.200` |
| DHCP network | `192.168.20.0/24` |

**Assign the gateway address**

```routeros
/ip/address
add address=192.168.20.1/24 interface=vlan200-hotspot
```

**Create an address pool**

```routeros
/ip/pool
add name=hotspot-pool ranges=192.168.20.10-192.168.20.200
```

**Configure DHCP**

```routeros
/ip/dhcp-server
add name=hotspot-dhcp \
    interface=vlan200-hotspot \
    address-pool=hotspot-pool \
    disabled=no
```

```routeros
/ip/dhcp-server/network
add address=192.168.20.0/24 \
    gateway=192.168.20.1 \
    dns-server=192.168.20.1
```

**Configure DNS**

```routeros
/ip/dns
set allow-remote-requests=yes servers=1.1.1.1,8.8.8.8
```

**Configure Hotspot**

```routeros
/ip/hotspot/setup
```

Follow the interactive setup and select the configured interface, gateway, address pool, and DNS settings.

**Create a test user**

```routeros
/ip/hotspot/user
add name=hotspot-user password=HotspotLab123! \
    profile=default
```

**Verification**

```routeros
/ip/hotspot/print
/ip/hotspot/active/print
/ip/hotspot/user/print
```

Connect a client, open a browser, and verify that the captive portal appears.

# 🗁 Lab: Configure RADIUS Authentication

**Objective**

Centralize subscriber authentication using a RADIUS server.

A RADIUS server can authenticate PPPoE and Hotspot users without requiring every subscriber account to be maintained locally on each access router.

**Example network**

```text
                 RADIUS Server
                  10.10.20.10
                       |
                       |
                 ISP Core Network
                       |
                 Access Router
                       |
                PPPoE Subscribers
```

**Configure the RADIUS client on MikroTik**

```routeros
/radius
add service=ppp,hotspot \
    address=10.10.20.10 \
    secret=RadiusLabSecret123! \
    authentication-port=1812 \
    accounting-port=1813 \
    timeout=3s
```

**Enable RADIUS for PPP authentication**

```routeros
/ppp/aaa
set use-radius=yes accounting=yes
```

**Enable RADIUS for Hotspot authentication**

```routeros
/ip/hotspot/profile
set [find default=yes] use-radius=yes
```

**Verification**

```routeros
/radius/print detail
/radius/monitor [find]
/ppp/active/print
/ip/hotspot/active/print
```

Check the RADIUS server logs for authentication requests and responses.

The RADIUS server must have the MikroTik router registered as a client with the same shared secret. UDP ports 1812 and 1813 must be reachable according to the server's firewall policy.

# 🗁 Lab: Configure User Manager

**Objective**

Use MikroTik User Manager to manage subscriber accounts and service packages centrally.

**Preparation**

Install and enable the User Manager package compatible with the RouterOS version and device architecture.

Verify the package:

```routeros
/system/package/print
```

**Configure the RADIUS client**

```routeros
/user-manager/router
add name=access-router \
    address=10.10.10.2 \
    shared-secret=UserManagerSecret123!
```

**Create a subscriber**

```routeros
/user-manager/user
add name=um-subscriber \
    password=UserManagerLab123!
```

**Create a service limitation**

```routeros
/user-manager/limitation
add name=basic-limit \
    rate-limit-rx=10M \
    rate-limit-tx=5M
```

**Create a service profile**

```routeros
/user-manager/profile
add name=basic-package \
    name-for-users=Basic \
    validity=30d
```

Assign the limitation to the profile using the User Manager session and profile configuration supported by the installed RouterOS v7 release.

**Verification**

```routeros
/user-manager/user/print
/user-manager/profile/print
/user-manager/limitation/print
```

Verify that the access router can communicate with User Manager and that subscriber authentication succeeds.

User Manager menu structure and available properties can vary between RouterOS releases. Validate the exact commands against the installed package version.

# 🗁 Lab: Configure CGNAT

**Objective**

Translate subscriber shared addresses into an ISP public IPv4 address.

**Example network**

| Component | Address |
| --- | --- |
| Subscriber network | `100.64.10.0/24` |
| Access router transit | `10.10.10.1/30` |
| CGNAT router transit | `10.10.10.2/30` |
| Documentation public address | `198.51.100.10` |

**Configure the CGNAT router**

Assign the transit address:

```routeros
/ip/address
add address=10.10.10.2/30 interface=ether2
```

Configure a documentation-only public address for an isolated lab:

```routeros
/ip/address
add address=198.51.100.10/24 interface=ether1
```

Add a route to the subscriber network:

```routeros
/ip/route
add dst-address=100.64.10.0/24 gateway=10.10.10.1
```

Configure source NAT:

```routeros
/ip/firewall/nat
add chain=srcnat \
    src-address=100.64.10.0/24 \
    out-interface=ether1 \
    action=src-nat \
    to-addresses=198.51.100.10
```

**Configure the access router**

Add the transit address:

```routeros
/ip/address
add address=10.10.10.1/30 interface=ether2
```

Add a route toward the CGNAT router:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=10.10.10.2
```

**Verification**

```routeros
/ip/route/print
/ip/firewall/nat/print stats
/ip/firewall/connection/print detail
```

Generate traffic from a subscriber client and inspect the connection-tracking table.

For a real Internet test, replace the documentation address with a public address assigned and routed by the upstream provider.

# 🗁 Lab: Combine PPPoE and CGNAT

**Objective**

Build an ISP access network where PPPoE subscribers receive shared IPv4 addresses and access the Internet through CGNAT.

**Architecture**

```text
                     Internet
                         |
                    Upstream Router
                         |
                    CGNAT Router
                         |
                    ISP Core Router
                         |
                    Access Router
                         |
                     PPPoE Server
                         |
                 +-------+-------+
                 |               |
             Subscriber 1    Subscriber 2
              PPPoE Client    PPPoE Client
                 |               |
             Customer LAN    Customer LAN
```

**Configuration requirements**

* Configure PPPoE subscriber authentication.
* Assign subscriber addresses from a shared address range.
* Configure PPP profiles and bandwidth limits.
* Route subscriber traffic toward the CGNAT router.
* Configure public source NAT on the CGNAT router.
* Configure firewall forwarding.
* Verify return routing.
* Inspect active PPPoE sessions and NAT translations.

**Verification**

```routeros
/ppp/active/print detail
/ip/route/print
/ip/firewall/nat/print stats
/ip/firewall/connection/print count-only
```

Test Internet connectivity from multiple PPPoE subscribers.

# 🗁 Lab: Build a Multi-Service ISP Network

**Objective**

Combine PPPoE, Hotspot, DHCP, RADIUS, and CGNAT in one small ISP environment.

**Service design**

| Service | VLAN | Network | Authentication |
| --- | --- | --- | --- |
| PPPoE | VLAN 100 | `10.20.0.0/24` | RADIUS |
| Hotspot | VLAN 200 | `192.168.20.0/24` | RADIUS |
| DHCP access | VLAN 300 | `192.168.30.0/24` | DHCP |
| Management | VLAN 99 | `10.99.0.0/24` | Router administration |

**Implementation tasks**

* Create VLAN interfaces or bridge VLAN membership according to the access architecture.
* Configure subscriber-facing ports.
* Configure the PPPoE server on the correct subscriber-facing interface.
* Configure Hotspot and DHCP services on their respective VLANs.
* Configure RADIUS authentication and accounting.
* Configure CGNAT for subscriber IPv4 traffic.
* Restrict management access to the management network.
* Configure monitoring and logging.

Do not place unrelated subscriber services on the same VLAN unless the network design explicitly requires it.

# 🗁 Lab: Subscriber Management and Monitoring

**Objective**

Monitor subscriber sessions and identify common service problems.

**Monitor PPPoE subscribers**

```routeros
/ppp/active/print detail
```

**Monitor Hotspot subscribers**

```routeros
/ip/hotspot/active/print detail
```

**Monitor DHCP leases**

```routeros
/ip/dhcp-server/lease/print detail
```

**Monitor RADIUS**

```routeros
/radius/monitor [find]
```

**Monitor system resources**

```routeros
/system/resource/print
```

**Monitor interfaces**

```routeros
/interface/print stats
```

Create a monitoring checklist covering:

* Active subscriber count.
* Authentication failures.
* Disconnected sessions.
* DHCP lease utilization.
* Interface bandwidth.
* CPU and memory usage.
* Connection tracking utilization.
* RADIUS availability.
* CGNAT translation activity.

# 🗁 Lab: Troubleshooting Scenarios

**Scenario: PPPoE client cannot connect**

Investigate:

* Physical connectivity.
* VLAN membership.
* PPPoE server interface.
* Subscriber credentials.
* PPP profile.
* Authentication methods.
* Server-side logs.

Useful commands:

```routeros
/interface/pppoe-server/server/print detail
/ppp/secret/print detail
/ppp/active/print detail
/log/print
```

**Scenario: Hotspot login page does not appear**

Investigate:

* Client IP assignment.
* DHCP configuration.
* Hotspot interface.
* DNS configuration.
* Captive portal behavior.
* Firewall and NAT rules.

Useful commands:

```routeros
/ip/dhcp-server/lease/print
/ip/hotspot/print detail
/ip/hotspot/host/print
/ip/hotspot/active/print
```

**Scenario: RADIUS authentication fails**

Investigate:

* RADIUS server reachability.
* Shared secret.
* NAS address and identity.
* Authentication and accounting ports.
* Server-side client configuration.
* User authorization and service attributes.

Useful commands:

```routeros
/radius/print detail
/radius/monitor [find]
/log/print
```

**Scenario: Subscriber has no Internet access**

Investigate:

* Subscriber address and gateway.
* Routing table.
* Default route.
* Firewall forwarding.
* Source NAT.
* Upstream connectivity.
* Return routing.

Useful commands:

```routeros
/ip/address/print
/ip/route/print
/ip/firewall/filter/print
/ip/firewall/nat/print stats
```

**Scenario: CGNAT connections fail intermittently**

Investigate:

* Connection tracking capacity.
* Public address pool.
* Port utilization.
* CPU and memory.
* Upstream routing.
* NAT rule matching.
* Subscriber traffic patterns.

Useful commands:

```routeros
/ip/firewall/connection/print count-only
/ip/firewall/connection/tracking/print
/system/resource/print
/ip/firewall/nat/print stats
```

# 🗁 Final ISP Service Project

Build a complete small ISP network using RouterOS v7.

**Project requirements**

* A core router connected to an upstream network.
* An access router providing subscriber connectivity.
* PPPoE authentication and service profiles.
* Hotspot captive portal.
* DHCP-based subscriber access.
* RADIUS authentication and accounting.
* User Manager integration where supported.
* CGNAT for shared IPv4 connectivity.
* Management VLAN and restricted administration.
* Firewall rules for subscriber isolation and router protection.
* Monitoring and troubleshooting procedures.

**Project validation checklist**

* [ ] Core and access routers have working connectivity.
* [ ] Subscriber VLANs are configured correctly.
* [ ] PPPoE subscribers can authenticate.
* [ ] PPPoE profiles assign the expected addresses and rate limits.
* [ ] Hotspot clients receive DHCP addresses.
* [ ] Hotspot authentication works.
* [ ] DHCP subscribers receive valid network configuration.
* [ ] RADIUS authentication and accounting work.
* [ ] User Manager can manage test subscribers.
* [ ] Subscriber traffic reaches the CGNAT router.
* [ ] CGNAT translates subscriber traffic correctly.
* [ ] Firewall policies enforce the intended access restrictions.
* [ ] Management services are accessible only from authorized networks.
* [ ] Monitoring identifies service and resource problems.
* [ ] Configuration backups are available.

# 🗁 Lab Documentation and Best Practices

For every lab, document:

* Network topology.
* IP addressing plan.
* VLAN assignments.
* Router interface assignments.
* Subscriber service configuration.
* Authentication architecture.
* Routing configuration.
* Firewall and NAT rules.
* Verification commands.
* Expected results.
* Troubleshooting steps.

Use separate lab configurations for testing new services before deploying them in a production ISP network.

Keep RouterOS versions consistent across lab devices where possible, maintain backups before configuration changes, and use dedicated credentials rather than reusing example passwords.
