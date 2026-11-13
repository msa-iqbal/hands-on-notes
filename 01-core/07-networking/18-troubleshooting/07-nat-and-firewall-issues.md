# NAT and Firewall Issues

NAT and firewall troubleshooting focuses on identifying problems that prevent network traffic from being translated, permitted, tracked, or forwarded correctly.

Network Address Translation (NAT) modifies IP addresses and, in some cases, transport-layer ports as packets pass through a router or firewall.

A firewall controls network traffic according to configured security policies.

Although NAT and firewall functions are often implemented on the same device, they perform different tasks:

* NAT modifies packet addressing information.
* A firewall permits, rejects, or drops traffic according to security rules.
* Connection tracking maintains information about network connections and their states.

Common symptoms include:

* Clients cannot access the Internet.
* Port forwarding does not work.
* Internal services are inaccessible from external networks.
* Some applications work while others fail.
* Connections establish but stop transferring data.
* Traffic works in one direction but not the other.
* VPN connections fail because of NAT or firewall policies.
* Packets are dropped unexpectedly.

# 🗁 Understanding NAT

NAT translates IP addresses between different network addressing domains.

Common NAT types include:

| NAT Type | Description |
| --- | --- |
| SNAT | Changes the source IP address |
| DNAT | Changes the destination IP address |
| PAT | Translates IP addresses and transport-layer ports |
| Masquerade | Dynamically selects an outgoing interface address for source NAT |
| Static NAT | Maintains a fixed address mapping |
| CGNAT | Allows an ISP to share public IPv4 addresses among customers |

Example:

```text
Private Client
192.168.1.100
      |
      v
Router with NAT
LAN: 192.168.1.1
WAN: 203.0.113.10
      |
      v
Internet Server
198.51.100.20
```

When the client connects to an Internet server, the router may translate the source address:

```text
Before NAT:
192.168.1.100:50000 → 198.51.100.20:443

After NAT:
203.0.113.10:40001 → 198.51.100.20:443
```

The router maintains a translation entry so that return traffic can be associated with the original connection.

# 🗁 Understanding Firewall Behavior

A firewall evaluates traffic against a set of rules.

Common firewall actions include:

| Action | Meaning |
| --- | --- |
| Accept | Permit the packet |
| Drop | Silently discard the packet |
| Reject | Discard the packet and send an error response |
| Log | Record information about matching traffic |
| Jump | Continue processing in another rule chain |

Firewall rules may evaluate:

* Source IP address.
* Destination IP address.
* Source port.
* Destination port.
* Transport protocol.
* Incoming interface.
* Outgoing interface.
* Connection state.
* Packet marks.
* Interface or address lists.

A firewall rule can match traffic correctly but still produce unexpected results if rule order or chain processing is misunderstood.

# 🗁 Connection Tracking

Connection tracking maintains information about active or recently observed network flows.

Common connection states include:

| State | Description |
| --- | --- |
| NEW | A connection is being initiated |
| ESTABLISHED | Traffic belongs to an established connection |
| RELATED | Traffic is associated with an existing connection |
| INVALID | The packet cannot be associated with a valid tracked connection |
| UNTRACKED | Connection tracking is not being applied |

State names and behavior can vary between platforms.

For TCP, connection tracking observes connection establishment and subsequent packets. UDP is also tracked using flow information and timeouts, even though UDP itself is connectionless.

Problems with connection tracking can affect:

* NAT translations.
* Stateful firewall rules.
* Return traffic.
* Long-running sessions.
* High-connection-rate workloads.

# 🗁 Common NAT and Firewall Symptoms

| Symptom | Possible cause |
| --- | --- |
| LAN clients cannot access the Internet | Missing or incorrect source NAT |
| Public service is inaccessible | Incorrect DNAT or forwarding rule |
| Port forwarding works internally but not externally | Hairpin NAT issue or upstream filtering |
| Some connections work, others fail | Rule ordering or connection-tracking issue |
| Return traffic is dropped | Stateful firewall or asymmetric routing |
| Router can access the Internet but clients cannot | Forward-chain or NAT configuration issue |
| VPN traffic fails | Missing NAT exemption or firewall permission |
| Connections stop after a short time | Connection timeout or state-tracking issue |
| Traffic is unexpectedly translated | Incorrect NAT rule matching |
| Public access fails despite correct forwarding | CGNAT, upstream NAT, or provider filtering |

# 🗁 Troubleshooting Methodology

Use a structured process to isolate the problem.

**Identify the traffic flow**

Record:

* Source IP address.
* Destination IP address.
* Protocol.
* Source and destination ports.
* Incoming interface.
* Expected outgoing interface.
* Expected NAT translation.
* Expected firewall action.

Example:

```text
Source:      192.168.1.100
Destination: 198.51.100.20
Protocol:    TCP
Destination port: 443
Expected action: Allow
Expected NAT: Source address translation
```

**Verify basic connectivity**

Test the local gateway:

```bash
ping -c 4 192.168.1.1
```

Test the external destination:

```bash
ping -c 4 1.1.1.1
```

A failed ping does not prove that the destination is unreachable because ICMP may be filtered. Test the actual application service as well.

**Inspect routing**

```bash
ip route
ip route get 1.1.1.1
```

Confirm that traffic has a valid route toward its destination.

**Inspect firewall rules**

Check rule order, matching conditions, counters, and logging.

**Inspect NAT translations**

Verify whether the expected translation is being created.

**Capture packets**

Compare packet headers before and after the router.

**Inspect return traffic**

Verify that replies return through a path that can correctly process the connection state and NAT translation.

# 🗁 Linux Firewall Troubleshooting with nftables

Modern Linux distributions commonly use nftables as their packet-filtering framework.

**Display the active ruleset**

```bash
sudo nft list ruleset
```

**Display table information**

```bash
sudo nft list tables
```

**Inspect a specific table**

```bash
sudo nft list table inet filter
```

**Inspect chain rules and counters**

```bash
sudo nft -a list ruleset
```

The `-a` option displays rule handles, which can help identify individual rules.

Check:

* Whether the expected table exists.
* Whether the relevant chain exists.
* Whether the chain is attached to the correct hook.
* Whether the chain has an appropriate policy.
* Whether an earlier rule drops the packet.
* Whether counters increase when traffic is generated.

A rule's existence does not guarantee that it is evaluated for the traffic being investigated.

# 🗁 Linux Firewall Troubleshooting with iptables

Some Linux environments still use iptables directly or through compatibility layers.

**Display filter rules**

```bash
sudo iptables -L -n -v
```

**Display NAT rules**

```bash
sudo iptables -t nat -L -n -v
```

**Display rules with line numbers**

```bash
sudo iptables -L -n -v --line-numbers
```

**Display IPv6 firewall rules**

```bash
sudo ip6tables -L -n -v
```

Common chains:

| Chain | Purpose |
| --- | --- |
| INPUT | Traffic destined for the local machine |
| OUTPUT | Traffic generated by the local machine |
| FORWARD | Traffic passing through the machine |
| PREROUTING | Processing before routing decisions |
| POSTROUTING | Processing after routing decisions |

For NAT, commonly used chains include:

* PREROUTING for destination translation.
* OUTPUT for locally generated traffic.
* POSTROUTING for source translation.

When troubleshooting, verify which chain handles the packet instead of assuming all traffic passes through INPUT.

# 🗁 Linux Connection Tracking Diagnostics

The `conntrack` utility can inspect connection-tracking entries when the relevant kernel support and userspace package are available.

**Display connection entries**

```bash
sudo conntrack -L
```

**Display TCP connections**

```bash
sudo conntrack -L -p tcp
```

**Monitor connection events**

```bash
sudo conntrack -E
```

**Filter by destination address**

```bash
sudo conntrack -L -d 198.51.100.20
```

Look for:

* Whether a connection-tracking entry is created.
* Original and reply tuples.
* NAT-translated addresses and ports.
* Connection state.
* Unexpected connection expiration.

A missing entry may indicate that traffic is not reaching the tracking point, is being excluded from tracking, or is being discarded before an entry is established.

# 🗁 NAT Troubleshooting on Linux

Inspect NAT rules:

```bash
sudo nft list ruleset
```

For iptables-based systems:

```bash
sudo iptables -t nat -L -n -v
```

Check the following:

* Does the source address match the NAT rule?
* Is the outgoing interface correct?
* Is the correct translation address selected?
* Is the NAT rule being reached?
* Is the translated traffic leaving the router?
* Does return traffic reach the router?
* Is another NAT device present upstream?

For a Linux router using source NAT, the packet path normally includes routing and forwarding checks as well as POSTROUTING NAT processing.

# 🗁 MikroTik RouterOS v7 Firewall Diagnostics

RouterOS provides firewall filter, NAT, and connection-tracking information.

**Display filter rules**

```routeros
/ip firewall filter print detail
```

**Display NAT rules**

```routeros
/ip firewall nat print detail
```

**Display connection-tracking entries**

```routeros
/ip firewall connection print detail
```

**Inspect firewall counters**

```routeros
/ip firewall filter print stats
/ip firewall nat print stats
```

**Inspect address lists**

```routeros
/ip firewall address-list print detail
```

**Inspect interface information**

```routeros
/interface print detail
```

Check:

* Are packets reaching the expected chain?
* Are rule counters increasing?
* Is a drop rule matching the traffic?
* Is the NAT rule placed correctly?
* Does the source address match the intended subnet?
* Is the outgoing interface correct?
* Is connection tracking enabled?
* Does the connection table show the expected translation?

RouterOS uses separate firewall chains for different traffic paths:

| Chain | Purpose |
| --- | --- |
| input | Traffic destined for the router |
| forward | Traffic passing through the router |
| output | Traffic generated by the router |

For example, a LAN client accessing the Internet normally traverses the forward chain, not the input chain.

# 🗁 MikroTik Source NAT and Masquerade Issues

Source NAT is commonly used when private clients access external networks.

Example masquerade rule:

```routeros
/ip firewall nat add chain=srcnat \
    out-interface=ether1 \
    action=masquerade
```

Common problems:

* Incorrect outgoing interface.
* Wrong source subnet.
* NAT rule placed after a conflicting rule.
* Missing route to the Internet.
* Firewall forward chain drops traffic.
* Upstream provider blocks or does not route the public address.
* Router is behind another NAT device.

Inspect the active rules:

```routeros
/ip firewall nat print detail
```

Verify the selected route:

```routeros
/ip route print detail
```

Masquerade is useful for dynamically assigned interface addresses. For stable public addresses, an explicit `src-nat` rule may be more appropriate.

# 🗁 MikroTik Destination NAT and Port Forwarding

Destination NAT redirects traffic addressed to a public IP and port toward an internal service.

Example:

```text
Public address: 203.0.113.10
Public port:    8443
Internal server: 192.168.1.50
Internal port:  443
Protocol:       TCP
```

Example RouterOS rule:

```routeros
/ip firewall nat add chain=dstnat \
    in-interface=ether1 \
    protocol=tcp \
    dst-port=8443 \
    action=dst-nat \
    to-addresses=192.168.1.50 \
    to-ports=443
```

The forwarding firewall policy must also allow the translated traffic.

Troubleshooting steps:

* Verify that the public IP belongs to the router or is correctly routed to it.
* Check whether the provider uses CGNAT.
* Verify the destination NAT rule.
* Confirm the internal server's IP address.
* Confirm that the server listens on the expected port.
* Check the server's default gateway.
* Inspect the forward-chain firewall rules.
* Verify that return traffic follows the expected path.
* Test from a genuinely external network.

# 🗁 Port Forwarding and Upstream NAT

Port forwarding may fail even when the local router configuration is correct.

Consider this topology:

```text
Internet
   |
   v
ISP Router
Public or Shared Address
   |
   v
Customer Router
192.168.1.1
   |
   v
Internal Server
192.168.1.50
```

If both the ISP router and customer router perform NAT, the network uses double NAT.

The incoming connection may need forwarding rules on both devices.

If the ISP uses Carrier-Grade NAT (CGNAT), the customer may not control the upstream translation. Ordinary port forwarding on the customer router cannot independently create inbound reachability through the provider's CGNAT.

Possible solutions include:

* Requesting a public IPv4 address from the ISP.
* Configuring bridge mode or an appropriate upstream forwarding arrangement.
* Using IPv6 where end-to-end connectivity and firewall policy permit it.
* Using a VPN or reverse tunnel for inbound access.

# 🗁 Hairpin NAT

Hairpin NAT, also called NAT loopback, allows internal clients to access an internal server using the router's public address.

Example:

```text
Internal Client: 192.168.1.100
Public Address:  203.0.113.10
Internal Server: 192.168.1.50
```

The client connects to:

```text
203.0.113.10:8443
```

The router translates the destination to the internal server.

Hairpin NAT can fail when:

* The router does not support the required NAT loopback behavior.
* A matching DNAT rule is missing.
* Source NAT is not applied where needed.
* The internal client bypasses the expected router path.

An alternative is split-horizon DNS, where internal clients resolve the service hostname directly to the internal server address.

# 🗁 Firewall Rule Ordering

Firewall rules are often evaluated sequentially.

An earlier matching rule may determine the result before a later rule is reached.

Example:

```text
Rule A: Drop all TCP traffic
Rule B: Allow TCP destination port 443
```

In a first-match rule-processing model, Rule B may never be reached for matching packets.

Troubleshooting should include:

* Checking the order of rules.
* Checking matching conditions.
* Inspecting counters.
* Reviewing jump or chain behavior.
* Checking default chain policies.
* Verifying that the traffic enters the expected chain.

Do not move or disable rules without understanding the security implications.

# 🗁 Stateful Firewall and Return Traffic

Stateful firewalls maintain connection information and commonly permit packets belonging to established connections.

A typical policy concept is:

```text
Allow established and related traffic
Allow specifically authorized new connections
Drop other unsolicited traffic
```

A connection may fail when:

* The initial packet is blocked.
* Return traffic enters through an unexpected interface.
* Connection tracking is unavailable or disabled.
* A connection-tracking entry expires.
* Asymmetric routing sends packets through different stateful devices.
* NAT changes the flow in an unexpected way.

Inspect both directions of the connection rather than investigating only the initial request.

# 🗁 Firewall Logging and Packet Counters

Counters and logs help identify which rule handles a packet.

For Linux:

```bash
sudo nft -a list ruleset
```

For iptables:

```bash
sudo iptables -L -n -v
```

For RouterOS:

```routeros
/ip firewall filter print stats
/ip firewall nat print stats
```

A counter that increases when a test is performed can indicate that the rule is matching the traffic.

For deeper inspection, temporarily enable targeted logging for the specific traffic flow. Avoid enabling unrestricted packet logging on busy routers because it can produce excessive logs and additional CPU load.

# 🗁 Packet Capture for NAT and Firewall Analysis

Packet captures can reveal where traffic is being dropped or translated.

Capture traffic from a client:

```bash
sudo tcpdump -ni eth0 host 192.168.1.100
```

Capture traffic toward an external destination:

```bash
sudo tcpdump -ni eth1 host 198.51.100.20
```

Capture TCP traffic for a specific port:

```bash
sudo tcpdump -ni any 'tcp port 443'
```

Compare the packet headers on both sides of the router.

Example:

```text
LAN interface:
192.168.1.100:50000 → 198.51.100.20:443

WAN interface:
203.0.113.10:40001 → 198.51.100.20:443
```

This confirms that source NAT changed the source address and port.

If the packet appears on the LAN interface but not on the WAN interface, investigate forwarding, route selection, firewall policy, and neighbor resolution.

If the translated packet leaves the WAN interface but no response returns, investigate the upstream network, destination service, and return route.

# 🗁 VPN and NAT Issues

VPN traffic may require special NAT and firewall handling.

Common problems include:

* IPsec traffic blocked by firewall rules.
* NAT applied to traffic that should be excluded from translation.
* Missing routes to remote VPN subnets.
* Incorrect tunnel endpoint addresses.
* Overlapping local and remote networks.
* Asymmetric routing.
* Incorrect forwarding policies.

For a site-to-site VPN, traffic between the protected local and remote networks often needs to bypass ordinary Internet masquerading.

Example policy concept:

```text
Do not NAT:
Local VPN subnet → Remote VPN subnet

Apply source NAT:
Local LAN subnet → Internet
```

Verify both the VPN tunnel state and the routing/firewall policies carrying traffic through it.

# 🗁 Common Troubleshooting Scenarios

**Scenario: Router has Internet access, but LAN clients do not**

Check:

* LAN client IP configuration.
* Client default gateway.
* Router's default route.
* Forward-chain firewall rules.
* Source NAT configuration.
* DNS resolution.

Possible cause: Source NAT is missing or forwarded traffic is blocked.

**Scenario: Port forwarding does not work**

Check:

* Public IP ownership and reachability.
* Upstream NAT or CGNAT.
* Destination NAT rule.
* Forward-chain firewall permissions.
* Internal server service.
* Internal server default gateway.
* Return path.

Possible cause: The ISP uses CGNAT, preventing unsolicited inbound connections from reaching the customer router.

**Scenario: RouterOS NAT rule exists but does not work**

Check:

* Rule order.
* Source and destination matching.
* Incoming or outgoing interface.
* Protocol and port.
* Rule counters.
* Connection-tracking entries.

Possible cause: A previous NAT rule matches the traffic first.

**Scenario: Internal clients cannot access a server using its public IP**

Check:

* Hairpin NAT support.
* Destination NAT matching.
* Source NAT behavior.
* Internal DNS resolution.

Possible cause: NAT loopback is not configured.

**Scenario: VPN connects but remote LAN resources are inaccessible**

Check:

* Routes to the remote subnet.
* Firewall forward rules.
* NAT exemption.
* Return routes.
* IP address overlap.
* VPN traffic selectors or permitted networks.

Possible cause: VPN traffic is incorrectly translated by the ordinary Internet masquerade rule.

# 🗁 NAT and Firewall Troubleshooting Checklist

* [ ] Identify the complete source-to-destination traffic flow.
* [ ] Verify source and destination IP addresses.
* [ ] Verify protocol and port numbers.
* [ ] Confirm the routing decision.
* [ ] Identify the firewall chain handling the packet.
* [ ] Inspect firewall rule order.
* [ ] Check matching rule counters.
* [ ] Verify source NAT or destination NAT.
* [ ] Inspect connection-tracking entries.
* [ ] Check whether packets leave the expected interface.
* [ ] Verify return traffic.
* [ ] Check for double NAT or CGNAT.
* [ ] Inspect hairpin NAT when internal public-address access fails.
* [ ] Verify VPN NAT exemptions.
* [ ] Capture packets on ingress and egress interfaces.
* [ ] Document the root cause and corrective action.

# 🗁 Best Practices

* Keep firewall policies organized and documented.
* Use explicit allow rules and restrictive default policies.
* Avoid broad NAT rules that unintentionally match unrelated traffic.
* Place specific NAT rules before general rules where rule-processing semantics require it.
* Use connection tracking appropriately.
* Monitor firewall counters and relevant logs.
* Avoid unrestricted logging on high-throughput routers.
* Review NAT behavior when adding VPNs or new routing paths.
* Keep public-facing services limited to required ports.
* Test port forwarding from an external network.
* Maintain configuration backups before making firewall changes.
* Review IPv4 and IPv6 firewall policies separately.

# 🗁 Summary

NAT and firewall troubleshooting requires understanding how packets are translated, filtered, tracked, and forwarded.

The most important areas to investigate are:

* Source NAT and destination NAT.
* Firewall chains and rule ordering.
* Connection tracking.
* Return traffic and asymmetric routing.
* Port forwarding and upstream NAT.
* Hairpin NAT.
* VPN NAT exemptions.
* Packet captures and firewall counters.

A reliable investigation follows the packet through every relevant processing stage instead of assuming that the presence of a NAT or firewall rule guarantees successful connectivity.
