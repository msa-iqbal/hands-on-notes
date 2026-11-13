# RouterOS Users and Permissions

RouterOS provides a user management system that controls who can access a MikroTik router and which administrative operations they are allowed to perform.

Each administrator should have an individual account with permissions appropriate to their responsibilities. This helps protect router configuration, improve accountability, and reduce the risk of unauthorized changes.

User management is especially important in enterprise networks, ISP environments, and networks where multiple technicians administer the same infrastructure.

# 🗁 Understanding RouterOS User Management

RouterOS user management consists of three main components:

* **Users:** Individual accounts used to authenticate administrators.
* **Groups:** Collections of permission policies assigned to users.
* **Policies:** Permissions that determine which operations users can perform.

A user account is associated with a group. The group's policies determine the user's administrative capabilities.

For example:

| User | Group | Responsibility |
| --- | --- | --- |
| `admin` | `full` | Complete router administration |
| `network-admin` | `full` | Network configuration |
| `monitor` | `read` | View router configuration |
| `operator` | Custom group | Limited operational tasks |

The available permissions depend on the group assigned to the account.

# 🗁 Viewing Existing Users

Use the following command to display RouterOS user accounts:

```routeros
/user print
```

For detailed information:

```routeros
/user print detail
```

Example output:

```text
Flags: X - disabled
 0 name="admin" group=full
 1 name="monitor" group=read
```

The output identifies configured accounts and their assigned groups.

To inspect a particular account:

```routeros
/user print detail where name="admin"
```

# 🗁 Understanding User Groups

User groups define the permission policies available to their members.

RouterOS includes predefined groups, commonly including:

| Group | Purpose |
| --- | --- |
| `full` | Full administrative access |
| `read` | Read-only access |
| `write` | Configuration modification without full administrative privileges |

Inspect available groups:

```routeros
/user group print
```

Display detailed group policies:

```routeros
/user group print detail
```

A user's group determines which operations are permitted. Assign groups carefully because excessive permissions can allow unintended configuration changes.

# 🗁 Understanding RouterOS User Policies

RouterOS uses permission policies to control administrative operations.

Common policies include:

| Policy | Purpose |
| --- | --- |
| `local` | Local access to the router |
| `telnet` | Access through Telnet |
| `ssh` | Access through SSH |
| `ftp` | Access through FTP |
| `reboot` | Permission to reboot the router |
| `read` | Read configuration and status information |
| `write` | Modify configuration |
| `policy` | Manage user groups and policies |
| `test` | Run testing and diagnostic commands |
| `winbox` | Access through WinBox |
| `password` | Change passwords |
| `web` | Access through WebFig |
| `sniff` | Use packet-sniffing capabilities |
| `sensitive` | Access sensitive information |
| `api` | Access the RouterOS API |
| `romon` | Use RoMON functionality |
| `rest-api` | Access the RouterOS REST API |

The exact policy requirements depend on the operation and access method.

For example, a user may need the `ssh` policy to establish an SSH session and the `read` policy to inspect configuration.

A user with `write` permission can modify configuration, but this does not automatically grant every administrative capability.

# 🗁 Creating a New User

Use `/user add` to create a new RouterOS account.

Example:

```routeros
/user add name=network-admin group=full password="Strong-Unique-Password"
```

This creates a user named `network-admin` with full administrative permissions.

Verify the account:

```routeros
/user print detail where name="network-admin"
```

For production environments, use a strong, unique password and avoid placing real credentials in shared scripts or documentation.

# 🗁 Creating a Read-Only User

A read-only account is useful for network monitoring, troubleshooting, and configuration inspection.

Create a read-only user:

```routeros
/user add name=monitor group=read password="Strong-ReadOnly-Password"
```

The user can inspect permitted configuration and status information without receiving general configuration modification privileges.

Typical use cases include:

* Network monitoring.
* Configuration auditing.
* Troubleshooting.
* Technical support.
* Read-only access for network operators.

A read-only account should not be treated as a substitute for carefully restricting access to sensitive information.

# 🗁 Creating a Custom User Group

Predefined groups may not provide the exact permission combination required by an organization.

RouterOS allows administrators to create custom groups with selected policies.

For example, create a group for network operators:

```routeros
/user group add name=network-operator policy=local,ssh,read,write,test,winbox,web
```

Create a user assigned to that group:

```routeros
/user add name=operator1 group=network-operator password="Strong-Unique-Password"
```

Inspect the custom group:

```routeros
/user group print detail where name="network-operator"
```

The example provides broad configuration capabilities through selected management methods. It is not a strictly read-only or narrowly scoped role.

For production use, select only the policies required by the operator's actual responsibilities.

# 🗁 Modifying User Permissions

Use `/user set` to modify an existing account.

Change a user's group:

```routeros
/user set [find name="monitor"] group=full
```

Assign a custom group:

```routeros
/user set [find name="operator1"] group=network-operator
```

Verify the changes:

```routeros
/user print detail
```

Changing a user's group immediately changes the permissions associated with that account.

# 🗁 Changing User Passwords

RouterOS allows administrators with sufficient permissions to change user passwords.

Change a password:

```routeros
/user set [find name="network-admin"] password="New-Strong-Unique-Password"
```

Verify the account configuration:

```routeros
/user print detail where name="network-admin"
```

Password values are not displayed as ordinary readable password text in user listings.

Use unique passwords for different routers and administrative accounts. Avoid reusing credentials across network infrastructure.

# 🗁 Disabling and Enabling User Accounts

An account can be disabled without deleting its configuration.

Disable an account:

```routeros
/user disable [find name="operator1"]
```

Enable it again:

```routeros
/user enable [find name="operator1"]
```

Inspect account status:

```routeros
/user print
```

Disabling an account is useful when:

* An administrator temporarily leaves a project.
* An account is suspected of being compromised.
* Access is no longer required.
* An account is being reviewed before deletion.

For security-sensitive situations, also review active sessions and revoke any associated credentials that may have been exposed.

# 🗁 Removing User Accounts

Use `/user remove` to delete an account that is no longer required.

Example:

```routeros
/user remove [find name="operator1"]
```

Verify that the account has been removed:

```routeros
/user print
```

Before deleting an account, confirm that it is not required for ongoing administration or operational processes.

Avoid deleting the only usable administrative account.

# 🗁 Managing Active User Sessions

RouterOS provides information about currently active administrative sessions.

Display active users:

```routeros
/user active print
```

The output can help administrators identify:

* Logged-in usernames.
* Active management sessions.
* Connection addresses.
* Access methods.

For example, an active session may show whether an administrator connected through SSH, WinBox, or WebFig.

To terminate a particular active session, first inspect its current entry:

```routeros
/user active print detail
```

Then remove the appropriate active session using its current item identifier:

```routeros
/user active remove 0
```

The item number is only an example. Always inspect the current session list before removing an entry.

Terminating a session does not delete the user account.

# 🗁 Restricting Access by Management Method

RouterOS user policies can restrict which management methods an account may use.

For example, a user group can be configured with SSH and read permissions:

```routeros
/user group add name=ssh-monitor policy=local,ssh,read
```

Create a user in that group:

```routeros
/user add name=ssh-monitor1 group=ssh-monitor password="Strong-Unique-Password"
```

This group is intended for local SSH access with read permissions.

If a user should not access WebFig, do not grant the `web` policy. If WinBox access is not required, do not grant the `winbox` policy.

User policies should be combined with service-level restrictions and firewall rules.

# 🗁 Restricting Management Access by Source Address

RouterOS allows administrators to restrict management services to specific source IP addresses.

For example, restrict SSH access to the management subnet:

```routeros
/ip service set ssh address=192.168.10.0/24
```

Restrict WinBox access:

```routeros
/ip service set winbox address=192.168.10.0/24
```

Restrict WebFig HTTPS access:

```routeros
/ip service set www-ssl address=192.168.10.0/24
```

Inspect the service configuration:

```routeros
/ip service print detail
```

These restrictions apply to the corresponding services, regardless of which user account is used.

They should be combined with input-chain firewall filtering to provide layered protection.

# 🗁 Protecting the Default Administrator Account

The default `admin` account is a common target for unauthorized access attempts.

A better approach is to create a separate administrative account and verify that it works before disabling the default account.

Create a new administrator:

```routeros
/user add name=network-admin group=full password="Strong-Unique-Password"
```

Verify the account:

```routeros
/user print detail where name="network-admin"
```

Open a separate management session using the new account and confirm that it has the required access.

Only after successful verification should you consider disabling the default account:

```routeros
/user disable [find name="admin"]
```

Do not disable the existing administrator account until an alternative administrative account has been tested successfully.

# 🗁 User Management Through WinBox

User accounts can also be managed through WinBox.

Open:

**System → Users**

The user management window provides access to:

* User accounts.
* User groups.
* Permission policies.
* Password configuration.
* Account status.
* Active sessions.

Typical workflow:

* Open **System → Users**.
* Select **Users** to inspect accounts.
* Select **Add New** to create an account.
* Enter the username and password.
* Select the appropriate group.
* Apply the configuration.

To manage group permissions, open the **Groups** section and inspect the available policy options.

# 🗁 User Management Through WebFig

WebFig provides graphical access to RouterOS user management.

Open:

**System → Users**

From this section, administrators can manage user accounts and groups according to their permissions.

Typical operations include:

* Creating users.
* Changing passwords.
* Assigning groups.
* Disabling accounts.
* Removing accounts.
* Reviewing group policies.

The exact layout may vary between RouterOS versions.

# 🗁 User Management Security Best Practices

A secure user management strategy should follow the principle of least privilege.

Recommended practices:

* Create individual accounts for administrators.
* Avoid sharing administrator credentials.
* Assign only the permissions required for each role.
* Use strong, unique passwords.
* Prefer SSH for remote CLI administration.
* Restrict management services to trusted networks.
* Disable unused accounts.
* Remove accounts when access is no longer required.
* Review active sessions regularly.
* Audit user groups and permission policies.
* Protect sensitive configuration information.
* Maintain backups before major administrative changes.
* Keep RouterOS updated.

For an ISP or enterprise environment, administrative access should be separated by operational responsibility.

For example:

| Role | Suggested Access |
| --- | --- |
| Network Administrator | Full configuration access |
| NOC Monitoring | Read-only monitoring access |
| Field Technician | Limited operational permissions |
| Security Administrator | Security-related administrative access |
| Automation Account | Only the permissions required by its automation tasks |

These are organizational roles, not predefined RouterOS groups. Create custom groups based on the actual operations each role needs to perform.

# 🗁 Practical Lab: Create Administrative and Monitoring Users

This lab demonstrates how to create separate accounts for network administration and monitoring.

**Lab requirements:**

| Component | Configuration |
| --- | --- |
| Router | MikroTik RouterOS v7 |
| Administrator | `network-admin` |
| Monitoring user | `monitor` |
| Administrator group | `full` |
| Monitoring group | `read` |
| Management subnet | `192.168.10.0/24` |

**Inspect existing users and groups:**

```routeros
/user print
/user group print detail
```

**Create the administrative account:**

```routeros
/user add name=network-admin group=full password="Admin-Strong-Password"
```

**Create the monitoring account:**

```routeros
/user add name=monitor group=read password="Monitor-Strong-Password"
```

**Verify both accounts:**

```routeros
/user print detail
```

**Restrict SSH access to the management subnet:**

```routeros
/ip service set ssh address=192.168.10.0/24
```

**Inspect active sessions:**

```routeros
/user active print
```

**Test administrative access:**

Connect through SSH using the administrative account:

```bash
ssh network-admin@192.168.10.1
```

Verify that the account can inspect and modify permitted configuration.

**Test monitoring access:**

Connect using the monitoring account:

```bash
ssh monitor@192.168.10.1
```

Try inspecting configuration:

```routeros
/system resource print
/interface print
/ip address print
```

Then attempt a configuration modification in a controlled lab environment. The read-only group should prevent ordinary configuration changes.

**Final verification:**

```routeros
/user print detail
/user group print detail
/user active print
```

The expected result is two separate accounts with different permission levels.

# 🗁 Key Takeaways

* RouterOS user management controls administrative access.
* Users are assigned to groups.
* Groups contain permission policies.
* Predefined groups include `full`, `read`, and `write`.
* Custom groups can provide role-specific permissions.
* User accounts can be created, modified, disabled, enabled, and removed.
* Active sessions can be inspected and terminated.
* Management services can be restricted by source address.
* Individual accounts and least-privilege permissions improve security.
* Administrative access should be reviewed regularly.

# 🗁 Official Documentation

* MikroTik RouterOS documentation: <https://help.mikrotik.com/docs/>
* MikroTik User Management: <https://help.mikrotik.com/docs/>
