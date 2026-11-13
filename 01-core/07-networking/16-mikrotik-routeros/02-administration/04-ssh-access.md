# SSH Access

SSH (Secure Shell) is a cryptographic network protocol used to securely access and administer remote systems over an IP network.

In MikroTik RouterOS, SSH provides secure command-line access to a router. Administrators can execute commands, inspect configurations, troubleshoot network problems, and manage devices remotely.

SSH is an essential management method for MikroTik routers, particularly in ISP environments, enterprise networks, and multi-router deployments.

# 🗁 What Is SSH?

SSH provides an encrypted communication channel between an SSH client and an SSH server.

In a MikroTik environment:

* The MikroTik router acts as the SSH server.
* The administrator's computer acts as the SSH client.
* The SSH client connects to the router's management IP address.
* The administrator authenticates using an authorized account.
* RouterOS provides a command-line session after successful authentication.

SSH encrypts transmitted data, including authentication information and command output.

Unlike Telnet, SSH protects the management session from being transmitted as readable plaintext.

# 🗁 SSH vs Telnet

SSH and Telnet are both used for remote command-line access, but they differ significantly in security.

| Feature | SSH | Telnet |
| --- | --- | --- |
| Default port | 22 | 23 |
| Encryption | Yes | No |
| Authentication protection | Encrypted | Unencrypted |
| Secure remote administration | Yes | No |
| Data confidentiality | Yes | No |
| Recommended for production | Yes | No |

SSH should be preferred for remote RouterOS administration.

# 🗁 How SSH Works

SSH establishes a secure connection through several stages.

**Connection establishment**

The SSH client connects to the router's SSH service, normally using TCP port 22.

**Protocol negotiation**

The client and server negotiate supported SSH protocol algorithms.

**Server authentication**

The client verifies the server's host key. This helps ensure that it is connecting to the expected router.

**Key exchange**

The client and server establish shared session encryption keys.

**User authentication**

The administrator authenticates using an allowed method, such as a password or public-key authentication.

**Interactive session**

After authentication, the client receives access to the RouterOS CLI, subject to the user's permissions.

# 🗁 Requirements for SSH Access

Before connecting to a MikroTik router through SSH, ensure that:

* RouterOS is running.
* The router has a reachable management IP address.
* The SSH service is enabled.
* TCP port 22 is permitted by the router's firewall.
* The SSH source address is permitted by the service configuration.
* A valid RouterOS user account exists.
* The workstation has an SSH client installed.

Most Linux distributions include the OpenSSH client.

Check whether the SSH client is available:

```bash
ssh -V
```

On Ubuntu or Debian, install the client if necessary:

```bash
sudo apt update
sudo apt install openssh-client
```

# 🗁 Enabling SSH on RouterOS

RouterOS provides SSH as an IP service.

Inspect the available services:

```routeros
/ip service print
```

Find the SSH service in the output.

Enable SSH:

```routeros
/ip service enable ssh
```

Set the default SSH port explicitly if required:

```routeros
/ip service set ssh port=22
```

Verify the service:

```routeros
/ip service print detail
```

The SSH service should be enabled and listening on the configured port.

# 🗁 Restricting SSH Access

SSH should not be accessible from every network interface or source address.

RouterOS allows administrators to restrict management services to specific source IP addresses or networks.

For example, allow SSH access only from the management subnet:

```routeros
/ip service set ssh address=192.168.10.0/24
```

This allows SSH connections from addresses within `192.168.10.0/24`.

You can inspect the configuration:

```routeros
/ip service print detail where name=ssh
```

The service address restriction is an additional security control. Firewall rules should also enforce the intended management access policy.

# 🗁 Connecting to MikroTik Through SSH

From a Linux workstation, use the `ssh` command:

```bash
ssh admin@192.168.88.1
```

The general syntax is:

```bash
ssh username@router-ip-address
```

Example with a different management address:

```bash
ssh network-admin@192.168.10.1
```

If the connection is successful, the SSH client may display a host-key verification prompt the first time it connects.

Review the host-key fingerprint and confirm that it belongs to the expected router before accepting it.

After authentication, the RouterOS terminal prompt appears.

```text
[network-admin@MikroTik] >
```

You can now execute RouterOS commands.

For example:

```routeros
/system identity print
/system resource print
/interface print
/ip address print
```

# 🗁 Connecting Through a Custom SSH Port

RouterOS allows administrators to change the SSH service port.

For example, configure SSH to use TCP port `2222`:

```routeros
/ip service set ssh port=2222
```

Connect using the custom port:

```bash
ssh -p 2222 admin@192.168.88.1
```

The `-p` option specifies the destination SSH port.

Changing the default port can reduce routine automated scanning noise, but it does not replace authentication, firewall filtering, or encryption.

# 🗁 SSH Host Keys

SSH host keys identify the server to which a client connects.

When connecting to a router for the first time, the SSH client normally asks whether to trust the server's host key.

The client stores accepted host keys in its known-hosts database.

On Linux, this file is commonly located at:

```text
~/.ssh/known_hosts
```

When reconnecting, the SSH client compares the server's presented host key with the previously stored key.

If the key changes unexpectedly, the client displays a warning.

A host-key change can occur after:

* RouterOS host-key regeneration.
* Router replacement.
* Device reset.
* Restoration or migration to another router.
* A legitimate change in SSH host-key configuration.

Verify the router's identity and host-key fingerprint before accepting a changed key.

Do not automatically ignore host-key warnings.

# 🗁 Password-Based Authentication

Password authentication allows an administrator to log in using a RouterOS username and password.

Example:

```bash
ssh admin@192.168.88.1
```

The SSH client prompts for the password when required.

The password is not displayed while typing.

For security:

* Use a strong, unique password.
* Avoid shared administrator accounts.
* Create individual accounts for administrators.
* Restrict management access to trusted networks.
* Disable unused accounts.

Inspect RouterOS users:

```routeros
/user print
```

Create a dedicated administrator account:

```routeros
/user add name=network-admin group=full password="Use-A-Strong-Unique-Password"
```

The example password is a placeholder. Use a secure password and avoid embedding real credentials in scripts, documentation, or shared command history.

# 🗁 SSH Public-Key Authentication

SSH public-key authentication allows an administrator to authenticate using a cryptographic key pair instead of entering a password for every connection.

A key pair contains:

| Key | Purpose |
| --- | --- |
| Private key | Kept securely on the administrator's workstation |
| Public key | Installed on the MikroTik router |

The private key must never be shared.

Public-key authentication provides a convenient method for securely managing routers, especially when administrators work with multiple devices.

# 🗁 Generating an SSH Key Pair on Linux

Modern OpenSSH installations support Ed25519 keys.

Generate a new key pair:

```bash
ssh-keygen -t ed25519 -C "network-admin@example.com"
```

The command creates a private key and a public key.

The default filenames are:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

During generation, OpenSSH asks for a location and an optional passphrase.

Using a passphrase provides additional protection if the private-key file is exposed.

Set appropriate permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

Display the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the public key content for installation on the router.

Never upload or share the private key.

# 🗁 Importing a Public Key into RouterOS

RouterOS supports importing SSH public keys for user authentication.

First, transfer the public-key file to the router using a supported file-transfer method, such as SCP or SFTP.

For example, from a Linux workstation:

```bash
scp ~/.ssh/id_ed25519.pub admin@192.168.88.1:/
```

Then inspect the router's files:

```routeros
/file print
```

Import the public key and associate it with the intended RouterOS user:

```routeros
/user ssh-keys import public-key-file=id_ed25519.pub user=network-admin
```

Verify the imported key:

```routeros
/user ssh-keys print
```

The account must already exist, and the public key must be in a supported format.

# 🗁 Connecting with a Private Key

After importing the public key, connect from the workstation using the corresponding private key.

```bash
ssh -i ~/.ssh/id_ed25519 network-admin@192.168.88.1
```

If the private key is protected by a passphrase, the SSH client requests it when needed.

The public and private keys must belong to the same key pair.

# 🗁 Using SSH Agent

OpenSSH provides an SSH agent that can hold unlocked private keys during a login session.

Start the agent:

```bash
eval "$(ssh-agent -s)"
```

Add the private key:

```bash
ssh-add ~/.ssh/id_ed25519
```

List loaded keys:

```bash
ssh-add -l
```

Remove a specific key:

```bash
ssh-add -d ~/.ssh/id_ed25519
```

Using an SSH agent reduces the need to repeatedly enter a private-key passphrase.

# 🗁 Managing Multiple MikroTik Routers with SSH Config

When administering multiple routers, repeatedly typing usernames, IP addresses, ports, and key paths becomes inconvenient.

OpenSSH supports a client configuration file:

```text
~/.ssh/config
```

Example configuration:

```sshconfig
Host mikrotik-core
    HostName 192.168.10.1
    User network-admin
    Port 22
    IdentityFile ~/.ssh/id_ed25519

Host mikrotik-branch
    HostName 192.168.20.1
    User network-admin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

Set secure permissions:

```bash
chmod 600 ~/.ssh/config
```

Connect to the core router:

```bash
ssh mikrotik-core
```

Connect to the branch router:

```bash
ssh mikrotik-branch
```

This approach simplifies access to many routers and keeps connection settings organized.

# 🗁 Executing Commands Remotely Through SSH

SSH can execute a command directly on a remote router without opening an interactive session.

For example:

```bash
ssh network-admin@192.168.10.1 "/system identity print"
```

Inspect router resources:

```bash
ssh network-admin@192.168.10.1 "/system resource print"
```

Inspect IP addresses:

```bash
ssh network-admin@192.168.10.1 "/ip address print"
```

This capability is useful for administrative scripts and monitoring systems.

For example, a Linux shell script can execute commands against several routers.

```bash
#!/usr/bin/env bash

ROUTERS=(
    "192.168.10.1"
    "192.168.20.1"
    "192.168.30.1"
)

for ROUTER in "${ROUTERS[@]}"; do
    echo "Checking router: $ROUTER"

    ssh -o BatchMode=yes "network-admin@$ROUTER" \
        "/system identity print"
done
```

This example assumes that SSH authentication has already been configured for each router.

# 🗁 SSH Port Forwarding and Tunneling

SSH supports port forwarding, which can provide secure access to services through an encrypted SSH connection.

For example, local port forwarding can expose a remote service through a local workstation port.

General syntax:

```bash
ssh -L local-port:destination-host:destination-port username@ssh-server
```

SSH forwarding can be useful when accessing management services that should not be exposed directly to an untrusted network.

However, the forwarding behavior depends on the SSH server's configuration and the network reachability of the destination service.

For RouterOS administration, a VPN is generally a better long-term approach for securely accessing an entire management network.

# 🗁 SSH Firewall Configuration

The RouterOS firewall should permit SSH only from trusted management sources.

Suppose:

| Parameter | Value |
| --- | --- |
| Router management IP | `192.168.10.1` |
| Management subnet | `192.168.10.0/24` |
| SSH port | `22` |

An example input-chain policy:

```routeros
/ip firewall filter
add chain=input action=accept protocol=tcp dst-port=22 src-address=192.168.10.0/24 comment="Allow SSH from management LAN"
add chain=input action=drop protocol=tcp dst-port=22 comment="Block SSH from other sources"
```

These rules are examples, not a complete firewall configuration.

Place them appropriately within the existing input-chain policy. Ensure that legitimate established connections and other required router services remain permitted.

If SSH uses a custom port, update the firewall rules accordingly.

The service-level source restriction and firewall policy should be aligned.

# 🗁 SSH Troubleshooting

SSH connection failures can result from incorrect addressing, disabled services, firewall restrictions, authentication problems, or host-key verification issues.

| Problem | Possible Cause | What to Check |
| --- | --- | --- |
| Connection timed out | Firewall or routing problem | Check reachability and input rules |
| Connection refused | SSH service disabled or wrong port | Inspect `/ip service` |
| Permission denied | Invalid credentials or key | Check username and authentication configuration |
| Host key verification failed | Stored host key does not match | Verify router identity and fingerprint |
| No route to host | Routing or addressing problem | Check IP addressing and routing |
| Connection works locally but not remotely | Service restrictions or firewall | Check source-address restrictions |
| Public-key authentication fails | Incorrect key, user association, or format | Inspect imported SSH keys |

Useful RouterOS commands:

```routeros
/ip service print detail
/user print
/user ssh-keys print
/ip firewall filter print
/ip address print
/ip route print
/log print
```

Useful Linux diagnostic commands:

```bash
ping -c 4 192.168.10.1
```

Check whether TCP port 22 is reachable:

```bash
nc -vz 192.168.10.1 22
```

Enable verbose SSH output:

```bash
ssh -vvv network-admin@192.168.10.1
```

Verbose output helps identify connection negotiation, host-key verification, and authentication failures.

# 🗁 SSH Security Best Practices

SSH is a secure management protocol, but its security still depends on correct configuration.

Recommended practices:

* Use SSH instead of Telnet.
* Restrict SSH access to trusted management networks.
* Prefer public-key authentication where appropriate.
* Protect private keys with secure permissions and passphrases.
* Use individual RouterOS accounts for administrators.
* Assign only the permissions required for each account.
* Keep RouterOS updated.
* Review SSH service configuration regularly.
* Monitor authentication and system logs.
* Avoid exposing SSH directly to the public internet.
* Use VPN access for remote management.
* Verify host keys when connecting to routers.
* Remove unused public keys and accounts.

Inspect SSH service configuration:

```routeros
/ip service print detail where name=ssh
```

Inspect imported keys:

```routeros
/user ssh-keys print
```

Inspect system logs:

```routeros
/log print
```

# 🗁 Practical Lab: Configure SSH Key Authentication

This lab demonstrates secure SSH access using an Ed25519 key pair.

**Lab topology:**

| Component | Configuration |
| --- | --- |
| Router | MikroTik RouterOS v7 |
| Router IP | `192.168.10.1` |
| Management subnet | `192.168.10.0/24` |
| SSH port | `22` |
| SSH username | `network-admin` |
| Authentication | Public-key authentication |

**Create a dedicated RouterOS user:**

```routeros
/user add name=network-admin group=full password="Use-A-Strong-Unique-Password"
```

**Enable SSH and restrict its source addresses:**

```routeros
/ip service set ssh disabled=no port=22 address=192.168.10.0/24
```

**Generate an Ed25519 key pair on Linux:**

```bash
ssh-keygen -t ed25519 -C "network-admin@example.com"
```

**Transfer the public key to the router:**

```bash
scp ~/.ssh/id_ed25519.pub admin@192.168.10.1:/
```

**Import the public key:**

```routeros
/user ssh-keys import public-key-file=id_ed25519.pub user=network-admin
```

**Verify the imported key:**

```routeros
/user ssh-keys print
```

**Connect using the private key:**

```bash
ssh -i ~/.ssh/id_ed25519 network-admin@192.168.10.1
```

**Verify the RouterOS session:**

```routeros
/system identity print
/system resource print
```

**Verify the final configuration:**

```routeros
/ip service print detail where name=ssh
/user ssh-keys print
```

The administrator should be able to authenticate using the private key associated with the imported public key.

# 🗁 Official Documentation

* MikroTik RouterOS documentation: <https://help.mikrotik.com/docs/>
* MikroTik IP Services: <https://help.mikrotik.com/docs/spaces/ROS/pages/103841820/Services>
* MikroTik User Management: <https://help.mikrotik.com/docs/>
* OpenSSH Manual: <https://man.openbsd.org/ssh>
* OpenSSH Key Generation: <https://man.openbsd.org/ssh-keygen>

SSH provides secure remote command-line administration for MikroTik routers. Restricting access, using strong authentication, verifying host keys, and maintaining proper firewall policies are essential parts of a secure RouterOS management environment.
