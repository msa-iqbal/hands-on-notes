# Simple Queues

Simple Queues are one of the easiest ways to manage bandwidth in MikroTik RouterOS.

They allow administrators to control upload and download speeds for individual devices, IP addresses, or entire subnets.

Simple Queues are commonly used in home networks, small offices, and ISP environments where each customer needs a defined bandwidth limit.

# 🗁 What Are Simple Queues?

A Simple Queue is a RouterOS traffic management rule that matches traffic based on configured targets and applies bandwidth limits and queue policies.

For example, suppose a network has three customers:

| Customer | IP Address | Download Limit | Upload Limit |
| --- | --- | ---: | ---: |
| Customer A | 192.168.10.10 | 20 Mbps | 5 Mbps |
| Customer B | 192.168.10.20 | 30 Mbps | 10 Mbps |
| Customer C | 192.168.10.30 | 50 Mbps | 10 Mbps |

Simple Queues can enforce these individual bandwidth limits.

# 🗁 How Simple Queues Work

Simple Queues inspect traffic and determine whether it matches a configured queue target.

When traffic matches, RouterOS applies the queue's configured bandwidth limits and scheduling parameters.

Basic traffic flow:

```text
             Client Traffic
                   |
                   v
          +------------------+
          |  Simple Queues   |
          +------------------+
                   |
                   v
          Target Matching
                   |
                   v
          Bandwidth Control
                   |
                   v
             Network Link
```

Simple Queues can manage both upload and download traffic.

# 🗁 Simple Queue Configuration Path

Simple Queues are configured under:

```routeros
/queue simple
```

To display existing queues:

```routeros
/queue simple print
```

To display detailed queue information:

```routeros
/queue simple print detail
```

# 🗁 Creating a Simple Queue

Suppose a client has the following configuration:

| Parameter | Value |
| --- | --- |
| Client IP | 192.168.10.10 |
| Download limit | 20 Mbps |
| Upload limit | 5 Mbps |

Create the queue:

```routeros
/queue simple add name="Customer-A" target=192.168.10.10/32 max-limit=5M/20M
```

The `max-limit` parameter uses this order:

```text
max-limit=upload/download
```

Therefore:

* `5M` represents the upload limit.
* `20M` represents the download limit.

The client will be limited to 5 Mbps upload and 20 Mbps download.

# 🗁 Understanding Simple Queue Parameters

| Parameter | Description |
| --- | --- |
| `name` | Identifies the queue |
| `target` | IP address or subnet matched by the queue |
| `max-limit` | Maximum upload and download rate |
| `limit-at` | Guaranteed upload and download rate when resources are available |
| `priority` | Queue priority, from 1 to 8 |
| `burst-limit` | Maximum temporary burst rate |
| `burst-threshold` | Average traffic threshold used to determine burst eligibility |
| `burst-time` | Time interval used for burst calculations |
| `queue` | Upload and download queue types |
| `parent` | Defines a parent queue for hierarchical queue organization |

# 🗁 Managing Bandwidth for Multiple Clients

Create separate Simple Queues for multiple customers.

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 max-limit=5M/20M
add name="Customer-B" target=192.168.10.20/32 max-limit=10M/30M
add name="Customer-C" target=192.168.10.30/32 max-limit=10M/50M
```

Each customer receives an individual bandwidth ceiling.

The limits are independent queue ceilings, not reservations of physical bandwidth. If all customers transmit simultaneously, their combined demand can still exceed the available WAN capacity.

# 🗁 Limiting an Entire Subnet

Simple Queues can also manage bandwidth for an entire subnet.

For example, limit the `192.168.20.0/24` network:

```routeros
/queue simple add name="Office-Network" target=192.168.20.0/24 max-limit=20M/100M
```

This applies a shared upload limit of 20 Mbps and download limit of 100 Mbps to matching traffic.

It does not automatically allocate those limits equally to every device. Individual fairness requires an appropriate queue design, such as PCQ.

# 🗁 Understanding Max-Limit

`max-limit` defines the maximum permitted upload and download rates for a queue.

Example:

```routeros
/queue simple add name="Client-1" target=192.168.10.15/32 max-limit=5M/25M
```

The configured limits are:

| Direction | Maximum Rate |
| --- | ---: |
| Upload | 5 Mbps |
| Download | 25 Mbps |

Actual throughput may be lower because of congestion, other queue policies, link capacity, or protocol overhead.

# 🗁 Understanding Limit-At

`limit-at` defines the guaranteed data rate for a queue when its parent and available network resources can provide it.

For example:

```routeros
/queue simple add name="Client-2" target=192.168.10.20/32 limit-at=2M/10M max-limit=5M/25M
```

This queue requests:

* Guaranteed upload rate: 2 Mbps.
* Guaranteed download rate: 10 Mbps.
* Maximum upload rate: 5 Mbps.
* Maximum download rate: 25 Mbps.

The guaranteed rate is not an unconditional reservation. It depends on available capacity and the queue hierarchy.

# 🗁 Understanding Queue Priority

Simple Queue priority determines preference when queues compete for bandwidth under a shared parent or other constrained queue resource.

RouterOS uses priority values from 1 to 8:

| Priority | Meaning |
| --- | --- |
| 1 | Highest priority |
| 2 | Very high priority |
| 3 | High priority |
| 4 | Above normal |
| 5 | Normal |
| 6 | Below normal |
| 7 | Low priority |
| 8 | Lowest priority |

Example:

```routeros
/queue simple add name="Important-Client" target=192.168.10.50/32 max-limit=10M/50M priority=1/1
```

The first priority value applies to upload and the second to download.

Priority does not guarantee bandwidth by itself. It becomes relevant when queues compete for available resources.

# 🗁 Understanding Burst

Burst allows a queue to temporarily exceed its normal `max-limit` when traffic conditions satisfy the configured burst criteria.

It can improve the responsiveness of short transfers without allowing sustained traffic to remain at the higher rate.

Important burst parameters:

| Parameter | Description |
| --- | --- |
| `burst-limit` | Temporary maximum upload and download rate |
| `burst-threshold` | Average-rate threshold for burst eligibility |
| `burst-time` | Averaging interval used for burst calculations |

Example:

```routeros
/queue simple add name="Burst-Client" target=192.168.10.60/32 max-limit=5M/20M burst-limit=10M/40M burst-threshold=3M/15M burst-time=10s/10s
```

The queue has normal limits of 5 Mbps upload and 20 Mbps download, with temporary burst limits of 10 Mbps and 40 Mbps.

Burst behavior depends on the average traffic rate and configured thresholds. It is not a permanent increase in the customer's bandwidth allocation.

# 🗁 Queue Direction and Target Matching

A Simple Queue target identifies the IP addresses whose traffic should be managed.

For example:

```routeros
/queue simple add name="Client-A" target=192.168.10.10/32 max-limit=5M/20M
```

For a client behind the router:

* Upload is traffic sent from the client toward the network.
* Download is traffic sent from the network toward the client.

The queue's upload and download values are configured in that order, regardless of whether the router's WAN interface is named `ether1`, `wan1`, or something else.

# 🗁 Simple Queue Order

Simple Queues are evaluated according to their position in the queue list.

When a packet matches a queue, RouterOS processes it according to the matching queue and its hierarchy.

This makes queue ordering important, especially when individual client queues and broader subnet queues overlap.

For example:

```routeros
/queue simple
add name="Priority-Client" target=192.168.10.10/32 max-limit=10M/30M
add name="Office-Network" target=192.168.10.0/24 max-limit=20M/100M
```

The individual client queue is placed before the broader subnet queue.

A more specific queue should generally be placed before a broader overlapping target when the specific queue needs separate treatment.

Use the following command to inspect the queue order:

```routeros
/queue simple print
```

Move a queue when necessary:

```routeros
/queue simple move [find name="Priority-Client"] 0
```

Queue order and parent-child relationships should be designed together. A child queue belongs to its configured parent rather than acting as an unrelated top-level queue.

# 🗁 Creating a Parent and Child Queue

Simple Queues support hierarchical organization.

A parent queue can define a shared bandwidth ceiling, while child queues manage individual clients within that ceiling.

Example:

```routeros
/queue simple
add name="Office-Parent" target=192.168.10.0/24 max-limit=20M/100M
add name="Client-A" target=192.168.10.10/32 parent="Office-Parent" max-limit=5M/20M
add name="Client-B" target=192.168.10.20/32 parent="Office-Parent" max-limit=5M/20M
add name="Client-C" target=192.168.10.30/32 parent="Office-Parent" max-limit=10M/30M
```

The parent controls the shared bandwidth ceiling. Each child has its own individual maximum.

The combined child traffic remains subject to the parent's available bandwidth.

# 🗁 FastTrack and Simple Queues

FastTrack is a RouterOS firewall optimization that allows eligible connections to bypass parts of the normal packet-processing path.

FastTracked traffic bypasses Simple Queue processing.

If a client is FastTracked, its traffic may not be controlled by the expected Simple Queue policy.

For networks that require Simple Queue bandwidth management:

* Review the FastTrack rule in `/ip firewall filter`.
* Identify which connections are FastTracked.
* Exclude traffic that must be processed by Simple Queues.
* Verify queue counters while generating test traffic.

Do not assume that a configured Simple Queue is enforcing limits until its counters and actual traffic rates have been checked.

# 🗁 Monitoring Simple Queues

RouterOS provides queue statistics for monitoring traffic and verifying bandwidth policies.

Display configured queues:

```routeros
/queue simple print
```

Display detailed queue configuration:

```routeros
/queue simple print detail
```

Display queue statistics:

```routeros
/queue simple print stats
```

Monitor a specific queue:

```routeros
/queue simple monitor [find name="Customer-A"]
```

Useful statistics include:

* Current upload rate.
* Current download rate.
* Transmitted bytes.
* Transmitted packets.
* Dropped packets.
* Queue usage and activity.

Use these statistics to confirm whether the queue is matching traffic and enforcing the expected limits.

# 🗁 Modifying a Simple Queue

Change an existing customer's bandwidth:

```routeros
/queue simple set [find name="Customer-A"] max-limit=10M/40M
```

Change the queue priority:

```routeros
/queue simple set [find name="Customer-A"] priority=2/2
```

Change the guaranteed bandwidth:

```routeros
/queue simple set [find name="Customer-A"] limit-at=2M/10M
```

Disable a queue:

```routeros
/queue simple disable [find name="Customer-A"]
```

Enable it again:

```routeros
/queue simple enable [find name="Customer-A"]
```

Remove a queue:

```routeros
/queue simple remove [find name="Customer-A"]
```

# 🗁 Simple Queue Best Practices

* Use meaningful queue names.
* Use `/32` targets for individual IPv4 clients.
* Use subnet targets for shared network policies.
* Remember that `max-limit` uses upload/download order.
* Set realistic limits below actual WAN capacity when controlling the WAN bottleneck.
* Use `limit-at` when guaranteed bandwidth is required within a hierarchy.
* Configure priority according to actual service requirements.
* Understand queue ordering when targets overlap.
* Avoid creating unnecessary overlapping queues.
* Review FastTrack interaction before deployment.
* Monitor queue statistics after configuration.
* Test both upload and download behavior.

# 🗁 Summary

Simple Queues provide a straightforward way to manage bandwidth for individual clients and network subnets.

Important concepts include:

* Queue targets and traffic matching.
* Upload and download limits.
* `max-limit` and `limit-at`.
* Queue priority.
* Burst configuration.
* Parent and child queues.
* Queue ordering.
* FastTrack interaction.
* Queue monitoring and troubleshooting.

Simple Queues are particularly useful for straightforward per-customer bandwidth management. More advanced hierarchical traffic control can be implemented using Queue Trees.
