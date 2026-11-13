# Wireless Troubleshooting

Wireless troubleshooting is the process of identifying and resolving problems that affect Wi-Fi connectivity, performance, reliability, and security.

Unlike wired networks, wireless communication uses radio frequencies. Signal strength, interference, channel utilization, access point configuration, client capabilities, and physical obstacles can all affect connectivity.

Common wireless problems include:

* Devices cannot find the Wi-Fi network.
* Clients cannot authenticate.
* Devices connect but cannot access the Internet.
* Wi-Fi disconnects frequently.
* Wireless speed is significantly lower than expected.
* Signal strength is weak in certain areas.
* Some devices connect while others fail.
* Roaming between access points causes interruptions.
* Wireless clients receive incorrect IP addresses.
* Performance degrades when many devices connect.

# 🗁 Wireless Network Fundamentals

A typical Wi-Fi network contains:

* **Wireless client:** A laptop, smartphone, tablet, or other device connecting through Wi-Fi.
* **Access point (AP):** Provides wireless connectivity and bridges wireless clients to a network.
* **Wireless controller:** Centrally manages multiple access points in controller-based deployments.
* **Router:** Provides Layer 3 routing between networks.
* **DHCP server:** Assigns IP configuration to wireless clients.
* **Authentication server:** May provide centralized authentication through RADIUS.

Example topology:

```text
                 Internet
                    |
                    |
                  Router
                    |
                    |
                 Switch
                    |
                    |
               Access Point
                /       \
               /         \
            Laptop      Phone
```

When troubleshooting, identify whether the failure occurs at the wireless association, authentication, IP configuration, routing, or application layer.

# 🗁 Wi-Fi Frequency Bands

Wi-Fi commonly operates in these frequency bands:

| Band | Characteristics |
| --- | --- |
| 2.4 GHz | Longer propagation range, fewer non-overlapping channels, more interference |
| 5 GHz | More channel options, generally higher capacity, shorter range than 2.4 GHz |
| 6 GHz | Additional spectrum and wider channel options with compatible Wi-Fi 6E or Wi-Fi 7 equipment |

Actual channel availability and permitted transmit power depend on the regulatory domain.

Common causes of frequency-related problems:

* Interference from neighboring networks.
* Incorrect channel selection.
* Unsupported client frequency bands.
* Excessive channel width.
* Physical obstacles.
* Poor access point placement.

# 🗁 Signal Strength and Coverage

Wireless signal strength is commonly measured in dBm.

Typical interpretation:

| Signal Strength | General Interpretation |
| --- | --- |
| -30 dBm | Excellent |
| -50 dBm | Very strong |
| -60 dBm | Good |
| -67 dBm | Often suitable for reliable voice and real-time applications |
| -70 dBm | Moderate |
| -80 dBm | Weak |
| -90 dBm | Very weak |

These values are practical guidelines, not universal performance guarantees. Noise level, interference, client capabilities, and application requirements also matter.

Common coverage problems:

* Access point is too far from the client.
* Walls or metal objects attenuate signals.
* Access point is installed in a poor location.
* Transmit power is too low.
* Antenna orientation is unsuitable.
* The client uses a weaker radio than the access point.

# 🗁 Signal-to-Noise Ratio

Signal-to-Noise Ratio (SNR) measures the difference between the received signal level and the noise floor.

Formula:

```text
SNR = Signal Level - Noise Floor
```

Example:

```text
Signal level: -60 dBm
Noise floor:  -90 dBm

SNR = 30 dB
```

Higher SNR generally provides more reliable wireless communication.

A strong signal does not necessarily mean a good connection. High interference can reduce SNR and cause retransmissions.

# 🗁 Wireless Interference

Interference occurs when unwanted radio-frequency energy affects wireless communication.

Common sources include:

* Neighboring Wi-Fi networks.
* Bluetooth devices.
* Microwave ovens.
* Wireless cameras.
* Cordless devices.
* Poorly shielded electronic equipment.
* Non-Wi-Fi transmitters operating in nearby spectrum.

Symptoms include:

* High latency.
* Packet loss.
* Low throughput.
* Frequent retransmissions.
* Unstable connections.
* Performance changes during certain hours.

Troubleshooting steps:

* Scan nearby wireless networks.
* Inspect channel utilization.
* Compare signal strength and noise.
* Identify overlapping channel usage.
* Test alternative channels.
* Reduce unnecessary channel width.
* Reposition access points when necessary.

# 🗁 Channel Selection Problems

Wi-Fi devices use channels within their supported frequency bands.

In the 2.4 GHz band, channels overlap significantly when configured with wider channel widths.

Channels 1, 6, and 11 are commonly used as non-overlapping 20 MHz channel choices in regions where those channels are permitted.

In 5 GHz and 6 GHz bands, more channel choices are available, although regulatory restrictions and Dynamic Frequency Selection (DFS) requirements may apply.

Common channel problems:

* Multiple nearby access points use the same congested channel.
* Overlapping channels create interference.
* Channel width is unnecessarily large.
* DFS events cause channel changes.
* Client devices do not support the configured channel.

A channel scan should be combined with actual performance testing. The least-used channel is not automatically the best choice in every environment.

# 🗁 Channel Width Problems

Channel width affects the amount of spectrum used by a Wi-Fi transmission.

Common widths include:

* 20 MHz
* 40 MHz
* 80 MHz
* 160 MHz
* 320 MHz, supported by Wi-Fi 7 equipment where permitted

Wider channels can provide higher peak throughput but use more spectrum and may be more vulnerable to congestion or interference.

Common symptoms of excessive channel width:

* Unstable throughput.
* Poor performance in congested environments.
* Reduced channel availability.
* Incompatibility with some clients.

For dense environments, narrower channels may improve overall network capacity and reliability.

# 🗁 Authentication and Association Problems

A wireless client generally goes through several stages before accessing the network:

```text
Discover Network
       |
       v
Authenticate
       |
       v
Associate
       |
       v
Complete Security Handshake
       |
       v
Obtain IP Configuration
       |
       v
Access Network
```

Common causes of authentication failure:

* Incorrect Wi-Fi password.
* Unsupported security mode.
* WPA2/WPA3 compatibility issue.
* Incorrect RADIUS credentials.
* Certificate validation failure.
* Incorrect time on authentication infrastructure.
* Access point or authentication server configuration error.

Determine whether the failure occurs during authentication, association, or the security handshake.

# 🗁 WPA2 and WPA3 Troubleshooting

Wireless security configuration must be compatible with client capabilities.

Common security modes:

| Security Mode | Description |
| --- | --- |
| WPA2-Personal | Uses a pre-shared key |
| WPA2-Enterprise | Uses 802.1X and commonly RADIUS |
| WPA3-Personal | Uses SAE authentication |
| WPA3-Enterprise | Uses enterprise authentication with stronger security options |

Common problems:

* Client does not support the configured security mode.
* Incorrect passphrase.
* Enterprise authentication fails.
* RADIUS server is unreachable.
* Certificate is expired or untrusted.
* Mixed WPA2/WPA3 compatibility causes client-specific issues.

Troubleshooting:

* Verify the access point's security configuration.
* Confirm client compatibility.
* Check authentication logs.
* Verify RADIUS connectivity.
* Validate certificates when enterprise authentication is used.
* Test with a supported client.

Avoid weakening the network's security as the first troubleshooting step.

# 🗁 DHCP Problems in Wireless Networks

A client can successfully connect to an access point but fail to obtain an IP address.

Possible causes:

* DHCP server unavailable.
* DHCP pool exhausted.
* Incorrect VLAN assignment.
* Missing DHCP relay.
* Wireless-to-wired bridge failure.
* DHCP packets blocked by firewall rules.
* Incorrect VLAN tagging on the access point uplink.

On Linux, inspect the client address:

```bash
ip address show
```

Inspect the default route:

```bash
ip route
```

Check DHCP-related network logs:

```bash
journalctl -b | grep -i dhcp
```

A client with an IPv4 address in `169.254.0.0/16` may have self-assigned a link-local address because normal DHCP configuration was unsuccessful.

Verify the access point's bridge or VLAN configuration and the DHCP server's scope for the wireless client network.

# 🗁 Wireless Client Connectivity Problems

A client may associate successfully but still fail to communicate.

Troubleshooting sequence:

* Verify the assigned IP address.
* Check the subnet mask and default gateway.
* Test communication with the gateway.
* Check DNS resolution.
* Test external IP connectivity.
* Inspect VLAN membership.
* Verify access point client isolation settings.
* Inspect firewall policies.

Useful Linux commands:

```bash
ip address show
ip route
ip neigh show
ping -c 4 192.168.1.1
```

Test DNS:

```bash
resolvectl query example.com
```

Test an application service:

```bash
curl -I https://example.com
```

If the client can reach its gateway but not external destinations, investigate routing, NAT, firewall policies, and upstream connectivity.

# 🗁 Access Point Placement Problems

Access point placement has a major effect on wireless coverage.

Poor placement can create:

* Dead zones.
* Weak signals.
* Excessive overlap.
* Hidden-node problems.
* Uneven client distribution.

Placement guidelines:

* Install access points in central locations.
* Avoid enclosing access points in metal cabinets.
* Keep them away from major interference sources.
* Consider wall and floor materials.
* Use appropriate mounting orientation.
* Design coverage for the actual client locations.
* Avoid placing multiple access points too close together without a channel plan.

A coverage survey can identify weak areas more accurately than relying on access point transmit power alone.

# 🗁 Hidden Node Problems

A hidden node occurs when two wireless clients can communicate with an access point but cannot reliably detect each other's transmissions.

Example:

```text
Client A -------- Access Point -------- Client B

Client A cannot directly hear Client B.
Client B cannot directly hear Client A.
```

Both clients may transmit at the same time, causing collisions at the access point.

Symptoms:

* Retransmissions.
* Reduced throughput.
* Unstable performance.
* Poor performance despite acceptable signal levels.

Possible mitigations include:

* Improving access point placement.
* Reducing excessive cell size.
* Adjusting transmit power.
* Using RTS/CTS where appropriate.
* Improving channel planning.

# 🗁 Roaming Problems

Roaming occurs when a wireless client moves from one access point to another within the same wireless network.

The client generally makes the roaming decision, although access points and controllers can influence it.

Common roaming problems:

* Client remains connected to a distant access point.
* Frequent disconnects during movement.
* Authentication delays.
* Inconsistent signal strength.
* Poorly designed access point overlap.
* Incompatible roaming features.

Relevant Wi-Fi mechanisms include:

| Standard | Purpose |
| --- | --- |
| 802.11k | Provides neighbor information |
| 802.11v | Assists with network-guided roaming |
| 802.11r | Enables faster transition between access points |

These features require compatible infrastructure and client support.

Troubleshooting:

* Verify consistent SSID and security configuration.
* Inspect access point coverage overlap.
* Check client roaming behavior.
* Verify authentication infrastructure.
* Test movement between coverage areas.
* Review controller roaming logs when available.

# 🗁 Wireless Client Isolation

Client isolation prevents wireless clients from communicating directly with one another.

It is commonly used in guest networks and public Wi-Fi deployments.

Symptoms:

* Clients can access the Internet but cannot reach other wireless clients.
* Wireless printers are inaccessible.
* Local device discovery fails.
* Peer-to-peer applications do not work.

Check:

* AP isolation.
* Client isolation.
* Guest network policies.
* VLAN separation.
* Firewall rules.

Client isolation may be intentional. Verify the network policy before changing it.

# 🗁 Linux Wireless Diagnostics

Linux provides several tools for inspecting wireless interfaces and connectivity.

**Display wireless interfaces**

```bash
iw dev
```

**Display wireless interface details**

```bash
iw dev wlan0 link
```

**Display nearby networks**

```bash
sudo iw dev wlan0 scan
```

**Display interface status**

```bash
ip link show wlan0
```

**Display wireless statistics**

```bash
iw dev wlan0 station dump
```

**Inspect NetworkManager connections**

```bash
nmcli device status
nmcli connection show
```

**Inspect Wi-Fi device information**

```bash
nmcli device wifi list
```

**Reconnect to a network**

```bash
nmcli device disconnect wlan0
nmcli device connect wlan0
```

Interface names vary by system. Replace `wlan0` with the actual wireless interface name.

# 🗁 Windows Wireless Diagnostics

Windows provides built-in tools for investigating Wi-Fi problems.

**Display wireless interface information**

```powershell
netsh wlan show interfaces
```

**Display available Wi-Fi networks**

```powershell
netsh wlan show networks mode=bssid
```

**Display wireless driver information**

```powershell
netsh wlan show drivers
```

**Generate a wireless network report**

```powershell
netsh wlan show wlanreport
```

The wireless report provides historical connection events and diagnostic information that can help identify authentication and connectivity failures.

Other useful commands:

```powershell
ipconfig /all
route print
ping 192.168.1.1
```

# 🗁 MikroTik RouterOS v7 Wireless Diagnostics

MikroTik wireless configuration depends on the installed wireless package and hardware platform. RouterOS v7 devices may use the legacy wireless menu or the newer WiFi configuration menus.

For devices using the legacy wireless package:

**Display wireless interfaces**

```routeros
/interface wireless print detail
```

**Display registration table**

```routeros
/interface wireless registration-table print detail
```

**Inspect wireless interface status**

```routeros
/interface wireless monitor wlan1
```

For devices using the newer WiFi configuration menus:

**Display WiFi interfaces**

```routeros
/interface wifi print detail
```

**Display connected clients**

```routeros
/interface wifi registration-table print detail
```

**Inspect WiFi interface status**

```routeros
/interface wifi monitor wifi1
```

Available menu paths depend on the installed package and supported device.

Check:

* SSID configuration.
* Operating frequency.
* Channel width.
* Security profile.
* Signal strength.
* Client registration.
* Interface status.
* Country and regulatory settings.
* Wireless bridge and VLAN configuration.

# 🗁 MikroTik Wireless Performance Problems

Poor wireless performance may result from radio configuration or network congestion.

Investigate:

* Channel utilization.
* Interference.
* Signal strength.
* Noise floor.
* SNR.
* Channel width.
* Client transmit capabilities.
* Number of associated clients.
* Ethernet uplink speed.
* CPU utilization.

A wireless link can have excellent signal strength but poor throughput due to interference, retransmissions, or a congested channel.

Do not assume that increasing transmit power will solve every coverage problem. Client devices also have transmit-power limitations.

# 🗁 Wireless Security Problems

Wireless networks can be exposed to unauthorized access and misconfiguration.

Common issues include:

* Weak Wi-Fi passwords.
* Outdated encryption.
* Unauthorized access points.
* Rogue clients.
* Insecure guest networks.
* Exposed management interfaces.
* Misconfigured enterprise authentication.

Recommended practices:

* Use WPA3 where client compatibility permits.
* Use WPA2-AES when WPA3 is unavailable.
* Avoid WEP and obsolete WPA configurations.
* Use strong authentication credentials.
* Separate guest and internal networks.
* Restrict access to management interfaces.
* Monitor connected clients.
* Use enterprise authentication for environments that require centralized identity management.

# 🗁 Packet Capture and Wireless Analysis

Packet captures help investigate wireless authentication, DHCP, DNS, and application connectivity.

On a Linux client:

```bash
sudo tcpdump -ni wlan0
```

Capture DHCP traffic:

```bash
sudo tcpdump -ni wlan0 'udp port 67 or udp port 68'
```

Capture DNS traffic:

```bash
sudo tcpdump -ni wlan0 port 53
```

Capture ICMP traffic:

```bash
sudo tcpdump -ni wlan0 icmp
```

Wireshark can be used to inspect:

* Beacon frames.
* Probe requests and responses.
* Authentication frames.
* Association and reassociation frames.
* DHCP exchanges.
* ARP requests and replies.
* DNS queries.
* TCP retransmissions.

Capturing management and control frames may require a compatible wireless adapter operating in monitor mode.

# 🗁 Practical Troubleshooting Scenarios

**Scenario: Device cannot find the Wi-Fi network**

Check:

* Access point status.
* SSID broadcast configuration.
* Frequency band compatibility.
* Regulatory domain.
* Channel selection.
* Client wireless adapter status.

Possible cause: The access point uses a frequency band or channel unsupported by the client.

**Scenario: Device connects but has no Internet access**

Check:

* Client IP configuration.
* Default gateway.
* DHCP scope.
* Wireless VLAN assignment.
* DNS resolution.
* Router NAT and firewall rules.

Possible cause: The access point maps the client to a VLAN without a working DHCP scope or gateway.

**Scenario: Wi-Fi is slow despite strong signal**

Check:

* SNR.
* Channel utilization.
* Interference.
* Retransmissions.
* Channel width.
* AP uplink speed.
* Number of connected clients.

Possible cause: High channel utilization causes contention and retransmissions.

**Scenario: Wi-Fi disconnects when moving between rooms**

Check:

* Access point coverage overlap.
* Roaming configuration.
* Client roaming behavior.
* Authentication delay.
* Signal thresholds.

Possible cause: The client remains connected to a distant access point instead of roaming to a better one.

**Scenario: Some clients cannot authenticate**

Check:

* Security mode.
* Client compatibility.
* Password or credentials.
* RADIUS availability.
* Certificate validity.
* Authentication logs.

Possible cause: A client does not support the configured WPA3 authentication mode.

# 🗁 Wireless Troubleshooting Checklist

* [ ] Verify the access point is powered and operational.
* [ ] Confirm the SSID is available.
* [ ] Check client wireless adapter status.
* [ ] Verify frequency band compatibility.
* [ ] Inspect channel and channel width.
* [ ] Measure signal strength and SNR.
* [ ] Check channel utilization and interference.
* [ ] Verify authentication and encryption settings.
* [ ] Inspect client association status.
* [ ] Verify DHCP address assignment.
* [ ] Check VLAN membership.
* [ ] Test gateway connectivity.
* [ ] Verify DNS and Internet access.
* [ ] Inspect client isolation settings.
* [ ] Investigate roaming behavior.
* [ ] Check access point uplink performance.
* [ ] Review AP and authentication logs.
* [ ] Capture packets when necessary.
* [ ] Document the root cause and corrective action.

# 🗁 Best Practices

* Perform wireless site surveys before large deployments.
* Select channels based on actual RF conditions.
* Use suitable channel widths for the deployment density.
* Place access points strategically.
* Maintain consistent SSID and security settings where roaming is required.
* Use WPA2-AES or WPA3.
* Separate guest and internal wireless networks.
* Monitor client signal quality and channel utilization.
* Keep access point firmware updated.
* Avoid excessive transmit power.
* Document access point locations and radio settings.
* Test coverage and roaming after configuration changes.

# 🗁 Summary

Wireless troubleshooting requires analyzing both radio-frequency conditions and network connectivity.

The most important areas are:

* Signal strength and SNR.
* Channel selection and interference.
* Authentication and association.
* DHCP and VLAN configuration.
* Access point placement.
* Roaming and client isolation.
* Wireless security.
* Packet captures and client diagnostics.

A reliable investigation identifies whether the problem originates in the radio environment, wireless authentication, client configuration, or the underlying wired network.
