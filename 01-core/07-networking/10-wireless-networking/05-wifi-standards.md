# Wi-Fi Standards and Generations

Wi-Fi is a wireless networking technology based on the IEEE 802.11 family of standards.

Different IEEE 802.11 amendments introduce improvements in wireless speed, frequency support, channel utilization, security, reliability, and network capacity.

The Wi-Fi Alliance introduced generation-based naming to make Wi-Fi technologies easier to identify.

Understanding Wi-Fi standards is important when selecting access points, wireless adapters, routers, and client devices.

# 🗁 What Are Wi-Fi Standards?

Wi-Fi standards define how wireless devices communicate.

The IEEE develops the technical specifications, while the Wi-Fi Alliance develops certification programs that test interoperability and promote consistent implementation of Wi-Fi technologies.

For example:

* IEEE 802.11n introduced Wi-Fi 4.
* IEEE 802.11ac introduced Wi-Fi 5.
* IEEE 802.11ax introduced Wi-Fi 6.
* IEEE 802.11be introduced Wi-Fi 7.

A newer Wi-Fi generation generally introduces additional capabilities, but actual performance depends on the access point, client hardware, channel configuration, signal conditions, and network environment.

# 🗁 Wi-Fi Generations

| Wi-Fi Generation | IEEE Standard | Frequency Bands | Maximum Theoretical PHY Rate |
| --- | --- | --- | --- |
| Wi-Fi 1 | 802.11b | 2.4 GHz | 11 Mbps |
| Wi-Fi 2 | 802.11a | 5 GHz | 54 Mbps |
| Wi-Fi 3 | 802.11g | 2.4 GHz | 54 Mbps |
| Wi-Fi 4 | 802.11n | 2.4 / 5 GHz | Up to 600 Mbps |
| Wi-Fi 5 | 802.11ac | 5 GHz | Up to approximately 6.9 Gbps |
| Wi-Fi 6 | 802.11ax | 2.4 / 5 GHz | Up to approximately 9.6 Gbps |
| Wi-Fi 6E | 802.11ax | 2.4 / 5 / 6 GHz | Same PHY capabilities as Wi-Fi 6 |
| Wi-Fi 7 | 802.11be | 2.4 / 5 / 6 GHz | Up to approximately 46 Gbps |

These are theoretical maximum PHY rates under specified configurations, not guaranteed application throughput. Wi-Fi 1, Wi-Fi 2, and Wi-Fi 3 are retrospective generation labels rather than commonly marketed certification names.

# 🗁 IEEE 802.11b

IEEE 802.11b was one of the early widely adopted Wi-Fi standards.

**Characteristics**

* Operates in the 2.4 GHz band.
* Supports data rates up to 11 Mbps.
* Uses Direct Sequence Spread Spectrum (DSSS).
* Provides relatively broad compatibility with early Wi-Fi devices.

It is now considered obsolete for modern wireless deployments.

# 🗁 IEEE 802.11a

IEEE 802.11a introduced higher data rates through Orthogonal Frequency Division Multiplexing (OFDM).

**Characteristics**

* Operates in the 5 GHz band.
* Supports data rates up to 54 Mbps.
* Uses OFDM modulation.
* Provides more channel options than early 2.4 GHz deployments in many regulatory domains.

It helped establish the foundation for later high-throughput Wi-Fi technologies.

# 🗁 IEEE 802.11g

IEEE 802.11g improved the performance of 2.4 GHz wireless networks.

**Characteristics**

* Operates in the 2.4 GHz band.
* Supports data rates up to 54 Mbps.
* Uses OFDM for higher-rate operation.
* Maintains compatibility with many 802.11b devices.

Its backward compatibility can introduce additional overhead when older devices are present.

# 🗁 IEEE 802.11n — Wi-Fi 4

Wi-Fi 4 introduced significant improvements in wireless performance.

**Key Features**

* Supports 2.4 GHz and 5 GHz operation.
* Introduces Multiple-Input Multiple-Output (MIMO).
* Supports channel widths of 20 MHz and 40 MHz.
* Introduces frame aggregation.
* Improves wireless throughput and efficiency.

**MIMO**

MIMO uses multiple transmit and receive antennas to improve communication performance.

For example, a 2×2 MIMO device has two transmit and two receive spatial chains, subject to its actual hardware design.

Multiple spatial streams can increase the theoretical data rate.

# 🗁 IEEE 802.11ac — Wi-Fi 5

Wi-Fi 5 introduced higher wireless throughput through improvements in channel width, modulation, and spatial-stream capabilities.

**Key Features**

* Operates in the 5 GHz band.
* Supports channel widths up to 160 MHz.
* Supports 256-QAM in applicable configurations.
* Introduces downlink Multi-User MIMO (MU-MIMO).
* Supports higher data rates through multiple spatial streams.

Wi-Fi 5 is commonly found in home and enterprise wireless equipment.

# 🗁 IEEE 802.11ax — Wi-Fi 6

Wi-Fi 6 improves wireless efficiency and performance, especially in environments containing many connected devices.

**Key Features**

* Supports 2.4 GHz and 5 GHz.
* Uses Orthogonal Frequency Division Multiple Access (OFDMA).
* Supports uplink and downlink MU-MIMO.
* Supports 1024-QAM in applicable configurations.
* Introduces Target Wake Time (TWT).
* Improves performance in dense wireless environments.

**OFDMA**

OFDMA divides a wireless channel into smaller resource units that can be allocated to multiple users.

This allows an access point to serve multiple clients more efficiently rather than assigning the entire channel to one transmission at a time.

**Target Wake Time (TWT)**

TWT allows compatible devices to negotiate scheduled wake-up times.

It can reduce unnecessary wireless activity and improve power efficiency, particularly for supported battery-powered devices.

# 🗁 Wi-Fi 6E

Wi-Fi 6E extends Wi-Fi 6 operation into the 6 GHz frequency band.

It is not a completely new IEEE generation. It uses the same underlying 802.11ax technology.

**Key Features**

* Supports 6 GHz operation.
* Provides additional wireless spectrum.
* Supports wider channel configurations.
* Reduces competition with many legacy 2.4 GHz and 5 GHz devices.
* Requires compatible access points and client devices.

Wi-Fi 6E availability depends on the regulatory domain.

# 🗁 IEEE 802.11be — Wi-Fi 7

Wi-Fi 7 introduces additional capabilities designed to improve throughput, latency, reliability, and wireless capacity.

**Key Features**

* Supports 2.4 GHz, 5 GHz, and 6 GHz bands.
* Supports channel widths up to 320 MHz in the 6 GHz band where permitted.
* Supports 4096-QAM in applicable configurations.
* Introduces Multi-Link Operation (MLO).
* Improves multi-user scheduling and resource utilization.
* Supports enhanced puncturing capabilities.

**Multi-Link Operation (MLO)**

MLO allows compatible devices to use multiple wireless links across supported bands or channels.

Depending on the implementation, this can improve throughput, reliability, and latency by allowing traffic to use more than one link.

**4096-QAM**

4096-QAM represents a higher modulation order than the 1024-QAM used in Wi-Fi 6.

It can carry more bits per symbol when signal quality is sufficiently high.

**320 MHz Channels**

Wi-Fi 7 supports 320 MHz channel operation in the 6 GHz band where regulations and hardware permit.

Wider channels provide additional potential throughput but require substantial clean spectrum.

# 🗁 Wi-Fi Generations Comparison

| Feature | Wi-Fi 4 | Wi-Fi 5 | Wi-Fi 6 / 6E | Wi-Fi 7 |
| --- | --- | --- | --- | --- |
| IEEE Standard | 802.11n | 802.11ac | 802.11ax | 802.11be |
| Frequency | 2.4 / 5 GHz | 5 GHz | 2.4 / 5 / 6 GHz with 6E | 2.4 / 5 / 6 GHz |
| Maximum Channel Width | 40 MHz | 160 MHz | 160 MHz | 320 MHz |
| Maximum Modulation | 64-QAM | 256-QAM | 1024-QAM | 4096-QAM |
| OFDMA | No | No | Yes | Yes |
| MU-MIMO | Limited | Downlink | Uplink and downlink | Enhanced capabilities |
| MLO | No | No | No | Yes |

# 🗁 Wi-Fi Backward Compatibility

Modern Wi-Fi devices often support communication with older IEEE 802.11 generations.

For example, a Wi-Fi 6 access point may support compatible Wi-Fi 5 and Wi-Fi 4 clients.

However:

* The client and access point must share a supported frequency band.
* The connection uses capabilities supported by both devices.
* Older devices may introduce additional compatibility and airtime overhead.
* Some newer features require both the access point and client to support them.

Backward compatibility does not mean every older device can connect to every modern wireless network.

# 🗁 Wi-Fi Certification

The Wi-Fi Alliance provides certification programs for products that implement supported Wi-Fi technologies.

Certification helps verify interoperability and compliance with specified feature requirements.

Examples include:

* Wi-Fi CERTIFIED 6
* Wi-Fi CERTIFIED 6E
* Wi-Fi CERTIFIED 7

IEEE standardization and Wi-Fi Alliance certification are related but different activities.

# 🗁 Practical Wi-Fi Standard Identification

In Linux, wireless hardware capabilities can be inspected using `iw`.

**Identify Wireless Interfaces**

```bash
iw dev
```

**Inspect Wireless Hardware Capabilities**

```bash
iw phy
```

**Display Current Wireless Connection**

```bash
iw dev wlan0 link
```

The output may include:

* Connected access point.
* Frequency.
* Signal strength.
* Transmit and receive bitrate.
* Other connection information.

The negotiated bitrate and supported capabilities help identify the actual connection mode, but they do not independently guarantee a particular Wi-Fi generation or application throughput.

# 🗁 Choosing a Wi-Fi Generation

| Requirement | Suitable Consideration |
| --- | --- |
| Basic home connectivity | Wi-Fi 5 or newer |
| General home and office use | Wi-Fi 6 |
| Dense wireless environments | Wi-Fi 6 or Wi-Fi 7 |
| Additional 6 GHz spectrum | Wi-Fi 6E or Wi-Fi 7 |
| High-throughput modern deployments | Wi-Fi 7 |
| Compatibility with older clients | Check supported bands and IEEE amendments |

A practical wireless deployment should consider client compatibility, coverage, access point density, available spectrum, and budget rather than selecting hardware based only on its advertised maximum speed.

# 🗁 Key Takeaways

* Wi-Fi is based on the IEEE 802.11 family of wireless LAN standards.
* The Wi-Fi Alliance provides certification and generation-based naming.
* Wi-Fi 4 uses IEEE 802.11n.
* Wi-Fi 5 uses IEEE 802.11ac.
* Wi-Fi 6 and Wi-Fi 6E use IEEE 802.11ax.
* Wi-Fi 7 uses IEEE 802.11be.
* MIMO uses multiple spatial streams to improve wireless communication.
* OFDMA improves airtime efficiency by allocating resource units to multiple clients.
* Wi-Fi 6E extends Wi-Fi 6 into the 6 GHz band.
* Wi-Fi 7 introduces capabilities such as MLO and 320 MHz channels.
* Theoretical PHY rates are not equivalent to real-world application throughput.
* Actual wireless performance depends on access point capabilities, client hardware, signal quality, interference, and network configuration.
