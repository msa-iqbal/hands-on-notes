# MikroTik RouterOS Backup and Restore

Backing up a MikroTik router's configuration is essential for protecting network settings against accidental changes, hardware failures, corrupted configurations, failed upgrades, and unexpected outages.

RouterOS provides two primary methods for backing up configurations:

* **Binary backup (`.backup`):** Creates a binary snapshot of the router's configuration.
* **Text export (`.rsc`):** Exports configuration commands in a human-readable RouterOS script format.

Both methods are useful, but they serve different purposes. For important network infrastructure, maintaining both types of backups is a good practice.

# 🗁 Backup vs Export

| Feature | Binary Backup | Text Export |
| --- | --- | --- |
| File extension | `.backup` | `.rsc` |
| Format | Binary | Plain text |
| Human-readable | No | Yes |
| Easy to edit | No | Yes |
| Suitable for reviewing configuration | No | Yes |
| Restores configuration | Yes | Recreates configuration through commands |
| Portability | More device and version dependent | Generally more portable |
| Sensitive information | Can contain sensitive configuration data | Sensitive values hidden by default |
| Typical use | Restoring a router configuration | Reviewing, migrating, and documenting configuration |

**Important:** A binary backup is not a complete replacement for a text export. Keep both when possible.

# 🗁 Creating a Binary Backup

A binary backup captures the router's configuration in RouterOS backup format.

Run the following command in the terminal:

```routeros
/system backup save name=router-backup
```

RouterOS creates a file named:

```text
router-backup.backup
```

To create a backup with a password:

```routeros
/system backup save name=router-backup password=StrongPassword
```

Use a strong password and store it securely.

To view generated backup files:

```routeros
/file print
```

Binary backups are particularly useful when restoring a router to the configuration state captured by the backup.

**Important considerations:**

* Prefer restoring a binary backup to the same device or a compatible device.
* RouterOS version and hardware compatibility matter.
* Do not assume that a backup from one hardware model will restore safely to another.
* A binary backup can contain sensitive configuration information.
* Keep the backup file and its password protected.

# 🗁 Creating a Text Export

A text export generates RouterOS commands representing the current configuration.

Run:

```routeros
/export file=router-export
```

RouterOS creates:

```text
router-export.rsc
```

The exported file contains configuration commands that can be inspected and edited using a text editor.

To display the configuration directly in the terminal:

```routeros
/export
```

Text exports are useful for:

* Reviewing existing configurations.
* Comparing configuration changes.
* Documenting network settings.
* Migrating selected configurations.
* Recreating configuration on another router.
* Maintaining configuration files in Git repositories.

# 🗁 Exporting Sensitive Information

By default, RouterOS hides certain sensitive values in configuration exports.

To include sensitive information:

```routeros
/export show-sensitive file=router-export-sensitive
```

This generates:

```text
router-export-sensitive.rsc
```

A sensitive export may contain passwords, authentication details, and other confidential configuration values.

Protect this file carefully. Do not publish it in public repositories or share it through unsecured channels.

Only generate sensitive exports when they are genuinely required.

# 🗁 Downloading Backup Files

Creating a backup inside the router is not enough. If the router's storage fails, locally stored backup files may become inaccessible.

Always download important backup files to another system.

**Using WinBox**

1. Connect to the router using WinBox.
2. Open the **Files** menu.
3. Locate the `.backup` or `.rsc` file.
4. Drag the file from the router's file list to your computer.
5. Store it in a secure backup directory.

**Using WebFig**

1. Open WebFig in a browser.
2. Navigate to **Files**.
3. Locate the backup file.
4. Download it to your computer.

**Using SCP**

If SSH and SCP access are available, you can transfer files from a Linux system:

```bash
scp admin@192.168.88.1:router-backup.backup .
scp admin@192.168.88.1:router-export.rsc .
```

Replace the IP address and username with your router's actual management details.

# 🗁 Restoring a Binary Backup

A binary backup restores the router's configuration from a previously generated `.backup` file.

Before restoring:

* Confirm that the backup belongs to the intended router.
* Check RouterOS and hardware compatibility.
* Ensure that you have access to the backup file.
* Plan for the temporary loss of connectivity during restoration.
* Prefer testing the restore process in a CHR or lab environment first.

Upload the backup file to the router using WinBox, WebFig, or SCP.

Then run:

```routeros
/system backup load name=router-backup.backup
```

RouterOS will request confirmation before proceeding. The router restores the backup and reboots as part of the process.

After the router starts again, reconnect and verify its configuration.

**Important:** Restoring a binary backup replaces the current configuration state. Changes made after the backup was created may be lost.

# 🗁 Restoring Configuration from a Text Export

A text export can be imported as a RouterOS script.

Upload the `.rsc` file to the router, then execute:

```routeros
/import file-name=router-export.rsc
```

RouterOS processes the commands in the script.

Unlike binary restoration, importing a text export is command-based. It does not necessarily recreate every aspect of the original router state.

Potential issues include:

* Commands that depend on interfaces or objects that do not exist.
* Configuration conflicts with existing settings.
* Differences between RouterOS versions.
* Hardware-specific interface names.
* Commands that require a particular configuration order.
* Sensitive values that were omitted from the original export.

Review the import output and verify the resulting configuration.

For a clean migration, prepare the destination router appropriately and review the export before importing it. Do not assume that importing an export into an already configured router will produce an exact replacement of its existing configuration.

# 🗁 Backup Before RouterOS Upgrades

Always create backups before upgrading RouterOS.

A recommended workflow is:

```routeros
/system resource print
/system package print
/system backup save name=before-upgrade
/export file=before-upgrade
```

Download both files to an external system before continuing with the upgrade.

This provides two recovery options:

* Binary backup for restoring a compatible router to its previous configuration.
* Text export for reviewing or manually recreating configuration.

Also check the RouterOS upgrade documentation and release notes before upgrading production routers.

# 🗁 Automating Backups with Scheduler

RouterOS Scheduler can execute scripts automatically at configured intervals.

The following example creates a script that generates a binary backup and a text export using the current date.

Create the script:

```routeros
/system script add name=daily-backup policy=read,write,policy,test source={
    :local backupName ("daily-" . [/system clock get date])
    /system backup save name=$backupName
    /export file=$backupName
}
```

Create a scheduler entry:

```routeros
/system scheduler add name=daily-backup \
    start-time=03:00:00 \
    interval=1d \
    on-event=daily-backup \
    policy=read,write,policy,test
```

The scheduler executes the backup script every day at 03:00.

**Note:** Date formatting can vary by RouterOS configuration and version. Review generated filenames and test the script before relying on it in production. This example creates files on the router; it does not transfer them to an external backup server.

# 🗁 Managing Backup Files

Regularly review the router's stored files:

```routeros
/file print
```

Remove an unnecessary file by specifying its actual name:

```routeros
/file remove [find name="old-backup.backup"]
```

Avoid deleting backup files until you have verified that a usable external copy exists.

For production networks, use a retention strategy:

* Keep recent daily backups.
* Retain selected weekly backups.
* Keep monthly backups for longer-term recovery.
* Store copies outside the router.
* Maintain an additional copy in a separate physical or cloud location.

# 🗁 Backup Security and Best Practices

Configuration backups can expose sensitive network information. Protect them with appropriate access controls.

Recommended practices:

* Use strong passwords for binary backups when appropriate.
* Restrict access to backup files.
* Encrypt backup storage when supported by your storage system.
* Transfer files over secure protocols such as SCP or SFTP.
* Avoid storing credentials directly inside scripts.
* Do not expose sensitive exports in public Git repositories.
* Maintain copies outside the router.
* Test restoration procedures periodically.
* Create a fresh backup before major configuration changes.
* Keep backups organized by router identity, date, and RouterOS version.

A practical directory structure on a backup server could look like this:

```text
mikrotik-backups/
├── core-router/
│   ├── daily/
│   ├── weekly/
│   └── monthly/
├── edge-router/
│   ├── daily/
│   ├── weekly/
│   └── monthly/
└── distribution-router/
    ├── daily/
    ├── weekly/
    └── monthly/
```

# 🗁 Practical Lab: Backup and Restore with CHR

This lab uses MikroTik Cloud Hosted Router (CHR) to practice backup and restoration without affecting a production router.

**Lab requirements:**

* Two CHR instances or one CHR instance with a separate test configuration.
* WinBox or SSH access.
* A computer for storing backup files.

**Create a backup**

Connect to the first CHR and run:

```routeros
/system backup save name=lab-backup
/export file=lab-export
```

Check the generated files:

```routeros
/file print
```

Download both files to your computer.

**Make a configuration change**

For example, create a test interface comment:

```routeros
/interface ethernet set ether1 comment="Backup testing"
```

Verify the change:

```routeros
/interface ethernet print detail
```

**Restore using the binary backup**

Upload `lab-backup.backup` to the CHR.

Run:

```routeros
/system backup load name=lab-backup.backup
```

Confirm the operation when prompted. After the router reboots, reconnect and inspect the interface configuration.

**Practice text export restoration**

Use a separate lab instance or a clean test configuration.

Upload `lab-export.rsc` and run:

```routeros
/import file-name=lab-export.rsc
```

Review the terminal output and verify the imported settings.

**Lab objectives:**

* Understand the difference between binary backups and text exports.
* Create and download backup files.
* Restore a binary backup.
* Import a text export.
* Identify configuration differences after restoration.
* Practice recovery without risking production infrastructure.

# 🗁 Troubleshooting

**Backup file is not visible**

Check the router's file list:

```routeros
/file print
```

Confirm that the backup command completed successfully and that the router has sufficient storage space.

**Binary backup restoration fails**

Check:

* Whether the file is valid and complete.
* Whether the backup is compatible with the target device.
* Whether the RouterOS version is appropriate.
* Whether the file was transferred without corruption.

**Text export import reports errors**

Inspect the import output. Check for missing interfaces, invalid commands, configuration conflicts, and version differences.

If necessary, edit the `.rsc` file and retry in a clean lab environment.

**Router becomes unreachable after restoration**

Check the management IP address, interface assignments, bridge configuration, VLAN membership, firewall rules, and routing settings.

Use a direct Ethernet connection or another available management method to recover access.

**Backup files consume too much storage**

Review stored files:

```routeros
/file print
```

Download and verify important files externally before removing outdated local copies.

# 🗁 Official Documentation

* [MikroTik RouterOS Backup](https://help.mikrotik.com/docs/spaces/ROS/pages/40992852/Backup)
* [MikroTik RouterOS Configuration Management](https://help.mikrotik.com/docs/)
* [MikroTik RouterOS Scripting](https://help.mikrotik.com/docs/)
