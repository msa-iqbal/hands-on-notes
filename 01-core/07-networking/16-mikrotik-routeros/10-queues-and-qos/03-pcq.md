# Per Connection Queue (PCQ)

Per Connection Queue (PCQ) is a queue type in MikroTik RouterOS designed to distribute bandwidth fairly among multiple users or connections.

PCQ simplifies bandwidth management by dynamically creating sub-queues based on selected classifiers, such as source or destination IP addresses.

It is especially useful in ISP networks, offices, public Wi-Fi networks, and environments where many users share a common internet connection.

# 🗁 What Is PCQ?

PCQ stands for Per Connection Queue.

It is a queue algorithm that creates separate sub-queues for traffic matching a configured classifier.

Instead of manually creating an individual queue for every customer, PCQ can automatically distribute available bandwidth among multiple users or connections.

For example, consider a network with a shared download limit of 100 Mbps and five active users.

```text
              Shared Bandwidth
                  100 Mbps
                      |
                 PCQ Queue
                      |
          +-----------+-----------+
          |           |           |
        User A      User B      User C
        20 Mbps     20 Mbps     20 Mbps
          |           |           |
          +-----------+-----------+
                      |
               Other Users
```

If all five users are active and demand sufficient bandwidth, PCQ can distribute the available rate approximately equally among them.

Actual distribution depends on the configured classifier, queue limits, parent capacity, and traffic demand.

# 🗁 Why Use PCQ?

Without PCQ, administrators may need to create and maintain individual queues for every user.

For a network with hundreds of customers, this becomes difficult to manage.

PCQ provides several advantages:

* Automatic bandwidth distribution.
* Fairer sharing among users.
* Reduced manual queue configuration.
* Dynamic handling of active users.
* Suitable for large client subnets.
* Integration with Simple Queues and Queue Trees.

# 🗁 How PCQ Works

PCQ uses a classifier to identify how traffic should be divided into separate sub-queues.

For example, when `src-address` is used as the classifier, packets from different source IP addresses are placed into different sub-queues.

When `dst-address` is used, packets destined for different IP addresses are separated.

Basic processing:

```text
              Incoming Traffic
                     |
                     v
              PCQ Classifier
                     |
          +----------+----------+
          |          |          |
       Sub-Queue  Sub-Queue  Sub-Queue
        User A     User B     User C
          |          |          |
          +----------+----------+
                     |
                     v
              Queue Scheduler
                     |
                     v
                Network Link
```

Each sub-queue is managed according to the configured PCQ settings.

# 🗁 Understanding PCQ Classifiers

A classifier determines how PCQ separates traffic into sub-queues.

Common classifiers include:

| Classifier | Description | Typical Use |
| --- | --- | --- |
| `src-address` | Separates traffic by source IP | Upload fairness per client |
| `dst-address` | Separates traffic by destination IP | Download fairness per client |
| `src-port` | Separates traffic by source port | Per-source-port classification |
| `dst-port` | Separates traffic by destination port | Per-destination-port classification |
| `src-address,dst-address` | Separates traffic by source and destination IP combination | Per-address-pair fairness |
| `src-port,dst-port` | Separates traffic by port combination | Port-based classification |

For typical customer bandwidth management:

* Upload PCQ commonly uses `src-address`.
* Download PCQ commonly uses `dst-address`.

The correct classifier depends on the packet direction and where the queue processes traffic.

# 🗁 Understanding PCQ Rate

The `pcq-rate` parameter defines the rate allocated to each PCQ sub-queue.

Example:

```routeros
/queue type
add name=pcq-download-custom kind=pcq pcq-rate=10M pcq-classifier=dst-address
```

This configures a PCQ queue type with a rate of 10 Mbps per classified destination address.

If five classified users are active, the potential aggregate demand can reach 50 Mbps, provided the parent or interface has sufficient capacity.

When `pcq-rate=0`, PCQ does not impose a fixed individual rate through this parameter. The available bandwidth is shared according to the queue hierarchy and scheduler behavior.

# 🗁 Understanding PCQ Limit

The `pcq-limit` parameter controls the queue buffer size for each individual PCQ sub-queue.

It is measured in KiB.

Example:

```routeros
/queue type
add name=pcq-download-custom kind=pcq pcq-rate=10M pcq-limit=50KiB pcq-classifier=dst-address
```

A larger buffer can accommodate more queued packets but may increase delay during congestion.

A smaller buffer can reduce queueing delay but may cause more packet drops during traffic bursts.

# 🗁 Understanding PCQ Total Limit

The `pcq-total-limit` parameter controls the total memory limit for all sub-queues created by a PCQ queue instance.

It is measured in KiB.

Example:

```routeros
/queue type
add name=pcq-download-custom kind=pcq pcq-rate=10M pcq-limit=50KiB pcq-total-limit=2000KiB pcq-classifier=dst-address
```

The total limit is shared across the PCQ sub-queues.

It is important to distinguish:

* `pcq-limit`: buffer limit for an individual sub-queue.
* `pcq-total-limit`: aggregate buffer limit for the PCQ queue instance.

# 🗁 Understanding PCQ Burst Parameters

PCQ supports burst configuration.

| Parameter | Description |
| --- | --- |
| `pcq-burst-rate` | Temporary burst rate for each sub-queue |
| `pcq-burst-threshold` | Average-rate threshold for burst eligibility |
| `pcq-burst-time` | Averaging interval used for burst calculations |

Burst allows short periods of higher throughput when traffic conditions satisfy the configured thresholds.

Example:

```routeros
/queue type
add name=pcq-download-burst kind=pcq pcq-rate=10M pcq-burst-rate=15M pcq-burst-threshold=8M pcq-burst-time=10s pcq-classifier=dst-address
```

Burst is temporary and does not increase the permanent bandwidth allocation.

# 🗁 Creating a PCQ Download Queue Type

Suppose a network has multiple clients sharing a 100 Mbps download connection.

The goal is to distribute available download bandwidth fairly among client IP addresses.

Create the PCQ download queue type:

```routeros
/queue type
add name=pcq-download-custom kind=pcq pcq-classifier=dst-address pcq-rate=0
```

The classifier uses destination IP addresses to create separate sub-queues.

A zero PCQ rate allows the classified users to share the bandwidth available from the parent queue rather than enforcing a fixed individual rate.

# 🗁 Creating a PCQ Upload Queue Type

For upload traffic, use source IP addresses to classify clients.

```routeros
/queue type
add name=pcq-upload-custom kind=pcq pcq-classifier=src-address pcq-rate=0
```

Each source address receives its own PCQ sub-queue.

# 🗁 Using PCQ with Simple Queues

PCQ can be assigned as the queue type in a Simple Queue.

Suppose the LAN subnet is `192.168.10.0/24` and the available bandwidth is:

* Upload: 20 Mbps.
* Download: 100 Mbps.

Create the PCQ queue types:

```routeros
/queue type
add name=pcq-upload-custom kind=pcq pcq-classifier=src-address pcq-rate=0
add name=pcq-download-custom kind=pcq pcq-classifier=dst-address pcq-rate=0
```

Create a Simple Queue:

```routeros
/queue simple
add name="Office-PCQ" target=192.168.10.0/24 max-limit=20M/100M queue=pcq-upload-custom/pcq-download-custom
```

The Simple Queue controls the aggregate bandwidth for the subnet.

PCQ divides traffic among classified addresses within that queue.

This is useful when multiple devices share a common bandwidth limit.

# 🗁 Using PCQ with Queue Trees

PCQ can also be used as the queue type in Queue Tree.

For example, create a download parent queue:

```routeros
/queue tree
add name="WAN-Download" parent=bridge max-limit=100M
```

Create a PCQ child queue:

```routeros
/queue tree
add name="Fair-Download" parent="WAN-Download" packet-mark=download-traffic queue=pcq-download-custom max-limit=100M
```

The packet mark `download-traffic` must be created by an appropriate Mangle rule.

The PCQ queue then separates matching packets according to its configured classifier.

The selected parent must correspond to the actual egress path for the traffic.

# 🗁 PCQ Rate vs Parent Max-Limit

These parameters serve different purposes.

| Parameter | Function |
| --- | --- |
| `pcq-rate` | Rate applied to each classified sub-queue |
| Parent `max-limit` | Maximum aggregate bandwidth of the parent |
| Child `max-limit` | Maximum bandwidth available to the child queue |

Example:

```text
Parent Maximum: 100 Mbps
PCQ Rate:        10 Mbps
Active Users:    20
```

Each classified sub-queue has a configured rate of 10 Mbps, but the parent still limits the total aggregate traffic to 100 Mbps.

The configured individual rates do not create additional physical bandwidth.

# 🗁 PCQ Fairness Considerations

PCQ distributes traffic according to its classifier.

The definition of a user therefore depends on the selected classifier.

For example:

* `src-address` treats each source IP as a separate classification.
* `dst-address` treats each destination IP as a separate classification.
* A combination of addresses creates classifications based on address pairs.

If several devices share one private IP through another NAT device, the upstream router may see them as a single source address.

Likewise, one customer using multiple IP addresses may receive multiple PCQ sub-queues.

PCQ fairness is based on classification, not necessarily on the identity of a person or billing account.

# 🗁 PCQ and NAT Considerations

NAT changes packet address information as traffic passes through the router.

PCQ classification must be designed with the packet's address values at the queue processing stage in mind.

For typical routed customer networks:

* Upload classification uses the client's source address.
* Download classification uses the client's destination address.

When traffic passes through NAT, verify which address is visible at the selected queue stage.

Incorrect assumptions about NAT and packet direction can cause unexpected bandwidth distribution.

# 🗁 Monitoring PCQ

Display configured queue types:

```routeros
/queue type print
```

Display detailed queue type information:

```routeros
/queue type print detail
```

Display Simple Queue statistics:

```routeros
/queue simple print stats
```

Display Queue Tree statistics:

```routeros
/queue tree print stats
```

Monitor traffic on an interface:

```routeros
/tool torch interface=ether1
```

These commands help verify aggregate traffic rates and queue activity.

PCQ sub-queues are dynamically created as traffic is classified, so they are not necessarily displayed as individual permanent queue configuration entries.

# 🗁 Common PCQ Problems

| Problem | Possible Cause |
| --- | --- |
| Users do not receive expected rates | Incorrect classifier or parent limit |
| All clients share one sub-queue | Same classification address is visible |
| Download fairness is incorrect | Wrong classifier or packet direction |
| Upload fairness is incorrect | Incorrect source-address classification |
| Aggregate bandwidth exceeds expectation | Parent limit or queue attachment is incorrect |
| Users experience high latency | Excessive buffering or congestion |
| PCQ queue shows no traffic | Packet marks, target matching, or queue attachment is incorrect |

# 🗁 PCQ Best Practices

* Select classifiers according to traffic direction.
* Use `src-address` for typical upload fairness.
* Use `dst-address` for typical download fairness.
* Set realistic parent bandwidth limits.
* Use `pcq-rate=0` when dynamic sharing is required.
* Use a fixed `pcq-rate` when each classified sub-queue needs a defined rate.
* Understand the difference between individual and total queue limits.
* Consider NAT when selecting classifiers.
* Avoid excessively large buffers.
* Monitor actual traffic and queue statistics.
* Test with multiple active clients.
* Verify FastTrack interaction when using Queue Trees and Mangle.

# 🗁 Summary

PCQ is a RouterOS queue type that dynamically distributes bandwidth among classified traffic sources or destinations.

Important concepts include:

* PCQ classifiers.
* Source and destination address classification.
* `pcq-rate`.
* `pcq-limit`.
* `pcq-total-limit`.
* PCQ burst parameters.
* PCQ with Simple Queues.
* PCQ with Queue Trees.
* Fairness and NAT considerations.

PCQ is particularly useful for distributing shared bandwidth among multiple customers without manually creating a separate queue for every user.
