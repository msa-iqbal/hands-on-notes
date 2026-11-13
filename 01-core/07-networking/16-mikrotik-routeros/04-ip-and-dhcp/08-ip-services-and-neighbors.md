# IP Services and Neighbor Discovery

MikroTik RouterOS provides several IP-based services and Layer 2 discovery mechanisms for managing and monitoring network devices.

These features help administrators:

* Manage routers remotely.
* Control access to management services.
* Discover MikroTik and other network devices.
* Identify neighboring devices and their interfaces.
* Restrict MAC-based management access.
* Improve network visibility and security.

The main RouterOS menus covered in this document are:

| Feature | RouterOS Menu |
| --- | --- |
| IP Services | `/ip service` |
| Neighbor Discovery | `/ip neighbor` |
| Neighbor Discovery Settings | `/ip neighbor discovery-settings` |
| MAC Server | `/tool mac-server` |
| MAC WinBox Server | `/tool mac-server mac-winbox` |
| RoMON | `/tool romon` |

# 🗁 IP Services Overview

IP services provide remote management access to RouterOS through protocols such as SSH, HTTP, HTTPS, and API.

Display available IP services:

```routeros
/ip service print
```

Display detailed service configuration:

```routeros
/ip service print detail
```

Common RouterOS IP services include:

| Service | Default Port | Purpose |
| --- | ---: | --- |
| Telnet | 23 | Remote command-line access |
| FTP | 21 | File transfer |
| HTTP | 80 | WebFig access |
| SSH | 22 | Secure command-line access |
| HTTPS | 443 | Encrypted WebFig access |
| API | 8728 | RouterOS API access |
| API-SSL | 8729 | Encrypted RouterOS API access |
| WinBox | 8291 | WinBox management |

The exact enabled state and configuration should always be checked on the router.

# 🗁 Understanding IP Service Properties

An IP service entry contains several important properties.

| Property | Description |
| --- | --- |
| `name` | Service name |
| `port` | TCP port used by the service |
| `address` | Source IP addresses or networks allowed to access the service |
| `disabled` | Enables or disables the service |
| `available-from` | Availability information shown by RouterOS |
| `certificate` | Certificate used by TLS-based services where applicable |
| `max-sessions` | Maximum simultaneous sessions where supported |

Inspect a particular service:

```routeros
/ip service print detail where name=ssh
```

# 🗁 Enabling and Disabling IP Services

Services that are not required should be disabled.

Disable Telnet:

```routeros
/ip service disable telnet
```

Disable FTP:

```routeros
/ip service disable ftp
```

Disable API:

```routeros
/ip service disable api
```

Disable API-SSL:

```routeros
/ip service disable api-ssl
```

Enable SSH:

```routeros
/ip service enable ssh
```

Verify the configuration:

```routeros
/ip service print
```

Disabling unnecessary services reduces the number of management interfaces exposed to potential attackers.

# 🗁 Changing Service Ports

RouterOS allows administrators to change the TCP port used by an IP service.

For example, change the SSH port:

```routeros
/ip service set ssh port=2222
```

Change the WinBox port:

```routeros
/ip service set winbox port=8292
```

Verify:

```routeros
/ip service print
```

Clients must use the new port when connecting.

SSH example:

```bash
ssh -p 2222 admin@192.168.10.1
```

WinBox connections must also specify the configured port.

Changing a default port can reduce automated scans targeting common ports, but it is not a replacement for access restrictions and firewall rules.

# 🗁 Restricting IP Service Access

The `address` property restricts which source IP addresses or networks can connect to a particular service.

For example, allow SSH access only from the management subnet:

```routeros
/ip service set ssh address=192.168.10.0/24
```

Allow WinBox access only from the same management subnet:

```routeros
/ip service set winbox address=192.168.10.0/24
```

Allow HTTPS access from two management networks:

```routeros
/ip service set www-ssl address=192.168.10.0/24,192.168.20.0/24
```

Inspect the restrictions:

```routeros
/ip service print detail
```

The `address` property controls access to the individual service. It does not replace firewall filtering.

For stronger protection, combine service restrictions with firewall rules that permit management traffic only from trusted interfaces and networks.

# 🗁 Restricting WebFig Access

RouterOS provides HTTP and HTTPS services for WebFig.

The corresponding service names are:

* `www` — HTTP.
* `www-ssl` — HTTPS.

Disable unencrypted HTTP:

```routeros
/ip service disable www
```

Enable HTTPS:

```routeros
/ip service enable www-ssl
```

Restrict HTTPS access:

```routeros
/ip service set www-ssl address=192.168.10.0/24
```

For production management, prefer HTTPS over HTTP.

# 🗁 Managing API Services

RouterOS API allows external applications to communicate with the router programmatically.

Common use cases include:

* Network monitoring systems.
* ISP billing platforms.
* Customer management applications.
* Automated configuration systems.
* Network provisioning tools.

RouterOS provides two API services:

| Service | Port | Description |
|---|---:|---|
| API | 8728 | Unencrypted API |
| API-SSL | 8729 | TLS-encrypted API |

Disable the unencrypted API when it is not required:

```routeros
/ip service disable api
```

Restrict API-SSL to a trusted management network:

```routeros
/ip service set api-ssl address=192.168.10.0/24
```

Use dedicated RouterOS accounts with only the permissions required by the application.

# 🗁 Firewall Protection for IP Services

IP service restrictions and firewall filtering provide different layers of protection.

The IP service `address` property controls which source addresses can access a particular service.

Firewall rules control traffic entering the router and can additionally restrict access by interface, protocol, source address, and other matching conditions.

Example: allow SSH from the management subnet:

```routeros
/ip firewall filter add chain=input action=accept protocol=tcp dst-port=22 src-address=192.168.10.0/24 comment="Allow SSH from management LAN"
```

Drop other SSH attempts:

```routeros
/ip firewall filter add chain=input action=drop protocol=tcp dst-port=22 comment="Drop other SSH access"
```

These rules must be placed appropriately in the firewall input chain. An earlier broad accept rule may make a later restriction ineffective.

If the SSH port has been changed, update the firewall rules to match the new port.

# 🗁 Neighbor Discovery Overview

Neighbor Discovery allows RouterOS to identify devices connected to directly accessible Layer 2 networks.

It helps administrators discover:

* MikroTik routers.
* Switches.
* Network interfaces.
* Neighboring device identities.
* Management addresses.
* Device models and platform information.
* Neighboring network topology.

RouterOS supports several discovery protocols, including:

| Protocol | Description |
| --- | --- |
| MNDP | MikroTik Neighbor Discovery Protocol |
| CDP | Cisco Discovery Protocol |
| LLDP | Link Layer Discovery Protocol |

Neighbor discovery is useful for network mapping and troubleshooting.

It is a Layer 2 discovery mechanism and does not normally discover devices across routed boundaries.

# 🗁 Viewing Neighbor Devices

Display discovered neighbors:

```routeros
/ip neighbor print
```

Display detailed neighbor information:

```routeros
/ip neighbor print detail
```

Typical information includes:

| Property | Description |
| --- | --- |
| `interface` | Local interface where the neighbor was discovered |
| `identity` | Neighbor device identity |
| `address` | Neighbor IP address, when advertised |
| `mac-address` | Neighbor MAC address |
| `platform` | Device platform |
| `version` | RouterOS version, when advertised |
| `board` | Hardware board information, when advertised |
| `system-description` | Advertised device description, when available |
| `discovered-by` | Discovery protocol used |

The exact fields depend on the discovery protocol and the neighboring device.

# 🗁 Configuring Neighbor Discovery

RouterOS provides discovery settings to control which interfaces participate in neighbor discovery.

Inspect the current configuration:

```routeros
/ip neighbor discovery-settings print
```

The main property is:

`discover-interface-list`

This property specifies the interface list on which neighbor discovery is allowed.

For example, create a management interface list:

```routeros
/interface list add name=MGMT
```

Add the management bridge:

```routeros
/interface list member add list=MGMT interface=bridge-LAN
```

Restrict discovery to that interface list:

```routeros
/ip neighbor discovery-settings set discover-interface-list=MGMT
```

Verify:

```routeros
/ip neighbor discovery-settings print
```

This prevents discovery from being enabled indiscriminately on interfaces outside the selected list.

# 🗁 Configuring Discovery Protocols

RouterOS supports selecting which discovery protocols are enabled.

Inspect the current configuration:

```routeros
/ip neighbor discovery-settings print
```

Example: enable MNDP and LLDP while disabling CDP:

```routeros
/ip neighbor discovery-settings set protocol=mndp,lldp
```

To enable all supported discovery protocols:

```routeros
/ip neighbor discovery-settings set protocol=all
```

To disable neighbor discovery protocols:

```routeros
/ip neighbor discovery-settings set protocol=none
```

Use the protocol configuration appropriate for the network environment.

# 🗁 Neighbor Discovery Security

Neighbor discovery can expose information about network infrastructure.

Depending on the protocol and device, advertised information may include:

* Device identity.
* Hardware platform.
* RouterOS version.
* Interface information.
* Management addresses.

In an ISP or enterprise environment, exposing this information on public-facing or untrusted interfaces is undesirable.

Recommended practices:

* Restrict discovery to trusted management interfaces.
* Avoid enabling discovery on WAN-facing interfaces.
* Use only the discovery protocols required by the network.
* Review discovery settings after adding new interfaces.
* Separate management networks from customer networks.

# 🗁 MAC Server Overview

MAC Server provides Layer 2 management access without requiring an IP connection.

This is useful when:

* A router has no IP address configured yet.
* IP connectivity is unavailable.
* An administrator needs emergency local access.
* A device must be managed within the same Layer 2 network.

RouterOS provides MAC-based management through:

* MAC Telnet.
* MAC WinBox.

Unlike normal IP services, these mechanisms operate using MAC-layer communication.

They should be restricted to trusted interfaces.

# 🗁 Configuring MAC Server

Inspect MAC Server settings:

```routeros
/tool mac-server print
```

RouterOS uses an interface list to control where MAC Telnet access is allowed.

For example, create a management interface list:

```routeros
/interface list add name=MGMT
```

Add the management bridge:

```routeros
/interface list member add list=MGMT interface=bridge-LAN
```

Restrict MAC Server access:

```routeros
/tool mac-server set allowed-interface-list=MGMT
```

Verify:

```routeros
/tool mac-server print
```

This limits MAC Telnet access to the selected interface list.

# 🗁 Configuring MAC WinBox

MAC WinBox allows WinBox to connect to a RouterOS device using its MAC address rather than an IP address.

Inspect the configuration:

```routeros
/tool mac-server mac-winbox print
```

Restrict MAC WinBox access:

```routeros
/tool mac-server mac-winbox set allowed-interface-list=MGMT
```

Verify:

```routeros
/tool mac-server mac-winbox print
```

MAC WinBox is particularly useful during initial router setup and local troubleshooting.

# 🗁 Disabling MAC-Based Management

If MAC-based management is not required, disable it.

Disable MAC Telnet:

```routeros
/tool mac-server set allowed-interface-list=none
```

Disable MAC WinBox:

```routeros
/tool mac-server mac-winbox set allowed-interface-list=none
```

Verify:

```routeros
/tool mac-server print
/tool mac-server mac-winbox print
```

Keep in mind that MAC-based access can provide an alternative management path even when IP firewall rules block ordinary management traffic.

# 🗁 IP Management vs MAC Management

| Feature | IP Management | MAC Management |
| --- | --- | --- |
| Communication layer | Layer 3 | Layer 2 |
| Requires IP connectivity | Yes | No |
| Works across routed networks | Yes, if routing and filtering permit | No, normally limited to the same Layer 2 network |
| Common tools | SSH, WinBox, WebFig, API | MAC WinBox, MAC Telnet |
| Firewall IP filtering | Applicable | Ordinary IP firewall filtering does not control the MAC management traffic |
| Typical use | Routine remote administration | Initial setup and local recovery |

For routine administration, prefer secured IP-based management.

Keep MAC-based management restricted to trusted interfaces.

# 🗁 RoMON Overview

RoMON stands for Router Management Overlay Network.

It provides a separate management overlay that allows administrators to reach RouterOS devices through other RoMON-enabled MikroTik devices.

It can be useful when:

* Routers are connected through multiple network segments.
* Direct IP management is unavailable.
* An administrator needs an alternative management path.
* Several MikroTik devices must be managed through a central access point.

RoMON operates independently of ordinary IP routing.

It is not a replacement for normal routing or a general-purpose VPN.

# 🗁 Configuring RoMON

Inspect RoMON configuration:

```routeros
/tool romon print
```

Enable RoMON:

```routeros
/tool romon set enabled=yes
```

Disable RoMON:

```routeros
/tool romon set enabled=no
```

RoMON can also be configured with interface-specific settings and a secrets-based access policy.

For example, inspect the available configuration properties:

```routeros
/tool romon print detail
```

A RoMON design should define:

* Which devices participate.
* Which interfaces allow RoMON traffic.
* Which devices are trusted.
* How RoMON access is authenticated.
* How management traffic is protected.

Avoid enabling RoMON indiscriminately across untrusted networks.

# 🗁 Practical Lab: Secure Management Access

**Lab objective**

Configure IP management restrictions, neighbor discovery, and MAC-based management access.

**Network topology**

```text
                 Administrator PC
                  192.168.10.10
                         |
                    bridge-LAN
                         |
                  MikroTik Router
                  192.168.10.1
                         |
                      ether1
                         |
                       WAN
```

**Step A: Inspect available services**

```routeros
/ip service print detail
```

Identify the services currently enabled.

**Step B: Disable unnecessary services**

```routeros
/ip service disable telnet
/ip service disable ftp
/ip service disable www
/ip service disable api
```

Keep only the services required by your management environment.

**Step C: Restrict SSH and WinBox**

```routeros
/ip service set ssh address=192.168.10.0/24
/ip service set winbox address=192.168.10.0/24
```

**Step D: Create a management interface list**

```routeros
/interface list add name=MGMT
/interface list member add list=MGMT interface=bridge-LAN
```

**Step E: Restrict neighbor discovery**

```routeros
/ip neighbor discovery-settings set discover-interface-list=MGMT
/ip neighbor discovery-settings set protocol=mndp,lldp
```

**Step F: Restrict MAC management**

```routeros
/tool mac-server set allowed-interface-list=MGMT
/tool mac-server mac-winbox set allowed-interface-list=MGMT
```

**Step G: Verify the configuration**

```routeros
/ip service print
/ip neighbor discovery-settings print
/ip neighbor print
/tool mac-server print
/tool mac-server mac-winbox print
```

**Step H: Test management access**

From the administrator PC:

```bash
ssh admin@192.168.10.1
```

Open WinBox and connect using the router's IP address.

Verify that management access is limited to the intended management network and interfaces.

# 🗁 Troubleshooting

**Problem: SSH or WinBox connection fails**

Possible causes:

* Service is disabled.
* Incorrect TCP port.
* Source address is not allowed.
* Firewall input rules block the connection.
* Routing or connectivity problems.

Check:

```routeros
/ip service print detail
/ip firewall filter print
/ip address print
```

**Problem: Router does not appear in neighbor discovery**

Possible causes:

* Discovery is disabled.
* The local interface is not included in the discovery interface list.
* The neighboring device does not support the selected protocol.
* Devices are separated by a routed boundary.
* Layer 2 connectivity is unavailable.

Check:

```routeros
/ip neighbor discovery-settings print
/ip neighbor print detail
/interface list member print
```

**Problem: MAC WinBox cannot connect**

Possible causes:

* MAC WinBox is disabled.
* The interface is not permitted.
* The devices are not in the same Layer 2 network.
* Bridge or VLAN configuration prevents Layer 2 communication.

Check:

```routeros
/tool mac-server mac-winbox print
/interface bridge port print
/interface bridge vlan print
```

**Problem: MAC Telnet access is unavailable**

Check:

```routeros
/tool mac-server print
/interface list member print
```

Verify that the interface used by the administrator is included in the allowed interface list.

**Problem: API application cannot connect**

Possible causes:

* API service is disabled.
* Incorrect TCP port.
* Source address restriction.
* Firewall filtering.
* Incorrect user permissions.
* TLS configuration problems when using API-SSL.

Check:

```routeros
/ip service print detail
/user print detail
/ip firewall filter print
```

# 🗁 Best Practices

* Disable unused IP services.
* Prefer SSH and HTTPS for remote management.
* Restrict management services to trusted IP networks.
* Apply firewall rules to protect the router's input chain.
* Avoid exposing WinBox, SSH, or API directly to untrusted networks.
* Restrict neighbor discovery to management interfaces.
* Disable unnecessary discovery protocols.
* Restrict MAC WinBox and MAC Telnet to trusted interfaces.
* Disable MAC-based management when it is not required.
* Use dedicated management accounts with appropriate permissions.
* Enable RoMON only when the management design requires it.
* Regularly review IP services, interface lists, and management access policies.

# 🗁 Official Documentation

* [MikroTik RouterOS — IP Services](https://help.mikrotik.com/docs/spaces/ROS/pages/103841820/Services)
* [MikroTik RouterOS — Neighbor Discovery](https://help.mikrotik.com/docs/spaces/ROS/pages/24805517/Neighbor+discovery)
* [MikroTik RouterOS — MAC Server](https://help.mikrotik.com/docs/spaces/ROS/pages/98795539/MAC+server)
* [MikroTik RouterOS — RoMON](https://help.mikrotik.com/docs/spaces/ROS/pages/8978569/RoMON)
