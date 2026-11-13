# MikroTik Certified Wireless Engineer (MTCWE)

The MikroTik Certified Wireless Engineer (MTCWE) is a specialized certification focused on wireless networking technologies and their implementation using MikroTik RouterOS.

Wireless networking is an essential part of modern network infrastructure. It enables devices to communicate without physical Ethernet connections and provides connectivity for homes, offices, campuses, public areas, and wireless Internet Service Providers (WISPs).

This roadmap covers wireless fundamentals, IEEE 802.11 standards, radio frequencies, wireless security, access point configuration, wireless bridging, signal analysis, and troubleshooting.

The primary objective is to develop the practical skills required to design, deploy, manage, and troubleshoot MikroTik wireless networks.

# 🗁 Learning Objectives

After completing this roadmap, you should be able to:

* Understand wireless networking fundamentals.
* Explain IEEE 802.11 standards.
* Understand radio frequencies and wireless channels.
* Configure MikroTik wireless interfaces.
* Configure access points and wireless clients.
* Understand wireless security mechanisms.
* Configure wireless bridges.
* Analyze signal strength and signal quality.
* Understand wireless interference and channel planning.
* Optimize wireless network performance.
* Troubleshoot wireless connectivity problems.
* Design point-to-point and point-to-multipoint wireless networks.

# 🗁 Prerequisites

Before starting MTCWE preparation, you should understand:

* Networking fundamentals.
* Ethernet and MAC addressing.
* IPv4 addressing and subnetting.
* TCP/IP networking.
* Basic routing and switching.
* MikroTik RouterOS fundamentals.
* Interface and bridge configuration.
* WinBox and RouterOS CLI.

Completing MTCNA or having equivalent practical RouterOS knowledge is recommended.

# 🗁 Wireless Networking Fundamentals

Understand how wireless devices communicate using radio-frequency signals.

### 🖆 Wireless Network Concepts

* What is wireless networking?
* Wireless Local Area Network (WLAN).
* Wireless Access Point (AP).
* Wireless client or station.
* Wireless network interface.
* Infrastructure mode.
* Ad hoc networking.
* Point-to-point wireless.
* Point-to-multipoint wireless.
* Wireless distribution systems.
* Wireless bridging.

### 🖆 Wireless Network Components

Understand the purpose of common wireless networking components:

* Wireless router.
* Wireless access point.
* Wireless client adapter.
* Wireless antenna.
* Radio transceiver.
* Wireless controller.
* Outdoor wireless CPE.
* Point-to-point radio equipment.

### 🖆 Wireless Network Architecture

Understand common wireless network designs:

* Single access point network.
* Multiple access point network.
* Centralized wireless management.
* Distributed wireless deployment.
* Wireless backhaul.
* Outdoor wireless distribution.

# 🗁 IEEE 802.11 Standards

Understand the IEEE 802.11 family of wireless networking standards.

### 🖆 Wireless Standards

Study the major standards and their characteristics.

| Standard | Frequency Bands | Maximum Theoretical Data Rate |
| --- | --- | ---: |
| 802.11b | 2.4 GHz | 11 Mbps |
| 802.11a | 5 GHz | 54 Mbps |
| 802.11g | 2.4 GHz | 54 Mbps |
| 802.11n | 2.4 GHz / 5 GHz | Up to 600 Mbps |
| 802.11ac | 5 GHz | Several Gbps, depending on configuration |
| 802.11ax | 2.4 GHz / 5 GHz / 6 GHz where supported | Several Gbps, depending on configuration |
| 802.11be | 2.4 GHz / 5 GHz / 6 GHz where supported | Higher multi-gigabit capabilities |

Actual performance depends on channel width, spatial streams, modulation, interference, client capabilities, and regulatory restrictions.

### 🖆 Wireless Technologies

Understand the technologies used to improve wireless communication:

* OFDM.
* OFDMA.
* MIMO.
* MU-MIMO.
* Beamforming.
* Spatial streams.
* Modulation and coding schemes.
* Channel bonding.
* Guard intervals.

### 🖆 Wireless Compatibility

Understand:

* Backward compatibility.
* Client capability negotiation.
* Band compatibility.
* Channel width compatibility.
* Legacy wireless devices.
* Mixed wireless environments.

# 🗁 Radio Frequency Fundamentals

Wireless communication uses electromagnetic waves to transmit information.

### 🖆 Frequency and Wavelength

Understand:

* Radio frequency (RF).
* Frequency.
* Wavelength.
* Amplitude.
* Phase.
* Electromagnetic spectrum.
* Frequency bands.
* Signal propagation.

### 🖆 Common Wi-Fi Frequency Bands

Understand the characteristics of commonly used Wi-Fi frequency bands.

| Frequency Band | Characteristics |
| --- | --- |
| 2.4 GHz | Longer range in many environments, but often congested |
| 5 GHz | More channel capacity and generally less congestion than 2.4 GHz |
| 6 GHz | Additional spectrum for supported Wi-Fi generations and devices |

Availability of channels and permitted transmission power depend on the regulatory domain.

### 🖆 Wireless Channels

Learn:

* Channel numbers.
* Channel frequency.
* Channel width.
* Channel overlap.
* Non-overlapping channel planning.
* Dynamic frequency selection (DFS).
* Automatic channel selection.
* Regulatory domain restrictions.

### 🖆 Channel Width

Understand common channel widths:

* 20 MHz.
* 40 MHz.
* 80 MHz.
* 160 MHz.
* 320 MHz where supported.

Wider channels can increase theoretical throughput but consume more spectrum and may increase interference or reduce channel availability.

# 🗁 Wireless Signal Fundamentals

Understand how radio signals behave in real-world environments.

### 🖆 Signal Strength

Learn common wireless signal measurements:

* Received Signal Strength Indicator (RSSI).
* Received signal level.
* Signal-to-Noise Ratio (SNR).
* Noise floor.
* Signal quality.
* Transmit power.
* Receiver sensitivity.

Signal strength is commonly expressed in dBm.

A value closer to 0 dBm represents a stronger received signal. For example, -50 dBm is stronger than -80 dBm.

### 🖆 Signal-to-Noise Ratio

SNR represents the difference between received signal level and noise level, expressed in decibels.

Formula:

```text
SNR = Signal Level - Noise Level
```

Example:

```text
Signal Level = -55 dBm
Noise Level  = -95 dBm

SNR = -55 - (-95)
SNR = 40 dB
```

A higher SNR generally supports more reliable communication and higher modulation rates, depending on the radio system.

### 🖆 Wireless Signal Propagation

Understand factors that affect wireless signal propagation:

* Free-space path loss.
* Distance.
* Frequency.
* Antenna gain.
* Transmit power.
* Receiver sensitivity.
* Obstacles.
* Reflection.
* Diffraction.
* Scattering.
* Absorption.
* Multipath propagation.
* Fresnel zone clearance.

### 🖆 Line of Sight

Learn the importance of:

* Visual line of sight.
* Radio line of sight.
* Fresnel zone clearance.
* Antenna alignment.
* Terrain obstruction.
* Building obstruction.
* Environmental interference.

# 🗁 Wireless Antennas

Understand how antennas transmit and receive radio-frequency energy.

### 🖆 Antenna Types

Study common antenna types:

* Omnidirectional antenna.
* Directional antenna.
* Sector antenna.
* Panel antenna.
* Yagi antenna.
* Parabolic dish antenna.
* Grid antenna.

### 🖆 Antenna Characteristics

Understand:

* Antenna gain.
* Radiation pattern.
* Beamwidth.
* Polarization.
* Horizontal polarization.
* Vertical polarization.
* Dual polarization.
* Antenna orientation.

### 🖆 Effective Isotropic Radiated Power

Understand EIRP and how transmit power, cable losses, and antenna gain affect the radiated signal.

A simplified formula is:

```text
EIRP (dBm) = Transmit Power (dBm)
             + Antenna Gain (dBi)
             - Cable Loss (dB)
```

Always consider the applicable regulatory EIRP limits.

# 🗁 MikroTik Wireless Architecture

Understand the wireless capabilities available in MikroTik RouterOS.

### 🖆 Wireless Packages and Drivers

Learn the differences between MikroTik wireless implementations:

* Legacy wireless package.
* Wireless package and supported drivers.
* Newer WiFi configuration framework.
* Device and chipset compatibility.
* RouterOS version compatibility.

Wireless commands and available features depend on the hardware platform, wireless driver, installed package, and RouterOS version.

### 🖆 Wireless Interface Management

Learn how to:

* Identify wireless interfaces.
* Check interface status.
* Enable and disable wireless interfaces.
* Monitor wireless statistics.
* Inspect connected clients.
* Review wireless logs.
* Identify supported frequencies and channels.

### 🖆 Wireless Interface Properties

Understand important configuration parameters:

* SSID.
* Frequency.
* Band.
* Channel width.
* Wireless mode.
* Country or regulatory domain.
* Transmit power.
* Antenna settings.
* Security profile.
* Data rates.

# 🗁 Access Point Configuration

Configure MikroTik devices to provide wireless access to client devices.

### 🖆 Access Point Fundamentals

Understand:

* Access point operation.
* Wireless network identification.
* SSID broadcasting.
* Client association.
* Authentication.
* Encryption.
* Wireless client isolation.
* Access point capacity.

### 🖆 Access Point Configuration

Practice configuring:

* Wireless interface mode.
* SSID.
* Frequency.
* Channel width.
* Country settings.
* Security profile.
* Wireless interface enablement.
* Bridge integration.

### 🖆 Access Point and LAN Bridging

Understand how wireless clients communicate with wired LAN devices.

Study:

* Bridge configuration.
* Wireless interface membership.
* Ethernet interface membership.
* Layer 2 forwarding.
* DHCP integration.
* LAN connectivity verification.

# 🗁 Wireless Client Configuration

Configure MikroTik devices to connect to an existing wireless network.

### 🖆 Wireless Station Modes

Understand common wireless station modes and their purposes.

* Station.
* Station bridge where supported.
* Station pseudobridge where applicable.
* Station WDS in supported legacy environments.
* Other driver-specific station modes.

Different modes have different Layer 2 forwarding capabilities. Do not assume every station mode provides transparent Ethernet bridging.

### 🖆 Wireless Client Configuration

Practice:

* Selecting an access point.
* Configuring the SSID.
* Selecting a security profile.
* Configuring supported frequencies.
* Connecting to an access point.
* Monitoring signal quality.
* Verifying connectivity.
* Troubleshooting authentication failures.

# 🗁 Wireless Security

Understand how wireless networks protect communication and prevent unauthorized access.

### 🖆 Wireless Security Fundamentals

Study:

* Wireless authentication.
* Wireless encryption.
* Pre-shared keys.
* Enterprise authentication.
* Client access control.
* Rogue access points.
* Wireless network isolation.

### 🖆 Wireless Security Protocols

Understand the differences between:

* WEP.
* WPA.
* WPA2.
* WPA3.

WEP and original WPA are obsolete for modern secure deployments. Prefer WPA2 with AES or WPA3 where supported by the access point and clients.

### 🖆 Security Profiles

Learn how wireless security profiles define authentication and encryption settings.

Practice:

* Creating security profiles.
* Configuring strong passphrases.
* Selecting supported encryption methods.
* Applying profiles to wireless interfaces.
* Testing client authentication.

### 🖆 Enterprise Wireless Authentication

Understand the role of:

* IEEE 802.1X.
* EAP.
* RADIUS.
* Authentication servers.
* Centralized access control.

# 🗁 Wireless Bridging

Understand how wireless links can connect separate Ethernet networks.

### 🖆 Wireless Bridge Fundamentals

Study:

* Point-to-point wireless bridging.
* Point-to-multipoint wireless bridging.
* Layer 2 connectivity.
* Transparent bridging.
* MAC address forwarding.
* Wireless distribution systems.
* Bridge compatibility.

### 🖆 WDS

Understand Wireless Distribution System (WDS) and its role in connecting access points through wireless links.

Learn:

* WDS fundamentals.
* WDS modes.
* Dynamic WDS.
* Static WDS.
* Bridge integration.
* WDS limitations.
* Compatibility considerations.

WDS behavior depends on the wireless implementation and hardware compatibility.

### 🖆 Point-to-Point Wireless Link

Practice creating a wireless connection between two locations.

* Configure the central radio.
* Configure the remote radio.
* Align directional antennas.
* Select an appropriate frequency.
* Configure wireless security.
* Establish the wireless link.
* Bridge Ethernet networks.
* Test connectivity and throughput.

# 🗁 Wireless Network Planning

Learn how to design reliable wireless coverage before deployment.

### 🖆 Coverage Planning

Understand:

* Coverage area.
* Access point placement.
* Client density.
* Indoor and outdoor deployment.
* Coverage overlap.
* Signal propagation.
* Physical obstacles.
* Roaming requirements.

### 🖆 Channel Planning

Study:

* Frequency reuse.
* Channel separation.
* Co-channel interference.
* Adjacent-channel interference.
* Channel width selection.
* DFS channel considerations.
* Regulatory restrictions.

### 🖆 Wireless Capacity Planning

Understand the factors that influence wireless network capacity:

* Number of connected clients.
* Available channel bandwidth.
* Spatial streams.
* Modulation and coding scheme.
* Airtime utilization.
* Client capabilities.
* Traffic demand.
* Backhaul capacity.

# 🗁 Wireless Performance Optimization

Understand how to improve wireless network performance.

### 🖆 Performance Factors

Study:

* Signal strength.
* Signal-to-noise ratio.
* Interference.
* Channel utilization.
* Airtime fairness.
* Retransmissions.
* Data rates.
* Client distribution.
* Backhaul limitations.

### 🖆 Wireless Optimization Techniques

Practice:

* Selecting appropriate channels.
* Adjusting channel width.
* Improving antenna alignment.
* Reducing unnecessary transmit power.
* Optimizing access point placement.
* Managing connected clients.
* Reducing interference.
* Monitoring wireless utilization.

### 🖆 Wireless Roaming

Understand how wireless clients move between access points.

Study:

* Roaming fundamentals.
* SSID consistency.
* Security compatibility.
* Signal thresholds.
* Client roaming decisions.
* IEEE 802.11k.
* IEEE 802.11v.
* IEEE 802.11r.

Roaming behavior depends on client support, access point configuration, and wireless architecture.

# 🗁 Wireless Monitoring and Troubleshooting

Develop the ability to identify and resolve wireless network problems.

### 🖆 Wireless Monitoring

Learn how to inspect:

* Connected wireless clients.
* Signal strength.
* Signal-to-noise ratio.
* Noise floor.
* Wireless registration tables.
* Interface statistics.
* Frequency utilization.
* Transmission and reception rates.
* Wireless logs.

### 🖆 Wireless Diagnostic Tools

Understand the purpose of:

* Wireless registration table.
* Frequency scan.
* Wireless snooper.
* Interface monitor.
* Torch.
* Ping.
* Traceroute.
* RouterOS logging.

Tool availability and syntax vary between wireless drivers and RouterOS versions.

### 🖆 Common Wireless Problems

Practice troubleshooting:

* Client cannot find the SSID.
* Wireless authentication failure.
* Incorrect security settings.
* Weak signal strength.
* High noise level.
* Channel interference.
* Low wireless throughput.
* Frequent disconnections.
* Incorrect antenna alignment.
* Wireless bridge instability.
* Hidden or blocked channels.
* Client compatibility problems.

# 🗁 Practical Lab Exercises

Use compatible MikroTik wireless hardware or a suitable wireless lab environment.

CHR can help with routing and bridging exercises, but it does not replace physical wireless radios for RF, antenna, and signal testing.

### 🖆 Basic Access Point Lab

* Configure a MikroTik wireless access point.
* Create an SSID.
* Select an appropriate frequency.
* Configure channel width.
* Create a security profile.
* Connect wireless clients.
* Verify client connectivity.

### 🖆 Wireless Client Lab

* Configure a second MikroTik device as a wireless client.
* Connect it to the access point.
* Inspect registration information.
* Check signal strength.
* Verify IP connectivity.
* Troubleshoot authentication issues.

### 🖆 Wireless Bridge Lab

* Connect two MikroTik wireless devices.
* Configure a point-to-point wireless link.
* Integrate wireless interfaces into bridges where supported.
* Connect LAN clients at both ends.
* Verify Layer 2 connectivity.
* Test network performance.

### 🖆 Channel Planning Lab

* Scan available frequencies.
* Identify occupied channels.
* Select an appropriate frequency.
* Compare channel widths.
* Observe interference.
* Measure performance changes.

### 🖆 Signal Analysis Lab

* Measure signal strength.
* Measure noise floor.
* Calculate SNR.
* Adjust antenna alignment.
* Compare signal quality.
* Observe throughput changes.

### 🖆 Wireless Troubleshooting Lab

Introduce controlled problems into the wireless environment.

* Configure an incorrect passphrase.
* Select an incompatible frequency.
* Introduce channel interference.
* Reduce signal strength.
* Misconfigure bridge membership.
* Diagnose and resolve each problem.

# 🗁 Suggested Wireless Topology

Use this topology to practice a point-to-point wireless connection.

```mermaid
flowchart LR
    LAN1[Office LAN]
    R1[MikroTik AP]
    RF[Wireless Link]
    R2[MikroTik Station]
    LAN2[Remote LAN]

    LAN1 --- R1
    R1 --- RF
    RF --- R2
    R2 --- LAN2
```

Example addressing plan:

| Device | Interface | IP Address |
| --- | --- | --- |
| Main Router | LAN | 192.168.10.1/24 |
| Main Wireless Radio | Management | 192.168.10.2/24 |
| Remote Wireless Radio | Management | 192.168.10.3/24 |
| Remote Router | LAN | 192.168.20.1/24 |

The management network and client networks should be designed according to whether the wireless connection operates as a transparent bridge or a routed link.

# 🗁 Examination Preparation

Prepare for MTCWE by combining theoretical wireless knowledge with practical configuration.

Recommended preparation activities:

* Review wireless networking fundamentals.
* Understand IEEE 802.11 standards.
* Study radio frequency and channel behavior.
* Understand antenna characteristics.
* Practice access point configuration.
* Practice wireless client configuration.
* Understand wireless security.
* Practice wireless bridging.
* Study signal analysis and interference.
* Troubleshoot real wireless connectivity problems.
* Review the current official MikroTik MTCWE course outline.

# 🗁 Recommended Study Resources

* [MikroTik Official Training](https://mikrotik.com/training)
* [MikroTik RouterOS Documentation](https://help.mikrotik.com/docs/)
* [MikroTik Wireless Documentation](https://help.mikrotik.com/docs/)
* [IEEE 802.11 Working Group](https://www.ieee802.org/11/)
* [MikroTik Community Forum](https://forum.mikrotik.com/)

# 🗁 Completion Checklist

* [ ] Understand wireless networking fundamentals.
* [ ] Understand IEEE 802.11 standards.
* [ ] Understand radio frequency concepts.
* [ ] Understand wireless channels and channel widths.
* [ ] Understand signal strength and SNR.
* [ ] Understand wireless signal propagation.
* [ ] Understand antenna types and characteristics.
* [ ] Understand MikroTik wireless architecture.
* [ ] Configure wireless access points.
* [ ] Configure wireless clients.
* [ ] Understand wireless security protocols.
* [ ] Understand wireless bridging and WDS.
* [ ] Design point-to-point wireless links.
* [ ] Understand wireless channel planning.
* [ ] Optimize wireless performance.
* [ ] Understand wireless roaming.
* [ ] Monitor wireless interfaces and clients.
* [ ] Troubleshoot wireless connectivity.
* [ ] Complete practical wireless lab exercises.
* [ ] Review official examination requirements.

# 🗁 Final Goal

By completing this roadmap, you should be able to plan, deploy, configure, secure, and troubleshoot MikroTik wireless networks.

You should understand how radio frequencies, antennas, channels, wireless protocols, and signal quality affect network performance.

The ultimate goal is to develop the practical wireless engineering skills required to build reliable indoor, outdoor, point-to-point, and point-to-multipoint wireless networks.
