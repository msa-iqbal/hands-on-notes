# Bandwidth Management

Bandwidth management is the process of controlling how network bandwidth is distributed among users, devices, applications, and services.

MikroTik RouterOS v7 provides several traffic management mechanisms, including Simple Queues, Queue Trees, PCQ, queue types, and traffic classification through Mangle.

This project demonstrates how to implement bandwidth management for a small office network using MikroTik RouterOS.

# 🗁 Project Objectives

By completing this project, you will learn how to:

* Understand bandwidth management requirements.
* Configure per-user bandwidth limits.
* Implement shared bandwidth allocation.
* Use Simple Queues.
* Configure PCQ for dynamic bandwidth distribution.
* Prioritize important network traffic.
* Apply different bandwidth policies to VLANs.
* Monitor bandwidth consumption.
* Troubleshoot bandwidth-related problems.

# 🗁 Understanding Bandwidth Management

Bandwidth management ensures that network resources are distributed according to defined policies.

Without proper bandwidth management, a single user or application may consume a disproportionate amount of available bandwidth.

For example, consider an office with a 100 Mbps internet connection.

If one employee downloads a large file, other users may experience slow browsing, video calls, and application performance.

Bandwidth management helps prevent this by controlling traffic rates and allocating available capacity.

# 🗁 Understanding Bandwidth Terminology

| Term | Description |
| --- | --- |
| Bandwidth | Maximum capacity of a network connection |
| Throughput | Actual amount of data transferred per unit of time |
| Upload | Traffic sent from the local network |
| Download | Traffic received by the local network |
| Latency | Time required for packets to travel between endpoints |
| Jitter | Variation in packet delay |
| Packet loss | Packets that fail to reach their destination |
| Queue | Buffer or scheduling mechanism used to manage packets |
| Shaping | Controlling traffic transmission rates |
| Policing | Enforcing a traffic rate, often by dropping or remarking excess packets |
| Priority | Relative importance assigned to queued traffic |

# 🗁 Network Topology

The project uses a MikroTik router connected to an internet service provider and an internal office network.

```text
                   Internet
                      |
                     ISP
                      |
                 +----------+
                 | MikroTik |
                 | Router   |
                 +----------+
                      |
                     LAN
                      |
               Managed Switch
                      |
          +-----------+-----------+
          |           |           |
        Admin       Staff       Guest
       VLAN 10     VLAN 20     VLAN 30

```

# 🗁 Bandwidth Allocation Plan

Assume the ISP provides a 100 Mbps download and 50 Mbps upload connection.

The organization wants to allocate bandwidth as follows:

| Network | Download limit | Upload limit |
| --- | ---: | ---: |
| ADMIN | 30 Mbps | 15 Mbps |
| STAFF | 40 Mbps | 20 Mbps |
| GUEST | 20 Mbps | 10 Mbps |
| SERVERS | 10 Mbps | 5 Mbps |
| Total | 100 Mbps | 50 Mbps |

These are maximum allocations, not guaranteed reservations. The actual behavior depends on queue configuration and available parent bandwidth.

# 🗁 Preparing the Router

Before configuring queues:

* Confirm that internet connectivity is working.
* Identify the WAN interface.
* Identify the internal LAN or VLAN interfaces.
* Determine the actual ISP upload and download rates.
* Review existing queue configurations.
* Check whether FastTrack is enabled.
* Back up the router configuration.

For this project, assume:

| Interface | Purpose |
| --- | --- |
| `ether1` | WAN |
| `bridge1` | LAN bridge |
| `vlan10-admin` | ADMIN |
| `vlan20-staff` | STAFF |
| `vlan30-guest` | GUEST |
| `vlan40-servers` | SERVERS |

Replace these names with the actual interfaces in your configuration.

# 🗁 Understanding Queue Direction

Queue direction is important when configuring bandwidth limits.

From the perspective of a LAN client:

* Download traffic travels from the internet toward the client.
* Upload traffic travels from the client toward the internet.

In a Simple Queue, `max-limit` is expressed as upload/download from the perspective of the queue's target.

For a queue targeting a LAN subnet:

```text
max-limit=UPLOAD/DOWNLOAD
```

For example:

```text
max-limit=5M/10M
```

This limits the target network to 5 Mbps upload and 10 Mbps download.

Always verify the target and packet direction when designing queues.

# 🗁 Configuring Bandwidth Management with Simple Queues

Simple Queues are suitable for straightforward bandwidth management.

Create an ADMIN queue:

```routeros
/queue/simple/add name=ADMIN target=192.168.10.0/24 max-limit=15M/30M
```

Create a STAFF queue:

```routeros id="w1d5bx"
/queue/simple/add name=STAFF target=192.168.20.0/24 max-limit=20M/40M
```

Create a GUEST queue:

```routeros id="4bx6gp"
/queue/simple/add name=GUEST target=192.168.30.0/24 max-limit=10M/20M
```

Create a SERVERS queue:

```routeros id="p0p58x"
/queue/simple/add name=SERVERS target=192.168.40.0/24 max-limit=5M/10M
```

These queues enforce the configured maximum upload and download rates for each target network.

# 🗁 Understanding Simple Queue Priority

Simple Queues support priority values from 1 to 8.

* Priority 1 is the highest.
* Priority 8 is the lowest.

Priority matters when queues compete for available bandwidth under a shared parent or constrained queue hierarchy.

Example:

```routeros id="r0n1g8"
/queue/simple/set [find name=ADMIN] priority=1/1
/queue/simple/set [find name=STAFF] priority=3/3
/queue/simple/set [find name=GUEST] priority=8/8
/queue/simple/set [find name=SERVERS] priority=2/2
```

Priority does not reserve bandwidth by itself. It influences how available bandwidth is distributed when queue limits and parent constraints create contention.

# 🗁 Understanding Shared Bandwidth

A shared bandwidth policy allows multiple users to use a common bandwidth pool.

For example, an office may have 40 Mbps available for all staff members.

Instead of assigning each user a fixed 5 Mbps limit, the router can dynamically distribute the available bandwidth among active users.

If only one user is active, that user can consume most or all of the available pool.

When multiple users are active, they share the available capacity according to the queue configuration.

PCQ is commonly used to implement this behavior.

# 🗁 Understanding PCQ

Per Connection Queue (PCQ) is a queue type that classifies traffic into subqueues based on selected classifiers.

For bandwidth distribution among clients, common classifiers are:

* `src-address` for upload traffic.
* `dst-address` for download traffic.

For example, a PCQ queue can divide a shared 40 Mbps download pool among active client IP addresses.

If four clients are active, each may receive approximately 10 Mbps when the pool is fully utilized and the traffic distribution is comparable.

The actual rate depends on active traffic, queue limits, packet classification, and other network conditions.

# 🗁 Creating PCQ Queue Types

Create a PCQ type for download traffic:

```routeros id="2m1v6f"
/queue/type/add name=pcq-download kind=pcq pcq-classifier=dst-address pcq-rate=0
```

Create a PCQ type for upload traffic:

```routeros id="7n3r5c"
/queue/type/add name=pcq-upload kind=pcq pcq-classifier=src-address pcq-rate=0
```

The `pcq-rate=0` setting allows active subqueues to share the available parent bandwidth rather than imposing a fixed rate on each subqueue.

# 🗁 Configuring Shared Bandwidth with PCQ

Create a shared STAFF queue:

```routeros id="q1z7v8"
/queue/simple/add name=STAFF-PCQ target=192.168.20.0/24 max-limit=20M/40M queue=pcq-upload/pcq-download
```

The queue provides a shared maximum of:

* 20 Mbps upload.
* 40 Mbps download.

PCQ distributes the available bandwidth among the classified client addresses.

For example:

| Active clients | Approximate download per client |
| ---: | ---: |
| 1 | Up to 40 Mbps |
| 2 | Up to 20 Mbps |
| 4 | Up to 10 Mbps |
| 8 | Up to 5 Mbps |

These values are illustrative and assume all clients are continuously consuming traffic with comparable demand.

# 🗁 Configuring Per-User Bandwidth Limits

Sometimes each user needs a maximum bandwidth limit regardless of the total available bandwidth.

For example:

| User | Upload | Download |
| --- | ---: | ---: |
| User A | 2 Mbps | 5 Mbps |
| User B | 2 Mbps | 5 Mbps |
| User C | 3 Mbps | 8 Mbps |

Create individual Simple Queues:

```routeros id="w8k3mj"
/queue/simple/add name=USER-A target=192.168.20.101/32 max-limit=2M/5M
/queue/simple/add name=USER-B target=192.168.20.102/32 max-limit=2M/5M
/queue/simple/add name=USER-C target=192.168.20.103/32 max-limit=3M/8M
```

The `/32` target identifies a single IPv4 address.

For consistent user policies, use static DHCP leases or another mechanism that ensures each user retains the intended address.

# 🗁 Understanding Queue Trees

Queue Trees provide hierarchical traffic management.

They are useful when you need:

* Parent and child queue structures.
* Traffic classification through packet marks.
* Separate upload and download policies.
* More advanced bandwidth allocation.
* Priority management across traffic classes.

A typical queue hierarchy may look like this:

```text
INTERNET
   |
   +---- DOWNLOAD
   |       |
   |       +---- ADMIN
   |       +---- STAFF
   |       +---- GUEST
   |
   +---- UPLOAD
           |
           +---- ADMIN
           +---- STAFF
           +---- GUEST
```

Queue Trees generally require appropriate packet marking with Mangle when traffic must be classified into specific child queues.

The parent queue must correspond to the intended traffic direction and interface or queue hierarchy.

# 🗁 Understanding FastTrack and Queues

FastTrack can bypass parts of the normal packet-processing path, including queue processing for traffic that is eligible for FastTrack.

This can cause bandwidth policies to appear ineffective.

When using Simple Queues or Queue Trees, review FastTrack rules carefully.

For a queue-managed network, you may need to:

* Disable FastTrack.
* Exclude queue-managed connections from FastTrack.
* Design a deliberate Mangle and queue strategy.

For example, in a lab environment, you can temporarily disable an existing FastTrack rule:

```routeros id="j9z7vf"
/ip/firewall/filter/disable [find action=fasttrack-connection]
```

This command disables all matching FastTrack rules. In a production router, identify the specific rule before changing it.

# 🗁 Configuring Bandwidth Management for VLANs

The Simple Queue examples can be applied to individual VLAN subnets.

For example:

```routeros id="f5n8vk"
/queue/simple/add name=ADMIN-VLAN target=192.168.10.0/24 max-limit=15M/30M
/queue/simple/add name=STAFF-VLAN target=192.168.20.0/24 max-limit=20M/40M
/queue/simple/add name=GUEST-VLAN target=192.168.30.0/24 max-limit=10M/20M
/queue/simple/add name=SERVER-VLAN target=192.168.40.0/24 max-limit=5M/10M
```

Ensure that these queues do not duplicate or conflict with existing queues targeting the same traffic.

# 🗁 Monitoring Queue Traffic

Inspect Simple Queues:

```routeros id="f6t3ks"
/queue/simple/print
```

Display detailed queue information:

```routeros id="8d1h5w"
/queue/simple/print stats
```

Monitor a specific queue:

```routeros id="m3j7pn"
/queue/simple/monitor [find name=STAFF-VLAN]
```

Inspect queue types:

```routeros id="0t6g3b"
/queue/type/print detail
```

These commands help identify whether traffic is being shaped and whether the configured limits are effective.

# 🗁 Testing Bandwidth Limits

To test bandwidth management:

* Connect a client to the ADMIN VLAN.
* Run an internet speed test.
* Compare the result with the configured limit.
* Repeat the test from STAFF and GUEST devices.
* Generate traffic from multiple devices simultaneously.
* Observe queue statistics.

For example, if the STAFF queue is configured for 20 Mbps upload and 40 Mbps download, its aggregate traffic should remain within those configured maximum rates under normal queue operation.

Internet speed-test results can differ due to server capacity, protocol overhead, latency, and ISP congestion.

# 🗁 Troubleshooting Bandwidth Management

| Problem | Possible cause | Investigation |
| --- | --- | --- |
| Queue does not limit traffic | FastTrack bypasses queue processing | Inspect FastTrack rules |
| Download limit works but upload does not | Incorrect upload/download direction | Review target and queue configuration |
| PCQ does not distribute bandwidth | Incorrect classifier | Inspect PCQ queue type |
| Users receive less bandwidth than expected | Shared parent is congested | Inspect parent queue and traffic |
| One user consumes too much bandwidth | Incorrect target or classification | Inspect queue targets and PCQ classifier |
| Queue counters remain zero | Traffic does not match the queue | Check target subnet and routing |
| Multiple queues affect the same traffic | Overlapping queue targets | Review queue hierarchy and order |
| Speed tests exceed expected limits | Queue bypass or measurement differences | Inspect FastTrack and queue statistics |

Useful diagnostic commands:

```routeros
/queue/simple/print detail
/queue/simple/print stats
/queue/tree/print detail
/queue/tree/print stats
/queue/type/print detail
/ip/firewall/filter/print
/ip/firewall/mangle/print
```

# 🗁 Final Configuration Checklist

* [ ] ISP bandwidth confirmed.
* [ ] WAN and LAN interfaces identified.
* [ ] VLAN networks documented.
* [ ] Bandwidth allocation plan created.
* [ ] Simple Queues configured.
* [ ] Upload and download directions verified.
* [ ] Queue priorities configured where required.
* [ ] PCQ types configured where required.
* [ ] Shared bandwidth policies tested.
* [ ] Per-user limits tested.
* [ ] FastTrack compatibility reviewed.
* [ ] Queue statistics monitored.
* [ ] Bandwidth tests completed.
* [ ] Existing queue conflicts checked.
* [ ] Router configuration backup created.

# 🗁 Expected Result

After completing this project, the MikroTik router should be able to enforce bandwidth policies across office networks and individual users.

The network administrator can control maximum upload and download rates, distribute shared bandwidth, prioritize important traffic, and monitor queue activity.

These techniques provide a foundation for more advanced ISP bandwidth management, subscriber rate limiting, QoS, and traffic engineering.
