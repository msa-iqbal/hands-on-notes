# Queue Types and Priorities

MikroTik RouterOS provides different queue types and scheduling mechanisms to control how packets are buffered, processed, and transmitted.

Queue types determine how traffic is handled inside a queue, while queue priorities help determine which queues receive preferential treatment when bandwidth is limited.

Understanding these concepts is essential for designing effective bandwidth management policies using Simple Queues and Queue Trees.

# 🗁 What Are Queue Types?

A queue type defines the packet scheduling and buffering algorithm used by a queue.

RouterOS provides several queue types for different traffic-management requirements.

Common queue types include:

| Queue Type | Description |
| --- | --- |
| `pfifo` | Packet-based First-In, First-Out queue |
| `bfifo` | Byte-based First-In, First-Out queue |
| `mq-pfifo` | Multi-queue Packet FIFO |
| `red` | Random Early Detection |
| `sfq` | Stochastic Fairness Queuing |
| `pcq` | Per Connection Queue |
| `cake` | Common Applications Kept Enhanced |
| `fq-codel` | Fair Queuing Controlled Delay |

Available queue types and their supported properties depend on the RouterOS version and platform.

# 🗁 Understanding FIFO

FIFO stands for First-In, First-Out.

Packets are processed in the same order they enter the queue.

The first packet entering the queue is the first packet transmitted.

FIFO does not provide classification-based fairness between users or applications.

```text
             Incoming Packets
                    |
                    v
              FIFO Queue
                    |
             +------+------+
             |      |      |
            P1     P2     P3
             |      |      |
             +------+------+
                    |
                    v
              Outgoing Link
```

FIFO is simple and suitable for basic queueing requirements.

# 🗁 Packet FIFO (PFIFO)

PFIFO limits the queue based on the number of packets.

The `pfifo-limit` parameter defines the maximum number of packets that can wait in the queue.

Example:

```routeros
/queue type
add name=custom-pfifo kind=pfifo pfifo-limit=100
```

This creates a PFIFO queue type with a packet limit of 100.

When the queue becomes full, additional incoming packets are dropped.

PFIFO is useful when a simple packet-count-based buffer is required.

# 🗁 Byte FIFO (BFIFO)

BFIFO limits the queue based on the total number of bytes stored.

The `bfifo-limit` parameter defines the maximum queue size in bytes.

Example:

```routeros
/queue type
add name=custom-bfifo kind=bfifo bfifo-limit=100000
```

This creates a BFIFO queue type with a maximum buffer size of 100,000 bytes.

Unlike PFIFO, BFIFO accounts for packet sizes when determining how much data can be buffered.

# 🗁 Multi-Queue PFIFO (MQ-PFIFO)

MQ-PFIFO is a multi-queue packet FIFO mechanism.

It is designed for environments where network interfaces support multiple hardware transmit queues.

Multiple queues can help distribute packet processing across available transmission queues.

Example:

```routeros
/queue type
print where kind=mq-pfifo
```

MQ-PFIFO availability and behavior depend on the interface driver and hardware platform.

# 🗁 Stochastic Fairness Queuing (SFQ)

SFQ stands for Stochastic Fairness Queuing.

It attempts to distribute bandwidth fairly among multiple traffic flows by using hashing to assign packets to separate flows.

Instead of allowing one flow to dominate the queue, SFQ provides a fairer opportunity for different flows to transmit.

Example:

```routeros
/queue type
add name=custom-sfq kind=sfq
```

SFQ is useful when flow-based fairness is required.

Unlike PCQ, which can classify traffic by IP addresses, SFQ primarily distributes traffic among flows.

# 🗁 Random Early Detection (RED)

RED stands for Random Early Detection.

It manages congestion by randomly dropping packets before a queue becomes completely full.

The goal is to signal congestion early and reduce the likelihood of sustained queue overflow.

RED uses queue occupancy thresholds to determine when packet dropping should begin.

Example:

```routeros
/queue type
add name=custom-red kind=red
```

RED can be useful in congestion-management scenarios, particularly where early congestion signaling is desirable.

Its effectiveness depends on traffic characteristics and configuration.

# 🗁 Common Applications Kept Enhanced (CAKE)

CAKE is an advanced queueing discipline designed to improve bandwidth management and reduce latency under congestion.

It combines several mechanisms, including:

* Flow-based fairness.
* Active Queue Management.
* Traffic shaping.
* Congestion control.
* Optional host-based fairness.

CAKE can help reduce bufferbloat while distributing bandwidth among competing flows or hosts.

Example:

```routeros
/queue type
add name=custom-cake kind=cake
```

The available CAKE parameters depend on the RouterOS release.

# 🗁 Fair Queuing Controlled Delay (FQ-CoDel)

FQ-CoDel combines Fair Queuing with Controlled Delay.

It separates traffic into flows and uses active queue management to control excessive queueing delay.

Its main objectives are:

* Reduce bufferbloat.
* Improve latency during congestion.
* Provide fairness between flows.
* Maintain good throughput.

Example:

```routeros
/queue type
add name=custom-fq-codel kind=fq-codel
```

FQ-CoDel is useful for networks where interactive traffic, such as voice calls, gaming, and browsing, must remain responsive during congestion.

# 🗁 Comparing Queue Types

| Queue Type | Main Purpose | Fairness |
| --- | --- | --- |
| PFIFO | Packet-count-based buffering | No flow fairness |
| BFIFO | Byte-based buffering | No flow fairness |
| MQ-PFIFO | Multi-queue packet buffering | Depends on interface and queue arrangement |
| SFQ | Flow-based distribution | Flow fairness |
| RED | Early congestion management | Not a fairness scheduler |
| PCQ | Classified sub-queue distribution | Based on configured classifier |
| CAKE | Shaping, fairness, and latency management | Advanced flow/host fairness |
| FQ-CoDel | Fair queuing and delay control | Flow fairness |

Queue types solve different problems. Selecting one depends on the network's traffic patterns, hardware capabilities, and bandwidth-management requirements.

# 🗁 What Is Queue Priority?

Queue priority determines the relative preference of a queue when RouterOS schedules traffic under bandwidth constraints.

In RouterOS, queue priority ranges from `1` to `8`.

| Priority | Meaning |
| --- | --- |
| `1` | Highest priority |
| `2` | Very high priority |
| `3` | High priority |
| `4` | Above normal priority |
| `5` | Normal priority |
| `6` | Below normal priority |
| `7` | Low priority |
| `8` | Lowest priority |

A lower numerical value represents a higher priority.

For example:

```text
Priority 1 = Highest
Priority 4 = Medium
Priority 8 = Lowest
```

Priority is not a guaranteed bandwidth allocation.

It influences scheduling preference when queues compete for available bandwidth.

# 🗁 Priority in Simple Queues

Simple Queues support a priority setting.

Example:

```routeros
/queue simple
add name="VoIP" target=192.168.10.10/32 max-limit=10M/10M priority=1/1

add name="Office-PC" target=192.168.10.20/32 max-limit=50M/50M priority=4/4

add name="Guest-WiFi" target=192.168.10.30/32 max-limit=20M/20M priority=8/8
```

The priority property uses the format:

```text
priority=upload-priority/download-priority
```

In this example:

* VoIP traffic receives the highest configured priority.
* Office traffic receives normal priority.
* Guest Wi-Fi traffic receives the lowest priority.

The actual scheduling effect depends on queue hierarchy, traffic demand, and available bandwidth.

# 🗁 Priority in Queue Trees

Queue Trees also support priority.

Example:

```routeros
/queue tree
add name="WAN-Parent" parent=ether1 max-limit=100M

add name="VoIP" parent="WAN-Parent" packet-mark=voip-mark priority=1 max-limit=10M

add name="Business" parent="WAN-Parent" packet-mark=business-mark priority=4 max-limit=60M

add name="Guest" parent="WAN-Parent" packet-mark=guest-mark priority=8 max-limit=50M
```

The packet marks must be created using appropriate Mangle rules.

The parent queue provides the aggregate bandwidth limit.

The child queues use their priority values to influence scheduling when they compete for bandwidth.

# 🗁 Understanding Limit-At and Max-Limit

Priority works alongside bandwidth guarantees and maximum limits.

Two important queue properties are:

| Property | Description |
| --- | --- |
| `limit-at` | Committed or guaranteed bandwidth allocation |
| `max-limit` | Maximum bandwidth the queue can consume |

Consider three queues sharing a 100 Mbps parent:

| Queue | Priority | Limit-At | Max-Limit |
| --- | ---: | ---: | ---: |
| VoIP | 1 | 10 Mbps | 20 Mbps |
| Business | 4 | 30 Mbps | 70 Mbps |
| Guest | 8 | 10 Mbps | 50 Mbps |

The total guaranteed bandwidth is 50 Mbps.

The remaining capacity can be distributed according to traffic demand, queue limits, and scheduling priority.

Priority does not replace `limit-at`.

A high-priority queue without a guaranteed bandwidth allocation does not automatically receive a fixed amount of bandwidth.

# 🗁 Understanding Priority and Queue Hierarchy

Queue priority must be considered within the queue hierarchy.

For Queue Trees, priority is relevant among child queues sharing the same parent.

For Simple Queues, priority operates within the Simple Queue scheduling and hierarchy rules.

A queue's priority does not automatically override unrelated queues elsewhere in the configuration.

When designing hierarchical bandwidth management, consider:

* Parent queue limits.
* Child queue limits.
* Guaranteed bandwidth.
* Queue priority.
* Queue hierarchy.
* Actual traffic demand.

# 🗁 Priority Configuration Example

Suppose an organization has three traffic categories:

* VoIP: highest priority.
* Business applications: normal priority.
* Guest internet: lowest priority.

Create queue types if needed:

```routeros
/queue type
add name=voip-pfifo kind=pfifo pfifo-limit=50
add name=business-pfifo kind=pfifo pfifo-limit=100
add name=guest-pfifo kind=pfifo pfifo-limit=100
```

Create a parent Queue Tree:

```routeros
/queue tree
add name="WAN-Traffic" parent=ether1 max-limit=100M
```

Create child queues:

```routeros
/queue tree
add name="VoIP" parent="WAN-Traffic" packet-mark=voip-mark priority=1 limit-at=10M max-limit=20M queue=voip-pfifo

add name="Business" parent="WAN-Traffic" packet-mark=business-mark priority=4 limit-at=30M max-limit=70M queue=business-pfifo

add name="Guest" parent="WAN-Traffic" packet-mark=guest-mark priority=8 limit-at=10M max-limit=50M queue=guest-pfifo
```

The packet marks must be configured separately.

The example demonstrates queue hierarchy and priority configuration. Interface direction, packet marking, and parent placement must be adapted to the actual network topology.

# 🗁 Selecting the Right Queue Type

Use the following general guidance:

| Requirement | Suitable Queue Type |
| --- | --- |
| Basic packet buffering | PFIFO |
| Byte-based buffering | BFIFO |
| Fairness between traffic flows | SFQ |
| Fair bandwidth distribution by IP address | PCQ |
| Early congestion signaling | RED |
| Advanced shaping and bufferbloat control | CAKE |
| Fair queuing with delay management | FQ-CoDel |

The best queue type depends on the required behavior and RouterOS platform support.

# 🗁 FastTrack Considerations

FastTrack can bypass parts of the normal packet-processing path, including queue processing relevant to Simple Queues and Queue Trees.

When traffic is FastTracked, configured queue policies may not account for all packets as expected.

For accurate bandwidth management:

* Review FastTrack rules.
* Exclude traffic that must be shaped from FastTrack.
* Verify packet marks and queue counters.
* Test under realistic traffic conditions.

# 🗁 Monitoring Queue Types and Priorities

Display configured queue types:

```routeros
/queue type print
```

Display detailed queue type settings:

```routeros
/queue type print detail
```

Display Simple Queue configuration:

```routeros
/queue simple print detail
```

Display Simple Queue statistics:

```routeros
/queue simple print stats
```

Display Queue Tree configuration:

```routeros
/queue tree print detail
```

Display Queue Tree statistics:

```routeros
/queue tree print stats
```

These commands help verify queue configuration, traffic counters, and bandwidth usage.

# 🗁 Best Practices

* Select queue types according to the actual traffic-management requirement.
* Use PCQ for IP-based bandwidth distribution.
* Use SFQ when flow-based fairness is appropriate.
* Consider CAKE or FQ-CoDel for latency-sensitive networks where supported.
* Use RED only when its congestion-management behavior is appropriate.
* Set priorities according to business requirements.
* Remember that priority `1` is higher than priority `8`.
* Use `limit-at` for bandwidth guarantees.
* Use `max-limit` to control maximum bandwidth.
* Keep parent and child queue limits consistent.
* Verify queue hierarchy and packet direction.
* Review FastTrack behavior.
* Test queue performance before deploying changes to production.

# 🗁 Summary

Queue types define how RouterOS buffers, schedules, and manages packets.

Queue priorities determine the relative scheduling preference of competing queues.

Important concepts include:

* PFIFO and BFIFO.
* MQ-PFIFO.
* SFQ.
* RED.
* PCQ.
* CAKE.
* FQ-CoDel.
* Queue priority.
* `limit-at` and `max-limit`.
* Queue hierarchy and scheduling.

Selecting the correct queue type and priority configuration helps build predictable bandwidth-management policies while maintaining network performance.
