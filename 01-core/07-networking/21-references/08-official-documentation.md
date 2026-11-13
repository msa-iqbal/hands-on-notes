# Official Networking Documentation

Official networking documentation is the primary reference for understanding network technologies, configuring devices, implementing protocols, and troubleshooting infrastructure.

Networking knowledge evolves continuously. Protocol specifications are updated, software versions introduce new features, and vendors change configuration syntax. Using authoritative documentation helps ensure that technical information is accurate, current, and applicable to the environment being managed.

This document provides a curated collection of official networking documentation, standards organizations, vendor resources, Linux tools, security references, and learning materials.

# 🗁 Purpose of Official Documentation

Official documentation helps network engineers:

* Understand how networking protocols operate.
* Learn standards-based implementations.
* Configure routers, switches, firewalls, and servers.
* Verify command syntax and configuration parameters.
* Understand software versions and compatibility.
* Troubleshoot network connectivity and performance.
* Identify security recommendations and known limitations.
* Prepare for networking certifications.
* Keep technical knowledge up to date.

# 🗁 Networking Standards Organizations

Standards organizations publish technical specifications, protocol definitions, recommendations, and interoperability standards.

**Internet Engineering Task Force (IETF)**

<https://www.ietf.org/>

The IETF develops and maintains many of the protocols used across the internet.

Important areas include:

* IPv4 and IPv6.
* TCP and UDP.
* DNS-related standards.
* HTTP and TLS.
* Routing protocols.
* IPsec and VPN technologies.
* Network management.
* Internet architecture.

**RFC Editor**

<https://www.rfc-editor.org/>

The RFC Editor provides access to Requests for Comments (RFCs), which document internet protocols, standards, best practices, and technical specifications.

Useful RFC references:

| RFC | Subject |
| --- | --- |
| RFC 791 | Internet Protocol version 4 |
| RFC 793 | Transmission Control Protocol |
| RFC 768 | User Datagram Protocol |
| RFC 826 | Address Resolution Protocol |
| RFC 1034 | Domain Names - Concepts and Facilities |
| RFC 1035 | Domain Names - Implementation and Specification |
| RFC 1918 | Address Allocation for Private Internets |
| RFC 2131 | Dynamic Host Configuration Protocol |
| RFC 2328 | OSPF Version 2 |
| RFC 3022 | Traditional IP Network Address Translator |
| RFC 4632 | Classless Inter-domain Routing |
| RFC 4271 | Border Gateway Protocol 4 |
| RFC 4291 | IPv6 Addressing Architecture |
| RFC 8200 | Internet Protocol, Version 6 |
| RFC 8446 | TLS 1.3 |
| RFC 9000 | QUIC Transport Protocol |

RFCs may have different statuses, including Proposed Standard, Internet Standard, Informational, and Historic. Always check the status and any later updates or obsoleting documents before relying on a specification.

**Internet Assigned Numbers Authority (IANA)**

<https://www.iana.org/>

IANA coordinates important internet registries.

Useful resources include:

* Port number registry.
* IP protocol number registry.
* IPv4 and IPv6 address registries.
* DNS parameters.
* Autonomous System Number registries.
* Protocol parameters.

Port number registry:

<https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml>

IP protocol number registry:

<https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml>

**IEEE Standards Association**

<https://standards.ieee.org/>

IEEE develops standards used in Ethernet, wireless networking, and other technologies.

Important standards include:

| Standard | Subject |
| --- | --- |
| IEEE 802.1Q | VLAN tagging |
| IEEE 802.1D | MAC bridges and bridging |
| IEEE 802.1X | Port-based network access control |
| IEEE 802.1AX | Link aggregation |
| IEEE 802.3 | Ethernet |
| IEEE 802.11 | Wireless LAN |

Some complete IEEE standards require purchase or institutional access.

**ICANN**

<https://www.icann.org/>

The Internet Corporation for Assigned Names and Numbers coordinates important aspects of the domain name system and internet unique identifiers.

Useful for understanding:

* Domain name policy.
* Domain registration.
* DNS root system.
* Generic top-level domains.
* Internet governance.

**Regional Internet Registries (RIRs)**

Regional Internet Registries manage IP address allocations and Autonomous System Numbers within their respective regions.

| Organization | Region | Official Website |
| --- | --- | --- |
| APNIC | Asia-Pacific | <https://www.apnic.net/> |
| ARIN | North America | <https://www.arin.net/> |
| RIPE NCC | Europe, Middle East, and parts of Central Asia | <https://www.ripe.net/> |
| AFRINIC | Africa | <https://afrinic.net/> |
| LACNIC | Latin America and Caribbean | <https://www.lacnic.net/> |

For Bangladesh and other Asia-Pacific networks, APNIC is particularly relevant.

APNIC training and resources:

<https://www.apnic.net/community/education/>

# 🗁 MikroTik RouterOS Documentation

**MikroTik Official Website**

<https://mikrotik.com/>

MikroTik provides router and wireless networking products, RouterOS software, hardware specifications, and official documentation.

**RouterOS Documentation**

<https://help.mikrotik.com/docs/>

This is the primary reference for MikroTik RouterOS configuration and administration.

Important topics:

| Topic | Documentation |
| --- | --- |
| RouterOS | <https://help.mikrotik.com/docs/> |
| IP Routing | <https://help.mikrotik.com/docs/spaces/ROS/pages/328084/IP+Routing> |
| IP Firewall | <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall> |
| Bridge | <https://help.mikrotik.com/docs/spaces/ROS/pages/328068/Bridging+and+Switching> |
| VLAN | <https://help.mikrotik.com/docs/spaces/ROS/pages/88014957/VLAN> |
| DHCP | <https://help.mikrotik.com/docs/spaces/ROS/pages/24805500/DHCP> |
| Wireless | <https://help.mikrotik.com/docs/> |
| WireGuard | <https://help.mikrotik.com/docs/spaces/ROS/pages/69664792/WireGuard> |
| IPsec | <https://help.mikrotik.com/docs/spaces/ROS/pages/11993097/IPsec> |
| OSPF | <https://help.mikrotik.com/docs/spaces/ROS/pages/9863229/OSPF> |
| BGP | <https://help.mikrotik.com/docs/spaces/ROS/pages/328220/BGP> |
| Queues | <https://help.mikrotik.com/docs/spaces/ROS/pages/328088/Queues> |
| Netwatch | <https://help.mikrotik.com/docs/spaces/ROS/pages/8323208/Netwatch> |

**RouterOS Manual**

<https://help.mikrotik.com/docs/>

Use the official manual to verify configuration syntax and feature availability for the installed RouterOS release.

**RouterOS Change Logs**

<https://mikrotik.com/download/changelogs>

Review release notes before upgrading RouterOS. Pay attention to:

* New features.
* Bug fixes.
* Security fixes.
* Configuration changes.
* Known limitations.
* Compatibility considerations.

**MikroTik Downloads**

<https://mikrotik.com/download>

Official downloads include RouterOS packages, WinBox, Netinstall, CHR images, and other utilities.

**MikroTik Training**

<https://mikrotik.com/training>

Official training information includes MikroTik certification programs and training resources.

Always verify the current course names, prerequisites, examination requirements, and certification policies on the official website.

# 🗁 Cisco Networking Documentation

**Cisco Official Website**

<https://www.cisco.com/>

Cisco provides networking products, technical documentation, configuration guides, security information, and learning resources.

**Cisco Technical Documentation**

<https://www.cisco.com/c/en/us/support/index.html>

Useful documentation areas:

* Routing and switching.
* VLANs and trunking.
* Spanning Tree Protocol.
* OSPF and BGP.
* Access Control Lists.
* Network security.
* Wireless networking.
* VPN technologies.
* Cisco IOS and IOS XE.

**Cisco Configuration Guides**

<https://www.cisco.com/c/en/us/support/index.html>

Configuration guides explain how to configure and verify features on specific Cisco platforms and software releases.

Always select documentation matching the actual device model and software version.

**Cisco Learning Network**

<https://learningnetwork.cisco.com/>

Provides certification preparation and networking learning resources.

# 🗁 Juniper Networking Documentation

**Juniper Official Website**

<https://www.juniper.net/>

Juniper Networks provides enterprise networking, service provider routing, switching, and security technologies.

**Juniper Technical Documentation**

<https://www.juniper.net/documentation/>

Important subjects:

* Junos OS.
* Routing and switching.
* BGP and OSPF.
* VLAN configuration.
* Firewall filters.
* Network automation.
* Security technologies.

**Juniper Learning Portal**

<https://learningportal.juniper.net/>

Provides technical training and certification-related learning resources.

# 🗁 Linux Networking Documentation

Linux is widely used in network servers, routers, firewalls, monitoring systems, and infrastructure automation.

**Linux Kernel Documentation**

<https://docs.kernel.org/>

Useful for understanding:

* Network device drivers.
* TCP/IP implementation.
* Network namespaces.
* Traffic control.
* Netfilter.
* Kernel networking internals.

**Linux Manual Pages**

<https://man7.org/linux/man-pages/>

Manual pages document Linux commands, system calls, and configuration interfaces.

Important networking commands:

| Command | Manual Page |
| --- | --- |
| `ip` | <https://man7.org/linux/man-pages/man8/ip.8.html> |
| `ss` | <https://man7.org/linux/man-pages/man8/ss.8.html> |
| `ping` | <https://man7.org/linux/man-pages/man8/ping.8.html> |
| `traceroute` | <https://man7.org/linux/man-pages/man8/traceroute.8.html> |
| `ip route` | <https://man7.org/linux/man-pages/man8/ip-route.8.html> |
| `ip neigh` | <https://man7.org/linux/man-pages/man8/ip-neighbour.8.html> |
| `tcpdump` | <https://man7.org/linux/man-pages/man8/tcpdump.8.html> |
| `iptables` | <https://man7.org/linux/man-pages/man8/iptables.8.html> |
| `nft` | <https://man7.org/linux/man-pages/man8/nft.8.html> |

**Ubuntu Server Documentation**

<https://documentation.ubuntu.com/server/>

Useful topics:

* Network configuration.
* Netplan.
* DNS.
* DHCP.
* Firewall configuration.
* OpenSSH.
* VPN services.
* Server administration.

**Netplan Documentation**

<https://netplan.readthedocs.io/>

Netplan manages network configuration on supported Ubuntu systems.

**systemd Documentation**

<https://www.freedesktop.org/software/systemd/man/>

Useful for understanding network-related system services, logging, and service management.

# 🗁 Network Protocol and Service Documentation

**DNS**

ICANN:

<https://www.icann.org/>

IETF DNS standards:

<https://www.rfc-editor.org/>

ISC BIND documentation:

<https://bind9.readthedocs.io/>

**DHCP**

IETF DHCP specification:

<https://www.rfc-editor.org/rfc/rfc2131>

ISC Kea documentation:

<https://kea.readthedocs.io/>

**HTTP and HTTPS**

HTTP specifications:

<https://httpwg.org/specs/>

MDN HTTP documentation:

<https://developer.mozilla.org/en-US/docs/Web/HTTP>

**TLS**

IETF TLS 1.3 specification:

<https://www.rfc-editor.org/rfc/rfc8446>

OpenSSL documentation:

<https://docs.openssl.org/>

**SSH**

OpenSSH documentation:

<https://www.openssh.com/manual.html>

OpenSSH manual pages:

<https://man.openbsd.org/ssh>

**NTP**

Network Time Protocol:

<https://www.ntp.org/>

Chrony documentation:

<https://chrony-project.org/documentation.html>

**SNMP**

IETF SNMP standards:

<https://www.rfc-editor.org/>

Net-SNMP documentation:

<https://www.net-snmp.org/docs/>

**Syslog**

IETF Syslog protocol:

<https://www.rfc-editor.org/rfc/rfc5424>

Rsyslog documentation:

<https://www.rsyslog.com/doc/>

**FTP and SFTP**

OpenSSH documentation for SFTP:

<https://www.openssh.com/manual.html>

IETF FTP-related specifications:

<https://www.rfc-editor.org/>

# 🗁 Routing Protocol Documentation

Routing protocols determine how routers exchange network reachability information and select forwarding paths.

**OSPF**

* OSPF Version 2: <https://www.rfc-editor.org/rfc/rfc2328>
* OSPF Version 3: <https://www.rfc-editor.org/rfc/rfc5340>

**BGP**

* BGP-4: <https://www.rfc-editor.org/rfc/rfc4271>
* BGP route selection and policy extensions: consult the relevant IETF specifications.

**RIP**

* RIP Version 2: <https://www.rfc-editor.org/rfc/rfc2453>

**IS-IS**

* IS-IS for IP: <https://www.rfc-editor.org/rfc/rfc1195>

**FRRouting**

<https://docs.frrouting.org/>

FRRouting is an open-source routing software suite supporting protocols such as BGP, OSPF, IS-IS, and RIP.

It is useful for Linux-based routing labs and network simulations.

# 🗁 Wireless Networking Documentation

**Wi-Fi Alliance**

<https://www.wi-fi.org/>

Provides information about Wi-Fi technologies, certifications, and security standards.

**IEEE 802.11**

<https://standards.ieee.org/>

The IEEE 802.11 family defines wireless LAN standards.

**Hostapd**

<https://w1.fi/hostapd/>

Hostapd is commonly used to implement wireless access point and authentication functionality on Linux.

**Linux Wireless Documentation**

<https://wireless.docs.kernel.org/>

Provides information about Linux wireless drivers, hardware support, and wireless configuration.

# 🗁 VPN and Tunneling Documentation

**WireGuard**

<https://www.wireguard.com/>

Official documentation for WireGuard, a modern VPN protocol.

**OpenVPN**

<https://openvpn.net/community-resources/>

Community documentation and configuration references for OpenVPN.

**IPsec**

<https://www.rfc-editor.org/>

The IETF publishes specifications for IPsec architecture, protocols, and related extensions.

Useful references:

* RFC 4301: Security Architecture for the Internet Protocol.
* RFC 4303: IP Encapsulating Security Payload (ESP).
* RFC 7296: Internet Key Exchange Protocol Version 2 (IKEv2).

**GRE**

<https://www.rfc-editor.org/rfc/rfc2784>

GRE defines a tunneling protocol for encapsulating network-layer protocols.

# 🗁 Network Security Documentation

**NIST**

<https://www.nist.gov/cybersecurity>

The National Institute of Standards and Technology publishes cybersecurity guidance, frameworks, and technical publications.

Important resources:

* Cybersecurity Framework.
* Network security guidance.
* Security controls.
* Risk management.
* Identity and access management.
* Security architecture.

NIST Special Publications:

<https://csrc.nist.gov/publications/sp>

**CISA**

<https://www.cisa.gov/>

The Cybersecurity and Infrastructure Security Agency provides security advisories, vulnerability information, and operational security guidance.

Useful areas:

* Network infrastructure security.
* Vulnerability management.
* Secure configuration.
* Incident response.
* Known exploited vulnerabilities.
* Critical infrastructure protection.

**OWASP**

<https://owasp.org/>

OWASP provides application security guidance that is useful when documenting web application networks, reverse proxies, APIs, and application-facing security boundaries.

**CVE Program**

<https://www.cve.org/>

Provides identifiers for publicly disclosed cybersecurity vulnerabilities.

**NVD**

<https://nvd.nist.gov/>

The National Vulnerability Database provides vulnerability information, CVSS scores, and related security references.

# 🗁 Network Monitoring and Troubleshooting Tools

**Wireshark**

<https://www.wireshark.org/docs/>

Official documentation for packet capture and network protocol analysis.

**tcpdump**

<https://www.tcpdump.org/manpages/tcpdump.1.html>

Documentation for command-line packet capture and filtering.

**Zabbix**

<https://www.zabbix.com/documentation>

Documentation for infrastructure monitoring, alerting, and network device monitoring.

**LibreNMS**

<https://docs.librenms.org/>

Documentation for network monitoring, SNMP discovery, alerting, and performance monitoring.

**Prometheus**

<https://prometheus.io/docs/>

Documentation for metrics collection, querying, and monitoring infrastructure.

**Grafana**

<https://grafana.com/docs/>

Documentation for dashboards, visualization, and observability.

**NetFlow and IPFIX**

IETF IPFIX specifications:

<https://www.rfc-editor.org/rfc/rfc7011>

Useful for understanding flow-based network traffic monitoring and export.

# 🗁 Network Automation and Infrastructure as Code

Network automation reduces repetitive configuration work and helps maintain consistent infrastructure.

**Ansible**

<https://docs.ansible.com/>

Useful for automating network device configuration, Linux administration, and infrastructure tasks.

**Python**

<https://docs.python.org/3/>

Python is widely used for network automation, API integration, configuration generation, and monitoring scripts.

**Netmiko**

<https://ktbyers.github.io/netmiko/>

A Python library for interacting with network devices through SSH.

**NAPALM**

<https://napalm.readthedocs.io/>

A Python library providing a consistent interface for managing network devices across supported platforms.

**Nornir**

<https://nornir.readthedocs.io/>

A Python automation framework designed for network automation workflows.

**Terraform**

<https://developer.hashicorp.com/terraform/docs>

Useful for infrastructure provisioning and managing supported network resources through providers.

# 🗁 Network Diagram and Documentation Tools

**diagrams.net**

<https://www.diagrams.net/>

**Mermaid**

<https://mermaid.js.org/>

**PlantUML**

<https://plantuml.com/>

**Graphviz**

<https://graphviz.org/>

These tools are useful for creating physical, logical, security, and architecture diagrams.

# 🗁 Certification and Learning Resources

**MikroTik Training**

<https://mikrotik.com/training>

**Cisco Learning Network**

<https://learningnetwork.cisco.com/>

**Juniper Learning Portal**

<https://learningportal.juniper.net/>

**CompTIA**

<https://www.comptia.org/>

**Linux Foundation Training**

<https://training.linuxfoundation.org/>

**Cloudflare Learning Center**

<https://www.cloudflare.com/learning/>

Cloudflare's learning resources explain networking concepts such as DNS, CDN, DDoS protection, TLS, reverse proxies, and internet infrastructure.

# 🗁 How to Use Official Documentation Effectively

Official documentation is most useful when you approach it systematically.

**Identify the technology**

Determine which protocol, device, operating system, or service you are working with.

For example:

```text
Technology: MikroTik RouterOS
Feature: OSPF
Version: RouterOS v7
Task: Configure dynamic routing
```

**Find the official documentation**

Start with the vendor's documentation or the relevant standards organization.

Avoid relying exclusively on outdated blog posts, forum answers, or copied configuration examples.

**Check the software version**

A command or feature available in one release may not exist in another.

For example, RouterOS v6 and RouterOS v7 use different OSPF configuration structures.

**Understand the concept before copying commands**

Read the configuration explanation, prerequisites, supported parameters, and limitations.

**Study configuration examples**

Compare examples with your network requirements and adapt them carefully.

**Verify the configuration**

Use the documented verification commands and inspect the actual device state.

**Review changes and known issues**

Consult release notes and relevant security advisories before deploying major changes.

# 🗁 Recommended Documentation Research Workflow

Use this workflow when learning or troubleshooting a networking topic.

```text
Identify the Problem or Topic
          |
          v
Find the Relevant Official Documentation
          |
          v
Check the Protocol or Vendor Specification
          |
          v
Verify Software and Hardware Compatibility
          |
          v
Study Configuration Examples
          |
          v
Build a Lab Environment
          |
          v
Test and Verify the Implementation
          |
          v
Document the Results
```

# 🗁 Documentation Reference Management

For a technical knowledge base, maintain a record of important documentation sources.

Suggested reference table:

| Field | Description |
| --- | --- |
| Topic | Networking subject |
| Organization | Vendor or standards body |
| Document Title | Name of the reference |
| Official URL | Direct documentation link |
| Version | Applicable software or document version |
| Category | Routing, switching, security, etc. |
| Last Verified | Date the link and content were checked |
| Notes | Relevant technical information |

Example:

| Topic | Organization | Official Reference |
| --- | --- | --- |
| RouterOS Firewall | MikroTik | <https://help.mikrotik.com/docs/spaces/ROS/pages/250708064/Firewall> |
| IPv6 | IETF | <https://www.rfc-editor.org/rfc/rfc8200> |
| BGP | IETF | <https://www.rfc-editor.org/rfc/rfc4271> |
| WireGuard | WireGuard | <https://www.wireguard.com/> |
| Wireshark | Wireshark | <https://www.wireshark.org/docs/> |
| Linux Networking | Linux Kernel | <https://docs.kernel.org/networking/> |

# 🗁 Best Practices for Technical Research

* Prefer official vendor documentation for device-specific configuration.
* Prefer IETF RFCs for internet protocol specifications.
* Prefer IEEE standards for Ethernet and wireless LAN standards.
* Use the documentation version matching your installed software.
* Check whether an RFC has been updated, obsoleted, or replaced.
* Read release notes before upgrading network devices.
* Verify commands in a lab before applying them to production.
* Keep important documentation links in your knowledge base.
* Record the source and version of important technical information.
* Review frequently used references periodically.
* Distinguish official specifications from vendor-specific implementation details.

# 🗁 Final Notes

Official documentation should be your primary technical reference when designing, configuring, securing, and troubleshooting networks.

Use standards documents to understand how protocols are defined, vendor documentation to understand how technologies are implemented, and practical labs to verify your understanding.

A strong network engineer does not simply memorize commands. They understand the underlying technology and know how to find, interpret, validate, and apply authoritative technical information.
