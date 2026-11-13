# Proxy and Load Balancing

Proxy servers and load balancing are important networking technologies used to manage traffic, improve performance, control access, and increase service availability.

A **proxy server** acts as an intermediary between clients and destination servers. It can forward requests, filter traffic, cache content, and protect internal network resources.

**Load balancing** distributes network traffic across multiple connections, servers, or internet gateways to improve resource utilization, performance, and availability.

These technologies are widely used in enterprise networks, data centers, cloud infrastructure, and Internet Service Provider (ISP) environments.

# 🗁 Learning Objectives

After completing this section, you should understand:

* What proxy servers are and how they work.
* The differences between forward and reverse proxies.
* How transparent proxies operate.
* Load balancing concepts and algorithms.
* Per-connection and per-packet load balancing.
* Per Connection Classifier (PCC) load balancing.
* WAN load balancing and failover.
* Policy-based load balancing.
* MikroTik RouterOS v7 proxy and multi-WAN configurations.
* How to troubleshoot proxy and load balancing problems.

# 🗁 Technologies and Tools

| Technology / Tool    | Purpose                                                    |
| -------------------- | ---------------------------------------------------------- |
| Squid Proxy          | Forward proxy and web caching                              |
| Nginx                | Reverse proxy and HTTP load balancing                      |
| HAProxy              | High-performance proxy and load balancer                   |
| MikroTik RouterOS v7 | Proxy configuration, PCC, multi-WAN routing, and failover  |
| Linux Networking     | Routing, firewall, NAT, and traffic management             |
| DNS                  | Name resolution and DNS-based traffic distribution         |
| HTTP / HTTPS         | Application-layer communication                            |
| Connection Tracking  | Tracks network connections for stateful traffic management |

# 🗁 Practical Skills

This section focuses on practical networking scenarios, including:

* Deploying a forward proxy for client networks.
* Understanding reverse proxy architecture.
* Implementing transparent proxy configurations.
* Distributing traffic across multiple WAN connections.
* Configuring PCC load balancing on MikroTik RouterOS v7.
* Implementing automatic WAN failover.
* Routing selected traffic through specific internet gateways.
* Monitoring connections and troubleshooting traffic distribution.

# 🗁 Expected Outcome

By completing this section, you should be able to understand proxy architectures, design traffic distribution strategies, configure multi-WAN environments, implement load balancing and failover, and troubleshoot common traffic-management problems.
