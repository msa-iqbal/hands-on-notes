# Wireless Interface Setup

Wireless interface configuration in MikroTik RouterOS allows a router or wireless access point to provide Wi-Fi connectivity to client devices.

MikroTik wireless interfaces can operate in different modes depending on the network design. They can provide wireless access to clients, connect to another access point, or establish wireless links between network devices.

This document covers wireless interface fundamentals, interface modes, configuration, security basics, and verification using MikroTik RouterOS.

# 🗁 Understanding Wireless Interfaces

A wireless interface provides network connectivity over radio frequency instead of a physical Ethernet cable.

In MikroTik devices, wireless interfaces may be built into the hardware or provided through supported wireless adapters.

Common wireless interface names include:

```text
wlan1
wlan2
wlan3
```

The actual interface names depend on the device and installed wireless drivers.

MikroTik RouterOS wireless configuration depends on the wireless package and hardware capabilities. Traditional wireless interfaces commonly use the `wireless` package, while newer devices may use the `wifi` package.

# 🗁 Wireless Packages in RouterOS

MikroTik provides different wireless configuration systems.

| Wireless System | Configuration Path | Notes |
| --- | --- | --- |
| Legacy wireless | `/interface wireless` | Used by supported legacy wireless drivers |
| WiFi package | `/interface wifi` | Used by newer WiFi drivers and supported hardware |

The available configuration menus depend on the RouterOS version, installed packages, device model, and wireless hardware.

Check the available interfaces:

```routeros
/interface print
```

Check installed packages:

```routeros
/system package print
```

Check the available wireless menus:

```routeros
/interface wireless print
```

For devices using the newer WiFi configuration system:

```routeros
/interface wifi print
```

A command menu that does not exist on a particular device does not necessarily indicate a configuration problem. It may simply use a different wireless driver system.

# 🗁 Wireless Interface Modes

Wireless interfaces support different operating modes.

Common modes in the legacy wireless system include:

| Mode | Description |
| --- | --- |
| `ap-bridge` | Provides wireless access for multiple clients |
| `station` | Connects to an access point as a client |
| `station-bridge` | Connects as a station using MikroTik-specific Layer 2 bridging |
| `station-pseudobridge` | Provides a compatibility mechanism for bridging through a station |
| `bridge` | Provides a wireless point-to-point access point role |
| `wds-slave` | Participates in a Wireless Distribution System |

The supported modes depend on the wireless driver and hardware.

The newer WiFi system uses a different configuration model and should not be assumed to support the same legacy mode names.

# 🗁 Access Point and Station Roles

**Access Point (AP)**

An access point provides wireless connectivity to client devices.

Examples:

* Smartphones.
* Laptops.
* Tablets.
* Wireless cameras.
* IoT devices.

**Station**

A station connects to an existing wireless access point.

For example, a MikroTik router can connect to another Wi-Fi network as a wireless client.

```text
          Wireless Access Point
                   |
                Wi-Fi
                   |
            +------+------+
            | MikroTik    |
            | Station     |
            +-------------+
```

# 🗁 Identify the Wireless Interface

Before configuring wireless, identify the interface and its capabilities.

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

You should identify:

* Interface name.
* Wireless driver system.
* Supported frequency bands.
* Supported wireless standards.
* Interface state.
* Existing configuration.

# 🗁 Enable a Wireless Interface

A wireless interface may be disabled by default or disabled through previous configuration.

For a legacy wireless interface:

```routeros
/interface wireless
enable wlan1
```

For a WiFi interface:

```routeros
/interface wifi
enable wifi1
```

Use the interface name that exists on your device.

Verify its state:

```routeros
/interface print
```

# 🗁 Configure a Basic Access Point

The following example uses the legacy wireless configuration system.

It assumes that the device supports the `wireless` package and the `ap-bridge` mode.

Set the wireless interface mode, SSID, and frequency band:

```routeros
/interface wireless
set wlan1 mode=ap-bridge ssid=Office-WiFi band=2ghz-b/g/n
```

Enable the interface:

```routeros
/interface wireless
enable wlan1
```

The `ssid` parameter defines the wireless network name visible to client devices.

The `band` parameter selects the supported wireless standards and frequency band. Choose a value supported by the hardware.

This is a basic interface example. A production access point also requires an appropriate security profile, frequency plan, and network integration.

# 🗁 Configure a Wireless Station

A MikroTik wireless interface can connect to an existing access point.

The following example uses the legacy wireless system:

```routeros
/interface wireless
set wlan1 mode=station ssid=Office-WiFi band=2ghz-b/g/n
```

Enable the interface:

```routeros
/interface wireless
enable wlan1
```

The station must use compatible wireless settings and authentication credentials.

For a secured network, configure the appropriate security profile before attempting to connect.

# 🗁 Configure a Wireless Security Profile

Wireless security profiles define authentication and encryption settings for supported legacy wireless interfaces.

Create a WPA2-PSK security profile:

```routeros
/interface wireless security-profiles
add name=office-security \
    authentication-types=wpa2-psk \
    unicast-ciphers=aes-ccm \
    group-ciphers=aes-ccm \
    wpa2-pre-shared-key="ChangeThisStrongPassword"
```

Assign the profile to the access point:

```routeros
/interface wireless
set wlan1 security-profile=office-security
```

Use a strong, unique passphrase.

For newer WiFi interfaces, configure security using the `/interface wifi security` menu and the security configuration supported by the installed WiFi package.

# 🗁 Configure Wireless Interface as a LAN Port

A wireless access point may need to connect wireless clients to the existing wired LAN.

In a simple legacy wireless bridge design, add the wireless interface to the LAN bridge:

```routeros
/interface bridge
add name=bridge-lan
```

Add the Ethernet interface:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2
```

Add the wireless interface:

```routeros
/interface bridge port
add bridge=bridge-lan interface=wlan1
```

This example assumes the wireless interface operates in a compatible AP mode and the bridge design supports the intended traffic.

For a VLAN-aware wireless network, configure the wireless interface and bridge VLAN membership according to the device's wireless driver capabilities.

# 🗁 Configure a Basic Wireless Network Using the WiFi Package

Newer MikroTik devices may use the WiFi package rather than the legacy wireless package.

The following is an illustrative configuration for a device exposing the `/interface wifi` menu.

Inspect the WiFi configuration:

```routeros
/interface wifi print detail
```

Create a security configuration:

```routeros
/interface wifi security
add name=wifi-security authentication-types=wpa2-psk,wpa3-psk passphrase="ChangeThisStrongPassword"
```

Create a configuration profile:

```routeros
/interface wifi configuration
add name=office-wifi \
    mode=ap \
    ssid=Office-WiFi \
    country=Bangladesh \
    security=wifi-security
```

Apply the configuration to the wireless interface:

```routeros
/interface wifi
set wifi1 configuration=office-wifi disabled=no
```

The exact supported properties and security options depend on the installed WiFi package and device capabilities. Inspect the command menu and use the options supported by your hardware.

# 🗁 Verify Wireless Interface Status

For legacy wireless interfaces:

```routeros
/interface wireless print
```

For detailed wireless information:

```routeros
/interface wireless monitor wlan1
```

For WiFi package interfaces:

```routeros
/interface wifi print detail
```

Check wireless registration:

```routeros
/interface wireless registration-table print
```

For the WiFi package, inspect the available registration and station monitoring menus for the installed version.

# 🗁 Monitor Connected Wireless Clients

The registration table displays wireless clients associated with a legacy wireless access point.

```routeros
/interface wireless registration-table print
```

Useful information includes:

* Client MAC address.
* Signal strength.
* Connection uptime.
* Transmission and reception rates.
* Wireless interface.

For newer WiFi interfaces, use the registration information exposed by the WiFi package.

# 🗁 Common Wireless Interface Problems

**Wireless interface is missing**

Possible causes:

* Wireless hardware is not present.
* Required wireless package is not installed.
* Driver is not supported by the RouterOS version.
* Device hardware or wireless module is not detected.

**Wireless interface is disabled**

Check:

```routeros
/interface print
```

Enable the correct interface:

```routeros
/interface wireless enable wlan1
```

Use the WiFi menu instead if the device uses the WiFi package.

**Clients cannot find the SSID**

Possible causes:

* Wireless interface is disabled.
* SSID configuration is incorrect.
* Unsupported frequency band.
* Regulatory or country settings prevent operation on the selected channel.
* Wireless interface is not operating in an AP mode.

**Clients can connect but cannot access the network**

Possible causes:

* Wireless interface is not connected to the correct bridge.
* Bridge VLAN membership is incorrect.
* DHCP configuration is missing.
* IP addressing or gateway configuration is incorrect.
* Firewall rules block traffic.

**Station cannot connect to an access point**

Possible causes:

* Incorrect SSID.
* Incorrect wireless security credentials.
* Incompatible frequency band or channel.
* Unsupported wireless mode.
* Signal quality is insufficient.

# 🗁 Wireless Interface Best Practices

* Identify the wireless driver system before configuring an interface.
* Use the configuration menu supported by the installed package.
* Select a frequency band supported by the hardware and clients.
* Use WPA2-AES or WPA3 security where supported.
* Avoid obsolete wireless security methods.
* Use a strong and unique wireless passphrase.
* Select the correct regulatory country setting.
* Keep wireless firmware and RouterOS packages compatible.
* Monitor connected clients and signal quality.
* Use bridge VLAN filtering when separating wireless client networks.
* Document SSIDs, wireless interfaces, security profiles, and VLAN assignments.

# 🗁 Summary

MikroTik wireless interface configuration provides Wi-Fi connectivity through supported wireless hardware and RouterOS packages.

The essential tasks include:

* Identifying the wireless driver system.
* Understanding AP and station operating roles.
* Enabling wireless interfaces.
* Configuring SSIDs and wireless modes.
* Applying wireless security settings.
* Integrating wireless interfaces with bridges.
* Monitoring connected clients and interface status.

Correct wireless interface configuration is the foundation for building secure and reliable MikroTik wireless networks.
