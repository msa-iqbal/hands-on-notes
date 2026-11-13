# Network Security

Network security is the practice of protecting network infrastructure, devices, services, applications, and transmitted data from unauthorized access, misuse, modification, disruption, and other security threats.

It combines technical controls, security policies, monitoring, and operational procedures to maintain the confidentiality, integrity, and availability of network resources.

Network security is essential for home networks, enterprise environments, data centers, cloud infrastructure, and Internet Service Providers (ISPs).

# 🗁 Objectives of Network Security

The primary objectives of network security are based on the CIA triad:

| Objective | Description |
| --- | --- |
| Confidentiality | Ensures that information is accessible only to authorized users and systems. |
| Integrity | Protects information from unauthorized modification or destruction. |
| Availability | Ensures that network resources and services remain accessible when required. |

Additional security objectives include:

* **Authentication:** Verifying the identity of users, devices, or services.
* **Authorization:** Determining which resources an authenticated entity can access.
* **Accounting:** Recording user and system activities for auditing and analysis.
* **Non-repudiation:** Providing evidence that an action or communication occurred.

# 🗁 Network Security Components

Network security uses multiple technologies and controls to protect network infrastructure.

| Component | Purpose |
| --- | --- |
| Firewall | Controls incoming and outgoing network traffic based on security rules. |
| IDS | Detects suspicious activities and potential security threats. |
| IPS | Detects and blocks malicious network traffic. |
| VPN | Provides encrypted communication over untrusted networks. |
| AAA | Manages authentication, authorization, and accounting. |
| RADIUS | Provides centralized authentication and access management. |
| 802.1X | Controls network access through port-based authentication. |
| NAC | Evaluates devices and controls their access to network resources. |
| PKI | Manages digital certificates and public-key trust relationships. |
| DDoS Protection | Detects and mitigates distributed denial-of-service attacks. |
| Security Monitoring | Collects and analyzes security events and network activities. |

# 🗁 Network Security Architecture

A secure network uses multiple layers of protection rather than relying on a single security device or technology.

Common security layers include:

* **Perimeter Security:** Protects the boundary between trusted and untrusted networks.
* **Network Segmentation:** Separates network resources into security zones.
* **Access Control:** Restricts access based on identity, roles, and permissions.
* **Endpoint Security:** Protects servers, workstations, routers, and other devices.
* **Data Protection:** Protects sensitive information through encryption and access restrictions.
* **Monitoring and Detection:** Identifies suspicious activities and security incidents.
* **Incident Response:** Provides procedures for investigating and handling security incidents.

# 🗁 Network Security in Different Environments

Network security requirements depend on the environment and its operational responsibilities.

| Environment | Security Focus |
| --- | --- |
| Home Network | Wi-Fi security, router protection, strong passwords, and firmware updates. |
| Enterprise Network | Access control, segmentation, endpoint protection, and centralized monitoring. |
| Data Center | Infrastructure isolation, workload security, traffic filtering, and availability. |
| Cloud Network | Identity management, security groups, network policies, and encryption. |
| ISP Network | Subscriber isolation, infrastructure protection, DDoS mitigation, and secure management. |
| Wireless Network | Authentication, encryption, access-point protection, and rogue AP detection. |

# 🗁 Network Security Best Practices

* Use strong authentication and multi-factor authentication (MFA).
* Disable unnecessary services and unused network ports.
* Replace insecure protocols with secure alternatives.
* Apply security updates to network devices and operating systems.
* Use firewalls and access control lists (ACLs).
* Separate users, servers, management systems, and guest networks.
* Encrypt sensitive network communications.
* Restrict administrative access to trusted management networks.
* Monitor network traffic, authentication events, and system logs.
* Maintain configuration backups and documented recovery procedures.
* Apply the principle of least privilege.
* Regularly review security policies and access permissions.

# 🗁 Learning Roadmap

This section covers the following network security topics:

| File | Topic | Description |
| --- | --- | --- |
| `01-security-fundamentals.md` | Security Fundamentals | CIA triad, security principles, defense in depth, and security models. |
| `02-network-threats.md` | Network Threats | Common attacks, malware, spoofing, sniffing, and network vulnerabilities. |
| `03-aaa-authentication-authorization-accounting.md` | AAA | Authentication, authorization, accounting, and centralized access management. |
| `04-radius.md` | RADIUS | Centralized authentication and RADIUS client-server architecture. |
| `05-802-1x.md` | IEEE 802.1X | Port-based network access control and EAP authentication. |
| `06-secure-management-access.md` | Secure Management Access | SSH, HTTPS, management VLANs, and administrative access protection. |
| `07-ddos-fundamentals.md` | DDoS Fundamentals | Distributed denial-of-service attacks, detection, and mitigation. |
| `08-network-access-control.md` | Network Access Control | NAC architecture, device validation, and access policies. |
| `09-certificates-and-pki.md` | Certificates and PKI | Digital certificates, public-key infrastructure, and TLS trust. |
| `10-security-monitoring.md` | Security Monitoring | Logs, monitoring systems, alerts, and security event analysis. |
| `11-security-labs.md` | Security Labs | Practical network security configurations and exercises. |

# 🗁 Practical Learning Environments

The following tools and platforms can be used to practice network security concepts:

| Tool | Purpose |
| --- | --- |
| MikroTik RouterOS | Firewall configuration, access control, VPNs, and traffic filtering. |
| Ubuntu Server | Linux security, SSH hardening, firewall configuration, and logging. |
| Wireshark | Network packet inspection and traffic analysis. |
| Nmap | Network discovery and authorized security assessment. |
| GNS3 | Virtual network security laboratories. |
| EVE-NG | Emulated network infrastructure and security scenarios. |
| VirtualBox | Running isolated virtual machines for practical exercises. |

# 🗁 Expected Learning Outcomes

After completing this section, you should be able to:

* Understand fundamental network security principles.
* Identify common network threats and vulnerabilities.
* Explain authentication, authorization, and accounting.
* Configure centralized authentication using RADIUS.
* Understand IEEE 802.1X network access control.
* Secure network device management interfaces.
* Understand DDoS attacks and mitigation strategies.
* Explain Network Access Control architecture.
* Understand digital certificates and PKI.
* Monitor network activities and investigate security events.
* Apply practical security configurations in Linux and MikroTik RouterOS environments.
