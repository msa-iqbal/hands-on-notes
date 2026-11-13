# Essential Linux Commands

Linux commands are instructions executed through a shell to perform operations on the operating system.

Ubuntu provides many command-line utilities for managing files, directories, users, permissions, software packages, processes, networking, and system resources.

Understanding these commands is essential for working efficiently in Linux environments.

This chapter introduces commonly used Linux commands, their syntax, options, and practical examples.

# 🗁 Linux Command Syntax

Most Linux commands follow this general structure:

```bash
command [options] [arguments]
```

Example:

```bash
ls -lah /var/log
```

| Component | Description |
| --- | --- |
| `ls` | Command |
| `-l` | Detailed listing option |
| `-a` | Include hidden entries |
| `-h` | Human-readable file sizes |
| `/var/log` | Target directory |

### 🖆 Short Options

Short options generally begin with a single hyphen.

```bash
ls -l
```

Multiple short options can often be combined:

```bash
ls -lah
```

### 🖆 Long Options

Long options generally begin with two hyphens.

```bash
ls --all
```

Some commands support both short and long forms:

```bash
ls -a
ls --all
```

### 🖆 Command Arguments

Arguments specify the input, target, or resource on which a command operates.

```bash
mkdir projects
```

Here, `projects` is the argument passed to `mkdir`.

### 🖆 Multiple Arguments

```bash
mkdir frontend backend database
```

Creates three directories in the current working directory.

### 🖆 Quoting Arguments

Use quotes when an argument contains spaces or shell-special characters.

```bash
mkdir "my project"
```

Single quotes preserve the literal characters inside the quoted string:

```bash
echo '$HOME'
```

Double quotes allow variable expansion:

```bash
echo "$HOME"
```

# 🗁 Command Information and Discovery

Linux provides several utilities for identifying commands and accessing their documentation.

### 🖆 `man` — Manual Pages

Displays detailed documentation for commands.

```bash
man ls
```

Common navigation keys:

| Key | Description |
| --- | --- |
| `Space` | Next page |
| `b` | Previous page |
| `/word` | Search for a word |
| `n` | Next search result |
| `q` | Quit |

### 🖆 `--help` — Command Help

Displays a command's help information.

```bash
ls --help
```

### 🖆 `help` — Bash Built-in Documentation

Displays help for Bash built-in commands.

```bash
help cd
```

### 🖆 `type` — Identify Command Type

Determines how Bash interprets a command.

```bash
type ls
type cd
```

Example output:

```text
ls is aliased to `ls --color=auto'
cd is a shell builtin
```

### 🖆 `command -v` — Locate a Command

Displays how Bash resolves a command.

```bash
command -v ls
```

Example output:

```text
/usr/bin/ls
```

### 🖆 `whereis` — Locate Binary and Documentation Files

```bash
whereis ls
```

Can display locations of the binary, source files, and manual pages when available.

### 🖆 `apropos` — Search Manual Descriptions

Searches manual page descriptions using keywords.

```bash
apropos network
```

Useful when you know what you want to accomplish but do not know the command name.

# 🗁 System Information Commands

These commands display information about the Ubuntu system and its resources.

### 🖆 `uname` — Kernel Information

```bash
uname
```

Display detailed information:

```bash
uname -a
```

Display only the kernel release:

```bash
uname -r
```

### 🖆 `hostname` — System Hostname

```bash
hostname
```

Displays the system's hostname.

### 🖆 `hostnamectl` — Hostname and Operating System Information

```bash
hostnamectl
```

Displays system identity information, including hostname and operating system details.

### 🖆 `date` — Display Date and Time

```bash
date
```

Display the date in a custom format:

```bash
date "+%Y-%m-%d %H:%M:%S"
```

### 🖆 `uptime` — System Running Time

```bash
uptime
```

Displays how long the system has been running, the number of logged-in users, and load averages.

### 🖆 `free` — Memory Usage

```bash
free -h
```

Displays RAM and swap usage in human-readable units.

### 🖆 `lscpu` — CPU Information

```bash
lscpu
```

Displays CPU architecture and processor information.

### 🖆 `lsmem` — Memory Information

```bash
lsmem
```

Displays information about available memory blocks.

### 🖆 `lsblk` — Block Device Information

```bash
lsblk
```

Displays disks, partitions, and other block devices.

### 🖆 `lspci` — PCI Device Information

```bash
lspci
```

Lists PCI devices such as network adapters and graphics cards.

### 🖆 `lsusb` — USB Device Information

```bash
lsusb
```

Lists connected USB devices.

### 🖆 `cat /etc/os-release` — Ubuntu Version Information

```bash
cat /etc/os-release
```

Displays Linux distribution identification information.

# 🗁 User and Group Commands

Linux is a multi-user operating system. These commands help identify users and inspect account information.

### 🖆 `whoami` — Current Username

```bash
whoami
```

### 🖆 `id` — User and Group IDs

```bash
id
```

Displays the current user's UID, primary GID, and supplementary groups.

### 🖆 `who` — Logged-in Users

```bash
who
```

Displays information about users currently logged in.

### 🖆 `w` — User Activity

```bash
w
```

Displays logged-in users and their current activities.

### 🖆 `groups` — User Group Membership

```bash
groups
```

Displays the groups associated with the current user.

Check another user's groups:

```bash
groups username
```

### 🖆 `passwd` — Change Password

```bash
passwd
```

Changes the current user's password.

An authorized administrator can change another user's password:

```bash
sudo passwd username
```

### 🖆 `su` — Switch User

```bash
su username
```

Starts a shell as another user after successful authentication.

To start a login shell:

```bash
su - username
```

### 🖆 `sudo` — Execute with Elevated Privileges

```bash
sudo apt update
```

Runs the specified command with elevated privileges according to the user's sudo permissions.

# 🗁 Directory and File Commands

These commands are used to create, inspect, copy, move, and remove filesystem entries.

### 🖆 `pwd` — Current Working Directory

```bash
pwd
```

### 🖆 `ls` — List Directory Contents

```bash
ls
```

Useful options:

```bash
ls -l
ls -a
ls -lh
ls -lah
ls -lt
```

### 🖆 `cd` — Change Directory

```bash
cd /var/log
```

Navigate to the home directory:

```bash
cd ~
```

Navigate to the parent directory:

```bash
cd ..
```

Return to the previous directory:

```bash
cd -
```

### 🖆 `mkdir` — Create Directories

```bash
mkdir projects
```

Create nested directories:

```bash
mkdir -p projects/frontend/src
```

### 🖆 `rmdir` — Remove Empty Directories

```bash
rmdir empty-directory
```

It removes only empty directories.

### 🖆 `touch` — Create Files or Update Timestamps

```bash
touch notes.txt
```

Creates an empty file if it does not exist.

If the file already exists, it updates its access and modification timestamps.

### 🖆 `cp` — Copy Files and Directories

Copy a file:

```bash
cp notes.txt backup.txt
```

Copy a file into another directory:

```bash
cp notes.txt /tmp/
```

Copy a directory recursively:

```bash
cp -r projects projects-backup
```

### 🖆 `mv` — Move or Rename

Move a file:

```bash
mv notes.txt /tmp/
```

Rename a file:

```bash
mv old-name.txt new-name.txt
```

### 🖆 `rm` — Remove Files and Directories

Remove a file:

```bash
rm notes.txt
```

Remove an empty directory:

```bash
rmdir empty-directory
```

Remove a directory and its contents:

```bash
rm -r directory
```

Force removal without interactive confirmation:

```bash
rm -rf directory
```

Use recursive and force deletion carefully because it can permanently remove large amounts of data.

### 🖆 `ln` — Create Links

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Create a symbolic link:

```bash
ln -s original.txt symlink.txt
```

A symbolic link points to another filesystem path, while a hard link references the same underlying inode.

# 🗁 File Content Commands

These commands help read, inspect, and process file contents.

### 🖆 `cat` — Display File Contents

```bash
cat notes.txt
```

Display multiple files:

```bash
cat file1.txt file2.txt
```

### 🖆 `less` — Read Files Interactively

```bash
less /var/log/syslog
```

Useful keys:

| Key | Action |
| --- | --- |
| `Space` | Next page |
| `b` | Previous page |
| `/word` | Search |
| `n` | Next match |
| `q` | Quit |

### 🖆 `more` — Page Through Files

```bash
more notes.txt
```

Displays file contents one screen at a time.

### 🖆 `head` — Display Beginning of a File

```bash
head notes.txt
```

Display the first five lines:

```bash
head -n 5 notes.txt
```

### 🖆 `tail` — Display End of a File

```bash
tail notes.txt
```

Display the last twenty lines:

```bash
tail -n 20 notes.txt
```

Follow a log file as new lines are written:

```bash
tail -f /var/log/syslog
```

### 🖆 `wc` — Count Lines, Words, and Bytes

```bash
wc notes.txt
```

Count lines:

```bash
wc -l notes.txt
```

Count words:

```bash
wc -w notes.txt
```

Count bytes:

```bash
wc -c notes.txt
```

# 🗁 Text Processing Commands

Linux provides powerful utilities for searching, transforming, sorting, and extracting text.

### 🖆 `grep` — Search Text Patterns

Search for a word:

```bash
grep "error" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Display matching line numbers:

```bash
grep -n "error" application.log
```

Search recursively:

```bash
grep -r "TODO" .
```

### 🖆 `sort` — Sort Lines

```bash
sort names.txt
```

Sort in reverse order:

```bash
sort -r names.txt
```

Sort numerically:

```bash
sort -n numbers.txt
```

### 🖆 `uniq` — Remove Adjacent Duplicate Lines

```bash
uniq names.txt
```

To remove duplicates regardless of their original positions, sort first:

```bash
sort names.txt | uniq
```

Count repeated adjacent lines:

```bash
sort names.txt | uniq -c
```

### 🖆 `cut` — Extract Fields or Characters

Extract the first field using a colon delimiter:

```bash
cut -d ':' -f 1 /etc/passwd
```

Extract the first three characters:

```bash
cut -c 1-3 notes.txt
```

### 🖆 `tr` — Translate or Delete Characters

Convert lowercase letters to uppercase:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
```

Remove digits:

```bash
echo "user123" | tr -d '0-9'
```

### 🖆 `sed` — Stream Editor

Replace text in command output:

```bash
sed 's/old/new/' notes.txt
```

Replace all occurrences on each line:

```bash
sed 's/old/new/g' notes.txt
```

Modify a file directly:

```bash
sed -i 's/old/new/g' notes.txt
```

### 🖆 `awk` — Text Processing

Print the first field:

```bash
awk '{print $1}' names.txt
```

Print the first and third fields:

```bash
awk '{print $1, $3}' data.txt
```

Extract usernames from `/etc/passwd`:

```bash
awk -F ':' '{print $1}' /etc/passwd
```

# 🗁 Search Commands

### 🖆 `find` — Search Filesystem Entries

Find files by name:

```bash
find . -type f -name "*.txt"
```

Find directories:

```bash
find . -type d -name "projects"
```

Find files larger than 100 MB:

```bash
find . -type f -size +100M
```

### 🖆 `locate` — Search Indexed Paths

```bash
locate filename.txt
```

Install the utility if it is not available:

```bash
sudo apt install plocate
```

Refresh its database when needed:

```bash
sudo updatedb
```

### 🖆 `which` — Find an Executable

```bash
which python3
```

### 🖆 `whereis` — Locate Related Files

```bash
whereis python3
```

# 🗁 Package Management Commands

Ubuntu uses APT (Advanced Package Tool) to manage software packages.

The `apt` command provides a convenient interface for installing, updating, and removing software.

### 🖆 `apt update` — Refresh Package Information

```bash
sudo apt update
```

Downloads updated package information from configured repositories.

It does not upgrade installed packages.

### 🖆 `apt upgrade` — Upgrade Installed Packages

```bash
sudo apt upgrade
```

Installs available upgrades for installed packages.

### 🖆 `apt install` — Install Software

```bash
sudo apt install nginx
```

Installs the Nginx web server.

### 🖆 `apt remove` — Remove Software

```bash
sudo apt remove nginx
```

Removes the package but generally leaves its configuration files behind.

### 🖆 `apt purge` — Remove Software and Package Configuration

```bash
sudo apt purge nginx
```

Removes the package and its system package configuration files.

### 🖆 `apt search` — Search Packages

```bash
apt search nginx
```

### 🖆 `apt show` — Display Package Information

```bash
apt show nginx
```

### 🖆 `apt autoremove` — Remove Unneeded Dependencies

```bash
sudo apt autoremove
```

Removes packages that were automatically installed and are no longer required.

### 🖆 `dpkg` — Manage Debian Packages

Install a local `.deb` package:

```bash
sudo dpkg -i package.deb
```

List installed packages:

```bash
dpkg -l
```

Display information about an installed package:

```bash
dpkg -s nginx
```

APT is generally preferred for package installation because it can resolve dependencies automatically.

# 🗁 Disk and Storage Commands

### 🖆 `df` — Filesystem Disk Usage

```bash
df -h
```

Displays available and used disk space for mounted filesystems.

### 🖆 `du` — Directory Disk Usage

```bash
du -sh projects
```

Displays the total disk usage of a directory.

Display sizes of immediate subdirectories:

```bash
du -h --max-depth=1 .
```

### 🖆 `lsblk` — List Block Devices

```bash
lsblk
```

Displays disks and partitions.

### 🖆 `mount` — Display Mounted Filesystems

```bash
mount
```

Displays currently mounted filesystems.

### 🖆 `findmnt` — Inspect Mount Points

```bash
findmnt
```

Displays filesystem mount information in a structured format.

# 🗁 Network Commands

Ubuntu provides several utilities for inspecting and troubleshooting network connectivity.

### 🖆 `ip` — Network Configuration

Display network interfaces and IP addresses:

```bash
ip addr
```

Display routing information:

```bash
ip route
```

Display network links:

```bash
ip link
```

### 🖆 `ping` — Test Network Connectivity

```bash
ping google.com
```

Limit the number of packets:

```bash
ping -c 4 google.com
```

### 🖆 `ss` — Inspect Network Sockets

Display listening TCP and UDP sockets:

```bash
ss -tuln
```

Display processes associated with sockets:

```bash
sudo ss -tulpn
```

### 🖆 `curl` — Transfer Data Using URLs

Send an HTTP request:

```bash
curl https://example.com
```

Display only response headers:

```bash
curl -I https://example.com
```

### 🖆 `wget` — Download Files

```bash
wget https://example.com/file.zip
```

### 🖆 `dig` — DNS Lookup

```bash
dig example.com
```

Install it if needed:

```bash
sudo apt install dnsutils
```

### 🖆 `traceroute` — Trace Network Routes

```bash
traceroute example.com
```

Install it if needed:

```bash
sudo apt install traceroute
```

# 🗁 Process Commands

These commands help inspect and control running processes.

### 🖆 `ps` — Display Processes

```bash
ps
```

Display processes belonging to all users:

```bash
ps aux
```

Display processes in a hierarchical format:

```bash
ps auxf
```

### 🖆 `top` — Monitor Processes

```bash
top
```

Displays running processes and system resource usage in real time.

### 🖆 `htop` — Interactive Process Monitor

```bash
htop
```

Install it:

```bash
sudo apt install htop
```

### 🖆 `kill` — Send a Signal to a Process

```bash
kill PID
```

Send a termination signal explicitly:

```bash
kill -TERM PID
```

Force termination:

```bash
kill -KILL PID
```

Use the actual process ID in place of `PID`.

### 🖆 `pgrep` — Search for Process IDs

```bash
pgrep nginx
```

### 🖆 `pkill` — Send Signals by Process Name

```bash
pkill nginx
```

### 🖆 `jobs` — Display Shell Jobs

```bash
jobs
```

Displays background and suspended jobs associated with the current shell.

# 🗁 Command Chaining

Bash provides operators for executing multiple commands in one command line.

### 🖆 Semicolon (`;`)

Executes commands sequentially regardless of success or failure.

```bash
pwd; whoami; date
```

### 🖆 AND Operator (`&&`)

Executes the next command only if the previous command succeeds.

```bash
mkdir project && cd project
```

### 🖆 OR Operator (`||`)

Executes the next command only if the previous command fails.

```bash
cd project || echo "Directory not found"
```

### 🖆 Combine Operators

```bash
mkdir project && cd project || echo "Operation failed"
```

The operators are evaluated according to shell precedence rules. Use parentheses when you need to group more complex command expressions.

### 🖆 Background Execution (`&`)

Runs a command asynchronously in the background.

```bash
sleep 60 &
```

The shell returns control without waiting for the command to finish.

# 🗁 Aliases and Command History

Bash provides aliases and command history to improve productivity.

### 🖆 `alias` — Create a Shortcut

```bash
alias ll='ls -lah'
```

Now execute:

```bash
ll
```

Display configured aliases:

```bash
alias
```

Remove an alias:

```bash
unalias ll
```

To make an alias persistent, add it to:

```bash
~/.bashrc
```

### 🖆 `history` — Display Previous Commands

```bash
history
```

Display the last ten commands:

```bash
history 10
```

Search command history interactively:

```text
Ctrl + R
```

Search backward through previous commands.

Execute a command by its history number:

```bash
!25
```

This executes the command associated with history entry `25`.

# 🗁 Useful Command Combinations

Combining Linux utilities creates powerful workflows.

### 🖆 Find Large Files

```bash
find . -type f -size +100M
```

### 🖆 Count Files in a Directory

```bash
find . -maxdepth 1 -type f | wc -l
```

### 🖆 Find a Specific Word in Multiple Files

```bash
grep -rn "TODO" .
```

### 🖆 Display the Largest Directories

```bash
du -h --max-depth=1 . | sort -hr | head -10
```

### 🖆 Check Listening Network Ports

```bash
sudo ss -tulpn
```

### 🖆 Monitor System Logs

```bash
sudo journalctl -f
```

### 🖆 Check Disk Space

```bash
df -h
```

### 🖆 Display Recently Modified Files

```bash
find . -type f -mtime -1
```

# 🗁 Best Practices

* Use `man` and `--help` to understand unfamiliar commands.
* Prefer APT for installing Ubuntu software from configured repositories.
* Use `sudo` only when elevated privileges are necessary.
* Quote filenames and arguments containing spaces.
* Verify file paths before running commands that modify or delete data.
* Use `find` with specific conditions to avoid unnecessary filesystem traversal.
* Use `grep -n` when line numbers are useful.
* Use `less` for inspecting large text files.
* Use `df -h` and `du -sh` to investigate disk usage.
* Use `ss` instead of relying on older networking utilities such as `netstat`.
* Use `ip` instead of the older `ifconfig` command.
* Prefer `command -v` for checking how Bash resolves a command.
* Practice command combinations in a test directory.

# 🗁 Common Mistakes

### 🖆 Confusing `apt update` with `apt upgrade`

**Incorrect assumption:** `apt update` upgrades installed software.

**Correct understanding:**

```bash
sudo apt update
```

Refreshes package metadata.

```bash
sudo apt upgrade
```

Upgrades installed packages.

### 🖆 Using `rm` Without Understanding Its Options

**Problem:**

```bash
rm -rf important-directory
```

This recursively and forcibly removes the specified directory.

**Better approach:**

Inspect the target first:

```bash
ls -la important-directory
```

Use destructive commands only when the target is verified.

### 🖆 Forgetting Quotes Around Filenames

**Problem:**

```bash
cat my notes.txt
```

The shell interprets this as multiple arguments.

**Solution:**

```bash
cat "my notes.txt"
```

### 🖆 Using `uniq` Without Sorting

**Problem:**

```bash
uniq names.txt
```

`uniq` removes only adjacent duplicate lines.

**Solution:**

```bash
sort names.txt | uniq
```

### 🖆 Assuming Every Command Is an External Program

Some commands, such as `cd`, are Bash built-ins.

Check a command with:

```bash
type cd
```

# 🗁 Practical Exercises

### 🖆 Exercise: System Information

Execute the following commands and identify what each one displays:

```bash
uname -a
hostnamectl
uptime
free -h
lscpu
lsblk
```

### 🖆 Exercise: User Information

```bash
whoami
id
groups
who
```

Identify your username, UID, primary group, and supplementary groups.

### 🖆 Exercise: File Management

Create a practice directory and perform the following operations:

```bash
mkdir linux-practice
cd linux-practice

touch notes.txt
mkdir documents backups

cp notes.txt backups/
mv notes.txt notes-backup.txt

ls -lah
```

### 🖆 Exercise: Text Processing

Create a sample file:

```bash
cat > names.txt <<EOF
Charlie
Alice
Bob
Alice
David
Bob
EOF
```

Sort the names:

```bash
sort names.txt
```

Find unique names:

```bash
sort names.txt | uniq
```

Count occurrences:

```bash
sort names.txt | uniq -c
```

### 🖆 Exercise: Package Management

Refresh the package information:

```bash
sudo apt update
```

Search for `htop`:

```bash
apt search htop
```

Display package information:

```bash
apt show htop
```

Install it:

```bash
sudo apt install htop
```

### 🖆 Exercise: Network Inspection

```bash
ip addr
ip route
ping -c 4 google.com
ss -tuln
curl -I https://example.com
```

Identify your network interfaces, default route, connectivity status, listening sockets, and HTTP response headers.

# 🗁 Interview Questions

### 🖆 What is the difference between a command and an argument?

A command identifies the operation to execute, while an argument provides input or a target for that operation.

### 🖆 What is the difference between short and long options?

Short options generally use a single hyphen, such as `-l`. Long options generally use two hyphens, such as `--all`.

### 🖆 What is the difference between `which` and `whereis`?

`which` locates an executable through the shell's command search path. `whereis` searches for related binary, source, and manual-page locations.

### 🖆 What is the difference between `cp` and `mv`?

`cp` copies files or directories, while `mv` moves or renames them.

### 🖆 What is the difference between `rm` and `rmdir`?

`rm` removes files and can remove directories recursively with `-r`. `rmdir` removes empty directories only.

### 🖆 What is the difference between `head` and `tail`?

`head` displays the beginning of a file, while `tail` displays its end.

### 🖆 What is the purpose of `grep`?

`grep` searches text for lines matching specified patterns.

### 🖆 What is the difference between `sort` and `uniq`?

`sort` orders lines. `uniq` removes or counts adjacent duplicate lines.

### 🖆 What is the difference between `df` and `du`?

`df` reports filesystem-level disk usage, while `du` reports disk space used by files and directories.

### 🖆 What is the difference between `ps` and `top`?

`ps` displays a snapshot of process information, while `top` provides an interactive, continuously updating process monitor.

### 🖆 What is the difference between `apt update` and `apt upgrade`?

`apt update` refreshes package metadata. `apt upgrade` installs available upgrades for installed packages.

### 🖆 What is the purpose of `&&` and `||`?

`&&` executes the next command when the previous command succeeds. `||` executes the next command when the previous command fails.

# 🗁 References

* [Ubuntu Documentation](https://help.ubuntu.com/)
* [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/)
* [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/)
* [GNU Grep Manual](https://www.gnu.org/software/grep/manual/)
* [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
* [APT Documentation](https://manpages.ubuntu.com/manpages/)
* [Linux Manual Pages](https://man7.org/linux/man-pages/)
