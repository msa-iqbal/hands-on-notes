# MikroTik Simple Queues

Simple Queues are a bandwidth-management feature in MikroTik RouterOS used to control upload and download traffic for specific devices, IP addresses, or network ranges.

They allow network administrators to configure bandwidth limits, minimum bandwidth guarantees, traffic priorities, and queueing policies without creating complex firewall packet-marking configurations.

Simple Queues are commonly used in home networks, offices, and ISP environments to manage customer bandwidth.

# 🗁 What Are Simple Queues?

A Simple Queue is a RouterOS queue configuration that controls traffic associated with a specified target.

The target can be:

* A single IP address.
* A subnet.
* Multiple IP addresses.
* A network address range.

For example, an administrator can create a Simple Queue for a customer with the IP address `192.168.10.10` and configure:

* Upload maximum: 10 Mbps
* Download maximum: 20 Mbps

The router then manages traffic for that target according to the queue configuration.

# 🗁 Why Simple Queues Are Used

Simple Queues provide a straightforward way to implement bandwidth-management policies.

Common use cases include:

* **Bandwidth Limiting:** Restrict upload and download speeds.
* **Subscriber Management:** Assign bandwidth limits to individual ISP customers.
* **Network Fairness:** Manage bandwidth usage among multiple users.
* **Priority Management:** Give selected queues higher scheduling priority.
* **Minimum Bandwidth:** Configure committed bandwidth using `limit-at`.
* **Traffic Monitoring:** Observe queue traffic and packet statistics.

# 🗁 Simple Queue Architecture

A Simple Queue contains several important configuration properties.

| Property | Description |
| --- | --- |
| Name | Identifies the queue |
| Target | Defines the IP address or network being controlled |
| Max Limit | Defines the maximum upload and download rates |
| Limit At | Defines the committed or guaranteed upload and download rates, subject to available parent capacity |
| Priority | Defines queue scheduling priority |
| Queue Type | Selects the queue discipline |
| Burst | Configures temporary burst behavior |
| Parent | Places the queue within a hierarchical queue structure |

# 🗁 Upload and Download Direction

One important detail when configuring Simple Queues is the direction of traffic.

For Simple Queues, the rate parameters are written in this order:

```text
upload/download
```

Consider a customer with this IP address:

```text
192.168.10.10
```

The customer's traffic directions are:

| Direction | Traffic Flow |
| --- | --- |
| Upload | Customer → Internet |
| Download | Internet → Customer |

Example:

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 max-limit=10M/20M
```

This configures:

* Upload: 10 Mbps
* Download: 20 Mbps

# 🗁 Creating a Simple Queue

The following command creates a Simple Queue for a single customer.

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 max-limit=10M/20M
```

Explanation:

| Parameter | Description |
| --- | --- |
| `add` | Creates a new queue |
| `name` | Assigns a queue name |
| `target` | Identifies the customer IP address |
| `max-limit` | Sets maximum upload/download rates |

To view configured Simple Queues:

```routeros
/queue simple print
```

To display detailed queue information:

```routeros
/queue simple print detail
```

# 🗁 Understanding Max Limit

The `max-limit` parameter defines the maximum rate that a queue can reach.

For example:

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 max-limit=10M/20M
```

| Direction | Max Limit |
| --- | ---: |
| Upload | 10 Mbps |
| Download | 20 Mbps |

The queue cannot exceed these configured maximum rates under normal queue processing.

A maximum limit is not a guaranteed bandwidth reservation.

# 🗁 Understanding Limit At

The `limit-at` parameter defines the committed or guaranteed data rate for a queue, subject to available capacity and the configuration of its parent queue.

For example:

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 limit-at=5M/10M max-limit=10M/20M
```

This configures:

| Parameter | Upload | Download |
| --- | ---: | ---: |
| Limit At | 5 Mbps | 10 Mbps |
| Max Limit | 10 Mbps | 20 Mbps |

The customer has a committed rate of 5 Mbps upload and 10 Mbps download when sufficient parent capacity is available.

The customer can use more bandwidth when capacity is available, up to the configured maximum.

# 🗁 Understanding Priority

Simple Queues support eight priority levels.

| Priority | Meaning |
| --- | --- |
| 1 | Highest |
| 2 | Very High |
| 3 | High |
| 4 | Above Normal |
| 5 | Normal |
| 6 | Below Normal |
| 7 | Low |
| 8 | Lowest |

Example:

```routeros
/queue simple
add name="VoIP" target=192.168.10.10/32 max-limit=5M/5M priority=1/1
add name="Downloads" target=192.168.10.20/32 max-limit=20M/20M priority=8/8
```

The VoIP queue receives higher scheduling priority than the download queue when they compete for bandwidth under the relevant queue hierarchy.

Priority does not reserve bandwidth by itself. Use `limit-at` when a committed bandwidth rate is required.

# 🗁 Queue Types

Queue types define how packets are managed inside a queue.

RouterOS provides several queue disciplines, including:

| Queue Type | Description |
| --- | --- |
| `default` | Default queue type |
| `ethernet-default` | Queue type optimized for Ethernet traffic |
| `only-hardware-queue` | Uses hardware queueing where supported |
| `multi-queue-ethernet-default` | Supports multiple hardware transmit queues where available |
| `pfifo` | Packet-based First In, First Out |
| `bfifo` | Byte-based First In, First Out |
| `sfq` | Stochastic Fairness Queuing |
| `red` | Random Early Detection |
| `fq-codel` | Flow Queue Controlled Delay |
| `cake` | Common Applications Kept Enhanced |

Available queue types and their capabilities depend on the RouterOS version and device architecture.

To view available queue types:

```routeros
/queue type print
```

To create a custom queue type:

```routeros
/queue type
add name="Customer-FQ-CoDel" kind=fq-codel
```

The custom queue type can then be assigned to a Simple Queue.

# 🗁 Burst Configuration

Burst is a Simple Queue feature that allows traffic to temporarily exceed the configured `max-limit` under specific conditions.

Burst configuration uses the following parameters:

| Parameter | Description |
| --- | --- |
| `burst-limit` | Maximum rate allowed during a burst |
| `burst-threshold` | Average-rate threshold used to determine burst eligibility |
| `burst-time` | Time interval used to calculate the average rate |

Example:

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 \
    max-limit=10M/20M \
    burst-limit=15M/30M \
    burst-threshold=8M/16M \
    burst-time=16s
```

This configures temporary burst limits above the normal maximum rates.

Burst behavior depends on the calculated average traffic rate over the configured interval. It does not provide unlimited additional bandwidth.

# 🗁 Managing Multiple Customers

An ISP may use Simple Queues to assign individual bandwidth policies to multiple subscribers.

Example:

```routeros
/queue simple
add name="Customer-A" target=192.168.10.10/32 max-limit=10M/20M
add name="Customer-B" target=192.168.10.11/32 max-limit=15M/30M
add name="Customer-C" target=192.168.10.12/32 max-limit=20M/40M
```

Customer bandwidth policies:

| Customer | Upload | Download |
| --- | ---: | ---: |
| Customer A | 10 Mbps | 20 Mbps |
| Customer B | 15 Mbps | 30 Mbps |
| Customer C | 20 Mbps | 40 Mbps |

Each queue controls the traffic associated with its target.

For a larger subscriber network, administrators may use parent queues and hierarchical queueing to manage aggregate bandwidth and subscriber-level limits.

# 🗁 Parent and Child Queues

Simple Queues support hierarchical queue organization.

A parent queue can define an aggregate bandwidth limit, while child queues control individual customers.

Example:

```routeros
/queue simple
add name="ISP-Total" target=192.168.10.0/24 max-limit=50M/100M
add name="Customer-A" parent="ISP-Total" target=192.168.10.10/32 max-limit=10M/20M
add name="Customer-B" parent="ISP-Total" target=192.168.10.11/32 max-limit=15M/30M
add name="Customer-C" parent="ISP-Total" target=192.168.10.12/32 max-limit=20M/40M
```

The parent controls the aggregate rate, while each child controls an individual customer's rate.

The sum of child maximum rates can exceed the parent limit because the maximum rates represent individual ceilings, not simultaneous guarantees.

Actual traffic sharing depends on queue scheduling, committed rates, and available parent capacity.

# 🗁 Monitoring Simple Queues

RouterOS provides commands to monitor queue activity.

Display all Simple Queues:

```routeros
/queue simple print
```

Display detailed configuration:

```routeros
/queue simple print detail
```

Monitor queue statistics in real time:

```routeros
/queue simple monitor [find name="Customer-A"]
```

Monitor all queues:

```routeros
/queue simple monitor [find]
```

Important monitoring information includes:

* Current upload rate.
* Current download rate.
* Packet counters.
* Byte counters.
* Queue usage.
* Dropped packets.

These statistics help administrators identify bandwidth consumption and troubleshoot performance problems.

# 🗁 FastTrack Considerations

RouterOS FastTrack accelerates eligible connection processing by bypassing parts of the normal firewall and packet-processing path.

FastTracked traffic can bypass Simple Queue processing in applicable configurations.

Therefore, if a Simple Queue does not appear to limit expected traffic, check:

* Whether FastTrack is enabled.
* Whether the target traffic is FastTracked.
* Whether the queue target and packet direction are correct.
* Whether the traffic follows the expected RouterOS packet-processing path.

QoS designs must account for FastTrack rather than assuming every packet passes through every queue.

# 🗁 Simple Queues vs Queue Trees

Simple Queues and Queue Trees are both used for bandwidth management, but they have different configuration models.

| Feature | Simple Queues | Queue Trees |
| --- | --- | --- |
| Configuration | Relatively simple | More flexible |
| Target | Direct IP or network targeting | Parent and packet-mark-based classification |
| Packet marking | Not normally required for basic targeting | Commonly required for traffic classification |
| Hierarchical management | Supported | Supported |
| Advanced QoS | Suitable for common policies | Suitable for complex policies |
| Typical use | Subscriber bandwidth limits | Advanced traffic classes and aggregate policies |

Simple Queues are often convenient for small and medium networks.

Queue Trees are useful when administrators need more detailed classification and scheduling control.

# 🗁 Best Practices

* Use static IP addresses or reliable address assignments for subscriber targets.
* Configure upload and download limits in the correct order.
* Use `limit-at` only when committed bandwidth is required.
* Use `max-limit` to enforce the maximum permitted rate.
* Organize customer queues under appropriate parent queues when aggregate control is needed.
* Avoid unnecessary overlapping targets.
* Select suitable queue types based on the traffic profile.
* Monitor queue counters and dropped packets.
* Account for FastTrack behavior.
* Test bandwidth policies using realistic traffic conditions.
* Consider Queue Trees and PCQ when a more advanced QoS design is required.

# 🗁 Summary

MikroTik Simple Queues provide a straightforward way to manage upload and download bandwidth for individual devices and network ranges.

They support maximum bandwidth limits, committed bandwidth rates, priority scheduling, burst configuration, and hierarchical queue management.

Simple Queues are especially useful for subscriber bandwidth management and everyday network administration.

For more complex traffic classification and advanced scheduling, Queue Trees and PCQ provide additional flexibility.
