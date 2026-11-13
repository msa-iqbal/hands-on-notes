# Troubleshooting Workflows

Network troubleshooting is a systematic process of identifying, isolating, and resolving network problems.

In MikroTik RouterOS, troubleshooting involves examining interfaces, IP addressing, routing tables, firewall rules, DNS configuration, DHCP services, wireless connections, and system resources.

A structured troubleshooting workflow helps administrators identify the actual cause of a problem instead of making random configuration changes.

# 🗁 Understanding the Troubleshooting Process

A reliable troubleshooting process follows a logical sequence:

```text
       Identify the Problem
                |
                v
       Gather Information
                |
                v
       Check Physical Connectivity
                |
                v
       Check Interface Status
                |
                v
       Verify IP Configuration
                |
                v
       Verify Routing
                |
                v
       Test Network Connectivity
                |
                v
       Inspect Firewall and Services
                |
                v
       Identify Root Cause
                |
                v
       Apply Corrective Action
                |
                v
       Verify the Solution
                |
                v
       Document the Result
```

The objective is to isolate the failing network layer or service before changing the configuration.

# 🗁 Understanding the OSI-Based Troubleshooting Approach

The OSI model provides a useful framework for locating network problems.

| Layer | Area | Common Problems |
| --- | --- | --- |
| Physical | Cables, ports, signals | Disconnected cable, damaged port |
| Data Link | Ethernet, bridging, VLANs | VLAN mismatch, bridge problems |
| Network | IP addressing, routing | Incorrect IP, missing route |
| Transport | TCP, UDP, ports | Blocked ports, connection failures |
| Session | Session establishment | Session interruptions |
| Presentation | Data representation, encryption | TLS and certificate problems |
| Application | DNS, HTTP, DHCP, SSH | Service configuration failures |

Start with lower-layer connectivity and move toward higher-layer services.

For example, troubleshooting a website connection should begin by checking physical connectivity, IP addressing, routing, and DNS before investigating the web application.

# 🗁 Collecting Initial Information

Before changing anything, understand the reported problem.

Useful questions include:

* Is the problem affecting one device or multiple devices?
* Is the entire network unavailable or only one service?
* Did the problem start after a configuration change?
* Is the connection intermittent or completely unavailable?
* Does the problem affect wired clients, wireless clients, or both?
* Can the router itself access the Internet?
* Are there recent interface, routing, or firewall changes?

Collect the current router configuration and system information.

Display system resources:

```routeros
/system/resource/print
```

Display router identity:

```routeros
/system/identity/print
```

Display system logs:

```routeros
/log/print
```

Export the configuration for review:

```routeros
/export file=troubleshooting-before
```

Configuration exports may contain sensitive information. Store them securely and review sensitive fields before sharing them.

# 🗁 Physical Connectivity Troubleshooting

Physical connectivity problems can prevent all higher-layer communication.

Check Ethernet interface status:

```routeros
/interface/print
```

Inspect a specific Ethernet interface:

```routeros
/interface/ethernet/monitor ether1 once
```

Check:

* Whether the interface is running.
* Negotiated link speed.
* Duplex mode.
* Auto-negotiation status.
* Link partner information, when available.

Common physical problems include:

* Damaged Ethernet cables.
* Loose connectors.
* Incorrect SFP or SFP+ modules.
* Unsupported transceivers.
* Incorrect fiber polarity.
* Faulty switch ports.
* Speed or duplex negotiation problems.

Replace or test suspected physical components before changing unrelated network settings.

# 🗁 Interface Troubleshooting

If the physical link is available but traffic is not passing correctly, inspect the interface configuration and counters.

Display interfaces:

```routeros
/interface/print detail
```

Display interface statistics:

```routeros
/interface/print stats
```

Monitor traffic:

```routeros
/interface/monitor-traffic ether1
```

Inspect interface-specific errors and counters where available.

Important indicators include:

* RX and TX traffic.
* Receive and transmit drops.
* Error counters.
* Link state changes.
* Unexpected traffic patterns.

If an interface is disabled, inspect its configuration before enabling it.

Enable an interface when appropriate:

```routeros
/interface/enable ether1
```

Do not enable interfaces blindly. An interface may have been disabled intentionally for security, maintenance, or topology reasons.

# 🗁 Bridge Troubleshooting

Bridge problems can prevent communication between connected devices.

Display bridge configuration:

```routeros
/interface/bridge/print detail
```

Display bridge ports:

```routeros
/interface/bridge/port/print detail
```

Inspect the bridge host table:

```routeros
/interface/bridge/host/print
```

Check:

* Whether the expected ports belong to the bridge.
* Whether bridge ports are enabled.
* Whether VLAN filtering is configured correctly.
* Whether the expected MAC addresses are learned.
* Whether spanning-tree behavior is blocking a port.
* Whether hardware offloading is available where expected.

A device may have physical connectivity but remain unable to communicate because of an incorrect bridge or VLAN configuration.

# 🗁 VLAN Troubleshooting

Incorrect VLAN configuration is a common cause of connectivity problems in switched networks.

Display bridge VLAN configuration:

```routeros
/interface/bridge/vlan/print detail
```

Display bridge port configuration:

```routeros
/interface/bridge/port/print detail
```

Check:

* VLAN IDs.
* Port PVID values.
* Tagged membership.
* Untagged membership.
* Trunk and access port configuration.
* Bridge VLAN filtering.
* Bridge CPU port membership.

For a VLAN interface that requires router Layer 3 connectivity, verify that the bridge CPU port is included as a tagged member of that VLAN.

Display VLAN interfaces:

```routeros
/interface/vlan/print detail
```

Check whether the VLAN interface is created on the correct bridge and has the expected IP configuration.

A VLAN can pass traffic between switch ports while the router remains unable to access that VLAN if the bridge CPU port is not configured correctly.

# 🗁 IP Address Troubleshooting

Incorrect IP addressing can prevent devices from communicating.

Display configured IP addresses:

```routeros
/ip/address/print detail
```

Check:

* Correct IP address.
* Correct subnet prefix.
* Correct interface.
* Duplicate addresses.
* Expected network membership.
* Whether the address is disabled.

For example, these two addresses belong to the same subnet:

```text
192.168.10.1/24
192.168.10.100/24
```

However, the following address belongs to a different subnet:

```text
192.168.20.100/24
```

If a client uses the wrong subnet mask or default gateway, it may communicate locally but fail to reach remote networks.

# 🗁 DHCP Troubleshooting

If clients are not receiving IP addresses automatically, inspect the DHCP server configuration.

Display DHCP servers:

```routeros
/ip/dhcp-server/print detail
```

Display DHCP leases:

```routeros
/ip/dhcp-server/lease/print detail
```

Display IP pools:

```routeros
/ip/pool/print detail
```

Check:

* DHCP server enabled state.
* Correct serving interface.
* Correct address pool.
* Available addresses in the pool.
* DHCP network configuration.
* Gateway and DNS options.
* VLAN and bridge connectivity.

Inspect DHCP server networks:

```routeros
/ip/dhcp-server/network/print detail
```

If a DHCP server is configured on the wrong interface, clients may not receive addresses even though the DHCP service is enabled.

For additional troubleshooting, use a packet capture to inspect DHCP discovery and offer messages.

# 🗁 DNS Troubleshooting

DNS problems can make Internet access appear unavailable even when IP connectivity is working.

Inspect DNS configuration:

```routeros
/ip/dns/print
```

Check:

* Configured DNS servers.
* Dynamic DNS server settings.
* Whether remote requests are enabled when required.
* Client DNS settings.
* Firewall rules affecting DNS traffic.

Test name resolution from the router:

```routeros
:put [:resolve example.com]
```

Test IP connectivity separately:

```routeros
/ping 8.8.8.8 count=5
```

If IP connectivity works but DNS resolution fails, investigate DNS configuration and reachability instead of immediately changing routing or NAT rules.

# 🗁 Routing Troubleshooting

Routing problems occur when the router does not have a valid path to the destination or selects an unexpected route.

Display the routing table:

```routeros
/ip/route/print detail
```

Inspect route status and gateway reachability:

```routeros
/ip/route/print detail where active
```

Check:

* Default route availability.
* Destination prefixes.
* Gateway addresses.
* Route distance.
* Routing table.
* Route status.
* Recursive gateway resolution.
* Policy routing rules.

Inspect routing rules:

```routeros
/routing/rule/print detail
```

Inspect custom routing tables:

```routeros
/routing/table/print detail
```

A route may exist in the configuration but remain inactive because its gateway cannot be resolved or another required condition is not satisfied.

For policy-based routing, verify that traffic receives the intended routing mark or matches the correct routing rule.

# 🗁 Gateway Troubleshooting

If clients cannot reach external networks, verify that the router can reach its gateway.

Check the default route:

```routeros
/ip/route/print detail where dst-address=0.0.0.0/0
```

Ping the gateway:

```routeros
/ping 192.168.1.1 count=5
```

Replace the example gateway with the actual upstream gateway address.

If the gateway is unreachable, investigate:

* WAN interface status.
* WAN IP configuration.
* VLAN configuration.
* Upstream device status.
* Gateway address correctness.
* ARP resolution.
* Firewall policies.

Inspect the ARP table:

```routeros
/ip/arp/print
```

A gateway that cannot be reached at Layer 2 may indicate a physical, VLAN, or upstream connectivity problem.

# 🗁 NAT Troubleshooting

If the router can access the Internet but LAN clients cannot, inspect NAT configuration.

Display NAT rules:

```routeros
/ip/firewall/nat/print detail
```

For a typical IPv4 source NAT configuration using masquerade:

```routeros
/ip/firewall/nat/add chain=srcnat \
    out-interface=ether1 action=masquerade
```

Verify that:

* The correct WAN interface is selected.
* The source subnet is correct when using a source-address match.
* The NAT rule is enabled.
* The rule is positioned appropriately.
* Traffic actually leaves through the expected interface.

NAT translates addresses; it does not independently permit traffic through the firewall filter.

# 🗁 Firewall Troubleshooting

Firewall rules may block traffic even when routing and NAT are correct.

Inspect filter rules:

```routeros
/ip/firewall/filter/print detail
```

Inspect NAT rules:

```routeros
/ip/firewall/nat/print detail
```

Inspect Mangle rules:

```routeros
/ip/firewall/mangle/print detail
```

Inspect firewall connection tracking:

```routeros
/ip/firewall/connection/print
```

Understand the main filter chains:

| Chain | Purpose |
| --- | --- |
| `input` | Traffic destined for the router |
| `forward` | Traffic routed through the router |
| `output` | Traffic generated by the router |

Check:

* Rule order.
* Source and destination addresses.
* Input and output interfaces.
* Protocol and port conditions.
* Connection-state conditions.
* Address lists.
* Packet and byte counters.

Inspect counters:

```routeros
/ip/firewall/filter/print stats
```

Counters can help identify which rules are matching traffic.

Do not disable the entire firewall as a troubleshooting shortcut. Identify the relevant rule and evaluate its matching conditions.

# 🗁 TCP and UDP Connectivity Troubleshooting

When IP connectivity works but a service remains unavailable, investigate transport-layer connectivity.

Check whether the destination service is listening and whether firewall policies permit the required traffic.

For example, SSH commonly uses TCP port 22 and HTTPS commonly uses TCP port 443.

Inspect firewall rules:

```routeros
/ip/firewall/filter/print detail
```

Inspect active connections:

```routeros
/ip/firewall/connection/print
```

Use a client-side tool such as `nc` on Linux to test a TCP port:

```bash
nc -vz 192.168.10.100 22
```

Test HTTPS connectivity:

```bash
curl -I https://example.com
```

A successful ping does not prove that a TCP or UDP service is available.

# 🗁 Wireless Troubleshooting

Wireless connectivity problems can originate from radio configuration, interference, authentication, or signal quality.

Display wireless interfaces:

```routeros
/interface/wifi/print detail
```

Monitor wireless registration:

```routeros
/interface/wifi/registration-table/print detail
```

Check:

* SSID configuration.
* Security profile.
* Authentication credentials.
* Frequency and channel width.
* Signal strength.
* Noise and interference.
* Client compatibility.
* Regulatory domain settings.

Wireless command paths and available properties depend on the RouterOS wireless package and device configuration.

For legacy wireless interfaces, the relevant commands are under `/interface/wireless`.

# 🗁 VPN Troubleshooting

VPN failures may result from incorrect credentials, incompatible settings, firewall restrictions, or routing problems.

Inspect available VPN interfaces:

```routeros
/interface/print
```

For WireGuard:

```routeros
/interface/wireguard/print detail
```

```routeros
/interface/wireguard/peers/print detail
```

For IPsec:

```routeros
/ip/ipsec/active-peers/print detail
```

```routeros
/ip/ipsec/installed-sa/print detail
```

For PPP-based VPN connections:

```routeros
/ppp/active/print detail
```

Check:

* Peer addresses.
* Authentication credentials.
* Encryption settings.
* Allowed addresses.
* Routes.
* Firewall policies.
* NAT traversal requirements.
* Tunnel interface status.
* Recent logs.

A VPN tunnel can be established while traffic through it still fails because of missing routes, incorrect allowed-address configuration, or firewall restrictions.

# 🗁 PPPoE Troubleshooting

PPPoE problems can occur during discovery, authentication, session establishment, or IP assignment.

Inspect PPPoE client interfaces:

```routeros
/interface/pppoe-client/print detail
```

Inspect active PPP sessions:

```routeros
/ppp/active/print detail
```

Inspect PPP secrets:

```routeros
/ppp/secret/print detail
```

Check:

* Physical or VLAN connectivity.
* PPPoE server availability.
* Service name, when configured.
* Username and password.
* PPP profile.
* IP address assignment.
* MTU and MRU compatibility.
* Firewall rules affecting PPPoE traffic.

Review logs:

```routeros
/log/print
```

A PPPoE client that cannot establish a session may have a Layer 2 discovery problem or an authentication problem. Use logs and packet captures to distinguish between them.

# 🗁 Monitoring CPU and Memory Problems

High CPU utilization and memory pressure can affect routing, firewall processing, and overall network performance.

Inspect system resources:

```routeros
/system/resource/print
```

Monitor resources continuously:

```routeros
/system/resource/monitor
```

Identify CPU-consuming processes:

```routeros
/tool/profile
```

Check:

* CPU utilization.
* Memory usage.
* Free storage.
* CPU load by process.
* Unexpected resource consumption.
* Recent configuration changes.

Potential causes include excessive packet processing, traffic spikes, inefficient firewall rules, routing instability, and resource-intensive services.

# 🗁 Packet Capture Troubleshooting

Packet capture helps identify where traffic is being lost or modified.

RouterOS provides a packet sniffer:

```routeros
/tool/sniffer/print
```

Configure a capture interface:

```routeros
/tool/sniffer/set filter-interface=ether1
```

Start capturing:

```routeros
/tool/sniffer/start
```

Stop capturing:

```routeros
/tool/sniffer/stop
```

Review captured packets:

```routeros
/tool/sniffer/packet/print
```

Packet captures can help investigate:

* DHCP discovery and offer failures.
* DNS queries and responses.
* TCP connection establishment.
* ARP resolution.
* VLAN tagging.
* Unexpected retransmissions.
* Traffic arriving on the wrong interface.

For more detailed analysis, capture traffic to a file and inspect it with Wireshark.

Be aware that hardware offloading and traffic-processing paths can affect what is visible to a router's packet sniffer.

# 🗁 Troubleshooting with Ping

Ping is one of the simplest tools for testing IP connectivity.

Test the router's loopback address:

```routeros
/ping 127.0.0.1 count=5
```

Test a directly connected device:

```routeros
/ping 192.168.10.100 count=5
```

Test an upstream gateway:

```routeros
/ping 192.168.1.1 count=5
```

Test an external IP address:

```routeros
/ping 8.8.8.8 count=5
```

Test a hostname:

```routeros
/ping example.com count=5
```

Interpret the results carefully:

* Successful replies indicate that the tested destination responds to ICMP.
* Timeouts may indicate packet loss, filtering, routing problems, or an unavailable destination.
* High response times may indicate congestion or a longer network path.
* Hostname resolution failure may indicate DNS problems.

Ping alone cannot identify the exact cause of a connectivity failure.

# 🗁 Troubleshooting with Traceroute

Traceroute identifies the network hops along a path toward a destination.

Run:

```routeros
/tool/traceroute 8.8.8.8
```

Specify a source address when needed:

```routeros
/tool/traceroute 8.8.8.8 src-address=192.168.10.1
```

Traceroute can help identify:

* Unexpected routing paths.
* Upstream connectivity problems.
* Routing loops.
* High-latency network segments.
* Where responses stop appearing.

Some routers filter or rate-limit traceroute responses. A missing response from one hop does not necessarily indicate that forwarding has stopped at that router.

# 🗁 Troubleshooting High Latency and Packet Loss

High latency and packet loss can affect application performance even when basic connectivity works.

Measure latency:

```routeros
/ping 8.8.8.8 count=20
```

Monitor interface traffic:

```routeros
/interface/monitor-traffic ether1
```

Inspect interface statistics:

```routeros
/interface/print stats
```

Check CPU utilization:

```routeros
/tool/profile
```

Investigate:

* Interface congestion.
* Queue configuration.
* Wireless interference.
* Physical link errors.
* Upstream network congestion.
* Routing changes.
* CPU saturation.
* Excessive packet drops.

For QoS-related problems, inspect queue configuration:

```routeros
/queue/simple/print stats
```

```routeros
/queue/tree/print stats
```

Remember that a high ping response time does not always mean the router itself is overloaded. The delay may occur elsewhere along the network path.

# 🗁 Troubleshooting Load Balancing and Failover

Multi-WAN configurations require additional investigation because traffic may follow different routing paths.

Inspect routing tables:

```routeros
/ip/route/print detail
```

Inspect routing rules:

```routeros
/routing/rule/print detail
```

Inspect Mangle rules:

```routeros
/ip/firewall/mangle/print detail
```

Check:

* WAN interface status.
* Default routes.
* Recursive gateway checks.
* Routing marks.
* Connection marks.
* Custom routing tables.
* WAN-specific NAT rules.
* Return-path symmetry.
* Backup route availability.

FastTrack can interfere with policy routing and packet marking when traffic that requires special processing is FastTracked.

For PCC configurations, verify that connections are marked correctly and that each routing table has the intended primary and backup routes.

# 🗁 A Systematic Client Connectivity Workflow

When a client cannot access the Internet, troubleshoot in the following order.

**Check the physical connection**

Verify the cable, switch port, wireless association, and interface status.

**Check the client IP configuration**

Confirm that the client has:

* A valid IP address.
* The correct subnet mask.
* The correct default gateway.
* Valid DNS server addresses.

**Check local gateway connectivity**

From the client, ping its default gateway.

If the gateway is unreachable, investigate local Layer 2 connectivity, VLANs, addressing, and firewall rules.

**Check router Internet connectivity**

From MikroTik:

```routeros
/ping 8.8.8.8 count=5
```

If the router cannot reach the external IP, investigate WAN connectivity, routing, and upstream access.

**Check client Internet connectivity**

If the router can reach the Internet but the client cannot, inspect:

* Forward firewall rules.
* Source NAT.
* Client default gateway.
* Routing and policy routing.

**Check DNS resolution**

From MikroTik:

```routeros
:put [:resolve example.com]
```

From a Linux client:

```bash
nslookup example.com
```

If IP connectivity works but name resolution fails, investigate DNS.

**Test application connectivity**

From a Linux client:

```bash
curl -I https://example.com
```

If DNS and IP connectivity work but the application fails, investigate TCP connectivity, TLS, firewall policies, and the application service.

# 🗁 Common Network Problems and Their Likely Causes

| Problem | Possible Causes |
| --- | --- |
| Interface is down | Cable, transceiver, port, or disabled interface |
| Client receives no IP | DHCP server, VLAN, bridge, or address pool |
| Client can access LAN but not Internet | Default gateway, routing, NAT, or firewall |
| Router cannot reach gateway | WAN configuration, VLAN, ARP, or upstream failure |
| IP works but domain names fail | DNS configuration or DNS connectivity |
| Ping works but application fails | TCP port, firewall, TLS, or application issue |
| Intermittent connectivity | Packet loss, congestion, physical errors, or wireless interference |
| VPN connects but traffic fails | Routes, allowed addresses, firewall, or return path |
| One WAN fails in PCC | Routing marks, custom routes, NAT, or gateway checks |
| High CPU during traffic | Packet processing, firewall, queues, or hardware limitations |

These are starting points for investigation, not definitive diagnoses.

# 🗁 Safe Troubleshooting Practices

* Record the original problem before making changes.
* Export the current configuration before major modifications.
* Change one relevant setting at a time.
* Use a maintenance window for disruptive changes.
* Preserve administrative access while modifying firewall or management settings.
* Avoid disabling firewall rules without understanding their purpose.
* Avoid resetting the router as an initial troubleshooting step.
* Verify changes immediately after applying them.
* Keep a record of successful and unsuccessful tests.
* Revert changes that do not resolve the problem.
* Remove temporary diagnostic rules and services after testing.
* Protect configuration exports and packet captures.

# 🗁 Troubleshooting Documentation Template

For consistent network operations, document incidents using a standard structure.

| Field | Description |
| --- | --- |
| Incident ID | Unique incident reference |
| Date and time | When the problem occurred |
| Device | Affected router or network device |
| Network segment | Affected VLAN, subnet, or interface |
| Problem | Description of the reported issue |
| Symptoms | Observable behavior |
| Initial findings | Results of preliminary checks |
| Root cause | Confirmed technical cause |
| Corrective action | Changes made to resolve the issue |
| Verification | Tests confirming recovery |
| Prevention | Steps to reduce recurrence |

A clear troubleshooting record helps administrators identify recurring problems, understand previous fixes, and improve network reliability.
