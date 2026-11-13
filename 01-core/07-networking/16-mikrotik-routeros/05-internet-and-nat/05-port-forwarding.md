# Port Forwarding

Port forwarding is a networking technique that allows external clients to access services running on devices inside a private network.

In MikroTik RouterOS, port forwarding is implemented using Destination Network Address Translation (Destination NAT or DNAT).

It allows incoming connections to a public IP address and port to be redirected to a specific internal IP address and port.

Port forwarding is commonly used to publish web servers, application servers, remote access services, game servers, and other network applications.

# 🗁 What Is Port Forwarding?

Devices inside a private LAN usually use private IP addresses that cannot be directly reached from the public internet.

For example:

```text
Public IP: 203.0.113.10

Internal Server:
IP Address: 192.168.10.10
Service: HTTP
Port: 80
```

An external client cannot normally connect directly to `192.168.10.10` because it is a private IP address.

Port forwarding allows the client to connect to the router's public IP address instead.

```text
External Client
      |
      | 203.0.113.10:80
      |
      v
MikroTik Router
      |
      | Destination NAT
      | 203.0.113.10:80
      |          |
      |          v
      | 192.168.10.10:80
      |
      v
Internal Web Server
```

The router translates the destination address and forwards the connection to the internal server.

# 🗁 How Port Forwarding Works

Consider an external client accessing a web server inside a private network.

The connection process is:

* The external client sends a request to the router's public IP address and destination port.
* The packet reaches the MikroTik router through its WAN interface.
* RouterOS checks the Destination NAT rules.
* A matching rule changes the destination IP address and possibly the destination port.
* The router forwards the packet to the internal server.
* The server processes the request and sends a response.
* Connection tracking maintains the NAT translation.
* The response returns through the router and reaches the external client.

# 🗁 Port Forwarding Terminology

| Term | Description |
| --- | --- |
| Public IP | IP address used by external clients to reach the router |
| Private IP | Internal address of the destination server |
| External Port | Port exposed on the public side |
| Internal Port | Port on which the internal service listens |
| Protocol | Transport protocol, usually TCP or UDP |
| Destination NAT | Router operation that changes the destination address or port |
| Connection Tracking | Maintains connection state and NAT translations |
| WAN Interface | Interface connected toward the upstream network |
| LAN Interface | Interface connected to internal devices |

# 🗁 Basic Port Forwarding Configuration

Assume the following network:

| Component | Configuration |
| --- | --- |
| WAN interface | `ether1` |
| LAN interface | `ether2` |
| Public IP | `203.0.113.10` |
| Internal server | `192.168.10.10` |
| Service | HTTP |
| External port | `80` |
| Internal port | `80` |
| Protocol | TCP |

Create the port forwarding rule:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Forward HTTP to Web Server"
```

This rule forwards incoming TCP connections on port `80` to the internal server at `192.168.10.10:80`.

# 🗁 Forwarding HTTPS Traffic

HTTPS normally uses TCP port `443`.

To publish an internal HTTPS server:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=443 action=dst-nat to-addresses=192.168.10.10 to-ports=443 comment="Forward HTTPS to Web Server"
```

External clients can then access the service using:

```text
https://203.0.113.10
```

The internal server must have HTTPS configured and listening on port `443`.

# 🗁 External Port and Internal Port Mapping

The external port does not have to match the internal service port.

For example, an external client can connect to port `8080`, while the internal server listens on port `80`.

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="External 8080 to Internal 80"
```

Connection mapping:

| External Destination | Internal Destination |
|---|---|
| `203.0.113.10:8080` | `192.168.10.10:80` |

The external client connects to:

```text
http://203.0.113.10:8080
```

RouterOS forwards the connection to the internal server's port `80`.

# 🗁 Forwarding SSH Connections

Suppose an internal Linux server provides SSH access on TCP port `22`.

You want external clients to connect through public TCP port `2222`.

Configuration:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=2222 action=dst-nat to-addresses=192.168.10.20 to-ports=22 comment="Forward SSH to Linux Server"
```

External connection:

```bash
ssh -p 2222 username@203.0.113.10
```

The router translates the external destination port `2222` to internal port `22`.

# 🗁 Forwarding UDP Traffic

Port forwarding is not limited to TCP.

Some applications use UDP, including DNS, gaming, voice communication, and certain VPN services.

For example, forward UDP port `51820` to an internal WireGuard server:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=udp dst-port=51820 action=dst-nat to-addresses=192.168.10.30 to-ports=51820 comment="Forward WireGuard"
```

The internal WireGuard server should listen on UDP port `51820`.

# 🗁 Forwarding Multiple Ports

A server may provide several services.

Example:

| Service | Protocol | External Port | Internal Port |
| --- | --- | ---: | ---: |
| HTTP | TCP | 80 | 80 |
| HTTPS | TCP | 443 | 443 |
| SSH | TCP | 2222 | 22 |
| WireGuard | UDP | 51820 | 51820 |

Create separate NAT rules:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="HTTP"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=443 action=dst-nat to-addresses=192.168.10.10 to-ports=443 comment="HTTPS"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=2222 action=dst-nat to-addresses=192.168.10.20 to-ports=22 comment="SSH"

add chain=dstnat in-interface=ether1 protocol=udp dst-port=51820 action=dst-nat to-addresses=192.168.10.30 to-ports=51820 comment="WireGuard"
```

Each rule forwards a specific service to its corresponding internal server.

# 🗁 Forwarding Traffic to Multiple Internal Servers

A single public IP address can publish services hosted on different internal servers by using different external ports.

Example:

| External Port | Internal Server | Internal Port |
| ---: | --- | ---: |
| 8080 | `192.168.10.10` | 80 |
| 8081 | `192.168.10.20` | 80 |
| 8082 | `192.168.10.30` | 80 |

Configuration:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Web Server 1"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8081 action=dst-nat to-addresses=192.168.10.20 to-ports=80 comment="Web Server 2"

add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8082 action=dst-nat to-addresses=192.168.10.30 to-ports=80 comment="Web Server 3"
```

This approach allows multiple internal servers to share one public IP address while using different external ports.

# 🗁 Restricting Port Forwarding by Source IP

Sometimes a service should only be accessible from specific external IP addresses.

For example, allow SSH connections only from `198.51.100.25`.

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 src-address=198.51.100.25 protocol=tcp dst-port=2222 action=dst-nat to-addresses=192.168.10.20 to-ports=22 comment="SSH from Trusted IP"
```

This rule only matches incoming SSH forwarding requests from the specified source address.

You can also use a source address list for multiple trusted addresses.

Create an address list:

```routeros
/ip/firewall/address-list
add list=trusted-remote address=198.51.100.25 comment="Office IP"
add list=trusted-remote address=198.51.100.30 comment="Administrator IP"
```

Use the address list in the NAT rule:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 src-address-list=trusted-remote protocol=tcp dst-port=2222 action=dst-nat to-addresses=192.168.10.20 to-ports=22 comment="Restricted SSH Forwarding"
```

Source restrictions can also be enforced in the firewall filter rules. NAT matching alone should not be treated as a complete security policy.

# 🗁 Firewall Filter Rule for Port Forwarding

Destination NAT translates the packet destination, but the firewall filter still controls whether the forwarded packet is allowed.

A typical forward-chain rule for an HTTP server is:

```routeros
/ip/firewall/filter
add chain=forward in-interface=ether1 dst-address=192.168.10.10 protocol=tcp dst-port=80 connection-nat-state=dstnat action=accept comment="Allow Forwarded HTTP"
```

The `connection-nat-state=dstnat` matcher identifies connections that have undergone Destination NAT.

Make sure this rule is placed before a general rule that drops forwarded traffic.

A common firewall policy also accepts established and related connections:

```routeros
/ip/firewall/filter
add chain=forward connection-state=established,related action=accept comment="Allow Established and Related"
add chain=forward connection-state=invalid action=drop comment="Drop Invalid Connections"
```

Integrate these rules with the existing firewall policy rather than blindly appending duplicate or conflicting rules.

# 🗁 Hairpin NAT for Internal Access

Hairpin NAT allows LAN clients to access an internal server using the router's public IP address.

For example:

```text
LAN Client:       192.168.10.20
Router Public IP: 203.0.113.10
Internal Server:  192.168.10.10
```

The client requests:

```text
http://203.0.113.10
```

The router must translate the destination to the internal server.

Destination NAT rule:

```routeros
/ip/firewall/nat
add chain=dstnat src-address=192.168.10.0/24 dst-address=203.0.113.10 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Hairpin DNAT"
```

Source NAT rule:

```routeros
/ip/firewall/nat
add chain=srcnat src-address=192.168.10.0/24 dst-address=192.168.10.10 protocol=tcp dst-port=80 action=masquerade comment="Hairpin SNAT"
```

The Source NAT rule ensures that the server returns the response through the router, allowing connection tracking to reverse both translations correctly.

An alternative is to use internal DNS so that the service hostname resolves directly to the server's private IP address for LAN clients.

# 🗁 Port Forwarding with a WAN Interface List

Instead of matching a single WAN interface, you can use an interface list.

Create the WAN list if it does not already exist:

```routeros
/interface/list
add name=WAN
```

Add the WAN interface:

```routeros
/interface/list/member
add list=WAN interface=ether1
```

Configure the forwarding rule:

```routeros
/ip/firewall/nat
add chain=dstnat in-interface-list=WAN protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="HTTP Port Forwarding"
```

This approach is useful when a router has multiple WAN interfaces.

Ensure that the public destination address and routing design are appropriate for each WAN connection.

# 🗁 Port Forwarding Behind Another Router

Sometimes the MikroTik router does not receive the public IP address directly.

Example:

```text
Internet
   |
ISP Router
Public IP: 203.0.113.10
   |
   | Private WAN: 192.168.1.2
   |
MikroTik Router
WAN: 192.168.1.2
LAN: 192.168.10.1
   |
Internal Server
192.168.10.10
```

In this situation, forwarding must be configured on both devices:

* The ISP router forwards the required external port to the MikroTik WAN address.
* The MikroTik router forwards that port to the internal server.

For example:

```text
ISP Router:
203.0.113.10:80 -> 192.168.1.2:80

MikroTik Router:
192.168.1.2:80 -> 192.168.10.10:80
```

This is a double-NAT environment.

If the upstream router supports bridge mode or a suitable public IP routing configuration, those options may provide a simpler network design.

# 🗁 Port Forwarding Troubleshooting

If a forwarded service is unreachable, investigate each part of the connection path.

**Check the public WAN address**

```routeros
/ip/address/print
```

Confirm that the MikroTik router has the expected public address or that the upstream network correctly forwards traffic to it.

**Check the NAT rules**

```routeros
/ip/firewall/nat/print detail
```

Verify the incoming interface, protocol, external port, internal address, and internal port.

**Check NAT counters**

```routeros
/ip/firewall/nat/print stats
```

Increasing packet counters indicate that matching traffic is reaching the NAT rule.

**Check firewall filter rules**

```routeros
/ip/firewall/filter/print
```

Confirm that the forwarded connection is allowed by the `forward` chain.

**Check connection tracking**

```routeros
/ip/firewall/connection/print
```

Inspect active connections to determine whether requests are being translated and tracked.

**Check the internal server**

Verify that:

* The server is powered on and reachable.
* The service is running.
* The service listens on the expected IP address and port.
* The server firewall permits incoming connections.
* The server has a valid return route.

**Test from an external network**

Test using a device outside the LAN, such as a mobile connection.

Testing the public address from inside the same LAN may require Hairpin NAT or an appropriate internal DNS configuration.

**Check ISP restrictions**

Some ISPs use Carrier-Grade NAT (CGNAT), which prevents customers from receiving unsolicited inbound connections through a shared public IPv4 address.

If the MikroTik WAN address is private or belongs to the shared CGNAT range `100.64.0.0/10`, ordinary port forwarding on the MikroTik alone will not make the service reachable from the public internet.

# 🗁 Important Considerations

* Port forwarding is implemented using Destination NAT.
* The internal server should have a stable IP address, commonly through a static configuration or DHCP reservation.
* The server's default gateway must provide a valid return path.
* Firewall filter rules must permit the forwarded connection.
* Use specific protocols, ports, destination addresses, and source restrictions where appropriate.
* Do not expose router management services unnecessarily.
* Avoid forwarding the same public IP and protocol port to multiple internal servers simultaneously.
* An ISP may block inbound traffic or use CGNAT.
* Port forwarding does not encrypt application traffic. Use appropriate application-layer security, such as HTTPS or a VPN.
* NAT and firewall rules should be tested from an external network.

# 🗁 Practical Lab

Configure a MikroTik router to forward external HTTP traffic to an internal web server.

**Topology:**

```text
                 Internet
                     |
                     |
              Public IP Address
                203.0.113.10
                     |
                  ether1
                     |
              MikroTik Router
                     |
                  ether2
                     |
              LAN: 192.168.10.0/24
                     |
                 Web Server
                192.168.10.10
                   Port 80
```

**Network requirements:**

| Component | Configuration |
| --- | --- |
| WAN interface | `ether1` |
| LAN interface | `ether2` |
| LAN gateway | `192.168.10.1` |
| Web server | `192.168.10.10` |
| External port | TCP `8080` |
| Internal port | TCP `80` |

**Configure the LAN gateway:**

```routeros
/ip/address
add address=192.168.10.1/24 interface=ether2 comment="LAN Gateway"
```

**Create the port forwarding rule:**

```routeros
/ip/firewall/nat
add chain=dstnat in-interface=ether1 protocol=tcp dst-port=8080 action=dst-nat to-addresses=192.168.10.10 to-ports=80 comment="Lab HTTP Port Forwarding"
```

**Allow the forwarded traffic:**

```routeros
/ip/firewall/filter
add chain=forward in-interface=ether1 dst-address=192.168.10.10 protocol=tcp dst-port=80 connection-nat-state=dstnat action=accept comment="Allow Lab Web Server"
```

Place the allow rule before any general forward-chain drop rule.

**Verify NAT counters:**

```routeros
/ip/firewall/nat/print stats
```

**Test from an external network:**

```text
http://203.0.113.10:8080
```

The router should forward the request to:

```text
192.168.10.10:80
```

**Lab exercises:**

* Forward HTTPS traffic to an internal web server.
* Forward SSH using a different external port.
* Forward UDP traffic to an internal application.
* Restrict SSH forwarding to a trusted external IP address.
* Configure Hairpin NAT and test access from the LAN.
* Test port forwarding when the MikroTik router is behind another NAT device.
* Identify why port forwarding fails when the WAN connection uses CGNAT.

# 🗁 Summary

Port forwarding allows external clients to access selected services inside a private network through a router's public IP address.

In MikroTik RouterOS, it is implemented using Destination NAT rules in the `dstnat` chain.

A successful port forwarding configuration requires correct NAT translation, firewall filter rules, internal server configuration, return routing, and a reachable public WAN connection.
