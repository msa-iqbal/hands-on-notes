# Wireless Frequency Bands

Wireless frequency bands are ranges of electromagnetic frequencies used to transmit and receive data through the air.

Wi-Fi networks operate within designated radio-frequency bands. The frequency band determines the spectrum available for wireless communication and influences coverage, signal propagation, channel availability, and network performance.

The primary Wi-Fi frequency bands are:

* 2.4 GHz
* 5 GHz
* 6 GHz

Some newer Wi-Fi technologies and regional regulatory frameworks also support operation in the 60 GHz band.

# 🗁 Understanding Frequency

Frequency measures how many times an electromagnetic wave completes one cycle per second.

The unit of frequency is Hertz (Hz).

| Unit | Equivalent |
| --- | ---: |
| 1 Hz | 1 cycle per second |
| 1 kHz | 1,000 Hz |
| 1 MHz | 1,000,000 Hz |
| 1 GHz | 1,000,000,000 Hz |

For example:

* 2.4 GHz represents approximately 2.4 billion cycles per second.
* 5 GHz represents approximately 5 billion cycles per second.
* 6 GHz represents approximately 6 billion cycles per second.

Frequency is not the same as data transmission speed. A higher operating frequency does not automatically mean a faster wireless connection.

# 🗁 Relationship Between Frequency and Wavelength

Frequency and wavelength are inversely proportional.

The wavelength of an electromagnetic wave can be calculated using:

\[
\lambda = \frac{c}{f}
\]

Where:

* \(\lambda\) = Wavelength in meters
* \(c\) = Speed of light, approximately \(3 \times 10^8\) meters per second
* \(f\) = Frequency in Hertz

As frequency increases, wavelength decreases.

Approximate wavelengths:

| Frequency | Wavelength |
| --- | ---: |
| 2.4 GHz | 12.5 cm |
| 5 GHz | 6 cm |
| 6 GHz | 5 cm |
| 60 GHz | 0.5 cm |

Shorter wavelengths generally experience greater attenuation through many building materials and obstacles under comparable conditions.

# 🗁 2.4 GHz Frequency Band

The 2.4 GHz band is one of the oldest and most widely supported Wi-Fi frequency bands.

It provides relatively good propagation through walls and other obstacles compared with higher Wi-Fi bands under similar conditions.

**Characteristics**

* Frequency range: approximately 2.400–2.4835 GHz, subject to regional regulations.
* Longer practical coverage in many indoor environments.
* Better propagation through obstacles than 5 GHz and 6 GHz under comparable conditions.
* Fewer non-overlapping channel options.
* More susceptible to interference from other devices.
* Commonly used by older Wi-Fi devices and IoT equipment.

**Common Interference Sources**

* Bluetooth devices
* Microwave ovens
* Wireless cameras
* Other Wi-Fi networks
* Certain cordless phones
* Other devices operating in the 2.4 GHz band

**Common 2.4 GHz Channels**

In many regulatory domains, channels 1–13 are available, while some regions, including the United States, generally permit channels 1–11.

Commonly used non-overlapping 20 MHz channels are:

* Channel 1
* Channel 6
* Channel 11

These channels are commonly used because their 20 MHz channel allocations do not overlap each other.

# 🗁 5 GHz Frequency Band

The 5 GHz band provides more channel options than the 2.4 GHz band in many regulatory domains.

It supports higher channel widths and can provide greater wireless capacity.

**Characteristics**

* Frequency range: approximately 5 GHz, with permitted channels varying by region.
* More available channels than 2.4 GHz in many regions.
* Supports 20, 40, 80, and 160 MHz channel widths where permitted.
* Generally experiences less congestion than 2.4 GHz in many environments.
* Shorter coverage through obstacles under comparable conditions.
* Some channels require Dynamic Frequency Selection (DFS).

**Dynamic Frequency Selection (DFS)**

DFS allows certain 5 GHz Wi-Fi channels to share spectrum with protected radar systems.

An access point operating on a DFS channel must comply with regulatory requirements, which can include:

* Checking for radar signals before transmission.
* Monitoring the channel for radar activity.
* Changing channels when radar is detected.
* Observing channel availability restrictions.

DFS behavior depends on the regulatory domain and device implementation.

# 🗁 6 GHz Frequency Band

The 6 GHz band provides additional spectrum for newer Wi-Fi technologies.

It is supported by Wi-Fi 6E and Wi-Fi 7 devices, subject to regional regulations.

**Characteristics**

* Provides additional spectrum beyond traditional 2.4 GHz and 5 GHz Wi-Fi.
* Supports wider channels, including 80, 160, and 320 MHz where permitted.
* Offers more opportunities for high-capacity wireless deployments.
* Generally has shorter practical indoor coverage through obstacles than lower-frequency bands.
* Requires compatible client devices and access points.
* Availability and permitted transmit power vary by country.

**Wi-Fi 6E vs Wi-Fi 7**

* Wi-Fi 6E extends Wi-Fi 6 operation into the 6 GHz band.
* Wi-Fi 7 introduces additional capabilities, including support for 320 MHz channels and Multi-Link Operation (MLO), where supported.

# 🗁 60 GHz Frequency Band

The 60 GHz band is used by IEEE 802.11ad and IEEE 802.11ay technologies.

It provides very high-frequency wireless communication and supports high-throughput, short-range applications.

**Characteristics**

* Very wide available spectrum.
* Supports extremely high data rates.
* Shorter practical communication range.
* Significant attenuation through walls and other obstacles.
* Often requires relatively clear propagation paths.

This band is useful for specialized high-capacity wireless links and short-range communication.

# 🗁 Frequency Band Comparison

| Feature | 2.4 GHz | 5 GHz | 6 GHz | 60 GHz |
| --- | --- | --- | --- | --- |
| Coverage | Generally longer | Moderate | Generally shorter | Very short |
| Wall Penetration | Better | Lower | Lower | Very limited |
| Interference | Often high | Varies | Often lower | Environment-dependent |
| Channel Availability | Limited | Greater | Extensive where permitted | Extensive spectrum |
| Channel Width | Commonly 20 MHz | Up to 160 MHz where supported | Up to 320 MHz with Wi-Fi 7 where permitted | Very wide channels |
| Typical Use | General connectivity and IoT | High-capacity Wi-Fi | Modern high-capacity Wi-Fi | Specialized short-range links |

Actual performance depends on regulatory limits, channel selection, transmit power, antenna characteristics, obstacles, interference, and client capabilities.

# 🗁 Frequency Bands and Wireless Performance

Frequency affects wireless propagation, but it is only one factor in network performance.

Important factors include:

**Signal Attenuation**

Signal strength decreases as the radio signal travels through distance and obstacles.

Higher frequencies generally experience greater free-space path loss over the same distance when transmit power and antenna gains are held constant.

**Channel Width**

A wider channel can provide greater theoretical data throughput because it uses more radio spectrum.

However, wider channels also consume more available spectrum and may increase interference or reduce the number of independently usable channels.

**Interference**

Wireless interference can reduce signal quality and increase retransmissions.

A frequency band with less congestion may provide better practical performance even if its nominal capabilities are lower.

**Client Compatibility**

A wireless access point and client must support a compatible frequency band and channel configuration.

For example, a device supporting only 2.4 GHz cannot connect to a network operating exclusively on 5 GHz or 6 GHz.

# 🗁 Dual-Band and Tri-Band Wi-Fi

Wireless access points and routers can support multiple frequency bands.

**Single-Band**

Operates on one frequency band, commonly 2.4 GHz.

**Dual-Band**

Supports both:

* 2.4 GHz
* 5 GHz

**Tri-Band**

Common configurations include:

* 2.4 GHz
* 5 GHz
* 6 GHz

Some tri-band systems instead provide two separate 5 GHz radios alongside a 2.4 GHz radio.

The exact radio configuration depends on the hardware.

# 🗁 Band Steering

Band steering is a wireless access point feature that attempts to encourage compatible clients to use a preferred frequency band.

For example, an access point may encourage a dual-band client to connect through 5 GHz instead of 2.4 GHz.

Potential benefits:

* Better distribution of wireless clients.
* Reduced congestion on the 2.4 GHz band.
* Improved utilization of available radio capacity.

Band steering is generally advisory. The wireless client retains significant control over its connection decisions.

# 🗁 Practical Examples

**Example: Home Wi-Fi Network**

A home network may use:

* 2.4 GHz for IoT devices and devices farther from the access point.
* 5 GHz for laptops, smartphones, and streaming devices.
* 6 GHz for compatible modern devices requiring additional wireless capacity.

**Example: Enterprise Wi-Fi Network**

An enterprise may use:

* 2.4 GHz for legacy devices that require it.
* 5 GHz for general wireless connectivity.
* 6 GHz for compatible high-capacity client devices.

Frequency planning should account for access point placement, channel reuse, client density, and regulatory restrictions.

# 🗁 Key Takeaways

* Wi-Fi uses radio-frequency bands to transmit wireless data.
* The primary Wi-Fi bands are 2.4 GHz, 5 GHz, and 6 GHz.
* The 60 GHz band supports specialized short-range wireless communication.
* Frequency and wavelength are inversely proportional.
* Higher frequency does not automatically mean higher real-world throughput.
* The 2.4 GHz band generally provides better coverage through obstacles but has fewer channel options.
* The 5 GHz band provides additional channel capacity and supports wider channels.
* The 6 GHz band provides additional spectrum for Wi-Fi 6E and Wi-Fi 7.
* DFS is required for certain 5 GHz channels to protect radar systems.
* Channel width, interference, transmit power, and client compatibility all influence wireless performance.
