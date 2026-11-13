# PPPoE Client

A PPPoE client establishes a Point-to-Point Protocol over Ethernet connection to a PPPoE server.

In an ISP network, the PPPoE client is commonly configured on a subscriber's router or customer-premises equipment (CPE). It connects to the ISP's PPPoE server using a username and password.

After successful authentication, the client establishes a PPP session and receives network configuration, such as an IP address and DNS servers.

MikroTik RouterOS provides PPPoE client functionality through `/interface pppoe-client`.

# 🗁 What Is a PPPoE Client?

A PPPoE client is the device or interface that initiates a PPPoE connection to a server.

It performs the following operations:

* Discovers an available PPPoE server.
* Requests a PPPoE session.
* Authenticates using subscriber credentials.
* Negotiates PPP parameters.
* Receives an IP address and other network settings.
* Transmits and receives traffic through the established session.

A typical ISP connection looks like this:

```text
                 Internet
                    |
              +-----------+
              | ISP Router|
              | MikroTik  |
              +-----------+
                    |
              PPPoE Server
                    |
              Ethernet Link
                    |
              +-----------+
              | Customer  |
              | Router    |
              | MikroTik  |
              +-----------+
                    |
                 Local LAN
                    |
              +-----------+
              | Computer  |
              +-----------+
```

The customer router acts as the PPPoE client, while the ISP router acts as the PPPoE server.

# 🗁 PPPoE Client Requirements

Before configuring the client, ensure that:

* The Ethernet interface has physical connectivity to the ISP network.
* The ISP has an active PPPoE server.
* You have the correct PPPoE username and password.
* The ISP's PPPoE service is reachable through the connected Ethernet or VLAN.
* The router has an appropriate LAN interface for local devices.

Example configuration:

| Component | Configuration |
| --- | --- |
| WAN physical interface | ether1 |
| PPPoE interface | pppoe-out1 |
| PPPoE username | customer001 |
| PPPoE password | Subscriber password |
| ISP service name | ISP-PPPoE |
| LAN interface | bridge-LAN |
| Address assignment | Dynamic |

# 🗁 Check Available Interfaces

Inspect the router's interfaces:

```routeros
/interface print
```

Verify that the Ethernet interface connected to the ISP is available.

For example:

```text
ether1
ether2
ether3
ether4
ether5
```

In this example, `ether1` is connected to the ISP network.

The PPPoE client should be created on the physical interface or logical interface that carries the ISP's PPPoE service.

# 🗁 Configure the PPPoE Client

Create a PPPoE client interface:

```routeros
/interface pppoe-client
add name=pppoe-out1 \
    interface=ether1 \
    user=customer001 \
    password=StrongPassword001 \
    add-default-route=yes \
    use-peer-dns=yes \
    disabled=no
```

Important properties:

| Property | Description |
| --- | --- |
| `name` | PPPoE client interface name |
| `interface` | Physical or logical interface used for the connection |
| `user` | PPPoE subscriber username |
| `password` | PPPoE subscriber password |
| `add-default-route` | Automatically installs a default route |
| `use-peer-dns` | Uses DNS servers supplied by the PPPoE server |
| `disabled` | Enables or disables the client |

When authentication succeeds, the PPPoE interface should become operational.

# 🗁 Configure the PPPoE Service Name

Some ISPs configure a specific PPPoE service name.

You can specify it when creating the client:

```routeros
/interface pppoe-client
add name=pppoe-out1 \
    interface=ether1 \
    user=customer001 \
    password=StrongPassword001 \
    service-name=ISP-PPPoE \
    add-default-route=yes \
    use-peer-dns=yes \
    disabled=no
```

The service name helps the client select the intended PPPoE service.

If the ISP does not require a specific service name, leave the field empty.

# 🗁 Configure the PPPoE Client on a VLAN

Some ISPs deliver PPPoE through a tagged VLAN.

In this situation, create a VLAN interface on the physical WAN interface and run the PPPoE client over that VLAN.

Example: ISP provides PPPoE service on VLAN ID `100`.

Create the VLAN interface:

```routeros
/interface vlan
add name=vlan100-isp \
    interface=ether1 \
    vlan-id=100
```

Create the PPPoE client on the VLAN interface:

```routeros
/interface pppoe-client
add name=pppoe-out1 \
    interface=vlan100-isp \
    user=customer001 \
    password=StrongPassword001 \
    add-default-route=yes \
    use-peer-dns=yes \
    disabled=no
```

The traffic flow becomes:

```text
       ISP Ethernet Connection
                 |
               ether1
                 |
          VLAN 100 Interface
                 |
           vlan100-isp
                 |
           PPPoE Client
                 |
            pppoe-out1
                 |
           Router Internet
```

The VLAN ID must match the ISP's configuration.

Do not create a VLAN interface if the ISP provides untagged PPPoE service.

# 🗁 Default Route Configuration

The PPPoE client can automatically install a default route after a successful connection.

Enable automatic default-route installation:

```routeros
/interface pppoe-client
set pppoe-out1 add-default-route=yes
```

Inspect the routing table:

```routeros
/ip route print detail
```

A dynamically installed default route should appear with the PPPoE interface as its gateway.

You can also configure the default route manually:

```routeros
/interface pppoe-client
set pppoe-out1 add-default-route=no

/ip route
add dst-address=0.0.0.0/0 gateway=pppoe-out1
```

Manual route configuration is useful when you need to control route distance, routing tables, or multi-WAN behavior.

Avoid installing an unintended duplicate default route.

# 🗁 DNS Configuration

The PPPoE server can provide DNS server addresses to the client.

Enable the use of peer DNS:

```routeros
/interface pppoe-client
set pppoe-out1 use-peer-dns=yes
```

Inspect DNS configuration:

```routeros
/ip dns print
```

Alternatively, configure DNS servers manually:

```routeros
/ip dns
set servers=1.1.1.1,8.8.8.8 allow-remote-requests=yes
```

When `allow-remote-requests=yes` is enabled, protect the router's DNS service with firewall rules so it is accessible only from trusted networks.

# 🗁 Configure LAN Internet Access

A customer router usually provides internet connectivity to devices connected to its LAN.

Assume:

* WAN: `pppoe-out1`
* LAN bridge: `bridge-LAN`
* LAN network: `192.168.10.0/24`

Assign an IP address to the LAN bridge:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

Configure a DHCP server on the LAN if one is not already present:

```routeros
/ip pool
add name=lan-pool ranges=192.168.10.100-192.168.10.200

/ip dhcp-server
add name=dhcp-lan interface=bridge-LAN address-pool=lan-pool disabled=no

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

Configure source NAT:

```routeros
/ip firewall nat
add chain=srcnat \
    out-interface=pppoe-out1 \
    action=masquerade \
    comment="NAT LAN traffic through PPPoE"
```

Ensure the forward-chain firewall policy permits LAN traffic to reach the PPPoE interface.

If the router already has a working LAN, DHCP, NAT, and firewall configuration, do not duplicate those settings.

# 🗁 MTU and MRU Configuration

PPPoE adds encapsulation overhead to Ethernet frames.

With standard Ethernet MTU of 1500 bytes, the typical maximum PPPoE IP MTU is 1492 bytes.

Inspect the PPPoE client:

```routeros
/interface pppoe-client
print detail
```

You can configure MTU and MRU explicitly when required:

```routeros
/interface pppoe-client
set pppoe-out1 max-mtu=1492 max-mru=1492
```

The correct values depend on the ISP access network and end-to-end frame-size support.

If the ISP supports larger Ethernet frames, higher PPPoE MTU values may be possible. Do not assume that every network supports them.

# 🗁 Monitor PPPoE Client Status

Inspect PPPoE interfaces:

```routeros
/interface pppoe-client print
```

Display detailed information:

```routeros
/interface pppoe-client print detail
```

Monitor the interface:

```routeros
/interface monitor-traffic pppoe-out1
```

Check assigned IP addresses:

```routeros
/ip address print
```

Inspect the default route:

```routeros
/ip route print detail
```

Test connectivity to a public IP address:

```routeros
/ping 1.1.1.1
```

Test DNS resolution:

```routeros
/ping google.com
```

If the public IP test succeeds but DNS resolution fails, investigate DNS configuration.

# 🗁 Enable and Disable the PPPoE Client

Disable the PPPoE client:

```routeros
/interface pppoe-client
disable pppoe-out1
```

Enable it again:

```routeros
/interface pppoe-client
enable pppoe-out1
```

Disabling the interface terminates its active connection. Enabling it allows the client to attempt reconnection.

# 🗁 Configure Automatic Reconnection

PPPoE clients normally attempt to reconnect when the session is lost.

Inspect the client configuration:

```routeros
/interface pppoe-client
print detail
```

The `keepalive-timeout` property controls how long the client waits without receiving keepalive responses before considering the connection unavailable.

Example:

```routeros
/interface pppoe-client
set pppoe-out1 keepalive-timeout=10
```

Choose the timeout based on the ISP's behavior and network requirements. An overly aggressive timeout can cause unnecessary session reconnections during temporary network interruptions.

# 🗁 PPPoE Client Troubleshooting

| Problem | Possible cause | What to check |
| --- | --- | --- |
| Client does not connect | Physical link unavailable | Ethernet link and cabling |
| No PPPoE server found | Incorrect interface or VLAN | ISP service path and VLAN ID |
| Authentication failed | Incorrect credentials | Username and password |
| Client connects but has no internet | Missing default route | PPPoE route configuration |
| IP connectivity works but domain names fail | DNS issue | Peer DNS and router DNS configuration |
| LAN devices have no internet | NAT or firewall issue | Masquerade and forward-chain rules |
| Connection frequently drops | ISP instability or keepalive behavior | Logs, physical link, and PPPoE session |
| Some websites or large transfers fail | MTU or MSS issue | PPPoE MTU, MRU, and path MTU |
| Client receives no IP address | Server profile or pool issue | ISP-side PPP profile and address pool |

Enable PPP debugging:

```routeros
/system logging
add topics=ppp,debug action=memory
```

Inspect PPP-related logs:

```routeros
/log print where topics~"ppp"
```

Disable verbose debugging when troubleshooting is complete.

# 🗁 Security Best Practices

* Protect PPPoE credentials and use strong passwords.
* Avoid sharing subscriber credentials between devices.
* Keep RouterOS updated.
* Use firewall rules to protect router management services.
* Do not expose DNS or management services to untrusted WAN networks.
* Monitor connection status and unexpected disconnections.
* Use the ISP-provided VLAN configuration where required.
* Keep a backup of the working router configuration.
* Avoid storing subscriber credentials in publicly accessible scripts or repositories.

# 🗁 Practical Lab

Build a PPPoE client environment using two MikroTik routers.

**Topology**

```text
              Router A
           PPPoE Server
                  |
              ether2
                  |
              ether1
              Router B
           PPPoE Client
                  |
              bridge-LAN
                  |
             LAN Computer
```

**Router A requirements**

* Configure a PPPoE server on `ether2`.
* Create a subscriber IP pool.
* Create a PPP profile.
* Create a subscriber account.
* Verify that the PPPoE server is running.

**Router B requirements**

* Connect `ether1` to Router A.
* Create a PPPoE client named `pppoe-out1`.
* Configure the subscriber credentials.
* Enable automatic default-route installation.
* Enable peer DNS.
* Verify successful authentication.

**Verification tasks**

* Check that the PPPoE client becomes operational.
* Verify the assigned IP address.
* Confirm that a default route is installed.
* Test internet connectivity.
* Configure a LAN bridge and DHCP server.
* Configure NAT for LAN devices.
* Test internet access from a LAN computer.
* Disable and re-enable the PPPoE client.
* Inspect logs during connection and reconnection.

# 🗁 Key Takeaways

* A PPPoE client initiates a point-to-point session with a PPPoE server.
* RouterOS creates PPPoE client interfaces through `/interface pppoe-client`.
* Authentication requires the subscriber credentials configured on the server.
* The client can obtain an IP address and DNS settings from the server.
* PPPoE can operate over a physical Ethernet interface or a VLAN interface.
* Automatic default-route installation simplifies basic WAN configuration.
* LAN devices require appropriate routing, NAT, and firewall configuration.
* Standard Ethernet commonly limits PPPoE IP MTU to 1492 bytes.
* RouterOS provides commands to monitor, disable, enable, and troubleshoot PPPoE sessions.
