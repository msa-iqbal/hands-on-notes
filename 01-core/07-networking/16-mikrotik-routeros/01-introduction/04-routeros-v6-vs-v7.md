# RouterOS v6 vs v7

MikroTik RouterOS has two major version families commonly encountered in existing networks: RouterOS v6 and RouterOS v7.

Both versions provide essential networking functionality, including routing, switching, firewall, NAT, VPN, wireless, and network management. However, RouterOS v7 introduced significant architectural changes, new capabilities, and differences in configuration.

Understanding these differences is important when configuring new MikroTik devices, maintaining existing infrastructure, troubleshooting compatibility issues, and migrating production networks.

This document compares RouterOS v6 and v7, with particular attention to routing, configuration structure, packages, and operational considerations.

# 🗁 Overview of RouterOS v6

RouterOS v6 is an older major version family that has been widely deployed in MikroTik networks.

It provides many networking features used in production environments, including:

* IPv4 and IPv6 networking.
* Static and dynamic routing.
* OSPF and BGP.
* Ethernet bridging and VLANs.
* Firewall filtering and NAT.
* DHCP and DNS services.
* PPPoE and Hotspot.
* VPN and tunneling.
* Wireless networking.
* Queues and bandwidth management.
* Monitoring and troubleshooting.

Many existing MikroTik installations still use v6 because of legacy configurations, hardware requirements, or operational constraints.

# 🗁 Overview of RouterOS v7

RouterOS v7 is the newer major version family and the primary version used throughout this knowledge base.

It introduced significant improvements and changes, particularly in the routing subsystem.

Important areas include:

* Updated routing architecture.
* Expanded BGP functionality.
* Changes to OSPF configuration.
* Improved routing table management.
* Updated routing filters.
* Changes to routing protocol configuration.
* Additional networking capabilities.
* Platform and package compatibility changes.

RouterOS v7 should be treated as a distinct configuration environment rather than simply a newer patch release of v6.

# 🗁 Major Differences Between RouterOS v6 and v7

The following table summarizes important differences.

| Area | RouterOS v6 | RouterOS v7 |
| --- | --- | --- |
| Major version | 6.x | 7.x |
| Routing architecture | Older routing implementation | Updated routing architecture |
| Routing configuration | Older menu organization | Expanded `/routing` hierarchy |
| BGP | Older configuration model | Updated configuration model and capabilities |
| OSPF | Older configuration structure | Updated configuration structure |
| Routing filters | Older filter syntax and organization | Updated routing filter framework |
| Routing tables | Older routing table model | Expanded routing table management |
| IPv6 | Supported | Supported with continued development |
| Firewall | Supported | Supported |
| NAT | Supported | Supported |
| VLANs | Supported | Supported |
| VPN | Supported | Supported |
| Wireless | Supported according to hardware and packages | Supported according to hardware and packages |
| Scripts | Supported | Supported, with compatibility considerations |
| Packages | Version-dependent package structure | Updated package organization on supported platforms |

Feature availability and behavior depend on the exact release, device architecture, and installed packages.

# 🗁 Routing Architecture Differences

Routing is one of the most important areas affected by the transition from v6 to v7.

RouterOS v7 introduced a redesigned routing subsystem with changes to protocol configuration, routing tables, and route filtering.

In v6, administrators commonly work with routing configuration through menus such as:

```routeros
/routing ospf
```

```routeros
/routing bgp
```

In v7, routing configuration is organized through a broader hierarchy:

```routeros
/routing
```

The v7 routing framework provides separate configuration areas for different routing protocols and functions.

Inspect the routing menu:

```routeros
/routing print
```

The exact submenu structure depends on the configured routing features and RouterOS release.

# 🗁 BGP Configuration Differences

BGP configuration changed significantly between RouterOS v6 and v7.

RouterOS v7 uses a different BGP configuration model with separate connection, template, and session-related configuration concepts.

A simplified comparison:

| Concept | RouterOS v6 | RouterOS v7 |
| --- | --- | --- |
| BGP peer configuration | Older peer-oriented configuration | Connection and template-based configuration |
| Peer management | `/routing bgp peer` | `/routing bgp connection` and templates |
| BGP instances | Older instance configuration | Updated instance and template model |
| Routing filters | Older filter rules | Updated routing filter framework |
| Session inspection | Older peer status commands | Updated connection and session monitoring |

In RouterOS v7, inspect BGP connections:

```routeros
/routing bgp connection print
```

Inspect BGP sessions:

```routeros
/routing bgp session print
```

These commands are specific to the RouterOS v7 routing configuration model.

# 🗁 OSPF Configuration Differences

OSPF configuration also changed between v6 and v7.

RouterOS v6 commonly uses a configuration structure based on OSPF instances, areas, and interface-related configuration.

RouterOS v7 uses updated configuration menus and separates OSPF-related configuration into more specific objects.

Common v7 configuration areas include:

* OSPF instances.
* OSPF areas.
* OSPF interface templates.
* OSPF interface configuration.
* OSPF neighbor information.

Inspect OSPF instances:

```routeros
/routing ospf instance print
```

Inspect OSPF areas:

```routeros
/routing ospf area print
```

Inspect OSPF interface templates:

```routeros
/routing ospf interface-template print
```

The updated structure provides a different way of defining OSPF behavior and matching interfaces.

# 🗁 Routing Filter Differences

Routing filters determine which routes are accepted, rejected, modified, or assigned specific attributes.

They are particularly important in BGP and other dynamic routing environments.

RouterOS v6 uses an older routing filter configuration model.

RouterOS v7 introduces a different routing filter framework with updated rule syntax and configuration organization.

Inspect routing filter rules in RouterOS v7:

```routeros
/routing filter rule print
```

When migrating routing filters, administrators should review each rule rather than assuming that the old syntax can be copied directly.

Incorrect routing filters can unintentionally accept unwanted routes or prevent valid routes from being installed or advertised.

# 🗁 Routing Table Differences

RouterOS v7 provides expanded routing table management.

A routing table can be created explicitly:

```routeros
/routing table add name=to-isp2 fib
```

The `fib` property enables the table for forwarding lookups.

Display routing tables:

```routeros
/routing table print
```

Display IPv4 routes:

```routeros
/ip route print
```

Routing tables are useful for policy-based routing, multiple WAN connections, and advanced routing designs.

When migrating from v6, review routing rules, routing marks, and route selection behavior.

# 🗁 Package and Architecture Differences

RouterOS package organization and platform support have evolved between v6 and v7.

The exact packages available depend on the device architecture and RouterOS release.

Inspect installed packages:

```routeros
/system package print
```

Inspect the device architecture:

```routeros
/system resource print
```

Before upgrading, verify:

* Whether the device supports the target RouterOS version.
* Whether required packages are available.
* Whether wireless functionality uses the expected package.
* Whether existing configuration depends on older features.
* Whether the device has sufficient storage and memory.

Do not assume that all packages available in v6 are installed or organized identically in v7.

# 🗁 Wireless Configuration Considerations

Wireless configuration can differ depending on the device model, wireless chipset, RouterOS version, and installed wireless packages.

Some MikroTik devices use legacy wireless configuration, while newer hardware and releases may use updated wireless packages and configuration menus.

For example, older wireless configurations commonly use:

```routeros
/interface wireless
```

Newer wireless implementations may use:

```routeros
/interface wifi
```

The available menu depends on hardware and package support.

Before migrating a wireless device, verify its supported wireless package and configuration model.

# 🗁 Firewall and NAT Compatibility

Firewall and NAT configuration are generally familiar across RouterOS v6 and v7.

Common configuration areas remain:

```routeros
/ip firewall filter
```

```routeros
/ip firewall nat
```

```routeros
/ip firewall mangle
```

However, administrators should still validate firewall behavior after upgrading.

Pay particular attention to:

* Firewall rule ordering.
* Connection tracking.
* NAT rules.
* Routing marks.
* Interface lists.
* Bridge filtering.
* FastTrack.
* Policy-based routing interactions.

A migration should not assume that every existing configuration behaves identically in the new release.

# 🗁 Script and Automation Compatibility

RouterOS scripts can be affected by changes between major versions.

A script written for v6 may depend on:

* Menu paths that have changed.
* Command properties that no longer exist.
* Different routing configuration structures.
* Different output formats.
* Package-specific commands.
* Different behavior of networking features.

Before migrating, inspect:

* Scheduled scripts.
* Netwatch scripts.
* DHCP event scripts.
* PPP profile scripts.
* Firewall automation.
* External API integrations.
* Monitoring and provisioning systems.

Test scripts on a lab device running the target RouterOS version before deploying them to production.

# 🗁 Checking the Current RouterOS Version

Use the following command to identify the installed RouterOS version:

```routeros
/system resource print
```

Inspect installed packages:

```routeros
/system package print
```

Inspect hardware information:

```routeros
/system routerboard print
```

These commands help establish the current software and hardware environment before an upgrade or migration.

# 🗁 Migrating from RouterOS v6 to v7

Migrating from RouterOS v6 to v7 requires planning, testing, and configuration validation.

A recommended migration workflow is:

```text
Identify Current RouterOS Version
              |
              v
Inspect Device Hardware and Architecture
              |
              v
Export Existing Configuration
              |
              v
Create Binary Backup
              |
              v
Review v6/v7 Configuration Differences
              |
              v
Check Package and Feature Compatibility
              |
              v
Test Migration in a Lab
              |
              v
Schedule Production Maintenance
              |
              v
Upgrade RouterOS
              |
              v
Verify Configuration and Services
              |
              v
Monitor Network Operation
```

# 🗁 Pre-Migration Checklist

Before upgrading a production device, review the following areas.

**Hardware Compatibility**

* Verify the device model.
* Check supported RouterOS architecture.
* Confirm available storage and memory.
* Review hardware-specific limitations.

**Configuration**

* Export the existing configuration.
* Create a binary backup.
* Document important IP addresses.
* Record static routes and routing policies.
* Review firewall and NAT rules.
* Document VLAN and bridge configuration.

**Routing**

* Review OSPF configuration.
* Review BGP peers and policies.
* Check routing filters.
* Document routing tables.
* Review policy-based routing.

**Network Services**

* Check DHCP configuration.
* Review PPPoE services.
* Review Hotspot and RADIUS integration.
* Check VPN configuration.
* Review wireless package compatibility.

**Automation**

* Review scripts and scheduled tasks.
* Check external API integrations.
* Verify monitoring and alerting systems.

# 🗁 Post-Migration Validation

After upgrading to RouterOS v7, verify that the important network functions are operating correctly.

Check system information:

```routeros
/system resource print
```

Check interfaces:

```routeros
/interface print
```

Check IP addresses:

```routeros
/ip address print
```

Check routing tables:

```routeros
/routing table print
```

Check IPv4 routes:

```routeros
/ip route print
```

Check firewall rules:

```routeros
/ip firewall filter print
```

Check NAT configuration:

```routeros
/ip firewall nat print
```

Check system logs:

```routeros
/log print
```

For devices using dynamic routing, also verify the relevant OSPF or BGP sessions and confirm that expected routes are being received and advertised.

# 🗁 Common Migration Problems

| Problem | Possible Cause | Recommended Action |
| --- | --- | --- |
| BGP session does not establish | Changed configuration or incompatible settings | Review v7 BGP connection configuration |
| OSPF neighbors are missing | Interface or area configuration differences | Inspect OSPF instances, areas, and interface templates |
| Routes are missing | Routing table or filter differences | Review route installation and filter rules |
| Wireless interfaces are unavailable | Package or hardware compatibility issue | Verify supported wireless implementation |
| Scripts fail | Changed menu paths or command properties | Update and test scripts |
| VPN connections fail | Changed feature behavior or configuration | Verify VPN settings and logs |
| Unexpected traffic forwarding | Firewall or routing behavior differs | Inspect packet flow and configuration |

# 🗁 Which RouterOS Version Should You Use?

For new learning environments and new configurations, RouterOS v7 is the preferred target for this knowledge base.

It provides the current routing architecture and the configuration model used in the examples throughout the MikroTik section.

For existing production environments, the choice should depend on:

* Device compatibility.
* Required features.
* Operational stability.
* Vendor support and maintenance considerations.
* Application and script compatibility.
* Upgrade and rollback requirements.

RouterOS v6 knowledge remains useful when maintaining older installations or migrating existing networks.

# 🗁 Official References

| Resource                 | Link                                       |
| ------------------------ | ------------------------------------------ |
| RouterOS Documentation   | <https://help.mikrotik.com/docs/>          |
| RouterOS Downloads       | <https://mikrotik.com/download>            |
| MikroTik Changelogs      | <https://mikrotik.com/download/changelogs> |
| MikroTik Support         | <https://mikrotik.com/support>             |
| MikroTik Community Forum | <https://forum.mikrotik.com/>              |

# 🗁 Summary

RouterOS v6 and v7 provide many of the same core networking functions, but v7 introduces important changes to routing architecture, BGP, OSPF, routing filters, package organization, and other configuration areas.

RouterOS v7 is the primary version for new learning and configuration examples in this knowledge base.

Migrating from v6 requires careful review of hardware compatibility, routing configuration, firewall behavior, wireless packages, scripts, and network services.

The next topic covers RouterOS management interfaces, including WinBox, WebFig, CLI, SSH, and API.
