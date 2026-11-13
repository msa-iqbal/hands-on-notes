# Common Port Numbers

A port number is a 16-bit numerical identifier used by transport-layer protocols to identify application services and communication endpoints.

TCP and UDP use port numbers to help deliver incoming data to the correct application or service on a device.

For example:

* HTTP commonly uses TCP port `80`.
* HTTPS commonly uses TCP port `443`.
* DNS commonly uses UDP and TCP port `53`.
* SSH commonly uses TCP port `22`.

Understanding common port numbers is essential for network administration, firewall configuration, server management, troubleshooting, and security.

# 🗁 Understanding Port Numbers

An IP address identifies a network interface or host at the IP layer, while a port number identifies a transport-layer communication endpoint associated with an application.

Consider the following example:

```text
192.168.1.10:443
```

Where:

* `192.168.1.10` is the destination IP address.
* `443` is the destination port number.

The transport-layer protocol also matters. TCP port `443` and UDP port `443` are separate endpoints.

### 🖆 Port Number Structure

TCP and UDP port numbers are 16-bit unsigned integers.

\[
2^{16} = 65,536
\]

Therefore, the possible port numbers are:

```text
0 – 65535
```

Port `0` is reserved and has special meaning in some APIs and protocol contexts. It is not normally used as a destination service port.

### 🖆 Source and Destination Ports

A transport-layer segment or datagram contains two port fields:

* **Source Port:** Identifies the sending application's transport endpoint.
* **Destination Port:** Identifies the receiving application's transport endpoint.

Example:

```text
Client                         Web Server
192.168.1.20                   93.184.216.34
Port: 51524                    Port: 443
     |                              |
     |-------- TCP Request -------->|
     |                              |
     |<------- TCP Response --------|
```

In this example:

| Field | Value |
| --- | --- |
| Source IP | `192.168.1.20` |
| Source Port | `51524` |
| Destination IP | `93.184.216.34` |
| Destination Port | `443` |
| Transport Protocol | TCP |

The client uses a temporary source port, while the web server listens on its HTTPS service port.

# 🗁 Port Number Ranges

The Internet Assigned Numbers Authority (IANA) divides port numbers into three major ranges.

| Range | Name | Description |
| --- | --- | --- |
| 0–1023 | System / Well-Known Ports | Assigned to widely used standard services |
| 1024–49151 | User / Registered Ports | Registered for particular applications and services |
| 49152–65535 | Dynamic / Private Ports | Available for dynamic or private use |

### 🖆 Well-Known Ports

These ports are commonly associated with standard network services.

Examples:

* HTTP: `80`
* HTTPS: `443`
* SSH: `22`
* DNS: `53`
* SMTP: `25`

On Linux, binding to ports below `1024` normally requires elevated privileges or an appropriate capability.

### 🖆 Registered Ports

These ports are registered for particular applications or services.

Examples:

* MySQL: `3306`
* PostgreSQL: `5432`
* RDP: `3389`
* MongoDB: `27017`

Registration does not guarantee that every application will use its assigned port in every deployment.

### 🖆 Dynamic / Private Ports

These ports are commonly used as temporary client-side source ports.

Example:

```text
Client                         Server
192.168.1.20                   93.184.216.34
Port: 51524                    Port: 443
     |                              |
     |---------- HTTPS ------------>|
```

The operating system can automatically allocate an available ephemeral port for the client connection.

On Linux, inspect the configured ephemeral port range with:

```bash
sysctl net.ipv4.ip_local_port_range
```

A common Linux default is:

```text
32768 60999
```

The actual range depends on the operating system and its configuration. It does not have to match IANA's dynamic/private range exactly.

# 🗁 Common TCP Ports

The following table lists commonly encountered TCP ports.

| Port | Service | Description |
| ---: | --- | --- |
| 20 | FTP Data | Traditional active-mode FTP data connection |
| 21 | FTP Control | FTP command and control connection |
| 22 | SSH | Secure remote administration and file transfer |
| 23 | Telnet | Unencrypted remote terminal access |
| 25 | SMTP | Mail transfer between mail servers |
| 53 | DNS | DNS queries and responses over TCP |
| 80 | HTTP | Unencrypted HTTP communication |
| 110 | POP3 | Email retrieval |
| 111 | RPCBind | RPC service registration and discovery |
| 119 | NNTP | Network News Transfer Protocol |
| 135 | MS RPC | Microsoft RPC endpoint mapper |
| 139 | NetBIOS Session Service | Legacy Windows file and printer sharing |
| 143 | IMAP | Email access and synchronization |
| 389 | LDAP | Directory access |
| 443 | HTTPS | HTTP over TLS; also commonly used by QUIC over UDP |
| 445 | SMB | Windows file and printer sharing |
| 465 | SMTPS | SMTP submission over implicit TLS |
| 587 | SMTP Submission | Email submission by clients |
| 548 | AFP | Apple Filing Protocol |
| 636 | LDAPS | LDAP over TLS |
| 993 | IMAPS | IMAP over implicit TLS |
| 995 | POP3S | POP3 over implicit TLS |
| 1723 | PPTP | Point-to-Point Tunneling Protocol control connection |
| 1883 | MQTT | Message Queuing Telemetry Transport |
| 3306 | MySQL | MySQL database service |
| 3389 | RDP | Remote Desktop Protocol |
| 5432 | PostgreSQL | PostgreSQL database service |
| 5672 | AMQP | Advanced Message Queuing Protocol |
| 5900 | VNC | Remote desktop communication |
| 5985 | WinRM | Windows Remote Management over HTTP |
| 5986 | WinRM | Windows Remote Management over HTTPS |
| 6379 | Redis | Redis database service |
| 6443 | Kubernetes API | Common Kubernetes API server port |
| 8080 | HTTP Alternative | Common alternative HTTP and development port |
| 8443 | HTTPS Alternative | Common alternative HTTPS port |
| 8883 | MQTTS | MQTT over TLS |
| 9000 | Application Service | Commonly used by development and application services |
| 9090 | Application Service | Commonly used by monitoring and management services |
| 9200 | Elasticsearch HTTP | Elasticsearch REST API |
| 27017 | MongoDB | MongoDB database service |

These are common assignments and conventions. Actual service configurations can use different ports.

# 🗁 Common UDP Ports

The following table lists commonly encountered UDP ports.

| Port | Service | Description |
| ---: | --- | --- |
| 53 | DNS | DNS queries and responses |
| 67 | DHCP Server | DHCP server listens for client requests |
| 68 | DHCP Client | DHCP client receives server responses |
| 69 | TFTP | Trivial File Transfer Protocol |
| 111 | RPCBind | RPC service discovery |
| 123 | NTP | Network Time Protocol |
| 137 | NetBIOS Name Service | NetBIOS name resolution |
| 138 | NetBIOS Datagram Service | NetBIOS datagram communication |
| 161 | SNMP | Network monitoring and management requests |
| 162 | SNMP Trap | SNMP notifications and traps |
| 500 | IKE | Internet Key Exchange for IPsec |
| 514 | Syslog | Traditional Syslog transport over UDP |
| 520 | RIP | Routing Information Protocol |
| 1194 | OpenVPN | Common OpenVPN default port |
| 1701 | L2TP | Layer 2 Tunneling Protocol |
| 1812 | RADIUS Authentication | Authentication requests |
| 1813 | RADIUS Accounting | Accounting requests |
| 4500 | IPsec NAT-T | IPsec NAT traversal |
| 5060 | SIP | Session Initiation Protocol |
| 51820 | WireGuard | Common WireGuard default port |
| 5353 | mDNS | Multicast DNS |
| 5355 | LLMNR | Link-Local Multicast Name Resolution |
| 4789 | VXLAN | Virtual Extensible LAN encapsulation |

Some protocols use both TCP and UDP depending on their operation or implementation.

# 🗁 Ports Used by Multiple Transport Protocols

Some services use the same numeric port with both TCP and UDP.

This is possible because TCP and UDP maintain separate port namespaces.

| Port | Service | TCP | UDP |
| ---: | --- | :---: | :---: |
| 53 | DNS | Yes | Yes |
| 67 | DHCP Server | No | Yes |
| 68 | DHCP Client | No | Yes |
| 111 | RPCBind | Yes | Yes |
| 123 | NTP | No | Yes |
| 137 | NetBIOS Name Service | No | Yes |
| 138 | NetBIOS Datagram Service | No | Yes |
| 161 | SNMP | No | Yes |
| 162 | SNMP Trap | No | Yes |
| 389 | LDAP | Yes | Yes |
| 500 | IKE | Yes, in some configurations | Yes |
| 514 | Syslog | TCP is possible; commonly used with UDP | Yes |
| 1194 | OpenVPN | Yes | Yes |
| 5060 | SIP | Yes | Yes |
| 5353 | mDNS | No | Yes |

### 🖆 Why Can TCP and UDP Use the Same Port?

A transport endpoint is identified using the transport protocol along with its addressing information.

For example:

```text
TCP 192.168.1.10:53
UDP 192.168.1.10:53
```

These are different transport endpoints even though the IP address and port number are identical.

This allows a DNS implementation to provide DNS-over-TCP and DNS-over-UDP services using port `53`.

# 🗁 Database and Development Ports

Developers and system administrators frequently encounter application-specific ports.

| Port | Service | Typical Usage |
| ---: | --- | --- |
| 3000 | Development Server | Node.js, React and other development servers |
| 3306 | MySQL | Relational database |
| 4200 | Angular CLI | Angular development server |
| 5000 | Development Server | Flask and other application servers |
| 5173 | Vite | Frontend development server |
| 5432 | PostgreSQL | Relational database |
| 5672 | RabbitMQ / AMQP | Message broker |
| 6379 | Redis | In-memory data store |
| 8000 | Development Server | Django and other application servers |
| 8080 | HTTP Alternative | Application and development server |
| 8443 | HTTPS Alternative | Secure application server |
| 9000 | Application Service | Common development and service port |
| 9090 | Prometheus | Prometheus web interface |
| 9200 | Elasticsearch | Elasticsearch HTTP API |
| 27017 | MongoDB | Document database |

### 🖆 Important Notes

These are common conventions rather than universal requirements.

For example:

* A React development server can run on a different port.
* PostgreSQL can be configured to listen on a custom port.
* Multiple development servers can use different ports on the same machine.
* A container can expose one port internally and publish it using another host port.

### 🖆 Example: Docker Port Mapping

```bash
docker run -d \
  --name mysql-db \
  -p 3307:3306 \
  -e MYSQL_ROOT_PASSWORD=secret \
  mysql
```

The mapping follows this structure:

```text
HOST_PORT:CONTAINER_PORT
```

In this example:

* Host port: `3307`
* Container port: `3306`

A client connecting to the host on port `3307` reaches the MySQL service listening on container port `3306`.

# 🗁 Network Routing and VPN Protocols

Not every networking protocol uses TCP or UDP port numbers.

Routing and tunneling protocols may use IP protocol numbers instead.

| Protocol | IP Protocol Number | Description |
| --- | ---: | --- |
| ICMP | 1 | IPv4 network diagnostics and error reporting |
| TCP | 6 | Transmission Control Protocol |
| UDP | 17 | User Datagram Protocol |
| GRE | 47 | Generic Routing Encapsulation |
| ESP | 50 | IPsec Encapsulating Security Payload |
| AH | 51 | IPsec Authentication Header |
| ICMPv6 | 58 | IPv6 diagnostics and control messages |
| OSPF | 89 | Open Shortest Path First |

### 🖆 Routing Protocol Examples

| Protocol | Transport | Port / Protocol Number |
| --- | --- | --- |
| BGP | TCP | Port 179 |
| RIP | UDP | Port 520 |
| RIPng | UDP | Port 521 |
| OSPF | IP directly | IP protocol 89 |
| EIGRP | IP directly | IP protocol 88 |
| IS-IS | Layer 2 | No TCP/UDP port |

### 🖆 VPN Protocol Examples

| Protocol | Transport | Port / Protocol Number |
| --- | --- | --- |
| OpenVPN | TCP or UDP | Common default port 1194 |
| WireGuard | UDP | Common default port 51820 |
| IPsec IKE | UDP | Port 500 |
| IPsec NAT Traversal | UDP | Port 4500 |
| IPsec ESP | IP directly | IP protocol 50 |
| L2TP | UDP | Port 1701 |
| PPTP Control | TCP | Port 1723 |
| PPTP Data | GRE | IP protocol 47 |
| SSTP | TCP | Port 443 |

### 🖆 Important Distinction

OSPF does not use TCP port `89` or UDP port `89`.

Instead, OSPF is encapsulated directly in IP using IP protocol number `89`.

Similarly, GRE uses IP protocol number `47`, not TCP or UDP port `47`.

# 🗁 Port Numbers vs Protocol Numbers

Port numbers and IP protocol numbers identify different things.

| Feature | Port Number | IP Protocol Number |
| --- | --- | --- |
| Used by | TCP and UDP | IPv4 and IPv6 |
| Purpose | Identifies transport-layer application endpoints | Identifies the protocol carried by IP |
| Size | 16 bits | 8 bits |
| Example | TCP port 443 | OSPF protocol number 89 |
| Assigned by | IANA | IANA |

### 🖆 Example: HTTPS Traffic

Consider a TCP-based HTTPS connection:

```text
IP Header
    Protocol: 6 (TCP)

TCP Header
    Source Port: 51524
    Destination Port: 443

Application Data
    Encrypted HTTPS Traffic
```

The IP protocol field identifies TCP, while the TCP destination port identifies the HTTPS service endpoint.

For HTTP/3, the corresponding transport is QUIC over UDP:

```text
IP Header
    Protocol: 17 (UDP)

UDP Header
    Source Port: 51524
    Destination Port: 443

Application Data
    QUIC / HTTP/3 Traffic
```

# 🗁 How Port Numbers Work

When an application communicates with a remote service, the operating system uses socket information to deliver traffic to the appropriate application endpoint.

### 🖆 Client-Server Communication

Suppose a client accesses a web server.

```text
Client
192.168.1.20
    |
    | Source Port: 51524
    |
    | Destination Port: 443
    |
    v
Web Server
93.184.216.34
    |
    | Listening Port: 443
    |
    v
HTTPS Service
```

The client operating system selects an available local source port, while the server application listens on its configured service port.

### 🖆 TCP Connection Identification

A TCP connection is commonly identified by a four-tuple:

```text
Source IP Address
Source Port
Destination IP Address
Destination Port
```

Example:

```text
192.168.1.20:51524
        |
        v
93.184.216.34:443
```

The transport protocol and relevant networking context also matter when identifying a connection.

Multiple clients can connect to the same server port because their source addresses and/or source ports differ.

### 🖆 Multiple Clients Connecting to One Server

```text
Client A                         Web Server
192.168.1.10:51001  ---------->  93.184.216.34:443

Client B                         Web Server
192.168.1.11:51002  ---------->  93.184.216.34:443

Client C                         Web Server
192.168.1.12:51003  ---------->  93.184.216.34:443
```

All clients connect to destination port `443`, but each connection has a distinct endpoint combination.

# 🗁 Linux Commands for Port Inspection

Linux provides several commands for inspecting listening ports, active connections, and network services.

### 🖆 Display Listening TCP Ports

```bash
ss -tln
```

Options:

* `-t`: TCP sockets.
* `-l`: Listening sockets.
* `-n`: Display numeric addresses and ports.

### 🖆 Display Listening UDP Ports

```bash
ss -uln
```

### 🖆 Display All Listening TCP and UDP Ports

```bash
ss -tuln
```

### 🖆 Display Processes Using Network Ports

```bash
sudo ss -tulpn
```

This displays the processes associated with network sockets where the required permissions are available.

### 🖆 Display Active TCP Connections

```bash
ss -tn
```

### 🖆 Find a Specific Port

For example, inspect TCP port `443`:

```bash
sudo ss -tulpn | grep ':443'
```

Inspect TCP port `22`:

```bash
sudo ss -tulpn | grep ':22'
```

### 🖆 Use lsof

Display network connections and listening sockets:

```bash
sudo lsof -i -n -P
```

Find processes using TCP port `8080`:

```bash
sudo lsof -iTCP:8080 -sTCP:LISTEN
```

Find processes using UDP port `53`:

```bash
sudo lsof -iUDP:53
```

### 🖆 Use netstat

The `netstat` command is part of the `net-tools` package and is considered a legacy alternative to `ss`.

Install it on Ubuntu:

```bash
sudo apt update
sudo apt install net-tools
```

Display listening TCP and UDP ports:

```bash
sudo netstat -tulnp
```

### 🖆 Test Remote Port Connectivity

Use `nc` (Netcat) to test whether a TCP service is reachable.

```bash
nc -vz example.com 443
```

Test SSH:

```bash
nc -vz example.com 22
```

Test a UDP endpoint:

```bash
nc -vzu example.com 53
```

A successful UDP command does not necessarily prove that the remote application is listening, because UDP does not establish a connection.

# 🗁 Firewall Configuration Examples

Firewalls use protocol and port information to control network traffic.

### 🖆 UFW: Allow a TCP Port

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

### 🖆 UFW: Allow a UDP Port

Allow DNS over UDP:

```bash
sudo ufw allow 53/udp
```

Allow WireGuard:

```bash
sudo ufw allow 51820/udp
```

### 🖆 UFW: Allow Both TCP and UDP

Allow DNS over both protocols:

```bash
sudo ufw allow 53
```

When no protocol is specified, UFW normally creates rules for both TCP and UDP.

### 🖆 UFW: Deny a Port

Deny TCP port `23`:

```bash
sudo ufw deny 23/tcp
```

### 🖆 UFW: Check Firewall Rules

```bash
sudo ufw status verbose
```

### 🖆 UFW: Delete a Rule

```bash
sudo ufw delete allow 443/tcp
```

# 🗁 Common Port-Related Problems

### 🖆 Port Already in Use

An application may fail to start because another process is already listening on the requested port.

Example error:

```text
Address already in use
```

Find the process:

```bash
sudo ss -tulpn | grep ':8080'
```

Or:

```bash
sudo lsof -i :8080
```

### 🖆 Connection Refused

A connection-refused error commonly indicates that the destination host or an intermediate device actively rejected the connection.

Possible causes:

* No service is listening on the destination port.
* The service is bound to a different IP address.
* A firewall actively rejects the connection.
* The service is not running.

### 🖆 Connection Timeout

A connection timeout means the connection attempt did not receive a response within the expected time.

Possible causes:

* Firewall silently dropping packets.
* Incorrect routing.
* Network connectivity problems.
* Destination host unavailable.
* Service or network infrastructure not responding.

### 🖆 Wrong Transport Protocol

A service may use UDP while the client attempts TCP, or vice versa.

For example:

```bash
nc -vz example.com 53
```

This tests TCP port `53`.

To test UDP:

```bash
nc -vzu example.com 53
```

The transport protocol must match the service's expected communication method.

# 🗁 Security Best Practices

* Expose only the ports required by your applications.
* Disable unnecessary services and listeners.
* Avoid exposing database ports directly to public networks.
* Restrict SSH access using firewall rules and appropriate authentication.
* Prefer encrypted protocols such as SSH, HTTPS, and secure mail protocols.
* Monitor listening ports and unexpected network connections.
* Use network segmentation to isolate sensitive services.
* Apply firewall rules based on source, destination, protocol, and port.
* Change default application configurations when appropriate, but do not treat changing a port as a replacement for proper security.
* Regularly review firewall rules and service exposure.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is a port number?**

A port number is a 16-bit identifier used by TCP and UDP to identify application communication endpoints.

**How many port numbers are available?**

There are 65,536 possible values, ranging from `0` to `65535`.

**What are the three port-number ranges?**

* Well-known: `0–1023`
* Registered: `1024–49151`
* Dynamic/private: `49152–65535`

**What is the default HTTP port?**

TCP port `80`.

**What is the default HTTPS port?**

TCP port `443`; HTTP/3 commonly uses UDP port `443` through QUIC.

**What is the default SSH port?**

TCP port `22`.

**What is the default DNS port?**

Port `53`, using both UDP and TCP.

### 🖆 Intermediate Questions

**What is an ephemeral port?**

An ephemeral port is a temporary port commonly selected by the operating system for client-side communication.

**Can TCP and UDP use the same port number?**

Yes. They have separate port namespaces. For example, TCP port `53` and UDP port `53` are different endpoints.

**What is the difference between a port number and an IP address?**

An IP address identifies a network interface or host at the IP layer, while a port number identifies a transport-layer application endpoint.

**What is the difference between a listening port and an ephemeral port?**

A listening port is associated with a service waiting for incoming communication. An ephemeral port is commonly allocated temporarily for client-side communication.

**Which command displays listening ports on Linux?**

```bash
ss -tuln
```

### 🖆 Advanced Questions

**Does OSPF use TCP or UDP port 89?**

No. OSPF uses IP protocol number `89` directly.

**Does GRE use TCP or UDP port 47?**

No. GRE uses IP protocol number `47`.

**Why can multiple clients connect to the same server port?**

Each connection has a distinct endpoint combination, commonly identified by its source IP, source port, destination IP, and destination port.

**Is UDP port scanning always reliable?**

No. UDP services may not respond to arbitrary probes. A missing response can mean the port is open, filtered, or simply not responding.

**Can a service listen on multiple ports?**

Yes. An application can listen on multiple ports, and different applications can use different ports on the same host.

# 🗁 Summary

Port numbers identify transport-layer application endpoints and enable multiple network services to communicate through the same IP address.

Key points:

* TCP and UDP use 16-bit port numbers.
* The possible port-number range is `0–65535`.
* Well-known ports range from `0–1023`.
* Registered ports range from `1024–49151`.
* Dynamic/private ports range from `49152–65535`.
* TCP and UDP maintain separate port namespaces.
* Common services use standardized or widely recognized port numbers.
* Some networking protocols use IP protocol numbers instead of TCP or UDP ports.
* Linux tools such as `ss`, `lsof`, `nc`, and `tcpdump` help inspect and troubleshoot network ports.
* Firewalls use port and protocol information to control network access.

# 🗁 References

* [IANA — Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
* [RFC 6335 — Internet Assigned Numbers Authority (IANA) Procedures for the Management of the Service Name and Transport Protocol Port Number Registry](https://www.rfc-editor.org/rfc/rfc6335)
* [RFC 9293 — Transmission Control Protocol (TCP)](https://www.rfc-editor.org/rfc/rfc9293)
* [RFC 768 — User Datagram Protocol (UDP)](https://www.rfc-editor.org/rfc/rfc768)
* [RFC 9000 — QUIC: A UDP-Based Multiplexed and Secure Transport](https://www.rfc-editor.org/rfc/rfc9000)
* [Linux `ss` Manual](https://man7.org/linux/man-pages/man8/ss.8.html)
* [Linux `lsof` Manual](https://man7.org/linux/man-pages/man8/lsof.8.html)
* [Ubuntu UFW Documentation](https://documentation.ubuntu.com/server/how-to/security/firewalls/)
