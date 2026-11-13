
# Linux Directory Navigation

Directory navigation is the process of moving through the Linux filesystem, inspecting directory contents, and locating files from the terminal.

Ubuntu uses a hierarchical filesystem structure that starts at the root directory (`/`). Understanding how to navigate this structure is essential for managing files, working with projects, configuring the system, and performing administrative tasks.

This chapter covers the essential Linux navigation commands, path structures, directory inspection, and filesystem search techniques.

# 🗁 Understanding Linux Paths

A path identifies the location of a file or directory in the Linux filesystem.

Linux uses a hierarchical directory structure beginning at `/`, known as the root directory.

There are two primary types of paths:

### 🖆 Absolute Paths

An absolute path specifies the complete location of a file or directory starting from the filesystem root.

Example:

```bash
/home/user/projects/app
```

Absolute paths always begin with `/`.

You can use an absolute path regardless of your current working directory:

```bash
cd /home/user/projects
```

### 🖆 Relative Paths

A relative path specifies a location based on the current working directory.

Example:

```bash
cd projects/app
```

If your current directory is `/home/user`, the resulting location is:

```text
/home/user/projects/app
```

Relative paths do not begin with `/`.

### 🖆 Special Path Symbols

| Symbol | Meaning |
| --- | --- |
| `/` | Filesystem root directory |
| `~` | Current user's home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `-` | Previous directory when used with `cd` |

Examples:

```bash
cd /
cd ~
cd .
cd ..
cd -
```

### 🖆 Absolute vs Relative Paths

| Absolute Path | Relative Path |
| --- | --- |
| Starts from `/` | Starts from the current directory |
| Independent of the current directory | Depends on the current directory |
| Specifies the complete path | Usually shorter |
| Useful for fixed system locations | Useful for project navigation |

# 🗁 Current Working Directory

The current working directory is the directory in which the shell is currently operating.

Commands that use relative paths generally resolve those paths from this location.

### 🖆 `pwd` — Print Working Directory

The `pwd` command displays the current working directory.

Syntax:

```bash
pwd [OPTION]
```

Example:

```bash
pwd
```

Output:

```text
/home/user/projects
```

### 🖆 Logical Path

```bash
pwd -L
```

Displays the logical working directory, preserving symbolic-link components where supported.

### 🖆 Physical Path

```bash
pwd -P
```

Displays the physical directory path with symbolic links resolved.

Example:

```text
Logical path:  /home/user/current-project
Physical path: /mnt/storage/projects/my-project
```

The paths may differ when a directory is accessed through a symbolic link.

### 🖆 Check the Current Directory Using Bash

Bash also provides the `PWD` environment variable:

```bash
echo "$PWD"
```

It normally contains the logical path of the current working directory.

# 🗁 Changing Directories

The `cd` command changes the current working directory.

Syntax:

```bash
cd [DIRECTORY]
```

### 🖆 Navigate to the Home Directory

```bash
cd
```

Or:

```bash
cd ~
```

Both commands navigate to the current user's home directory.

Example:

```text
/home/user
```

### 🖆 Navigate to a Specific Directory

```bash
cd /var/log
```

Navigates to the system log directory.

### 🖆 Navigate to the Parent Directory

```bash
cd ..
```

Moves one directory level upward.

Example:

```text
Current: /home/user/projects/app
After:   /home/user/projects
```

### 🖆 Navigate to the Root Directory

```bash
cd /
```

Moves to the top-level directory of the Linux filesystem.

### 🖆 Navigate to the Previous Directory

```bash
cd -
```

Switches to the previous working directory.

Example:

```bash
cd /var/log
cd /etc
cd -
```

The final command returns to `/var/log`.

### 🖆 Navigate Multiple Levels Up

Move up two levels:

```bash
cd ../..
```

Move up three levels:

```bash
cd ../../..
```

### 🖆 Navigate to a Child Directory

```bash
cd projects/frontend
```

Moves into the `frontend` directory if it exists relative to the current working directory.

### 🖆 Navigate to a Directory with Spaces

Use quotes around directory names containing spaces:

```bash
cd "My Projects"
```

Alternatively, escape the space:

```bash
cd My\ Projects
```

# 🗁 Listing Directory Contents

The `ls` command displays files and directories.

Syntax:

```bash
ls [OPTION] [FILE_OR_DIRECTORY]
```

### 🖆 Basic Directory Listing

```bash
ls
```

Displays the contents of the current directory.

List a specific directory:

```bash
ls /var/log
```

### 🖆 Detailed Listing

```bash
ls -l
```

Displays detailed information about directory entries.

Example:

```text
drwxr-xr-x  2 user user 4096 Nov  8 09:00 projects
-rw-r--r--  1 user user 1024 Nov  8 09:15 notes.txt
```

The output includes:

* File type and permissions.
* Number of hard links.
* Owner.
* Group.
* File size.
* Modification timestamp.
* Filename.

### 🖆 Display Hidden Files

```bash
ls -a
```

Includes hidden entries, including `.` and `..`.

### 🖆 Display Hidden Files Without `.` and `..`

```bash
ls -A
```

Includes hidden entries but excludes the current and parent directory references.

### 🖆 Human-Readable File Sizes

```bash
ls -lh
```

Displays file sizes using readable units such as KB, MB, and GB.

### 🖆 List Files Recursively

```bash
ls -R
```

Displays directory contents recursively.

### 🖆 Sort by Modification Time

```bash
ls -lt
```

Displays the most recently modified entries first.

Reverse the order:

```bash
ls -ltr
```

### 🖆 Display Directory Entries Themselves

```bash
ls -ld /var/log
```

Displays information about the directory itself rather than its contents.

### 🖆 Display Directory Names

```bash
ls -d */
```

Lists directories in the current location using Bash pathname expansion.

### 🖆 Common `ls` Options

| Option | Description |
| --- | --- |
| `-l` | Detailed listing |
| `-a` | Include all entries |
| `-A` | Include hidden entries except `.` and `..` |
| `-h` | Human-readable sizes |
| `-R` | Recursive listing |
| `-t` | Sort by modification time |
| `-r` | Reverse sorting order |
| `-d` | Display directory entries themselves |
| `-F` | Append file-type indicators |

Combine options:

```bash
ls -lah
```

# 🗁 Viewing Directory Structures

The `tree` command displays directory contents in a hierarchical tree format.

It is particularly useful for understanding project structures.

### 🖆 Basic Usage

```bash
tree
```

Example output:

```text
project/
├── README.md
├── package.json
├── src/
│   ├── components/
│   │   └── Header.js
│   └── index.js
├── public/
└── tests/
    └── app.test.js
```

### 🖆 Limit Directory Depth

```bash
tree -L 2
```

Displays the directory structure up to two levels deep.

### 🖆 Include Hidden Files

```bash
tree -a
```

Includes hidden files and directories.

### 🖆 Display Directories Only

```bash
tree -d
```

Displays directories without listing regular files.

### 🖆 Combine Options

```bash
tree -a -L 3
```

Displays hidden entries up to three directory levels.

### 🖆 Install `tree` on Ubuntu

The `tree` utility may not be installed by default.

Install it using APT:

```bash
sudo apt update
sudo apt install tree
```

Verify the installation:

```bash
tree --version
```

# 🗁 Searching for Files and Directories

Linux provides utilities for finding files and directories by name, type, size, modification time, and other properties.

### 🖆 `find` — Search the Filesystem

The `find` command searches directory hierarchies and evaluates conditions against filesystem entries.

Syntax:

```bash
find [PATH] [EXPRESSION]
```

Search the current directory recursively:

```bash
find .
```

Search a specific directory:

```bash
find /home/user/projects
```

### 🖆 Search by Filename

```bash
find . -name "notes.txt"
```

Search for files with a specific extension:

```bash
find . -name "*.js"
```

### 🖆 Case-Insensitive Search

```bash
find . -iname "README.md"
```

Matches filenames regardless of letter case.

### 🖆 Search by File Type

Find regular files:

```bash
find . -type f
```

Find directories:

```bash
find . -type d
```

Find symbolic links:

```bash
find . -type l
```

Common file types:

| Option | File Type |
| --- | --- |
| `-type f` | Regular file |
| `-type d` | Directory |
| `-type l` | Symbolic link |
| `-type c` | Character device |
| `-type b` | Block device |
| `-type p` | Named pipe |
| `-type s` | Unix domain socket |

### 🖆 Limit Search Depth

Search the current directory and its immediate children:

```bash
find . -maxdepth 2
```

Start searching at a minimum depth:

```bash
find . -mindepth 2
```

Combine both:

```bash
find . -mindepth 2 -maxdepth 3
```

### 🖆 Search by Modification Time

Find files modified within the last seven days:

```bash
find . -type f -mtime -7
```

Find files modified more than seven days ago:

```bash
find . -type f -mtime +7
```

Find files modified within the last 24 hours:

```bash
find . -type f -mmin -1440
```

### 🖆 Search by File Size

Find files larger than 100 MB:

```bash
find . -type f -size +100M
```

Find files smaller than 1 MB:

```bash
find . -type f -size -1M
```

### 🖆 Search by Path

```bash
find . -path "*/src/*.js"
```

Matches paths against the supplied pattern.

### 🖆 Search by Owner

Find files owned by a specific user:

```bash
find /home -user username
```

### 🖆 Search by Group

```bash
find /home -group developers
```

### 🖆 Execute Commands on Search Results

The `-exec` option executes another command for matching entries.

Display detailed information about matching files:

```bash
find . -type f -name "*.log" -exec ls -lh {} \;
```

The components mean:

* `{}` represents the current search result.
* `\;` terminates the command executed for each result.

A more efficient approach processes multiple results together:

```bash
find . -type f -name "*.log" -exec ls -lh {} +
```

The `+` form passes multiple matching paths to the command where possible.

Use `-exec` carefully when the executed command modifies or deletes files.

### 🖆 `locate` — Search Using an Index

The `locate` command searches a prebuilt filename database.

Example:

```bash
locate notes.txt
```

Install the Ubuntu package if necessary:

```bash
sudo apt update
sudo apt install plocate
```

Refresh the database:

```bash
sudo updatedb
```

Unlike `find`, `locate` searches an index rather than traversing the filesystem for every query.

Its results depend on database freshness.

### 🖆 `find` vs `locate`

| Feature | `find` | `locate` |
| --- | --- | --- |
| Search method | Filesystem traversal | Indexed database |
| Results | Reflect current filesystem state | Depend on database freshness |
| Speed | Can be slower for large trees | Usually very fast |
| Filtering | Extensive search conditions | Primarily filename/path matching |
| Best use | Precise filesystem searches | Quick filename lookup |

# 🗁 Understanding Hidden Files

In Linux, filenames beginning with a dot (`.`) are treated as hidden by convention.

Examples:

```text
.bashrc
.profile
.gitignore
.env
.config/
.ssh/
```

Hidden files are not displayed by a normal `ls` command.

### 🖆 Display Hidden Files

```bash
ls -a
```

### 🖆 Display Hidden Files in Detailed Format

```bash
ls -lah
```

### 🖆 Navigate to a Hidden Directory

```bash
cd ~/.config
```

### 🖆 Search for Hidden Files

```bash
find . -name ".*"
```

**Important:** Hidden files are not automatically protected. Their visibility is simply controlled by naming conventions and command behavior.

# 🗁 Wildcards and Pattern Matching

Bash supports pathname expansion, commonly called globbing.

It allows commands to work with multiple filenames without specifying every filename individually.

The shell normally expands unquoted wildcard patterns before passing arguments to a command.

### 🖆 Asterisk (`*`)

Matches zero or more characters within a filename component.

```bash
ls *.txt
```

Matches files such as:

```text
notes.txt
report.txt
readme.txt
```

### 🖆 Question Mark (`?`)

Matches exactly one character.

```bash
ls file?.txt
```

Matches:

```text
file1.txt
file2.txt
fileA.txt
```

It does not match `file10.txt`.

### 🖆 Character Set (`[abc]`)

Matches one character from the specified set.

```bash
ls file[123].txt
```

Matches:

```text
file1.txt
file2.txt
file3.txt
```

### 🖆 Character Range (`[a-z]`)

Matches one character within the specified range.

```bash
ls [a-z]*.txt
```

Matches `.txt` filenames beginning with a lowercase letter.

### 🖆 Negated Character Set

Matches one character that is not in the specified set.

```bash
ls file[!0-9].txt
```

Matches filenames where the character after `file` is not a digit.

### 🖆 Hidden Files and Globbing

By default, Bash pathname expansion does not match filenames beginning with `.` when the pattern does not explicitly begin with a dot.

For example:

```bash
ls *
```

Normally excludes hidden entries.

To match hidden filenames explicitly:

```bash
ls .*
```

For a general hidden-file listing, prefer:

```bash
ls -A
```

### 🖆 Quoting Wildcards

Quote patterns when you want the command to receive the pattern literally.

Example:

```bash
find . -name "*.js"
```

The shell passes `"*.js"` as one argument, and `find` performs the filename matching.

Without quotes:

```bash
find . -name *.js
```

Bash may expand `*.js` before `find` receives it, causing unexpected results.

# 🗁 Directory Stack Navigation

Bash provides directory stack commands that help users move between multiple locations.

### 🖆 `pushd` — Save and Change Directory

```bash
pushd /var/log
```

Changes to `/var/log` and saves the previous directory in the directory stack.

### 🖆 `popd` — Return Through the Directory Stack

```bash
popd
```

Removes the top directory from the stack and changes to the new top entry.

### 🖆 `dirs` — Display Directory Stack

```bash
dirs
```

Displays the current directory stack.

### 🖆 Example Workflow

```bash
cd ~
pushd /var/log
pushd /etc
dirs
popd
popd
```

This provides a convenient way to switch between several working locations.

# 🗁 Tab Completion

Bash supports tab completion for commands, filenames, and directory names.

It reduces typing and helps prevent spelling mistakes.

### 🖆 Complete a Directory Name

Suppose the current directory contains:

```text
projects/
documents/
downloads/
```

Start typing:

```bash
cd pro
```

Press `Tab`.

Bash may complete the command to:

```bash
cd projects/
```

### 🖆 Display Available Matches

If multiple entries match the same prefix, press `Tab` twice to display available matches.

Example:

```bash
cd do
```

Press `Tab` twice to inspect possible completions.

### 🖆 Complete Command Names

You can also use Tab completion when entering commands.

For example:

```bash
sys
```

Press `Tab` twice to display matching command names, depending on your Bash completion configuration.

# 🗁 Practical Navigation Workflow

Consider the following Ubuntu project structure:

```text
my-project/
├── README.md
├── package.json
├── .gitignore
├── src/
│   ├── components/
│   ├── pages/
│   └── index.js
├── public/
└── tests/
```

### 🖆 Inspect the Current Location

```bash
pwd
```

### 🖆 Explore the Project Root

```bash
ls -lah
```

### 🖆 View the Project Structure

```bash
tree -a -L 3
```

### 🖆 Navigate to the Source Directory

```bash
cd src
```

### 🖆 Inspect Source Files

```bash
ls -lh
```

### 🖆 Navigate to Components

```bash
cd components
```

### 🖆 Return to the Project Root

```bash
cd ../..
```

### 🖆 Search for JavaScript Files

```bash
find . -type f -name "*.js"
```

### 🖆 Search for Configuration Files

```bash
find . -type f -name "*.json"
```

### 🖆 Find Hidden Configuration Files

```bash
find . -maxdepth 2 -type f -name ".*"
```

This workflow is useful when exploring unfamiliar Linux projects and repositories.

# 🗁 Best Practices

* Use `pwd` to verify your current location before executing important commands.
* Use absolute paths when a command must target a fixed filesystem location.
* Use relative paths for operations within a project.
* Use `ls -lah` to inspect visible and hidden entries.
* Use `tree -L` to understand unfamiliar directory structures.
* Use `find` for accurate filesystem searches with specific conditions.
* Use `locate` for fast filename searches when its database is available.
* Quote wildcard patterns passed to commands such as `find`.
* Use Tab completion to reduce typing errors.
* Learn `cd -`, `pushd`, and `popd` to move efficiently between directories.
* Avoid running filesystem searches across the entire system unnecessarily.
* Verify target paths before performing destructive operations.

# 🗁 Common Mistakes

### 🖆 Confusing Absolute and Relative Paths

**Problem:**

```bash
cd home/user/projects
```

This is interpreted as a relative path.

**Solution:**

```bash
cd /home/user/projects
```

Use `/` at the beginning for an absolute path.

### 🖆 Forgetting Hidden Files

**Problem:**

```bash
ls
```

Does not normally display `.env` or `.gitignore`.

**Solution:**

```bash
ls -la
```

### 🖆 Forgetting to Quote Search Patterns

**Problem:**

```bash
find . -name *.log
```

Bash may expand the wildcard before `find` receives it.

**Solution:**

```bash
find . -name "*.log"
```

### 🖆 Assuming `locate` Always Shows Current Results

**Problem:**

A newly created file does not appear in the search results.

**Solution:**

Refresh the database when appropriate:

```bash
sudo updatedb
```

Alternatively, use `find` for a live filesystem search.

### 🖆 Using `cd` with a Directory Name Containing Spaces

**Problem:**

```bash
cd My Projects
```

Bash interprets this as multiple arguments.

**Solution:**

```bash
cd "My Projects"
```

Or:

```bash
cd My\ Projects
```

# 🗁 Practical Exercises

### 🖆 Exercise: Basic Navigation

Execute the following commands:

```bash
pwd
cd ~
pwd
cd /
pwd
cd -
pwd
```

Observe how the working directory changes.

### 🖆 Exercise: Directory Inspection

* List the current directory.
* Display hidden entries.
* Display detailed file information.
* Sort entries by modification time.
* Display directory contents recursively.

Commands to practice:

```bash
ls
ls -a
ls -lah
ls -lt
ls -R
```

### 🖆 Exercise: Directory Tree

* Display your home directory structure.
* Limit the output to two levels.
* Include hidden entries.
* Display directories only.

```bash
tree ~ -L 2
tree -a ~ -L 2
tree -d ~
```

### 🖆 Exercise: File Searching

Find all `.txt` files:

```bash
find ~ -type f -name "*.txt"
```

Find all `.js` files:

```bash
find ~ -type f -name "*.js"
```

Find directories named `node_modules`:

```bash
find ~ -type d -name "node_modules"
```

Find files larger than 50 MB:

```bash
find ~ -type f -size +50M
```

Find files modified within the last three days:

```bash
find ~ -type f -mtime -3
```

### 🖆 Exercise: Path Understanding

Assume your current directory is:

```text
/home/user/projects/app/src/components
```

Determine the resulting directory after executing each command:

```bash
cd ..
```

```bash
cd ../..
```

```bash
cd ../../../
```

```bash
cd ~
```

### 🖆 Exercise: Directory Stack

Practice switching between multiple directories:

```bash
cd ~
pushd /etc
pushd /var/log
dirs
popd
popd
```

Observe how the directory stack changes.

# 🗁 Interview Questions

### 🖆 What does the `pwd` command do?

It displays the current working directory.

### 🖆 What is the difference between `pwd -L` and `pwd -P`?

`pwd -L` displays the logical path, while `pwd -P` displays the physical path with symbolic links resolved.

### 🖆 What does the `cd` command do?

It changes the current working directory.

### 🖆 What is the difference between `cd ..` and `cd -`?

* `cd ..` moves to the parent directory.
* `cd -` switches to the previous working directory.

### 🖆 What is the difference between `ls -a` and `ls -A`?

`ls -a` includes all entries, including `.` and `..`. `ls -A` includes hidden entries but excludes `.` and `..`.

### 🖆 What is the purpose of the `tree` command?

It displays directory contents in a hierarchical tree structure.

### 🖆 What is the difference between `find` and `locate`?

`find` traverses the filesystem and evaluates search conditions. `locate` searches a prebuilt filename database.

### 🖆 What is the difference between absolute and relative paths?

An absolute path starts from the filesystem root `/`. A relative path starts from the current working directory.

### 🖆 What does the `~` symbol represent?

It represents the current user's home directory through Bash tilde expansion.

### 🖆 What is the purpose of wildcard characters?

Wildcards match filename patterns, allowing commands to operate on multiple matching entries.

### 🖆 What is the difference between `pushd` and `popd`?

`pushd` changes directories while maintaining a directory stack. `popd` removes the top entry and navigates to the new top entry.

### 🖆 Why should wildcard patterns be quoted when using `find`?

Quoting prevents Bash from expanding the wildcard before `find` receives the pattern.

# 🗁 References

* [Ubuntu Documentation](https://help.ubuntu.com/)
* [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/)
* [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/)
* [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
* [Linux Manual Pages](https://man7.org/linux/man-pages/)
