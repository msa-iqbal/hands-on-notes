# Wireless Troubleshooting

Wireless troubleshooting is the process of identifying and resolving problems affecting Wi-Fi connectivity, authentication, signal quality, performance, and network access.

In MikroTik RouterOS, wireless problems can originate from radio configuration, frequency selection, interference, authentication, bridging, VLANs, DHCP, routing, or firewall rules.

A systematic troubleshooting approach helps identify the actual cause instead of changing multiple settings without understanding the problem.

# 🗁 Understanding Wireless Troubleshooting

Wireless connectivity involves several components:

* Wireless hardware and drivers.
* Access point configuration.
* Frequency and channel selection.
* Wireless authentication and encryption.
* Signal strength and radio interference.
* Bridge and VLAN configuration.
* IP addressing and DHCP.
* Routing and firewall policies.

A client may successfully connect to an access point but still fail to access the LAN or internet.

Therefore, troubleshoot each layer independently.

# 🗁 Identify the Wireless Configuration System

Before troubleshooting, identify which wireless configuration system the device uses.

Check installed packages:

```routeros
/system package print
```

Check available interfaces:

```routeros
/interface print
```

For legacy wireless devices:

```routeros
/interface wireless print detail
```

For devices using the WiFi package:

```routeros
/interface wifi print detail
```

Use the commands supported by the installed wireless package. Legacy Wireless commands cannot be assumed to work on devices using the newer WiFi configuration system.

# 🗁 Check Wireless Interface Status

An interface may be disabled, disconnected, or unable to establish a wireless connection.

For legacy wireless:

```routeros
/interface wireless print
```

Inspect detailed interface information:

```routeros
/interface wireless print detail
```

Monitor the wireless interface:

```routeros
/interface wireless monitor wlan1
```

For the WiFi package:

```routeros
/interface wifi print detail
```

Check whether:

* The interface is enabled.
* The interface is operating in the expected mode.
* The configured SSID is correct.
* The selected frequency is supported.
* The interface reports an active connection where applicable.

# 🗁 Wireless Interface Is Missing

**Symptoms:**

* No wireless interface appears.
* Wireless commands are unavailable.
* WinBox does not display wireless configuration menus.

**Possible causes:**

* The device does not contain wireless hardware.
* The required wireless package is missing.
* The installed package does not support the device.
* Wireless hardware is not detected.
* The RouterOS version is incompatible with the required driver.

**Troubleshooting:**

Check installed packages:

```routeros
/system package print
```

Check available interfaces:

```routeros
/interface print
```

Review system logs:

```routeros
/log print
```

Confirm the device model and supported wireless package before installing or changing drivers.

# 🗁 Wireless Interface Is Disabled

**Symptoms:**

* The SSID is not visible.
* The access point does not accept clients.
* The wireless interface does not operate.

Check the interface:

```routeros
/interface wireless print
```

Enable it:

```routeros
/interface wireless enable wlan1
```

For the WiFi package:

```routeros
/interface wifi enable wifi1
```

Verify the interface again after enabling it.

# 🗁 SSID Is Not Visible

**Symptoms:**

* Clients cannot find the wireless network.
* The access point appears unavailable.
* Some devices can discover the network while others cannot.

**Possible causes:**

* Wireless interface is disabled.
* Incorrect SSID configuration.
* SSID broadcasting is hidden.
* Unsupported frequency band.
* Unsupported channel.
* Regulatory restrictions.
* Incorrect wireless operating mode.

For legacy wireless, inspect the configuration:

```routeros
/interface wireless print detail
```

Check the SSID and broadcasting settings:

```routeros
/interface wireless
set wlan1 hide-ssid=no
```

Verify that the interface is operating as an access point and that the selected frequency is supported.

# 🗁 Wireless Client Cannot Connect

**Symptoms:**

* The SSID is visible.
* The client attempts to connect but fails.
* The client repeatedly disconnects during authentication.

**Possible causes:**

* Incorrect wireless password.
* Authentication mismatch.
* Unsupported encryption.
* WPA3 incompatibility.
* Incorrect security profile.
* Signal quality is insufficient.

Inspect the legacy security profile:

```routeros
/interface wireless security-profiles print detail
```

Inspect the wireless interface:

```routeros
/interface wireless print detail
```

Verify that the access point and client use compatible authentication and encryption settings.

For WPA2-Personal, use AES-based encryption rather than obsolete TKIP configurations.

# 🗁 Check Wireless Registration

The registration table shows wireless clients associated with a legacy wireless access point.

```routeros
/interface wireless registration-table print
```

Inspect detailed information:

```routeros
/interface wireless registration-table print detail
```

Useful information includes:

* Client MAC address.
* Connected interface.
* Signal strength.
* Connection uptime.
* Transmit and receive rates.
* Wireless link information.

For the newer WiFi package, inspect its available registration and monitoring menus.

The registration table is useful for determining whether the problem occurs before or after wireless association.

# 🗁 Weak Wireless Signal

**Symptoms:**

* Frequent disconnections.
* Low transmission rates.
* Unstable connections.
* Poor coverage in certain areas.
* High packet loss.

**Possible causes:**

* Excessive distance from the access point.
* Walls and physical obstacles.
* Incorrect antenna orientation.
* Poor antenna placement.
* Radio interference.
* Insufficient transmit power.
* Poor line of sight for outdoor links.

Check signal information:

```routeros
/interface wireless registration-table print detail
```

Monitor the interface:

```routeros
/interface wireless monitor wlan1
```

Possible improvements:

* Reposition the access point.
* Improve antenna alignment.
* Reduce unnecessary physical obstacles.
* Select a cleaner wireless channel.
* Use suitable antennas.
* Deploy additional access points where coverage requires them.

Increasing transmit power is not always the correct solution. Wireless communication requires a usable signal in both directions.

# 🗁 Wireless Interference

**Symptoms:**

* Unstable throughput.
* High latency.
* Frequent retransmissions.
* Wireless connections perform poorly despite strong signal strength.

**Possible causes:**

* Nearby access points using overlapping channels.
* Bluetooth devices.
* Microwave ovens.
* Other radio transmitters.
* Excessive channel width.
* High wireless client density.

For legacy wireless, scan nearby networks:

```routeros
/interface wireless scan wlan1
```

The scan temporarily uses the radio and may interrupt normal operation on that interface.

Review:

* Nearby SSIDs.
* Operating frequencies.
* Channel overlap.
* Signal strength of neighboring access points.

Possible improvements:

* Select a less congested channel.
* Use 20 MHz channel width in crowded environments.
* Prefer 5 GHz when suitable.
* Coordinate channels between nearby access points.
* Reassess access point placement.

# 🗁 Wireless Link Disconnects Frequently

**Symptoms:**

* Wireless station repeatedly disconnects.
* Point-to-point links become unstable.
* Remote network connectivity drops.
* Clients reconnect frequently.

**Possible causes:**

* Weak signal.
* Interference.
* DFS channel changes.
* Incorrect security settings.
* Antenna misalignment.
* Unstable power supply.
* Hardware or driver problems.

Inspect the interface:

```routeros
/interface wireless monitor wlan1
```

Review logs:

```routeros
/log print where topics~"wireless"
```

For the WiFi package, inspect the available WiFi logging topics and interface status.

For outdoor point-to-point links, check antenna alignment, line of sight, mounting stability, and environmental obstructions.

# 🗁 Station Cannot Connect to Access Point

**Symptoms:**

* Station interface remains disconnected.
* Access point does not show the station in its registration table.
* Wireless connection cannot be established.

**Possible causes:**

* Incorrect SSID.
* Incorrect security profile.
* Unsupported wireless mode.
* Incompatible frequency band.
* Regulatory restrictions.
* Access point does not permit the station.
* Signal strength is insufficient.

Check the station configuration:

```routeros
/interface wireless print detail
```

Check the access point registration table:

```routeros
/interface wireless registration-table print detail
```

Verify that both devices use compatible:

* Wireless standards.
* Frequency bands.
* Authentication methods.
* Encryption settings.
* Wireless operating modes.

# 🗁 Wireless Client Connects but Cannot Obtain an IP Address

**Symptoms:**

* Client connects successfully.
* Wireless signal appears normal.
* Client does not receive an IP address.
* Client may assign itself an automatic private address.

**Possible causes:**

* Wireless interface is not connected to the LAN bridge.
* Incorrect bridge port configuration.
* DHCP server is disabled.
* DHCP pool is exhausted.
* VLAN membership is incorrect.
* DHCP traffic is blocked.

Inspect bridge ports:

```routeros
/interface bridge port print
```

Inspect DHCP servers:

```routeros
/ip dhcp-server print
```

Inspect DHCP leases:

```routeros
/ip dhcp-server lease print
```

Inspect DHCP networks:

```routeros
/ip dhcp-server network print
```

Verify that the wireless interface belongs to the correct bridge or VLAN and that DHCP services are available in the client's network.

# 🗁 Wireless Client Gets an IP but Cannot Access the Internet

**Symptoms:**

* Client receives an IP address.
* Client can communicate with the local gateway.
* Internet access does not work.

**Possible causes:**

* Incorrect default gateway.
* Missing source NAT rule.
* Missing default route.
* DNS configuration problem.
* Firewall restrictions.
* WAN connectivity failure.

Check the client's network configuration first.

On MikroTik, inspect:

```routeros
/ip address print
/ip route print
/ip firewall nat print
/ip firewall filter print
/ip dns print
```

Test gateway connectivity:

```routeros
/ping 192.168.50.1
```

Test an external IP address:

```routeros
/ping 1.1.1.1
```

Test DNS resolution:

```routeros
/resolve example.com
```

Interpretation:

* Gateway unreachable: investigate wireless bridging, VLANs, and local addressing.
* Gateway reachable but external IP unreachable: investigate routing, WAN connectivity, NAT, and firewall policies.
* External IP reachable but domain resolution fails: investigate DNS configuration.

# 🗁 Wireless Client Cannot Access Other LAN Devices

**Symptoms:**

* Client connects successfully.
* Client receives an IP address.
* Internet works, but internal services are inaccessible.

**Possible causes:**

* Guest network isolation.
* Incorrect VLAN assignment.
* Firewall forward-chain restrictions.
* Incorrect subnet configuration.
* Missing routes between networks.
* Client isolation settings.

Inspect bridge VLAN configuration:

```routeros
/interface bridge vlan print
```

Inspect bridge ports:

```routeros
/interface bridge port print
```

Inspect firewall rules:

```routeros
/ip firewall filter print
```

Check whether the client belongs to the intended VLAN and whether firewall policies permit the required communication.

# 🗁 Wireless Bridge Is Not Forwarding Traffic

**Symptoms:**

* Wireless connection is established.
* Both wireless devices show connected status.
* Remote Ethernet devices cannot communicate with the main LAN.

**Possible causes:**

* Wireless interface is missing from the bridge.
* Incorrect station mode.
* Incompatible bridging capabilities.
* Bridge filtering rules.
* Incorrect VLAN membership.
* Pseudobridge limitations.

Inspect bridge ports:

```routeros
/interface bridge port print
```

Inspect bridge host entries:

```routeros
/interface bridge host print
```

Inspect wireless registration:

```routeros
/interface wireless registration-table print detail
```

For transparent Layer 2 connectivity, verify that the wireless hardware and operating modes support the intended bridging behavior.

Do not assume that every station mode provides transparent Ethernet bridging.

# 🗁 Poor Wireless Throughput

**Symptoms:**

* Wireless connection is stable but slow.
* Actual throughput is much lower than the advertised radio rate.
* Upload and download performance differ significantly.

**Possible causes:**

* Low signal-to-noise ratio.
* Interference.
* High airtime utilization.
* Excessive channel width.
* Weak client radio capability.
* Ethernet bottleneck.
* CPU limitations.
* Poor wireless link alignment.

Inspect wireless registration:

```routeros
/interface wireless registration-table print detail
```

Check interface statistics:

```routeros
/interface print stats
```

Check CPU utilization:

```routeros
/system resource monitor
```

Possible improvements:

* Use a cleaner channel.
* Optimize channel width.
* Improve antenna alignment.
* Reduce unnecessary radio interference.
* Check Ethernet link speed.
* Review CPU utilization.
* Test throughput using a controlled environment.

Remember that wireless physical-layer rates are not equivalent to application throughput.

# 🗁 Check RouterOS Logs

Logs help identify wireless connection events, authentication problems, and interface errors.

Display all logs:

```routeros
/log print
```

Filter wireless-related messages:

```routeros
/log print where topics~"wireless"
```

Inspect WiFi-related logs on devices using the newer WiFi package:

```routeros
/log print where topics~"wifi"
```

The exact topics depend on the installed RouterOS version and wireless driver.

For more detailed troubleshooting, temporarily configure appropriate logging topics and inspect the resulting messages.

Avoid leaving excessive debug logging enabled during normal operation.

# 🗁 Wireless Troubleshooting with Ping

Ping verifies IP connectivity between devices.

Test the local gateway:

```routeros
/ping 192.168.50.1
```

Test another LAN device:

```routeros
/ping 192.168.50.10
```

Test an external destination:

```routeros
/ping 1.1.1.1
```

Use ping results to distinguish local wireless connectivity problems from routing or internet connectivity problems.

A successful ping does not prove that all application traffic is working, but it provides a useful starting point.

# 🗁 Troubleshooting with Torch

Torch provides real-time traffic visibility on a selected interface.

Example:

```routeros
/tool torch interface=bridge-lan
```

Torch can help identify:

* Active source and destination addresses.
* Traffic protocols.
* Traffic rates.
* Unexpected broadcast traffic.
* Whether traffic reaches the expected interface.

Use the correct wireless, bridge, or VLAN interface depending on the problem being investigated.

Torch is a traffic inspection tool, not a wireless signal analyzer.

# 🗁 Troubleshooting with Packet Sniffer

The RouterOS packet sniffer can capture packets for deeper analysis.

Start a capture on a bridge:

```routeros
/tool sniffer set filter-interface=bridge-lan
/tool sniffer start
```

Stop the capture:

```routeros
/tool sniffer stop
```

Inspect the available capture results:

```routeros
/tool sniffer packet print
```

Packet captures can help investigate:

* DHCP discovery and offer exchanges.
* ARP requests and replies.
* DNS queries.
* ICMP traffic.
* VLAN-tagged packets.
* Unexpected traffic drops.

Packet sniffing on a bridge or Ethernet interface does not necessarily expose all over-the-air Wi-Fi management and control frames. Use wireless-specific monitoring tools where required.

# 🗁 Systematic Wireless Troubleshooting Workflow

Follow this sequence when diagnosing a wireless connectivity problem:

```text
Start
  |
  v
Check Wireless Interface
  |
  v
Is Interface Enabled?
  |             |
 No             Yes
  |              |
Enable It        v
             Check SSID
                 |
                 v
          Is Client Connected?
             |          |
            No          Yes
             |           |
      Check Security     v
      Frequency and   Check IP Address
      Signal Quality      |
                          v
                   IP Address Assigned?
                      |          |
                     No          Yes
                      |           |
                 Check Bridge,    v
                 VLAN and DHCP  Test Gateway
                                  |
                                  v
                            Internet Working?
                               |       |
                              No       Yes
                               |        |
                         Check Routing, v
                         NAT, DNS and  Complete
                         Firewall
```

# 🗁 Wireless Troubleshooting Best Practices

* Identify the wireless package before using configuration commands.
* Check interface status before changing configuration.
* Verify SSID and authentication settings.
* Inspect signal quality and registration information.
* Scan nearby wireless networks when interference is suspected.
* Check bridge ports and VLAN membership.
* Verify DHCP and IP addressing.
* Test gateway, external IP, and DNS separately.
* Review RouterOS logs for connection errors.
* Use Torch and packet sniffer for traffic-level diagnosis.
* Change one configuration variable at a time.
* Record working configurations before making major changes.
* Test connectivity again after every corrective action.

# 🗁 Summary

MikroTik wireless troubleshooting requires examining radio connectivity, authentication, signal quality, bridging, VLANs, DHCP, routing, and firewall policies.

A structured troubleshooting process should:

* Confirm the wireless interface is available and enabled.
* Verify that clients can associate with the access point.
* Inspect signal quality and interference.
* Confirm correct bridge and VLAN configuration.
* Verify IP address assignment and gateway connectivity.
* Test routing, NAT, and DNS.
* Use logs and packet inspection when necessary.

Identifying the failure point at the correct network layer makes troubleshooting faster and prevents unnecessary configuration changes.
