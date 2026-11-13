# RouterOS Terminal and CLI

The RouterOS Command-Line Interface (CLI) is a text-based administration environment used to configure, monitor, troubleshoot, and automate MikroTik routers.

Unlike graphical interfaces such as WinBox and WebFig, the CLI provides direct access to RouterOS configuration menus and commands.

It is especially useful for advanced network administration, scripting, automation, troubleshooting, and managing multiple MikroTik devices consistently.

# 🗁 What Is RouterOS CLI?

RouterOS CLI is the command interpreter built into MikroTik RouterOS.

Administrators use commands to interact with different configuration menus, inspect system information, modify settings, and execute scripts.

For example, the following command displays available interfaces:

```routeros
/interface print
```

The following command displays configured IP addresses:

```routeros
/ip address print
```

RouterOS CLI uses a hierarchical command structure. Each command belongs to a particular configuration menu.

# 🗁 Ways to Access RouterOS CLI

There are several ways to access the RouterOS command line.

| Access Method | Description |
| --- | --- |
| WinBox Terminal | CLI through the WinBox application |
| WebFig Terminal | CLI through a web browser |
| SSH | Secure remote command-line access |
| Serial Console | Direct console access through a serial connection |
| Local Terminal | CLI access through supported local console environments |

**WinBox Terminal**

Open WinBox, connect to the router, and select **New Terminal**.

**WebFig Terminal**

Log in to WebFig and open the **Terminal** menu.

**SSH**

Connect to the router from a Linux workstation:

```bash
ssh admin@192.168.88.1
```

Replace `admin` and `192.168.88.1` with the appropriate username and router management address.

**Serial Console**

A serial console provides direct access to RouterOS through a compatible console port and terminal application. It is particularly useful when network-based management is unavailable.

# 🗁 Understanding the CLI Prompt

After connecting to RouterOS, the terminal displays a command prompt.

A typical prompt looks similar to:

```text
[admin@MikroTik] >
```

The prompt identifies the currently logged-in user and router identity.

RouterOS CLI uses `>` for its normal command prompt.

When entering a configuration menu, the prompt changes to reflect the current menu.

For example:

```text
[admin@MikroTik] > /ip
[admin@MikroTik] /ip>
```

The current menu is now `/ip`.

Commands entered from this context are interpreted relative to that menu.

# 🗁 RouterOS Command Structure

RouterOS commands generally follow a hierarchical structure:

```text
/menu/submenu command parameters
```

For example:

```routeros
/ip address print
```

This command contains:

| Component | Meaning |
| --- | --- |
| `/ip` | IP configuration menu |
| `address` | IP address submenu |
| `print` | Display configured entries |

Another example:

```routeros
/interface ethernet set ether1 disabled=yes
```

This command disables the Ethernet interface named `ether1`.

Understanding the menu hierarchy makes RouterOS commands easier to read and organize.

# 🗁 Navigating Between Menus

RouterOS CLI allows administrators to move between configuration menus.

**Enter a menu:**

```routeros
/ip
```

**Enter a submenu:**

```routeros
/address
```

**Return to the parent menu:**

```routeros
..
```

**Return to the root menu:**

```routeros
/
```

**Display the current menu path:**

```routeros
..
```

The `..` command moves one level upward. Repeating it returns you toward the root.

You can also execute commands using their complete paths without navigating through menus:

```routeros
/ip address print
/interface ethernet print
/system resource print
```

Using complete command paths is often easier when copying commands into documentation or scripts.

# 🗁 Common RouterOS CLI Commands

RouterOS provides commands for inspecting and managing almost every part of the system.

**System information:**

```routeros
/system identity print
/system resource print
/system routerboard print
/system clock print
/system package print
```

**Interface information:**

```routeros
/interface print
/interface ethernet print
/interface monitor-traffic ether1
```

**IP configuration:**

```routeros
/ip address print
/ip route print
/ip dns print
/ip neighbor print
```

**DHCP information:**

```routeros
/ip dhcp-client print
/ip dhcp-server print
/ip dhcp-server lease print
```

**Firewall information:**

```routeros
/ip firewall filter print
/ip firewall nat print
/ip firewall mangle print
/ip firewall address-list print
```

**Wireless information:**

```routeros
/interface wifi print
```

The wireless menu depends on the RouterOS wireless package and device capabilities. Some older devices and configurations use the legacy `/interface wireless` menu instead.

# 🗁 The Print Command

The `print` command displays configuration entries from the current menu.

Examples:

```routeros
/interface print
/ip address print
/ip route print
/ip firewall filter print
```

RouterOS also supports useful print options.

**Display detailed information:**

```routeros
/ip address print detail
```

**Display configuration in a compact format:**

```routeros
/ip address print terse
```

**Display entries with their internal item numbers:**

```routeros
/interface print
```

The item numbers shown in output can be used in some commands, but they may change when configuration entries are added or removed.

For scripts and repeatable administration, identifying entries through stable properties such as names is often safer.

# 🗁 The Add Command

The `add` command creates a new configuration entry.

For example, assign an IP address to an interface:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
```

Create an IP address pool:

```routeros
/ip pool add name=LAN-POOL ranges=192.168.10.100-192.168.10.200
```

Create a firewall address-list entry:

```routeros
/ip firewall address-list add list=MANAGEMENT address=192.168.10.0/24
```

The exact parameters depend on the configuration menu.

# 🗁 The Set Command

The `set` command modifies an existing configuration entry.

For example, rename an interface:

```routeros
/interface set ether2 name=LAN
```

Disable an interface:

```routeros
/interface set ether3 disabled=yes
```

Change the router identity:

```routeros
/system identity set name=Core-Router
```

Modify an existing IP address entry:

```routeros
/ip address set [find address="192.168.10.1/24"] interface=ether3
```

The `find` expression identifies the configuration entry to modify.

Always verify the selected entry before applying changes, particularly when multiple entries have similar properties.

# 🗁 The Remove Command

The `remove` command deletes a configuration entry.

For example:

```routeros
/ip address remove [find address="192.168.10.1/24"]
```

Remove a firewall rule using its matching comment:

```routeros
/ip firewall filter remove [find comment="Old test rule"]
```

You can also use an item number displayed by `print`:

```routeros
/ip address remove 0
```

Item numbers are not permanent identifiers. Check the current configuration before using them.

# 🗁 The Enable and Disable Commands

RouterOS uses `enable` and `disable` to control whether supported configuration entries are active.

**Disable an interface:**

```routeros
/interface disable ether3
```

**Enable an interface:**

```routeros
/interface enable ether3
```

**Disable a firewall rule:**

```routeros
/ip firewall filter disable [find comment="Temporary test rule"]
```

**Enable the firewall rule again:**

```routeros
/ip firewall filter enable [find comment="Temporary test rule"]
```

Disabling an entry generally preserves its configuration, while removing it deletes the entry.

# 🗁 Command Completion and Context Help

RouterOS CLI provides command completion and contextual help.

**Tab completion**

Press `Tab` while typing a command to complete an available command, menu, or parameter where possible.

For example, start typing:

```text
/interface eth
```

Press `Tab` to see whether the CLI can complete the command.

**Question mark help**

Use `?` to inspect available commands or parameters.

Examples:

```routeros
?
```

```routeros
/ip ?
```

```routeros
/ip address ?
```

Help output depends on the current menu and RouterOS version.

**Command history**

Use the keyboard's Up and Down arrow keys to navigate previously entered commands in interactive terminal sessions.

This is useful when repeating or correcting commands.

# 🗁 Finding Configuration Entries

RouterOS provides the `find` command to identify configuration entries based on conditions.

For example:

```routeros
/ip address find
```

Find an address matching a specific value:

```routeros
/ip address find address="192.168.10.1/24"
```

Find disabled interfaces:

```routeros
/interface find disabled=yes
```

Find firewall rules with a specific comment:

```routeros
/ip firewall filter find comment="Allow management"
```

The `find` command returns internal identifiers for matching entries. These identifiers can be used with commands such as `set`, `remove`, `enable`, and `disable`.

For example:

```routeros
/interface set [find name="ether2"] disabled=yes
```

# 🗁 Filtering and Inspecting Output

RouterOS CLI supports filtering configuration output with the `where` condition.

For example, display disabled interfaces:

```routeros
/interface print where disabled=yes
```

Display interfaces that are running:

```routeros
/interface print where running=yes
```

Display IP addresses assigned to a particular interface:

```routeros
/ip address print where interface=ether2
```

Display firewall rules from a particular chain:

```routeros
/ip firewall filter print where chain=input
```

Filtering makes it easier to inspect large configurations without displaying every entry.

# 🗁 Understanding Properties and Values

RouterOS configuration entries contain properties that define their behavior.

For example:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2
```

The properties are:

| Property | Value |
| --- | --- |
| `address` | `192.168.10.1/24` |
| `interface` | `ether2` |

Boolean properties commonly use:

```text
yes
no
```

For example:

```routeros
/interface set ether2 disabled=yes
```

Some properties accept IP addresses, interface names, numbers, lists, or other configuration-specific values.

Use contextual help to identify the supported parameters and values for a command.

# 🗁 Running Multiple Commands

RouterOS allows multiple commands to be executed in sequence using a semicolon.

Example:

```routeros
/system identity print; /system resource print; /interface print
```

This executes three commands in one terminal input.

For configuration changes, separate commands make troubleshooting easier because each operation can be checked independently.

# 🗁 Safe Mode

Safe Mode is a RouterOS feature designed to help protect administrators from accidentally losing remote access while changing configuration.

It is particularly useful when modifying:

* Firewall rules.
* IP addresses.
* Routing configuration.
* Bridge settings.
* VLAN configuration.
* Management access policies.

In supported interactive RouterOS terminal sessions, press `Ctrl + X` to enter Safe Mode.

The prompt indicates that Safe Mode is active.

Make the required configuration changes and verify that connectivity remains available.

Press `Ctrl + X` again to leave Safe Mode and keep the changes.

If the Safe Mode session terminates unexpectedly before changes are committed, RouterOS can automatically undo changes made during that Safe Mode session.

Safe Mode has limitations, including session and change-tracking constraints. It should not replace backups or careful configuration planning.

# 🗁 RouterOS CLI and Configuration Comments

Comments help administrators document why a configuration entry exists.

For example:

```routeros
/ip address add address=192.168.10.1/24 interface=ether2 comment="Office LAN gateway"
```

Add a comment to an existing firewall rule:

```routeros
/ip firewall filter set [find chain=input] comment="Router input policy"
```

A comment should describe the purpose of the configuration rather than simply repeat its technical values.

Useful comments improve troubleshooting and long-term maintenance.

# 🗁 Practical CLI Configuration Example

The following example demonstrates a basic LAN configuration using RouterOS CLI.

**Lab requirements:**

| Component | Value |
| --- | --- |
| LAN interface | `ether2` |
| Router IP | `192.168.10.1/24` |
| Network | `192.168.10.0/24` |
| DHCP pool | `192.168.10.100-192.168.10.200` |
| Gateway | `192.168.10.1` |
| DNS | `192.168.10.1` |

**Assign the router's LAN IP address:**

```routeros
/ip address add address=192.168.10.1/24 interface=ether2 comment="LAN gateway"
```

**Create an IP pool:**

```routeros
/ip pool add name=LAN-POOL ranges=192.168.10.100-192.168.10.200
```

**Configure the DHCP network:**

```routeros
/ip dhcp-server network add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=192.168.10.1
```

**Create the DHCP server:**

```routeros
/ip dhcp-server add name=LAN-DHCP interface=ether2 address-pool=LAN-POOL disabled=no
```

**Verify the configuration:**

```routeros
/ip address print
/ip pool print
/ip dhcp-server print
/ip dhcp-server network print
```

This example assumes that `ether2` is available for LAN use and that no conflicting DHCP server or network configuration already exists. It does not configure NAT, DNS forwarding, or firewall policy.

# 🗁 CLI Troubleshooting Commands

RouterOS CLI provides many tools for diagnosing network problems.

**System status:**

```routeros
/system resource print
/system health print
/system logging print
/log print
```

**Interface status:**

```routeros
/interface print
/interface ethernet monitor ether1 once
/interface monitor-traffic ether1
```

**IP and routing:**

```routeros
/ip address print
/ip route print detail
/ip arp print
/ip dns print
```

**Connectivity testing:**

```routeros
/ping 8.8.8.8
/tool traceroute 8.8.8.8
```

**Connection tracking:**

```routeros
/ip firewall connection print
```

**Traffic inspection:**

```routeros
/tool torch interface=ether1
```

These commands help identify interface failures, incorrect IP configuration, routing problems, DNS issues, and unexpected traffic patterns.

# 🗁 CLI Security and Best Practices

RouterOS CLI provides powerful administrative access. Commands should be executed carefully, especially on production routers.

Recommended practices:

* Use SSH instead of Telnet for remote CLI access.
* Restrict SSH access to trusted management networks.
* Use strong and unique administrator credentials.
* Create separate user accounts with appropriate permissions.
* Review commands before executing them.
* Use Safe Mode when making risky remote changes.
* Create backups before major configuration changes.
* Add meaningful comments to important configuration entries.
* Verify configuration after applying changes.
* Avoid executing unfamiliar scripts on production routers.
* Keep RouterOS updated.
* Maintain an alternative recovery method.

Inspect available management services:

```routeros
/ip service print
```

Inspect configured users:

```routeros
/user print
```

Restrict SSH access to a trusted management subnet:

```routeros
/ip service set ssh address=192.168.10.0/24
```

The permitted subnet must match the actual management network.

# 🗁 CLI vs RouterOS Scripting

The CLI is used for interactive administration, while RouterOS scripting allows administrators to execute repeatable sequences of commands.

For example, a simple script can display system information:

```routeros
:put "Router Identity:"
/system identity print

:put "System Resources:"
/system resource print
```

Scripts can be stored, executed manually, or scheduled through the RouterOS scheduler.

This makes scripting useful for:

* Repeated configuration tasks.
* Monitoring and reporting.
* Automated maintenance.
* Backup operations.
* Network event handling.
* Routine administrative workflows.

Scripting is covered separately in the automation and maintenance section.

# 🗁 Key Takeaways

* RouterOS CLI is the primary text-based administration environment.
* Commands are organized into hierarchical menus.
* `print` displays configuration, `add` creates entries, and `set` modifies them.
* `remove`, `enable`, and `disable` manage configuration entries.
* `find` and `where` help identify and filter configuration.
* Tab completion and contextual help make command entry easier.
* Safe Mode helps protect remote configuration changes.
* CLI is essential for advanced troubleshooting, scripting, and automation.
* SSH provides secure remote CLI access.
