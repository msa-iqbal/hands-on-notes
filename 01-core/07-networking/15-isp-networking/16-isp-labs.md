# ISP Labs and Practical Exercises

ISP networking involves multiple technologies working together to deliver internet connectivity to subscribers. Understanding these technologies requires practical experience with routing, switching, VLANs, PPPoE, RADIUS, DHCP, NAT, CGNAT, BGP, fiber access, monitoring, and troubleshooting.

This document provides hands-on labs designed to simulate real-world Internet Service Provider (ISP) environments.

The labs progress from basic ISP network setup to more advanced subscriber management, routing, redundancy, and monitoring scenarios.

# 🗁 Lab Objectives

By completing these labs, you should be able to:

* Design a basic ISP network topology.
* Configure MikroTik RouterOS v7 for ISP operations.
* Build a multi-POP network.
* Configure VLAN-based subscriber separation.
* Configure PPPoE servers and clients.
* Implement centralized subscriber authentication concepts.
* Configure DHCP-based subscriber access.
* Implement NAT and CGNAT.
* Configure bandwidth management.
* Understand basic upstream routing and BGP peering.
* Implement network monitoring.
* Troubleshoot common subscriber connectivity problems.
* Understand how ISP infrastructure components work together.

# 🗁 Recommended Lab Environment

You can build the lab environment using physical devices or network simulation platforms.

| Platform | Purpose |
| --- | --- |
| MikroTik CHR | Virtual RouterOS router |
| VirtualBox | Virtual machine hosting |
| GNS3 | Network topology simulation |
| EVE-NG | Multi-vendor network emulation |
| VMware Workstation | Virtual machine hosting |
| Ubuntu Server | DNS, DHCP, RADIUS, monitoring, and Linux services |
| Wireshark | Packet capture and protocol analysis |
| FreeRADIUS | Centralized subscriber authentication |
| Zabbix | Network monitoring |
| The Dude | MikroTik network monitoring |

# 🗁 Lab Architecture

The following topology represents a simplified ISP environment.

```text
                         INTERNET
                            |
                     [Upstream Router]
                            |
                       [Core Router]
                            |
                 +----------+----------+
                 |                     |
             [POP-01]               [POP-02]
                 |                     |
           [Access Switch]       [Access Switch]
                 |                     |
          +------+------+        +-----+------+
          |             |        |            |
       VLAN 120      VLAN 130  VLAN 120     VLAN 130
          |             |        |            |
       Customer A   Customer B Customer C  Customer D
          |             |        |            |
       Router        Router    Router       Router
```

This topology is suitable for practicing subscriber access, VLAN separation, routing, and centralized management.

# 🗁 Lab 01: Build a Basic ISP Network

**Objective:** Create a basic ISP topology with an upstream connection, core router, access network, and customer devices.

**Topology:**

```text
                  Internet Simulation
                          |
                     ether1
                    [ISP-Core]
                     ether2
                          |
                     [Switch]
                          |
                   +------+------+
                   |             |
              Customer-01   Customer-02
```

**IP addressing plan:**

| Device | Interface | IP Address |
| --- | --- | --- |
| Upstream Router | LAN | 192.0.2.1/30 |
| ISP-Core | ether1 | 192.0.2.2/30 |
| ISP-Core | ether2 | 10.10.10.1/24 |
| Customer-01 | LAN | 10.10.10.10/24 |
| Customer-02 | LAN | 10.10.10.20/24 |

The `192.0.2.0/24` range is reserved for documentation and examples. Use an isolated lab environment rather than a real public internet connection for this addressing plan.

**RouterOS configuration:**

Configure the upstream-facing interface:

```routeros
/ip address
add address=192.0.2.2/30 interface=ether1
```

Configure the customer-facing interface:

```routeros
/ip address
add address=10.10.10.1/24 interface=ether2
```

Configure a default route:

```routeros
/ip route
add dst-address=0.0.0.0/0 gateway=192.0.2.1
```

Configure source NAT for the lab subnet:

```routeros
/ip firewall nat
add chain=srcnat out-interface=ether1 action=masquerade
```

**Verification:**

```routeros
/ip address print
/ip route print
/ip firewall nat print
/ping 192.0.2.1
```

From a customer device, test connectivity to the ISP gateway and upstream test network.

**Expected result:**

* The ISP router has connectivity to the upstream router.
* Customer devices can reach their default gateway.
* Customer traffic can be translated through the ISP router.

# 🗁 Lab 02: Configure VLAN-Based Subscriber Separation

**Objective:** Separate customers into different VLANs.

**Topology:**

```text
                    [ISP Router]
                         |
                    VLAN Trunk
                         |
                     [Switch]
                    /         \
              VLAN 120      VLAN 130
                 |              |
            Customer A     Customer B
```

**VLAN plan:**

| VLAN ID | Purpose | Network |
| --- | --- | --- |
| 120 | Subscriber A | 10.120.0.0/24 |
| 130 | Subscriber B | 10.130.0.0/24 |
| 999 | Management | 10.99.0.0/24 |

**RouterOS configuration:**

Create a VLAN-aware bridge:

```routeros
/interface bridge
add name=bridge-isp vlan-filtering=no
```

Add the trunk interface:

```routeros
/interface bridge port
add bridge=bridge-isp interface=ether1
```

Create VLAN interfaces:

```routeros
/interface vlan
add name=vlan120 vlan-id=120 interface=bridge-isp
add name=vlan130 vlan-id=130 interface=bridge-isp
```

Assign gateway addresses:

```routeros
/ip address
add address=10.120.0.1/24 interface=vlan120
add address=10.130.0.1/24 interface=vlan130
```

For a production-style bridge VLAN configuration, define the VLAN membership explicitly before enabling VLAN filtering. The following example assumes ether1 is a tagged trunk and the router's bridge interface is tagged for CPU access.

```routeros
/interface bridge vlan
add bridge=bridge-isp tagged=bridge-isp,ether1 vlan-ids=120
add bridge=bridge-isp tagged=bridge-isp,ether1 vlan-ids=130
```

Enable VLAN filtering:

```routeros
/interface bridge
set bridge-isp vlan-filtering=yes
```

**Verification:**

```routeros
/interface bridge vlan print
/interface vlan print
/ip address print
```

**Expected result:**

* VLAN 120 and VLAN 130 are configured separately.
* Each VLAN has its own gateway.
* Subscriber traffic remains separated at Layer 2.

# 🗁 Lab 03: Configure a PPPoE Server

**Objective:** Create a PPPoE server and authenticate subscribers using local PPP secrets.

**Topology:**

```text
                  [ISP Router]
                       |
                  ether2 / LAN
                       |
                    [Switch]
                       |
                 [Customer Router]
                       |
                   PPPoE Client
```

**Configure the subscriber network:**

```routeros
/ip pool
add name=pppoe-pool ranges=10.20.0.10-10.20.0.100
```

Create a PPP profile:

```routeros
/ppp profile
add name=HOME-30M \
    local-address=10.20.0.1 \
    remote-address=pppoe-pool \
    rate-limit=30M/30M
```

Create a subscriber account:

```routeros
/ppp secret
add name=cust1001 \
    password=StrongLabPassword \
    service=pppoe \
    profile=HOME-30M
```

Configure the PPPoE server:

```routeros
/interface pppoe-server server
add interface=ether2 \
    service-name=ISP-PPPoE \
    default-profile=HOME-30M \
    disabled=no
```

**Configure a customer PPPoE client:**

```routeros
/interface pppoe-client
add name=pppoe-out1 \
    interface=ether1 \
    user=cust1001 \
    password=StrongLabPassword \
    add-default-route=yes \
    use-peer-dns=yes \
    disabled=no
```

**Verification:**

On the ISP router:

```routeros
/ppp active print
/ppp secret print
```

On the customer router:

```routeros
/interface pppoe-client print detail
/ip address print
/ip route print
```

**Expected result:**

* The customer authenticates successfully.
* The PPPoE session receives an IP address.
* The subscriber receives the configured bandwidth profile.

# 🗁 Lab 04: Configure Multiple PPPoE Subscribers

**Objective:** Create multiple subscriber accounts with different service plans.

**Service plan:**

| Username | Profile | Download | Upload |
| --- | --- | ---: | ---: |
| cust1001 | HOME-10M | 10 Mbps | 10 Mbps |
| cust1002 | HOME-30M | 30 Mbps | 30 Mbps |
| cust1003 | HOME-50M | 50 Mbps | 50 Mbps |

Create profiles:

```routeros
/ppp profile
add name=HOME-10M local-address=10.20.0.1 remote-address=pppoe-pool rate-limit=10M/10M

add name=HOME-30M local-address=10.20.0.1 remote-address=pppoe-pool rate-limit=30M/30M

add name=HOME-50M local-address=10.20.0.1 remote-address=pppoe-pool rate-limit=50M/50M
```

Create subscriber accounts:

```routeros
/ppp secret
add name=cust1001 password=LabPass1001 profile=HOME-10M service=pppoe

add name=cust1002 password=LabPass1002 profile=HOME-30M service=pppoe

add name=cust1003 password=LabPass1003 profile=HOME-50M service=pppoe
```

**Verification:**

```routeros
/ppp secret print detail
/ppp active print detail
```

Connect each customer router using its corresponding PPPoE credentials.

**Expected result:**

* Each subscriber authenticates independently.
* Each subscriber receives the correct service profile.
* Bandwidth policies are applied to the respective sessions.

# 🗁 Lab 05: Configure RADIUS Authentication

**Objective:** Understand centralized subscriber authentication using a RADIUS server.

**Topology:**

```text
          [Customer Router]
                  |
             PPPoE Access
                  |
             [MikroTik]
                  |
               RADIUS
                  |
           [Ubuntu Server]
             FreeRADIUS
```

**Lab requirements:**

* One MikroTik router acting as the NAS.
* One Ubuntu Server running FreeRADIUS.
* One customer router acting as a PPPoE client.

**Ubuntu installation:**

```bash
sudo apt update
sudo apt install freeradius freeradius-utils -y
```

Check the service:

```bash
sudo systemctl status freeradius
```

For a basic lab, configure the NAS client in the FreeRADIUS clients configuration.

Example client entry:

```text
client mikrotik {
    ipaddr = 10.10.10.1
    secret = LabSharedSecret
    require_message_authenticator = auto
}
```

Use the actual source IP address of the MikroTik RADIUS requests.

Add a test subscriber to the FreeRADIUS users configuration:

```text
cust1001 Cleartext-Password := "LabPass1001"
    Service-Type = Framed-User
```

Restart FreeRADIUS:

```bash
sudo systemctl restart freeradius
```

Configure the MikroTik router:

```routeros
/radius
add service=ppp \
    address=10.10.10.10 \
    secret=LabSharedSecret \
    authentication-port=1812 \
    accounting-port=1813
```

Enable RADIUS authentication for PPP:

```routeros
/ppp aaa
set use-radius=yes accounting=yes
```

**Test authentication:**

Run FreeRADIUS in debug mode during testing:

```bash
sudo systemctl stop freeradius
sudo freeradius -X
```

Connect the customer PPPoE client and observe the authentication exchange.

**Expected result:**

* The MikroTik sends an authentication request to FreeRADIUS.
* FreeRADIUS verifies the subscriber credentials.
* The RADIUS server returns an Access-Accept or Access-Reject.
* Accounting records are generated when accounting is configured correctly.

# 🗁 Lab 06: Configure DHCP-Based Subscriber Access

**Objective:** Provide subscriber IP addresses using DHCP.

**Topology:**

```text
                 [ISP Router]
                      |
                  [Access VLAN]
                      |
                  [Customer]
                      |
                  DHCP Client
```

Configure the DHCP address pool:

```routeros
/ip pool
add name=dhcp-subscribers ranges=10.30.0.10-10.30.0.200
```

Configure the gateway:

```routeros
/ip address
add address=10.30.0.1/24 interface=ether2
```

Configure the DHCP server:

```routeros
/ip dhcp-server
add name=dhcp-isp \
    interface=ether2 \
    address-pool=dhcp-subscribers \
    disabled=no
```

Configure the DHCP network:

```routeros
/ip dhcp-server network
add address=10.30.0.0/24 \
    gateway=10.30.0.1 \
    dns-server=1.1.1.1,8.8.8.8
```

**Verification:**

```routeros
/ip dhcp-server print
/ip dhcp-server lease print
```

Connect a customer device using DHCP.

**Expected result:**

* The customer receives an IP address.
* The customer receives the correct gateway and DNS configuration.
* The DHCP lease appears on the ISP router.

# 🗁 Lab 07: Configure NAT for Subscriber Connectivity

**Objective:** Allow private-addressed subscribers to access an upstream network through source NAT.

**Addressing plan:**

| Device | Address |
| --- | --- |
| Upstream gateway | 192.0.2.1 |
| ISP router WAN | 192.0.2.2/30 |
| Subscriber network | 10.30.0.0/24 |

Configure the WAN interface:

```routeros
/ip address
add address=192.0.2.2/30 interface=ether1
```

Configure the default route:

```routeros
/ip route
add dst-address=0.0.0.0/0 gateway=192.0.2.1
```

Configure source NAT:

```routeros
/ip firewall nat
add chain=srcnat \
    src-address=10.30.0.0/24 \
    out-interface=ether1 \
    action=masquerade
```

**Verification:**

```routeros
/ip firewall nat print
/ip firewall connection print
/ping 192.0.2.1
```

Test connectivity from a subscriber device.

**Expected result:**

* Subscriber traffic reaches the upstream network.
* The ISP router translates the private source addresses.

# 🗁 Lab 08: Simulate CGNAT

**Objective:** Understand how multiple subscribers share a public IPv4 address through Carrier-Grade NAT.

**Topology:**

```text
 Subscriber A          Subscriber B
 10.20.0.10             10.20.0.11
      |                      |
      +----------+-----------+
                 |
            [ISP Router]
                 |
              CGNAT
                 |
          198.51.100.10
                 |
             Upstream
```

The `198.51.100.0/24` network is reserved for documentation. Use it only in the isolated lab.

Configure a CGNAT-style source NAT rule:

```routeros
/ip firewall nat
add chain=srcnat \
    src-address=10.20.0.0/24 \
    out-interface=ether1 \
    action=src-nat \
    to-addresses=198.51.100.10
```

This demonstrates multiple private subscriber addresses being translated to one shared address.

For a larger lab, experiment with a source NAT address pool:

```routeros
/ip pool
add name=cgnat-public-pool ranges=198.51.100.10-198.51.100.20
```

```routeros
/ip firewall nat
add chain=srcnat \
    src-address=10.20.0.0/24 \
    out-interface=ether1 \
    action=src-nat \
    to-addresses=198.51.100.10-198.51.100.20
```

**Verification:**

```routeros
/ip firewall nat print
/ip firewall connection print
```

Inspect translated connections and observe how multiple private subscribers can share translated public addresses.

**Expected result:**

* Multiple subscribers can access the upstream network.
* Their private source addresses are translated.
* Connection tracking maintains individual translation state.

# 🗁 Lab 09: Configure Bandwidth Management

**Objective:** Apply different bandwidth limits to subscribers.

For PPPoE subscribers, create separate profiles:

```routeros
/ppp profile
add name=PLAN-10M rate-limit=10M/10M
add name=PLAN-30M rate-limit=30M/30M
add name=PLAN-50M rate-limit=50M/50M
```

Assign a profile to a subscriber:

```routeros
/ppp secret
set [find name="cust1001"] profile=PLAN-10M
```

Inspect the subscriber session:

```routeros
/ppp active print detail where name="cust1001"
```

Monitor interface traffic:

```routeros
/interface monitor-traffic ether2
```

**Expected result:**

* Each subscriber receives the configured rate limit.
* Traffic monitoring displays interface throughput.
* Service profiles can be changed without recreating the subscriber account.

# 🗁 Lab 10: Configure Static Routing Between POPs

**Objective:** Connect two ISP Points of Presence using static routing.

**Topology:**

```text
             [POP-01 Router]
                    |
              10.255.0.0/30
                    |
             [POP-02 Router]

     LAN-01: 10.10.1.0/24
     LAN-02: 10.10.2.0/24
```

**Addressing plan:**

| Router | Interface | Address |
| --- | --- | --- |
| POP-01 | WAN | 10.255.0.1/30 |
| POP-02 | WAN | 10.255.0.2/30 |
| POP-01 | LAN | 10.10.1.1/24 |
| POP-02 | LAN | 10.10.2.1/24 |

On POP-01:

```routeros
/ip address
add address=10.255.0.1/30 interface=ether1
add address=10.10.1.1/24 interface=ether2
```

Add a route to POP-02:

```routeros
/ip route
add dst-address=10.10.2.0/24 gateway=10.255.0.2
```

On POP-02:

```routeros
/ip address
add address=10.255.0.2/30 interface=ether1
add address=10.10.2.1/24 interface=ether2
```

Add a route to POP-01:

```routeros
/ip route
add dst-address=10.10.1.0/24 gateway=10.255.0.1
```

**Verification:**

```routeros
/ip route print
/ping 10.10.2.1
```

Test communication between hosts in both LANs.

**Expected result:**

* POP-01 and POP-02 can communicate.
* Each POP can reach the remote subscriber network.

# 🗁 Lab 11: Configure OSPF Between POPs

**Objective:** Replace static routing with dynamic routing using OSPF.

Use the same topology from Lab 10.

On POP-01:

```routeros
/routing ospf instance
add name=ospf-instance-1 router-id=1.1.1.1
```

```routeros
/routing ospf area
add name=backbone area-id=0.0.0.0 instance=ospf-instance-1
```

Configure the OSPF interface template:

```routeros
/routing ospf interface-template
add area=backbone networks=10.255.0.0/30
add area=backbone networks=10.10.1.0/24
```

On POP-02:

```routeros
/routing ospf instance
add name=ospf-instance-1 router-id=2.2.2.2
```

```routeros
/routing ospf area
add name=backbone area-id=0.0.0.0 instance=ospf-instance-1
```

```routeros
/routing ospf interface-template
add area=backbone networks=10.255.0.0/30
add area=backbone networks=10.10.2.0/24
```

**Verification:**

```routeros
/routing ospf neighbor print
/ip route print
```

**Expected result:**

* Both routers establish an OSPF adjacency.
* POP networks are advertised dynamically.
* Remote routes appear in the routing table.

# 🗁 Lab 12: Configure BGP Upstream Peering

**Objective:** Understand basic eBGP peering between an ISP and an upstream provider.

**Topology:**

```text
                 [Upstream ISP]
                   AS 64501
                       |
                 203.0.113.0/30
                       |
                   [Your ISP]
                   AS 64510
                       |
                 Customer Network
                  10.10.0.0/24
```

The AS numbers and addresses are examples for an isolated lab. Documentation prefixes are not suitable for real internet announcements.

**Addressing plan:**

| Device | Address | AS |
| --- | --- | --- |
| Upstream | 203.0.113.1/30 | 64501 |
| Your ISP | 203.0.113.2/30 | 64510 |

Configure the ISP router:

```routeros
/ip address
add address=203.0.113.2/30 interface=ether1
```

Create the BGP connection:

```routeros
/routing bgp connection
add name=upstream \
    local.as=64510 \
    local.role=ebgp \
    remote.as=64501 \
    remote.address=203.0.113.1 \
    local.address=203.0.113.2
```

Configure the upstream router with the corresponding remote AS, local AS, and peer address.

**Verification:**

```routeros
/routing bgp session print
/routing route print
```

For a complete lab, configure address-family negotiation, route filters, and the required network advertisements on both peers.

**Expected result:**

* The BGP session establishes successfully.
* Routes are exchanged according to the configured policies.
* Route filtering controls which prefixes are accepted and advertised.

# 🗁 Lab 13: Configure Dual-WAN Failover

**Objective:** Maintain connectivity when the primary upstream connection fails.

**Topology:**

```text
            [Primary ISP]
                  |
                  |
              [Router]
                  |
                  |
            [Secondary ISP]
```

Configure primary and secondary default routes with different distances:

```routeros
/ip route
add dst-address=0.0.0.0/0 gateway=192.0.2.1 distance=1

add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2
```

This provides basic route preference, but a route may remain active even when the upstream internet path beyond its gateway has failed.

For a more reliable failover lab, investigate recursive routing or RouterOS v7 routing rules and health-check mechanisms.

**Verification:**

```routeros
/ip route print detail
```

Disable the primary upstream path and observe whether the secondary route becomes active.

**Expected result:**

* The primary route is preferred while available.
* The secondary route can provide an alternative path when the primary route becomes unavailable.
* Recovery behavior is verified after restoring the primary path.

# 🗁 Lab 14: Configure Network Monitoring

**Objective:** Monitor router interfaces, subscriber connectivity, and network performance.

Start by inspecting interface status:

```routeros
/interface print
```

Monitor traffic:

```routeros
/interface monitor-traffic ether1
```

Inspect interface statistics:

```routeros
/interface print stats
```

Check CPU and memory usage:

```routeros
/system resource print
```

Inspect active PPPoE subscribers:

```routeros
/ppp active print
```

Inspect DHCP leases:

```routeros
/ip dhcp-server lease print
```

For an external monitoring system, install Zabbix on an Ubuntu Server and configure supported monitoring access on the MikroTik device.

Monitor the following metrics:

| Metric | Purpose |
| --- | --- |
| Router CPU | Detect processing bottlenecks |
| Memory | Identify resource exhaustion |
| Interface throughput | Measure bandwidth utilization |
| Interface errors | Detect physical or link problems |
| PPPoE sessions | Monitor subscriber activity |
| DHCP leases | Monitor address allocation |
| Packet loss | Identify connectivity degradation |
| Latency | Measure network responsiveness |
| Uptime | Track service availability |

**Expected result:**

* Router resource utilization can be inspected.
* Interface traffic and errors can be monitored.
* Subscriber and infrastructure status can be tracked.

# 🗁 Lab 15: Troubleshoot PPPoE Authentication

**Objective:** Diagnose common PPPoE authentication failures.

Create a subscriber account with an incorrect password on the customer router.

Inspect active sessions:

```routeros
/ppp active print
```

Inspect PPP configuration:

```routeros
/ppp secret print detail
```

Inspect router logs:

```routeros
/log print where topics~"ppp"
```

If using RADIUS, inspect the RADIUS server debug output.

Investigate these possible causes:

| Problem | Investigation |
| --- | --- |
| Incorrect username | Verify subscriber credentials |
| Incorrect password | Verify authentication configuration |
| Subscriber disabled | Check account status |
| Wrong service interface | Check PPPoE server interface |
| RADIUS unreachable | Verify routing and firewall rules |
| RADIUS rejection | Inspect server logs and subscriber policy |
| VLAN mismatch | Verify subscriber VLAN membership |
| Session limit reached | Inspect existing active sessions |

**Expected result:**

* Identify the authentication failure.
* Correct the configuration.
* Establish a successful PPPoE session.

# 🗁 Lab 16: Troubleshoot Customer Connectivity

**Objective:** Practice a structured troubleshooting process for a subscriber reporting no internet access.

Use the following troubleshooting sequence:

```text
Customer reports no internet
          |
          v
Check physical connection
          |
          v
Check ONT / ONU status
          |
          v
Check VLAN and access network
          |
          v
Check PPPoE or DHCP session
          |
          v
Check IP address assignment
          |
          v
Check default gateway
          |
          v
Check upstream connectivity
          |
          v
Check DNS resolution
          |
          v
Verify customer internet access
```

Useful RouterOS commands:

```routeros
/interface print
/ip address print
/ip route print
/ip firewall nat print
/ppp active print
/ip dhcp-server lease print
/log print
```

Useful Linux commands:

```bash
ip address
ip route
ping -c 4 1.1.1.1
dig example.com
traceroute 1.1.1.1
```

**Expected result:**

* Identify whether the fault is physical, access-related, authentication-related, routing-related, or DNS-related.
* Record the diagnosis and corrective action.
* Confirm service restoration.

# 🗁 Lab 17: Build a Multi-POP ISP Network

**Objective:** Combine switching, routing, subscriber access, and upstream connectivity into a larger ISP topology.

**Topology:**

```text
                      [Upstream ISP]
                            |
                       [Core Router]
                            |
                    [Core Distribution]
                       /          \
                      /            \
                  [POP-01]       [POP-02]
                     |               |
                [Access SW]     [Access SW]
                  /    \          /    \
                 /      \        /      \
              VLAN120 VLAN130 VLAN120 VLAN130
                |       |       |       |
              PPPoE   PPPoE   PPPoE   PPPoE
              Users   Users   Users   Users
```

**Required configuration:**

* Configure the upstream connection.
* Configure core routing.
* Create POP-to-core links.
* Configure VLANs for subscriber separation.
* Configure PPPoE services.
* Create multiple subscriber profiles.
* Configure RADIUS authentication.
* Configure NAT or the appropriate public addressing model.
* Implement monitoring.
* Configure routing redundancy if required.

**Expected result:**

* Both POPs communicate with the core.
* Subscribers can authenticate from both POPs.
* Subscriber networks remain separated.
* The core can route traffic between the required network segments.

# 🗁 Lab 18: Build an Integrated ISP Environment

**Objective:** Build a complete small-scale ISP simulation by integrating business systems and network infrastructure.

**Suggested architecture:**

```text
                   [Customer / Subscriber]
                            |
                       [Access Layer]
                            |
                    [MikroTik / BNG]
                            |
                 +----------+----------+
                 |          |          |
              RADIUS       DHCP       IPAM
                 |          |          |
                 +----------+----------+
                            |
                       [Core Router]
                            |
                      [Upstream ISP]

       [CRM] -------- [Provisioning API]
                            |
       [Billing] ----- [Service Database]
                            |
       [NMS] --------- [Monitoring System]
```

**Implementation tasks:**

* Create customer and service records.
* Design an IP addressing plan.
* Configure subscriber VLANs.
* Configure PPPoE and DHCP access.
* Integrate FreeRADIUS.
* Create subscriber service profiles.
* Configure IPv4 and IPv6 addressing.
* Implement NAT and CGNAT concepts.
* Configure routing between POPs.
* Establish an upstream routing lab.
* Implement monitoring and alerting.
* Simulate billing-driven activation and suspension.
* Create support tickets for network incidents.
* Document troubleshooting procedures.

**Expected result:**

A working ISP simulation that demonstrates how customer management, subscriber authentication, network provisioning, routing, and monitoring operate together.

# 🗁 Lab Validation Checklist

Use this checklist to evaluate the completed ISP environment.

| Area | Validation |
| --- | --- |
| Network topology | Core and access network are connected |
| IP addressing | Address plan is documented and implemented |
| VLANs | Subscriber networks are separated |
| PPPoE | Subscribers authenticate successfully |
| RADIUS | Centralized authentication works |
| DHCP | Dynamic address assignment works |
| NAT | Private subscribers can reach the upstream network |
| CGNAT | Multiple subscribers can share translated addresses |
| QoS | Bandwidth policies are enforced |
| Static routing | POPs can reach remote networks |
| OSPF | Dynamic routing adjacency works |
| BGP | Upstream peering establishes successfully |
| Failover | Secondary connectivity is tested |
| Monitoring | Infrastructure metrics are visible |
| Troubleshooting | Common subscriber faults can be diagnosed |
| Documentation | Configurations and topology are documented |

# 🗁 Recommended Advanced Exercises

After completing the main labs, extend the environment with the following exercises:

* Implement QinQ for wholesale or multi-tenant subscriber transport.
* Configure IPv6 subscriber connectivity with DHCPv6 Prefix Delegation.
* Implement dual-stack PPPoE access.
* Configure RADIUS accounting and subscriber session reporting.
* Test RADIUS Change of Authorization and Disconnect messages.
* Implement redundant OSPF routing between multiple POPs.
* Configure BGP route filtering and prefix limits.
* Simulate CGNAT port utilization and logging.
* Configure SNMP-based monitoring.
* Integrate Zabbix with MikroTik routers.
* Implement configuration backup automation.
* Test network failure scenarios and measure recovery time.
* Build a centralized subscriber provisioning workflow.

# 🗁 Lab Documentation Template

Document every lab so that configurations can be reproduced and troubleshooting results can be reviewed.

```text
Lab Name:
Lab Number:
Date:
Objective:

Topology:
Devices:
Software Versions:

IP Addressing Plan:
VLAN Configuration:
Routing Configuration:
Subscriber Configuration:

Implementation Steps:

Verification Commands:

Expected Results:

Actual Results:

Problems Encountered:

Troubleshooting Steps:

Final Status:

Additional Notes:
```

# 🗁 Best Practices for ISP Lab Environments

* Maintain a separate lab environment from production infrastructure.
* Use documentation-only IP ranges for simulated public addressing.
* Save router configurations before making major changes.
* Record RouterOS and software versions.
* Use consistent interface and device naming.
* Document VLAN IDs and IP address assignments.
* Test firewall policies before connecting lab devices to external networks.
* Use unique subscriber credentials.
* Keep RADIUS shared secrets and administrative credentials protected.
* Verify routing and NAT behavior with packet captures when necessary.
* Test failure scenarios instead of assuming redundancy works.
* Record expected and actual results for each lab.
* Rebuild labs periodically to reinforce practical knowledge.

# 🗁 Summary

These labs provide practical experience with the technologies commonly used in ISP environments. They cover the path from basic network connectivity to subscriber provisioning, authentication, bandwidth management, routing, upstream connectivity, monitoring, and troubleshooting.

Completing the integrated ISP lab provides a foundation for understanding how an ISP operates its network and delivers services to residential, business, and enterprise customers.
