# NAT and Firewall

NAT (Network Address Translation) and firewalls are fundamental components of modern IP networks.

NAT modifies IP address information in packet headers as traffic passes through a network device. It is commonly used to connect private networks to public networks, translate addresses, and publish internal services.

A firewall controls network traffic according to security policies. It determines which connections or packets are permitted, rejected, or dropped.

Together, NAT and firewalls help organizations manage network connectivity, conserve public IPv4 addresses, control access, and protect internal systems.

# 🗁 Prerequisites

Before studying this section, you should understand:

* IPv4 addressing and subnetting.
* Private and public IP addresses.
* Default gateways.
* Routing tables and packet forwarding.
* TCP and UDP.
* Transport-layer ports.
* Network interfaces.
* Basic firewall concepts.

Recommended previous sections:

* `03-ip-addressing/`
* `04-transport-layer/`
* `08-routing/`

# 🗁 Learning Objectives

After completing this section, you should be able to:

* Explain why NAT is used in IPv4 networks.
* Differentiate static NAT, dynamic NAT, and PAT.
* Understand SNAT, DNAT, and masquerading.
* Explain how NAT affects packet headers and transport ports.
* Understand stateful and stateless firewall operation.
* Explain connection tracking and connection states.
* Configure firewall rules and filtering policies.
* Configure port forwarding for internal services.
* Understand firewall chains and rule evaluation.
* Design basic network segmentation policies.
* Configure NAT and firewall rules on Linux and MikroTik RouterOS v7.
* Troubleshoot common connectivity and firewall problems.

# 🗁 Recommended Learning Approach

Study the topics in order, starting with NAT fundamentals and progressing toward firewall configuration and practical labs.

Focus on understanding packet flow through a router, especially how routing, NAT, connection tracking, and firewall filtering interact.

For practical exercises, use Ubuntu/Linux with nftables and MikroTik RouterOS v7 as the primary platforms.
