# GitHub SSH Authentication

SSH (Secure Shell) is a cryptographic network protocol used to establish secure connections between computers.

GitHub supports SSH authentication, allowing developers to interact with remote repositories without repeatedly entering their credentials.

Once an SSH key is configured, you can perform Git operations such as:

* Cloning repositories.
* Fetching changes.
* Pulling updates.
* Pushing commits.
* Managing remote repositories.

SSH authentication is commonly used by developers who work with GitHub from the command line.

# 🗁 What Is SSH?

SSH stands for **Secure Shell**.

It provides an encrypted communication channel between a client and a remote server over an unsecured network.

SSH is commonly used for:

* Secure remote server access.
* Authentication using cryptographic keys.
* Secure file transfers.
* Remote command execution.
* Git repository authentication.

SSH normally uses TCP port `22`.

For GitHub, SSH connections use the SSH user `git` and the hostname `github.com`.

Example:

```bash
ssh -T git@github.com
```

Here:

| Component | Description |
| --- | --- |
| `ssh` | SSH client command |
| `-T` | Disable pseudo-terminal allocation |
| `git` | SSH username used by GitHub |
| `github.com` | GitHub SSH server |

GitHub does not provide interactive shell access through this connection. SSH is used to authenticate Git operations.

# 🗁 Why Use SSH with GitHub?

SSH provides a convenient way to authenticate with GitHub.

**Main advantages:**

* No need to enter a username and personal access token for every Git operation.
* Public-key cryptography provides secure authentication.
* Private keys remain on your local machine.
* SSH keys can be protected with passphrases.
* Multiple GitHub accounts can use separate SSH keys.
* SSH works well with automated development workflows when configured appropriately.

# 🗁 SSH vs HTTPS

GitHub supports two common authentication methods for Git operations: SSH and HTTPS.

| Feature | SSH | HTTPS |
| --- | --- | --- |
| Connection | SSH protocol | HTTPS protocol |
| Authentication | SSH key pair | Personal access token or credential manager |
| Remote URL | `git@github.com:OWNER/REPO.git` | `https://github.com/OWNER/REPO.git` |
| Credential handling | SSH agent and private key | Credential manager or token |
| Default port | TCP 22 | TCP 443 |
| Setup | Requires SSH key configuration | Usually easier to start |
| Common use | Developer workstations and servers | Workstations, restricted networks, and CI environments |

**SSH remote example:**

```bash
git clone git@github.com:username/repository.git
```

**HTTPS remote example:**

```bash
git clone https://github.com/username/repository.git
```

Both methods support normal Git operations. Choose the method that best fits your environment.

# 🗁 How SSH Authentication Works

SSH authentication with GitHub uses public-key cryptography.

You generate a pair of mathematically related keys:

* **Private key:** Stored securely on your local machine.
* **Public key:** Added to your GitHub account.

The private key proves your identity without being transmitted to GitHub.

### 🖆 Authentication workflow

```text
              Local Machine
                   |
                   |
          SSH Client / Git
                   |
                   |
             SSH Agent
                   |
                   |
          Private SSH Key
                   |
                   |
             Encrypted
          Authentication
                   |
                   v
             GitHub Server
                   |
                   |
          Checks Public Key
                   |
                   |
          Authentication
              Successful
                   |
                   v
           Git Operations
       Clone / Pull / Push
```

### 🖆 Authentication process

* Generate a public/private SSH key pair.
* Add the public key to your GitHub account.
* Keep the private key on your local machine.
* Start the SSH agent and load your private key.
* Connect to GitHub using SSH.
* GitHub verifies that the client can authenticate using the corresponding private key.
* Once authenticated, GitHub allows authorized repository operations.

**Important:** Never upload or share your private SSH key.

# 🗁 Understanding SSH Key Files

SSH stores configuration and key files inside the `.ssh` directory.

On Linux and macOS:

```bash
~/.ssh/
```

On Windows:

```text
C:\Users\YourUsername\.ssh\
```

Common SSH files:

| File | Purpose |
| --- | --- |
| `id_ed25519` | Ed25519 private key |
| `id_ed25519.pub` | Ed25519 public key |
| `id_rsa` | RSA private key |
| `id_rsa.pub` | RSA public key |
| `known_hosts` | Records identities of previously contacted SSH servers |
| `config` | SSH client configuration |
| `authorized_keys` | Public keys authorized to access an SSH server |

The `.pub` extension identifies a public key.

The corresponding file without `.pub` is the private key.

# 🗁 Check for Existing SSH Keys

Before generating a new key, check whether SSH keys already exist.

```bash
ls -al ~/.ssh
```

Example output:

```text
drwx------  2 user user 4096 Nov  7 10:00 .
drwxr-x--- 20 user user 4096 Nov  7 09:00 ..
-rw-------  1 user user  464 Nov  7 10:00 id_ed25519
-rw-r--r--  1 user user  104 Nov  7 10:00 id_ed25519.pub
```

In this example:

* `id_ed25519` is the private key.
* `id_ed25519.pub` is the public key.

If you already have an SSH key, you may be able to reuse it.

However, consider generating a separate key for GitHub if you want to isolate authentication or manage multiple accounts.

# 🗁 Generate a New SSH Key

Ed25519 is a modern and recommended SSH key algorithm for most supported environments.

Generate a new key:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Explanation:

| Option | Description |
| --- | --- |
| `ssh-keygen` | Generates SSH keys |
| `-t ed25519` | Selects the Ed25519 algorithm |
| `-C` | Adds a descriptive comment |

Example:

```bash
ssh-keygen -t ed25519 -C "developer@example.com"
```

You will see a prompt similar to:

```text
Enter file in which to save the key
(/home/user/.ssh/id_ed25519):
```

Press Enter to use the default location.

Next:

```text
Enter passphrase (empty for no passphrase):
```

Enter a passphrase to protect your private key, then confirm it.

The generated files are:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

**Alternative: RSA**

If your environment does not support Ed25519, you can generate an RSA key:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Ed25519 is generally preferable when supported.

# 🗁 Start the SSH Agent

The SSH agent manages private keys and can keep decrypted keys available for authentication.

Start the agent on Linux or macOS:

```bash
eval "$(ssh-agent -s)"
```

Example output:

```text
Agent pid 12345
```

The agent is now running in the current environment.

### 🖆 Windows

On Windows, OpenSSH Authentication Agent can be managed through PowerShell.

Check its status:

```powershell
Get-Service ssh-agent
```

Start the service:

```powershell
Start-Service ssh-agent
```

Configure it to start automatically:

```powershell
Set-Service -Name ssh-agent -StartupType Automatic
```

Run PowerShell with the permissions required to manage the service.

# 🗁 Add Your SSH Key to the Agent

After starting the SSH agent, add your private key.

Linux and macOS:

```bash
ssh-add ~/.ssh/id_ed25519
```

Windows PowerShell:

```powershell
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

If the private key has a passphrase, SSH may ask you to enter it.

Verify loaded keys:

```bash
ssh-add -l
```

To display the public keys associated with loaded identities:

```bash
ssh-add -L
```

If no identities are loaded, the agent may report that it has no identities.

# 🗁 Add Your SSH Key to GitHub

GitHub needs your public key to recognize your SSH identity.

First, display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output.

It typically begins with:

```text
ssh-ed25519
```

### 🖆 Copy the public key

Linux with `xclip`:

```bash
xclip -selection clipboard < ~/.ssh/id_ed25519.pub
```

macOS:

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

Windows PowerShell:

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

If clipboard utilities are unavailable, open the `.pub` file and copy its complete contents manually.

### 🖆 Add the key through GitHub

Open:

<https://github.com/settings/keys>

Then:

* Navigate to **Settings**.
* Select **SSH and GPG keys**.
* Click **New SSH key**.
* Enter a descriptive title, such as `Personal Laptop`.
* Select **Authentication Key** as the key type.
* Paste your public key.
* Click **Add SSH key**.
* Confirm your GitHub password or other requested verification.

Your public key is now associated with your GitHub account.

**Never paste your private key into GitHub.** Only the `.pub` file should be uploaded.

# 🗁 Test Your SSH Connection

After adding the public key, test the connection.

```bash
ssh -T git@github.com
```

The first time you connect, SSH may display a host authenticity prompt:

```text
The authenticity of host 'github.com' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Verify the fingerprint against GitHub's published SSH host key fingerprints before accepting it.

After verification, enter:

```text
yes
```

A successful authentication typically displays:

```text
Hi username! You've successfully authenticated,
but GitHub does not provide shell access.
```

This message means:

* Your SSH connection reached GitHub.
* GitHub recognized your SSH key.
* Authentication succeeded.
* Interactive shell access is not provided.

The command may return a non-zero exit status even though authentication succeeded. The greeting is the important confirmation.

# 🗁 Use SSH with Git Repositories

Once SSH authentication is configured, you can use SSH remote URLs.

### 🖆 Clone a repository using SSH

```bash
git clone git@github.com:username/repository.git
```

### 🖆 Check the remote URL

Inside an existing repository:

```bash
git remote -v
```

Example HTTPS output:

```text
origin  https://github.com/username/repository.git (fetch)
origin  https://github.com/username/repository.git (push)
```

### 🖆 Change HTTPS to SSH

If your repository currently uses HTTPS, change the remote URL:

```bash
git remote set-url origin git@github.com:username/repository.git
```

Verify:

```bash
git remote -v
```

Expected output:

```text
origin  git@github.com:username/repository.git (fetch)
origin  git@github.com:username/repository.git (push)
```

### 🖆 Push and pull using SSH

```bash
git pull origin main
```

```bash
git push origin main
```

You can now authenticate through SSH when accessing repositories for which your GitHub account has permission.

# 🗁 Configure Multiple GitHub Accounts

Developers sometimes use separate GitHub accounts for personal and professional projects.

For example:

* Personal account: `personal-user`
* Work account: `work-user`

Using separate SSH keys helps keep authentication identities isolated.

### 🖆 Generate separate SSH keys

Personal account:

```bash
ssh-keygen -t ed25519 -C "personal@example.com" -f ~/.ssh/id_ed25519_personal
```

Work account:

```bash
ssh-keygen -t ed25519 -C "work@example.com" -f ~/.ssh/id_ed25519_work
```

This creates:

```text
~/.ssh/
├── id_ed25519_personal
├── id_ed25519_personal.pub
├── id_ed25519_work
└── id_ed25519_work.pub
```

Add each public key to its corresponding GitHub account.

### 🖆 Configure SSH config

Open the SSH configuration file:

```bash
nano ~/.ssh/config
```

Add:

```sshconfig
# Personal GitHub Account
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes

# Work GitHub Account
Host github.com-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes
```

Save the file.

The configuration defines two SSH host aliases:

| Alias | SSH key |
| --- | --- |
| `github.com-personal` | `id_ed25519_personal` |
| `github.com-work` | `id_ed25519_work` |

`IdentitiesOnly yes` instructs SSH to use the configured identity rather than offering unrelated agent keys.

### 🖆 Clone repositories using account-specific aliases

Personal repository:

```bash
git clone git@github.com-personal:personal-user/repository.git
```

Work repository:

```bash
git clone git@github.com-work:work-user/repository.git
```

### 🖆 Update an existing repository

For a personal repository:

```bash
git remote set-url origin git@github.com-personal:personal-user/repository.git
```

For a work repository:

```bash
git remote set-url origin git@github.com-work:work-user/repository.git
```

### 🖆 Test each account

Personal:

```bash
ssh -T git@github.com-personal
```

Work:

```bash
ssh -T git@github.com-work
```

Each connection should authenticate as the corresponding GitHub account.

# 🗁 SSH Key Passphrases

A passphrase adds protection to your private key.

Without a passphrase, anyone who obtains the private key may be able to use it, subject to other access controls.

With a passphrase, the private key must be unlocked before it can be used.

Example:

```bash
ssh-keygen -t ed25519 -C "developer@example.com"
```

When prompted, enter a passphrase.

The SSH agent can cache the unlocked key so you do not have to enter the passphrase for every Git operation.

To remove a loaded key from the agent:

```bash
ssh-add -d ~/.ssh/id_ed25519
```

To remove all loaded identities:

```bash
ssh-add -D
```

These commands affect the agent, not the key files stored on disk.

# 🗁 SSH Key Security and Management

SSH keys are credentials. Protect them accordingly.

### 🖆 Protect private keys

Private keys should never be:

* Uploaded to GitHub.
* Committed to a Git repository.
* Shared through email or messaging applications.
* Stored in public cloud storage without appropriate protection.
* Included in application source code.

### 🖆 Set appropriate file permissions

On Linux and macOS:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/config
```

Private keys should not be readable by other local users.

### 🖆 Review SSH keys on GitHub

Open:

<https://github.com/settings/keys>

Review the keys associated with your account.

Remove keys that are:

* No longer in use.
* Associated with devices you no longer control.
* Suspected of being compromised.
* Replaced by newer keys.

Removing an SSH key from GitHub prevents that key from authenticating to your account.

### 🖆 Rotate SSH keys

Key rotation means replacing an existing key with a new one.

A typical rotation process:

* Generate a new SSH key pair.
* Add the new public key to GitHub.
* Test the new connection.
* Update local SSH configuration if necessary.
* Remove the old public key from GitHub after confirming the new key works.

This avoids losing access during the transition.

# 🗁 SSH Authentication vs Commit Signing

SSH authentication and commit signing are different security features.

| Feature | SSH Authentication | SSH Commit Signing |
| --- | --- | --- |
| Purpose | Authenticate Git operations | Verify commit authenticity |
| Used for | Clone, fetch, pull, push | Signed commits and tags |
| Key configuration | Authentication key | Signing key |
| GitHub setting | SSH and GPG keys | SSH and GPG keys |
| Main benefit | Secure repository access | Verify who signed a commit |

A key configured for authentication is not automatically configured for commit signing.

GitHub supports SSH-based commit signing, but it requires signing configuration in Git.

Example:

```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_signing.pub
git config --global commit.gpgsign true
```

Create a signed commit:

```bash
git commit -S -m "Add signed commit"
```

For GitHub to verify SSH signatures, the relevant public key must also be registered as a signing key.

Using a dedicated signing key is a useful way to separate repository access from commit-signing identity.

# 🖆 Troubleshooting

### 🖆 Permission denied (publickey)

Error:

```text
Permission denied (publickey).
```

Possible causes:

* Public key has not been added to GitHub.
* SSH agent is not running.
* Private key is not loaded.
* SSH is using the wrong private key.
* Repository remote URL is incorrect.

Check loaded keys:

```bash
ssh-add -l
```

Add your key:

```bash
ssh-add ~/.ssh/id_ed25519
```

Test authentication:

```bash
ssh -T git@github.com
```

### 🖆 SSH agent has no identities

Error:

```text
The agent has no identities.
```

Solution:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

On Windows, make sure the OpenSSH Authentication Agent service is running.

### 🖆 Wrong SSH key is being used

Use verbose SSH output:

```bash
ssh -vT git@github.com
```

Look for lines showing which identity files are offered.

For multiple accounts, verify the `Host` alias and `IdentityFile` settings in `~/.ssh/config`.

### 🖆 Repository uses HTTPS instead of SSH

Check:

```bash
git remote -v
```

If the URL starts with `https://`, change it:

```bash
git remote set-url origin git@github.com:username/repository.git
```

### 🖆 Bad permissions on private key

SSH may reject a private key with overly permissive permissions.

Fix the permissions:

```bash
chmod 600 ~/.ssh/id_ed25519
```

### 🖆 Host key verification failed

Error:

```text
Host key verification failed.
```

This can happen when the server host key changes or the saved host entry does not match.

Inspect the known-hosts entry:

```bash
ssh-keygen -F github.com
```

Verify GitHub's current published host key fingerprints before changing any saved entry.

If the entry is confirmed to be stale, remove it:

```bash
ssh-keygen -R github.com
```

Then reconnect and verify the new fingerprint.

Never blindly accept an unexpected host key change.

### 🖆 SSH connection times out

Possible causes:

* Network restrictions.
* Firewall rules.
* ISP restrictions on outbound TCP port 22.
* SSH server connectivity problems.

GitHub also provides SSH access over port 443.

Test the connection:

```bash
ssh -T -p 443 git@ssh.github.com
```

To configure it in `~/.ssh/config`:

```sshconfig
Host github.com
    HostName ssh.github.com
    User git
    Port 443
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

This uses GitHub's SSH endpoint on TCP port 443.

### 🖆 Debug SSH authentication

For detailed connection diagnostics:

```bash
ssh -vT git@github.com
```

For more detailed output:

```bash
ssh -vvvT git@github.com
```

These commands help identify:

* Which SSH configuration is loaded.
* Which private keys are considered.
* Which keys are offered.
* Whether the SSH agent is working.
* Where authentication fails.

# 🖆 Best Practices

* Prefer Ed25519 keys when supported.
* Protect private keys with a strong passphrase.
* Never share private keys.
* Add only public keys to GitHub.
* Use separate keys for different accounts or security contexts.
* Use descriptive SSH key titles.
* Review and remove unused keys.
* Rotate keys when devices are replaced or credentials may be compromised.
* Verify SSH host fingerprints when connecting to a server for the first time.
* Use `IdentitiesOnly yes` when managing multiple SSH identities.
* Keep SSH configuration organized and readable.
* Use dedicated signing keys when separating commit signing from authentication.

# 🖆 Practical Exercises

### 🖆 Exercise: Generate an SSH key

Generate an Ed25519 SSH key pair and inspect the generated files.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 🖆 Exercise: Configure GitHub authentication

* Start the SSH agent.
* Add your private key.
* Add your public key to GitHub.
* Test your SSH connection.

```bash
ssh-add ~/.ssh/id_ed25519
ssh -T git@github.com
```

### 🖆 Exercise: Clone using SSH

Clone a GitHub repository using its SSH URL.

```bash
git clone git@github.com:username/repository.git
```

### 🖆 Exercise: Convert HTTPS to SSH

Change an existing repository's remote URL from HTTPS to SSH.

```bash
git remote set-url origin git@github.com:username/repository.git
```

Verify:

```bash
git remote -v
```

### 🖆 Exercise: Configure multiple accounts

* Generate separate SSH keys for personal and work accounts.
* Add each public key to the correct GitHub account.
* Configure SSH host aliases.
* Test both connections.
* Clone repositories using the appropriate alias.

# 🖆 Interview Questions

**What is SSH?**

SSH is a secure network protocol that provides encrypted communication and authentication between a client and a remote server.

**What is an SSH key pair?**

An SSH key pair consists of a private key and a mathematically related public key. The private key remains on the client, while the public key is registered with the remote service.

**Why is SSH used with GitHub?**

SSH provides secure authentication for Git operations without requiring users to repeatedly enter HTTPS credentials.

**What is the difference between a public key and a private key?**

A public key can be shared with services such as GitHub. A private key must remain secret and is used by the client to prove possession of the corresponding identity.

**What is `ssh-agent`?**

`ssh-agent` is a background process that manages private keys and can retain unlocked identities for authentication.

**What is the purpose of `~/.ssh/config`?**

It stores SSH client configuration, including host aliases, usernames, ports, and identity file paths.

**What does `ssh -T git@github.com` do?**

It tests SSH authentication with GitHub without requesting an interactive terminal.

**What does `Permission denied (publickey)` mean?**

SSH authentication failed because GitHub did not accept any public-key identity offered by the client.

**Can one computer use multiple GitHub accounts through SSH?**

Yes. Separate SSH key pairs and host aliases in `~/.ssh/config` can be used to authenticate different accounts.

**What is the difference between SSH authentication and SSH commit signing?**

Authentication verifies access to GitHub repositories, while commit signing provides a cryptographic signature that can be used to verify the identity associated with a commit.

# 🖆 RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

**References**

* [GitHub: Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
* [GitHub: Checking for Existing SSH Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)
* [GitHub: Generating a New SSH Key and Adding It to the SSH Agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
* [GitHub: Adding a New SSH Key to Your GitHub Account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
* [GitHub: Testing Your SSH Connection](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)
* [GitHub: Reviewing Your SSH Keys](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/reviewing-your-ssh-keys)
* [OpenSSH: ssh Manual](https://man.openbsd.org/ssh)
* [OpenSSH: ssh-keygen Manual](https://man.openbsd.org/ssh-keygen)
