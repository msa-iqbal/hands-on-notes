# Bandwidth Planning

Bandwidth planning is the process of estimating, allocating, monitoring, and managing network capacity to ensure reliable connectivity and sufficient performance.

For an Internet Service Provider (ISP), bandwidth planning is essential for delivering Internet services to subscribers while maintaining network performance and controlling operational costs.

An ISP must plan capacity across its entire infrastructure, including upstream transit, Internet Exchange Points (IXPs), core routers, aggregation links, access networks, and subscriber connections.

Effective bandwidth planning helps prevent congestion, improve Quality of Service (QoS), support business growth, and determine when network upgrades are required.

# 🗁 Bandwidth Planning Fundamentals

Bandwidth represents the maximum data-transfer capacity of a network link over a given period.

It is generally expressed in bits per second.

Common bandwidth units:

| Unit | Meaning |
| --- | --- |
| Kbps | Kilobits per second |
| Mbps | Megabits per second |
| Gbps | Gigabits per second |
| Tbps | Terabits per second |

For example:

```text
1 Gbps = 1,000 Mbps
1 Mbps = 1,000 Kbps
1 Gbps = 1,000,000 Kbps
```

Networking equipment and service providers commonly use decimal units for link speeds.

Bandwidth planning involves understanding how much capacity is available, how much is being consumed, and how much will be needed in the future.

### 🖆 Why Bandwidth Planning Matters for ISPs

An ISP must provide sufficient capacity for:

* Residential subscribers.
* Business customers.
* Dedicated Internet access.
* Upstream transit.
* IXP and peering connections.
* Internal infrastructure.
* Network management and monitoring.
* Future subscriber growth.

Insufficient capacity can cause congestion, packet loss, increased latency, and poor customer experience.

Excessive unused capacity, on the other hand, can increase infrastructure and transit costs.

The objective is to balance performance, reliability, scalability, and cost.

# 🗁 Bandwidth vs Throughput

Bandwidth and throughput are related but represent different concepts.

| Term | Description |
| --- | --- |
| Bandwidth | Maximum theoretical capacity of a link |
| Throughput | Actual rate of successful data transfer |
| Goodput | Useful application payload delivered per second |
| Utilization | Percentage of available link capacity being used |

Example:

```text
Link Capacity: 1 Gbps
Actual Throughput: 650 Mbps
Link Utilization: 65%
```

Throughput is generally lower than the theoretical link capacity because of protocol overhead, congestion, packet loss, and other network conditions.

### 🖆 Calculating Link Utilization

The basic formula is:

```text
Utilization (%) =
(Actual Traffic / Link Capacity) × 100
```

Example:

```text
Link Capacity: 10 Gbps
Current Traffic: 7 Gbps

Utilization = (7 / 10) × 100

Utilization = 70%
```

This calculation is useful for monitoring individual network links.

# 🗁 Network Capacity and Utilization

Network capacity is the maximum traffic a network component or link can handle under its specified operating conditions.

An ISP should monitor capacity at every important network layer.

| Network Component | Capacity Planning Focus |
| --- | --- |
| Upstream transit | International and global Internet traffic |
| IXP connection | Local and regional peering traffic |
| Core routers | Aggregate traffic forwarding |
| Aggregation switches | Combined access network traffic |
| OLT uplinks | PON subscriber traffic |
| Access switches | Local subscriber distribution |
| Wireless backhaul | Radio link capacity |
| Server infrastructure | DNS, RADIUS, caching, and other services |

### 🖆 Link Utilization Thresholds

An ISP can establish operational thresholds for identifying links that require attention.

Illustrative thresholds:

| Utilization | Operational Interpretation |
| --- | --- |
| Below 50% | Generally comfortable capacity |
| 50–70% | Normal utilization range in many designs |
| 70–80% | Monitor growth and peak traffic |
| 80–90% | Review upgrade requirements |
| Above 90% | High congestion risk, depending on traffic patterns |

These are planning guidelines, not universal engineering limits.

A link with bursty traffic may experience congestion even when its average utilization is low.

Conversely, a consistently utilized link may operate acceptably if its queueing, latency, and loss characteristics remain within service objectives.

# 🗁 ISP Bandwidth Requirements

An ISP must estimate bandwidth requirements based on its subscribers, service packages, network topology, and traffic patterns.

Important factors include:

* Number of active subscribers.
* Subscriber speed packages.
* Simultaneous usage.
* Average traffic per subscriber.
* Peak-hour traffic.
* Business customer commitments.
* Video streaming consumption.
* Software downloads and updates.
* Local CDN traffic.
* International traffic.
* Expected subscriber growth.

A simple subscriber count is not enough to determine upstream bandwidth requirements.

For example, an ISP with 5,000 subscribers on 50 Mbps packages does not automatically need 250 Gbps of Internet capacity. Not all subscribers use their maximum package speed simultaneously.

# 🗁 Subscriber Bandwidth Planning

Subscriber bandwidth planning estimates the capacity required to serve customers according to their subscribed packages and actual usage behavior.

### 🖆 Subscriber Speed Packages

Example ISP service packages:

| Package | Download Speed | Upload Speed |
| --- | ---: | ---: |
| Basic | 20 Mbps | 10 Mbps |
| Standard | 40 Mbps | 20 Mbps |
| Premium | 60 Mbps | 30 Mbps |
| Business | 100 Mbps | 100 Mbps |
| Enterprise | 500 Mbps | 500 Mbps |

An ISP should distinguish between:

* Advertised package speed.
* Configured subscriber rate limit.
* Actual network throughput.
* Guaranteed or committed bandwidth, if offered.

A residential best-effort package does not necessarily guarantee that the subscriber can continuously consume the full advertised rate.

### 🖆 Aggregate Subscriber Bandwidth

Aggregate subscribed bandwidth is the sum of the configured package speeds.

Example:

| Package | Subscribers | Speed | Aggregate Download |
| --- | ---: | ---: | ---: |
| Basic | 1,000 | 20 Mbps | 20 Gbps |
| Standard | 2,000 | 40 Mbps | 80 Gbps |
| Premium | 1,000 | 60 Mbps | 60 Gbps |
| Business | 100 | 100 Mbps | 10 Gbps |
| **Total** | **4,100** | | **170 Gbps** |

The total subscribed download capacity is 170 Gbps.

This does not mean the ISP must purchase 170 Gbps of upstream bandwidth.

Actual demand depends on simultaneous usage and traffic patterns.

### 🖆 Concurrent Usage

Concurrent usage refers to the number of subscribers actively consuming network resources at the same time.

Example:

```text
Total Subscribers: 4,100
Active Subscribers During Peak Hour: 2,500
Concurrent Usage Percentage:

(2,500 / 4,100) × 100 = 60.98%
```

Concurrent subscriber count alone does not determine bandwidth consumption because active users may consume very different amounts of traffic.

A more useful planning model combines concurrency with measured average throughput.

# 🗁 Oversubscription and Contention Ratios

Oversubscription is the practice of selling more aggregate subscriber bandwidth than the network has available for simultaneous use.

It is based on the fact that subscribers generally do not use their maximum package speed continuously.

### 🖆 Oversubscription Ratio

A simple oversubscription ratio is:

```text
Oversubscription Ratio =
Total Subscribed Bandwidth / Available Capacity
```

Example:

```text
Total Subscribed Bandwidth: 100 Gbps
Available Capacity: 10 Gbps

Oversubscription Ratio:

100 / 10 = 10:1
```

The network has a 10:1 aggregate subscription-to-capacity ratio.

This does not mean that every subscriber will receive one-tenth of their package speed. It is a planning ratio, not a direct per-user speed guarantee.

### 🖆 Example Oversubscription Ratios

| Network Type | Illustrative Ratio |
| --- | ---: |
| Residential broadband | 10:1 |
| High-density residential access | 20:1 |
| Business broadband | 5:1 |
| Dedicated Internet access | 1:1 |
| Enterprise dedicated service | 1:1 |

These are illustrative planning examples, not universal industry standards. Actual ratios should be based on measured demand, service commitments, network topology, and acceptable congestion levels.

### 🖆 Factors Affecting Oversubscription

* Subscriber usage patterns.
* Package speeds.
* Peak-hour demand.
* Number of active subscribers.
* Traffic distribution.
* Local CDN availability.
* Upstream capacity.
* Service-level commitments.
* Acceptable performance targets.

Oversubscription should be managed using real traffic measurements rather than relying on a fixed ratio.

# 🗁 Upstream Bandwidth Planning

Upstream bandwidth planning determines how much Internet transit capacity an ISP needs from its upstream providers.

It is important to measure traffic separately for each upstream connection.

Example:

```text
                 Internet
                    |
           +--------+--------+
           |                 |
        Transit A         Transit B
         10 Gbps           10 Gbps
           |                 |
           +--------+--------+
                    |
                 ISP Core
                    |
                Subscribers
```

### 🖆 Calculate Required Upstream Capacity

Suppose an ISP measures the following peak traffic:

```text
Transit A Peak: 6 Gbps
Transit B Peak: 4 Gbps

Combined Peak Traffic: 10 Gbps
```

If the ISP targets a maximum planned utilization of 70% during normal peak operation, the required combined capacity for that measured demand is:

```text
Required Capacity =
Measured Peak Traffic / Target Utilization

Required Capacity =
10 Gbps / 0.70

Required Capacity ≈ 14.29 Gbps
```

The ISP would need to plan for approximately 14.29 Gbps of combined usable capacity to meet that utilization target under the same traffic distribution.

This calculation does not automatically establish the correct capacity for each individual link. Traffic distribution and failure scenarios must also be considered.

### 🖆 Download and Upload Capacity

ISPs should analyze download and upload traffic separately.

Residential networks often have asymmetric traffic patterns, with download traffic significantly higher than upload traffic.

Business services, cloud applications, backups, and hosting services may have different upload requirements.

Capacity planning should account for both directions on each full-duplex link.

# 🗁 IXP and Peering Bandwidth Planning

IXP capacity planning focuses on traffic exchanged with local and regional networks.

An ISP should monitor each IXP connection independently.

Example:

| IXP Connection | Port Capacity | Peak Traffic |
| --- | ---: | ---: |
| Local IXP A | 10 Gbps | 6 Gbps |
| Regional IXP B | 10 Gbps | 4 Gbps |
| CDN Peering | 10 Gbps | 7 Gbps |

Total peak traffic across these connections should not automatically be calculated by adding their individual peak values unless the measurements refer to the same time interval and direction.

### 🖆 Benefits of Peering for Capacity Planning

Peering can reduce the amount of traffic that must travel through upstream transit providers.

For example:

```text
Without Local Peering:

ISP → Transit Provider → Local Network

With Local Peering:

ISP → IXP → Local Network
```

Potential benefits include:

* Reduced transit utilization.
* Lower international bandwidth requirements.
* Improved local traffic performance.
* Better traffic distribution.

An ISP should measure the actual traffic transferred through peering connections to understand their value.

# 🗁 Core and Aggregation Capacity Planning

The core network transports traffic between edge routers, aggregation devices, services, and access networks.

Core capacity must support the combined traffic generated by all connected regions and services.

### 🖆 Core Network Planning

Consider an ISP with four aggregation regions:

| Region | Peak Traffic |
| --- | ---: |
| Region A | 3 Gbps |
| Region B | 4 Gbps |
| Region C | 2 Gbps |
| Region D | 3 Gbps |
| **Total** | **12 Gbps** |

If all four regions can generate their measured peak traffic simultaneously, the core network must be able to carry at least 12 Gbps in the relevant direction, before adding engineering headroom.

In practice, the ISP should examine actual synchronized traffic measurements rather than assuming all regional peaks occur at the same time.

### 🖆 OLT Uplink Planning

An Optical Line Terminal (OLT) connects multiple Passive Optical Network (PON) subscribers to the aggregation network.

An ISP must consider:

* Number of PON ports.
* Number of subscribers per PON port.
* PON technology.
* Optical split ratio.
* Subscriber speed packages.
* Busy-hour traffic.
* OLT uplink capacity.
* Redundancy requirements.

The optical line rate of a PON system is shared among subscribers on the same PON segment. Subscriber package speeds should not be interpreted as dedicated physical capacity on that segment.

### 🖆 Aggregation Link Planning

Aggregation links collect traffic from multiple access devices.

Example:

```text
             Core Router
                  |
              40 Gbps
                  |
          Aggregation Switch
           /       |       \
          /        |        \
      10 Gbps  10 Gbps   10 Gbps
       OLT 1    OLT 2     OLT 3
```

The aggregation link should be sized according to the combined demand and redundancy requirements of its connected access devices.

# 🗁 Peak Traffic and Busy-Hour Analysis

Peak traffic is the highest observed traffic level during a defined measurement period.

Busy-hour analysis identifies the period when network demand is consistently highest.

For residential ISPs, the busy hour commonly occurs during evening usage, but actual patterns vary by customer base, geography, and service type.

### 🖆 Why Busy-Hour Analysis Matters

Daily average traffic can hide congestion during high-demand periods.

Example:

| Time | Traffic |
| --- | ---: |
| 02:00 | 1 Gbps |
| 06:00 | 2 Gbps |
| 10:00 | 3 Gbps |
| 14:00 | 4 Gbps |
| 18:00 | 7 Gbps |
| 20:00 | 9 Gbps |
| 22:00 | 8 Gbps |

The network reaches its highest measured traffic at 20:00.

Capacity planning should pay particular attention to this period.

### 🖆 Measuring Peak Traffic

Useful measurement intervals include:

* 1-minute averages.
* 5-minute averages.
* 15-minute averages.
* Hourly averages.
* Daily peak values.
* Monthly peak values.

The selected interval matters. A short burst may be visible in a 1-minute measurement but disappear in a 15-minute average.

For capacity planning, retain both short-interval and longer-term measurements.

### 🖆 Traffic Percentiles

The 95th percentile is commonly used in some bandwidth billing models.

It is calculated by sorting traffic measurements from highest to lowest and excluding the highest 5% of samples.

The remaining highest sample represents the 95th-percentile traffic level.

For example, with 30 days of 5-minute samples:

```text
30 × 24 × 12 = 8,640 samples

Highest 5% excluded:
8,640 × 0.05 = 432 samples
```

After excluding the highest 432 samples, the next highest value is the 95th-percentile measurement.

Billing rules vary by provider, including measurement interval, direction, aggregation method, and rounding policy.

# 🗁 Bandwidth Forecasting

Bandwidth forecasting estimates future network capacity requirements using historical traffic data, subscriber growth, and expected service changes.

An ISP should forecast capacity for:

* Upstream transit.
* IXP connections.
* Core network links.
* Aggregation links.
* OLT uplinks.
* Business services.
* Internal infrastructure.

### 🖆 Traffic Growth Calculation

A simple compound growth model is:

```text
Future Traffic =
Current Traffic × (1 + Growth Rate)^Years
```

Example:

```text
Current Peak Traffic: 10 Gbps
Annual Growth Rate: 30%
Forecast Period: 3 Years

Future Traffic =
10 × (1 + 0.30)^3

Future Traffic =
21.97 Gbps
```

The ISP should plan for approximately 21.97 Gbps of peak demand after three years if the assumed growth rate continues.

This is a mathematical forecast, not a guarantee of future demand.

### 🖆 Subscriber Growth Forecast

An ISP should estimate future subscribers alongside traffic growth.

Example:

| Year | Subscribers |
| --- | ---: |
| Current | 5,000 |
| Year 1 | 6,000 |
| Year 2 | 7,200 |
| Year 3 | 8,640 |

If subscriber growth and per-subscriber usage both increase, total traffic may grow faster than subscriber count alone.

### 🖆 Forecasting Considerations

* Historical traffic growth.
* New subscriber connections.
* Changes in speed packages.
* Increased video streaming.
* Cloud service usage.
* New business customers.
* CDN deployment.
* Seasonal demand.
* Network expansion plans.
* New technologies and applications.

Forecasts should be reviewed regularly and adjusted using actual measurements.

# 🗁 Capacity Planning for Redundancy

Redundancy ensures that network connectivity can continue when a link, router, or provider fails.

Bandwidth planning must account for failure scenarios rather than considering only normal operation.

### 🖆 Active-Active Upstream Design

Suppose an ISP operates two 10 Gbps transit links.

```text
Transit A: 10 Gbps
Transit B: 10 Gbps

Normal Traffic:
Transit A: 6 Gbps
Transit B: 4 Gbps
```

If Transit A fails, its traffic may move to Transit B.

The remaining 10 Gbps link cannot carry the entire 10 Gbps demand while also maintaining utilization below 100%.

The ISP must consider additional capacity or a different traffic distribution strategy.

### 🖆 N+1 Capacity Planning

N+1 planning provides an additional capacity component so that the system can continue operating after one component fails.

For example:

```text
Required Capacity: 20 Gbps

Available Links:
Link A: 10 Gbps
Link B: 10 Gbps
Link C: 10 Gbps

Total Installed Capacity: 30 Gbps
```

Depending on the traffic distribution and failure design, the ISP may maintain 20 Gbps of capacity after one link fails.

The design must ensure that remaining links and routers can actually carry the traffic.

### 🖆 Failure Scenarios to Test

* Upstream provider failure.
* IXP connection failure.
* Core router failure.
* Aggregation switch failure.
* Fiber cut.
* Power failure.
* OLT uplink failure.
* Multiple simultaneous link failures where required by the availability objective.

# 🗁 QoS and Bandwidth Management

Quality of Service (QoS) helps manage traffic when network resources become constrained.

Bandwidth management allows an ISP to control how available capacity is distributed among subscribers and traffic classes.

Common techniques include:

| Technique | Purpose |
| --- | --- |
| Traffic shaping | Controls traffic rate by buffering excess packets |
| Traffic policing | Enforces rate limits by dropping or remarking excess traffic |
| Priority queuing | Gives selected traffic preferential treatment |
| Fair queuing | Distributes resources among traffic flows |
| PCQ | Distributes bandwidth across multiple users or flows |
| Hierarchical QoS | Applies policies at multiple network levels |

### 🖆 Subscriber Rate Limiting

An ISP can configure rate limits for each subscriber.

Example:

```text
Subscriber A:
Download: 50 Mbps
Upload:   20 Mbps

Subscriber B:
Download: 100 Mbps
Upload:   50 Mbps
```

The ISP can enforce these limits using its BNG, router, or subscriber management platform.

### 🖆 Aggregate Bandwidth Control

An ISP may also apply limits at higher network levels.

Example:

```text
ISP Core
   |
   +-- Region A: 5 Gbps
   |
   +-- Region B: 4 Gbps
   |
   +-- Region C: 3 Gbps
```

Aggregate controls should be designed carefully to avoid creating unnecessary bottlenecks.

Detailed QoS mechanisms are covered in the QoS and bandwidth documentation section.

# 🗁 Bandwidth Monitoring and Analysis

Monitoring provides the measurements required for effective capacity planning.

An ISP should collect interface statistics continuously and retain historical data.

### 🖆 SNMP Monitoring

Simple Network Management Protocol (SNMP) can collect:

* Interface throughput.
* Interface utilization.
* Interface errors.
* Interface discards.
* Device CPU usage.
* Memory utilization.
* Device availability.

SNMP monitoring platforms can generate traffic graphs and alert operators when utilization exceeds defined thresholds.

### 🖆 Traffic Flow Monitoring

Flow technologies provide more detailed visibility into traffic sources and destinations.

Common technologies include:

* NetFlow.
* IPFIX.
* sFlow.

They help identify:

* Top traffic sources.
* Top destinations.
* Protocol distribution.
* Application traffic patterns.
* Heavy traffic consumers.
* Traffic exchanged with specific networks.

Flow monitoring is particularly useful when an ISP needs to understand why bandwidth demand is increasing.

### 🖆 Interface Counters

Linux network interfaces can be inspected using:

```bash
ip -s link
```

For detailed interface statistics:

```bash
ethtool -S eth0
```

For live interface traffic:

```bash
sar -n DEV 1
```

These tools provide useful visibility on supported Linux systems.

# 🗁 Bandwidth Planning Tools

Several tools help network engineers calculate, monitor, and forecast bandwidth requirements.

| Tool | Purpose |
| --- | --- |
| LibreNMS | SNMP-based network monitoring |
| Zabbix | Infrastructure monitoring and alerting |
| Prometheus | Metrics collection and storage |
| Grafana | Monitoring dashboards and visualization |
| PRTG | Network monitoring |
| Cacti | Traffic graphing and SNMP monitoring |
| ntopng | Traffic analysis and visibility |
| NetFlow Analyzer | Flow-based traffic monitoring |
| MikroTik Torch | Real-time traffic inspection |
| MikroTik Traffic Flow | Flow export from RouterOS |

### 🖆 MikroTik Interface Monitoring

RouterOS can display real-time traffic statistics for an interface.

```routeros
/interface monitor-traffic ether1
```

Display interface details:

```routeros
/interface print stats
```

Inspect interface configuration:

```routeros
/interface print detail
```

### 🖆 Linux Bandwidth Monitoring

Install useful network utilities:

```bash
sudo apt update
sudo apt install iftop nload vnstat sysstat
```

Monitor traffic by connection:

```bash
sudo iftop -i eth0
```

Monitor interface utilization:

```bash
nload eth0
```

View historical traffic statistics:

```bash
vnstat -i eth0
```

# 🗁 Practical Bandwidth Planning Example

Consider an ISP with the following subscriber base:

```text
Total Subscribers: 5,000

Basic Package:
2,000 subscribers
20 Mbps

Standard Package:
2,000 subscribers
40 Mbps

Premium Package:
1,000 subscribers
60 Mbps
```

### 🖆 Calculate Aggregate Subscribed Bandwidth

Basic:

```text
2,000 × 20 Mbps = 40,000 Mbps
                     = 40 Gbps
```

Standard:

```text
2,000 × 40 Mbps = 80,000 Mbps
                     = 80 Gbps
```

Premium:

```text
1,000 × 60 Mbps = 60,000 Mbps
                     = 60 Gbps
```

Total:

```text
40 + 80 + 60 = 180 Gbps
```

The ISP has 180 Gbps of aggregate subscribed download bandwidth.

### 🖆 Estimate Busy-Hour Demand

Assume measurements show that peak-hour average consumption is:

```text
Basic Package:    2 Mbps per active subscriber
Standard Package: 4 Mbps per active subscriber
Premium Package:  6 Mbps per active subscriber
```

Assume 60% of subscribers in each package group are active during the measured busy hour.

Basic:

```text
2,000 × 0.60 × 2 Mbps
= 2,400 Mbps
= 2.4 Gbps
```

Standard:

```text
2,000 × 0.60 × 4 Mbps
= 4,800 Mbps
= 4.8 Gbps
```

Premium:

```text
1,000 × 0.60 × 6 Mbps
= 3,600 Mbps
= 3.6 Gbps
```

Estimated busy-hour demand:

```text
2.4 + 4.8 + 3.6 = 10.8 Gbps
```

### 🖆 Add Capacity Headroom

Assume the ISP targets 70% planned utilization.

```text
Required Capacity =
10.8 Gbps / 0.70

Required Capacity ≈ 15.43 Gbps
```

The ISP should plan for approximately 15.43 Gbps of usable capacity under these assumptions.

This is a simplified model. Actual capacity planning must account for measured peak traffic, download and upload direction, peering and transit distribution, protocol overhead, growth, and failure scenarios.

### 🖆 Example Capacity Plan

| Network Component | Current Capacity | Planned Capacity |
| --- | ---: | ---: |
| Transit A | 10 Gbps | 10 Gbps |
| Transit B | 10 Gbps | 10 Gbps |
| IXP | 10 Gbps | 20 Gbps |
| Core | 20 Gbps | 40 Gbps |
| Aggregation | 10 Gbps | 20 Gbps |
| Access uplinks | 10 Gbps | 10 Gbps or higher as required |

These values are illustrative. Individual links must be sized according to their own traffic measurements and redundancy requirements. Installed capacity should not be treated as automatically interchangeable across different network paths.

# 🗁 Bandwidth Planning Best Practices

* Measure actual traffic before purchasing additional capacity.
* Analyze busy-hour traffic rather than relying only on daily averages.
* Monitor download and upload utilization separately.
* Track each upstream and IXP connection independently.
* Maintain historical traffic records.
* Forecast subscriber and traffic growth.
* Define acceptable utilization and performance targets.
* Plan for failure scenarios and backup capacity.
* Consider physical path diversity when purchasing redundant connectivity.
* Monitor latency, jitter, packet loss, and interface errors.
* Use QoS policies to manage constrained network resources.
* Upgrade capacity before persistent congestion affects subscribers.
* Review bandwidth forecasts regularly.
* Maintain accurate capacity and topology documentation.
* Coordinate capacity upgrades with transit providers, IXPs, and infrastructure teams.

# 🗁 References

* [RFC 6349 — Framework for TCP Throughput Testing](https://datatracker.ietf.org/doc/html/rfc6349)
* [RFC 5136 — Defining Network Capacity](https://datatracker.ietf.org/doc/html/rfc5136)
* [RFC 7567 — Active Queue Management Recommendations](https://datatracker.ietf.org/doc/html/rfc7567)
* [RFC 8290 — The Flow Queue CoDel Packet Scheduler and Active Queue Management Algorithm](https://datatracker.ietf.org/doc/html/rfc8290)
* [RIPE NCC — Measurement and Analysis](https://www.ripe.net/)
* [MikroTik RouterOS — Queues](https://help.mikrotik.com/docs/)
