# DHCP Client in MikroTik RouterOS

A DHCP client is a network device or interface that automatically obtains its IP configuration from a DHCP server.

In MikroTik RouterOS, the DHCP client feature allows the router to receive network configuration dynamically instead of requiring manual IP address configuration.

DHCP clients are commonly used on:

* WAN interfaces connected to an ISP.
* Ethernet interfaces connected to an upstream router.
* Wireless interfaces connected to another network.
* VLAN interfaces receiving dynamically assigned addresses.
* Lab environments where IP addressing is provided by a DHCP server.

For example, when a MikroTik router connects to an ISP modem or an upstream router, the WAN interface may receive its IP address, gateway, and DNS information automatically through DHCP.

# 🗁 Understanding DHCP Client

DHCP stands for Dynamic Host Configuration Protocol.

It allows a DHCP server to provide network configuration to clients automatically.

A DHCP client can receive:

* IPv4 address.
* Subnet prefix.
* Default gateway.
* DNS server addresses.
* DHCP lease duration.
* Other DHCP options supported by the server and client.

Example network:

```text
                Internet
                   |
                ISP Router
             DHCP Server
                   |
                ether1
                   |
             MikroTik Router
              DHCP Client
```

The MikroTik router requests an IP address from the ISP router's DHCP server.

# 🗁 DHCP Client vs DHCP Server

These two RouterOS services perform different roles.

| Feature | DHCP Client | DHCP Server |
| --- | --- | --- |
| Purpose | Obtains network configuration | Assigns network configuration |
| Address allocation | Receives an address | Provides addresses |
| Typical interface | WAN | LAN |
| Main menu | `/ip dhcp-client` | `/ip dhcp-server` |
| Example | ISP connection | Office LAN clients |

A MikroTik router can run both services simultaneously.

For example:

* `ether1` operates as a DHCP client and receives an address from the ISP.
* `bridge-LAN` operates as a DHCP server and provides addresses to local devices.

# 🗁 DHCP Client Configuration Menu

RouterOS manages DHCP clients through:

```routeros
/ip dhcp-client
```

Common operations include:

* Creating a DHCP client.
* Enabling or disabling a DHCP client.
* Configuring default-route behavior.
* Configuring DNS behavior.
* Setting DHCP client options.
* Inspecting lease information.
* Releasing or renewing a lease.

# 🗁 Configure a DHCP Client on WAN

Suppose an ISP router provides IP configuration through DHCP.

Network topology:

```text
               ISP Router
             192.168.1.1
                   |
                   |
                ether1
                   |
             MikroTik Router
```

Configure the DHCP client:

```routeros
/ip dhcp-client
add interface=ether1 add-default-route=yes use-peer-dns=yes disabled=no
```

Explanation:

| Property | Description |
| --- | --- |
| `interface=ether1` | Interface connected to the upstream network |
| `add-default-route=yes` | Allows the DHCP client to install a default route |
| `use-peer-dns=yes` | Allows DNS servers supplied by the DHCP server to be used |
| `disabled=no` | Enables the DHCP client |

The router will attempt to obtain an IP configuration from the DHCP server.

# 🗁 Verify DHCP Client Status

Display configured DHCP clients:

```routeros
/ip dhcp-client print
```

Display detailed information:

```routeros
/ip dhcp-client print detail
```

A successful DHCP client commonly displays a status of `bound`.

Example:

```text
Flags: X - DISABLED, I - INVALID
Columns: INTERFACE, USE-PEER-DNS, ADD-DEFAULT-ROUTE, STATUS, ADDRESS
# INTERFACE  USE-PEER-DNS  ADD-DEFAULT-ROUTE  STATUS  ADDRESS
0 ether1     yes           yes                bound   192.168.1.100/24
```

The `bound` status indicates that the DHCP client successfully obtained a lease.

Important fields include:

* `status`: Current DHCP client state.
* `address`: Assigned IPv4 address and prefix.
* `gateway`: Gateway supplied through DHCP, when provided.
* `expires-after`: Remaining lease time.
* `primary-dns`: Primary DNS server, when supplied.
* `secondary-dns`: Secondary DNS server, when supplied.

# 🗁 DHCP Client States

A DHCP client can pass through different operational states.

| State | Meaning |
| --- | --- |
| searching | Looking for an available DHCP server |
| requesting | Requesting an IP configuration |
| bound | Successfully received a DHCP lease |
| renewing | Attempting to renew an existing lease |
| rebinding | Attempting to obtain a lease from an available server |

The exact status displayed depends on the DHCP client lifecycle and RouterOS version.

# 🗁 Understanding DHCP Lease Process

DHCP commonly uses the DORA process:

* Discover
* Offer
* Request
* Acknowledgment

<escape>DHCP client-server communication:</escape>

```text
       DHCP Client                 DHCP Server
            |                           |
            |------ Discover ---------->|
            |                           |
            |<------- Offer -------------|
            |                           |
            |------- Request ---------->|
            |                           |
            |<------ Acknowledgment -----|
            |                           |
            |       Lease Active        |
```

**DHCP Discover**

The client broadcasts a message to locate available DHCP servers.

**DHCP Offer**

A DHCP server offers an available IP address and network configuration.

**DHCP Request**

The client requests the offered configuration.

**DHCP Acknowledgment**

The server confirms the lease and provides the configuration.

After successful completion, the DHCP client can use the assigned address.

# 🗁 Configure Default Route Behavior

A DHCP server can provide a default gateway through DHCP option 3.

RouterOS can install that gateway as a dynamic default route.

Enable default-route installation:

```routeros
/ip dhcp-client
set [find interface=ether1] add-default-route=yes
```

Disable default-route installation:

```routeros
/ip dhcp-client
set [find interface=ether1] add-default-route=no
```

Inspect the routing table:

```routeros
/ip route print detail
```

A dynamically installed default route commonly appears with the `D` and `A` flags.

Example:

```text
DAv 0.0.0.0/0 gateway=192.168.1.1
```

The actual gateway depends on the DHCP server's configuration.

# 🗁 Configure Default Route Distance

The `default-route-distance` property controls the administrative distance of the default route installed by a DHCP client.

Example:

```routeros
/ip dhcp-client
set [find interface=ether1] add-default-route=yes default-route-distance=1
```

If another WAN connection should be preferred, configure a higher distance on the backup DHCP client.

Example:

```routeros
/ip dhcp-client
add interface=ether2 add-default-route=yes default-route-distance=2 use-peer-dns=no disabled=no
```

In this example:

* `ether1` has route distance 1.
* `ether2` has route distance 2.
* The lower-distance route is preferred when both routes are otherwise eligible.

This provides a basic mechanism for primary and backup default routes.

However, route distance alone does not provide comprehensive WAN health monitoring. A route can remain active even when the upstream connection has lost Internet connectivity. Recursive routing or another health-check mechanism may be required for reliable failover.

# 🗁 Configure DNS from DHCP

A DHCP server can provide DNS server addresses through DHCP option 6.

RouterOS can use these addresses automatically.

Enable peer DNS:

```routeros
/ip dhcp-client
set [find interface=ether1] use-peer-dns=yes
```

Disable peer DNS:

```routeros
/ip dhcp-client
set [find interface=ether1] use-peer-dns=no
```

Inspect DNS configuration:

```routeros
/ip dns print
```

When multiple DHCP clients provide DNS information, RouterOS may maintain dynamic DNS server entries according to the configured client settings.

For a manually controlled DNS configuration:

```routeros
/ip dns
set servers=1.1.1.1,8.8.8.8
```

If clients on the LAN should use the MikroTik router as their DNS resolver, enable remote requests:

```routeros
/ip dns
set allow-remote-requests=yes
```

Configure firewall rules to restrict DNS access to trusted networks rather than exposing the router's DNS resolver to untrusted interfaces.

# 🗁 Configure DHCP Client on a VLAN Interface

A DHCP client can also run on a VLAN interface.

This is useful when an ISP provides Internet connectivity through a tagged VLAN.

Example:

```text
                ISP Network
                     |
                 ether1
                     |
               MikroTik Router
                     |
                  VLAN 100
                 DHCP Client
```

Create the VLAN interface:

```routeros
/interface vlan
add name=vlan100 interface=ether1 vlan-id=100
```

Configure the DHCP client:

```routeros
/ip dhcp-client
add interface=vlan100 add-default-route=yes use-peer-dns=yes disabled=no
```

The DHCP client now requests an address through VLAN 100.

The VLAN ID and interface arrangement must match the ISP's requirements.

If the physical interface is part of a VLAN-aware bridge, the VLAN interface should normally be created on the appropriate bridge interface, with correct bridge VLAN table membership and CPU tagging.

# 🗁 Configure DHCP Client on a Wireless Interface

A DHCP client can obtain an address through a wireless interface when that interface is connected to an upstream network.

Example:

```routeros
/ip dhcp-client
add interface=wlan1 add-default-route=yes use-peer-dns=yes disabled=no
```

The wireless interface must already be configured and connected to a compatible access point.

On newer RouterOS devices using the WiFi configuration menus, use the actual interface name shown by the device.

# 🗁 DHCP Client Hostname

A DHCP client can send a hostname to the DHCP server.

This can help the upstream network identify the MikroTik router.

Example:

```routeros
/ip dhcp-client
set [find interface=ether1] host-name=router-wan
```

The DHCP server may use this hostname for lease identification, logging, or DNS registration if it supports those functions.

# 🗁 DHCP Client Options

RouterOS DHCP clients support additional configuration properties.

Inspect available properties:

```routeros
/ip dhcp-client add ?
```

Inspect an existing DHCP client:

```routeros
/ip dhcp-client print detail
```

Commonly used properties include:

| Property | Purpose |
| --- | --- |
| `interface` | Interface used for DHCP communication |
| `add-default-route` | Controls installation of a default route |
| `default-route-distance` | Sets the distance of the installed default route |
| `use-peer-dns` | Controls use of DNS servers provided by the DHCP server |
| `use-peer-ntp` | Controls use of NTP servers provided by the DHCP server |
| `host-name` | Hostname sent to the DHCP server |
| `client-id` | DHCP client identifier |
| `disabled` | Enables or disables the DHCP client |

Available properties and behavior can vary by RouterOS release.

# 🗁 Release and Renew a DHCP Lease

A DHCP client may need to release its current lease and request a new one.

**Release a lease**

```routeros
/ip dhcp-client
release [find interface=ether1]
```

Releasing the lease causes the client to relinquish its current DHCP assignment.

**Renew a lease**

```routeros
/ip dhcp-client
renew [find interface=ether1]
```

The client attempts to renew its DHCP lease.

**Inspect the result**

```routeros
/ip dhcp-client print detail
/ip address print
```

A successful operation should return the client to the `bound` state.

# 🗁 Enable and Disable DHCP Client

Disable a DHCP client:

```routeros
/ip dhcp-client
disable [find interface=ether1]
```

Enable it again:

```routeros
/ip dhcp-client
enable [find interface=ether1]
```

Disabling a DHCP client may remove its dynamically obtained address and associated dynamic routes.

# 🗁 Remove DHCP Client

Remove a DHCP client:

```routeros
/ip dhcp-client
remove [find interface=ether1]
```

Verify:

```routeros
/ip dhcp-client print
```

Removing the client stops DHCP address acquisition on that interface.

Any address and default route previously supplied by that client will no longer be maintained by it.

# 🗁 Static IP vs DHCP Client on WAN

A WAN interface can use either static addressing or DHCP, depending on the upstream network.

| Feature | Static WAN | DHCP WAN |
| --- | --- | --- |
| IP assignment | Manual | Automatic |
| Gateway | Manually configured | Usually provided by DHCP |
| DNS | Manual or separate configuration | Can be provided by DHCP |
| Address changes | Administrator-controlled | Server-controlled |
| Common use | Static ISP connection | Dynamic ISP connection |

**Static WAN example**

```routeros
/ip address
add address=203.0.113.2/30 interface=ether1

/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1
```

`203.0.113.0/24` is a documentation-only address range, not a real public ISP allocation.

**DHCP WAN example**

```routeros
/ip dhcp-client
add interface=ether1 add-default-route=yes use-peer-dns=yes disabled=no
```

Choose the method specified by the upstream provider.

Do not configure conflicting static and dynamic WAN addressing on the same interface unless the design explicitly requires multiple addresses.

# 🗁 DHCP Client and Multiple WAN Connections

A MikroTik router can use multiple DHCP clients on separate WAN interfaces.

Example:

```text
                   Internet
                  /        \
                 /          \
             ISP 1          ISP 2
                |              |
             ether1          ether2
                |              |
                +--- MikroTik -+
```

Configure the primary WAN:

```routeros
/ip dhcp-client
add interface=ether1 add-default-route=yes default-route-distance=1 use-peer-dns=no disabled=no
```

Configure the secondary WAN:

```routeros
/ip dhcp-client
add interface=ether2 add-default-route=yes default-route-distance=2 use-peer-dns=no disabled=no
```

Inspect the installed routes:

```routeros
/ip route print detail
```

This configuration establishes preference between the dynamically installed default routes.

For production failover, monitor actual upstream reachability and configure appropriate routing behavior. A reachable ISP gateway does not necessarily mean that the Internet is reachable.

# 🗁 Troubleshooting DHCP Client Problems

**DHCP client remains in searching state**

Possible causes:

* No DHCP server is available.
* Ethernet cable is disconnected.
* Upstream device is not operational.
* Wrong VLAN ID.
* DHCP requests are blocked.
* The ISP requires a specific VLAN or client identity.

Check:

```routeros
/interface print
/interface ethernet monitor ether1
/ip dhcp-client print detail
```

**DHCP client is bound but Internet does not work**

Possible causes:

* Default route was not installed.
* Incorrect gateway supplied by upstream network.
* DNS configuration is incorrect.
* Firewall rules block traffic.
* Upstream network has no Internet connectivity.

Check:

```routeros
/ip address print
/ip route print detail
/ip dns print
/ping 8.8.8.8
/ping google.com
```

**DHCP client receives an unexpected IP address**

Possible causes:

* Wrong upstream DHCP server.
* Incorrect physical connection.
* Unexpected VLAN configuration.
* DHCP server has a different address pool.
* Existing lease reservation.

Check:

```routeros
/ip dhcp-client print detail
/ip address print detail
```

**DHCP client receives an IP address but no default route**

Possible causes:

* `add-default-route=no`.
* DHCP server does not provide a default gateway.
* The received gateway information is invalid.

Check:

```routeros
/ip dhcp-client print detail
/ip route print detail
```

If appropriate, enable default-route installation:

```routeros
/ip dhcp-client
set [find interface=ether1] add-default-route=yes
```

**DNS resolution fails**

Possible causes:

* DHCP server does not provide DNS servers.
* `use-peer-dns=no`.
* DNS servers are unreachable.
* Router DNS configuration is incorrect.

Check:

```routeros
/ip dhcp-client print detail
/ip dns print
/ping 1.1.1.1
/ping google.com
```

# 🗁 Practical DHCP Client Configuration Lab

**Lab topology**

```text
                Upstream Router
                192.168.1.1/24
                       |
                       |
                    ether1
                       |
                MikroTik Router
                 DHCP Client
                       |
                    bridge-LAN
                       |
                    ether2
                       |
                    LAN PC
```

**Network requirements**

| Parameter | Value |
| --- | --- |
| WAN interface | ether1 |
| Upstream network | 192.168.1.0/24 |
| Upstream gateway | 192.168.1.1 |
| WAN addressing | DHCP |
| LAN interface | bridge-LAN |
| LAN address | 192.168.10.1/24 |

**Configure the WAN DHCP client**

```routeros
/ip dhcp-client
add interface=ether1 add-default-route=yes use-peer-dns=yes disabled=no
```

**Configure the LAN bridge**

```routeros
/interface bridge
add name=bridge-LAN

/interface bridge port
add bridge=bridge-LAN interface=ether2
```

**Assign the LAN IP address**

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

**Verify the WAN lease**

```routeros
/ip dhcp-client print detail
/ip address print
```

**Verify the default route**

```routeros
/ip route print detail
```

**Test upstream connectivity**

```routeros
/ping 192.168.1.1
/ping 8.8.8.8
```

**Test DNS resolution**

```routeros
/ping google.com
```

Expected results:

* The WAN DHCP client obtains an address from the upstream router.
* The router installs a default route if the DHCP server provides a gateway.
* The LAN bridge has `192.168.10.1/24`.
* The router can communicate with the upstream gateway.
* Internet and DNS connectivity work when the upstream network provides those services.

# 🗁 Best Practices

* Use DHCP clients only on interfaces that receive dynamic configuration.
* Verify the DHCP client reaches the `bound` state.
* Inspect dynamically installed default routes.
* Use route distance deliberately when configuring multiple WAN connections.
* Avoid unintended DNS changes by controlling `use-peer-dns`.
* Use the correct VLAN interface when an ISP requires tagged WAN traffic.
* Check upstream connectivity before troubleshooting RouterOS.
* Monitor lease expiration and renewal behavior.
* Document ISP-specific DHCP requirements.
* Keep a backup before changing WAN configuration.

# 🗁 Official Documentation

* MikroTik DHCP: <https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP>
* MikroTik IP Routing: <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing>
