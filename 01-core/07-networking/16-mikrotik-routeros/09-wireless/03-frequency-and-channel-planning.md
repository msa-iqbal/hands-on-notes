# Frequency and Channel Planning

Wireless frequency and channel planning determine how MikroTik access points use radio spectrum to provide reliable Wi-Fi connectivity.

Poor frequency planning can cause interference, reduced throughput, unstable connections, and weak wireless performance.

A proper wireless channel plan considers frequency bands, channel width, regulatory restrictions, nearby access points, signal propagation, and client device capabilities.

# 🗁 Understanding Wireless Frequency

Wireless communication uses radio frequencies to transmit and receive data.

Wi-Fi commonly operates in these frequency bands:

| Frequency Band | Characteristics | Common Usage |
| --- | --- | --- |
| 2.4 GHz | Longer range, better wall penetration, more interference | General Wi-Fi connectivity |
| 5 GHz | More channels, higher potential throughput, shorter range | High-performance Wi-Fi |
| 6 GHz | Additional spectrum and wider channel availability | Modern Wi-Fi 6E and Wi-Fi 7 devices |

The available frequency bands depend on the wireless hardware, regulatory domain, RouterOS version, and supported wireless standards.

# 🗁 Understanding the 2.4 GHz Band

The 2.4 GHz band is widely supported by wireless devices.

Characteristics:

* Better propagation through walls than 5 GHz in many environments.
* Longer practical coverage in typical indoor deployments.
* More susceptible to interference.
* Limited non-overlapping channel availability.
* Shared with Bluetooth and other radio technologies.

Common channel center frequencies include:

| Channel | Center Frequency |
| --- | ---: |
| 1 | 2412 MHz |
| 2 | 2417 MHz |
| 3 | 2422 MHz |
| 4 | 2427 MHz |
| 5 | 2432 MHz |
| 6 | 2437 MHz |
| 7 | 2442 MHz |
| 8 | 2447 MHz |
| 9 | 2452 MHz |
| 10 | 2457 MHz |
| 11 | 2462 MHz |
| 12 | 2467 MHz |
| 13 | 2472 MHz |

Channel availability varies by country and regulatory domain.

# 🗁 Non-Overlapping Channels in 2.4 GHz

In a typical 20 MHz channel-width deployment, channels 1, 6, and 11 are commonly used as a non-overlapping channel plan.

```text
2.4 GHz Channel Layout

Channel 1       Channel 6       Channel 11
   |                 |                |
 [20 MHz]          [20 MHz]         [20 MHz]
   |                 |                |
2412 MHz          2437 MHz         2462 MHz
```

This arrangement is commonly used in regions where those channels are permitted.

Some regulatory domains allow additional channels, and alternative plans may be appropriate depending on the local spectrum environment.

Avoid assigning nearby access points overlapping channels when a cleaner channel plan is available.

# 🗁 Understanding the 5 GHz Band

The 5 GHz band provides more channel choices than 2.4 GHz in many regulatory domains.

Characteristics:

* Higher potential throughput.
* More available channel combinations.
* Less crowded than 2.4 GHz in many environments.
* Shorter practical coverage in some indoor environments.
* Some channels require Dynamic Frequency Selection (DFS).

The 5 GHz band is commonly used for:

* Office wireless networks.
* High-density access point deployments.
* Wireless point-to-point links.
* High-throughput client connectivity.

Actual channel availability depends on the country, hardware, and regulatory restrictions.

# 🗁 Understanding the 6 GHz Band

The 6 GHz band provides additional spectrum for modern Wi-Fi technologies.

It is associated with Wi-Fi 6E and Wi-Fi 7 devices.

Characteristics:

* Additional channel capacity.
* Support for wider channel widths.
* Less congestion in many deployments.
* Shorter practical range than lower-frequency bands under comparable conditions.
* Requires compatible access points and client devices.

Not all MikroTik devices support 6 GHz operation. Regulatory permissions also vary significantly between countries.

# 🗁 Understanding Wireless Channels

A wireless channel is a defined portion of radio spectrum used for communication.

Each Wi-Fi channel has a center frequency and occupies a portion of the available spectrum.

For example:

* Channel 1 in 2.4 GHz has a center frequency of 2412 MHz.
* Channel 6 has a center frequency of 2437 MHz.
* Channel 11 has a center frequency of 2462 MHz.

Nearby access points using overlapping channels can interfere with each other.

Channel selection should consider both the center frequency and channel width.

# 🗁 Understanding Channel Width

Channel width determines how much radio spectrum a wireless connection occupies.

Common channel widths include:

| Channel Width | Characteristics |
| --- | --- |
| 20 MHz | Better channel reuse and generally less interference |
| 40 MHz | Higher potential throughput, greater spectrum usage |
| 80 MHz | Higher throughput, fewer independent channel blocks |
| 160 MHz | Very high potential throughput, requires substantial clean spectrum |
| 320 MHz | Supported by Wi-Fi 7 hardware in compatible 6 GHz deployments |

Wider channels can increase the maximum physical-layer data rate, but they do not guarantee higher real-world throughput.

A wider channel may overlap with more neighboring networks and reduce the number of independent channels available for other access points.

# 🗁 Selecting Channel Width

Choose channel width according to the deployment environment.

| Environment | Suggested Approach |
| --- | --- |
| Crowded 2.4 GHz environment | 20 MHz |
| General 5 GHz deployment | 20 or 40 MHz |
| Low-interference 5 GHz environment | 40 or 80 MHz |
| High-density wireless deployment | Prefer narrower channels for reuse |
| Modern 6 GHz deployment | Select width based on spectrum availability and client support |

For multi-access-point deployments, narrower channels can improve frequency reuse and reduce co-channel contention.

# 🗁 Understanding Channel Interference

Wireless interference occurs when unwanted radio transmissions affect communication quality.

Common sources include:

* Nearby Wi-Fi access points.
* Bluetooth devices.
* Microwave ovens.
* Wireless cameras.
* Other radio transmitters.
* Poorly planned wireless links.

Two important Wi-Fi interference conditions are:

**Co-channel interference**

Multiple access points use the same channel.

They share the available airtime and must coordinate access to the medium when they can detect each other.

**Adjacent-channel interference**

Nearby access points use partially overlapping channels.

This can cause interference when transmissions overlap in frequency.

A good channel plan reduces unnecessary overlap and improves airtime efficiency.

# 🗁 Understanding DFS Channels

DFS stands for Dynamic Frequency Selection.

Some 5 GHz channels are shared with radar systems and require wireless devices to detect radar activity.

When radar is detected, the access point must stop using the affected channel and move to another permitted channel according to regulatory requirements.

DFS behavior can cause:

* Channel changes.
* Temporary wireless interruptions.
* Delayed AP startup while checking for radar.
* Client reconnection events.

DFS channel availability and requirements depend on the regulatory domain.

# 🗁 Understand Regulatory Domains

Wireless devices must operate within the radio regulations of their country.

Regulatory settings may control:

* Permitted frequency ranges.
* Allowed channels.
* Maximum transmit power.
* Indoor and outdoor operation.
* DFS requirements.
* Other radio transmission restrictions.

For Bangladesh deployments, select Bangladesh when it is available as a supported country setting in the applicable wireless configuration.

Do not select another country simply to unlock additional channels or transmit power.

For legacy wireless interfaces, inspect the available regulatory settings:

```routeros
/interface wireless info country-info Bangladesh
```

For the newer WiFi package, inspect the supported country and configuration properties through the WiFi menus.

# 🗁 Configure Frequency on Legacy Wireless

The following example uses the legacy Wireless package.

Configure the 2.4 GHz access point to use channel 6:

```routeros
/interface wireless
set wlan1 frequency=2437 band=2ghz-b/g/n channel-width=20mhz
```

Configure a 5 GHz access point using an example frequency:

```routeros
/interface wireless
set wlan2 frequency=5180 band=5ghz-a/n/ac channel-width=20/40mhz-Ce
```

The selected frequency and channel width must be supported by the hardware and permitted by the regulatory configuration.

Do not assume that every frequency is available on every device.

# 🗁 Configure Country and Installation Settings

For legacy wireless interfaces:

```routeros
/interface wireless
set wlan1 country=Bangladesh installation=indoor
```

The installation setting helps apply the appropriate regulatory behavior for the selected deployment type.

Use `outdoor` only when the access point is actually deployed outdoors and the selected regulatory domain supports the intended operation.

For the WiFi package, configure the country through the WiFi configuration profile where supported.

# 🗁 Scan Available Wireless Networks

A wireless scan helps identify nearby access points and their channel usage.

For legacy wireless interfaces:

```routeros
/interface wireless
scan wlan1
```

A scan may show:

* SSID.
* BSSID.
* Frequency.
* Signal strength.
* Wireless capabilities.

The scan temporarily uses the radio for scanning and may interrupt normal operation on the interface being scanned.

For production networks, perform scans during an appropriate maintenance window or use a separate radio for monitoring when available.

# 🗁 Monitor Wireless Interface Conditions

For legacy wireless interfaces:

```routeros
/interface wireless monitor wlan1
```

Inspect detailed interface information:

```routeros
/interface wireless print detail
```

Check connected wireless clients:

```routeros
/interface wireless registration-table print detail
```

Useful metrics include:

* Operating frequency.
* Channel width.
* Signal strength.
* Transmit and receive rates.
* Client connection status.
* Signal-to-noise ratio where reported.

For newer WiFi interfaces, use the monitoring and registration information exposed by the installed WiFi package.

# 🗁 Plan Channels for Multiple Access Points

A multi-access-point deployment requires coordinated channel assignment.

Example 2.4 GHz plan:

| Access Point | Channel | Channel Width |
| --- | ---: | ---: |
| AP-01 | 1 | 20 MHz |
| AP-02 | 6 | 20 MHz |
| AP-03 | 11 | 20 MHz |
| AP-04 | 1 | 20 MHz |

Example 5 GHz planning:

| Access Point | Channel Assignment |
| --- | --- |
| AP-01 | Permitted low-band channel |
| AP-02 | Separate permitted channel block |
| AP-03 | Another available channel block |
| AP-04 | Reuse a channel where physical separation permits |

The actual channel plan should be based on a site survey, regulatory restrictions, and available spectrum.

Do not assign channels solely according to access point numbering.

# 🗁 Wireless Channel Planning Best Practices

* Perform a wireless survey before deploying access points.
* Use 20 MHz channels in crowded 2.4 GHz environments.
* Prefer channels 1, 6, and 11 where permitted for typical 2.4 GHz deployments.
* Use 5 GHz when clients and coverage requirements support it.
* Avoid unnecessarily wide channels in high-density environments.
* Check DFS requirements before selecting 5 GHz channels.
* Select the correct regulatory country.
* Avoid excessive transmit power.
* Coordinate channel assignments between nearby access points.
* Monitor interference and client signal quality after deployment.
* Reassess channel assignments when the wireless environment changes.

# 🗁 Troubleshooting Frequency and Channel Problems

**Access point does not start on the selected frequency**

Possible causes:

* Frequency is not supported by the hardware.
* Channel is not permitted by the regulatory domain.
* Channel width is unsupported.
* DFS requirements prevent immediate operation.
* Wireless driver limitations.

**Wireless clients cannot discover the network**

Possible causes:

* Client devices do not support the selected band.
* The access point is using an unsupported channel.
* SSID broadcasting is disabled.
* Regulatory restrictions prevent the selected frequency.

**Wireless performance is poor**

Possible causes:

* Co-channel interference.
* Adjacent-channel interference.
* Excessive channel width.
* Weak signal strength.
* High client density.
* Poor access point placement.

**Connection interruptions occur on DFS channels**

Possible causes:

* Radar detection.
* Automatic channel changes.
* Client devices taking time to reconnect.
* Inconsistent DFS support between access points and clients.

# 🗁 Summary

Frequency and channel planning are essential for reliable wireless communication.

Important concepts include:

* 2.4 GHz, 5 GHz, and 6 GHz frequency bands.
* Channel numbers and center frequencies.
* Channel width.
* Co-channel and adjacent-channel interference.
* DFS operation.
* Regulatory domains.
* Wireless scanning and monitoring.
* Multi-access-point channel planning.

A good channel plan balances coverage, throughput, spectrum availability, and interference rather than simply selecting the widest available channel.
