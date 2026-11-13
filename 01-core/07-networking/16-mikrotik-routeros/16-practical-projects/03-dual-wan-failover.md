# Dual-WAN Failover

Dual-WAN failover allows a MikroTik router to use two internet connections and automatically switch to a backup connection when the primary connection becomes unavailable.

This configuration improves network availability by providing an alternative internet path without requiring manual intervention.

Dual-WAN failover is commonly used in:

* Small and medium-sized businesses.
* ISP infrastructure.
* Enterprise networks.
* Remote offices.
* Networks requiring continuous internet connectivity.
* Environments where internet downtime affects business operations.

# 🗁 Project Objectives

By completing this project, you will learn how to:

* Configure two WAN connections.
* Configure primary and backup default routes.
* Understand route distance and gateway monitoring.
* Implement recursive gateway checking.
* Configure source NAT for both WAN connections.
* Verify automatic failover.
* Verify automatic recovery.
* Troubleshoot routing and connectivity problems.

# 🗁 Understanding Dual-WAN Failover

A dual-WAN configuration uses two independent internet connections.

* **WAN1:** Primary internet connection.
* **WAN2:** Backup internet connection.

Under normal conditions, the router forwards internet traffic through WAN1.

When WAN1 becomes unavailable, the router switches to WAN2.

When WAN1 recovers, the router can automatically return to the primary connection.

```text
                    Internet
                       |
                +------+------+
                |             |
              ISP 1         ISP 2
                |             |
             WAN1           WAN2
                |             |
                +------+------+
                       |
                 +-----------+
                 | MikroTik  |
                 | Router    |
                 +-----------+
                       |
                    LAN
                       |
                 Office Devices

```

# 🗁 Network Addressing Plan

This project uses DHCP for both WAN connections.

| Component | Interface | Example configuration |
| --- | --- | --- |
| Primary WAN | `ether1` | DHCP client |
| Backup WAN | `ether2` | DHCP client |
| LAN | `ether3` | `192.168.10.1/24` |
| Primary gateway | ISP 1 | Assigned through DHCP |
| Backup gateway | ISP 2 | Assigned through DHCP |
| LAN network | — | `192.168.10.0/24` |

The example assumes both ISPs provide an IPv4 address and gateway through DHCP.

If your ISP uses PPPoE or static addressing, configure the corresponding WAN client or static address instead.

# 🗁 Preparing the Router

Before configuration:

* Identify the two WAN interfaces.
* Identify the LAN interface.
* Confirm both ISP connections are operational.
* Confirm the WAN connections use independent upstream paths.
* Save the existing router configuration.
* Ensure you have local or out-of-band management access.

The configuration below is intended for a clean or appropriately prepared router. Avoid applying it blindly to a router with existing default routes, DHCP clients, NAT rules, or firewall policies.

# 🗁 Configuring Router Identity

Set the router identity:

```routeros
/system/identity/set name=Dual-WAN-Router
```

Verify:

```routeros id="f7z3lk"
/system/identity/print
```

# 🗁 Configuring WAN Interface Lists

Create interface lists:

```routeros id="w3d4km"
/interface/list/add name=WAN
/interface/list/add name=LAN
```

Add both WAN interfaces:

```routeros id="6qg6ga"
/interface/list/member/add list=WAN interface=ether1
/interface/list/member/add list=WAN interface=ether2
```

Add the LAN interface:

```routeros id="f9q5su"
/interface/list/member/add list=LAN interface=ether3
```

These interface lists simplify NAT and firewall configuration.

# 🗁 Configuring LAN Connectivity

Assign a private IP address to the LAN interface:

```routeros id="8w9qz4"
/ip/address/add address=192.168.10.1/24 interface=ether3 comment="LAN Gateway"
```

Create a DHCP pool:

```routeros id="4f3v2q"
/ip/pool/add name=lan-pool ranges=192.168.10.100-192.168.10.200
```

Create a DHCP server:

```routeros id="7z5g2m"
/ip/dhcp-server/add name=lan-dhcp interface=ether3 address-pool=lan-pool lease-time=12h disabled=no
```

Configure the DHCP network:

```routeros id="w6k5nb"
/ip/dhcp-server/network/add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

Configure DNS:

```routeros id="a3n8vm"
/ip/dns/set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

Protect recursive DNS access with an appropriate input firewall policy that permits trusted LAN clients and denies untrusted WAN clients.

# 🗁 Configuring Primary WAN

Configure the primary ISP connection through DHCP:

```routeros id="b6c7dn"
/ip/dhcp-client/add interface=ether1 add-default-route=yes default-route-distance=1 use-peer-dns=no disabled=no comment="Primary WAN"
```

The route distance is set to `1`.

A lower route distance makes this route preferred over a competing route to the same destination in the same routing table.

# 🗁 Configuring Backup WAN

Configure the backup ISP connection:

```routeros id="v5x8qt"
/ip/dhcp-client/add interface=ether2 add-default-route=yes default-route-distance=2 use-peer-dns=no disabled=no comment="Backup WAN"
```

The route distance is set to `2`.

Because the primary route has a lower distance, RouterOS prefers WAN1 while its route is active.

The backup route becomes eligible when the primary route is no longer available.

# 🗁 Understanding Route Distance

Route distance determines the preference between competing routes to the same destination within a routing table.

| Route | Gateway | Distance | Preference |
| --- | --- | ---: | --- |
| Primary WAN | ISP 1 | 1 | Preferred |
| Backup WAN | ISP 2 | 2 | Secondary |

A simplified routing decision:

```text
WAN1 Route Available?
        |
        +---- Yes ----> Use WAN1
        |
        +---- No -----> Use WAN2
```

Route distance alone does not guarantee that an internet connection is healthy.

A WAN gateway may remain reachable even when the ISP has lost upstream connectivity.

For more reliable failover, use recursive gateway monitoring.

# 🗁 Understanding Gateway Monitoring

RouterOS can monitor gateway reachability using `check-gateway`.

For example, a route can monitor its gateway through ICMP.

However, this only confirms reachability of the configured gateway. It does not necessarily confirm that the ISP can reach the wider internet.

Recursive routing allows the router to monitor a remote IP address through a specific WAN connection.

This provides a more meaningful connectivity check.

# 🗁 Configuring Recursive Gateway Checks

For this example, use:

| Component | Primary WAN | Backup WAN |
| --- | --- | --- |
| Interface | `ether1` | `ether2` |
| Monitoring address | `1.1.1.1` | `8.8.8.8` |
| Gateway | DHCP-provided | DHCP-provided |
| Default route distance | 1 | 2 |

The monitoring addresses should be reachable independently through their corresponding WAN connections.

First, identify the gateways assigned by DHCP:

```routeros
/ip/dhcp-client/print detail
```

Assume the ISP gateways are:

```text
WAN1 Gateway: 203.0.113.1
WAN2 Gateway: 198.51.100.1
```

These are documentation-only addresses. Replace them with the actual gateways provided by the ISPs.

Create host routes for the monitoring destinations:

```routeros
/ip/route/add dst-address=1.1.1.1/32 gateway=203.0.113.1 scope=10 comment="WAN1 Monitoring Host"
/ip/route/add dst-address=8.8.8.8/32 gateway=198.51.100.1 scope=10 comment="WAN2 Monitoring Host"
```

Create the recursive default routes:

```routeros
/ip/route/add dst-address=0.0.0.0/0 gateway=1.1.1.1 distance=1 target-scope=11 check-gateway=ping comment="Primary WAN Recursive"
/ip/route/add dst-address=0.0.0.0/0 gateway=8.8.8.8 distance=2 target-scope=11 check-gateway=ping comment="Backup WAN Recursive"
```

The recursive routes resolve their monitoring addresses through the corresponding host routes.

The `scope` and `target-scope` values control route resolution.

**Important:** This recursive configuration is an alternative to relying solely on the default routes installed by DHCP. Avoid leaving conflicting DHCP-installed default routes active alongside these manually configured routes. Configure the DHCP clients with `add-default-route=no` when using this static recursive design.

Example:

```routeros
/ip/dhcp-client/set [find interface=ether1] add-default-route=no
/ip/dhcp-client/set [find interface=ether2] add-default-route=no
```

If your router receives dynamic gateway addresses, the host routes must be updated when those gateways change. For a production design, use a gateway-resolution strategy appropriate to the ISP's addressing behavior.

# 🗁 Configuring NAT for Both WAN Connections

Both WAN interfaces require source NAT so that LAN devices can access the internet through either ISP.

Create a masquerade rule:

```routeros
/ip/firewall/nat/add chain=srcnat action=masquerade out-interface-list=WAN comment="Dual-WAN Internet NAT"
```

This rule applies to traffic leaving through either WAN interface.

Verify:

```routeros
/ip/firewall/nat/print
```

NAT does not select the WAN path. Routing selects the egress interface, and NAT translates the source address accordingly.

# 🗁 Configuring Basic Firewall Protection

The following example provides a basic IPv4 firewall policy for the LAN and WAN interfaces.

Input chain:

```routeros
/ip/firewall/filter/add chain=input action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=input action=drop connection-state=invalid comment="Drop invalid"
/ip/firewall/filter/add chain=input action=accept in-interface-list=LAN comment="Allow LAN access to router"
/ip/firewall/filter/add chain=input action=drop in-interface-list=WAN comment="Drop WAN access to router"
```

Forward chain:

```routeros
/ip/firewall/filter/add chain=forward action=accept connection-state=established,related,untracked comment="Accept established and related"
/ip/firewall/filter/add chain=forward action=drop connection-state=invalid comment="Drop invalid"
/ip/firewall/filter/add chain=forward action=accept in-interface-list=LAN out-interface-list=WAN comment="Allow LAN to internet"
/ip/firewall/filter/add chain=forward action=drop in-interface-list=WAN connection-state=new connection-nat-state=!dstnat comment="Drop unsolicited WAN forwarding"
```

Rule order matters. These rules are an example for a simple single-LAN network and must be integrated with existing firewall policies carefully.

If you require VPN access, port forwarding, or additional internal networks, add explicit rules for those services.

# 🗁 Testing Primary WAN Connectivity

Verify the DHCP clients:

```routeros
/ip/dhcp-client/print detail
```

Inspect the routing table:

```routeros
/ip/route/print detail
```

Test the primary monitoring destination:

```routeros
/ping 1.1.1.1
```

Test DNS resolution:

```routeros
/ping google.com
```

Confirm that the active default route uses WAN1.

# 🗁 Testing Failover

Perform the failover test carefully.

A simple physical test is to disconnect the primary WAN cable.

For a controlled test:

* Confirm WAN1 is working.
* Start a continuous ping to an external destination.
* Disconnect WAN1.
* Observe the active default route.
* Verify internet connectivity through WAN2.
* Reconnect WAN1.
* Confirm that the primary route becomes preferred again.

Inspect route state:

```routeros
/ip/route/print detail
```

Inspect interface status:

```routeros
/interface/print
```

With recursive monitoring, the primary route should become inactive when its monitored destination is no longer reachable through the intended path.

Existing internet connections may be interrupted during failover because the public source address changes.

# 🗁 Testing Automatic Recovery

After reconnecting the primary WAN:

* Confirm that its gateway is reachable.
* Confirm that the primary monitoring destination responds.
* Check that the primary recursive default route becomes active.
* Confirm that new internet connections use WAN1.

Inspect routing:

```routeros
/ip/route/print detail
```

Automatic recovery depends on successful route reactivation and the configured routing preference.

# 🗁 Monitoring WAN Traffic

Monitor primary WAN traffic:

```routeros
/interface/monitor-traffic ether1
```

Monitor backup WAN traffic:

```routeros
/interface/monitor-traffic ether2
```

Inspect connection tracking:

```routeros
/ip/firewall/connection/print
```

Inspect the routing table:

```routeros
/ip/route/print
```

These commands help determine which WAN is carrying traffic and whether the router has selected the expected route.

# 🗁 Common Troubleshooting Scenarios

| Problem | Possible cause | Investigation |
| --- | --- | --- |
| WAN1 never becomes active | DHCP failure or invalid route | Check DHCP client and route state |
| WAN2 does not take over | Backup route unavailable or incorrect distance | Inspect default routes |
| Failover does not detect internet failure | Only the ISP gateway is being monitored | Use recursive monitoring |
| Internet works through WAN1 but not WAN2 | Missing NAT or incorrect backup gateway | Check NAT and routing |
| Primary WAN does not recover | Monitoring destination remains unreachable | Check recursive host route |
| Monitoring traffic uses the wrong WAN | Host route or gateway resolution is incorrect | Inspect specific host routes |
| Existing sessions disconnect during failover | WAN source address changes | Establish new connections after failover |

Useful diagnostic commands:

```routeros
/ip/dhcp-client/print detail
/ip/route/print detail
/ip/address/print
/ip/firewall/nat/print
/ip/firewall/filter/print
/interface/print
```

# 🗁 Final Configuration Checklist

* [ ] Primary WAN configured.
* [ ] Backup WAN configured.
* [ ] WAN interface lists created.
* [ ] LAN interface configured.
* [ ] DHCP server configured for LAN.
* [ ] DNS configured.
* [ ] Primary route preferred.
* [ ] Backup route available.
* [ ] Gateway monitoring configured.
* [ ] Recursive routes verified, if used.
* [ ] NAT configured for both WAN interfaces.
* [ ] Firewall protection configured.
* [ ] Primary WAN failure tested.
* [ ] Backup WAN connectivity verified.
* [ ] Automatic recovery tested.
* [ ] Configuration backup created.

# 🗁 Expected Result

After completing this project, the MikroTik router should provide:

* Two independent WAN connections.
* Primary and backup routing.
* Automatic failover when the primary route becomes unavailable.
* Automatic recovery when the primary route returns.
* NAT connectivity through both ISPs.
* Basic firewall protection.
* WAN monitoring and troubleshooting capability.

This project establishes the foundation for more advanced multi-WAN designs involving PCC load balancing, policy-based routing, recursive gateway monitoring, and per-connection failover.
