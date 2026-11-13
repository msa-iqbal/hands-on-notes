# MikroTik RouterOS Upgrade

RouterOS is the operating system used by MikroTik routers. Regular upgrades provide new features, security improvements, bug fixes, hardware support, and performance enhancements.

Upgrading RouterOS is an important part of network administration, particularly when managing production routers, ISP infrastructure, wireless networks, and enterprise environments.

A RouterOS upgrade should be planned carefully because an unsuccessful upgrade or incompatible configuration can interrupt network connectivity.

# 🗁 Understanding RouterOS Release Channels

MikroTik provides different release channels to help administrators select appropriate software versions.

| Release Channel | Description | Typical Usage |
| --- | --- | --- |
| Stable | Production-oriented releases with tested changes | Most production routers |
| Long-term | Conservative releases maintained for longer periods | Environments prioritizing stability |
| Testing | Pre-release versions for testing new features and fixes | Lab and development environments |
| Development | Early development builds | Advanced testing and development |

The available channels and their naming can change over time. Check the official MikroTik documentation before selecting a release.

For most production environments, use a suitable Stable or Long-term release rather than deploying experimental versions without testing.

# 🗁 Checking the Current RouterOS Version

Before upgrading, identify the currently installed RouterOS version.

Run:

```routeros
/system resource print
```

Look for the `version` field.

You can also run:

```routeros
/system package print
```

This displays installed RouterOS packages and their versions.

To check the router model and architecture:

```routeros
/system routerboard print
```

For additional hardware information:

```routeros
/system resource print
```

Important information to record:

* RouterOS version.
* Router model.
* Hardware architecture.
* Installed packages.
* RouterBOARD firmware version.
* Available storage.
* Current network configuration.

# 🗁 Checking Available Updates

RouterOS includes an update service that can check for available releases.

Display the current update channel:

```routeros
/system package update print
```

Set the update channel:

```routeros
/system package update set channel=stable
```

Check for available updates:

```routeros
/system package update check-for-updates
```

The command displays information about the installed version and available update.

To inspect the update configuration again:

```routeros
/system package update print
```

The update service requires working internet connectivity and access to MikroTik's update infrastructure.

# 🗁 Upgrading RouterOS Through Terminal

After checking the available version and reviewing the release information, start the upgrade.

Set the desired channel:

```routeros
/system package update set channel=stable
```

Check for updates:

```routeros
/system package update check-for-updates
```

Download and install the available update:

```routeros
/system package update install
```

RouterOS downloads the required packages and installs them.

The router normally reboots automatically to complete the upgrade.

After rebooting, reconnect to the router and verify the installed version:

```routeros
/system resource print
```

**Important:** Do not interrupt the router's power during the upgrade process.

# 🗁 Upgrading RouterOS Through WinBox

WinBox provides a graphical interface for managing RouterOS updates.

Follow these steps:

1. Connect to the router using WinBox.
2. Open **System**.
3. Select **Packages**.
4. Open the **Check for Updates** section.
5. Select the desired update channel.
6. Click **Check for Updates**.
7. Review the available version.
8. Click **Download and Install**.
9. Wait for the router to download and install the update.
10. Reconnect after the router reboots.

<text>WinBox menu names and layout may differ slightly between RouterOS releases.</text>

# 🗁 Upgrading RouterOS Through WebFig

WebFig also supports RouterOS package updates.

General procedure:

1. Connect to the router using WebFig.
2. Open **System**.
3. Navigate to **Packages**.
4. Locate the update section.
5. Select the desired channel.
6. Check for updates.
7. Download and install the available version.
8. Wait for the router to reboot.
9. Reconnect and verify the installed version.

# 🗁 Manual RouterOS Upgrade

Manual upgrading is useful when the router does not have direct internet access or when administrators need to control the exact package version being installed.

**Identify the router architecture**

Run:

```routeros
/system resource print
```

Find the `architecture-name` field.

Examples of MikroTik architectures include:

* `arm`
* `arm64`
* `mipsbe`
* `mmips`
* `smips`
* `tile`
* `ppc`
* `x86`

The correct package depends on the router's architecture and the RouterOS release.

**Download the appropriate package**

Visit the official MikroTik download page:

[https://mikrotik.com/download](https://mikrotik.com/download)

Select the appropriate RouterOS version and architecture.

**Upload the package**

Use WinBox, WebFig, SCP, or SFTP to transfer the package file to the router.

The package file is generally provided as an `.npk` file.

**Install the package**

After uploading, RouterOS detects the package and schedules installation during reboot.

Reboot the router:

```routeros
/system reboot
```

Confirm the reboot when prompted.

After the router starts, verify the installed version:

```routeros
/system resource print
/system package print
```

**Important:** Use packages intended for the exact architecture and compatible RouterOS release. Do not mix packages from different releases or architectures.

# 🗁 Upgrading RouterBOARD Firmware

RouterOS software and RouterBOARD firmware are related but separate components.

RouterBOARD firmware is the low-level firmware used by supported MikroTik hardware.

After upgrading RouterOS, check the current and available RouterBOARD firmware versions:

```routeros
/system routerboard print
```

Look for:

* `current-firmware`
* `upgrade-firmware`

If a newer firmware is available, upgrade it:

```routeros
/system routerboard upgrade
```

Confirm the operation when prompted.

Then reboot the router:

```routeros
/system reboot
```

After rebooting, verify:

```routeros
/system routerboard print
```

The current firmware should reflect the available upgraded version.

**Note:** RouterBOARD firmware procedures apply to supported RouterBOARD hardware. CHR and other RouterOS platforms have different hardware considerations.

# 🗁 RouterOS Upgrade vs RouterBOARD Firmware Upgrade

| Feature | RouterOS Upgrade | RouterBOARD Firmware Upgrade |
| --- | --- | --- |
| Component | Router operating system | RouterBOARD low-level firmware |
| Main purpose | Software features and fixes | Hardware firmware improvements |
| Package management | RouterOS packages | RouterBOARD firmware |
| Upgrade command | `/system package update install` | `/system routerboard upgrade` |
| Reboot | Normally required | Required to apply the firmware |
| Platform | RouterOS devices | Supported RouterBOARD hardware |

Both should be considered during routine maintenance, but they are not the same operation.

# 🗁 Pre-Upgrade Preparation

Before upgrading a production router, follow a structured preparation process.

**Check router health**

```routeros
/system resource print
/system health print
```

The health menu is hardware-dependent. Some devices do not expose all health information.

**Check installed packages**

```routeros
/system package print
```

Review installed packages and identify any package dependencies.

**Check available storage**

```routeros
/system resource print
/file print
```

Confirm that sufficient storage is available for the upgrade process.

**Create a binary backup**

```routeros
/system backup save name=before-upgrade
```

**Create a text export**

```routeros
/export file=before-upgrade
```

Download both files to a separate computer or backup server.

**Review release notes**

Read the release notes for the target RouterOS version.

Pay attention to:

* Breaking changes.
* Deprecated features.
* Routing changes.
* Firewall behavior.
* Wireless package changes.
* Hardware-specific issues.
* Known bugs and fixes.

**Plan a maintenance window**

For production networks, schedule upgrades during a period of lower network activity.

Inform relevant users or network administrators about the expected interruption.

# 🗁 Post-Upgrade Verification

After the router reboots, verify that the upgrade completed successfully.

**Check RouterOS version**

```routeros
/system resource print
```

**Check installed packages**

```routeros
/system package print
```

**Check system resources**

```routeros
/system resource print
```

**Check interfaces**

```routeros
/interface print
```

**Check IP addresses**

```routeros
/ip address print
```

**Check routing**

```routeros
/ip route print
```

**Check firewall rules**

```routeros
/ip firewall filter print
/ip firewall nat print
```

**Test connectivity**

```routeros
/ping 192.168.88.1
/ping 8.8.8.8
/ping google.com
```

Replace the first address with an appropriate reachable gateway or management address.

Also verify critical services such as:

* DHCP.
* DNS.
* PPPoE.
* VPN.
* Wireless.
* VLANs.
* Routing protocols.
* Queue management.
* Monitoring systems.

# 🗁 RouterOS Downgrade

A downgrade installs an earlier RouterOS version.

Downgrading may be necessary when:

* A new release introduces an unexpected problem.
* A required feature behaves differently.
* Hardware compatibility issues occur.
* A production service becomes unstable after an upgrade.

However, downgrading is not always a straightforward reversal.

Configuration changes introduced by a newer RouterOS release may not be fully compatible with an older version.

**Check available downgrade packages**

Download the intended older version from:

[https://mikrotik.com/download](https://mikrotik.com/download)

Ensure that the package matches the router architecture.

**Upload the package**

Transfer the appropriate `.npk` package to the router.

**Start the downgrade**

Run:

```routeros
/system package downgrade
```

Confirm the operation when prompted.

The router reboots and attempts to install the available downgrade package.

**Verify the result**

```routeros
/system resource print
/system package print
```

Review the configuration and test network services.

**Important:** Downgrading does not guarantee that every configuration change will be reversed. Maintain a pre-upgrade backup and test recovery procedures before relying on them.

# 🗁 Upgrade Strategy for Production Networks

For ISP and enterprise environments, avoid upgrading all routers simultaneously.

Use a staged deployment strategy:

```text
Lab Router
    |
    v
Test Router
    |
    v
Low-Risk Production Router
    |
    v
Distribution Routers
    |
    v
Core and Edge Routers
```

**Lab Router**

Test the new RouterOS version in CHR or supported lab hardware.

**Test Router**

Verify the behavior of important network services and configurations.

**Low-Risk Production Router**

Upgrade a less critical production device and monitor its behavior.

**Distribution Routers**

Upgrade additional devices after successful testing.

**Core and Edge Routers**

Upgrade critical infrastructure only after validating the release and recovery procedure.

This staged approach reduces the risk of widespread service interruption.

# 🗁 Common Upgrade Problems

**No updates are available**

Check internet connectivity and the selected update channel.

```routeros
/system package update print
/system package update check-for-updates
```

**Router cannot download packages**

Check DNS resolution, routing, firewall rules, and internet access.

```routeros
/ping 8.8.8.8
/ping mikrotik.com
```

**Insufficient storage**

Check available storage and remove unnecessary files only after confirming that important backups are stored elsewhere.

```routeros
/system resource print
/file print
```

**Manual package installation fails**

Verify:

* Router architecture.
* RouterOS version.
* Package compatibility.
* File integrity.
* Available storage.

**Router is unreachable after upgrading**

Try connecting through an alternative management method, such as a direct Ethernet connection, WinBox MAC connection where supported, or serial console where available.

Check interface configuration, IP addressing, firewall rules, and routing.

**RouterBOARD firmware was not upgraded**

Check:

```routeros
/system routerboard print
```

If an upgrade is available, run:

```routeros
/system routerboard upgrade
/system reboot
```

# 🗁 Practical Lab: Upgrade MikroTik CHR

This lab demonstrates a controlled RouterOS upgrade using CHR.

**Lab requirements:**

* MikroTik CHR instance.
* Internet connectivity.
* WinBox or SSH access.
* Access to the official MikroTik download page.

**Record the current version**

```routeros
/system resource print
/system package print
```

**Create backups**

```routeros
/system backup save name=lab-before-upgrade
/export file=lab-before-upgrade
```

Download the backup files to your computer.

**Check for updates**

```routeros
/system package update set channel=stable
/system package update check-for-updates
```

**Install the update**

```routeros
/system package update install
```

Wait for the router to reboot.

**Verify the new version**

```routeros
/system resource print
/system package print
```

**Test network connectivity**

```routeros
/ping 8.8.8.8
/ping google.com
```

**Review system logs**

```routeros
/log print
```

**Lab objectives:**

* Identify the installed RouterOS version.
* Check available updates.
* Create pre-upgrade backups.
* Upgrade RouterOS.
* Verify the installed version.
* Test network connectivity.
* Review system logs after upgrading.

# 🗁 Upgrade Best Practices

* Prefer Stable or Long-term releases for production environments, according to operational requirements.
* Read release notes before upgrading.
* Create binary backups and text exports.
* Download backups outside the router.
* Verify the router architecture before manual package installation.
* Check available storage.
* Test upgrades in CHR or lab hardware.
* Schedule maintenance windows.
* Upgrade production routers in stages.
* Avoid interrupting power during installation.
* Verify network services after rebooting.
* Upgrade RouterBOARD firmware when appropriate.
* Maintain a documented rollback and recovery procedure.
* Keep records of router versions and upgrade dates.

# 🗁 Official Documentation

* [MikroTik RouterOS Upgrade](https://help.mikrotik.com/docs/)
* [MikroTik Downloads](https://mikrotik.com/download)
* [MikroTik RouterBOARD Documentation](https://help.mikrotik.com/docs/)
