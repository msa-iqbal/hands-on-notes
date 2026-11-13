# Signal Strength and Interference

Wireless signal strength and interference are important factors that determine Wi-Fi performance, reliability, coverage, and connection stability.

A wireless device may successfully connect to an access point but still experience slow speeds, high latency, packet loss, or frequent disconnections.

Understanding signal strength, signal quality, noise, and interference helps network engineers design reliable wireless networks and troubleshoot connectivity problems.

# 🗁 Wireless Signal Strength

Wireless signal strength represents the power level of a received radio signal.

It is commonly measured in **dBm (decibels relative to one milliwatt)**.

The dBm scale is logarithmic. A value closer to zero represents a stronger received signal.

| Signal Strength | General Interpretation |
| --- | --- |
| -30 dBm | Extremely strong |
| -40 dBm | Excellent |
| -50 dBm | Very good |
| -60 dBm | Good |
| -67 dBm | Generally suitable for reliable voice and data |
| -70 dBm | Moderate |
| -75 dBm | Weak |
| -80 dBm | Very weak |
| -90 dBm | Extremely weak |

These values are general guidelines, not universal performance thresholds. Actual usability depends on noise, channel utilization, modulation, application requirements, and device capabilities.

# 🗁 Understanding dBm

dBm measures power relative to 1 milliwatt.

The mathematical relationship is:

\[
P_{\text{dBm}} = 10\log_{10}\left(\frac{P_{\text{mW}}}{1\text{ mW}}\right)
\]

Examples:

| Power | dBm |
| ---: | ---: |
| 1 mW | 0 dBm |
| 10 mW | 10 dBm |
| 100 mW | 20 dBm |
| 1000 mW | 30 dBm |
| 0.1 mW | -10 dBm |
| 0.01 mW | -20 dBm |

Because dBm is logarithmic:

* Increasing power by 3 dB approximately doubles power.
* Increasing power by 10 dB multiplies power by 10.
* Decreasing power by 10 dB reduces power to one-tenth.

# 🗁 Signal-to-Noise Ratio (SNR)

Signal-to-Noise Ratio (SNR) compares the received signal power with the background noise power.

It is expressed in decibels (dB).

\[
SNR_{\text{dB}} = Signal_{\text{dBm}} - Noise_{\text{dBm}}
\]

Example:

```text
Received Signal = -55 dBm
Noise Level     = -90 dBm

SNR = -55 - (-90)
SNR = 35 dB
```

The SNR is 35 dB.

A higher SNR generally allows a wireless device to use more efficient modulation and coding schemes.

| SNR | General Interpretation |
| --- | --- |
| 40 dB or higher | Excellent |
| 25–40 dB | Very good |
| 20–25 dB | Good |
| 15–20 dB | Fair |
| 10–15 dB | Poor |
| Below 10 dB | Very poor |

These are approximate guidelines. Required SNR varies with the modulation and coding scheme, channel width, receiver implementation, and environmental conditions.

# 🗁 Noise Floor

The noise floor is the background radio-frequency energy detected by a wireless receiver.

It includes thermal noise and other unwanted radio-frequency energy.

Common sources include:

* Other wireless transmitters.
* Electronic equipment.
* Radio-frequency interference.
* Nearby access points.
* Environmental noise.

A lower noise floor is generally better.

For example:

```text
Signal:    -60 dBm
Noise:     -95 dBm
SNR:        35 dB
```

If the noise increases:

```text
Signal:    -60 dBm
Noise:     -80 dBm
SNR:        20 dB
```

The received signal strength remains unchanged, but the SNR decreases because the noise level has increased.

# 🗁 Types of Wireless Interference

Wireless interference occurs when unwanted radio-frequency energy affects communication.

Common types include:

**Co-Channel Interference**

Co-channel interference occurs when multiple wireless networks use the same channel.

Wi-Fi devices sharing a channel generally coordinate access using carrier sensing and contention mechanisms.

As the number of active devices increases, available airtime becomes more heavily shared.

**Adjacent-Channel Interference**

Adjacent-channel interference occurs when nearby channels overlap or when transmitters produce energy outside their intended channel boundaries.

It can reduce wireless performance and increase retransmissions.

**Non-Wi-Fi Interference**

Other radio-frequency devices can interfere with Wi-Fi communication.

Examples include:

* Microwave ovens.
* Bluetooth devices.
* Wireless video transmitters.
* Cordless devices.
* Other equipment operating in overlapping frequency ranges.

**Physical Obstructions**

Physical objects can weaken or obstruct wireless signals.

Common examples include:

* Concrete walls.
* Metal doors.
* Furniture.
* Glass with metallic coatings.
* Buildings.
* Trees and vegetation.

Different materials attenuate radio signals differently.

# 🗁 Co-Channel and Adjacent-Channel Interference

Understanding channel relationships is important when designing wireless networks.

```text
Co-Channel Interference:

AP 1  ───────── Channel 36
AP 2  ───────── Channel 36

Adjacent-Channel Interference:

AP 1  ───────── Channel 36
AP 2  ───────── Overlapping Channel
```

Co-channel networks may coordinate through Wi-Fi medium-access mechanisms, but they still compete for airtime.

Adjacent-channel interference can be particularly problematic because overlapping transmissions may not coordinate as effectively.

# 🗁 Signal Attenuation

Signal attenuation is the reduction of radio signal power as it travels through an environment.

It can occur because of:

* Distance.
* Physical obstacles.
* Absorption.
* Reflection.
* Diffraction.
* Scattering.

Free-space path loss describes the reduction in received signal power as distance increases in an ideal unobstructed environment.

The free-space path loss equation is:

\[
FSPL(dB) = 32.44 + 20\log_{10}(f_{\text{MHz}}) + 20\log_{10}(d_{\text{km}})
\]

Where:

* \(f\) = Frequency in MHz.
* \(d\) = Distance in kilometers.

For the same distance, a higher frequency generally experiences greater free-space path loss.

Real wireless environments also include walls, reflections, interference, and antenna characteristics.

# 🗁 Frequency and Signal Propagation

Wi-Fi commonly operates in the 2.4 GHz, 5 GHz, and 6 GHz frequency bands.

| Frequency Band | General Characteristics |
| --- | --- |
| 2.4 GHz | Longer propagation range in many environments, fewer non-overlapping channels, more interference sources |
| 5 GHz | More channel options, generally less congestion in many environments, shorter range than 2.4 GHz under comparable conditions |
| 6 GHz | Additional spectrum and wider-channel opportunities, but higher-frequency propagation limitations |

Actual coverage depends on transmit power, receiver sensitivity, antenna design, regulatory limits, channel width, and environmental conditions.

# 🗁 Fresnel Zone

The Fresnel zone is an ellipsoidal region surrounding the direct radio path between two antennas.

Obstacles within this region can cause diffraction and reduce signal quality.

This is especially important for outdoor point-to-point and point-to-multipoint wireless links.

A common planning target is to maintain at least 60% clearance of the first Fresnel zone.

The approximate first Fresnel zone radius is:

\[
r_1 = 17.32\sqrt{\frac{d_1d_2}{fD}}
\]

Where:

* \(r_1\) = First Fresnel zone radius in meters.
* \(d_1\) = Distance from the first endpoint in kilometers.
* \(d_2\) = Distance from the obstacle to the second endpoint in kilometers.
* \(D\) = Total link distance in kilometers.
* \(f\) = Frequency in GHz.

The maximum first Fresnel zone radius occurs approximately halfway along the path.

# 🗁 Modulation and Coding Scheme (MCS)

The Modulation and Coding Scheme determines how data is encoded and transmitted over a wireless channel.

Higher MCS levels generally provide higher theoretical data rates but require better signal conditions.

Common modulation techniques include:

* BPSK.
* QPSK.
* 16-QAM.
* 64-QAM.
* 256-QAM.
* 1024-QAM.
* 4096-QAM.

Higher-order modulation carries more bits per symbol but is more sensitive to noise and interference.

Wi-Fi standards also use different coding rates and spatial-stream configurations.

The selected MCS depends on link conditions and device capabilities.

# 🗁 RSSI vs SNR

RSSI and SNR describe different aspects of wireless reception.

| Metric | Meaning | What It Indicates |
| --- | --- | --- |
| RSSI | Received Signal Strength Indicator | Received signal level |
| SNR | Signal-to-Noise Ratio | Signal quality relative to noise |
| Noise Floor | Background radio-frequency energy | Unwanted signal environment |

RSSI values are often displayed in dBm, but RSSI itself is an implementation-dependent indicator and may be reported differently by hardware vendors.

A strong RSSI does not necessarily indicate a good connection if the noise floor is also high.

# 🗁 Channel Utilization

Channel utilization represents how much of the available wireless airtime is occupied.

High channel utilization can reduce available airtime for other devices.

For example:

```text
Channel Utilization: 20%
Available Airtime:   Approximately 80%

Channel Utilization: 80%
Available Airtime:   Approximately 20%
```

These figures are simplified. Actual usable airtime also depends on protocol overhead, contention, retransmissions, and measurement methods.

Channel utilization can increase because of:

* Many active wireless clients.
* Nearby access points.
* High-bandwidth applications.
* Slow clients occupying airtime.
* Retransmissions caused by poor signal quality.

# 🗁 Wireless Performance Optimization

Several practices can improve wireless performance.

**Access Point Placement**

* Install access points in suitable central locations.
* Avoid placing access points inside cabinets.
* Keep antennas away from large metal objects.
* Consider building materials and floor layouts.
* Use additional access points when coverage requires them.

**Channel Planning**

* Select channels based on the local radio environment.
* Avoid unnecessary overlapping channel assignments.
* Use channel widths appropriate for the available spectrum.
* Consider dynamic channel selection where supported.

**Transmit Power**

* Use appropriate transmit power.
* Avoid unnecessarily high transmit power.
* Balance access point coverage with client transmit capabilities.

Increasing access point transmit power alone does not solve weak client uplinks.

**Client Distribution**

* Distribute clients across available access points.
* Consider airtime utilization and client density.
* Use band steering where supported and appropriate.

**Wireless Security**

* Use WPA2-AES or WPA3.
* Disable obsolete security protocols.
* Use appropriate management-frame protection.

# 🗁 Wireless Signal Monitoring on Linux

Linux provides several tools for inspecting wireless signal conditions.

**Display Wireless Interface Information**

```bash
iw dev
```

**Inspect the Current Wireless Connection**

```bash
iw dev wlan0 link
```

Typical output includes:

```text
Connected to aa:bb:cc:dd:ee:ff
SSID: Office-WiFi
freq: 5180
signal: -55 dBm
tx bitrate: 866.7 MBit/s
```

**Inspect Nearby Wireless Networks**

```bash
sudo iw dev wlan0 scan
```

**Display Wireless Device Statistics**

```bash
iw dev wlan0 station dump
```

**List Available Wireless Networks**

```bash
nmcli device wifi list
```

**Monitor Wireless Interface Statistics**

```bash
ip -s link show wlan0
```

The available metrics depend on the wireless chipset, driver, and interface mode.

# 🗁 MikroTik Wireless Monitoring

MikroTik RouterOS provides wireless monitoring tools for inspecting signal conditions and connected clients.

**Inspect Wireless Interfaces**

```routeros
/interface wifi print
```

**Monitor WiFi Interface**

```routeros
/interface wifi monitor wifi1
```

**Inspect Connected WiFi Clients**

```routeros
/interface wifi registration-table print
```

The newer WiFi package uses `/interface wifi` commands. Devices using the legacy wireless package have different commands, such as:

```routeros
/interface wireless monitor wlan1
/interface wireless registration-table print
```

Useful monitoring information includes:

* Signal strength.
* Signal-to-noise ratio, where exposed.
* Transmit and receive rates.
* Frequency and channel width.
* Connected client information.
* Connection uptime.

# 🗁 Wireless Interference Troubleshooting

| Symptom | Possible Cause | Recommended Investigation |
| --- | --- | --- |
| Weak signal | Excessive distance or physical obstruction | Inspect RSSI and access point placement |
| Strong signal but poor performance | High noise or channel utilization | Inspect SNR and channel occupancy |
| Frequent disconnections | Unstable signal or interference | Review wireless logs and link statistics |
| Low throughput | Low MCS, retransmissions, or congestion | Inspect negotiated rates and airtime |
| Different performance between devices | Client hardware or antenna differences | Compare device capabilities and signal conditions |
| Outdoor link instability | Fresnel obstruction or antenna misalignment | Inspect the radio path and antenna alignment |
| Poor roaming | Incorrect AP placement or client behavior | Review coverage overlap and roaming configuration |

# 🗁 Key Takeaways

* Signal strength describes received radio power and is commonly measured in dBm.
* A signal closer to 0 dBm is generally stronger.
* SNR measures the difference between signal power and noise power.
* A strong signal does not guarantee good wireless performance.
* Co-channel interference increases airtime competition.
* Adjacent-channel interference can disrupt wireless communication.
* Physical obstacles and distance cause signal attenuation.
* The Fresnel zone is important for outdoor wireless links.
* Higher MCS levels require suitable signal quality.
* Channel utilization affects available wireless airtime.
* Proper access point placement, channel planning, and transmit power improve wireless reliability.
