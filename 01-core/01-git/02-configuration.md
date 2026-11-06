# Git Configuration

Git is highly configurable. Developers can customize Git to match their operating system, development environment, preferred tools, security requirements, and team workflow.

Git configuration controls settings such as:

* Commit author and committer identity.
* Default branch names.
* Preferred text editors and pagers.
* Line-ending behavior.
* Command aliases.
* Credential helpers.
* SSH-related behavior.
* Commit and tag signing.
* Merge, diff, pull, and push behavior.
* Repository-specific settings.

Git stores configuration in plain-text files and also accepts settings from command-line options and environment variables. Understanding configuration scopes and precedence is essential when working with multiple projects, organizations, or development environments.

# 🗁 What Is Git Configuration?

Git configuration is a collection of key-value settings that control Git's behavior.

The primary command for managing configuration is:

```bash
git config
```

General syntax:

```bash
git config [<options>] <name> [<value>]
```

For example:

```bash
git config --global user.name "Muhammad Iqbal"
```

This sets the `user.name` value in the current operating-system user's global configuration.

Another example:

```bash
git config --global init.defaultBranch main
```

This sets `main` as the default initial branch name for newly initialized repositories, unless an initialization option explicitly overrides it.

### Install Git

On Debian or Ubuntu:

```bash
sudo apt update
sudo apt install git
```

On Fedora:

```bash
sudo dnf install git
```

On macOS with Homebrew:

```bash
brew install git
```

On Windows, install Git for Windows from the official Git website or use an available package manager.

Verify the installation:

```bash
git --version
```

### Check Your Git Identity

Check whether a name and email are configured:

```bash
git config --get user.name
git config --get user.email
```

If either value is missing or incorrect, configure it globally:

```bash
git config --global user.name "Muhammad Iqbal"
git config --global user.email "you@example.com"
```

Use an email address you intend to associate with your commits. If you use a hosting provider's privacy email, configure that address where appropriate.

### Configuration Key Structure

Git configuration keys generally consist of a section and a variable.

Example:

```ini
[user]
    name = Muhammad Iqbal
    email = developer@example.com
```

The corresponding keys are:

```text
user.name
user.email
```

Another example:

```ini
[core]
    editor = code --wait
    autocrlf = input
```

The corresponding keys are:

```text
core.editor
core.autocrlf
```

### Why Git Configuration Matters

Correct configuration helps provide:

1. Consistent commit identity.
2. Predictable Git behavior.
3. Consistent development environments.
4. Reliable collaboration.
5. Cross-platform consistency.
6. Improved productivity through aliases and tools.
7. Better authentication and signing workflows.

# 🗁 Git Configuration Scopes

Git supports several configuration scopes. Each scope has a different purpose and applies to a different context.

The principal scopes are:

1. System.
2. Global.
3. Local.
4. Worktree.
5. Command.

### System Configuration

System configuration applies to all users and repositories on a particular installation of Git.

A common location on Linux is:

```text
/etc/gitconfig
```

Set a system-level value:

```bash
sudo git config --system core.editor vim
```

List system configuration:

```bash
git config --system --list
```

Edit the system configuration file:

```bash
sudo git config --system --edit
```

Characteristics:

* Applies across users and repositories for that Git installation.
* Usually requires administrator privileges to modify.
* Can establish organization-wide defaults on managed machines.
* Can be overridden by higher-precedence configuration.

The exact system configuration path can vary by platform and Git installation.

### Global Configuration

Global configuration normally applies to all repositories used by the current operating-system user, unless a more specific setting overrides it.

Common locations on Linux:

```text
~/.gitconfig
~/.config/git/config
```

Set a global username and email:

```bash
git config --global user.name "Muhammad Iqbal"
git config --global user.email "you@example.com"
```

List global configuration:

```bash
git config --global --list
```

Edit global configuration:

```bash
git config --global --edit
```

Characteristics:

* Applies to the current user.
* Provides defaults for that user's repositories.
* Normally does not require administrator privileges.
* Can be overridden by local, worktree, or command-level settings.

Global configuration is the usual place for personal Git preferences.

### Local Configuration

Local configuration applies to one repository.

For a standard repository, its configuration is normally stored in:

```text
<repository>/.git/config
```

Set repository-specific identity from inside the repository:

```bash
git config --local user.name "Project Developer"
git config --local user.email "project@example.com"
```

List local configuration:

```bash
git config --local --list
```

Edit local configuration:

```bash
git config --local --edit
```

Characteristics:

* Applies to one repository.
* Overrides system and global values for the same single-valued key.
* Is useful for project-specific settings and work identities.
* Is stored in the repository's Git metadata.

Example:

Suppose your global identity is:

```text
Name: Muhammad Iqbal
Email: personal@example.com
```

A company repository requires:

```text
Name: Muhammad Iqbal
Email: muhammad@company.com
```

Inside the company repository, configure:

```bash
git config --local user.email "muhammad@company.com"
```

Commits in that repository will use the repository-specific email, while other repositories can continue using the global email.

### Worktree Configuration

Git worktrees allow multiple working directories to be associated with one repository.

A repository can enable worktree-specific configuration with:

```bash
git config extensions.worktreeConfig true
```

After enabling the extension, configure a setting for the current worktree:

```bash
git config --worktree user.email "worktree@example.com"
```

List worktree configuration:

```bash
git config --worktree --list
```

Worktree-specific configuration is stored in the configuration file associated with that worktree, commonly:

```text
<common-git-dir>/worktrees/<worktree-id>/config.worktree
```

The exact Git directory layout can vary, particularly for linked worktrees and repositories whose Git directory is not inside the working directory.

Characteristics:

* Applies to an individual working tree.
* Is useful when multiple working directories share repository history but need different settings.
* Requires the `extensions.worktreeConfig` extension to be enabled.
* Takes precedence over ordinary local configuration for applicable keys.

### Command-Level Configuration

Command-level configuration applies to one Git invocation.

Use `-c` before the subcommand:

```bash
git -c user.name="Temporary Developer" commit -m "Temporary commit"
```

This supplies a temporary value for that invocation without permanently changing a configuration file.

Another example:

```bash
git -c core.editor=vim config --edit
```

Characteristics:

* Temporary and invocation-specific.
* Does not permanently modify configuration files.
* Useful in scripts, automation, testing, and one-time overrides.
* Has high precedence among the standard configuration scopes.

### Configuration Scope Comparison

| Scope    | Applies to                                        | Typical location or source                                     |
| -------- | ------------------------------------------------- | -------------------------------------------------------------- |
| System   | All users and repositories for a Git installation | `/etc/gitconfig` on many Linux systems                         |
| Global   | Current operating-system user                     | `~/.gitconfig` or `~/.config/git/config`                       |
| Local    | One repository                                    | Repository configuration file                                  |
| Worktree | One working tree                                  | Worktree-specific `config.worktree`                            |
| Command  | One Git invocation                                | `git -c key=value` or command-scoped environment configuration |

# 🗁 Configuration Precedence

When the same configuration key is defined at multiple scopes, Git determines which value applies according to configuration precedence.

For the standard scopes, the general order from lower to higher precedence is:

```text
System
  ↓
Global
  ↓
Local
  ↓
Worktree
  ↓
Command
```

For ordinary single-valued settings, the higher-precedence value generally takes effect.

Other configuration sources can also matter. Conditional includes, environment variables, and command-line options can affect the configuration Git reads. Do not assume that every setting is represented by only one value: some keys are multi-valued and are accumulated or interpreted specially.

### Example of Precedence

Set a value at the system level:

```bash
sudo git config --system user.name "System User"
```

Set a different global value:

```bash
git config --global user.name "Global User"
```

Set a repository-specific value inside a repository:

```bash
git config --local user.name "Local User"
```

For this single-valued setting, the effective value in that repository is:

```text
Local User
```

The local value takes precedence over the global and system values.

### Inspect Configuration Origins and Scopes

To see values and the files from which they were read:

```bash
git config --list --show-origin
```

To include scope names:

```bash
git config --list --show-origin --show-scope
```

To inspect one key and its source:

```bash
git config --show-origin --show-scope --get user.email
```

These commands are especially useful when a setting does not behave as expected.

### Single-Valued and Multi-Valued Settings

Many settings are used as a single effective value. Other settings can have multiple entries.

For example, a remote can have one or more fetch refspecs:

```ini
[remote "origin"]
    fetch = +refs/heads/*:refs/remotes/origin/*
```

Retrieve every configured value for a key with:

```bash
git config --get-all remote.origin.fetch
```

Use `--add` to add another value where the setting supports multiple values:

```bash
git config --global --add some.multiValuedKey "another-value"
```

The behavior of repeated values depends on the configuration key and the command that consumes it.

# 🗁 Git Configuration Files

Git stores many settings in plain-text configuration files.

### System Configuration

A common Linux location is:

```text
/etc/gitconfig
```

This file contains system-wide settings.

### Global Configuration

Common locations include:

```text
~/.gitconfig
~/.config/git/config
```

The exact file Git uses can be affected by environment variables and installation details.

### Local Configuration

A standard repository commonly stores local configuration at:

```text
project/
└── .git/
    └── config
```

For linked worktrees, the Git directory may be a file reference rather than a directory inside the working tree. Use Git commands to inspect configuration rather than assuming every repository has the same physical layout.

### Worktree Configuration

When worktree configuration is enabled, a linked worktree can have its own configuration file, commonly located under the repository's common Git directory:

```text
<common-git-dir>/
└── worktrees/
    └── <worktree-id>/
        └── config.worktree
```

### Example `.gitconfig`

A global configuration file might look like this:

```ini
[user]
    name = Muhammad Iqbal
    email = developer@example.com

[init]
    defaultBranch = main

[core]
    editor = code --wait
    autocrlf = input

[pull]
    rebase = false

[push]
    default = simple

[fetch]
    prune = true

[alias]
    st = status
    co = switch
    br = branch
    ci = commit
    lg = log --oneline --graph --decorate --all
```

This file defines:

* User identity.
* Default initial branch.
* Preferred editor.
* Line-ending behavior.
* Pull integration preference.
* Push behavior.
* Fetch pruning.
* Command aliases.

# 🗁 Viewing Git Configuration

Git provides several commands for inspecting configuration.

### View Configuration Values

```bash
git config --list
```

This lists configuration entries Git reads for the current context. A key may appear more than once.

### View Global Configuration

```bash
git config --global --list
```

### View Local Configuration

Run inside a repository:

```bash
git config --local --list
```

### View System Configuration

```bash
git config --system --list
```

Reading system configuration may depend on platform permissions and installation details.

### View a Specific Value

```bash
git config user.name
git config user.email
git config init.defaultBranch
```

Without a scope option, Git reads the applicable configuration sources for the current context.

### View Configuration with Origins

```bash
git config --list --show-origin
```

### View Configuration with Scopes

```bash
git config --list --show-scope
```

### View Both Origin and Scope

```bash
git config --list --show-origin --show-scope
```

### Important Inspection Commands

| Command                                             | Purpose                                |
| --------------------------------------------------- | -------------------------------------- |
| `git config --list`                                 | List configuration entries read by Git |
| `git config --global --list`                        | List global configuration              |
| `git config --local --list`                         | List local repository configuration    |
| `git config --system --list`                        | List system configuration              |
| `git config --show-origin --list`                   | Show configuration origins             |
| `git config --show-scope --list`                    | Show configuration scopes              |
| `git config --show-origin --show-scope --get <key>` | Inspect one key and its source         |

# 🗁 Setting Git Identity

Git records author and committer identities in commit metadata.

The primary identity settings are:

* `user.name`
* `user.email`

### Configure Username

```bash
git config --global user.name "Muhammad Iqbal"
```

### Configure Email

```bash
git config --global user.email "developer@example.com"
```

### Verify Identity

```bash
git config --global user.name
git config --global user.email
```

To see the identity that applies in the current repository:

```bash
git config user.name
git config user.email
```

### Configure Repository-Specific Identity

Inside a repository:

```bash
git config --local user.name "Muhammad Iqbal"
git config --local user.email "work@example.com"
```

### Author vs. Committer

Git distinguishes between the author and committer.

**Author:** The person credited with originally writing the changes.

**Committer:** The person who created the commit object.

These are often the same person. However, operations such as rebasing, cherry-picking, and applying patches can result in different author and committer identities.

Inspect commit metadata with:

```bash
git log --format=fuller
```

### Identity Is Not Authentication

`user.name` and `user.email` configure commit metadata.

They do not authenticate you to GitHub, GitLab, or another Git hosting service.

Remote authentication is handled separately through mechanisms such as:

* SSH keys.
* Personal access tokens.
* Credential managers.
* Other supported authentication methods.

# 🗁 Configuring the Default Branch

A branch is a reference to a line of development. Many modern projects use `main` as their primary branch name.

Set the default initial branch:

```bash
git config --global init.defaultBranch main
```

Verify:

```bash
git config --global init.defaultBranch
```

Initialize a repository:

```bash
mkdir my-project
cd my-project
git init
```

The initial branch will be named `main`, provided no explicit initialization option overrides the configured default.

### Initialize with an Explicit Branch Name

```bash
git init -b main
```

The `-b` option explicitly specifies the initial branch name for that repository.

### Why Configure the Default Branch?

* Keeps branch naming consistent across projects.
* Aligns with the naming used by many hosting platforms.
* Makes repository initialization predictable.
* Simplifies collaboration and automation.

Changing `init.defaultBranch` affects future repository initialization. It does not rename branches in existing repositories.

# 🗁 Configuring the Default Editor

Git sometimes opens a text editor for:

* Commit messages.
* Merge messages.
* Interactive rebase instructions.
* Configuration files.

Configure the editor with `core.editor`.

### Visual Studio Code

```bash
git config --global core.editor "code --wait"
```

The `--wait` option makes the command wait for the VS Code editing session to close before Git continues.

### Vim

```bash
git config --global core.editor "vim"
```

### Nano

```bash
git config --global core.editor "nano"
```

### Notepad on Windows

```powershell
git config --global core.editor "notepad"
```

### Verify the Editor

```bash
git config --global core.editor
```

### Common Editors

| Editor | Configuration |
| --- | --- |
| VS Code | `code --wait` |
| Vim | `vim` |
| Nano | `nano` |
| Emacs | `emacs` |
| Windows Notepad | `notepad` |

The editor executable must be available in the environment where Git runs.

# 🗁 Configuring Line Endings

Operating systems commonly use different line-ending conventions.

| Platform | Common line ending | Representation |
| --- | --- | --- |
| Windows | CRLF | `\r\n` |
| Linux | LF | `\n` |
| macOS | LF | `\n` |

Git provides `core.autocrlf` to control certain automatic line-ending conversions between the working tree and the index.

### `core.autocrlf = true`

```bash
git config --global core.autocrlf true
```

Typically:

* Converts CRLF to LF when adding text files to the index.
* Converts LF to CRLF when checking text files out on supported platforms.

This is commonly used in Windows development environments.

### `core.autocrlf = input`

```bash
git config --global core.autocrlf input
```

Typically:

* Converts CRLF to LF when adding text files to the index.
* Does not convert LF to CRLF during checkout.

This is commonly used on Linux and macOS.

### `core.autocrlf = false`

```bash
git config --global core.autocrlf false
```

Disables automatic conversion through this setting. It does not prevent `.gitattributes` rules or other tools from managing line endings.

### Repository-Level `.gitattributes`

For teams, a repository-level `.gitattributes` file provides explicit and shareable line-ending rules.

Example:

```gitattributes
# Normalize text files and use LF in the working tree
* text=auto eol=lf

# Explicit text file types
*.sh text eol=lf
*.py text eol=lf
*.js text eol=lf
*.ts text eol=lf
*.json text eol=lf
*.md text eol=lf

# Windows command scripts
*.bat text eol=crlf
*.cmd text eol=crlf
```

The exact rules should match the project's file types and platform requirements. `.gitattributes` is generally more predictable for team-wide behavior than relying only on each developer's global setting.

### Inspect Line-Ending Behavior

```bash
git ls-files --eol
```

This reports line-ending information for tracked files.

# 🗁 Configuring File Permissions

Git primarily tracks the executable bit for regular files on filesystems where that permission is available.

The relevant setting is:

```text
core.filemode
```

### Enable File-Mode Tracking

```bash
git config --local core.filemode true
```

Git can detect executable-bit changes in the working tree.

### Disable File-Mode Tracking

```bash
git config --local core.filemode false
```

Git ignores executable-bit differences detected in the working tree. This can be useful on filesystems that do not preserve executable permissions reliably.

### Inspect Tracked File Modes

```bash
git ls-files --stage
```

Common Git modes include:

| Mode | Meaning |
| --- | --- |
| `100644` | Regular non-executable file |
| `100755` | Regular executable file |
| `120000` | Symbolic link |
| `160000` | Git submodule entry |

Git represents symbolic links and submodule entries using special modes.

# 🗁 Configuring Aliases

Aliases create shortcuts for frequently used Git subcommands. They reduce repetitive typing and can make common workflows faster.

### Create a Basic Alias

```bash
git config --global alias.st status
```

Run:

```bash
git st
```

This is equivalent to:

```bash
git status
```

### Useful Git Aliases

```bash
# Status
git config --global alias.st status

# Branch listing
git config --global alias.br branch

# Switch branches
git config --global alias.sw switch

# Commit
git config --global alias.ci commit

# Compact graphical log
git config --global alias.lg "log --oneline --graph --decorate --all"

# Show the latest commit
git config --global alias.last "log -1 HEAD"

# Show unstaged changes
git config --global alias.df diff

# Show staged changes
git config --global alias.dfc "diff --cached"
```

Usage:

```bash
git st
git br
git sw main
git lg
git last
```

### Aliases with Shell Commands

A Git alias can execute a shell command when its value begins with `!`.

Example:

```bash
git config --global alias.root '!git rev-parse --show-toplevel'
```

Usage:

```bash
git root
```

This prints the root directory of the current repository.

### Configure Aliases in `.gitconfig`

```ini
[alias]
    st = status
    br = branch
    sw = switch
    ci = commit
    lg = log --oneline --graph --decorate --all
    last = log -1 HEAD
    df = diff
    dfc = diff --cached
    root = !git rev-parse --show-toplevel
```

Git aliases are invoked as subcommands. They do not replace existing Git commands, and aliases generally cannot override built-in commands.

# 🗁 Configuring Credentials

Git needs authentication when communicating with protected remote repositories.

For example:

```bash
git clone https://github.com/username/project.git
```

Depending on the server and authentication method, Git may need credentials.

Git supports credential helpers that assist with retrieving and storing authentication information.

### What Is a Credential Helper?

A credential helper is a program or mechanism that helps Git obtain credentials when communicating with a remote service.

Common approaches include:

* Git Credential Manager.
* Operating-system credential stores.
* Secure keychain integrations.
* Temporary in-memory caching.
* Custom credential helper programs.

### Inspect Credential Helpers

```bash
git config --show-origin --get-all credential.helper
```

The `--get-all` option is useful because multiple helpers may be configured.

### Configure Git Credential Manager

If Git Credential Manager is installed and its helper name is `manager`, configure it with:

```bash
git config --global credential.helper manager
```

The helper name can vary by installation and version. Use the installation's documentation and verify the available command:

```bash
git credential-manager --version
```

### Configure Temporary Credential Cache

```bash
git config --global credential.helper 'cache --timeout=3600'
```

This uses Git's cache helper to keep credentials in memory for up to 3,600 seconds, subject to helper behavior.

### Remove Credential Helper Configuration

```bash
git config --global --unset-all credential.helper
```

This removes all global values for that key. It does not necessarily erase credentials already stored by an external credential manager.

### Security Considerations

* Avoid placing access tokens or passwords directly in `.gitconfig`.
* Avoid using plain-text credential storage on shared or untrusted systems.
* Prefer an appropriate operating-system credential manager.
* Protect credential-manager access with the security controls provided by your operating system.
* Revoke credentials that are no longer needed.

# 🗁 SSH Configuration

SSH is a common authentication method for connecting to remote Git repositories.

It is widely used with GitHub, GitLab, and private Git servers.

**Check If You Already Have an SSH Key:**

First, check whether you already have an SSH key:

```bash
ls -al ~/.ssh
```

Look for these files:

* `id_ed25519` and `id_ed25519.pub` — Ed25519 key pair.
* `id_rsa` and `id_rsa.pub` — RSA key pair.

If you already have a suitable key pair, you can use it instead of generating a new one.

If not, follow the steps below.

### Step 1: Generate a New SSH Key

GitHub documentation: [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

1. Open Git Bash or your terminal.
2. Run the following command, replacing the example email with your GitHub email address:

    ```bash
    ssh-keygen -t ed25519 -C "your-email@example.com"
    ```

3. When prompted to choose where to save the key, press **Enter** to accept the default location:

    ```bash
    Enter file in which to save the key (/home/youruser/.ssh/id_ed25519):
    ```

4. When prompted, enter a passphrase to protect your private key. You can also press Enter to leave it empty.

    ```bash
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    ```

    If you set a passphrase, you may need to enter it when using the key unless it is managed by an SSH agent.

Your SSH key pair is now generated:

* **Private key:** `~/.ssh/id_ed25519`
* **Public key:** `~/.ssh/id_ed25519.pub`

**Important:** Never share your private key. Only the public key (`id_ed25519.pub`) should be added to GitHub.

To display your public key in the terminal, run:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the complete output. You will need it in Step 3.

### Step 2: Start the SSH Agent and Add Your Key

The SSH agent manages your private key and can remember its passphrase during your session.

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add your private key to the agent:

```bash
ssh-add ~/.ssh/id_ed25519
```

If you configured a passphrase, enter it when prompted.

### Step 3: Add Your SSH Public Key to GitHub

GitHub documentation: [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

1. Copy your SSH public key using the command from Step 1.
2. Open GitHub's SSH key settings: [https://github.com/settings/keys](https://github.com/settings/keys).
3. Click **New SSH key**.
4. Enter a descriptive title, such as `My Development Machine`.
5. Select **Authentication Key** as the key type.
6. Paste your public key into the **Key** field.
7. Click **Add SSH key**.

Your public key is now registered with your GitHub account.

### Step 4: Test the SSH Connection

Run the following command:

```bash
ssh -T git@github.com
```

On your first connection, SSH may ask you to verify GitHub's host fingerprint. Verify it against GitHub's published fingerprints before accepting it.

If authentication is successful, you should see a message similar to:

```text
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

**SSH is now configured and ready to use with GitHub.**

### SSH vs. HTTPS

Git supports two common methods for connecting to remote repositories: SSH and HTTPS.

|Feature|SSH|HTTPS|
|---|---|---|
|Authentication|SSH key pair|Token or credential manager, depending on provider|
|Remote URL|`git@github.com:user/repo.git`|`https://github.com/user/repo.git`|
|Credential management|SSH keys and agent|Credential helper or manager|
|Common use|Developer machines and servers|General workflows and managed environments|

SSH configuration and hosting-provider-specific authentication are covered further in the GitHub section.

# 🗁 Commit Signing Configuration

Git supports signing commits and tags. A signature allows recipients to verify that an object was signed using a particular cryptographic identity.

Git supports several signing formats, including:

* OpenPGP (GPG).
* SSH.
* X.509/S/MIME.

Signing is distinct from authenticating to a remote repository.

### Configure GPG Signing

After creating and configuring a GPG key, set the signing key:

```bash
git config --global user.signingkey <GPG-KEY-ID>
```

Enable commit signing by default:

```bash
git config --global commit.gpgsign true
```

### Configure SSH Signing

Set SSH as the signing format:

```bash
git config --global gpg.format ssh
```

Configure the public signing key:

```bash
git config --global user.signingkey ~/.ssh/id_ed25519.pub
```

Enable signing by default:

```bash
git config --global commit.gpgsign true
```

For SSH signing, Git uses the corresponding private key to create the signature. The public key is used for verification.

### Sign an Individual Commit

```bash
git commit -S -m "Add signed commit"
```

The `-S` option requests a signed commit.

### Verify a Commit Signature

```bash
git verify-commit HEAD
```

Verify a signed tag:

```bash
git verify-tag v1.0.0
```

### Important Distinction

* **Authentication** establishes whether you can access a remote service.
* **Commit signing** provides a cryptographic signature that can be verified.
* **Hosting-provider verification** may require registering the relevant public signing key with that provider.

An SSH key used for repository authentication does not automatically mean that commits are signed.

# 🗁 Conditional Configuration

Git supports conditional includes. They allow Git to load additional configuration files when specified conditions match.

This is useful when maintaining separate personal, company, or client identities.

### Directory-Based Configuration with `includeIf`

Add conditional includes to your global configuration file:

```ini
[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work

[includeIf "gitdir:~/personal/"]
    path = ~/.gitconfig-personal
```

The `gitdir` condition matches repositories based on their Git directory location. Pay attention to path matching and trailing slashes when designing directory rules.

### Work Configuration

Create:

```text
~/.gitconfig-work
```

Content:

```ini
[user]
    name = Muhammad Iqbal
    email = muhammad@company.com
```

### Personal Configuration

Create:

```text
~/.gitconfig-personal
```

Content:

```ini
[user]
    name = Muhammad Iqbal
    email = personal@example.com
```

Repositories whose Git directories match the corresponding conditions can use the appropriate identity.

### Branch-Based Conditional Configuration

Git also supports conditions based on the currently checked-out branch.

Example:

```ini
[includeIf "onbranch:release/*"]
    path = ~/.gitconfig-release
```

This condition matches when the current branch matches the specified pattern.

Branch-based conditions are context-dependent: they require a repository with a current branch that can be evaluated. They are not a substitute for directory-based identity separation in every workflow.

### Why Use Conditional Configuration?

* Separate personal and company identities.
* Apply project-specific defaults automatically.
* Avoid repeatedly changing global settings.
* Keep configuration organized across multiple development environments.

# 🗁 Git Configuration Syntax

Git configuration files use an INI-like syntax.

### Basic Structure

```ini
[section]
    variable = value
```

Example:

```ini
[user]
    name = Muhammad Iqbal
    email = developer@example.com
```

### Subsections

Git supports subsections.

Example:

```ini
[remote "origin"]
    url = https://github.com/username/project.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```

Here, `origin` is a subsection of the `remote` section.

### Boolean Values

Git supports boolean configuration values.

Example:

```ini
[core]
    autocrlf = true
    filemode = false
```

Common boolean representations include:

```text
true
false
yes
no
on
off
1
0
```

Git also accepts certain boolean values without an explicit value in configuration-file syntax. Consult the documentation for the particular key.

### Comments

Configuration files support comments beginning with `#` or `;`.

Example:

```ini
# Git user configuration

[user]
    name = Muhammad Iqbal
    email = developer@example.com
```

### Multiple Values

Some keys can have multiple values.

For example:

```ini
[remote "origin"]
    fetch = +refs/heads/*:refs/remotes/origin/*
```

Retrieve all values for a key:

```bash
git config --get-all remote.origin.fetch
```

Add another value where appropriate:

```bash
git config --global --add <key> <value>
```

Not every configuration key is designed to have multiple values.

# 🖆 Important Git Configuration Options

The following settings are useful in everyday development.

| Configuration | Purpose |
| --- | --- |
| `user.name` | Commit author and committer name |
| `user.email` | Commit author and committer email |
| `init.defaultBranch` | Default initial branch name |
| `core.editor` | Default text editor |
| `core.pager` | Pager for Git output |
| `core.autocrlf` | Automatic line-ending conversion |
| `core.filemode` | Executable-bit tracking behavior |
| `core.ignorecase` | Filesystem case-sensitivity behavior |
| `pull.rebase` | Default pull integration preference |
| `pull.ff` | Fast-forward policy for pull integration |
| `push.default` | Default push behavior |
| `fetch.prune` | Remove stale remote-tracking references during fetch |
| `commit.gpgsign` | Sign commits by default |
| `gpg.format` | Signing format |
| `credential.helper` | Credential management |
| `rerere.enabled` | Reuse recorded conflict resolutions |
| `diff.algorithm` | Diff algorithm |
| `merge.tool` | Preferred merge tool |
| `color.ui` | Colorized command output |

### Configure Pull Behavior

Merge-based pull:

```bash
git config --global pull.rebase false
```

Rebase-based pull:

```bash
git config --global pull.rebase true
```

Fast-forward-only pull:

```bash
git config --global pull.ff only
```

These settings represent different integration policies. Choose the policy that matches your team's workflow.

* `pull.rebase=false` uses merge-based integration.
* `pull.rebase=true` rebases local commits on top of the fetched upstream history.
* `pull.ff=only` allows pull to update only when a fast-forward is possible; it refuses a divergent update rather than creating a merge commit.

### Configure Push Behavior

```bash
git config --global push.default simple
```

The `simple` setting is a common and predictable default. It generally pushes the current branch to its upstream branch when the names match.

### Prune Stale Remote-Tracking References

```bash
git config --global fetch.prune true
```

This removes remote-tracking references for branches that no longer exist on the remote during fetch operations.

### Configure Conflict Resolution Reuse

```bash
git config --global rerere.enabled true
```

`rerere` means **reuse recorded resolution**.

Git can record how you resolve a conflict and reuse that resolution when it encounters a sufficiently similar conflict again.

### Configure the Diff Algorithm

```bash
git config --global diff.algorithm histogram
```

Git supports several diff algorithms, including:

* Myers.
* Minimal.
* Patience.
* Histogram.

Different algorithms can produce different diff presentations. The best choice depends on the content and review workflow.

# 🖆 Managing Configuration

Git provides commands for adding, changing, reading, and deleting configuration values.

#### Set a Configuration Value

```bash
git config --global user.name "Muhammad Iqbal"
```

#### Read a Configuration Value

```bash
git config --global user.name
```

#### Remove a Configuration Value

```bash
git config --global --unset user.name
```

#### Remove All Values for a Key

```bash
git config --global --unset-all credential.helper
```

#### Replace All Values

```bash
git config --global --replace-all user.email "new@example.com"
```

#### Add Another Value

```bash
git config --global --add remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*"
```

Use `--add` only when the key's semantics support multiple values. For single-valued settings such as `user.email`, a single effective value is generally preferable.

#### Edit a Configuration File

```bash
git config --global --edit
```

#### Inspect Configuration Syntax

Git parses configuration files when reading them. A useful check is:

```bash
git config --list
```

If a configuration file contains invalid syntax, Git may report a parsing error.

#### Environment-Based Configuration

Git supports environment variables that influence configuration.

For example:

```bash
GIT_CONFIG_NOSYSTEM=1 git config --list
```

This prevents Git from reading the system configuration for that command.

Other configuration-related environment variables include:

```text
GIT_CONFIG_GLOBAL
GIT_CONFIG_SYSTEM
GIT_CONFIG_COUNT
GIT_CONFIG_KEY_0
GIT_CONFIG_VALUE_0
```

The `GIT_CONFIG_COUNT`, `GIT_CONFIG_KEY_<n>`, and `GIT_CONFIG_VALUE_<n>` variables can provide command-scoped configuration entries in supported Git versions and contexts.

Environment variables are useful in automation and controlled execution environments. Their behavior and precedence should be checked against the Git version and the specific variable's documentation.

# 🖆 Git Configuration Best Practices

A well-configured Git environment reduces mistakes and improves productivity.

#### Configure Identity Globally

```bash
git config --global user.name "Muhammad Iqbal"
git config --global user.email "developer@example.com"
```

#### Use Repository-Specific Identity When Necessary

For a company repository:

```bash
git config --local user.email "muhammad@company.com"
```

#### Use `main` as the Default Initial Branch

```bash
git config --global init.defaultBranch main
```

#### Choose an Appropriate Editor

```bash
git config --global core.editor "code --wait"
```

#### Establish Consistent Line Endings

Use a repository-level `.gitattributes` file to establish shared file-handling rules.

#### Use Meaningful Aliases

Create shortcuts for frequently used commands without sacrificing clarity.

#### Use Secure Credential Management

Prefer an operating-system credential manager or another secure credential helper. Do not store access tokens in plain-text configuration files.

#### Use Conditional Configuration for Multiple Identities

Use `includeIf` to separate personal, company, and client identities.

#### Inspect Configuration Sources

When a setting behaves unexpectedly, run:

```bash
git config --list --show-origin --show-scope
```

#### Avoid Unnecessary Global Overrides

Keep repository-specific settings local whenever possible. This prevents one project's requirements from unexpectedly affecting unrelated projects.

#### Keep Shared Repository Rules in the Repository

Use repository-level files such as `.gitattributes` when a rule should apply consistently to everyone working on the project.

# 🖆 Common Git Configuration Problems

#### Problem 1: Wrong Commit Author

**Symptom:** Commits show an unexpected name or email address.

Check:

```bash
git config user.name
git config user.email
```

Inspect the source:

```bash
git config --show-origin --show-scope --get user.email
```

Fix globally:

```bash
git config --global user.email "correct@example.com"
```

Or fix only the current repository:

```bash
git config --local user.email "correct@example.com"
```

Changing configuration affects future commits. It does not automatically rewrite identity metadata in existing commits.

#### Problem 2: Wrong Default Branch

**Symptom:** New repositories initialize with an unexpected branch name.

Fix:

```bash
git config --global init.defaultBranch main
```

This affects future repository initialization. It does not rename existing branches.

#### Problem 3: Editor Does Not Open

**Symptom:** Git cannot find the configured editor.

Check:

```bash
git config core.editor
```

On Linux or macOS, check whether an executable is available:

```bash
command -v code
```

On Windows PowerShell:

```powershell
Get-Command code
```

Configure an editor that is installed and available in the environment:

```bash
git config --global core.editor "vim"
```

For VS Code, use `code --wait` so Git waits for the editing session to finish.

#### Problem 4: Unexpected Line-Ending Changes

**Symptom:** Git reports changes across many lines even though the visible content appears identical.

Check:

```bash
git config --show-origin --get core.autocrlf
```

Inspect tracked files:

```bash
git ls-files --eol
```

Review the repository's `.gitattributes` rules and coordinate line-ending settings with the team.

#### Problem 5: Authentication Failure

**Symptom:** Git cannot authenticate when pushing or pulling.

Check the remote:

```bash
git remote -v
```

Inspect credential helpers:

```bash
git config --show-origin --get-all credential.helper
```

For SSH:

```bash
ssh -T git@github.com
```

For HTTPS, verify that the credential manager is configured and that the account has the required repository permissions.

#### Problem 6: Unexpected Configuration Override

**Symptom:** You change a global setting, but Git continues using another value.

Inspect configuration sources:

```bash
git config --list --show-origin --show-scope
```

Then inspect the specific key:

```bash
git config --show-origin --show-scope --get user.email
```

A local, worktree, command-level, conditional, or environment-provided value may be affecting the result.

#### Problem 7: Git Reports an Invalid Configuration File

**Symptom:** Git reports a configuration parsing error.

Inspect the configuration file that Git identifies in the error message. Common causes include:

* Missing or malformed section headers.
* Invalid quoting.
* Incorrect subsection syntax.
* Accidental characters or malformed values.

Use:

```bash
git config --list --show-origin
```

After correcting the file, run the command again to confirm that Git can read the configuration.

# 🖆 Hands-on Practical Exercises

These exercises build practical experience with Git configuration. Perform them in a test environment or a practice repository.

#### Exercise 1: Configure Git Identity

**Objective:** Configure your global Git identity.

```bash
git config --global user.name "Muhammad Iqbal"
git config --global user.email "developer@example.com"
git config --global --list
```

Verify:

```bash
git config user.name
git config user.email
```

Expected learning:

* Global configuration.
* User identity.
* Configuration inspection.

#### Exercise 2: Configure the Default Branch

**Objective:** Set `main` as the default initial branch.

```bash
git config --global init.defaultBranch main
```

Create a repository:

```bash
mkdir config-practice
cd config-practice

git init
git branch --show-current
```

Expected output:

```text
main
```

Expected learning:

* `init.defaultBranch`.
* Repository initialization.
* Initial branch naming.

#### Exercise 3: Configure a Repository-Specific Identity

**Objective:** Override the global identity for one repository.

Inside the practice repository:

```bash
git config --local user.name "Project Developer"
git config --local user.email "project@example.com"
```

Inspect:

```bash
git config --list --show-origin --show-scope
```

Expected learning:

* Local configuration.
* Configuration precedence.
* Multiple development identities.

#### Exercise 4: Create Git Aliases

**Objective:** Improve command-line productivity.

```bash
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.last "log -1 HEAD"
```

Test:

```bash
git st
git lg
git last
```

Expected learning:

* Git aliases.
* Custom subcommands.
* Configuration files.

#### Exercise 5: Configure the Default Editor

**Objective:** Set your preferred editor.

For VS Code:

```bash
git config --global core.editor "code --wait"
```

Verify:

```bash
git config --global core.editor
```

Expected learning:

* Editor configuration.
* External editor integration.
* Git's editor invocation behavior.

#### Exercise 6: Explore Configuration Precedence

**Objective:** Understand how global and local settings interact.

Set different usernames:

```bash
git config --global user.name "Global User"
git config --local user.name "Local User"
```

Inspect:

```bash
git config --list --show-origin --show-scope
```

Check the effective value:

```bash
git config user.name
```

Expected value:

```text
Local User
```

Remove the local setting:

```bash
git config --local --unset user.name
```

Check again:

```bash
git config user.name
```

Expected value:

```text
Global User
```

Expected learning:

* Configuration precedence.
* Configuration origins.
* Removing an override.

#### Exercise 7: Create a Practical Global Configuration

**Objective:** Configure common Git defaults.

```bash
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --global pull.rebase false
git config --global push.default simple
git config --global fetch.prune true
git config --global rerere.enabled true
```

Inspect:

```bash
git config --global --list
```

Expected learning:

* Multiple Git settings.
* Global configuration management.
* Practical environment setup.

#### Exercise 8: Configure Conditional Identity

**Objective:** Use different identities for personal and work repositories.

Create directories:

```bash
mkdir -p ~/personal
mkdir -p ~/work
```

Add to `~/.gitconfig`:

```ini
[includeIf "gitdir:~/personal/"]
    path = ~/.gitconfig-personal

[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work
```

Create `~/.gitconfig-personal`:

```ini
[user]
    name = Muhammad Iqbal
    email = personal@example.com
```

Create `~/.gitconfig-work`:

```ini
[user]
    name = Muhammad Iqbal
    email = work@example.com
```

Create a repository inside each directory:

```bash
cd ~/personal
mkdir personal-project
cd personal-project
git init
git config user.email
```

Then test the work directory:

```bash
cd ~/work
mkdir work-project
cd work-project
git init
git config user.email
```

Expected learning:

* Conditional configuration.
* Identity separation.
* Multiple development environments.

#### Exercise 9: Inspect Configuration Origins

**Objective:** Find where a setting is defined.

Run:

```bash
git config --list --show-origin --show-scope
```

Find the source of the current email:

```bash
git config --show-origin --show-scope --get user.email
```

Expected learning:

* Configuration debugging.
* Scope inspection.
* Configuration file locations.

#### Exercise 10: Configure and Inspect Line Endings

**Objective:** Understand line-ending behavior.

Inspect tracked files:

```bash
git ls-files --eol
```

Create a `.gitattributes` file in a practice repository:

```gitattributes
* text=auto eol=lf
*.bat text eol=crlf
*.cmd text eol=crlf
```

Inspect the result:

```bash
git status
git ls-files --eol
```

Expected learning:

* LF and CRLF.
* `core.autocrlf`.
* Repository-level attributes.

# 🖆 Interview Questions

### Beginner Level

**1. What is Git configuration?**

Git configuration controls Git's behavior through settings stored in configuration files and other supported configuration sources.

**2. What is the purpose of `git config`?**

It is used to read, write, and manage Git configuration settings.

**3. What are the main Git configuration scopes?**

System, global, local, worktree, and command.

**4. What is the difference between global and local configuration?**

Global configuration normally applies to repositories used by the current operating-system user. Local configuration applies to one repository and takes precedence for applicable single-valued settings.

**5. Where is global Git configuration stored?**

Common Linux locations are `~/.gitconfig` and `~/.config/git/config`.

**6. How do you configure your Git username?**

```bash
git config --global user.name "Your Name"
```

**7. How do you configure your Git email?**

```bash
git config --global user.email "you@example.com"
```

**8. How do you view Git configuration entries?**

```bash
git config --list
```

### Intermediate Level

**9. What is Git configuration precedence?**

It determines how Git resolves settings when the same key is defined by multiple configuration sources.

**10. How do you identify where a Git setting comes from?**

```bash
git config --list --show-origin --show-scope
```

**11. How do you configure the default initial branch?**

```bash
git config --global init.defaultBranch main
```

**12. What is `core.editor`?**

It specifies the editor Git uses for operations that require text input.

**13. What is `core.autocrlf`?**

It controls automatic line-ending conversion between the working tree and Git's index for applicable text files.

**14. What is a Git alias?**

An alias is a custom shortcut for a Git subcommand.

**15. What is the difference between `user.name` and `user.email`?**

Both are identity fields recorded in commit metadata. The name identifies the contributor, while the email provides the configured email identity.

**16. Does `user.email` authenticate a user to GitHub?**

No. It is commit metadata, not an authentication credential.

### Advanced Level

**17. What is conditional Git configuration?**

It allows Git to include additional configuration files when conditions such as repository location or current branch match.

**18. What is worktree-specific configuration?**

It is configuration associated with an individual working tree in a repository where the worktree configuration extension is enabled.

**19. What is the difference between `--unset` and `--unset-all`?**

`--unset` removes a matching value, while `--unset-all` removes all values associated with the specified key in the selected configuration scope.

**20. What is Git Credential Manager?**

It is a credential management application that helps Git authenticate with supported hosting services and can integrate with secure credential storage.

**21. What is the difference between commit signing and authentication?**

Authentication establishes access to a remote service. Commit signing provides a cryptographic signature that can be verified independently.

**22. What does `fetch.prune` do?**

It removes stale remote-tracking references for branches that no longer exist on the remote during fetch operations.

**23. What is `rerere`?**

It is Git's mechanism for recording and reusing previous conflict resolutions.

**24. What is the purpose of `git config --show-origin`?**

It displays the source file or origin from which a configuration entry was read.

**25. Can Git use different identities for different repositories?**

Yes. Local configuration and conditional includes can define different identities for different repositories or repository locations.
