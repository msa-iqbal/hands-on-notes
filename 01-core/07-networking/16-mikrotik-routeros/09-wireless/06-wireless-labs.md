# Wireless Labs

Wireless labs provide practical experience configuring, securing, monitoring, and troubleshooting MikroTik wireless networks.

These exercises cover access point setup, wireless security, frequency planning, wireless bridging, guest network separation, and troubleshooting.

The examples use MikroTik RouterOS v7. Commands under `/interface wireless` apply only to devices using the legacy Wireless package. Devices using the newer WiFi package require the corresponding `/interface wifi` configuration.

# 🗁 Lab Environment

You can perform these labs using:

* MikroTik RouterBOARD devices.
* MikroTik wireless access points.
* MikroTik CHR for practicing non-radio networking configurations.
* Physical wireless client devices.
* Ethernet switches.
* GNS3 or EVE-NG for supported virtual network components.

CHR does not provide physical Wi-Fi radio interfaces. Wireless radio configuration and real wireless client testing require supported physical hardware.

# 🗁 Lab Topology

The following topology will be used for the main wireless access point lab.

```text
                 Internet
                    |
              Main Router
              192.168.10.1
                    |
                 ether2
                    |
             MikroTik AP
             192.168.10.2
                    |
                 wlan1
                    |
                Wi-Fi
                    |
              Wireless Clients
          192.168.10.100-200
```

Network configuration:

| Component | Configuration |
| --- | --- |
| Main router | `192.168.10.1/24` |
| Access point | `192.168.10.2/24` |
| Wireless interface | `wlan1` |
| Wireless SSID | `Lab-WiFi` |
| Wireless security | WPA2-PSK with AES |
| DHCP server | Main router |
| Client address range | `192.168.10.100-192.168.10.200` |

# 🗁 Lab: Configure a Basic Wireless Access Point

**Objective**

Configure a MikroTik device as a wireless access point and allow wireless clients to connect to the existing LAN.

**Prerequisites**

* MikroTik device with supported wireless hardware.
* RouterOS with the appropriate wireless package.
* Ethernet connectivity to the main router.
* A wireless client device.

**Configure the LAN bridge**

Create a bridge:

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

**Configure the access point**

The following commands apply to the legacy Wireless package:

```routeros
/interface wireless
set wlan1 \
    mode=ap-bridge \
    ssid=Lab-WiFi \
    band=2ghz-b/g/n \
    frequency=2437 \
    channel-width=20mhz \
    country=Bangladesh \
    installation=indoor \
    disabled=no
```

Configure the access point's management address:

```routeros
/ip address
add address=192.168.10.2/24 interface=bridge-lan
```

**Verify the configuration**

```routeros
/interface wireless print detail
/interface bridge port print
/ip address print
```

**Expected result**

* The wireless interface is enabled.
* The SSID `Lab-WiFi` is visible.
* Wireless clients can associate with the access point.
* Clients can communicate with the main LAN.

# 🗁 Lab: Configure WPA2 Wireless Security

**Objective**

Protect the wireless network using WPA2-Personal authentication and AES encryption.

Create a security profile:

```routeros
/interface wireless security-profiles
add name=lab-security \
    authentication-types=wpa2-psk \
    unicast-ciphers=aes-ccm \
    group-ciphers=aes-ccm \
    wpa2-pre-shared-key="LabStrongPassword2026!"
```

Assign the security profile:

```routeros
/interface wireless
set wlan1 security-profile=lab-security
```

Verify the configuration:

```routeros
/interface wireless security-profiles print detail
/interface wireless print detail
```

Reconnect a wireless client using the configured password.

**Expected result**

* The SSID remains visible.
* Clients must authenticate before joining.
* Clients with an incorrect password cannot connect.
* Authorized clients can access the LAN.

# 🗁 Lab: Configure a Wireless Station

**Objective**

Connect a MikroTik wireless interface to an existing access point.

**Topology**

```text
       Main Access Point
          Lab-WiFi
              |
           Wireless
              |
        MikroTik Station
              |
           ether2
              |
         Remote Device
```

Assume the main access point uses the following settings:

| Parameter | Value |
| --- | --- |
| SSID | `Lab-WiFi` |
| Frequency | `2437 MHz` |
| Band | `2.4 GHz` |
| Security | WPA2-PSK |
| Security profile | `lab-security` |

Configure the remote station using the legacy Wireless package:

```routeros
/interface wireless
set wlan1 \
    mode=station \
    ssid=Lab-WiFi \
    band=2ghz-b/g/n \
    security-profile=lab-security \
    disabled=no
```

Check the connection:

```routeros
/interface wireless monitor wlan1
```

**Expected result**

* The station discovers the configured SSID.
* The station authenticates successfully.
* The wireless interface establishes a connection with the access point.

# 🗁 Lab: Configure a Point-to-Point Wireless Bridge

**Objective**

Connect two LAN segments through a wireless link using MikroTik devices.

**Topology**

```text
        Location A                       Location B

      Main Router                      Remote Switch
          |                                 |
       ether2                            ether2
          |                                 |
      MikroTik AP                      MikroTik Station
          |                                 |
          +========= Wireless Link =========+
```

Use two compatible MikroTik devices supporting the legacy Wireless package and transparent station bridging.

**Main access point configuration**

Create a bridge:

```routeros
/interface bridge
add name=bridge-lan
```

Add Ethernet and wireless interfaces:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2

/interface bridge port
add bridge=bridge-lan interface=wlan1
```

Configure the access point:

```routeros
/interface wireless
set wlan1 \
    mode=bridge \
    ssid=PTP-Lab \
    band=5ghz-a/n/ac \
    frequency=5180 \
    channel-width=20/40mhz-Ce \
    security-profile=lab-security \
    disabled=no
```

**Remote station configuration**

Create a bridge:

```routeros
/interface bridge
add name=bridge-lan
```

Add Ethernet and wireless interfaces:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2

/interface bridge port
add bridge=bridge-lan interface=wlan1
```

Configure the station:

```routeros
/interface wireless
set wlan1 \
    mode=station-bridge \
    ssid=PTP-Lab \
    band=5ghz-a/n/ac \
    security-profile=lab-security \
    disabled=no
```

Configure management IP addresses:

Main access point:

```routeros
/ip address
add address=192.168.20.2/24 interface=bridge-lan
```

Remote station:

```routeros
/ip address
add address=192.168.20.3/24 interface=bridge-lan
```

Verify the wireless link:

```routeros
/interface wireless registration-table print detail
```

Verify bridge membership:

```routeros
/interface bridge port print
```

Test connectivity:

```routeros
/ping 192.168.20.2
```

**Expected result**

* The wireless station connects to the main access point.
* Both devices participate in the same Layer 2 network.
* Remote Ethernet devices can communicate with the main LAN.

# 🗁 Lab: Configure a Separate Guest Wireless Network

**Objective**

Create a guest wireless network that uses a separate VLAN and IP subnet.

**Topology**

```text
                    Internet
                       |
                 MikroTik Router
                       |
                 VLAN Filtering
                       |
                 Managed Switch
                       |
                  Access Point
                   /       \
                  /         \
          Staff-WiFi      Guest-WiFi
           VLAN 10         VLAN 20
```

Network plan:

| Network | VLAN | Subnet |
| --- | ---: | --- |
| Staff | 10 | `192.168.10.0/24` |
| Guest | 20 | `192.168.20.0/24` |
| Management | 99 | `192.168.99.0/24` |

**Configure the bridge**

The following is an example for a VLAN-aware RouterOS bridge. Adapt the physical interface names and existing bridge configuration to your topology.

Create the bridge:

```routeros
/interface bridge
add name=bridge-lan vlan-filtering=no
```

Add the trunk interface:

```routeros
/interface bridge port
add bridge=bridge-lan interface=ether2
```

Add the VLAN interfaces:

```routeros
/interface vlan
add name=vlan10-staff interface=bridge-lan vlan-id=10

/interface vlan
add name=vlan20-guest interface=bridge-lan vlan-id=20

/interface vlan
add name=vlan99-management interface=bridge-lan vlan-id=99
```

Configure VLAN membership:

```routeros
/interface bridge vlan
add bridge=bridge-lan vlan-ids=10 tagged=bridge-lan,ether2

/interface bridge vlan
add bridge=bridge-lan vlan-ids=20 tagged=bridge-lan,ether2

/interface bridge vlan
add bridge=bridge-lan vlan-ids=99 tagged=bridge-lan,ether2
```

Assign gateway addresses:

```routeros
/ip address
add address=192.168.10.1/24 interface=vlan10-staff

/ip address
add address=192.168.20.1/24 interface=vlan20-guest

/ip address
add address=192.168.99.1/24 interface=vlan99-management
```

Enable VLAN filtering after confirming the bridge VLAN table and management connectivity:

```routeros
/interface bridge
set bridge-lan vlan-filtering=yes
```

The access point must also support the intended VLAN-aware wireless configuration.

**Expected result**

* Staff and guest networks use separate VLANs.
* Each network uses its own IP subnet.
* Guest traffic can be restricted through firewall policies.
* Management access remains available through the management VLAN.

# 🗁 Lab: Configure Guest Network Firewall Isolation

**Objective**

Prevent guest clients from accessing internal networks while allowing internet access.

Assume:

* Guest subnet: `192.168.20.0/24`
* Staff subnet: `192.168.10.0/24`
* Management subnet: `192.168.99.0/24`

Create a firewall rule to prevent guest access to internal networks:

```routeros
/ip firewall filter
add chain=forward \
    src-address=192.168.20.0/24 \
    dst-address=192.168.10.0/24 \
    action=drop \
    comment="Block guest access to staff network"
```

Block guest access to management:

```routeros
/ip firewall filter
add chain=forward \
    src-address=192.168.20.0/24 \
    dst-address=192.168.99.0/24 \
    action=drop \
    comment="Block guest access to management network"
```

Place these rules before any broad forward-chain accept rules that would otherwise permit the traffic.

Verify that an appropriate WAN forwarding policy and source NAT configuration exist.

Test the following:

* Guest client can access the internet.
* Guest client cannot access staff devices.
* Guest client cannot access management devices.
* Staff clients retain their intended network access.

# 🗁 Lab: Perform Wireless Frequency Scanning

**Objective**

Identify nearby wireless networks and choose an appropriate channel.

For legacy wireless devices:

```routeros
/interface wireless
scan wlan1
```

Inspect the results for:

* SSID.
* Frequency.
* Signal strength.
* Nearby access points.
* Channel overlap.

Configure a selected channel:

```routeros
/interface wireless
set wlan1 frequency=2437 channel-width=20mhz
```

Verify the interface:

```routeros
/interface wireless monitor wlan1
```

**Expected result**

* Nearby wireless networks are identified.
* An appropriate channel is selected.
* Wireless performance is tested after the change.

Perform scanning carefully because it temporarily uses the radio and may interrupt service.

# 🗁 Lab: Monitor Connected Wireless Clients

**Objective**

Monitor wireless client connections and inspect their signal quality.

Display the registration table:

```routeros
/interface wireless registration-table print
```

Display detailed information:

```routeros
/interface wireless registration-table print detail
```

Monitor the wireless interface:

```routeros
/interface wireless monitor wlan1
```

Observe:

* Connected client MAC addresses.
* Signal strength.
* Connection uptime.
* Transmission rates.
* Reception rates.

For the newer WiFi package, use its supported registration and monitoring menus.

**Expected result**

You can identify connected clients and evaluate the wireless link condition.

# 🗁 Lab: Troubleshoot a Wireless Client Without DHCP

**Objective**

Identify why a wireless client connects but does not receive an IP address.

Create the following test condition:

* Configure the access point.
* Connect a wireless client.
* Disable the DHCP server or disconnect the DHCP service temporarily.

Inspect the bridge:

```routeros
/interface bridge port print
```

Inspect the DHCP server:

```routeros
/ip dhcp-server print
```

Inspect DHCP leases:

```routeros
/ip dhcp-server lease print
```

Inspect DHCP network configuration:

```routeros
/ip dhcp-server network print
```

Restore the DHCP service and verify that the client receives an address.

**Expected result**

You can distinguish a wireless association problem from a DHCP configuration problem.

# 🗁 Lab: Troubleshoot Wireless Signal Problems

**Objective**

Investigate a wireless client experiencing weak signal strength and unstable connectivity.

Inspect client registration:

```routeros
/interface wireless registration-table print detail
```

Monitor the wireless interface:

```routeros
/interface wireless monitor wlan1
```

Scan nearby networks:

```routeros
/interface wireless scan wlan1
```

Perform controlled tests:

* Move the client closer to the access point.
* Compare signal quality at different locations.
* Check nearby channel usage.
* Adjust channel width where appropriate.
* Reposition the access point or antenna.

Record the results before and after each adjustment.

**Expected result**

You can identify whether poor performance is related to distance, obstacles, interference, or radio configuration.

# 🗁 Lab: Wireless Connectivity Verification

**Objective**

Verify connectivity across the complete wireless network.

From a wireless client, test:

```bash
ping 192.168.10.1
```

Test an external IP address:

```bash
ping 1.1.1.1
```

Test DNS resolution:

```bash
nslookup example.com
```

On the MikroTik router, inspect:

```routeros
/interface wireless registration-table print detail

/interface bridge port print

/ip dhcp-server lease print

/ip route print

/ip firewall filter print
```

For devices using the newer WiFi package, replace legacy wireless inspection commands with the corresponding WiFi package commands.

**Expected result**

* Wireless clients can associate successfully.
* Clients receive valid IP addresses.
* Gateway connectivity works.
* DNS resolution works.
* Internet access works.
* Internal network access follows the intended security policy.

# 🗁 Lab Exercises

Complete these additional exercises to strengthen your understanding:

* Configure two wireless access points with different SSIDs.
* Configure separate security profiles for different wireless networks.
* Configure a guest SSID with VLAN isolation.
* Connect two MikroTik devices using a point-to-point wireless bridge.
* Experiment with 2.4 GHz and 5 GHz channel selection.
* Compare 20 MHz and 40 MHz channel widths.
* Identify interference using wireless scanning.
* Monitor wireless clients and signal quality.
* Investigate a wireless authentication failure.
* Investigate a DHCP failure affecting wireless clients.
* Compare Layer 2 wireless bridging with Layer 3 routed connectivity.

# 🗁 Lab Completion Checklist

* [ ] Configured a basic wireless access point.
* [ ] Configured WPA2 wireless security.
* [ ] Connected a MikroTik station to an access point.
* [ ] Established a point-to-point wireless bridge.
* [ ] Configured a separate guest VLAN.
* [ ] Applied firewall isolation to the guest network.
* [ ] Performed wireless frequency scanning.
* [ ] Monitored connected wireless clients.
* [ ] Troubleshot a DHCP connectivity problem.
* [ ] Investigated weak signal and interference.
* [ ] Verified complete wireless network connectivity.

# 🗁 Summary

These labs provide practical experience with MikroTik wireless networking.

They cover access point configuration, wireless security, station connectivity, point-to-point bridging, VLAN segmentation, firewall isolation, frequency planning, monitoring, and troubleshooting.

Completing these exercises builds the foundation for deploying and maintaining reliable MikroTik wireless networks.
