# Latency and Packet Loss

Latency and packet loss are two important indicators of network performance and reliability.

They help administrators understand how efficiently data travels between network endpoints and whether packets are successfully delivered.

Poor latency and packet loss can affect:

* Web application responsiveness.
* Voice over IP (VoIP).
* Video conferencing.
* Online gaming.
* Remote desktop sessions.
* File transfers.
* Cloud applications.
* VPN connections.

Monitoring these metrics helps identify congestion, routing problems, faulty links, wireless interference, and other network performance issues.

# 🗁 What Is Network Latency?

Network latency is the time required for data to travel from one point to another across a network.

It is commonly measured in milliseconds (ms).

For example, if a packet takes 20 milliseconds to travel from a client to a server, the one-way latency is 20 ms.

Latency is influenced by several factors:

* Physical distance between endpoints.
* Transmission medium.
* Network congestion.
* Routing path.
* Queueing delays.
* Router and switch processing.
* Firewall and security inspection.
* Wireless interference.

Lower latency generally means faster communication between endpoints.

# 🗁 Types of Network Delay

Network latency can be understood through four common delay components.

**Processing Delay**

The time required by a network device to process a packet.

Examples include:

* Examining packet headers.
* Performing routing lookups.
* Applying firewall rules.
* Processing network protocols.

**Queueing Delay**

The time a packet waits in a buffer before being transmitted.

Queueing delay increases when network traffic approaches or exceeds available link capacity.

**Transmission Delay**

The time required to place all packet bits onto a communication link.

It depends on packet size and link bandwidth.

\[
\text{Transmission Delay} =
\frac{\text{Packet Size (bits)}}{\text{Link Rate (bits/s)}}
\]

For example, transmitting a 1,500-byte packet over a 100 Mbps link:

\[
\frac{1500 \times 8}{100,000,000}
=0.00012\text{ seconds}
\]

This equals 0.12 milliseconds.

**Propagation Delay**

The time required for a signal to travel through the physical medium.

It depends on the distance and propagation speed of the signal.

\[
\text{Propagation Delay} =
\frac{\text{Distance}}{\text{Propagation Speed}}
\]

Propagation delay is particularly important for long-distance fiber, satellite, and undersea cable connections.

# 🗁 Round-Trip Time (RTT)

Round-trip time is the total time required for a packet to travel from a source to a destination and for a response to return.

RTT is commonly measured using ICMP Echo requests and replies.

Example:

```bash
ping -c 5 1.1.1.1
```

Example output:

```text
64 bytes from 1.1.1.1: icmp_seq=1 ttl=57 time=18.4 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=57 time=20.1 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=57 time=19.2 ms

--- 1.1.1.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss
rtt min/avg/max/mdev = 18.4/19.2/20.1/0.6 ms
```

The `time` value represents the measured RTT for each successful probe.

RTT is not the same as one-way latency. Dividing RTT by two only provides an approximation when forward and return paths have similar delays.

# 🗁 What Is Jitter?

Jitter refers to variation in packet delay over time.

For example, consider these measured packet delays:

```text
Packet 1: 20 ms
Packet 2: 21 ms
Packet 3: 45 ms
Packet 4: 22 ms
Packet 5: 38 ms
```

The delay is not consistent, even though the average may appear acceptable.

Jitter is particularly important for real-time communication because packets must arrive at predictable intervals.

High jitter can cause:

* Distorted VoIP audio.
* Video interruptions.
* Robotic voice quality.
* Unstable gaming performance.
* Buffering during live communication.

Jitter calculations vary by measurement method. Monitoring systems should document whether they report average absolute delay variation, inter-packet delay variation, or another metric.

# 🗁 What Is Packet Loss?

Packet loss occurs when transmitted packets fail to reach their intended destination.

For example, if a sender transmits 100 packets but only 95 responses are received:

\[
\text{Packet Loss} =
\frac{\text{Lost Packets}}{\text{Sent Packets}}
\times100
\]

\[
\frac{100-95}{100}\times100=5\%
\]

The measured packet loss is 5%.

Packet loss can occur because of:

* Network congestion.
* Interface errors.
* Faulty cables or connectors.
* Wireless interference.
* Queue overflow.
* Routing problems.
* Hardware failures.
* Firewall filtering.
* Policing and rate limiting.

Packet loss can significantly affect TCP throughput because TCP may retransmit lost segments and adjust its sending rate.

Real-time UDP applications may experience missing audio, video artifacts, or interruptions.

# 🗁 Understanding Latency, Jitter, and Packet Loss

These metrics describe different aspects of network performance.

| Metric | Meaning | Common impact |
| --- | --- | --- |
| Latency | Time required for data delivery | Slow application response |
| RTT | Round-trip communication time | Delayed request-response operations |
| Jitter | Variation in packet delay | Unstable real-time communication |
| Packet loss | Percentage of packets not successfully delivered | Retransmissions and missing data |
| Throughput | Actual successful data-transfer rate | Reduced transfer performance |

A network can have low latency but still experience packet loss.

Likewise, a network can have zero packet loss but high latency.

All relevant metrics should be considered together when evaluating network quality.

# 🗁 Factors Affecting Latency and Packet Loss

**Network Congestion**

Congestion occurs when traffic demand exceeds the available forwarding or transmission capacity.

Possible symptoms:

* Increasing RTT.
* Packet drops.
* Queue buildup.
* Reduced throughput.
* Increased jitter.

**Faulty Physical Connections**

Damaged cables, connectors, optical modules, or fiber connections can cause transmission errors.

Possible symptoms:

* Interface errors.
* CRC errors.
* Link flapping.
* Packet drops.
* Reduced link speed.

**Wireless Interference**

Wireless networks share radio-frequency spectrum with other devices and networks.

Interference may cause:

* Retransmissions.
* Reduced throughput.
* Variable latency.
* Increased packet loss.

**Routing Problems**

Incorrect or inefficient routing can introduce additional network hops or unstable paths.

Examples:

* Routing loops.
* Asymmetric paths.
* Suboptimal routing.
* Frequent route changes.
* Incorrect default gateways.

**Firewall and QoS Policies**

Firewalls, traffic policing, queue management, and QoS policies can affect packet forwarding and delay.

Incorrect configurations may cause unexpected packet drops or prioritization problems.

# 🗁 Measuring Latency with Ping

`ping` is a basic network diagnostic utility that sends ICMP Echo requests and measures response times.

**Test gateway latency**

```bash
ping -c 10 192.168.1.1
```

**Test external connectivity**

```bash
ping -c 10 1.1.1.1
```

**Continuously monitor a destination**

```bash
ping 1.1.1.1
```

Stop the continuous test using:

```text
Ctrl + C
```

**Set the packet interval**

```bash
ping -i 0.5 -c 20 1.1.1.1
```

This sends 20 probes at approximately half-second intervals.

**Use larger packets**

```bash
ping -s 1400 -c 10 1.1.1.1
```

The `-s` option specifies the ICMP payload size in bytes. The total IP packet size also includes IP and ICMP headers.

Ping is useful for initial testing, but ICMP traffic may be filtered, rate-limited, or deprioritized. A failed ping does not always mean that the destination's application services are unavailable.

# 🗁 Measuring Packet Loss

Ping reports packet transmission and reception statistics.

Example:

```text
20 packets transmitted, 18 received, 10% packet loss
```

This means two probes did not receive replies.

Packet loss can be measured over a specific interval to identify intermittent problems.

Example:

```bash
ping -c 100 192.168.1.1
```

For longer monitoring sessions:

```bash
ping -D 1.1.1.1 | tee ping-log.txt
```

The `-D` option adds timestamps to output on supported Linux implementations.

Analyze the results over time rather than relying on a single test.

# 🗁 Measuring Latency with traceroute

`traceroute` identifies the network hops between a source and destination.

It uses packets with progressively increasing TTL values to discover intermediate routers.

Install it on Ubuntu:

```bash
sudo apt update
sudo apt install traceroute
```

Run a trace:

```bash
traceroute 1.1.1.1
```

Example output:

```text
traceroute to 1.1.1.1

1  192.168.1.1       1.2 ms
2  10.10.0.1         8.5 ms
3  203.0.113.1      12.3 ms
4  1.1.1.1          19.2 ms
```

Each row represents a discovered hop.

The displayed times are measurements from the source to the responding hop and back, not direct measurements of the one-way delay between consecutive routers.

Some routers do not respond to traceroute probes, so an asterisk does not necessarily indicate packet forwarding failure.

# 🗁 Using mtr for Continuous Path Monitoring

`mtr` combines ping and traceroute functionality.

It continuously measures response times and packet loss across network hops.

Install it:

```bash
sudo apt update
sudo apt install mtr
```

Run an interactive test:

```bash
mtr 1.1.1.1
```

Generate a report:

```bash
mtr --report --report-cycles 100 1.1.1.1
```

Example report:

```text
HOST: workstation

Host             Loss%   Snt   Last   Avg   Best   Wrst
192.168.1.1       0.0%   100    1.2   1.4    0.9    3.2
10.10.0.1         0.0%   100    8.1   8.5    7.9   12.0
203.0.113.1       1.0%   100   12.4  12.8   11.9   19.0
1.1.1.1           0.0%   100   19.1  19.4   18.2   25.0
```

Important columns:

* `Loss%`: Percentage of probes without a response from that hop.
* `Snt`: Number of probes sent.
* `Last`: Most recent response time.
* `Avg`: Average response time.
* `Best`: Lowest response time.
* `Wrst`: Highest response time.

Loss reported at an intermediate hop does not necessarily indicate forwarded traffic is being dropped. Routers may rate-limit or deprioritize control-plane responses.

Loss that continues through subsequent hops and reaches the final destination is stronger evidence of an end-to-end problem.

# 🗁 Path MTU and Fragmentation Testing

MTU (Maximum Transmission Unit) defines the maximum packet size that can be transmitted over a link without fragmentation at that layer.

Incorrect MTU configuration can cause connectivity and performance problems.

On Linux, the `-M do` option can be used with ping to request that IPv4 packets not be fragmented.

Example:

```bash
ping -M do -s 1472 -c 4 1.1.1.1
```

For a standard 1,500-byte IPv4 MTU:

* ICMP payload: 1,472 bytes.
* ICMP header: 8 bytes.
* IPv4 header: 20 bytes.
* Total IP packet: 1,500 bytes.

If the path cannot carry the packet without fragmentation, the test may return an MTU-related error or fail.

Path MTU discovery behavior depends on the network, IP version, and firewall configuration.

# 🗁 Measuring TCP and Application Latency

ICMP measurements alone do not always represent actual application performance.

Application-specific tests provide more useful visibility into service availability and response time.

**Test TCP connectivity**

Install netcat if necessary:

```bash
sudo apt install netcat-openbsd
```

Check HTTPS connectivity:

```bash
nc -vz google.com 443
```

**Measure HTTP response timing**

```bash
curl -o /dev/null -s -w \
"DNS: %{time_namelookup}s\nConnect: %{time_connect}s\nTLS: %{time_appconnect}s\nFirst Byte: %{time_starttransfer}s\nTotal: %{time_total}s\n" \
https://example.com
```

This separates several timing components:

* DNS resolution.
* TCP connection establishment.
* TLS handshake completion.
* Time to first response byte.
* Total request duration.

These measurements help identify whether delays originate from DNS, connection establishment, TLS negotiation, server processing, or content transfer.

# 🗁 Latency Monitoring with Smokeping

Smokeping is an open-source latency and packet-loss monitoring tool.

It periodically probes configured destinations and generates historical graphs.

It can help monitor:

* WAN gateway latency.
* ISP connectivity.
* DNS server responsiveness.
* Remote office links.
* Data center connectivity.
* VPN endpoints.

Its historical graphs help identify:

* Increasing latency.
* Intermittent packet loss.
* Daily congestion patterns.
* Network instability.
* Long-term performance changes.

A typical deployment includes a Smokeping server, configured probe targets, and a web interface for viewing historical measurements.

# 🗁 Monitoring Latency and Packet Loss with MikroTik RouterOS v7

RouterOS provides several tools for testing network performance.

**Ping**

Test a destination:

```routeros
/ping 1.1.1.1 count=20
```

Test a gateway:

```routeros
/ping 192.168.1.1 count=20
```

**Traceroute**

```routeros
/tool traceroute 1.1.1.1
```

**Netwatch**

Netwatch can monitor the availability of remote hosts and execute configured actions when their status changes.

Example:

```routeros
/tool netwatch add \
    host=1.1.1.1 \
    type=icmp \
    interval=10s \
    timeout=2s \
    comment="Monitor external connectivity"
```

Inspect configured entries:

```routeros
/tool netwatch print
```

Netwatch is useful for monitoring reachability and triggering operational actions. It should not be treated as a complete latency and packet-loss analytics platform.

# 🗁 Interpreting Network Performance Results

Consider these example results:

| Destination | Average RTT | Packet loss | Interpretation |
| --- | ---: | ---: | --- |
| Local gateway | 1 ms | 0% | Healthy local response |
| ISP gateway | 8 ms | 0% | Stable upstream response |
| External DNS | 25 ms | 0% | Normal response in this example |
| Remote server | 180 ms | 0% | High latency, but no measured loss |
| Remote endpoint | 45 ms | 8% | Packet delivery problem |

These values are illustrative. Acceptable performance depends on application requirements and network topology.

A useful troubleshooting approach is to test multiple destinations along the path:

* Local gateway.
* ISP next hop.
* Public IP address.
* DNS server.
* Actual application endpoint.

This helps narrow down the location of a problem.

# 🗁 Troubleshooting Latency and Packet Loss

| Symptom | Possible cause | Investigation |
| --- | --- | --- |
| High latency to gateway | Local congestion or device issue | Check LAN, Wi-Fi, and router load |
| High latency beyond ISP gateway | Upstream congestion or routing | Compare multiple external destinations |
| Intermittent packet loss | Congestion, interference, or physical errors | Inspect interface counters and wireless metrics |
| Loss only on Wi-Fi | Weak signal or interference | Check signal strength and channel utilization |
| High latency during peak hours | Link saturation | Review bandwidth utilization and queues |
| TCP application slow but ping normal | Server, TLS, or application delay | Measure TCP and HTTP timing |
| Large packets fail | MTU or PMTU problem | Perform DF packet-size tests |
| Loss at one intermediate hop only | ICMP rate limiting | Compare downstream and destination results |

Useful Linux commands:

```bash
ip -s link
```

```bash
ip route
```

```bash
ss -s
```

```bash
mtr --report --report-cycles 100 1.1.1.1
```

Useful MikroTik commands:

```routeros
/interface print stats
```

```routeros
/interface monitor-traffic ether1
```

```routeros
/tool torch interface=ether1
```

# 🗁 Best Practices for Latency and Packet Loss Monitoring

* Monitor important destinations continuously.
* Measure both local and remote network performance.
* Establish normal latency and loss baselines.
* Use application-level probes in addition to ICMP.
* Monitor jitter for real-time communication services.
* Investigate interface errors and packet drops.
* Correlate performance changes with bandwidth utilization.
* Monitor different times of day to identify congestion patterns.
* Use multiple monitoring locations when possible.
* Distinguish actual forwarding loss from ICMP response filtering.
* Track historical performance rather than relying on individual tests.
* Configure alerts based on service requirements and sustained degradation.
* Synchronize monitoring systems and devices using NTP.

# 🗁 Summary

Latency, jitter, and packet loss are fundamental indicators of network quality.

Latency measures communication delay, jitter measures delay variation, and packet loss measures unsuccessful packet delivery.

Tools such as ping, traceroute, mtr, Smokeping, and MikroTik Netwatch help administrators identify and investigate network performance problems.

Combining these measurements with interface statistics, traffic analysis, and application-level monitoring provides a more complete understanding of network health.
