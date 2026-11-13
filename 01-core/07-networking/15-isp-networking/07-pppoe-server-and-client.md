# PPPoE Server and Client Configuration

A PPPoE network requires two main components:

* **PPPoE Server:** Accepts incoming PPPoE connections from subscriber devices and establishes PPP sessions.
* **PPPoE Client:** Initiates a connection to the PPPoE server using subscriber credentials.

In an ISP network, the PPPoE server usually runs on a router or Broadband Network Gateway (BNG), while the PPPoE client runs on the customer's router.

This document covers practical PPPoE server and client configuration using MikroTik RouterOS v7.

# 🗁 Network Topology

The following example represents a small ISP access network.

```text
                 INTERNET
                    |
                    |
              ISP Gateway
                    |
                    |
             MikroTik Router
               PPPoE Server
                    |
                    |
              Access Switch
                    |
                    |
             Customer Router
               PPPoE Client
                    |
                    |
              Customer LAN
                    |
                    |
              Client Devices
```

Example network parameters:

| Parameter | Value |
| --- | --- |
| PPPoE Server | MikroTik RouterOS v7 |
| Server Interface | `ether2` |
| Service Name | `ISP-PPPoE` |
| PPPoE Pool | `10.10.10.100-10.10.10.200` |
| Local Address | `10.10.10.1` |
| DNS Server | `1.1.1.1`, `8.8.8.8` |
| Subscriber Username | `customer001` |
| Subscriber Password | `StrongPassword123` |
| PPP Profile | `pppoe-profile` |
| Subscriber Rate Limit | `10M/10M` |

These addresses and credentials are examples. Replace them with values appropriate for your network.

# 🗁 Preparing the PPPoE Server Interface

Before configuring the PPPoE server, identify the interface connected to the subscriber access network.

Check available interfaces:

```routeros
/interface print
```

If subscribers connect through a dedicated Ethernet interface, that interface can be used as the PPPoE server interface.

If subscribers connect through a VLAN, bridge, or aggregation network, configure the appropriate logical interface instead.

**Important:** The PPPoE server must receive the subscribers' Ethernet discovery traffic. Ensure that the access and aggregation network forwards the relevant traffic to the server.

# 🗁 Creating an IP Address Pool

An IP pool defines the addresses available for subscriber connections.

Create a pool:

```routeros
/ip pool add \
    name=pppoe-pool \
    ranges=10.10.10.100-10.10.10.200
```

Verify the pool:

```routeros
/ip pool print
```

The pool provides addresses that can be assigned to PPPoE subscribers.

# 🗁 Creating a PPP Profile

A PPP profile defines common settings applied to subscriber sessions.

Create a profile:

```routeros
/ppp profile add \
    name=pppoe-profile \
    local-address=10.10.10.1 \
    remote-address=pppoe-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    rate-limit=10M/10M
```

Profile parameters:

| Parameter | Description |
| --- | --- |
| `name` | Name of the PPP profile |
| `local-address` | Local PPP endpoint address |
| `remote-address` | Address pool or address assigned to the remote subscriber |
| `dns-server` | DNS servers provided to subscribers |
| `rate-limit` | Subscriber bandwidth limit |

The `local-address` is the ISP-side PPP endpoint. The `remote-address` is assigned to the subscriber.

The example applies a 10 Mbps upload and 10 Mbps download limit according to the RouterOS rate-limit convention.

Verify the profile:

```routeros
/ppp profile print detail
```

# 🗁 Creating a PPPoE Server

Create the PPPoE server on the subscriber-facing interface.

```routeros
/interface pppoe-server server add \
    interface=ether2 \
    service-name=ISP-PPPoE \
    default-profile=pppoe-profile \
    authentication=chap,mschap2 \
    one-session-per-host=yes \
    disabled=no
```

Configuration parameters:

| Parameter | Description |
| --- | --- |
| `interface` | Interface receiving PPPoE discovery traffic |
| `service-name` | PPPoE service name advertised by the server |
| `default-profile` | Default PPP profile |
| `authentication` | Permitted authentication protocols |
| `one-session-per-host` | Restricts a host to one active session |
| `disabled` | Enables or disables the server |

The server can now respond to PPPoE discovery requests arriving on `ether2`.

Verify the configuration:

```routeros
/interface pppoe-server server print detail
```

# 🗁 Creating a PPPoE Subscriber Account

A PPP secret defines an individual subscriber account.

Create a subscriber:

```routeros
/ppp secret add \
    name=customer001 \
    password=StrongPassword123 \
    service=pppoe \
    profile=pppoe-profile
```

Parameter descriptions:

| Parameter | Description |
| --- | --- |
| `name` | Subscriber username |
| `password` | Subscriber password |
| `service` | Service type |
| `profile` | PPP profile assigned to the subscriber |

The `service=pppoe` parameter restricts this account to PPPoE service.

Verify subscriber accounts:

```routeros
/ppp secret print
```

# 🗁 Applying Individual Subscriber Bandwidth

Different subscribers may have different subscription packages.

For example:

| Subscriber | Package | Upload | Download |
| --- | --- | ---: | ---: |
| customer001 | Basic | 5 Mbps | 5 Mbps |
| customer002 | Standard | 10 Mbps | 10 Mbps |
| customer003 | Premium | 20 Mbps | 20 Mbps |
| customer004 | Business | 50 Mbps | 50 Mbps |

Create separate profiles:

```routeros
/ppp profile add \
    name=basic-5m \
    local-address=10.10.10.1 \
    remote-address=pppoe-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    rate-limit=5M/5M

/ppp profile add \
    name=standard-10m \
    local-address=10.10.10.1 \
    remote-address=pppoe-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    rate-limit=10M/10M

/ppp profile add \
    name=premium-20m \
    local-address=10.10.10.1 \
    remote-address=pppoe-pool \
    dns-server=1.1.1.1,8.8.8.8 \
    rate-limit=20M/20M
```

Assign the appropriate profile to each subscriber:

```routeros
/ppp secret set [find name=customer001] profile=basic-5m

/ppp secret set [find name=customer002] profile=standard-10m

/ppp secret set [find name=customer003] profile=premium-20m
```

When a subscriber establishes a new session, the assigned profile determines the applicable PPP settings.

# 🗁 Configuring the PPPoE Client

The customer router must be configured as a PPPoE client.

On a MikroTik customer router, identify the WAN interface connected to the ISP access network.

```routeros
/interface print
```

Create a PPPoE client:

```routeros
/interface pppoe-client add \
    name=pppoe-out1 \
    interface=ether1 \
    user=customer001 \
    password=StrongPassword123 \
    service-name=ISP-PPPoE \
    add-default-route=yes \
    use-peer-dns=yes \
    disabled=no
```

Configuration parameters:

| Parameter | Description |
| --- | --- |
| `name` | Name of the PPPoE client interface |
| `interface` | Physical or logical WAN interface |
| `user` | Subscriber username |
| `password` | Subscriber password |
| `service-name` | Requested PPPoE service name |
| `add-default-route` | Adds a default route through the PPPoE connection |
| `use-peer-dns` | Uses DNS servers provided by the ISP |
| `disabled` | Enables or disables the client |

If the server does not require a specific service name, the client can omit `service-name` or use the appropriate value configured by the ISP.

# 🗁 Configuring the Customer LAN

After the PPPoE connection is established, the customer router can provide internet access to devices on its local network.

Example LAN configuration:

```routeros
/ip address add \
    address=192.168.88.1/24 \
    interface=bridge
```

Configure a source NAT rule:

```routeros
/ip firewall nat add \
    chain=srcnat \
    out-interface=pppoe-out1 \
    action=masquerade \
    comment="NAT for PPPoE Internet"
```

Configure a DHCP pool:

```routeros
/ip pool add \
    name=lan-pool \
    ranges=192.168.88.10-192.168.88.200
```

Create a DHCP server:

```routeros
/ip dhcp-server add \
    name=lan-dhcp \
    interface=bridge \
    address-pool=lan-pool \
    disabled=no
```

Add the LAN network configuration:

```routeros
/ip dhcp-server network add \
    address=192.168.88.0/24 \
    gateway=192.168.88.1 \
    dns-server=1.1.1.1,8.8.8.8
```

This example assumes the customer LAN bridge already exists and that its DHCP and firewall configuration does not conflict with existing settings.

# 🗁 Verifying the PPPoE Connection

After configuring the server and client, verify that the connection is established.

**Check the PPPoE Client Interface**

```routeros
/interface pppoe-client print detail
```

A successfully connected interface should show the running flag.

**Check Active PPPoE Sessions on the Server**

```routeros
/ppp active print detail
```

This command displays active subscriber sessions, including:

* Username.
* Assigned IP address.
* Session uptime.
* Service type.
* Remote MAC address.
* Session ID.

**Check Subscriber Accounts**

```routeros
/ppp secret print detail
```

**Check PPPoE Server Configuration**

```routeros
/interface pppoe-server server print detail
```

**Check Interface Traffic**

```routeros
/interface monitor-traffic ether2
```

Traffic counters help determine whether subscriber packets are reaching the server interface.

# 🗁 Testing Internet Connectivity

After the PPPoE client connects, test the connection from the customer router.

Test the ISP-side PPP endpoint or another reachable address:

```routeros
/ping 10.10.10.1
```

Test an external IP address:

```routeros
/ping 1.1.1.1
```

Test DNS resolution:

```routeros
:put [:resolve example.com]
```

If the PPPoE session is active but external connectivity fails, investigate routing, firewall rules, NAT, DNS, and upstream connectivity.

# 🗁 Disconnecting a PPPoE Subscriber

An ISP administrator may need to disconnect a subscriber.

First, inspect active sessions:

```routeros
/ppp active print
```

Disconnect a specific subscriber:

```routeros
/ppp active remove [find name=customer001]
```

The subscriber's PPPoE client may automatically attempt to reconnect, depending on its configuration.

To prevent the subscriber from reconnecting, disable the account:

```routeros
/ppp secret disable [find name=customer001]
```

Re-enable the account when access should be restored:

```routeros
/ppp secret enable [find name=customer001]
```

# 🗁 Configuring RADIUS Authentication

For larger ISP networks, maintaining every subscriber account locally becomes difficult.

RADIUS allows the PPPoE server to authenticate subscribers through a centralized server.

Add a RADIUS server:

```routeros
/radius add \
    service=ppp \
    address=10.20.20.10 \
    secret=RadiusSharedSecret \
    authentication-port=1812 \
    accounting-port=1813
```

Enable RADIUS authentication and accounting:

```routeros
/ppp aaa set \
    use-radius=yes \
    accounting=yes \
    interim-update=5m
```

Parameter descriptions:

| Parameter | Description |
| --- | --- |
| `service=ppp` | Applies RADIUS to PPP services |
| `address` | RADIUS server IP address |
| `secret` | Shared secret configured on both devices |
| `authentication-port` | RADIUS authentication port |
| `accounting-port` | RADIUS accounting port |
| `use-radius` | Enables RADIUS authentication |
| `accounting` | Enables accounting |
| `interim-update` | Requests periodic accounting updates |

The RADIUS server must also be configured to recognize the MikroTik router as a RADIUS client.

Network connectivity, firewall permissions, and matching shared secrets are required for successful communication.

When RADIUS is enabled, RouterOS can use locally configured PPP secrets as a fallback according to its authentication behavior and configuration. Test the intended fallback policy before deploying it in production.

# 🗁 Troubleshooting PPPoE Server and Client

| Problem | Possible Cause | Solution |
| --- | --- | --- |
| Client cannot discover server | Wrong interface or VLAN | Verify the server interface and access VLAN |
| Client repeatedly sends PADI | Server is unreachable or not responding | Check interface state, VLAN forwarding, and server configuration |
| Client receives PADO but cannot establish a session | Session request or service configuration problem | Verify service name and server settings |
| Authentication failed | Incorrect username or password | Verify subscriber credentials |
| Subscriber cannot obtain an IP address | Incorrect pool or PPP profile | Check `remote-address` and available pool addresses |
| PPPoE connects but internet does not work | Routing, NAT, or firewall issue | Verify default route, upstream connectivity, and firewall policies |
| Subscriber receives incorrect bandwidth | Wrong profile assignment | Inspect the subscriber's PPP profile |
| RADIUS authentication fails | Incorrect shared secret or unreachable server | Verify RADIUS configuration and connectivity |
| Subscriber disconnects frequently | Unstable access link or session timeout | Inspect logs, physical links, and PPP settings |
| Some websites fail to load | MTU or MSS issue | Check path MTU and TCP MSS handling |

Useful monitoring commands:

```routeros
/log print where topics~"ppp"

/ppp active print detail

/interface pppoe-server server print detail

/radius monitor [find]
```

# 🗁 Security and Operational Best Practices

* Use strong, unique subscriber passwords.
* Prefer CHAP or other supported authentication methods that meet the deployment's security requirements.
* Protect management access to the PPPoE server.
* Restrict subscriber access to management networks.
* Use VLAN segmentation where appropriate.
* Monitor active sessions and available server resources.
* Configure appropriate subscriber session limits.
* Keep RouterOS updated and maintain configuration backups.
* Protect RADIUS shared secrets and restrict RADIUS access to trusted network devices.
* Monitor authentication failures and unusual session activity.
* Use separate PPP profiles for different service packages.
* Test MTU and MSS behavior across the complete access path.

# 🗁 Practical Notes

* A PPPoE server accepts subscriber connections; a PPPoE client initiates them.
* The PPPoE server interface must receive subscriber discovery traffic.
* PPP profiles define shared subscriber settings.
* PPP secrets define individual local subscriber credentials.
* RADIUS provides centralized subscriber authentication and accounting.
* An active PPPoE session does not automatically guarantee internet connectivity.
* The customer's LAN requires its own IP addressing and routing configuration.
* RouterOS v7 commands should be tested against the actual interface and network design before production deployment.

# 🗁 References

* [MikroTik RouterOS — PPP](https://help.mikrotik.com/docs/spaces/ROS/pages/132350049/PPP)
* [MikroTik RouterOS — RADIUS](https://help.mikrotik.com/docs/spaces/ROS/pages/328097/RADIUS)
* [RFC 2516 — A Method for Transmitting PPP Over Ethernet (PPPoE)](https://www.rfc-editor.org/rfc/rfc2516)
* [RFC 1661 — The Point-to-Point Protocol (PPP)](https://www.rfc-editor.org/rfc/rfc1661)
