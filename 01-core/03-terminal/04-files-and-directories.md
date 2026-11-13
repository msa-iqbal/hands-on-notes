# Files and Directories in Linux

Linux provides a collection of commands for creating, copying, moving, renaming, deleting, inspecting, and managing files and directories.

Understanding these operations is essential for working efficiently in the Ubuntu terminal, managing projects, organizing data, and administering Linux systems.

This guide covers the most commonly used file and directory management commands in Ubuntu/Linux.

# 🗁 Linux Filesystem Basics

Linux organizes files and directories in a hierarchical filesystem structure.

Unlike systems that use drive letters, Linux starts its filesystem hierarchy from the root directory:

```bash
/
```

Every file, directory, mounted filesystem, and storage location exists somewhere beneath this root.

### 🖆 Files and Directories

A **file** stores data or represents an object in the filesystem.

A **directory** stores references to filesystem entries, allowing files and other directories to be organized hierarchically.

Common filesystem locations:

| Directory | Purpose |
| --- | --- |
| `/` | Root of the filesystem |
| `/home` | User home directories |
| `/root` | Root user's home directory |
| `/etc` | System configuration files |
| `/var` | Variable data, logs, caches |
| `/tmp` | Temporary files |
| `/usr` | User-space programs and resources |
| `/opt` | Optional third-party software |
| `/dev` | Device files |
| `/proc` | Virtual process and kernel information |
| `/mnt` | Temporary mount points |
| `/media` | Removable media mount points |

### 🖆 Common Linux File Types

Linux filesystems support several types of filesystem objects.

| Symbol | Type | Description |
| --- | --- | --- |
| `-` | Regular file | Text, source code, images, binaries |
| `d` | Directory | Contains directory entries |
| `l` | Symbolic link | References another path |
| `c` | Character device | Character-oriented device interface |
| `b` | Block device | Block-oriented device interface |
| `p` | FIFO | Named pipe for inter-process communication |
| `s` | Socket | Communication endpoint |

Use `ls -l` to inspect file types:

```bash
ls -l
```

Example:

```text
-rw-r--r--  1 user user  1200 Nov 08 10:00 notes.txt
drwxr-xr-x  2 user user  4096 Nov 08 10:05 projects
lrwxrwxrwx  1 user user    10 Nov 08 10:10 shortcut -> notes.txt
```

The first character identifies the file type.

### 🖆 Understanding Inodes

An inode is a filesystem data structure that stores metadata about a filesystem object.

It typically contains:

* File type and permissions.
* Owner and group IDs.
* File size.
* Timestamps.
* Link count.
* Pointers or references to the file's data blocks.

An inode does not normally store the filename. Directory entries associate filenames with inode numbers.

Inspect inode numbers:

```bash
ls -i
```

Get detailed inode information:

```bash
stat notes.txt
```

# 🗁 Creating Files

Linux provides several ways to create files.

### 🖆 Using `touch`

The `touch` command creates an empty file if it does not exist.

```bash
touch notes.txt
```

Create multiple files:

```bash
touch file1.txt file2.txt file3.txt
```

Create files with spaces in their names:

```bash
touch "project notes.txt"
```

Create files inside another directory:

```bash
touch ~/Documents/notes.txt
```

If a file already exists, `touch` updates its access and modification timestamps by default without changing its contents.

### 🖆 Creating Files with Redirection

You can create an empty file using shell output redirection:

```bash
> notes.txt
```

Create a file containing text:

```bash
echo "Hello Linux" > notes.txt
```

Append text to an existing file:

```bash
echo "Learning Ubuntu" >> notes.txt
```

The `>` and `>>` operators are explained in detail in the Input/Output and Redirection chapters.

### 🖆 Creating Files with `cat`

Create a file and enter multiple lines:

```bash
cat > notes.txt
```

Type your content:

```text
Linux is an operating system.
Ubuntu is a Linux distribution.
The terminal provides command-line access.
```

Press `Ctrl + D` to finish and save the input.

# 🗁 Creating Directories

Use `mkdir` to create directories.

### 🖆 Creating a Single Directory

```bash
mkdir projects
```

Create multiple directories:

```bash
mkdir documents downloads backups
```

### 🖆 Creating Nested Directories

Use the `-p` option to create parent directories automatically.

```bash
mkdir -p projects/web/frontend
```

This creates the complete directory structure even if the parent directories do not exist.

### 🖆 Creating Multiple Nested Directories

```bash
mkdir -p projects/{frontend,backend,database}
```

Create a nested project structure:

```bash
mkdir -p my-project/{src,tests,docs,config}
```

Bash expands the brace expressions into multiple directory names.

# 🗁 Copying Files and Directories

The `cp` command copies files and directories.

### 🖆 Copying a File

```bash
cp source.txt destination.txt
```

Copy a file into another directory:

```bash
cp notes.txt ~/Documents/
```

Copy multiple files into a directory:

```bash
cp file1.txt file2.txt ~/Documents/
```

The destination directory must already exist.

### 🖆 Copying Directories

Use `-r` to copy a directory recursively.

```bash
cp -r source-directory destination-directory
```

Example:

```bash
cp -r projects projects-backup
```

This copies the directory and its contents.

### 🖆 Useful `cp` Options

| Option | Description |
| --- | --- |
| `-r` | Copy directories recursively |
| `-a` | Archive mode; preserve attributes and copy symbolic links as links |
| `-i` | Ask before overwriting |
| `-u` | Copy only when the source is newer or the destination is missing |
| `-v` | Display copied files |
| `-n` | Do not overwrite existing destination files |

Examples:

```bash
cp -i notes.txt backup.txt
```

```bash
cp -v file.txt ~/Documents/
```

```bash
cp -a projects/ projects-backup/
```

For backups where preserving metadata and symbolic links matters, `cp -a` is often more appropriate than a basic recursive copy.

# 🗁 Moving and Renaming Files and Directories

The `mv` command moves or renames files and directories.

### 🖆 Renaming a File

```bash
mv old-name.txt new-name.txt
```

Example:

```bash
mv draft.txt final.txt
```

### 🖆 Moving a File

```bash
mv notes.txt ~/Documents/
```

### 🖆 Moving and Renaming at the Same Time

```bash
mv notes.txt ~/Documents/important-notes.txt
```

### 🖆 Moving a Directory

```bash
mv projects ~/Documents/
```

### 🖆 Useful `mv` Options

| Option | Description |
| --- | --- |
| `-i` | Ask before overwriting |
| `-n` | Do not overwrite an existing destination |
| `-u` | Move only when the source is newer or the destination is missing |
| `-v` | Display the operation |

Examples:

```bash
mv -i old.txt new.txt
```

```bash
mv -v projects ~/Documents/
```

A move within the same filesystem is generally performed by changing directory entries rather than copying all file contents. Moving across filesystems may require copying and removing the original.

# 🗁 Removing Files and Directories

Linux provides different commands for deleting files and directories.

### 🖆 Removing a File with `rm`

```bash
rm notes.txt
```

Remove multiple files:

```bash
rm file1.txt file2.txt file3.txt
```

### 🖆 Useful `rm` Options

| Option | Description |
| --- | --- |
| `-i` | Ask before removing each file |
| `-f` | Force removal without prompting for nonexistent files |
| `-r` | Remove directories and their contents recursively |
| `-v` | Display removed files |

Examples:

```bash
rm -i notes.txt
```

```bash
rm -v file.txt
```

### 🖆 Removing Directories

The `rmdir` command removes empty directories only.

```bash
rmdir empty-directory
```

Remove a directory containing files and subdirectories:

```bash
rm -r projects
```

For interactive confirmation:

```bash
rm -ri projects
```

### 🖆 Understanding Recursive Deletion

The following command recursively removes a directory and its contents:

```bash
rm -rf directory-name
```

Use recursive deletion carefully. It does not normally move files to the desktop Trash or provide an automatic recovery mechanism.

Before deleting a directory, inspect its contents:

```bash
pwd
ls -la directory-name
```

Avoid running destructive commands with broad or unclear paths.

# 🗁 Hard Links and Symbolic Links

Linux supports two common types of filesystem links:

* Hard links.
* Symbolic links, also called soft links.

### 🖆 Hard Links

A hard link is another directory entry that references the same inode as an existing file.

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Inspect inode numbers:

```bash
ls -li original.txt hardlink.txt
```

Both filenames should show the same inode number.

Important characteristics:

* Both names refer to the same underlying file data.
* Changes made through either name affect the same file.
* Removing one filename does not remove the data while another hard link still references it.
* Hard links generally cannot cross filesystem boundaries.
* Creating hard links to directories is normally restricted.

### 🖆 Symbolic Links

A symbolic link stores a path pointing to another file or directory.

Create a symbolic link:

```bash
ln -s original.txt symlink.txt
```

Create a symbolic link to a directory:

```bash
ln -s ~/projects ~/projects-shortcut
```

Inspect symbolic links:

```bash
ls -l
```

Read the link target:

```bash
readlink symlink.txt
```

Important characteristics:

* A symbolic link has its own inode.
* It points to a pathname rather than sharing the target's inode.
* It can reference files and directories.
* It can cross filesystem boundaries.
* It can become a dangling link if the target is removed or moved.

### 🖆 Hard Link vs Symbolic Link

| Feature | Hard Link | Symbolic Link |
| --- | --- | --- |
| Inode | Same as target | Different inode |
| References | Underlying file inode | Target pathname |
| Directories | Normally restricted | Supported |
| Cross-filesystem | No | Yes |
| Broken link | Not in the same way | Can become dangling |
| Target deletion | Data remains while another hard link exists | Link may become dangling |

# 🗁 Inspecting Files and Directories

Linux includes several commands for inspecting filesystem objects.

### 🖆 Using `file`

The `file` command identifies a file's type using information such as its contents and filesystem metadata.

```bash
file notes.txt
```

Example:

```text
notes.txt: ASCII text
```

Inspect an image:

```bash
file image.png
```

Inspect a binary executable:

```bash
file /usr/bin/ls
```

A filename extension does not determine the actual file type. The `file` command can identify content even when the extension is missing or misleading.

### 🖆 Using `stat`

The `stat` command displays detailed metadata.

```bash
stat notes.txt
```

Typical information includes:

* File size.
* Inode number.
* Permissions.
* Owner and group.
* Access time.
* Modification time.
* Change time.
* Filesystem block information.

### 🖆 Using `ls -li`

Display detailed file information and inode numbers:

```bash
ls -li
```

Include hidden files:

```bash
ls -lai
```

### 🖆 Using `readlink`

Display the target of a symbolic link:

```bash
readlink shortcut
```

Resolve a symbolic link to its canonical path:

```bash
readlink -f shortcut
```

### 🖆 Using `find`

Search for files and directories.

Find files by name:

```bash
find ~/Documents -name "notes.txt"
```

Find all Markdown files:

```bash
find . -type f -name "*.md"
```

Find directories:

```bash
find . -type d
```

Find files larger than 100 MB:

```bash
find . -type f -size +100M
```

Find files modified within the last seven days:

```bash
find . -type f -mtime -7
```

Preview matching files before performing any destructive operation.

# 🗁 File Metadata and Timestamps

Linux filesystems maintain several timestamps.

### 🖆 Common File Timestamps

| Timestamp | Meaning |
| --- | --- |
| atime | Last access time |
| mtime | Last content modification time |
| ctime | Last inode status change time |
| birth time | File creation time, when supported |

The `ctime` timestamp is not the file creation time. It changes when inode metadata changes, such as permissions or ownership.

Inspect timestamps:

```bash
stat notes.txt
```

### 🖆 Updating Timestamps with `touch`

Update access and modification timestamps:

```bash
touch notes.txt
```

Update only access time:

```bash
touch -a notes.txt
```

Update only modification time:

```bash
touch -m notes.txt
```

Set a specific timestamp:

```bash
touch -t 202611081200 notes.txt
```

The timestamp format is:

```text
[[CC]YY]MMDDhhmm[.ss]
```

# 🗁 File Permissions and Ownership

Linux uses permissions and ownership to control access to files and directories.

### 🖆 Understanding Permissions

Run:

```bash
ls -l
```

Example:

```text
-rwxr-xr-- 1 user developers 1200 Nov 08 10:00 script.sh
```

The permission string can be interpreted as:

```text
- rwx r-x r--
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── Owner
```

Permission types:

| Permission | Symbol | Numeric Value |
| --- | --- | --- |
| Read | `r` | 4 |
| Write | `w` | 2 |
| Execute | `x` | 1 |

### 🖆 Changing Permissions with `chmod`

Make a script executable:

```bash
chmod +x script.sh
```

Set permissions using numeric notation:

```bash
chmod 755 script.sh
```

Set permissions using symbolic notation:

```bash
chmod u+x script.sh
```

### 🖆 Changing Ownership

Change the owner:

```bash
sudo chown username file.txt
```

Change owner and group:

```bash
sudo chown username:groupname file.txt
```

Change group only:

```bash
sudo chgrp groupname file.txt
```

### 🖆 Understanding Directory Permissions

Directory permissions have specific meanings:

* `r`: List directory entries.
* `w`: Create, remove, or rename entries, subject to other restrictions.
* `x`: Search or traverse the directory.

Directory access often requires the execute permission, even when read permission is available.

# 🗁 Disk Usage

Linux provides commands for understanding storage consumption.

### 🖆 Using `du`

The `du` command reports disk usage for files and directories.

Check a directory:

```bash
du projects/
```

Display human-readable sizes:

```bash
du -h projects/
```

Display the total size of a directory:

```bash
du -sh projects/
```

Display sizes of immediate subdirectories:

```bash
du -h --max-depth=1 projects/
```

### 🖆 Using `df`

The `df` command reports available and used space on mounted filesystems.

```bash
df -h
```

Check the filesystem containing a particular path:

```bash
df -h ~/Documents
```

**Difference:**

* `du` reports space consumed by files and directories.
* `df` reports filesystem-level storage capacity and availability.

# 🗁 Archives and Compression

Linux provides tools for packaging and compressing files.

### 🖆 Creating a TAR Archive

Create an archive:

```bash
tar -cvf backup.tar projects/
```

Options:

| Option | Description |
| --- | --- |
| `-c` | Create archive |
| `-v` | Verbose output |
| `-f` | Specify archive filename |
| `-x` | Extract archive |
| `-t` | List archive contents |

List archive contents:

```bash
tar -tf backup.tar
```

Extract an archive:

```bash
tar -xvf backup.tar
```

### 🖆 Creating a Compressed TAR Archive

Create a gzip-compressed archive:

```bash
tar -czvf backup.tar.gz projects/
```

Extract it:

```bash
tar -xzvf backup.tar.gz
```

Create a bzip2-compressed archive:

```bash
tar -cjvf backup.tar.bz2 projects/
```

Extract it:

```bash
tar -xjvf backup.tar.bz2
```

### 🖆 Using `gzip` and `gunzip`

Compress a file:

```bash
gzip notes.txt
```

This normally creates `notes.txt.gz` and removes the original uncompressed file.

Decompress it:

```bash
gunzip notes.txt.gz
```

Keep the original file when compressing:

```bash
gzip -k notes.txt
```

### 🖆 Using ZIP Archives

Ubuntu may require installing the ZIP utilities:

```bash
sudo apt update
sudo apt install zip unzip
```

Create a ZIP archive:

```bash
zip backup.zip notes.txt
```

Compress a directory recursively:

```bash
zip -r project.zip projects/
```

Extract a ZIP archive:

```bash
unzip project.zip
```

List its contents without extracting:

```bash
unzip -l project.zip
```

# 🗁 Practical Workflows

### 🖆 Creating a Project Structure

Create a typical web development project structure:

```bash
mkdir -p my-app/{src,public,tests,docs,config}
```

Create initial files:

```bash
touch my-app/README.md
touch my-app/src/index.js
touch my-app/tests/index.test.js
touch my-app/.gitignore
```

Inspect the structure:

```bash
tree my-app/
```

If `tree` is not installed:

```bash
sudo apt install tree
```

### 🖆 Creating a Backup

Copy a project while preserving filesystem attributes:

```bash
cp -a my-app/ my-app-backup/
```

Alternatively, create a compressed archive:

```bash
tar -czvf my-app-backup.tar.gz my-app/
```

### 🖆 Organizing Downloaded Files

Create destination directories:

```bash
mkdir -p ~/Documents/{images,archives,documents}
```

Move files into their respective directories:

```bash
mv ~/Downloads/*.png ~/Documents/images/
mv ~/Downloads/*.zip ~/Documents/archives/
mv ~/Downloads/*.pdf ~/Documents/documents/
```

Check the destination before moving files if matching patterns may select more files than expected.

### 🖆 Finding and Reviewing Large Files

Find files larger than 100 MB:

```bash
find ~/Downloads -type f -size +100M
```

Review the directory's total size:

```bash
du -sh ~/Downloads
```

### 🖆 Safely Removing Temporary Files

First, list matching files:

```bash
find /tmp -type f -name "*.tmp" -print
```

After reviewing the results, remove matching files:

```bash
find /tmp -type f -name "*.tmp" -delete
```

Use the same search criteria for previewing and deletion. Be especially careful when changing the search path or matching pattern.

# 🗁 Best Practices

* Use meaningful filenames and directory names.
* Prefer relative paths when working inside a project and absolute paths when clarity is important.
* Use `mkdir -p` for creating nested directory structures.
* Use `cp -a` when preserving metadata and symbolic links is important.
* Use `-i` with `cp`, `mv`, and `rm` when interactive overwrite or deletion confirmation is useful.
* Inspect directory contents before recursive deletion.
* Use `stat` to investigate file metadata.
* Use `du -sh` to check directory sizes.
* Understand the difference between hard links and symbolic links.
* Avoid unnecessary use of `sudo` for ordinary file operations.
* Keep backups of important files before performing destructive operations.
* Verify paths before executing recursive commands.

# 🗁 Common Mistakes

| Mistake | Explanation | Better Approach |
| --- | --- | --- |
| Using `cp` on a directory without `-r` | Basic directory copying requires recursive mode | Use `cp -r` or `cp -a` |
| Using `rmdir` on a non-empty directory | `rmdir` removes empty directories only | Use it only for empty directories |
| Confusing `mv` with `cp` | `mv` moves or renames rather than creating an independent copy | Use `cp` when a separate copy is required |
| Assuming `touch` clears a file | `touch` updates timestamps but does not empty existing content | Use `truncate` or shell redirection when emptying a file is intended |
| Assuming `ctime` means creation time | `ctime` records inode status changes | Inspect filesystem metadata carefully |
| Deleting directories without inspection | Recursive deletion can remove important data | Preview contents and verify paths |
| Assuming symbolic links contain file data | Symbolic links store target paths | Use `readlink` to inspect their targets |
| Assuming file extensions determine file types | Extensions are naming conventions | Use `file` to inspect content |

# 🗁 Practice Exercises

### 🖆 Beginner Exercises

* Create a directory named `linux-practice`.
* Inside it, create `documents`, `images`, and `backups`.
* Create three text files inside `documents`.
* Rename one file.
* Copy another file into `backups`.
* List all files, including hidden files.

### 🖆 Intermediate Exercises

* Create a nested directory structure using `mkdir -p`.
* Copy a complete directory using `cp -a`.
* Move a directory to another location.
* Create a hard link and compare inode numbers.
* Create a symbolic link to a directory.
* Inspect file metadata using `stat`.
* Find all `.txt` files under a directory.

### 🖆 Advanced Exercises

* Find files larger than 50 MB inside your home directory.
* Create a compressed archive of a project directory.
* Extract the archive into a different directory.
* Compare directory sizes using `du`.
* Change file permissions using both symbolic and numeric notation.
* Find temporary files, preview the results, and remove only the intended matches.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is the difference between a file and a directory?**

A file stores data or represents a filesystem object. A directory organizes filesystem entries by associating names with their corresponding objects.

**Which command creates an empty file?**

```bash
touch file.txt
```

**Which command creates directories?**

```bash
mkdir directory-name
```

**What is the difference between `rm` and `rmdir`?**

`rm` removes files and can remove directories recursively with `-r`. `rmdir` removes empty directories only.

### 🖆 Intermediate Questions

**What is the difference between `cp -r` and `cp -a`?**

`cp -r` recursively copies directories. `cp -a` uses archive mode, preserving attributes and copying symbolic links as links where possible.

**What is an inode?**

An inode is a filesystem data structure containing metadata about a filesystem object and references to its data. Directory entries associate names with inode numbers.

**What is the difference between a hard link and a symbolic link?**

A hard link references the same inode as another filename. A symbolic link references a pathname and has its own inode.

**What is the difference between `du` and `df`?**

`du` reports disk usage by files and directories. `df` reports used and available space on mounted filesystems.

### 🖆 Advanced Questions

**Why can a symbolic link become dangling?**

Because it stores a pathname. If the target is moved or removed, the symbolic link may no longer resolve to an existing object.

**Why do hard links generally not work across filesystems?**

Hard links reference inode numbers within a filesystem. Inode numbers are meaningful within their own filesystem, so a hard link cannot directly reference an inode on another filesystem.

**Does deleting a filename always delete the underlying file data?**

No. Removing a directory entry decreases the inode's link count. The filesystem can reclaim the data when no hard links remain and no process is still holding the file open.

**Does `rm` securely erase a file?**

No. `rm` removes filesystem references; it does not guarantee that the underlying storage data is securely overwritten.

# 🗁 Official References

* [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/)
* [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/)
* [Linux man-pages Project](https://www.kernel.org/doc/man-pages/)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
