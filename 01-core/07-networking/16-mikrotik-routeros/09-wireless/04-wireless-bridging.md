# Wireless Bridging

Wireless bridging connects network segments through a wireless link instead of a physical Ethernet cable.

In MikroTik RouterOS, wireless bridging can connect two LAN segments, extend network connectivity between buildings, or provide connectivity to remote network devices.

A wireless bridge can operate at Layer 2, allowing devices on both sides to communicate within the same bridged network.

Understanding wireless operating modes, MAC address handling, bridge configuration, and VLAN behavior is essential when designing reliable wireless links.

# 🗁 Understanding Wireless Bridging

A wireless bridge connects two network segments using a wireless connection.

Instead of running an Ethernet cable between two locations, wireless devices establish a radio link and forward network traffic between their Ethernet interfaces.

Example:

```text
          Main Network
               |
        +--------------+
        | MikroTik AP  |
        | Bridge       |
        +--------------+
               |
          Wireless Link
               |
        +--------------+
        | MikroTik     |
        | Station      |
        +--------------+
               |
          Remote Network
```

Both network segments can communicate through the wireless link.

# 🗁 Wireless Bridge Use Cases

Wireless bridging is useful in several network environments.

**Building-to-building connectivity**

Connect two buildings without installing a physical cable between them.

**Remote LAN extension**

Extend an existing LAN to a remote office, warehouse, or security camera location.

**Temporary network connectivity**

Establish connectivity between network segments when physical cabling is unavailable.

**ISP distribution networks**

Connect remote customer locations or distribution points through supported wireless infrastructure.

**Network infrastructure extension**

Provide Layer 2 connectivity between switches or routers across a wireless link.

# 🗁 Understanding Wireless Bridge Operating Modes

MikroTik wireless devices support different operating modes depending on the wireless driver system.

Common legacy wireless modes include:

| Mode | Description |
| --- | --- |
| `bridge` | Access point mode commonly used for a point-to-point wireless bridge |
| `station` | Connects to an access point as a wireless client |
| `station-bridge` | MikroTik-specific station mode supporting Layer 2 bridging |
| `station-pseudobridge` | Provides a compatibility mechanism for client-side bridging |
| `ap-bridge` | Access point mode supporting multiple wireless clients |

For a traditional MikroTik-to-MikroTik point-to-point bridge, the legacy `bridge` and `station-bridge` modes are commonly used.

The newer WiFi package has a different configuration model. Do not assume legacy wireless mode names or bridging behavior apply to it.

# 🗁 Understanding Station Bridge

In the legacy Wireless package, `station-bridge` allows a MikroTik wireless station to participate in a Layer 2 bridge with a compatible MikroTik access point.

It preserves client MAC addresses across the wireless connection more effectively than pseudo-bridge mechanisms.

This mode is particularly useful for MikroTik-to-MikroTik wireless links.

The access point must support the corresponding bridging behavior.

# 🗁 Understanding Station Pseudobridge

Some wireless access points do not support the four-address wireless bridging behavior required for transparent Layer 2 forwarding.

Legacy MikroTik `station-pseudobridge` provides a compatibility mechanism for connecting such networks.

However, pseudobridge operation has limitations:

* MAC address translation or learning behavior may affect downstream devices.
* Multiple devices behind the station may not behave like a fully transparent Ethernet bridge.
* Some Layer 2 protocols may not pass correctly.
* It is not equivalent to a true transparent bridge.

Use a proper bridge-capable wireless link whenever transparent Layer 2 connectivity is required.

# 🗁 Point-to-Point Wireless Bridge Design

A point-to-point wireless bridge connects two network locations using two wireless devices.

Example:

```text
       Location A                         Location B

    Main Router                         Remote Switch
         |                                   |
      Switch                              LAN Devices
         |                                   |
    MikroTik AP                         MikroTik Station
         |                                   |
         +---------- Wireless Link ----------+
```

Typical responsibilities:

| Device | Role |
| --- | --- |
| Main MikroTik | Wireless access point |
| Remote MikroTik | Wireless station |
| Main bridge | Connects AP to the local LAN |
| Remote bridge | Connects station to the remote LAN |

Both devices must use compatible wireless settings.

# 🗁 Configure the Main Access Point

The following example uses the legacy Wireless package.

Assume:

* Wireless interface: `wlan1`
* Main LAN interface: `ether2`
* Wireless link SSID: `PTP-Link`
* Wireless security profile: `ptp-security`

Create a bridge:

```routeros
/interface bridge
add name=bridge-lan
```

Add the main LAN interface:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2
```

Configure the wireless interface as a point-to-point access point:

```routeros
/interface wireless
set wlan1 \
    mode=bridge \
    ssid=PTP-Link \
    band=5ghz-a/n/ac \
    frequency=5180 \
    channel-width=20/40mhz-Ce \
    security-profile=ptp-security \
    disabled=no
```

Add the wireless interface to the bridge:

```routeros
/interface bridge port
add bridge=bridge-lan interface=wlan1
```

The selected frequency and channel width must be supported and permitted by the regulatory domain.

# 🗁 Configure the Remote Station

Assume:

* Wireless interface: `wlan1`
* Remote LAN interface: `ether2`

Create a bridge:

```routeros
/interface bridge
add name=bridge-lan
```

Add the remote Ethernet interface:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2
```

Configure the wireless station:

```routeros
/interface wireless
set wlan1 \
    mode=station-bridge \
    ssid=PTP-Link \
    band=5ghz-a/n/ac \
    security-profile=ptp-security \
    disabled=no
```

Add the wireless interface to the bridge:

```routeros
/interface bridge port
add bridge=bridge-lan interface=wlan1
```

The remote station must use the same compatible security profile settings and connect to the intended access point.

# 🗁 Configure a Wireless Security Profile

Both devices must use compatible wireless authentication and encryption settings.

Create the profile on both devices:

```routeros
/interface wireless security-profiles
add name=ptp-security \
    authentication-types=wpa2-psk \
    unicast-ciphers=aes-ccm \
    group-ciphers=aes-ccm \
    wpa2-pre-shared-key="ReplaceWithAStrongPassword"
```

Assign the profile to the wireless interface.

For the main access point:

```routeros
/interface wireless
set wlan1 security-profile=ptp-security
```

For the remote station:

```routeros
/interface wireless
set wlan1 security-profile=ptp-security
```

Use a unique, strong passphrase for the wireless bridge.

# 🗁 Configure Management IP Addresses

A wireless bridge may use the same management subnet as the main network.

Example addressing:

| Device | Interface | IP Address |
| --- | --- | --- |
| Main Router | LAN bridge | `192.168.50.1/24` |
| Main AP | LAN bridge | `192.168.50.2/24` |
| Remote Station | LAN bridge | `192.168.50.3/24` |

Configure the main AP:

```routeros
/ip address
add address=192.168.50.2/24 interface=bridge-lan
```

Configure the remote station:

```routeros
/ip address
add address=192.168.50.3/24 interface=bridge-lan
```

If the main router provides DHCP and gateway services, the remote network can use those services across the Layer 2 bridge.

Do not assign the same IP address to multiple devices.

# 🗁 Verify Wireless Bridge Connectivity

Check wireless interface status:

```routeros
/interface wireless print detail
```

Monitor the wireless link:

```routeros
/interface wireless monitor wlan1
```

Check the registration table on the access point:

```routeros
/interface wireless registration-table print detail
```

Check bridge ports:

```routeros
/interface bridge port print
```

Check bridge host entries:

```routeros
/interface bridge host print
```

Check IP connectivity:

```routeros
/ping 192.168.50.1
```

From the remote station, ping the main router's management address.

# 🗁 Understanding Wireless Bridge Performance

Wireless bridge performance depends on several factors:

* Signal strength.
* Signal-to-noise ratio.
* Channel interference.
* Channel width.
* Wireless modulation and coding rates.
* Antenna alignment.
* Physical distance.
* Radio capabilities.
* Ethernet port speed.
* Traffic direction and airtime utilization.

A wireless link's physical data rate is not the same as its application-level throughput.

For point-to-point links, properly aligned directional antennas and a clear line of sight can improve reliability.

# 🗁 Wireless Bridging and VLANs

VLANs can be carried across a wireless bridge when the wireless link and bridge configuration support the required Layer 2 traffic.

A common design transports tagged VLAN traffic between two locations.

```text
       Main Switch                       Remote Switch
           |                                  |
      VLAN 10, 20                        VLAN 10, 20
           |                                  |
      MikroTik AP  ===== Wireless =====  MikroTik Station
           |                                  |
       Tagged VLANs                       Tagged VLANs
```

For a VLAN-aware bridge:

* Configure VLAN filtering on the bridge.
* Define the correct VLAN membership.
* Configure tagged and untagged ports appropriately.
* Include the bridge CPU port when VLAN interfaces on the bridge need access to a VLAN.
* Ensure the wireless interface transports the intended Ethernet frames.

VLAN transparency depends on the wireless operating mode and driver capabilities.

Test tagged traffic before using the link in production.

# 🗁 Wireless Bridging vs Routing

Wireless bridging and wireless routing solve different network design problems.

| Feature | Wireless Bridging | Wireless Routing |
| --- | --- | --- |
| OSI layer | Primarily Layer 2 | Layer 3 |
| IP subnet | Can extend the same subnet | Usually separates subnets |
| Broadcast traffic | Can cross the bridge | Not forwarded by ordinary routing |
| DHCP | Can use the same DHCP broadcast domain | Requires a DHCP relay or separate DHCP service |
| Network isolation | Requires VLANs and bridge controls | Can use routed firewall policies |
| Typical use | Transparent LAN extension | Connecting separate IP networks |

Use bridging when the remote location must participate in the same Layer 2 network.

Use routing when separate subnets, traffic control, or network isolation are required.

# 🗁 Common Wireless Bridging Problems

**Wireless link does not establish**

Possible causes:

* Incorrect SSID.
* Mismatched security settings.
* Unsupported frequency or channel width.
* Incompatible wireless modes.
* Weak signal.
* Regulatory restrictions.

**Wireless connection works but LAN devices cannot communicate**

Possible causes:

* Wireless interface is missing from the bridge.
* Incorrect bridge port configuration.
* Station mode does not support transparent bridging.
* VLAN membership is incorrect.
* Firewall or Layer 2 filtering blocks traffic.

**Only one device behind the station works**

Possible causes:

* Pseudobridge limitations.
* MAC address translation behavior.
* Incompatible wireless bridging mode.
* Upstream access point does not support transparent Layer 2 forwarding.

**Bridge performance is poor**

Possible causes:

* Radio interference.
* Poor antenna alignment.
* Insufficient signal quality.
* Excessively wide channel.
* High wireless airtime utilization.
* Ethernet link operating at a lower speed than expected.

# 🗁 Wireless Bridging Best Practices

* Use compatible MikroTik devices for transparent Layer 2 bridging.
* Prefer dedicated point-to-point links for building-to-building connectivity.
* Use directional antennas where appropriate.
* Maintain proper antenna alignment.
* Select channels based on a wireless survey.
* Use strong wireless authentication and encryption.
* Keep management access restricted to trusted networks.
* Configure VLAN membership explicitly.
* Avoid using pseudobridge when transparent Layer 2 forwarding is required.
* Monitor signal quality and throughput regularly.
* Consider Layer 3 routing when separate network boundaries are preferable.

# 🗁 Summary

Wireless bridging provides Layer 2 connectivity between network segments using a wireless link.

Important concepts include:

* Access point and station roles.
* `bridge` and `station-bridge` modes in legacy wireless.
* Pseudobridge limitations.
* Point-to-point wireless topology.
* Bridge configuration.
* Management IP addressing.
* VLAN transport.
* Wireless link monitoring and troubleshooting.

A reliable wireless bridge requires compatible devices, correct bridge configuration, appropriate radio planning, and careful verification of Layer 2 connectivity.
