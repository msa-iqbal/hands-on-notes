# Bandwidth Testing

Bandwidth testing is the process of measuring the data transfer capacity and performance of a network connection.

In MikroTik RouterOS, bandwidth testing helps administrators evaluate throughput, identify bottlenecks, verify link performance, and troubleshoot connectivity between network devices.

RouterOS provides built-in tools such as Bandwidth Test, interface monitoring, traffic monitoring, and other diagnostic utilities.

# 🗁 Understanding Bandwidth and Throughput

Bandwidth and throughput are related but different concepts.

* **Bandwidth:** The maximum theoretical data transfer capacity of a network connection.
* **Throughput:** The actual amount of data successfully transferred over a period.
* **Goodput:** The useful application data delivered, excluding protocol overhead and retransmissions.
* **Latency:** The time required for data to travel between two endpoints.
* **Jitter:** Variation in packet delay.
* **Packet loss:** The percentage of packets that fail to reach their destination.

For example, a connection with a capacity of 100 Mbps may deliver an actual throughput of 85 Mbps because of protocol overhead, congestion, hardware limitations, or other factors.

# 🗁 Understanding RouterOS Bandwidth Test

RouterOS includes a built-in Bandwidth Test tool that can measure traffic between MikroTik devices.

The test requires a Bandwidth Test server on the receiving device.

The general architecture is:

```text
       MikroTik Router A
       Bandwidth Test Client
                |
                |
          Network Link
                |
                |
       MikroTik Router B
       Bandwidth Test Server
```

The test can measure:

* Transmit throughput.
* Receive throughput.
* Bidirectional throughput.
* TCP performance.
* UDP performance.
* Packet loss during UDP testing.

Bandwidth testing consumes network and router resources. Run tests carefully on production networks.

# 🗁 Enabling the Bandwidth Test Server

On the receiving MikroTik router, enable the Bandwidth Test server:

```routeros
/tool/bandwidth-server/set enabled=yes
```

By default, the server uses TCP and UDP port `2000`.

Check the configuration:

```routeros
/tool/bandwidth-server/print
```

For better security, restrict access to trusted test clients:

```routeros
/tool/bandwidth-server/set enabled=yes authenticate=yes \
    allowed-addresses=192.168.10.0/24
```

Configure authentication credentials on the client and use an appropriate trusted source address.

Do not expose the Bandwidth Test server to the public Internet without strict access controls.

# 🗁 Running a TCP Bandwidth Test

TCP testing measures throughput using TCP connections.

Assume:

| Device | IP Address | Role |
| --- | --- | --- |
| Router A | `192.168.10.1` | Test client |
| Router B | `192.168.10.2` | Test server |

Run the test from Router A:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp \
    direction=both user=test password=StrongPassword
```

The test displays transmit and receive throughput.

You can also test one direction at a time.

Transmit traffic:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp \
    direction=transmit user=test password=StrongPassword
```

Receive traffic:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp \
    direction=receive user=test password=StrongPassword
```

TCP performance depends on factors such as:

* TCP window size.
* Round-trip latency.
* Packet loss.
* CPU performance.
* Number of parallel connections.
* Network congestion.

A TCP bandwidth test is useful for evaluating practical transfer performance, but it does not isolate every possible network bottleneck.

# 🗁 Running a UDP Bandwidth Test

UDP testing measures throughput without TCP congestion control.

Example:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=udp \
    direction=both local-tx-speed=50M remote-tx-speed=50M \
    user=test password=StrongPassword
```

The speed settings define the requested transmit rates for the respective test directions.

UDP testing can help evaluate:

* Maximum offered traffic rate.
* Packet loss under load.
* Link capacity.
* Performance under high packet rates.

Unlike TCP, UDP does not automatically reduce its sending rate in response to congestion.

If the requested UDP rate exceeds the network's capacity, packet loss may increase significantly.

Start with a conservative rate and increase it gradually.

# 🗁 Testing Different Traffic Directions

The `direction` parameter controls the traffic direction.

| Direction | Description |
| --- | --- |
| `transmit` | Tests traffic transmitted by the local test client |
| `receive` | Tests traffic received by the local test client |
| `both` | Tests traffic in both directions |

For example:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp direction=transmit
```

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp direction=receive
```

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp direction=both
```

When testing bidirectional throughput, remember that both directions share router and network resources.

# 🗁 Configuring Authentication

The Bandwidth Test server can require authentication.

Enable authentication:

```routeros
/tool/bandwidth-server/set authenticate=yes
```

Set the client credentials when running a test:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp \
    user=test password=StrongPassword
```

If authentication is enabled, the client must provide valid credentials.

Use a dedicated test account where appropriate, and restrict the server to trusted source addresses.

# 🗁 Restricting Bandwidth Test Access

Bandwidth Test should only be available to authorized devices.

Configure the server to accept connections from a trusted subnet:

```routeros
/tool/bandwidth-server/set allowed-addresses=192.168.10.0/24
```

For a single test client:

```routeros
/tool/bandwidth-server/set allowed-addresses=192.168.10.10/32
```

Review the server configuration:

```routeros
/tool/bandwidth-server/print
```

Also consider firewall rules when the test server is reachable through routed or external networks.

The allowed-address setting and firewall policy should work together to restrict access.

# 🗁 Monitoring Interface Traffic

Interface monitoring helps determine whether a network interface is transmitting or receiving traffic at the expected rate.

Display interface statistics:

```routeros
/interface/print stats
```

Monitor a specific interface:

```routeros
/interface/monitor-traffic ether1
```

Monitor multiple interfaces:

```routeros
/interface/monitor-traffic [find]
```

Typical statistics include:

* Receive bits per second.
* Transmit bits per second.
* Receive packets per second.
* Transmit packets per second.
* Receive drops.
* Transmit drops.

Interface traffic monitoring is useful for identifying whether a link is saturated during a bandwidth test.

# 🗁 Checking Interface Link Speed

The negotiated Ethernet link speed can limit available throughput.

Inspect interface details:

```routeros
/interface/ethernet/monitor ether1 once
```

Check properties such as:

* Link status.
* Negotiated rate.
* Duplex mode.
* Auto-negotiation status.
* Link partner information, when available.

For example, a link negotiated at 100 Mbps cannot deliver 1 Gbps throughput.

A speed mismatch, damaged cable, or faulty transceiver may cause unexpected performance limitations.

# 🗁 Monitoring CPU During Bandwidth Tests

Router CPU utilization can become a bottleneck during bandwidth testing.

Monitor CPU usage:

```routeros
/system/resource/monitor
```

Display system resources:

```routeros
/system/resource/print
```

Inspect individual CPU usage:

```routeros
/tool/profile
```

If CPU usage reaches its limit during a test, the router may be unable to process additional traffic.

Potential causes include:

* Insufficient CPU capacity.
* Software-based packet processing.
* Firewall processing overhead.
* Encryption overhead.
* Complex routing or packet-marking rules.
* High packet-per-second traffic.

A bandwidth test performed through the router itself may measure the router's processing capability rather than the full capacity of the physical link.

# 🗁 Understanding Bandwidth Test Limitations

RouterOS Bandwidth Test is useful, but its results must be interpreted carefully.

**CPU limitations**

Bandwidth testing can consume significant CPU resources on both MikroTik devices.

A router may reach its processing limit before the physical link reaches its maximum capacity.

**Traffic path**

The test measures the path between the test endpoints. It does not necessarily represent the performance of every client connected to the network.

**Firewall and FastTrack**

Firewall processing, connection tracking, and FastTrack behavior can influence the traffic path and measured results.

**TCP behavior**

TCP throughput is influenced by latency, congestion control, packet loss, and connection characteristics.

**UDP behavior**

UDP tests can generate high traffic rates without TCP congestion control. Excessive test rates can cause packet loss and disrupt other services.

**Production traffic**

Running high-load tests on a production network can affect customers and real-time applications.

# 🗁 Testing Throughput with External Tools

For more controlled performance testing, use dedicated tools such as `iperf3`.

A Linux server can run an `iperf3` server:

```bash
iperf3 -s
```

From another Linux device, run a TCP test:

```bash
iperf3 -c 192.168.10.2
```

Run a reverse-direction test:

```bash
iperf3 -c 192.168.10.2 -R
```

Run a bidirectional test:

```bash
iperf3 -c 192.168.10.2 --bidir
```

Run a UDP test with a target rate of 50 Mbps:

```bash
iperf3 -c 192.168.10.2 -u -b 50M
```

Run multiple parallel TCP streams:

```bash
iperf3 -c 192.168.10.2 -P 4
```

`iperf3` provides additional control over test duration, parallel streams, TCP and UDP behavior, and reporting.

The test endpoints should be connected through the network path being evaluated.

# 🗁 Calculating Throughput

Throughput can be calculated from the amount of data transferred and the time required.

Formula:

\[
\text{Throughput} = \frac{\text{Transferred Data in Bits}}{\text{Time in Seconds}}
\]

Example:

A network transfers 500 MB of data in 50 seconds.

Convert megabytes to megabits:

\[
500 \times 8 = 4000 \text{ Mb}
\]

Calculate throughput:

\[
\frac{4000}{50} = 80 \text{ Mbps}
\]

The average throughput is 80 Mbps.

When calculating throughput, distinguish between decimal MB/Mbps and binary MiB/Mibps units.

# 🗁 Bandwidth Testing Best Practices

* Use two capable devices as test endpoints.
* Prefer wired Ethernet connections for baseline tests.
* Verify negotiated interface speed before testing.
* Monitor CPU utilization on both MikroTik devices.
* Test transmit and receive directions separately.
* Use TCP for practical transfer performance.
* Use UDP when investigating packet loss under a controlled offered load.
* Start with low UDP rates and increase gradually.
* Avoid unnecessary parallel tests on production networks.
* Restrict access to the Bandwidth Test server.
* Compare results with interface counters and link capacity.
* Repeat tests under different network conditions.
* Use external tools such as `iperf3` when more detailed measurements are required.

# 🗁 Practical Bandwidth Testing Lab

**Lab topology**

```text
       MikroTik Router A
       192.168.10.1
       Bandwidth Test Client
                |
                |
          Ethernet Link
                |
                |
       MikroTik Router B
       192.168.10.2
       Bandwidth Test Server
```

**Configure IP addresses**

On Router A:

```routeros
/ip/address/add address=192.168.10.1/24 interface=ether1
```

On Router B:

```routeros
/ip/address/add address=192.168.10.2/24 interface=ether1
```

Ensure the devices have Layer 2 connectivity and that their firewall policies permit the required test traffic.

**Enable the Bandwidth Test server**

On Router B:

```routeros
/tool/bandwidth-server/set enabled=yes authenticate=yes \
    allowed-addresses=192.168.10.1/32
```

**Run a TCP test**

On Router A:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=tcp \
    direction=both user=test password=StrongPassword
```

**Run a UDP test**

On Router A:

```routeros
/tool/bandwidth-test address=192.168.10.2 protocol=udp \
    direction=both local-tx-speed=20M remote-tx-speed=20M \
    user=test password=StrongPassword
```

**Monitor interface traffic**

```routeros
/interface/monitor-traffic ether1
```

**Monitor CPU utilization**

```routeros
/tool/profile
```

**Disable the Bandwidth Test server after testing**

On Router B:

```routeros
/tool/bandwidth-server/set enabled=no
```

**Lab objectives**

* Enable and configure the Bandwidth Test server.
* Run TCP and UDP bandwidth tests.
* Compare transmit and receive throughput.
* Observe interface traffic during testing.
* Monitor CPU utilization on both routers.
* Identify potential throughput bottlenecks.
* Restrict test server access to authorized devices.
* Disable the test server after completing the lab.

This lab demonstrates how MikroTik administrators can measure network throughput and correlate test results with interface and system performance.
