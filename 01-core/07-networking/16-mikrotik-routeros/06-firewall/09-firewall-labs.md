# Firewall Labs

Practical firewall labs help develop the skills needed to configure, secure, and troubleshoot MikroTik RouterOS firewalls.

This section combines the firewall concepts covered in the previous documents:

* Firewall fundamentals.
* Filter rules and chains.
* Connection tracking.
* Address lists.
* NAT and firewall interaction.
* Raw, mangle, and filter.
* Router hardening.

All examples target RouterOS v7.

The labs use a small network environment that can be implemented with MikroTik routers, MikroTik CHR, or a network simulator.

# 🗁 Lab Environment

The following topology will be used throughout the exercises.

```text
                    Internet
                       |
                       |
                  ether1 (WAN)
                       |
                +---------------+
                | MikroTik R1   |
                | RouterOS v7   |
                +---------------+
                       |
                 bridge-LAN
                       |
              -------------------
              |                 |
          PC-CLIENT         SERVER-01
        192.168.10.10      192.168.10.100
```

**Network addressing**

| Device | Interface | IP Address |
| --- | --- | --- |
| MikroTik R1 | WAN | `203.0.113.10/24` |
| MikroTik R1 | LAN | `192.168.10.1/24` |
| PC-CLIENT | Ethernet | `192.168.10.10/24` |
| SERVER-01 | Ethernet | `192.168.10.100/24` |
| Upstream gateway | WAN network | `203.0.113.1` |

The public addresses belong to documentation ranges and are placeholders for lab use.

**Assumptions**

* `ether1` is connected to the WAN.
* `ether2` and `ether3` are connected to the LAN.
* `bridge-LAN` contains the LAN interfaces.
* The router has a working upstream gateway.
* The server provides an HTTP service on TCP port `80`.
* The lab router is isolated from production networks.

# 🗁 Prepare the Lab Router

Create the LAN bridge:

```routeros
/interface/bridge
add name=bridge-LAN comment="Lab LAN bridge"
```

Add LAN interfaces:

```routeros
/interface/bridge/port
add bridge=bridge-LAN interface=ether2
add bridge=bridge-LAN interface=ether3
```

Configure the LAN address:

```routeros
/ip/address
add address=192.168.10.1/24 interface=bridge-LAN comment="Lab LAN gateway"
```

Configure the WAN address:

```routeros
/ip/address
add address=203.0.113.10/24 interface=ether1 comment="Lab WAN address"
```

Configure the default route:

```routeros
/ip/route
add dst-address=0.0.0.0/0 gateway=203.0.113.1
```

Configure source NAT:

```routeros
/ip/firewall/nat
add chain=srcnat out-interface=ether1 action=masquerade comment="Lab WAN masquerade"
```

These commands assume a clean lab configuration. If the router already contains bridges, addresses, routes, or NAT rules, inspect them before applying the examples.

# 🗁 Lab: Configure Basic Firewall Rules

**Objective:** Allow established connections, drop invalid traffic, permit LAN clients to access the internet, and block other forwarded traffic.

**Step: Allow established and related connections**

```routeros
/ip/firewall/filter
add chain=forward connection-state=established,related action=accept comment="Allow established and related"
```

**Step: Drop invalid connections**

```routeros
/ip/firewall/filter
add chain=forward connection-state=invalid action=drop comment="Drop invalid connections"
```

**Step: Allow LAN-to-WAN traffic**

```routeros
/ip/firewall/filter
add chain=forward in-interface=bridge-LAN out-interface=ether1 action=accept comment="Allow LAN to WAN"
```

**Step: Drop other forwarded traffic**

```routeros
/ip/firewall/filter
add chain=forward action=drop comment="Drop other forwarding"
```

**Verification**

From PC-CLIENT:

```bash
ping 192.168.10.1
ping 203.0.113.1
```

Test internet access using a reachable external destination.

Inspect the filter counters:

```routeros
/ip/firewall/filter/print stats
```

Expected result:

* The LAN client can reach the router.
* LAN-to-WAN forwarding is permitted.
* Other forwarded traffic is dropped by the final rule.

The input chain is separate and requires its own policy for traffic destined for the router.

# 🗁 Lab: Protect Router Management Access

**Objective:** Allow router management only from the trusted management network.

Assume the trusted management subnet is `192.168.10.0/24`.

Create an address list:

```routeros
/ip/firewall/address-list
add list=trusted-management address=192.168.10.0/24 comment="Trusted management LAN"
```

Create an input-chain policy:

```routeros
/ip/firewall/filter
add chain=input connection-state=established,related action=accept comment="Allow established and related"
add chain=input connection-state=invalid action=drop comment="Drop invalid"
add chain=input protocol=icmp action=accept comment="Allow ICMP"
add chain=input src-address-list=trusted-management protocol=tcp dst-port=22,8291 action=accept comment="Allow trusted SSH and WinBox"
add chain=input in-interface=bridge-LAN protocol=udp dst-port=67 action=accept comment="Allow DHCP server requests"
add chain=input in-interface=bridge-LAN protocol=udp dst-port=53 action=accept comment="Allow LAN DNS UDP"
add chain=input in-interface=bridge-LAN protocol=tcp dst-port=53 action=accept comment="Allow LAN DNS TCP"
add chain=input action=drop comment="Drop other router access"
```

This example assumes DHCP and DNS services are provided by the router and SSH and WinBox are the intended management methods.

If those services are not enabled, remove their corresponding permissions. Add any other required router services before the final drop rule.

**Verification**

Inspect the rules:

```routeros
/ip/firewall/filter/print
```

Test SSH or WinBox access from the trusted LAN.

If possible, test management access from an untrusted network and verify that it is blocked.

# 🗁 Lab: Use Address Lists to Block Sources

**Objective:** Block selected source addresses using a reusable address list.

Create an address list:

```routeros
/ip/firewall/address-list
add list=blocked-sources address=198.51.100.50 comment="Blocked test source"
add list=blocked-sources address=198.51.100.60 comment="Second blocked test source"
```

Add a raw firewall rule:

```routeros
/ip/firewall/raw
add chain=prerouting src-address-list=blocked-sources action=drop comment="Drop blocked sources early"
```

The raw rule drops matching incoming packets before connection tracking.

**Verification**

```routeros
/ip/firewall/address-list/print
/ip/firewall/raw/print stats
```

For a meaningful test, generate matching traffic from a test machine using an address in the blocked list.

The example addresses are documentation addresses and are not normally assigned to public hosts.

# 🗁 Lab: Configure Port Forwarding

**Objective:** Publish the internal web server through the router's WAN address.

Assume:

| Component | Value |
| --- | --- |
| WAN interface | `ether1` |
| Server address | `192.168.10.100` |
| External port | `8080` |
| Internal port | `80` |
| Protocol | TCP |

Configure destination NAT:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.100 to-ports=80 comment="Forward HTTP to internal server"
```

Allow the forwarded connection:

```routeros
/ip/firewall/filter
add chain=forward in-interface=ether1 connection-nat-state=dstnat protocol=tcp dst-address=192.168.10.100 dst-port=80 action=accept comment="Allow forwarded HTTP"
```

The permission must appear before any broader forward-chain drop rule.

**Verification**

From an external test machine, access:

```text
http://203.0.113.10:8080
```

Confirm that:

* The destination NAT counter increases.
* The forward filter rule counter increases.
* The internal web server receives the request.
* The server responds successfully.

The server must have a working HTTP service, a correct default gateway, and an appropriate local firewall policy.

# 🗁 Lab: Configure Connection Tracking

**Objective:** Observe how RouterOS tracks network connections.

Generate traffic from PC-CLIENT to an external server.

Display connection tracking entries:

```routeros
/ip/firewall/connection/print
```

Display detailed information:

```routeros
/ip/firewall/connection/print detail
```

Inspect connection properties such as:

* Original source address.
* Original destination address.
* Protocol.
* Connection state.
* NAT-related state.
* Timeout.

Create a filter rule that drops invalid forwarded connections:

```routeros
/ip/firewall/filter
add chain=forward connection-state=invalid action=drop comment="Drop invalid forwarding"
```

**Verification**

Observe the connection table while generating new connections.

Compare connection states and NAT information for different types of traffic.

# 🗁 Lab: Configure Mangle Packet Marking

**Objective:** Mark LAN packets for traffic classification.

Create a packet-marking rule:

```routeros
/ip/firewall/mangle
add chain=prerouting in-interface=bridge-LAN src-address=192.168.10.0/24 action=mark-packet new-packet-mark=lan-traffic passthrough=no comment="Mark LAN packets"
```

Generate traffic from a LAN client.

Inspect mangle counters:

```routeros
/ip/firewall/mangle/print stats
```

The packet mark can be used by appropriate queue configurations for traffic management.

For advanced QoS deployments, consider connection marking and packet marking together, and review FastTrack compatibility.

# 🗁 Lab: Configure a Basic Router Hardening Policy

**Objective:** Restrict unnecessary services and limit administrative access.

Inspect enabled services:

```routeros
/ip/service/print
```

Disable unused services:

```routeros
/ip/service
disable telnet
disable ftp
disable www
disable api
disable api-ssl
```

Restrict WinBox and SSH to the trusted LAN:

```routeros
/ip/service
set winbox address=192.168.10.0/24
set ssh address=192.168.10.0/24
```

Restrict Neighbor Discovery:

```routeros
/ip/neighbor/discovery-settings
set discover-interface-list=LAN
```

This assumes an interface list named `LAN` already exists and contains the trusted LAN interfaces.

**Verification**

```routeros
/ip/service/print
/ip/neighbor/discovery-settings/print
```

Confirm that required management access still works before ending the maintenance session.

# 🗁 Lab: Troubleshoot Firewall Rules

**Objective:** Identify why a LAN client cannot access the internet.

Use the following troubleshooting sequence.

**Inspect interface status**

```routeros
/interface/print
```

**Inspect IP addressing**

```routeros
/ip/address/print
```

**Inspect routing**

```routeros
/ip/route/print
```

**Inspect NAT rules**

```routeros
/ip/firewall/nat/print stats
```

**Inspect filter rules**

```routeros
/ip/firewall/filter/print stats
```

**Inspect connection tracking**

```routeros
/ip/firewall/connection/print
```

**Inspect DNS configuration**

```routeros
/ip/dns/print
```

Check the following possibilities:

* The LAN client has an incorrect IP address or gateway.
* The router has no valid default route.
* The WAN interface is disconnected.
* The source NAT rule does not match the outgoing interface.
* The forward chain blocks LAN-to-WAN traffic.
* The DNS resolver is unavailable.
* A more general firewall rule matches before the intended permission.

Test IP connectivity separately from DNS resolution to identify where the failure occurs.

# 🗁 Firewall Rule Inspection and Counter Management

Rule counters help determine whether packets match the intended rules.

Display filter statistics:

```routeros
/ip/firewall/filter/print stats
```

Display NAT statistics:

```routeros
/ip/firewall/nat/print stats
```

Display raw statistics:

```routeros
/ip/firewall/raw/print stats
```

Display mangle statistics:

```routeros
/ip/firewall/mangle/print stats
```

Reset filter counters when starting a controlled test:

```routeros
/ip/firewall/filter/reset-counters
```

Reset NAT counters:

```routeros
/ip/firewall/nat/reset-counters
```

Resetting counters does not remove firewall rules or connection-tracking entries.

# 🗁 Lab Assessment

Complete the following practical tasks independently.

* [ ] Configure a basic input-chain policy.
* [ ] Configure a basic forward-chain policy.
* [ ] Permit established and related connections.
* [ ] Drop invalid connections.
* [ ] Configure source NAT for LAN internet access.
* [ ] Create an address list for blocked sources.
* [ ] Block unwanted traffic using raw prerouting.
* [ ] Configure destination NAT for an internal web server.
* [ ] Permit the forwarded service through the forward chain.
* [ ] Observe connection-tracking entries.
* [ ] Mark selected traffic using mangle.
* [ ] Restrict management services to trusted addresses.
* [ ] Verify firewall counters.
* [ ] Troubleshoot a deliberately misconfigured rule.
* [ ] Save a backup of the final configuration.

# 🗁 Final Project: Secure Small Office Router

Build a complete small-office firewall configuration with the following requirements:

**Network design**

| Component | Configuration |
| --- | --- |
| WAN | `ether1` |
| LAN | `bridge-LAN` |
| LAN subnet | `192.168.10.0/24` |
| Router LAN address | `192.168.10.1` |
| Web server | `192.168.10.100` |
| Management network | `192.168.10.0/24` |

**Security requirements**

* Allow LAN clients to access the internet.
* Permit established and related connections.
* Drop invalid connections.
* Block unsolicited WAN access to router management services.
* Permit SSH and WinBox only from trusted management addresses.
* Publish the internal web server through destination NAT.
* Allow only the required forwarded web service.
* Restrict Neighbor Discovery to the LAN.
* Disable unused management services.
* Create and securely store a configuration backup.

**Expected deliverables**

* Working network connectivity.
* Correct source NAT configuration.
* Working destination NAT.
* Appropriate input and forward firewall policies.
* Restricted management access.
* Verified firewall counters.
* Documented configuration and troubleshooting results.

# 🗁 Best Practices

* Perform labs in CHR, GNS3, EVE-NG, or isolated physical equipment.
* Understand the existing configuration before adding rules.
* Keep rule comments descriptive.
* Test firewall changes from both trusted and untrusted networks.
* Check counters rather than assuming rules are matching.
* Keep a recovery method available when changing management policies.
* Save backups before major configuration changes.
* Avoid copying example firewall configurations into production without reviewing their assumptions.
* Document the final firewall policy.

# 🗁 Knowledge Check

* What is the difference between the input and forward chains?
* Why should established and related connections usually be accepted?
* What does connection tracking provide?
* Why does destination NAT require an appropriate forward-chain permission?
* What is the purpose of a raw firewall rule?
* How can address lists simplify firewall management?
* What is the purpose of mangle packet marking?
* Why should management services be restricted?
* How can firewall counters help diagnose connectivity problems?
* What should be checked before deploying a firewall configuration to production?

# 🗁 References

* MikroTik RouterOS Documentation — Firewall: <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall>
* MikroTik RouterOS Documentation — Connection Tracking: <https://help.mikrotik.com/docs/spaces/ROS/pages/130220087/Connection+tracking>
* MikroTik RouterOS Documentation — Packet Flow: <https://help.mikrotik.com/docs/spaces/ROS/pages/328227/Packet+Flow>
* MikroTik RouterOS Documentation — Services: <https://help.mikrotik.com/docs/spaces/ROS/pages/103841820/Services>
