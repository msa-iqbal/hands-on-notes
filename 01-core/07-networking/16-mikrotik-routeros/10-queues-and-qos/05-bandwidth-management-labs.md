# Bandwidth Management Labs

Bandwidth management is an important part of MikroTik network administration. It allows administrators to control bandwidth consumption, distribute available capacity among users, prioritize important traffic, and prevent individual clients from consuming all available resources.

These practical labs use MikroTik RouterOS v7 to demonstrate bandwidth management with Simple Queues, Queue Trees, PCQ, and queue priorities.

# 🗁 Lab Environment

The following topology will be used throughout the labs.

```text
                  Internet
                     |
                     |
                ISP Gateway
                     |
                     |
              ether1 (WAN)
                     |
              MikroTik Router
                     |
               bridge (LAN)
                     |
          +----------+----------+
          |          |          |
       Client A   Client B   Client C
       192.168.10.10  .20       .30
```

Network configuration:

| Component | Configuration |
| --- | --- |
| WAN interface | `ether1` |
| LAN interface | `bridge` |
| LAN network | `192.168.10.0/24` |
| Router LAN IP | `192.168.10.1` |
| Client A | `192.168.10.10` |
| Client B | `192.168.10.20` |
| Client C | `192.168.10.30` |
| WAN upload capacity | 20 Mbps |
| WAN download capacity | 100 Mbps |

Assumptions:

* WAN connectivity is already working.
* LAN clients can access the internet.
* The LAN bridge and IP addressing are configured.
* NAT and routing are already configured.
* The interface names match the lab environment.

Adapt interface names and bandwidth values to your actual network.

# 🗁 Lab: Configure a Simple Queue for One Client

**Objective:** Limit the bandwidth of an individual client.

Suppose Client A should receive:

* Upload: 5 Mbps.
* Download: 10 Mbps.

Create a Simple Queue:

```routeros
/queue simple
add name="Client-A" target=192.168.10.10/32 max-limit=5M/10M
```

Verify the configuration:

```routeros
/queue simple print
```

Monitor traffic:

```routeros
/queue simple print stats
```

**Expected result:**

Client A should be limited to approximately 5 Mbps upload and 10 Mbps download, subject to available bandwidth and network conditions.

# 🗁 Lab: Configure Bandwidth Limits for Multiple Clients

**Objective:** Assign different bandwidth limits to multiple clients.

Create individual queues:

```routeros
/queue simple
add name="Client-A" target=192.168.10.10/32 max-limit=5M/10M

add name="Client-B" target=192.168.10.20/32 max-limit=3M/8M

add name="Client-C" target=192.168.10.30/32 max-limit=2M/5M
```

Display the configured queues:

```routeros
/queue simple print detail
```

Monitor their statistics:

```routeros
/queue simple print stats
```

**Expected result:**

| Client | Upload Limit | Download Limit |
| --- | ---: | ---: |
| Client A | 5 Mbps | 10 Mbps |
| Client B | 3 Mbps | 8 Mbps |
| Client C | 2 Mbps | 5 Mbps |

Each client has an individual bandwidth limit.

# 🗁 Lab: Configure a Shared Simple Queue

**Objective:** Limit the total bandwidth available to an entire subnet.

Suppose the entire LAN subnet should share:

* Upload: 20 Mbps.
* Download: 100 Mbps.

Create a shared queue:

```routeros
/queue simple
add name="LAN-SHARED" target=192.168.10.0/24 max-limit=20M/100M
```

Monitor the queue:

```routeros
/queue simple print stats
```

**Expected result:**

All matching LAN traffic shares the aggregate bandwidth limit.

This configuration does not guarantee equal bandwidth distribution among clients. Individual clients may consume different amounts depending on traffic demand and queue behavior.

# 🗁 Lab: Configure PCQ for Equal Client Bandwidth Sharing

**Objective:** Distribute available bandwidth among active clients using PCQ.

Suppose the LAN subnet has a shared bandwidth limit of:

* Upload: 20 Mbps.
* Download: 100 Mbps.

Create PCQ queue types:

```routeros
/queue type
add name=pcq-upload kind=pcq pcq-classifier=src-address pcq-rate=0

add name=pcq-download kind=pcq pcq-classifier=dst-address pcq-rate=0
```

Create a Simple Queue:

```routeros
/queue simple
add name="LAN-PCQ" target=192.168.10.0/24 max-limit=20M/100M queue=pcq-upload/pcq-download
```

Verify the configuration:

```routeros
/queue type print detail
/queue simple print detail
```

Monitor queue statistics:

```routeros
/queue simple print stats
```

**Expected result:**

PCQ creates separate sub-queues based on client IP addresses.

If three clients actively download at the same time and demand sufficient bandwidth, they can share the available 100 Mbps approximately equally.

The same principle applies to upload traffic using source-address classification.

Actual throughput depends on active demand, queue limits, and other network constraints.

# 🗁 Lab: Configure a Fixed PCQ Rate per Client

**Objective:** Limit the maximum bandwidth available to each classified client.

Suppose every client should receive a maximum of:

* Upload: 5 Mbps.
* Download: 10 Mbps.

Create fixed-rate PCQ types:

```routeros
/queue type
add name=pcq-upload-5M kind=pcq pcq-classifier=src-address pcq-rate=5M

add name=pcq-download-10M kind=pcq pcq-classifier=dst-address pcq-rate=10M
```

Create the subnet queue:

```routeros
/queue simple
add name="LAN-PCQ-FIXED" target=192.168.10.0/24 max-limit=20M/100M queue=pcq-upload-5M/pcq-download-10M
```

**Expected result:**

Each classified client can receive up to 5 Mbps upload and 10 Mbps download.

The aggregate bandwidth remains constrained by the parent Simple Queue limits.

For example, ten clients cannot collectively consume 100 Mbps upload when the parent upload limit is only 20 Mbps.

# 🗁 Lab: Configure Queue Types for Different Traffic Classes

**Objective:** Create separate queue types for different traffic-management requirements.

Create a PFIFO queue type:

```routeros
/queue type
add name=custom-pfifo kind=pfifo pfifo-limit=100
```

Create an SFQ queue type:

```routeros
/queue type
add name=custom-sfq kind=sfq
```

Create a PCQ upload queue type:

```routeros
/queue type
add name=custom-pcq-upload kind=pcq pcq-classifier=src-address pcq-rate=0
```

Create a PCQ download queue type:

```routeros
/queue type
add name=custom-pcq-download kind=pcq pcq-classifier=dst-address pcq-rate=0
```

Verify the queue types:

```routeros
/queue type print detail
```

**Expected result:**

The router has multiple queue types available for use with Simple Queues or Queue Trees.

# 🗁 Lab: Configure Queue Tree with Packet Marks

**Objective:** Manage upload and download bandwidth using Queue Tree.

Queue Tree commonly uses Firewall Mangle packet marks to classify traffic.

For this lab:

* WAN interface: `ether1`
* LAN interface: `bridge`
* LAN subnet: `192.168.10.0/24`
* Upload limit: 20 Mbps
* Download limit: 100 Mbps

Create PCQ queue types:

```routeros
/queue type
add name=pcq-upload-tree kind=pcq pcq-classifier=src-address pcq-rate=0

add name=pcq-download-tree kind=pcq pcq-classifier=dst-address pcq-rate=0
```

Create an upload packet mark:

```routeros
/ip firewall mangle
add chain=forward src-address=192.168.10.0/24 out-interface=ether1 action=mark-packet new-packet-mark=upload-traffic passthrough=no comment="PCQ upload traffic"
```

Create a download packet mark:

```routeros
/ip firewall mangle
add chain=forward dst-address=192.168.10.0/24 in-interface=ether1 action=mark-packet new-packet-mark=download-traffic passthrough=no comment="PCQ download traffic"
```

Create the upload Queue Tree:

```routeros
/queue tree
add name="WAN-UPLOAD" parent=ether1 packet-mark=upload-traffic queue=pcq-upload-tree max-limit=20M
```

Create the download Queue Tree:

```routeros
/queue tree
add name="WAN-DOWNLOAD" parent=bridge packet-mark=download-traffic queue=pcq-download-tree max-limit=100M
```

Verify packet marks:

```routeros
/ip firewall mangle print stats
```

Verify Queue Tree statistics:

```routeros
/queue tree print stats
```

**Expected result:**

* Upload traffic is classified as `upload-traffic`.
* Download traffic is classified as `download-traffic`.
* PCQ distributes traffic among classified client addresses.
* The WAN upload and LAN download egress queues enforce their respective aggregate limits.

The example assumes the bridge is the LAN egress interface and that the traffic traverses the normal forwarding and queue-processing path.

If the router has FastTrack rules, ensure the traffic being shaped is not bypassing the required Mangle and queue processing.

# 🗁 Lab: Prioritize Important Traffic

**Objective:** Give important applications higher scheduling priority than ordinary traffic.

Suppose the network has three traffic categories:

| Traffic | Priority |
| --- | ---: |
| VoIP | 1 |
| Business applications | 4 |
| Guest internet | 8 |

For this example, assume packet marks named `voip-mark`, `business-mark`, and `guest-mark` have already been created using appropriate Mangle rules.

Create a parent Queue Tree:

```routeros
/queue tree
add name="WAN-PARENT" parent=ether1 max-limit=20M
```

Create the child queues:

```routeros
/queue tree
add name="VOIP" parent="WAN-PARENT" packet-mark=voip-mark priority=1 limit-at=2M max-limit=5M

add name="BUSINESS" parent="WAN-PARENT" packet-mark=business-mark priority=4 limit-at=5M max-limit=15M

add name="GUEST" parent="WAN-PARENT" packet-mark=guest-mark priority=8 limit-at=2M max-limit=10M
```

Verify the configuration:

```routeros
/queue tree print detail
```

Monitor traffic:

```routeros
/queue tree print stats
```

**Expected result:**

* VoIP has the highest configured priority.
* Business applications have normal priority.
* Guest traffic has the lowest configured priority.

The `limit-at` values define committed bandwidth requirements, while `max-limit` controls maximum queue bandwidth.

Priority affects scheduling among competing queues under their shared parent. It does not reserve bandwidth by itself.

# 🗁 Lab: Monitor Bandwidth Consumption

**Objective:** Inspect traffic rates and queue activity.

Display Simple Queue statistics:

```routeros
/queue simple print stats
```

Display Queue Tree statistics:

```routeros
/queue tree print stats
```

Display Mangle rule counters:

```routeros
/ip firewall mangle print stats
```

Monitor WAN traffic:

```routeros
/tool torch interface=ether1
```

Monitor LAN traffic:

```routeros
/tool torch interface=bridge
```

Inspect queue types:

```routeros
/queue type print detail
```

Use these commands while clients generate traffic.

Compare the observed traffic rates with the configured bandwidth limits.

# 🗁 Lab: Test Bandwidth Distribution

**Objective:** Verify whether bandwidth is distributed as expected among clients.

Use at least three LAN clients.

Start a download or run an `iperf3` test from each client toward a suitable external or internal test server.

For example, on a Linux test server:

```bash
iperf3 -s
```

On a client:

```bash
iperf3 -c 192.168.10.100 -t 60
```

Replace `192.168.10.100` with the actual test server address.

For multiple clients, run simultaneous tests.

Observe:

* Individual client throughput.
* Aggregate throughput.
* Upload and download behavior.
* Queue statistics.
* Packet-mark counters.

**Expected result:**

When PCQ is configured correctly, active classified clients should share the available bandwidth according to the configured queue rate and parent limits.

A single-client test may reach a higher rate than a multi-client test because no other active sub-queues are competing for bandwidth.

# 🗁 Lab: Troubleshoot Queue Configuration

**Objective:** Identify common bandwidth-management problems.

**Problem: Queue is not limiting bandwidth**

Inspect the queue configuration:

```routeros
/queue simple print detail
/queue tree print detail
```

Check:

* Whether the queue target matches the client.
* Whether the correct interfaces are selected.
* Whether the parent queue is configured correctly.
* Whether traffic is passing through the expected queue.

**Problem: PCQ does not distribute bandwidth correctly**

Inspect PCQ configuration:

```routeros
/queue type print detail
```

Check:

* Whether upload uses the appropriate source-address classifier.
* Whether download uses the appropriate destination-address classifier.
* Whether clients have distinct addresses at the classification stage.
* Whether parent bandwidth limits are configured correctly.

**Problem: Queue Tree counters remain at zero**

Inspect Mangle counters:

```routeros
/ip firewall mangle print stats
```

Check:

* Whether the Mangle rules match the traffic.
* Whether interface names are correct.
* Whether source and destination address conditions are correct.
* Whether packet marks match the Queue Tree configuration.

**Problem: FastTrack bypasses bandwidth management**

Inspect Firewall Filter rules:

```routeros
/ip firewall filter print detail
```

Review FastTrack rules and ensure traffic requiring shaping follows the appropriate packet-processing path.

**Problem: Clients experience high latency**

Review:

* Queue buffer limits.
* Queue type.
* Configured bandwidth limits.
* Parent queue utilization.
* WAN congestion.
* Whether the configured shaping rate accurately reflects the available link capacity.

# 🗁 Lab: Remove Test Configurations

After completing the exercises, remove the lab-specific configurations if they are no longer required.

Remove Simple Queues:

```routeros
/queue simple
remove [find name="Client-A"]
remove [find name="Client-B"]
remove [find name="Client-C"]
remove [find name="LAN-SHARED"]
remove [find name="LAN-PCQ"]
remove [find name="LAN-PCQ-FIXED"]
```

Remove Queue Tree entries:

```routeros
/queue tree
remove [find name="VOIP"]
remove [find name="BUSINESS"]
remove [find name="GUEST"]
remove [find name="WAN-PARENT"]
remove [find name="WAN-UPLOAD"]
remove [find name="WAN-DOWNLOAD"]
```

Remove lab Mangle rules:

```routeros
/ip firewall mangle
remove [find comment="PCQ upload traffic"]
remove [find comment="PCQ download traffic"]
```

Remove lab queue types:

```routeros
/queue type
remove [find name="pcq-upload"]
remove [find name="pcq-download"]
remove [find name="pcq-upload-5M"]
remove [find name="pcq-download-10M"]
remove [find name="pcq-upload-tree"]
remove [find name="pcq-download-tree"]
remove [find name="custom-pfifo"]
remove [find name="custom-sfq"]
remove [find name="custom-pcq-upload"]
remove [find name="custom-pcq-download"]
```

Only remove entries created specifically for these labs. Do not remove existing production rules or queue types that other configurations depend on.

# 🗁 Practical Exercises

Complete these exercises to strengthen your bandwidth-management skills.

**Exercise: Individual Bandwidth Management**

* Create a 5 Mbps upload and 10 Mbps download limit for one client.
* Create different limits for three clients.
* Verify each client's actual throughput.

**Exercise: Shared Bandwidth Management**

* Configure a 20 Mbps upload and 100 Mbps download limit for a subnet.
* Test with multiple active clients.
* Compare aggregate bandwidth with individual client throughput.

**Exercise: PCQ Bandwidth Distribution**

* Create upload and download PCQ types.
* Configure PCQ with `pcq-rate=0`.
* Test with three or more clients.
* Repeat with a fixed PCQ rate.
* Compare the results.

**Exercise: Queue Tree**

* Create upload and download packet marks.
* Configure separate Queue Tree parents.
* Attach PCQ queue types.
* Verify Mangle and Queue Tree counters.

**Exercise: Traffic Prioritization**

* Classify VoIP, business, and guest traffic.
* Assign different queue priorities.
* Configure `limit-at` and `max-limit`.
* Generate competing traffic and observe scheduling behavior.

# 🗁 Lab Completion Checklist

* [ ] Configure an individual Simple Queue.
* [ ] Configure bandwidth limits for multiple clients.
* [ ] Configure a shared subnet queue.
* [ ] Create PCQ upload and download types.
* [ ] Configure equal bandwidth sharing.
* [ ] Configure fixed per-client PCQ rates.
* [ ] Create custom queue types.
* [ ] Configure Queue Tree with packet marks.
* [ ] Configure traffic priorities.
* [ ] Monitor queue statistics.
* [ ] Test bandwidth distribution with multiple clients.
* [ ] Troubleshoot queue and packet-mark configurations.
* [ ] Remove temporary lab configurations.

# 🗁 Summary

These labs demonstrate practical bandwidth management in MikroTik RouterOS v7.

You have practiced:

* Simple Queue configuration.
* Individual and shared bandwidth limits.
* PCQ-based bandwidth distribution.
* Custom queue types.
* Queue Tree configuration.
* Mangle packet marking.
* Traffic prioritization.
* Bandwidth monitoring.
* Queue troubleshooting.

These techniques provide a foundation for managing bandwidth in home networks, business networks, wireless networks, and ISP environments.
