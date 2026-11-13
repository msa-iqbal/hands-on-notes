# Common Network Port Numbers

Port numbers identify application endpoints used by transport-layer protocols such as TCP and UDP. They help operating systems and network devices deliver incoming traffic to the correct application or service.

This document provides a practical reference to commonly used network ports, their protocols, security considerations, and troubleshooting methods.

# 🗁 What Is a Network Port?

A network port is a logical communication endpoint associated with a transport protocol.

When an application communicates over TCP or UDP, the transport header contains:

* Source port: Identifies the sending application endpoint.
* Destination port: Identifies the receiving application endpoint.

**Example:**

```text
Source IP:        192.168.10.20
Source Port:      51524

Destination IP:   192.168.10.100
Destination Port: 443

Transport:        TCP
```

In this example, a client at `192.168.10.20` connects to an HTTPS service listening on TCP port `443` at `192.168.10.100`.

# 🗁 Port Number Range

TCP and UDP each use a 16-bit port number field, providing 65,536 possible values.

| Range | Category | Description |
| --- | --- | --- |
| 0 | Reserved | Not normally assigned to an application service |
| 0–1023 | System Ports (Well-Known Ports) | Common standard services |
| 1024–49151 | User Ports (Registered Ports) | Registered application services |
| 49152–65535 | Dynamic / Private Ports | Commonly used for temporary client-side ports |

**Important:** The IANA port ranges describe registration categories. Operating systems may use different ephemeral port ranges, and applications can be configured to listen on non-default ports.

# 🗁 TCP and UDP Ports

TCP and UDP have separate port namespaces.

For example:

* TCP port 53 can be used by DNS.
* UDP port 53 can also be used by DNS.

These are separate transport endpoints.

| Feature | TCP | UDP |
| --- | --- | --- |
| Connection | Connection-oriented | Connectionless |
| Reliability | Built-in retransmission and ordering | No built-in delivery guarantee |
| Header Size | 20 bytes minimum | 8 bytes |
| Port Numbers | 0–65535 | 0–65535 |
| Common Uses | SSH, HTTPS, SMTP | DNS, DHCP, NTP |

# 🗁 Common Well-Known Ports

### 🖆 Remote Access and Management

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 20 | TCP | FTP Data | Traditional active-mode FTP data connection |
| 21 | TCP | FTP Control | FTP command and control connection |
| 22 | TCP | SSH | Secure remote administration and file transfer |
| 23 | TCP | Telnet | Unencrypted remote terminal access |
| 49 | TCP | TACACS+ | Centralized device administration authentication |
| 161 | UDP | SNMP | Network monitoring requests and responses |
| 162 | UDP | SNMP Trap | SNMP notifications |
| 443 | TCP | HTTPS | Secure web communication |
| 3389 | TCP/UDP | RDP | Remote Desktop Protocol |

### 🖆 Web Services

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 80 | TCP | HTTP | Standard web communication |
| 443 | TCP | HTTPS | HTTP protected by TLS |
| 443 | UDP | QUIC / HTTP/3 | Modern HTTP transport |
| 8080 | TCP | HTTP Alternative | Common alternative HTTP port |
| 8443 | TCP | HTTPS Alternative | Common alternative HTTPS port |
| 8000 | TCP | HTTP Development | Frequently used by development servers |
| 3000 | TCP | Application Development | Common development server port |
| 5000 | TCP | Application Development | Common development server port |

### 🖆 DNS and Network Configuration

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 53 | UDP/TCP | DNS | Domain name resolution |
| 67 | UDP | DHCP Server | DHCP server communication |
| 68 | UDP | DHCP Client | DHCP client communication |
| 546 | UDP | DHCPv6 Client | DHCPv6 client communication |
| 547 | UDP | DHCPv6 Server | DHCPv6 server and relay communication |
| 853 | TCP | DNS over TLS | Encrypted DNS transport |
| 853 | UDP | DNS over QUIC | Encrypted DNS transport using QUIC |

### 🖆 Email Services

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 25 | TCP | SMTP | Mail server-to-server transfer |
| 110 | TCP | POP3 | Email retrieval |
| 143 | TCP | IMAP | Email access and synchronization |
| 465 | TCP | SMTP over TLS | Message submission using implicit TLS |
| 587 | TCP | SMTP Submission | Authenticated email submission, commonly with STARTTLS |
| 993 | TCP | IMAPS | IMAP over TLS |
| 995 | TCP | POP3S | POP3 over TLS |

### 🖆 File Transfer Services

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 20 | TCP | FTP Data | Active-mode FTP data connection |
| 21 | TCP | FTP Control | FTP control connection |
| 22 | TCP | SFTP | File transfer over SSH |
| 69 | UDP | TFTP | Simple file transfer |
| 989 | TCP | FTPS Data | Common implicit FTPS data port |
| 990 | TCP | FTPS Control | Common implicit FTPS control port |

### 🖆 Network Monitoring and Logging

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 123 | UDP | NTP | Network time synchronization |
| 161 | UDP | SNMP | Device monitoring |
| 162 | UDP | SNMP Trap | Monitoring notifications |
| 514 | UDP | Syslog | Traditional log transport |
| 514 | TCP | Syslog | Syslog over TCP where configured |
| 6514 | TCP | Syslog over TLS | Encrypted centralized logging |
| 2055 | UDP | NetFlow | Common flow-export destination port |
| 6343 | UDP | sFlow | Common sFlow collector port |

### 🖆 Authentication and Directory Services

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 88 | TCP/UDP | Kerberos | Network authentication |
| 1812 | UDP | RADIUS Authentication | Centralized authentication |
| 1813 | UDP | RADIUS Accounting | Session accounting |
| 1645 | UDP | Legacy RADIUS Authentication | Historical authentication port |
| 1646 | UDP | Legacy RADIUS Accounting | Historical accounting port |
| 389 | TCP/UDP | LDAP | Directory access |
| 636 | TCP | LDAPS | LDAP over TLS |
| 3268 | TCP | Global Catalog LDAP | Active Directory global catalog |
| 3269 | TCP | Global Catalog LDAPS | Secure global catalog access |

### 🖆 VPN and Tunneling Services

| Port / Protocol | Transport | Service | Purpose |
| --- | --- | --- | --- |
| 500 | UDP | IPsec IKE | Security association negotiation |
| 4500 | UDP | IPsec NAT-T | IPsec through NAT |
| 1701 | UDP | L2TP | Layer 2 Tunneling Protocol |
| 1723 | TCP | PPTP Control | PPTP control connection |
| 1194 | UDP/TCP | OpenVPN | Common OpenVPN port |
| 51820 | UDP | WireGuard | Default WireGuard port |
| 47 | IP Protocol | GRE | Generic Routing Encapsulation |
| 50 | IP Protocol | ESP | IPsec Encapsulating Security Payload |
| 51 | IP Protocol | AH | IPsec Authentication Header |

### 🖆 Database Services

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 1433 | TCP | Microsoft SQL Server | Database communication |
| 1521 | TCP | Oracle Database Listener | Oracle database connections |
| 3306 | TCP | MySQL | Database communication |
| 5432 | TCP | PostgreSQL | Database communication |
| 6379 | TCP | Redis | In-memory data store |
| 27017 | TCP | MongoDB | Database communication |
| 9200 | TCP | Elasticsearch HTTP API | Search and indexing API |
| 9300 | TCP | Elasticsearch Transport | Cluster communication |

### 🖆 File Sharing and Remote Services

| Port | Protocol | Service | Purpose |
| ---: | --- | --- | --- |
| 111 | TCP/UDP | RPCbind | RPC service discovery |
| 137 | UDP | NetBIOS Name Service | NetBIOS name resolution |
| 138 | UDP | NetBIOS Datagram Service | NetBIOS datagram communication |
| 139 | TCP | NetBIOS Session Service | NetBIOS session communication |
| 445 | TCP | SMB | Windows file and printer sharing |
| 2049 | TCP/UDP | NFS | Network File System |

### 🖆 ISP and Network Infrastructure

| Port / Protocol | Transport | Service | Purpose |
| --- | --- | --- | --- |
| 179 | TCP | BGP | Inter-domain routing |
| 520 | UDP | RIP | IPv4 Routing Information Protocol |
| 521 | UDP | RIPng | IPv6 Routing Information Protocol |
| 646 | TCP | LDP | Label Distribution Protocol |
| 4789 | UDP | VXLAN | Layer 2 overlay tunneling |
| 5246 | UDP | CAPWAP Control | Wireless AP control |
| 5247 | UDP | CAPWAP Data | Wireless AP data |
| 2601 | TCP | Zebra VTY | Common routing-daemon terminal port |
| 2604 | TCP | Quagga / FRR VTY | Common routing-daemon terminal port |

**Important:** Some ports listed above are common conventions rather than mandatory defaults. Actual service configuration and vendor implementation may differ.

# 🗁 Important IP Protocol Numbers That Are Not Ports

Not every networking protocol uses TCP or UDP ports.

Some protocols are identified directly by the IP protocol field.

| Protocol Number | Protocol | Purpose |
| ---: | --- | --- |
| 1 | ICMP | IPv4 diagnostics and error reporting |
| 2 | IGMP | IPv4 multicast group management |
| 6 | TCP | Reliable transport |
| 17 | UDP | Datagram transport |
| 41 | IPv6 Encapsulation | IPv6 packets inside IPv4 |
| 47 | GRE | Generic Routing Encapsulation |
| 50 | ESP | IPsec encrypted payload |
| 51 | AH | IPsec authentication header |
| 58 | ICMPv6 | IPv6 diagnostics and control |
| 89 | OSPF | Interior routing |
| 103 | PIM | Protocol Independent Multicast |
| 112 | VRRP | Virtual Router Redundancy Protocol |
| 132 | SCTP | Stream Control Transmission Protocol |

For example, OSPF uses IP protocol number 89. It does not use TCP port 89 or UDP port 89.

# 🗁 Common Port Usage in MikroTik RouterOS

MikroTik devices provide several services that administrators should understand and secure.

| Service | Common Default Port | Transport |
| --- | ---: | --- |
| WinBox | 8291 | TCP |
| SSH | 22 | TCP |
| Telnet | 23 | TCP |
| WebFig HTTP | 80 | TCP |
| WebFig HTTPS | 443 | TCP |
| API | 8728 | TCP |
| API-SSL | 8729 | TCP |
| MAC Telnet | 20561 | Ethernet-level service |
| MAC WinBox | 20561 | Ethernet-level service |
| Bandwidth Test Server | 2000 | TCP/UDP |

**Inspect enabled RouterOS services:**

```routeros
/ip service print
```

**Disable unused services:**

```routeros
/ip service disable telnet
/ip service disable ftp
/ip service disable www
```

**Change a service port:**

```routeros
/ip service set winbox port=8292
```

**Restrict management access:**

```routeros
/ip service set winbox address=192.168.10.0/24
/ip service set ssh address=192.168.10.0/24
```

Restrict management services to trusted management networks and apply appropriate firewall policies.

# 🗁 Linux Commands for Port Inspection

Linux provides several commands for checking listening ports, active connections, and service availability.

**Show listening TCP and UDP sockets:**

```bash
ss -tuln
```

**Show listening sockets with process information:**

```bash
sudo ss -tulnp
```

**Show TCP connections:**

```bash
ss -tan
```

**Show UDP sockets:**

```bash
ss -uan
```

**Check whether a service is listening on TCP port 22:**

```bash
sudo ss -lntp | grep ':22'
```

**Check whether a service is listening on UDP port 53:**

```bash
sudo ss -lnup | grep ':53'
```

**Identify the process using a port:**

```bash
sudo lsof -i :8080
```

**Alternative:**

```bash
sudo fuser -v 8080/tcp
```

# 🗁 Test Port Connectivity

A service may be running locally but unreachable remotely because of firewall rules, routing problems, or service binding configuration.

**Test a TCP port using Netcat:**

```bash
nc -vz 192.168.10.100 22
```

**Test HTTPS connectivity:**

```bash
nc -vz example.com 443
```

**Test an HTTP service:**

```bash
curl -I http://192.168.10.100:80
```

**Test HTTPS:**

```bash
curl -I https://example.com
```

**Inspect a TCP connection with Nmap:**

```bash
nmap -sT -p 22,80,443 192.168.10.100
```

**Scan common UDP services:**

```bash
sudo nmap -sU -p 53,123,161 192.168.10.100
```

UDP scanning can be inconclusive because many UDP services do not respond to arbitrary probes. A filtered or open|filtered result does not necessarily establish that a service is unavailable.

Only scan systems you own or are authorized to assess.

# 🗁 Nmap Port Scanning

Nmap is a network discovery and security auditing tool used to identify hosts, open ports, and services.

**Scan common ports:**

```bash
nmap 192.168.10.100
```

**Scan selected TCP ports:**

```bash
nmap -p 22,80,443 192.168.10.100
```

**Scan all TCP ports:**

```bash
nmap -p- 192.168.10.100
```

**Detect service versions:**

```bash
nmap -sV 192.168.10.100
```

**Detect operating system where possible:**

```bash
sudo nmap -O 192.168.10.100
```

**Scan a local subnet:**

```bash
nmap -sn 192.168.10.0/24
```

**Common Nmap options:**

| Option | Meaning |
| --- | --- |
| `-p` | Specify ports |
| `-p-` | Scan all TCP ports |
| `-sT` | TCP connect scan |
| `-sS` | TCP SYN scan |
| `-sU` | UDP scan |
| `-sV` | Service version detection |
| `-O` | Operating system detection |
| `-A` | Enable several advanced detection features |
| `-sn` | Host discovery without port scanning |
| `-Pn` | Skip host discovery and treat targets as online |

# 🗁 Understanding Port States

Port scanners commonly report the following states:

| State | Meaning |
| --- | --- |
| Open | A service is accepting connections or responding to probes |
| Closed | The target is reachable, but no service is listening on the tested port |
| Filtered | A firewall or network condition prevents determining whether the port is open |
| Open\|Filtered | The scanner cannot distinguish an open UDP port from a filtered one |
| Closed\|Filtered | The scanner cannot distinguish a closed port from a filtered one |

**Example:**

```text
PORT     STATE    SERVICE
22/tcp   open     ssh
80/tcp   open     http
443/tcp  open     https
3306/tcp closed   mysql
```

This indicates that SSH, HTTP, and HTTPS appear accessible, while TCP port 3306 is reachable but has no listening service or otherwise responds as closed.

# 🗁 Firewall Rules and Port Management

A firewall controls whether traffic is permitted to reach a service.

A typical firewall decision may consider:

* Source IP address.
* Destination IP address.
* Transport protocol.
* Source port.
* Destination port.
* Connection state.
* Incoming and outgoing interfaces.
* Security zones and policies.

**Example UFW rule for SSH:**

```bash
sudo ufw allow 22/tcp
```

**Allow HTTPS:**

```bash
sudo ufw allow 443/tcp
```

**Allow DNS:**

```bash
sudo ufw allow 53/udp
sudo ufw allow 53/tcp
```

**Show firewall rules:**

```bash
sudo ufw status verbose
```

**MikroTik RouterOS v7 example:**

```routeros
/ip firewall filter add \
    chain=input \
    protocol=tcp \
    dst-port=22 \
    src-address=192.168.10.0/24 \
    action=accept \
    comment="Allow SSH from management LAN"
```

This example permits SSH connections addressed to the router from the specified management subnet. It should be placed appropriately within the input-chain firewall policy, before any rule that would drop the same traffic.

# 🗁 Common Port Security Practices

* Disable unnecessary services and listening ports.
* Avoid exposing SSH, WinBox, databases, and administrative interfaces directly to the public internet.
* Restrict management access using trusted source IP addresses or dedicated management networks.
* Use VPN access for remote administration where appropriate.
* Prefer encrypted protocols such as SSH, HTTPS, SFTP, and TLS-protected services.
* Replace default credentials with strong authentication.
* Keep network services and operating systems updated.
* Monitor logs for repeated authentication failures and suspicious connection attempts.
* Use firewall rules to permit only required traffic.
* Document custom service ports and firewall changes.
* Review exposed ports periodically.
* Use SNMPv3 rather than exposing SNMPv1 or SNMPv2c community strings to untrusted networks.

# 🗁 Practical Troubleshooting Scenarios

| Scenario | Possible Cause | Investigation |
| --- | --- | --- |
| SSH connection refused | SSH service stopped or wrong port | Check `ss -lntp` and SSH configuration |
| SSH connection times out | Firewall or routing issue | Check routes, firewall, and reachability |
| Website unavailable | HTTP/HTTPS service or DNS problem | Check DNS and ports 80/443 |
| DNS resolution fails | DNS server unavailable or blocked | Test UDP/TCP 53 |
| Email cannot be sent | SMTP port blocked or server unavailable | Check TCP 25, 465, or 587 |
| RADIUS authentication fails | Incorrect shared secret or blocked traffic | Check UDP 1812 and server logs |
| SNMP monitoring fails | Incorrect credentials or blocked requests | Check UDP 161 and SNMP configuration |
| VPN tunnel fails | Required protocol or port blocked | Check IKE, NAT-T, GRE, or VPN-specific transport |
| Database connection fails | Database service or firewall issue | Check the configured database port |
| WinBox inaccessible | Router service disabled or restricted | Check `/ip service print` and firewall rules |

# 🗁 Practical Lab Exercises

**Lab: Identify Listening Ports**

Run the following command on an Ubuntu system:

```bash
sudo ss -tulnp
```

Tasks:

* Identify all listening TCP ports.
* Identify all listening UDP ports.
* Find the process associated with each listening port.
* Identify services exposed on all interfaces.
* Identify services bound only to localhost.

**Lab: Test a Remote TCP Port**

```bash
nc -vz 192.168.10.100 22
nc -vz 192.168.10.100 80
nc -vz 192.168.10.100 443
```

Tasks:

* Test an open port.
* Test a closed port.
* Test a port blocked by a firewall.
* Compare the connection results.
* Identify whether the failure is a refusal or timeout.

**Lab: Scan a Test Network**

```bash
nmap -sV 192.168.10.0/24
```

Tasks:

* Discover active hosts.
* Identify open ports.
* Detect available services.
* Compare scan results with local service configurations.
* Document unexpected exposed services.

**Lab: MikroTik Service Hardening**

Inspect the router:

```routeros
/ip service print
```

Tasks:

* Identify enabled management services.
* Disable unused services.
* Restrict SSH and WinBox to the management subnet.
* Verify that legitimate management access still works.
* Document the changes.

# 🗁 Learning Outcomes

After studying this document, you should be able to:

* Explain TCP and UDP port numbers.
* Understand well-known, registered, and dynamic port ranges.
* Identify common network services and their ports.
* Distinguish transport ports from IP protocol numbers.
* Inspect listening services on Linux.
* Test remote TCP port connectivity.
* Use Nmap for authorized network discovery.
* Interpret common port-scanning results.
* Configure basic port-based firewall rules.
* Secure management services on MikroTik RouterOS.

# 🗁 Official Documentation and References

* [IANA Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
* [RFC 6335 — Internet Assigned Numbers Authority Procedures](https://www.rfc-editor.org/rfc/rfc6335)
* [Nmap Reference Guide](https://nmap.org/book/man.html)
* [Linux ss Manual](https://man7.org/linux/man-pages/man8/ss.8.html)
* [Netcat Manual](https://manpages.ubuntu.com/manpages/latest/en/man1/nc.1.html)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
