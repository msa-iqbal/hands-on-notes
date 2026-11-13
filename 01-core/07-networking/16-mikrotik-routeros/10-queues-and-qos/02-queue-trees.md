# Queue Trees

Queue Tree is an advanced bandwidth management mechanism in MikroTik RouterOS.

It allows administrators to build hierarchical queue structures and control traffic according to packet marks, interfaces, and parent-child relationships.

Queue Trees are particularly useful for ISP networks, enterprise environments, and networks that require traffic classification and differentiated bandwidth allocation.

Unlike Simple Queues, Queue Trees do not automatically identify individual clients through a target address. They commonly rely on firewall Mangle packet marks to classify traffic.

# 🗁 What Are Queue Trees?

Queue Tree is a hierarchical queuing system that allows multiple queues to share bandwidth through parent-child relationships.

A parent queue can control the total bandwidth available to a group, while child queues allocate bandwidth to different traffic classes or users.

For example:

```text
                 WAN Interface
                 100 Mbps
                      |
                 Parent Queue
                      |
          +-----------+-----------+
          |           |           |
        Voice       Browsing    Downloads
        10 Mbps      30 Mbps     60 Mbps
```

This structure allows administrators to apply different bandwidth policies to different types of traffic.

# 🗁 Why Use Queue Trees?

Queue Trees are useful when a network requires more advanced traffic management than individual Simple Queues can provide.

Common use cases include:

* Prioritizing VoIP traffic.
* Separating browsing and download traffic.
* Managing upload and download bandwidth independently.
* Implementing hierarchical bandwidth allocation.
* Managing ISP customer traffic.
* Applying bandwidth policies using packet marks.
* Controlling traffic on specific interfaces.

# 🗁 Simple Queues vs Queue Trees

| Feature | Simple Queues | Queue Trees |
| --- | --- | --- |
| Configuration | Relatively simple | More advanced |
| Target matching | IP addresses and subnets | Packet marks and queue hierarchy |
| Hierarchy | Parent-child support | Parent-child support |
| Traffic classification | Target and other queue properties | Commonly uses Mangle packet marks |
| Interface-based design | Less direct | Parent can be an interface |
| Queue ordering | Important | Parent-child structure is central |
| Typical use | Per-user bandwidth limits | Advanced traffic management |

Both mechanisms can manage bandwidth. The appropriate choice depends on network requirements and configuration complexity.

# 🗁 Queue Tree Configuration Path

Queue Trees are configured under:

```routeros
/queue tree
```

Display existing Queue Trees:

```routeros
/queue tree print
```

Display detailed configuration:

```routeros
/queue tree print detail
```

Display queue statistics:

```routeros
/queue tree print stats
```

# 🗁 Understanding Queue Tree Parameters

| Parameter | Description |
| --- | --- |
| `name` | Queue name |
| `parent` | Parent queue or supported interface/global parent |
| `packet-mark` | Matches packets carrying a specified packet mark |
| `max-limit` | Maximum queue bandwidth |
| `limit-at` | Guaranteed rate when parent capacity and resources allow |
| `priority` | Queue priority from 1 to 8 |
| `queue` | Queue type used for packet scheduling |
| `burst-limit` | Temporary burst rate |
| `burst-threshold` | Average traffic threshold for burst eligibility |
| `burst-time` | Averaging interval for burst calculations |

# 🗁 Understanding Parent Queues

A parent queue defines a shared bandwidth resource for its child queues.

For example, an organization has a 100 Mbps download connection.

The parent queue controls the total bandwidth available to the child queues.

```text
               Download Parent
                   100 Mbps
                       |
             +---------+---------+
             |         |         |
           Voice     Office    Guest
           10 Mbps   60 Mbps   30 Mbps
```

The child queues share the parent's available capacity.

A parent queue does not automatically guarantee that every child will receive its configured maximum. Actual allocation depends on demand, `limit-at`, priority, and available capacity.

# 🗁 Understanding Child Queues

Child queues belong to a parent queue and manage individual traffic classes or groups.

For example:

* Voice traffic can receive higher priority.
* Business traffic can receive guaranteed bandwidth.
* Guest traffic can have a lower priority.
* Bulk downloads can be restricted.

A child queue can have its own `max-limit`, `limit-at`, and `priority`.

The sum of child guarantees should be planned against the parent's available capacity.

# 🗁 Understanding Global and Interface Parents

Queue Tree supports different parent attachment points.

**Global parent**

A global parent is used for traffic associated with the global queue hierarchy.

Example:

```routeros
/queue tree add name="Global-Download" parent=global max-limit=100M
```

The global parent is useful for building a queue hierarchy that is not directly attached to a specific interface.

**Interface parent**

An interface parent attaches a queue to traffic associated with a particular interface.

Example:

```routeros
/queue tree add name="WAN-Upload" parent=ether1 max-limit=20M
```

This creates a queue associated with the `ether1` interface.

The correct parent depends on the direction of traffic and where the bottleneck is located.

# 🗁 Understanding Upload and Download Direction

Queue Tree design must account for the direction in which packets are transmitted.

For a typical internet connection:

**Upload**

Traffic travels from the LAN toward the WAN.

```text
LAN Client
    |
    v
MikroTik Router
    |
    v
WAN Interface
    |
    v
Internet
```

The WAN interface is the egress interface for upload traffic.

**Download**

Traffic travels from the WAN toward the LAN.

```text
Internet
    |
    v
WAN Interface
    |
    v
MikroTik Router
    |
    v
LAN Client
```

The LAN interface or bridge is the egress interface for download traffic.

Queue Tree parent selection must follow the actual packet-processing and egress path.

# 🗁 Understanding Packet Marks

Queue Trees commonly use packet marks created by firewall Mangle rules.

A packet mark is an internal RouterOS label attached to a packet for later processing.

For example, packets can be classified into:

* Voice traffic.
* Browsing traffic.
* Download traffic.
* Upload traffic.

Mangle rules assign packet marks, and Queue Tree entries use those marks to identify which packets belong to each queue.

Packet marks are not transmitted as part of the packet over the network.

# 🗁 Creating Packet Marks

The following example marks traffic from a specific LAN client.

```routeros
/ip firewall mangle
add chain=forward src-address=192.168.10.10 action=mark-packet new-packet-mark=client_upload passthrough=no
add chain=forward dst-address=192.168.10.10 action=mark-packet new-packet-mark=client_download passthrough=no
```

The rules classify:

* Traffic sourced from `192.168.10.10` as upload.
* Traffic destined for `192.168.10.10` as download.

These are basic examples. A production classification design must account for traffic direction, routing, existing Mangle rules, and other packet marks.

# 🗁 Creating a Basic Upload Queue Tree

Assume:

| Parameter | Value |
| --- | --- |
| WAN interface | ether1 |
| Upload capacity | 20 Mbps |
| Client IP | 192.168.10.10 |
| Client upload limit | 5 Mbps |

First, mark upload traffic:

```routeros
/ip firewall mangle
add chain=forward src-address=192.168.10.10 action=mark-packet new-packet-mark=client_upload passthrough=no
```

Create the upload parent queue:

```routeros
/queue tree
add name="WAN-Upload" parent=ether1 max-limit=20M
```

Create the client child queue:

```routeros
/queue tree
add name="Client-Upload" parent="WAN-Upload" packet-mark=client_upload max-limit=5M
```

The parent controls the total upload rate, while the child limits the marked client's upload traffic.

# 🗁 Creating a Basic Download Queue Tree

Assume:

| Parameter | Value |
| --- | --- |
| LAN bridge | bridge |
| Download capacity | 100 Mbps |
| Client IP | 192.168.10.10 |
| Client download limit | 20 Mbps |

Mark download traffic:

```routeros
/ip firewall mangle
add chain=forward dst-address=192.168.10.10 action=mark-packet new-packet-mark=client_download passthrough=no
```

Create the download parent queue:

```routeros
/queue tree
add name="LAN-Download" parent=bridge max-limit=100M
```

Create the client child queue:

```routeros
/queue tree
add name="Client-Download" parent="LAN-Download" packet-mark=client_download max-limit=20M
```

The queue is attached to the LAN bridge because download packets leave the router through that interface.

This example assumes the bridge is the actual egress interface for the client traffic.

# 🗁 Creating Multiple Traffic Classes

Queue Trees can separate traffic into multiple classes.

For example:

| Traffic Class | Maximum Rate | Priority |
| --- | ---: | ---: |
| Voice | 10 Mbps | 1 |
| Business | 40 Mbps | 2 |
| Browsing | 30 Mbps | 4 |
| Downloads | 20 Mbps | 8 |

Create the parent queue:

```routeros
/queue tree
add name="WAN-Upload" parent=ether1 max-limit=100M
```

Create child queues:

```routeros
/queue tree
add name="Voice" parent="WAN-Upload" packet-mark=voice max-limit=10M priority=1
add name="Business" parent="WAN-Upload" packet-mark=business max-limit=40M priority=2
add name="Browsing" parent="WAN-Upload" packet-mark=browsing max-limit=30M priority=4
add name="Downloads" parent="WAN-Upload" packet-mark=downloads max-limit=20M priority=8
```

The packet marks must already exist and match the traffic classification rules.

These maximum rates are ceilings, not guaranteed reservations. Priority influences scheduling when queues compete for resources.

# 🗁 Understanding Queue Tree Priority

Queue Tree priority determines the relative preference of child queues under the same parent when bandwidth is contested.

RouterOS uses values from 1 to 8:

| Priority | Meaning |
| --- | --- |
| 1 | Highest |
| 2 | Very high |
| 3 | High |
| 4 | Above normal |
| 5 | Normal |
| 6 | Below normal |
| 7 | Low |
| 8 | Lowest |

Priority is meaningful in a hierarchy with competing child queues.

It does not mean that a queue with priority 1 always receives bandwidth regardless of its parent or available capacity.

# 🗁 Understanding Limit-At and Max-Limit

Queue Tree supports both guaranteed and maximum bandwidth parameters.

* `limit-at` defines the guaranteed rate when parent capacity and resources allow.
* `max-limit` defines the maximum permitted rate.

Example:

```routeros
/queue tree
add name="Business" parent="WAN-Upload" packet-mark=business limit-at=10M max-limit=40M priority=2
```

The queue requests a guaranteed rate of 10 Mbps and can use up to 40 Mbps when resources are available.

When designing a hierarchy, ensure that the total guaranteed rates are compatible with the parent capacity.

# 🗁 FastTrack and Queue Trees

FastTrack can bypass parts of the normal firewall and Mangle processing path.

This can prevent marked traffic from reaching the expected Queue Tree processing stage.

For Queue Trees that depend on packet marks:

* Review FastTrack rules.
* Verify that relevant traffic receives packet marks.
* Exclude traffic from FastTrack when required.
* Check queue counters during traffic tests.
* Confirm that the selected parent processes the intended traffic.

Queue Trees attached to an interface and queue trees using global processing can have different interactions with packet-processing paths. Validate the actual configuration rather than assuming all queue types behave identically.

# 🗁 Hardware Offloading Considerations

Hardware offloading allows supported network hardware to process certain traffic operations without requiring the CPU to handle every packet.

Depending on the device, bridge configuration, interface, and traffic path, hardware offloading can affect which packets are visible to CPU-based processing features.

When implementing Queue Trees:

* Verify whether traffic traverses the CPU processing path.
* Check bridge and switch-chip offloading behavior.
* Confirm that packet marks are being applied.
* Monitor queue counters.
* Test traffic on the actual hardware.

Do not assume that every hardware-offloaded packet will be processed by a CPU-based queue configuration.

# 🗁 Monitoring Queue Trees

Display configured Queue Trees:

```routeros
/queue tree print
```

Display detailed configuration:

```routeros
/queue tree print detail
```

Display queue statistics:

```routeros
/queue tree print stats
```

Display Mangle rules:

```routeros
/ip firewall mangle print
```

Display Mangle rule counters:

```routeros
/ip firewall mangle print stats
```

Monitor interface traffic:

```routeros
/tool torch interface=ether1
```

These commands help verify:

* Whether traffic matches the Mangle rules.
* Whether packet marks are being applied.
* Whether Queue Tree counters increase.
* Whether configured bandwidth limits are effective.
* Whether the expected interface carries the traffic.

# 🗁 Common Queue Tree Problems

| Problem | Possible Cause |
| --- | --- |
| Queue counters remain zero | Packet marks do not match or traffic bypasses the queue |
| Traffic exceeds expected limit | Incorrect parent, mark, or FastTrack interaction |
| Download queue does not work | Wrong egress interface or direction |
| Upload queue does not work | Incorrect WAN parent or packet classification |
| Priority has no visible effect | No contention between competing queues |
| Guaranteed rate is unavailable | Parent capacity is insufficient |
| Some traffic is not classified | Mangle rules do not match all required traffic |

# 🗁 Queue Tree Best Practices

* Plan the queue hierarchy before configuring it.
* Identify the actual WAN and LAN egress interfaces.
* Use meaningful parent and child queue names.
* Create packet marks that clearly represent traffic classes.
* Ensure Mangle rules classify the intended packets.
* Set realistic parent bandwidth limits.
* Keep guaranteed rates within available parent capacity.
* Use priority only where differentiated scheduling is required.
* Review FastTrack interaction.
* Check hardware offloading behavior.
* Monitor queue and Mangle counters.
* Test upload and download independently.

# 🗁 Summary

Queue Tree is an advanced RouterOS bandwidth management mechanism that supports hierarchical traffic control.

Important concepts include:

* Parent and child queues.
* Global and interface parents.
* Upload and download direction.
* Mangle packet marks.
* `max-limit` and `limit-at`.
* Queue priority.
* FastTrack interaction.
* Hardware offloading considerations.
* Queue monitoring and troubleshooting.

Queue Trees are particularly useful when traffic must be classified and managed through a structured hierarchy.
