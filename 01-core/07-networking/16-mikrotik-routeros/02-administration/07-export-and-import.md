# MikroTik RouterOS Export and Import

RouterOS provides configuration export and import mechanisms that allow administrators to save, inspect, modify, transfer, and recreate router configurations.

Unlike binary backups, which store configuration in a proprietary backup format, exports contain RouterOS commands in a readable script format.

The two main operations are:

* **Export:** Converts the current configuration into RouterOS commands.
* **Import:** Executes RouterOS commands from an exported script file.

Export and import are particularly useful for configuration migration, documentation, troubleshooting, automation, and version control.

# 🗁 Understanding Configuration Export

The `/export` command displays the router's configuration as RouterOS commands.

Display the current configuration:

```routeros
/export
```

Save the configuration to a file:

```routeros
/export file=router-config
```

RouterOS creates:

```text
router-config.rsc
```

The `.rsc` extension identifies a RouterOS script file.

The exported configuration can be downloaded, inspected, edited, and imported into another RouterOS device.

# 🗁 Exporting Specific Configuration Sections

RouterOS allows administrators to export individual configuration menus instead of the entire configuration.

For example, export IP addresses:

```routeros
/ip address export
```

Export firewall filter rules:

```routeros
/ip firewall filter export
```

Export NAT rules:

```routeros
/ip firewall nat export
```

Export routing configuration:

```routeros
/ip route export
```

Export bridge configuration:

```routeros
/interface bridge export
```

Save a specific section to a file:

```routeros
/ip firewall filter export file=firewall-filter
```

This creates:

```text
firewall-filter.rsc
```

Section-specific exports are useful when troubleshooting or migrating only selected configuration components.

# 🗁 Exporting Sensitive Information

By default, RouterOS hides certain sensitive values in configuration exports.

To include sensitive information:

```routeros
/export show-sensitive
```

To save an export containing sensitive values:

```routeros
/export show-sensitive file=router-config-sensitive
```

Use sensitive exports only when necessary.

They may contain credentials and other confidential configuration details. Store them securely and restrict access.

# 🗁 Understanding Configuration Import

The `/import` command executes the RouterOS commands contained in an `.rsc` file.

Basic syntax:

```routeros
/import file-name=router-config.rsc
```

RouterOS reads the file and processes its commands.

Importing a configuration is not equivalent to replacing the router's entire configuration with a backup. Commands are executed against the router's existing configuration.

Consequently:

* Existing configuration may remain unchanged.
* New configuration objects may be created.
* Existing objects may be modified if the script contains corresponding commands.
* Duplicate or conflicting settings may cause errors.
* Commands may fail if required interfaces or configuration objects do not exist.

Always inspect the script before importing it.

# 🗁 Importing with Verbose Output

Verbose output helps administrators understand which commands are being processed during an import.

Use:

```routeros
/import file-name=router-config.rsc verbose=yes
```

This provides additional information about script execution.

For debugging, it helps identify the command associated with an import error.

# 🗁 Importing with Dry Run

RouterOS supports dry-run execution for testing an import script before applying its configuration changes.

Use:

```routeros
/import file-name=router-config.rsc dry-run
```

A dry run helps identify syntax and execution problems without applying the script's configuration changes.

However, it should not be treated as a guarantee that the complete import will succeed in the actual environment. Runtime dependencies and differences in existing configuration can still affect the result.

# 🗁 Preparing a Configuration for Migration

Moving a configuration from one router to another requires more than simply exporting and importing a file.

Before migration, review the following:

| Item | What to Check |
| --- | --- |
| RouterOS version | Check compatibility between source and destination |
| Interface names | Verify Ethernet, wireless, and other interface names |
| Bridge configuration | Check bridge names, ports, and VLAN settings |
| IP addresses | Review address assignments and subnets |
| DHCP configuration | Verify pools, networks, and interface references |
| Firewall rules | Check interface and address-list references |
| NAT rules | Verify WAN and LAN interface references |
| Routing | Review gateways, routing tables, and protocol settings |
| VPN configuration | Check certificates, keys, peers, and authentication |
| Credentials | Confirm how sensitive values are handled |

Hardware-specific settings may not be suitable for another router model.

A configuration should be reviewed and adapted before importing it into different hardware.

# 🗁 Migrating Configuration Between Routers

Consider two MikroTik routers:

```text
Source Router
192.168.88.1
RouterOS v7
       |
       | Configuration Export
       v
router-config.rsc
       |
       | Review and Modify
       v
Destination Router
192.168.88.2
RouterOS v7
```

**Export from the source router**

```routeros
/export file=migration-config
```

**Download the export file**

Use WinBox, WebFig, SCP, or SFTP to download `migration-config.rsc`.

**Review the configuration**

Open the file using a text editor.

Check interface names, IP addresses, bridge configuration, routing, firewall rules, and other hardware-dependent settings.

Remove or modify commands that are not appropriate for the destination router.

**Upload the modified file**

Transfer the updated `.rsc` file to the destination router.

**Import the configuration**

```routeros
/import file-name=migration-config.rsc verbose=yes
```

**Verify the destination router**

Inspect the imported configuration and test network connectivity.

Useful verification commands:

```routeros
/system identity print
/ip address print
/interface print
/interface bridge print
/ip route print
/ip firewall filter print
/ip firewall nat print
```

# 🗁 Understanding Export File Structure

A RouterOS export generally contains commands organized by configuration menu.

For example:

```routeros
/interface bridge
add name=bridge-LAN

/interface bridge port
add bridge=bridge-LAN interface=ether2

/ip address
add address=192.168.10.1/24 interface=bridge-LAN

/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1
```

This example creates:

* A bridge named `bridge-LAN`.
* A bridge port using `ether2`.
* An IP address assigned to the bridge.
* A source NAT masquerade rule using `ether1`.

The order of commands matters when later commands depend on objects created earlier.

For example, a bridge should exist before a script attempts to add ports to it.

# 🗁 Editing Export Files

Because `.rsc` files are text-based, they can be edited using common text editors.

Examples include:

* Visual Studio Code.
* Notepad++.
* Vim.
* Nano.

Consider this exported configuration:

```routeros
/ip address
add address=192.168.10.1/24 interface=bridge-LAN
```

You can modify the IP address before importing:

```routeros
/ip address
add address=192.168.20.1/24 interface=bridge-LAN
```

You can also remove commands that are not required on the destination router.

**Important:** Editing an export requires understanding RouterOS command syntax and configuration dependencies. Incorrect modifications can cause import errors or unexpected network behavior.

# 🗁 Using Export Files with Git

RouterOS exports can be maintained in Git repositories to track configuration changes over time.

A possible repository structure:

```text
mikrotik-configurations/
├── core-router/
│   ├── router-config.rsc
│   └── README.md
├── edge-router/
│   ├── router-config.rsc
│   └── README.md
└── distribution-router/
    ├── router-config.rsc
    └── README.md
```

Initialize a Git repository:

```bash
git init mikrotik-configurations
cd mikrotik-configurations
```

Add the configuration files:

```bash
git add .
git commit -m "Add initial MikroTik configurations"
```

After modifying a router configuration and exporting it again:

```bash
git diff
```

This allows administrators to review configuration changes before committing them.

**Security practice:** Never commit sensitive exports containing passwords, private keys, or other credentials to a public repository. Use private repositories with appropriate access controls, and consider excluding sensitive files through `.gitignore`.

# 🗁 Export and Import Best Practices

Follow these practices when working with RouterOS configuration files:

* Create a binary backup before making major changes.
* Export the configuration before modifying important settings.
* Keep a copy of the original export.
* Review configuration files before importing them.
* Use dry-run and verbose options when appropriate.
* Check hardware and RouterOS version compatibility.
* Import configuration in a controlled environment.
* Avoid importing an entire configuration blindly into a production router.
* Protect sensitive exports.
* Verify the configuration after every import.
* Keep configuration history using Git or another version-control system.

# 🗁 Practical Lab: Export, Edit, and Import

This lab demonstrates how to export a configuration, modify it, and import it into a MikroTik CHR instance.

**Lab requirements:**

* MikroTik CHR.
* WinBox or SSH access.
* A text editor.
* Basic knowledge of RouterOS commands.

**Create a test configuration**

Create a bridge:

```routeros
/interface bridge
add name=bridge-LAB
```

Assign an IP address:

```routeros
/ip address
add address=192.168.50.1/24 interface=bridge-LAB
```

**Export the configuration**

```routeros
/export file=lab-config
```

**Download and inspect the file**

Download `lab-config.rsc` and open it in a text editor.

Locate the bridge and IP address configuration.

**Modify the export**

Change the IP address from:

```routeros
add address=192.168.50.1/24 interface=bridge-LAB
```

To:

```routeros
add address=192.168.60.1/24 interface=bridge-LAB
```

**Prepare a test destination**

Use a separate CHR instance or an isolated lab configuration. Ensure the destination has the required bridge interface.

**Upload and import**

Upload the modified file and execute:

```routeros
/import file-name=lab-config.rsc verbose=yes
```

**Verify the result**

```routeros
/ip address print
/interface bridge print
```

Review the output and identify whether the imported address was added successfully.

**Lab objectives:**

* Understand RouterOS export structure.
* Export the complete configuration.
* Edit configuration commands.
* Import an `.rsc` file.
* Use verbose output to inspect execution.
* Understand the difference between importing and restoring a binary backup.

# 🗁 Troubleshooting

**Import reports a syntax error**

Check the affected command for incorrect parameters, missing values, quotation marks, or unsupported syntax.

**Import reports an unknown interface**

The destination router may use different interface names, or the required interface may not exist.

Review:

```routeros
/interface print
```

Modify the export to match the destination router.

**Import creates duplicate configuration objects**

The script may contain `add` commands for objects that already exist.

Review the existing configuration and adjust the script before importing again.

**Some configuration commands are not imported**

Inspect the import output and identify commands that failed.

Check dependencies, RouterOS version compatibility, permissions, and existing configuration.

**Sensitive values are missing**

The original export may have hidden sensitive values.

Generate a new export using:

```routeros
/export show-sensitive file=router-config-sensitive
```

Protect the resulting file appropriately.

# 🗁 Official Documentation

* [MikroTik RouterOS Configuration Management](https://help.mikrotik.com/docs/)
* [MikroTik RouterOS Scripting](https://help.mikrotik.com/docs/)
