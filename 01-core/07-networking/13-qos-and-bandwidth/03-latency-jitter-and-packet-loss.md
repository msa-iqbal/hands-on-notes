# Latency, Jitter, and Packet Loss

Latency, jitter, and packet loss are three important network performance parameters that directly affect communication quality and application responsiveness.

They are particularly important for real-time applications such as:

* Voice over IP (VoIP).
* Video conferencing.
* Online gaming.
* Live streaming.
* Remote desktop connections.
* Interactive cloud applications.

A network connection may have sufficient bandwidth but still deliver poor performance because of excessive latency, unstable packet delivery, or packet loss.

Understanding these parameters is essential for network monitoring, troubleshooting, and Quality of Service (QoS) implementation.

# 🗁 What Is Latency?

Latency is the time required for data to travel from a source to a destination.

It represents the delay experienced by packets while moving through a network.

Latency is commonly measured in milliseconds (ms).

For example:

* A device sends a packet to a remote server.
* The packet travels through multiple network devices.
* The server receives the packet.
* The response travels back to the original device.

The time required for this complete exchange is called Round-Trip Time (RTT).

# 🗁 One-Way Latency vs Round-Trip Time

There are two common ways to describe network delay.

**One-Way Latency**

One-way latency measures the time required for a packet to travel from the source to the destination.

\[
\text{One-Way Latency} =
\text{Destination Arrival Time} -
\text{Source Transmission Time}
\]

Accurate one-way measurements require synchronized clocks at both endpoints.

**Round-Trip Time (RTT)**

RTT measures the time required for a packet to travel from the source to the destination and for a response to return.

\[
RTT = T_{\text{response received}} - T_{\text{request sent}}
\]

For example:

If a device sends a ping request and receives a response after 40 milliseconds:

\[
RTT = 40\text{ ms}
\]

The measured RTT is 40 ms.

RTT is commonly used by tools such as `ping`.

# 🗁 Types of Network Latency

Network latency consists of several delay components.

| Latency Type | Description |
| --- | --- |
| Processing Delay | Time required for a network device to examine and process a packet |
| Queuing Delay | Time a packet waits in a queue before transmission |
| Transmission Delay | Time required to place all packet bits onto a communication link |
| Propagation Delay | Time required for the signal to travel through the physical medium |

The total one-way delay can be represented as:

\[
D_{\text{total}} =
D_{\text{processing}} +
D_{\text{queuing}} +
D_{\text{transmission}} +
D_{\text{propagation}}
\]

In a multi-hop network, these components accumulate across the communication path.

# 🗁 Factors Affecting Latency

Several conditions can increase network latency.

**Physical Distance**

Packets traveling across longer distances generally experience greater propagation delay.

For example, communication between two devices in the same building normally has lower propagation delay than communication between continents.

**Network Congestion**

When network traffic exceeds available transmission capacity, packets wait in queues.

This increases queuing delay.

**Network Device Processing**

Routers, firewalls, switches, and other devices require time to process packets.

Heavy processing workloads can increase packet-processing delay.

**Transmission Medium**

Different physical technologies have different transmission characteristics.

Fiber-optic, copper Ethernet, satellite, and wireless connections can have different latency profiles.

**Routing Path**

An inefficient routing path can introduce unnecessary network hops and increase total delay.

**Wireless Interference**

Interference and retransmissions in wireless networks can increase packet delivery time.

# 🗁 What Is Jitter?

Jitter is the variation in packet delay over time.

It describes how consistently packets arrive at their destination.

A network connection may have an average latency of 30 ms, but individual packets might arrive with delays of:

* 25 ms
* 32 ms
* 28 ms
* 45 ms
* 35 ms

The variation between packet delays represents jitter.

Jitter is particularly important for real-time communication because audio and video applications depend on packets arriving at predictable intervals.

# 🗁 Jitter Example

Consider a VoIP application sending packets at regular intervals.

The expected packet arrival times are:

| Packet | Expected Arrival | Actual Arrival |
| --- | ---: | ---: |
| Packet 1 | 20 ms | 20 ms |
| Packet 2 | 40 ms | 42 ms |
| Packet 3 | 60 ms | 65 ms |
| Packet 4 | 80 ms | 75 ms |
| Packet 5 | 100 ms | 110 ms |

The actual arrival times are not perfectly consistent.

The receiving application may need to buffer packets to compensate for these variations.

If jitter becomes excessive, users may experience:

* Distorted audio.
* Choppy conversations.
* Audio gaps.
* Video interruptions.
* Delayed playback.

# 🗁 Measuring Jitter

One simplified way to estimate jitter is to calculate the average absolute difference between consecutive packet delays.

\[
J =
\frac{1}{N-1}
\sum_{i=2}^{N}
|D_i-D_{i-1}|
\]

Where:

* \(J\) = Average absolute delay variation.
* \(D_i\) = Delay measurement for packet \(i\).
* \(N\) = Total number of packet-delay measurements.

Consider these packet delays:

\[
20,\ 25,\ 22,\ 30,\ 28\text{ ms}
\]

The absolute differences are:

\[
5,\ 3,\ 8,\ 2\text{ ms}
\]

Therefore:

\[
J = \frac{5+3+8+2}{4}
\]

\[
J = 4.5\text{ ms}
\]

The average absolute difference between consecutive delay measurements is 4.5 ms.

This is a simplified jitter calculation. Different monitoring tools and real-time protocols may use other jitter definitions and algorithms.

# 🗁 What Is Packet Loss?

Packet loss occurs when transmitted network packets fail to reach their intended destination.

Packets may be dropped because of:

* Network congestion.
* Full packet queues.
* Physical link problems.
* Wireless interference.
* Faulty network hardware.
* Incorrect network configurations.
* Routing problems.
* Firewall filtering.

Packet loss is commonly expressed as a percentage.

# 🗁 Packet Loss Calculation

Packet loss percentage can be calculated using:

\[
\text{Packet Loss} =
\frac{\text{Packets Sent} - \text{Packets Received}}
{\text{Packets Sent}}
\times 100
\]

For example:

* Packets sent: 1,000
* Packets received: 980
* Packets lost: 20

Calculation:

\[
\text{Packet Loss} =
\frac{1000-980}{1000}
\times 100
\]

\[
= 2\%
\]

The packet loss rate is 2%.

# 🗁 Effects of Packet Loss

The impact of packet loss depends on the application and transport protocol.

**TCP Applications**

TCP detects missing data and uses retransmission mechanisms to recover lost segments.

This can increase transfer time and reduce throughput.

**UDP Applications**

UDP does not provide built-in retransmission or guaranteed delivery.

Lost UDP packets may result in missing audio, video artifacts, or incomplete application data unless the application implements its own recovery mechanisms.

**Real-Time Applications**

Voice and video applications may be particularly sensitive to packet loss because retransmitting delayed packets may not be useful after their playback deadline.

# 🗁 Latency vs Jitter vs Packet Loss

These parameters describe different network-performance problems.

| Parameter | Meaning | Common Impact | Unit |
| --- | --- | --- | --- |
| Latency | Packet delivery delay | Slow response | Milliseconds |
| Jitter | Variation in packet delay | Unstable audio/video | Milliseconds |
| Packet Loss | Packets that fail to arrive | Missing or retransmitted data | Percentage |

Example:

A network connection may have:

* Latency: 40 ms.
* Jitter: 5 ms.
* Packet loss: 1%.

These measurements describe three different aspects of the same network connection.

Low latency does not automatically mean low jitter or zero packet loss.

# 🗁 Typical Performance Targets

The acceptable values depend on application requirements, network architecture, and service-level agreements.

The following table provides general operational reference points rather than universal guarantees.

| Parameter | General Target | Possible Concern |
| --- | --- | --- |
| Latency | Lower is generally better | Excessive delay affects responsiveness |
| Jitter | Low and consistent | Variable packet arrival affects real-time media |
| Packet Loss | As close to 0% as practical | Packet drops affect reliability and media quality |

For voice services, ITU-T Recommendation G.114 commonly identifies 150 ms one-way delay as a planning guideline for most general voice applications. Some interactive applications require substantially lower delay.

# 🗁 Bufferbloat

Bufferbloat occurs when excessive buffering in network devices causes high latency during periods of congestion.

For example:

* Idle network latency: 20 ms.
* Latency during a large download: 250 ms.
* Latency during upload congestion: 400 ms.

The network may still provide high throughput, but interactive applications become slow and unresponsive.

Bufferbloat is commonly associated with oversized packet queues that allow too much traffic to wait before transmission.

Active Queue Management (AQM) techniques such as CoDel and FQ-CoDel can help control excessive queueing delay.

Traffic shaping can also help when configured at a rate slightly below the actual bottleneck capacity, allowing the managed device to control the queue instead of allowing congestion to build upstream.

# 🗁 Monitoring Latency, Jitter, and Packet Loss

Network administrators use different tools to monitor network quality.

**Ping**

`ping` measures RTT and reports packet responses or timeouts.

Linux example:

```bash
ping -c 10 8.8.8.8
```

Continuous ping:

```bash
ping 8.8.8.8
```

The output provides information about:

* Response time.
* Minimum RTT.
* Average RTT.
* Maximum RTT.
* Packet loss.

**Traceroute**

Traceroute helps identify the network path toward a destination.

Linux example:

```bash
traceroute 8.8.8.8
```

Alternative using ICMP probes:

```bash
traceroute -I 8.8.8.8
```

A similar tool is available on many systems:

```bash
tracepath 8.8.8.8
```

Traceroute results can help identify where delay or packet loss may occur, but individual routers may rate-limit or deprioritize diagnostic responses.

**MTR**

MTR combines continuous ping measurements with route information.

Example:

```bash
mtr -rw 8.8.8.8
```

MTR is useful for observing packet loss and latency across multiple network hops.

Loss shown at an intermediate hop does not necessarily mean that the router is dropping forwarded traffic. Compare later hops and the final destination before identifying a problem.

# 🗁 Monitoring in MikroTik RouterOS

MikroTik RouterOS provides several tools for network monitoring and troubleshooting.

**Ping**

```routeros
/ping 8.8.8.8 count=10
```

Continuous ping:

```routeros
/ping 8.8.8.8
```

**Traceroute**

```routeros
/tool traceroute 8.8.8.8
```

**Interface Statistics**

```routeros
/interface print stats
```

**Interface Traffic Monitoring**

```routeros
/interface monitor-traffic ether1
```

**Torch**

```routeros
/tool torch interface=ether1
```

These tools help identify packet loss symptoms, traffic utilization, routing problems, and interface-level issues.

# 🗁 Troubleshooting Guidelines

When investigating network latency, jitter, or packet loss, follow a structured troubleshooting process.

**Check Physical Connectivity**

* Inspect Ethernet cables and connectors.
* Check fiber-optic connections.
* Review wireless signal quality.
* Verify interface link status.

**Check Interface Statistics**

Look for:

* RX and TX errors.
* Dropped packets.
* Interface resets.
* CRC errors where reported.
* Excessive traffic utilization.

**Check Network Congestion**

Identify whether network interfaces are operating near their available capacity.

Examine queue utilization and traffic distribution.

**Check Routing**

Use traceroute or MTR to investigate the path toward the destination.

Compare measurements from different network locations.

**Check Packet Loss**

Run repeated ping tests to multiple destinations.

Determine whether loss occurs locally, across an upstream connection, or only toward a particular destination.

**Check Wireless Conditions**

Inspect signal strength, interference, channel utilization, and retransmission statistics.

**Check Queue Configuration**

Review traffic shaping, queue limits, priority settings, and bandwidth allocation policies.

# 🗁 Summary

Latency measures packet delivery delay, jitter measures variation in packet delay, and packet loss measures packets that fail to reach their destination.

All three parameters are important for evaluating network quality.

High latency affects responsiveness, excessive jitter affects real-time communication, and packet loss reduces communication reliability.

Understanding these measurements helps network administrators identify performance problems and design effective QoS policies.
