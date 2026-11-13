# RouterOS Versions and Licensing

RouterOS is MikroTik's network operating system. It is developed and maintained through different software releases, with features and capabilities that vary according to the version, platform, and license.

Understanding RouterOS versions and licensing is important when selecting MikroTik hardware, deploying Cloud Hosted Router (CHR), maintaining existing devices, and planning network upgrades.

This document explains RouterOS version identification, software releases, licensing concepts, and the main licensing considerations for MikroTik platforms.

# 🗁 Understanding RouterOS Versions

RouterOS uses version numbers to identify software releases.

A version number commonly follows this format:

```text
MAJOR.MINOR.PATCH
```

For example:

```text
7.16.2
```

| Component | Example | Description |
| --- | --- | --- |
| Major version | `7` | Major RouterOS version family |
| Minor version | `16` | Feature and development release level |
| Patch version | `2` | Maintenance and bug-fix release level |

The exact significance of a version change depends on the published release notes and changelog.

RouterOS has two major version families commonly encountered in existing networks:

* RouterOS v6
* RouterOS v7

Each version family has its own features, configuration differences, and compatibility considerations.

# 🗁 Checking the Installed RouterOS Version

The `/system resource` menu provides information about the installed RouterOS version and system resources.

Run:

```routeros
/system resource print
```

Example output:

```text
                   uptime: 2d04h12m
                  version: 7.x.x (stable)
               build-time: ...
         factory-software: ...
              free-memory: ...
             total-memory: ...
                      cpu: ...
                cpu-count: ...
          architecture-name: ...
```

The output depends on the RouterOS release and hardware platform.

You can also inspect installed packages:

```routeros
/system package print
```

This command displays installed package information and their versions.

# 🗁 RouterOS Release Channels

MikroTik provides different release channels for distributing RouterOS software updates.

The available channels have different purposes and levels of release maturity.

| Channel | Purpose |
| --- | --- |
| Stable | General-purpose releases for normal deployments |
| Long-term | More conservative release stream |
| Testing | Releases intended for testing and evaluation |
| Development | Development builds for evaluation and development |

The availability and behavior of release channels can change. Consult MikroTik's current documentation before selecting a channel.

**Stable**

The Stable channel is generally appropriate for production environments that need maintained releases with a balance between reliability and newer functionality.

**Long-term**

The Long-term channel is intended for environments that prioritize a more conservative update cycle.

It can be suitable for infrastructure where predictable maintenance and operational stability are important.

**Testing**

The Testing channel provides releases for testing new functionality, changes, and compatibility before broader deployment.

It is useful for lab environments and controlled evaluation.

**Development**

Development builds are intended for development and technical evaluation. They may contain unfinished functionality or changes that are not suitable for normal production deployment.

# 🗁 Choosing a Release Channel

The appropriate channel depends on the device's role and the network's operational requirements.

| Environment | Consideration |
| --- | --- |
| Home lab | Stable or Testing, depending on experimentation requirements |
| Home network | Prefer a validated production release |
| Small business | Prioritize stability and compatibility |
| Enterprise | Test updates before production deployment |
| ISP infrastructure | Use a validated release with a maintenance and rollback plan |
| Development environment | Testing or Development where appropriate |

For production infrastructure, do not select a release only because it has the highest version number.

Review its changelog, known issues, compatibility, and impact on the services running on the device.

# 🗁 Understanding RouterOS Licensing

RouterOS licensing determines the permitted use and capabilities of RouterOS on supported software platforms.

The licensing model depends on how RouterOS is deployed.

Common deployment categories include:

* RouterBOARD hardware.
* Cloud Hosted Router (CHR).
* Supported x86-based installations.

RouterBOARD devices generally include a RouterOS license associated with the hardware.

CHR uses a separate licensing model with different license levels and capabilities.

Licensing should be considered when planning a deployment, especially when RouterOS is running as a virtual machine or on supported x86 hardware.

# 🗁 RouterBOARD Licensing

MikroTik RouterBOARD devices generally come with RouterOS preinstalled and licensed.

The license is associated with the device rather than being managed like a typical software subscription.

Important considerations include:

* RouterOS licensing is included with supported RouterBOARD products.
* The license level depends on the device and platform.
* Hardware replacement and license transfer rules depend on the licensing model.
* RouterOS updates are subject to device and platform compatibility.

For ordinary RouterBOARD deployments, administrators generally do not need to purchase a separate RouterOS license.

# 🗁 Cloud Hosted Router Licensing

Cloud Hosted Router (CHR) is a RouterOS version designed to run as a virtual machine.

Unlike RouterBOARD, CHR uses a dedicated licensing model.

CHR is commonly used for:

* Virtual routing labs.
* Network simulation environments.
* Virtualized ISP infrastructure.
* Cloud-based routing deployments.
* Testing RouterOS features without physical hardware.

CHR licensing is based on its license level and supported platform capabilities.

# 🗁 CHR License Levels

CHR provides different license levels with different capabilities.

The commonly documented CHR license levels include:

| License | General Purpose |
| --- | --- |
| Free | Evaluation and basic virtual router usage |
| P1 | Paid license level |
| P10 | Higher-capacity paid license level |
| P-Unlimited | Highest-capacity license level |

CHR license levels primarily differ in supported throughput and licensing capabilities.

Exact throughput limits, pricing, and licensing conditions can change. Check MikroTik's official CHR documentation for current details.

Official documentation:

<https://help.mikrotik.com/docs/spaces/ROS/pages/18350234/Cloud+Hosted+Router+CHR>

# 🗁 CHR Free License

The Free CHR license allows users to run RouterOS in a virtualized environment without purchasing a paid license.

It is useful for:

* Learning RouterOS.
* Building virtual networking labs.
* Testing routing protocols.
* Practicing firewall configuration.
* Exploring VPN and tunneling.
* Evaluating virtual network architectures.

The Free license has throughput limitations compared with paid CHR license levels.

For production deployments that require higher throughput, evaluate the available paid license levels.

# 🗁 CHR Paid Licenses

Paid CHR licenses provide higher throughput capabilities than the Free license.

The appropriate license depends on the expected network traffic and deployment requirements.

When selecting a CHR license, consider:

* Expected aggregate traffic.
* Number of virtual network interfaces.
* CPU and memory allocation.
* Hypervisor performance.
* Routing and firewall workload.
* VPN encryption requirements.
* Required throughput.

A higher license level does not automatically guarantee higher real-world performance. The virtual machine's allocated resources and underlying hardware also affect performance.

# 🗁 CHR Licensing and Virtual Machine Identity

CHR licensing is associated with the virtual machine and its licensing information.

When deploying CHR, administrators should understand that virtual machine changes can affect licensing and activation requirements.

Important considerations include:

* Preserve the CHR system identity and licensing information.
* Follow MikroTik's documented procedures when moving or restoring virtual machines.
* Avoid assuming that cloning a licensed virtual machine creates an independently licensed instance.
* Check the licensing requirements before migrating CHR to another hypervisor or virtual machine.

For production deployments, maintain a record of the CHR license and deployment details.

# 🗁 x86 RouterOS Licensing

RouterOS has also been available for compatible x86-based hardware.

This allows RouterOS to run on supported conventional computer hardware instead of MikroTik RouterBOARD devices.

Historically, x86 installations have used a separate licensing model.

When considering an x86 deployment, verify:

* Whether the intended installation method is currently supported.
* Hardware and architecture compatibility.
* Licensing requirements.
* Upgrade and maintenance procedures.
* Network interface driver support.

Do not assume that CHR licensing and x86 licensing are interchangeable.

# 🗁 Inspecting RouterOS License Information

RouterOS provides a system license menu for displaying license information where supported.

Run:

```routeros
/system license print
```

The output depends on the platform and licensing model.

For CHR, inspect the license information to verify the current license state and related details.

You can also inspect system information:

```routeros
/system resource print
```

For RouterBOARD hardware, inspect the device information:

```routeros
/system routerboard print
```

These commands help identify the platform and available licensing information.

# 🗁 RouterOS Updates and Licensing

RouterOS software updates and licensing are related but separate concerns.

A valid license does not guarantee that every RouterOS release supports every hardware platform or feature.

Before upgrading, verify:

* Device architecture.
* Hardware compatibility.
* RouterOS version requirements.
* Package availability.
* License conditions.
* Feature support.

For CHR, ensure that the selected license level meets the deployment requirements.

For RouterBOARD, confirm that the device supports the intended RouterOS release.

# 🗁 Licensing Considerations for Network Design

Licensing should be included in the initial planning of a RouterOS deployment.

| Deployment | Licensing Consideration |
| --- | --- |
| Home RouterBOARD | Included device license |
| Office RouterBOARD | Included device license |
| Physical ISP router | Device-specific RouterOS license |
| Virtual lab using CHR | Free license may be sufficient |
| Virtual production router | Evaluate paid CHR license levels |
| Supported x86 installation | Verify applicable x86 licensing requirements |

The licensing model should be selected according to the deployment platform and operational requirements.

# 🗁 Common Licensing Mistakes

**Assuming CHR and RouterBOARD Use the Same Licensing Model**

CHR has its own licensing structure. RouterBOARD devices generally include a license associated with the hardware.

**Ignoring CHR Throughput Limits**

The Free CHR license has throughput limitations. This should be considered when building virtual labs or production environments.

**Assuming a License Guarantees Performance**

Actual performance depends on CPU resources, memory, network interfaces, virtualization overhead, packet size, and configuration.

**Cloning Licensed Virtual Machines Without Reviewing Licensing Conditions**

Virtual machine cloning and migration can affect licensing requirements. Follow MikroTik's official procedures.

**Upgrading Without Checking Platform Compatibility**

A valid license does not remove hardware or architecture compatibility requirements.

# 🗁 Official MikroTik References

| Resource                  | Link                                                                               |
| ------------------------- | ---------------------------------------------------------------------------------- |
| MikroTik Official Website | <https://mikrotik.com/>                                                            |
| RouterOS Documentation    | <https://help.mikrotik.com/docs/>                                                  |
| RouterOS Downloads        | <https://mikrotik.com/download>                                                    |
| CHR Documentation         | <https://help.mikrotik.com/docs/spaces/ROS/pages/18350234/Cloud+Hosted+Router+CHR> |
| MikroTik Support          | <https://mikrotik.com/support>                                                     |

# 🗁 Summary

RouterOS versions identify the installed software release, while release channels determine the stream from which updates are obtained.

RouterBOARD devices generally include a RouterOS license associated with the hardware. CHR uses a separate licensing model with Free and paid license levels. Supported x86 installations have their own licensing considerations.

Understanding the version and licensing model helps administrators select the right platform, plan deployments, and maintain MikroTik infrastructure.

The next topic explains the differences between RouterOS v6 and v7, including changes in routing, configuration, and feature availability.
