# Bandwidth vs Throughput

Bandwidth and throughput are two fundamental concepts in computer networking. Both describe data-transfer capacity, but they represent different measurements.

**Bandwidth** refers to the maximum data-transfer capacity of a network connection under specified conditions.

**Throughput** refers to the actual amount of data successfully transferred through a network over a period of time.

Understanding the difference is important when designing networks, evaluating internet connections, troubleshooting performance problems, and managing bandwidth using QoS.

# 🗁 What Is Bandwidth?

Bandwidth is the maximum data-transfer capacity of a communication channel.

It indicates how much data a network connection can theoretically or nominally carry per second.

Bandwidth is commonly measured in bits per second (bps).

Common bandwidth units include:

| Unit | Meaning |
| --- | --- |
| bps | Bits per second |
| Kbps | Kilobits per second |
| Mbps | Megabits per second |
| Gbps | Gigabits per second |
| Tbps | Terabits per second |

The relationship between these units is based on decimal multiples:

\[
1\text{ Kbps} = 1,000\text{ bps}
\]

\[
1\text{ Mbps} = 1,000\text{ Kbps}
\]

\[
1\text{ Gbps} = 1,000\text{ Mbps}
\]

For example, an internet connection advertised as 100 Mbps has a nominal download capacity of 100 megabits per second.

However, this does not mean that applications will always achieve 100 Mbps of useful data throughput.

# 🗁 What Is Throughput?

Throughput is the actual rate at which data is successfully transferred through a network.

It measures the amount of data delivered over a specific period.

Throughput is also commonly measured in bits per second.

For example:

* Configured connection bandwidth: 100 Mbps
* Actual measured throughput: 85 Mbps

In this situation, the network is delivering 85 Mbps of data throughput.

Throughput can vary because of:

* Network congestion.
* Protocol overhead.
* Packet loss and retransmissions.
* Hardware limitations.
* Wireless interference.
* Server performance.
* ISP network conditions.
* Traffic-management policies.

Throughput is therefore a practical measurement of network performance.

# 🗁 Bandwidth vs Throughput

The main difference is that bandwidth describes available or configured capacity, while throughput describes the actual achieved transfer rate.

| Feature | Bandwidth | Throughput |
| --- | --- | --- |
| Meaning | Maximum or configured data-transfer capacity | Actual successful data-transfer rate |
| Measurement | Bits per second | Bits per second |
| Nature | Capacity | Performance |
| Typical behavior | Relatively fixed for a configured link | Changes with network conditions |
| Example | 100 Mbps connection | 85 Mbps measured transfer |
| Influencing factors | Link technology and configuration | Congestion, overhead, loss, and device performance |

A useful way to understand the difference is to think of a road:

* **Bandwidth:** The maximum number of vehicles the road can accommodate per unit of time.
* **Throughput:** The actual number of vehicles successfully passing through per unit of time.

A wide road has high capacity, but traffic congestion can reduce the actual number of vehicles moving through it.

# 🗁 Bandwidth and Throughput Example

Consider a network with a 100 Mbps internet connection.

During a speed test, the measured download throughput is 80 Mbps.

The difference is:

\[
100 - 80 = 20\text{ Mbps}
\]

The connection has a nominal capacity of 100 Mbps, while the measured throughput is 80 Mbps.

The achieved throughput percentage is:

\[
\text{Throughput Efficiency} =
\frac{\text{Actual Throughput}}{\text{Bandwidth}}
\times 100
\]

Substituting the values:

\[
= \frac{80}{100} \times 100
\]

\[
= 80\%
\]

<text>In this example, the measured throughput is 80% of the nominal connection bandwidth.</text>

This percentage is a simplified comparison, not a universal measure of network efficiency. The result depends on what the speed test measures and how the connection's capacity is defined.

# 🗁 Bits vs Bytes

Networking bandwidth is normally measured in bits per second, while file sizes and many download applications display data in bytes.

One byte contains eight bits:

\[
1\text{ Byte} = 8\text{ bits}
\]

Therefore:

\[
\text{Bytes per second} =
\frac{\text{Bits per second}}{8}
\]

For example, a 100 Mbps connection has a theoretical maximum data rate of:

\[
\frac{100}{8} = 12.5\text{ MB/s}
\]

This means a 100 Mbps connection can theoretically transfer 12.5 megabytes per second before accounting for protocol overhead and other limitations.

Common unit distinctions:

| Unit | Meaning |
| --- | --- |
| b | Bit |
| B | Byte |
| Kb | Kilobit |
| KB | Kilobyte |
| Mb | Megabit |
| MB | Megabyte |
| Gbps | Gigabits per second |
| GB | Gigabytes |

Remember:

* `b` represents bits.
* `B` represents bytes.
* `Mbps` is not the same as `MB/s`.

# 🗁 Download Speed Calculation

You can estimate the time required to download a file using the following formula:

\[
\text{Download Time} =
\frac{\text{File Size in Bits}}{\text{Throughput in Bits per Second}}
\]

Consider downloading a 1 GB file over a 100 Mbps connection.

Assuming decimal units:

\[
1\text{ GB} = 1,000\text{ MB}
\]

\[
1\text{ GB} = 8,000\text{ Mb}
\]

The theoretical download time is:

\[
\frac{8,000\text{ Mb}}{100\text{ Mbps}} = 80\text{ seconds}
\]

Therefore, downloading a 1 GB file at a sustained throughput of 100 Mbps takes approximately 80 seconds.

Actual download time may be longer because of protocol overhead, server limitations, and variations in throughput.

# 🗁 Factors Affecting Throughput

Several factors can prevent a network connection from achieving its nominal bandwidth.

**Network Congestion**

When multiple users transmit data through the same network link, they compete for available capacity.

Congestion can increase packet queueing delays and cause packet drops.

**Protocol Overhead**

Networking protocols add headers and control information to transmitted data.

For example:

* Ethernet adds Layer 2 framing overhead.
* IP adds network-layer headers.
* TCP adds transport-layer headers and control information.
* TLS adds encryption-related record overhead.

Consequently, application data throughput is generally lower than the raw link capacity.

**Packet Loss**

Lost packets may require retransmission, reducing useful data throughput.

TCP connections can also reduce their sending rate when congestion or packet loss is detected.

**Latency**

High round-trip time can limit throughput, particularly for TCP connections with limited outstanding data.

**Hardware Limitations**

Routers, switches, network interfaces, CPUs, and storage devices can become bottlenecks.

**Wireless Interference**

Wireless networks share radio resources and are affected by interference, signal quality, channel utilization, and client capabilities.

**Server Limitations**

A remote server may not have enough processing capacity, storage performance, or outgoing bandwidth to deliver data at the maximum possible rate.

# 🗁 Bandwidth, Throughput, and Goodput

Goodput is another important networking measurement.

It represents the rate at which useful application-level data is successfully delivered, excluding protocol overhead and retransmitted data.

| Measurement | Description |
| --- | --- |
| Bandwidth | Maximum or configured link capacity |
| Throughput | Actual data-transfer rate measured at a defined network layer |
| Goodput | Useful application data delivered per second |

Consider the following simplified example:

* Link bandwidth: 100 Mbps
* Measured throughput: 90 Mbps
* Application goodput: 85 Mbps

The difference occurs because throughput can include protocol headers and retransmitted traffic, while goodput counts useful application data.

The exact values depend on the measurement point and methodology.

# 🗁 Bandwidth in Full-Duplex and Half-Duplex Links

Duplex describes whether a communication link can transmit and receive data simultaneously.

**Full-Duplex**

A full-duplex link can transmit and receive simultaneously.

For example, a 100 Mbps full-duplex Ethernet link can support:

* 100 Mbps transmission.
* 100 Mbps reception.

These are independent directions. It is not normally described as a single 200 Mbps connection in either direction.

**Half-Duplex**

A half-duplex link can transmit or receive at a given time, but not both simultaneously.

The devices must share the available transmission opportunity.

| Feature | Full-Duplex | Half-Duplex |
| --- | --- | --- |
| Simultaneous transmission and reception | Yes | No |
| Direction handling | Independent directions | Shared transmission opportunity |
| Collision behavior | No Ethernet collisions in normal full-duplex operation | Collisions can occur in shared Ethernet |
| Typical example | Modern switched Ethernet | Legacy shared Ethernet |

Modern switched Ethernet networks normally operate in full-duplex mode.

# 🗁 Measuring Bandwidth and Throughput

Different tools can be used to measure network capacity and actual transfer performance.

**Speedtest**

Internet speed-test services measure download throughput, upload throughput, and often latency.

**iPerf3**

iPerf3 is a network-performance testing tool that measures throughput between two endpoints.

Example server:

```bash
iperf3 -s
```

Example client:

```bash
iperf3 -c 192.168.1.10
```

Test reverse-direction throughput:

```bash
iperf3 -c 192.168.1.10 -R
```

Test simultaneous bidirectional traffic:

```bash
iperf3 -c 192.168.1.10 --bidir
```

Use multiple parallel streams:

```bash
iperf3 -c 192.168.1.10 -P 4
```

**Linux Network Interface Statistics**

Display interface statistics:

```bash
ip -s link
```

Display interface configuration:

```bash
ip link show
```

These commands help inspect interface counters and link information, but they do not independently measure end-to-end throughput.

**MikroTik RouterOS**

Monitor interface traffic:

```routeros
/interface monitor-traffic ether1
```

Display interface statistics:

```routeros
/interface print stats
```

Monitor active traffic through a specific interface:

```routeros
/tool torch interface=ether1
```

RouterOS monitoring tools help identify traffic rates and utilization on network interfaces.

# 🗁 Bandwidth Management and QoS

Bandwidth management controls how available capacity is distributed among users, applications, and services.

QoS applies traffic-management policies to influence how packets are handled during congestion.

For example, an ISP may have a 1 Gbps upstream connection and need to distribute bandwidth among multiple subscribers.

A bandwidth-management policy might define:

| Subscriber | Configured Download Limit |
| --- | ---: |
| Customer A | 20 Mbps |
| Customer B | 30 Mbps |
| Customer C | 50 Mbps |
| Customer D | 100 Mbps |

These configured limits control each subscriber's maximum permitted rate under the applicable queue policy.

The total of configured subscriber limits can exceed the ISP's upstream capacity when oversubscription is intentionally used.

However, the ISP must plan capacity carefully to maintain acceptable performance during peak usage.

# 🗁 Summary

Bandwidth represents the maximum or configured data-transfer capacity of a network connection, while throughput represents the actual transfer rate achieved.

Goodput measures the useful application data delivered per second.

Understanding these measurements is essential for network capacity planning, performance testing, troubleshooting, and QoS configuration.

A network connection with high bandwidth does not automatically guarantee high throughput. Actual performance depends on congestion, protocol overhead, packet loss, latency, hardware capabilities, and the conditions of the complete communication path.
