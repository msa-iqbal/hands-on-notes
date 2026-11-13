# SSID and Wireless Security

SSID and wireless security configuration determine how users identify and authenticate with a MikroTik wireless network.

A properly configured wireless network should provide clear network identification, strong authentication, modern encryption, and controlled client access.

MikroTik RouterOS supports different wireless configuration systems. The configuration commands depend on whether the device uses the legacy Wireless package or the newer WiFi package.

# 🗁 Understanding SSID

SSID stands for Service Set Identifier. It is the name of a wireless network displayed when users search for available Wi-Fi networks.

Examples:

```text
Office-WiFi
Guest-WiFi
Management-WiFi
Home-Network
```

An SSID helps users identify which wireless network they want to join.

A wireless access point broadcasts its SSID through beacon frames unless SSID broadcasting is disabled.

# 🗁 Configure SSID Using Legacy Wireless

For devices using the legacy `/interface wireless` configuration system, configure the SSID with:

```routeros
/interface wireless
set wlan1 ssid=Office-WiFi
```

To verify the configuration:

```routeros
/interface wireless print detail
```

The configured SSID should appear in the wireless interface details.

# 🗁 Configure SSID Using the WiFi Package

Devices using the newer WiFi package configure SSIDs through WiFi configuration profiles.

Create a configuration:

```routeros
/interface wifi configuration
add name=office-config \
    mode=ap \
    ssid=Office-WiFi
```

Apply it to the wireless interface:

```routeros
/interface wifi
set wifi1 configuration=office-config disabled=no
```

Verify the configuration:

```routeros
/interface wifi print detail
```

The available properties depend on the installed WiFi package and RouterOS version.

# 🗁 SSID Naming Best Practices

A good SSID should be easy to recognize and maintain.

Recommended practices:

* Use meaningful network names.
* Keep names reasonably short.
* Avoid unnecessary personal information.
* Avoid confusing names that resemble unrelated networks.
* Use separate SSIDs when different security or access policies are required.
* Maintain consistent naming conventions across multiple access points.

Example naming convention:

| Network Purpose | SSID |
| --- | --- |
| Corporate users | `Company-Staff` |
| Guests | `Company-Guest` |
| Network administration | `Company-Management` |
| IoT devices | `Company-IoT` |

# 🗁 Understanding Wireless Authentication

Wireless authentication determines how a client proves that it is authorized to connect to a wireless network.

Common wireless security methods include:

| Security Method | Description |
| --- | --- |
| Open | No wireless authentication or encryption |
| WPA-PSK | Pre-shared key authentication using older WPA |
| WPA2-PSK | Pre-shared key authentication using WPA2 |
| WPA3-PSK | Modern personal authentication using WPA3-Personal |
| WPA2-EAP | Enterprise authentication using an authentication server |
| WPA3-EAP | Enterprise authentication using WPA3 mechanisms |

For most small and medium-sized networks, WPA2-Personal with AES or WPA3-Personal is appropriate.

For enterprise environments, use enterprise authentication with a suitable RADIUS infrastructure where supported.

Avoid WEP, original WPA, and TKIP because they are obsolete security options.

# 🗁 Understanding Wireless Encryption

Wireless encryption protects data transmitted between wireless clients and access points.

Common encryption technologies include:

* **AES-CCM:** Encryption used with WPA2.
* **AES-GCMP:** An encryption option associated with newer WiFi security configurations.
* **TKIP:** An obsolete encryption mechanism that should not be used in modern deployments.

Use AES-based encryption wherever supported.

Encryption protects wireless data confidentiality, but it does not replace network segmentation, firewall rules, or secure device administration.

# 🗁 Configure WPA2-PSK Using Legacy Wireless

Create a WPA2 security profile:

```routeros
/interface wireless security-profiles
add name=office-security \
    authentication-types=wpa2-psk \
    unicast-ciphers=aes-ccm \
    group-ciphers=aes-ccm \
    wpa2-pre-shared-key="ReplaceWithAStrongPassword"
```

Assign the profile to the wireless interface:

```routeros
/interface wireless
set wlan1 security-profile=office-security
```

Verify the profile:

```routeros
/interface wireless security-profiles print detail
```

The WPA2 pre-shared key should be strong, unique, and protected from unauthorized access.

# 🗁 Configure WPA2 and WPA3 Using the WiFi Package

The newer WiFi package provides security configuration through `/interface wifi security`.

Create a security profile:

```routeros
/interface wifi security
add name=office-security \
    authentication-types=wpa2-psk,wpa3-psk \
    passphrase="ReplaceWithAStrongPassword"
```

Assign the profile to a WiFi configuration:

```routeros
/interface wifi configuration
set [find name=office-config] security=office-security
```

Verify the security configuration:

```routeros
/interface wifi security print detail
```

WPA2/WPA3 transition support and exact security options depend on the hardware, installed package, and RouterOS version.

If all client devices support WPA3, a WPA3-only configuration may provide a stronger modern security baseline.

# 🗁 WPA2-Personal vs WPA3-Personal

| Feature | WPA2-Personal | WPA3-Personal |
| --- | --- | --- |
| Authentication | Pre-shared key | SAE |
| Password-based protection | Yes | Yes |
| Forward secrecy | Not provided by PSK authentication | Improved through SAE |
| Compatibility | Very broad | Requires WPA3-capable clients |
| Security level | Strong when configured with AES and a strong key | Stronger modern design |

WPA3-Personal uses Simultaneous Authentication of Equals (SAE), which improves resistance to offline password-guessing attacks.

WPA2 may still be necessary when supporting older client devices.

# 🗁 Configure a Guest Wireless Network

Guest networks should be separated from internal business and management networks.

A common design uses:

* A dedicated guest SSID.
* A separate VLAN.
* A separate IP subnet.
* DHCP service for guest clients.
* Firewall restrictions preventing access to internal networks.
* Internet access according to the guest policy.

Example network design:

```text
                    Internet
                       |
                 MikroTik Router
                       |
                 VLAN Filtering
                       |
              +--------+--------+
              |                 |
          VLAN 10            VLAN 20
          Staff               Guest
              |                 |
        Staff-WiFi          Guest-WiFi
```

The SSID alone does not provide network isolation. VLAN configuration and firewall policies must enforce separation.

# 🗁 Hide SSID Broadcasting

Hiding an SSID prevents the access point from including the network name in ordinary beacon advertisements.

For legacy wireless:

```routeros
/interface wireless
set wlan1 hide-ssid=yes
```

To make it visible again:

```routeros
/interface wireless
set wlan1 hide-ssid=no
```

SSID hiding is not a meaningful security boundary. The network name can still be discovered through wireless management traffic and client connection activity.

Use proper authentication and encryption instead of relying on hidden SSIDs.

# 🗁 Configure Access Restrictions

Wireless security can be strengthened by controlling which clients are permitted to connect.

Possible approaches include:

* WPA2 or WPA3 authentication.
* RADIUS-based authentication.
* VLAN assignment based on user or device identity.
* Client isolation.
* Firewall restrictions.
* Separate guest and internal networks.

MAC address filtering can be used as an additional administrative control, but MAC addresses can be spoofed. It should never replace strong authentication.

# 🗁 MAC Address Access Lists in Legacy Wireless

Legacy wireless interfaces support access-list rules.

Example:

```routeros
/interface wireless access-list
add interface=wlan1 mac-address=AA:BB:CC:DD:EE:FF authentication=yes forwarding=yes
```

This permits a matching wireless client according to the configured access-list policy.

Access-list behavior depends on the interface's default authentication and forwarding settings.

Inspect existing entries:

```routeros
/interface wireless access-list print detail
```

Use this feature carefully. Large-scale client management is better handled through appropriate authentication and network access control systems.

# 🗁 Wireless Client Isolation

Client isolation prevents wireless clients from communicating directly with other clients connected to the same access point.

It is particularly useful for guest networks and public wireless access.

The exact isolation controls depend on the wireless package and device capabilities.

Client isolation should be tested with actual client devices after configuration.

For stronger separation between different network groups, use VLANs and firewall policies.

# 🗁 Secure Wireless Configuration Checklist

A secure wireless deployment should follow these practices:

* Use WPA2-AES or WPA3 where supported.
* Avoid WEP, WPA, and TKIP.
* Use strong and unique wireless passwords.
* Separate guest and internal wireless networks.
* Apply firewall restrictions to guest clients.
* Use VLANs to separate different wireless user groups.
* Avoid treating hidden SSIDs as a security mechanism.
* Restrict administrative access to trusted management networks.
* Review connected clients regularly.
* Update RouterOS and compatible wireless packages.
* Use RADIUS for centralized authentication when appropriate.

# 🗁 Troubleshooting SSID and Security Problems

**SSID is not visible**

Possible causes:

* Wireless interface is disabled.
* SSID is incorrectly configured.
* SSID broadcasting is hidden.
* Wireless frequency is unsupported by the client.
* Regulatory restrictions prevent operation on the selected channel.

**Client cannot authenticate**

Possible causes:

* Incorrect passphrase.
* Incompatible authentication method.
* Unsupported WPA3 configuration.
* Mismatched encryption settings.
* Incorrect security profile assignment.

**Older devices cannot connect**

Possible causes:

* WPA3-only authentication is enabled.
* The device does not support the selected frequency band.
* The wireless standard is incompatible.
* The client does not support the configured cipher.

**Client connects but cannot communicate with the LAN**

Possible causes:

* Incorrect bridge configuration.
* VLAN membership is missing.
* DHCP is unavailable.
* Firewall rules restrict client traffic.
* Client isolation is enabled.

# 🗁 Summary

SSID and wireless security configuration determine how clients identify and authenticate with a MikroTik wireless network.

Important concepts include:

* SSID naming and broadcasting.
* WPA2 and WPA3 authentication.
* AES-based wireless encryption.
* Security profiles.
* Guest network separation.
* Client access restrictions.
* Wireless client isolation.

A secure wireless network combines strong authentication with VLAN segmentation, firewall policies, and proper network administration.
