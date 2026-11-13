# Shell Pipes

Pipes are a fundamental feature of Ubuntu/Linux command-line environments. They allow the output of one command to become the input of another command.

Instead of saving intermediate results in temporary files, pipes connect commands directly and allow them to work together.

Pipes are especially useful for:

* Searching and filtering text.
* Processing log files.
* Sorting and counting data.
* Extracting information from command output.
* Automating system administration tasks.
* Building command-line data-processing workflows.
* Combining small utilities into powerful operations.

Bash provides the pipe operator `|` and the pipeline operator `|&`.

This chapter focuses on pipes and command pipelines in Bash.

# 🗁 Pipe Fundamentals

A pipe connects two or more commands so that data flows from one command to another.

In Linux, commands commonly read input from standard input (`stdin`) and write output to standard output (`stdout`).

A pipe connects the standard output of one command to the standard input of the next command.

### 🖆 Basic Concept

```text
Command A  ──stdout──>  Pipe  ──stdin──>  Command B
```

For example:

```bash
ls | sort
```

The `ls` command produces a list of directory entries. The `sort` command receives that output and sorts it alphabetically.

### 🖆 Why Pipes Are Useful

Without pipes, you might need intermediate files:

```bash
ls > files.txt
sort files.txt
```

With pipes:

```bash
ls | sort
```

Pipes simplify command composition and avoid unnecessary intermediate files.

# 🗁 The Pipe Operator

The pipe operator is represented by the vertical bar:

```bash
|
```

It connects the output of one command to the input of another.

### 🖆 Syntax

```bash
command1 | command2
```

The execution flow is:

* `command1` executes.
* Its standard output is sent through the pipe.
* `command2` receives that data through standard input.
* `command2` processes the received data.

### 🖆 Basic Example

```bash
printf '%s\n' "Ubuntu" "Debian" "Fedora" | sort
```

Output:

```text
Debian
Fedora
Ubuntu
```

### 🖆 Filtering Command Output

```bash
ls /etc | grep ssh
```

This displays entries in `/etc` whose names contain `ssh`.

### 🖆 Counting Command Output

```bash
ls /etc | wc -l
```

This counts the number of lines produced by `ls`.

The exact count depends on the contents of the directory.

# 🗁 How Pipes Work

Linux pipes use a kernel-managed communication channel.

When a pipeline executes, the shell connects the standard output file descriptor of one command to the standard input file descriptor of the next command.

### 🖆 File Descriptors

Linux processes commonly use three standard file descriptors:

| File Descriptor | Name | Purpose |
| --- | --- | --- |
| `0` | stdin | Standard input |
| `1` | stdout | Standard output |
| `2` | stderr | Standard error |

For a basic pipeline:

```bash
command1 | command2
```

The shell establishes a connection equivalent to:

```text
command1:
    stdout (1)
       |
       v
     PIPE
       |
       v
command2:
    stdin (0)
```

The output of `command1` becomes the input of `command2`.

### 🖆 Kernel Buffering

A pipe has a limited kernel-managed buffer.

* The producer writes data into the pipe.
* The consumer reads data from the pipe.
* If the buffer becomes full, the producer may block until space becomes available.
* If the buffer is empty, the consumer may wait for more data.

This allows commands to process data concurrently rather than requiring the entire output to be generated first.

### 🖆 Anonymous Pipes

The ordinary `|` operator creates an anonymous pipe.

An anonymous pipe:

* Exists as a communication channel between related processes.
* Is generally created and managed by the shell.
* Does not require a named filesystem entry.
* Is normally used for communication between processes in a pipeline.

# 🗁 Combining Multiple Commands

A pipeline can contain multiple commands connected through several pipe operators.

### 🖆 Basic Pipeline

```bash
command1 | command2 | command3
```

Data flows from left to right.

```text
Command 1
    |
    v
Command 2
    |
    v
Command 3
    |
    v
Final Output
```

### 🖆 Multiple Commands Example

```bash
printf '%s\n' "Ubuntu" "Debian" "Fedora" "Ubuntu" |
sort |
uniq
```

Output:

```text
Debian
Fedora
Ubuntu
```

Explanation:

| Command | Responsibility |
| --- | --- |
| `printf` | Produces the input data |
| `sort` | Sorts the values |
| `uniq` | Removes adjacent duplicate lines |

`uniq` removes only consecutive duplicate lines. Sorting first groups identical lines together.

### 🖆 Counting Duplicate Values

```bash
printf '%s\n' "Ubuntu" "Debian" "Ubuntu" "Fedora" "Ubuntu" |
sort |
uniq -c
```

Output:

```text
      1 Debian
      1 Fedora
      3 Ubuntu
```

### 🖆 Searching and Sorting

```bash
ls /etc |
grep '^ssh' |
sort
```

This pipeline:

* Lists entries in `/etc`.
* Selects names beginning with `ssh`.
* Sorts the resulting entries.

# 🗁 Common Linux Commands Used with Pipes

Linux provides many small utilities that work effectively together through pipelines.

### 🖆 grep

`grep` searches text for matching patterns.

```bash
cat /etc/passwd | grep "root"
```

A more efficient approach is to pass the file directly:

```bash
grep "root" /etc/passwd
```

Common options:

| Option | Purpose |
| --- | --- |
| `-i` | Ignore case |
| `-n` | Display line numbers |
| `-v` | Invert the match |
| `-r` | Search recursively |
| `-c` | Count matching lines |
| `-E` | Use extended regular expressions |

### 🖆 sort

`sort` sorts lines of text.

```bash
printf '%s\n' "banana" "apple" "orange" | sort
```

Common options:

| Option | Purpose |
| --- | --- |
| `-r` | Reverse order |
| `-n` | Numeric sorting |
| `-k` | Sort by a field |
| `-u` | Remove duplicate lines |
| `-h` | Human-readable numeric sorting |

### 🖆 uniq

`uniq` detects or removes adjacent duplicate lines.

```bash
printf '%s\n' "apple" "apple" "banana" |
uniq
```

Common options:

| Option | Purpose |
| --- | --- |
| `-c` | Count occurrences |
| `-d` | Display repeated lines |
| `-u` | Display unique lines |

### 🖆 wc

`wc` counts lines, words, and bytes.

```bash
cat /etc/passwd | wc -l
```

Common options:

| Option | Purpose |
| --- | --- |
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |

### 🖆 cut

`cut` extracts selected fields or character positions.

```bash
printf '%s\n' "Muhammad:Developer:Bangladesh" |
cut -d ':' -f 1
```

Output:

```text
Muhammad
```

Common options:

| Option | Purpose |
| --- | --- |
| `-d` | Specify delimiter |
| `-f` | Select fields |
| `-c` | Select character positions |
| `-b` | Select bytes |

### 🖆 awk

`awk` processes structured text and fields.

```bash
printf '%s\n' "Muhammad 25 Developer" |
awk '{print $1, $3}'
```

Output:

```text
Muhammad Developer
```

### 🖆 sed

`sed` performs stream editing.

```bash
printf '%s\n' "Hello World" |
sed 's/World/Ubuntu/'
```

Output:

```text
Hello Ubuntu
```

### 🖆 head and tail

`head` displays the beginning of input, while `tail` displays the end.

```bash
ls /etc | head -n 5
```

```bash
ls /etc | tail -n 5
```

# 🗁 The Pipe Operator and Standard Streams

By default, the `|` operator connects standard output (`stdout`) to the next command's standard input (`stdin`).

Standard error (`stderr`) is not included automatically.

### 🖆 Standard Output Only

```bash
find /etc -name "*.conf" | grep ssh
```

The standard output of `find` is sent to `grep`.

### 🖆 Redirecting Standard Error into a Pipeline

Use `2>&1` to redirect standard error to standard output.

```bash
find /etc -name "*.conf" 2>&1 | grep Permission
```

The redirection order matters.

The following:

```bash
command 2>&1 | grep error
```

sends both standard output and standard error through the pipe.

### 🖆 Using |&

Bash provides the `|&` operator.

It connects both standard output and standard error of the preceding command to the next command's standard input.

```bash
command |& grep error
```

In Bash, `|&` is shorthand for:

```bash
command 2>&1 | grep error
```

The redirection is applied after the preceding command's explicit redirections.

### 🖆 Comparing Pipe Behavior

| Syntax | Data forwarded |
| --- | --- |
| `command1 \| command2` | Standard output |
| `command1 2>&1 \| command2` | Standard output and standard error |
| `command1 \|& command2` | Standard output and standard error in Bash |

# 🗁 The Pipefail Option

A pipeline normally returns the exit status of its last command.

This can hide failures in earlier commands.

Bash provides the `pipefail` option to make pipeline failures easier to detect.

### 🖆 Default Pipeline Behavior

```bash
false | true

echo "$?"
```

Output:

```text
0
```

The first command fails, but the last command succeeds. Therefore, the pipeline returns status `0`.

### 🖆 Enabling pipefail

```bash
set -o pipefail

false | true

echo "$?"
```

Output:

```text
1
```

With `pipefail` enabled, a pipeline returns the status of the rightmost command that failed, or zero if all commands succeeded.

### 🖆 Using pipefail in Scripts

```bash
#!/usr/bin/env bash

set -o pipefail

if find /etc -type f | grep -q "ssh"; then
    echo "Matching files found."
else
    echo "No matches found or pipeline failed."
fi
```

For more precise error handling, distinguish between a legitimate no-match result and an actual command failure.

### 🖆 Common Strict Mode

Many Bash scripts begin with:

```bash
#!/usr/bin/env bash

set -euo pipefail
```

These options enable:

| Option | Behavior |
| --- | --- |
| `-e` | Exit on certain unhandled command failures |
| `-u` | Treat unset variables as errors in relevant expansions |
| `-o pipefail` | Detect failures within pipelines |

Strict mode is useful, but it does not eliminate the need for explicit error handling. Each option has exceptions and context-dependent behavior.

# 🗁 Pipeline Exit Status

A command's exit status indicates whether an operation succeeded.

In Bash, the special parameter `PIPESTATUS` stores the exit statuses of commands in the most recently executed foreground pipeline.

### 🖆 Checking Pipeline Status

```bash
false | true | false

echo "${PIPESTATUS[@]}"
```

Output:

```text
1 0 1
```

Each value corresponds to a command in the pipeline, from left to right.

### 🖆 Checking Individual Commands

Capture `PIPESTATUS` immediately after the pipeline because another command can overwrite it.

```bash
#!/usr/bin/env bash

false | true | false

statuses=("${PIPESTATUS[@]}")

for status in "${statuses[@]}"; do
    echo "Exit status: $status"
done
```

### 🖆 Using PIPESTATUS with pipefail

`PIPESTATUS` and `pipefail` serve related but different purposes:

| Feature | Purpose |
| --- | --- |
| `$?` | Exit status of the most recent command or pipeline |
| `PIPESTATUS` | Exit statuses of individual pipeline commands |
| `pipefail` | Changes how a pipeline's overall exit status is determined |

# 🗁 The tee Command

The `tee` command reads standard input and writes the received data to both standard output and one or more files.

It is useful when you want to inspect command output while saving it.

### 🖆 Basic Syntax

```bash
command | tee filename
```

### 🖆 Display and Save Output

```bash
ls /etc | tee files.txt
```

The output is:

* Displayed in the terminal.
* Written to `files.txt`.

### 🖆 Append Output

Use `-a` to append instead of replacing the destination file.

```bash
date | tee -a system.log
```

### 🖆 Multiple Output Files

```bash
date | tee date.txt backup-date.txt
```

### 🖆 Combining tee with Other Commands

```bash
df -h | tee disk-usage.txt | grep '/dev/'
```

This displays the filtered output while saving the complete `df -h` output to a file.

### 🖆 Logging Package Installation Output

```bash
sudo apt update 2>&1 | tee apt-update.log
```

This displays the output and records it in `apt-update.log`.

The pipeline's overall exit status normally reflects `tee` unless `pipefail` is enabled.

# 🗁 The xargs Command

The `xargs` command builds and executes commands using items received from standard input.

It is useful when one command produces a list of values that another command needs as arguments.

### 🖆 Basic Syntax

```bash
command1 | xargs command2
```

### 🖆 Basic Example

```bash
printf '%s\n' file1.txt file2.txt file3.txt |
xargs rm --
```

This passes the filenames as arguments to `rm`.

Only run destructive operations when the input list is trusted and the intended targets have been verified.

### 🖆 Display Arguments

```bash
printf '%s\n' Ubuntu Debian Fedora |
xargs echo "Distributions:"
```

Output:

```text
Distributions: Ubuntu Debian Fedora
```

### 🖆 Handling Filenames Safely

Ordinary `xargs` separates input using whitespace. This can break filenames containing spaces or newlines.

Use null-delimited input with `-0`.

```bash
find . -type f -name "*.tmp" -print0 |
xargs -0 -r rm --
```

Explanation:

| Option | Purpose |
| --- | --- |
| `-0` | Read null-delimited input |
| `-r` | Do not execute the command when input is empty |
| `--` | End command options before filenames |

### 🖆 Executing Commands for Each Item

```bash
printf '%s\n' Ubuntu Debian Fedora |
xargs -n 1 echo "Operating system:"
```

The `-n 1` option limits each command invocation to one input argument.

### 🖆 Running Commands in Parallel

```bash
printf '%s\n' file1.txt file2.txt file3.txt |
xargs -n 1 -P 2 wc -l
```

The `-P 2` option allows up to two command processes to run concurrently.

Parallel execution is useful for independent operations, but output order may vary.

# 🗁 Process Substitution and Pipes

Process substitution is a Bash feature that allows command output or input to be represented as a file-like path.

It is useful when commands need multiple input streams or when a pipeline would otherwise create subshell-related problems.

### 🖆 Basic Syntax

Output process substitution:

```bash
<(command)
```

Input process substitution:

```bash
>(command)
```

### 🖆 Comparing Two Command Outputs

```bash
diff <(sort file1.txt) <(sort file2.txt)
```

This compares the sorted contents of two files without creating temporary sorted files.

### 🖆 Reading Command Output in a Loop

```bash
count=0

while IFS= read -r line; do
    ((count++))
done < <(printf '%s\n' alpha beta gamma)

echo "Total: $count"
```

Output:

```text
Total: 3
```

The loop executes in the current shell, allowing the updated `count` variable to remain available afterward.

### 🖆 Pipe vs Process Substitution

| Feature | Pipe | Process Substitution |
| --- | --- | --- |
| Main purpose | Connect command output to input | Provide command input or output through a file-like path |
| Syntax | `command1 \| command2` | `<(command)` or `>(command)` |
| Typical use | Sequential command processing | Multiple inputs or specialized command connections |
| Bash-specific | Basic pipe is widely supported | Yes |

# 🗁 Practical Examples

### 🖆 Find a Running Process

```bash
ps aux | grep nginx
```

A better option for process-name searching is:

```bash
pgrep -a nginx
```

### 🖆 Display Top Memory-Consuming Processes

```bash
ps aux |
sort -nrk 4 |
head -n 10
```

This sorts processes by the memory percentage column and displays the first ten entries.

### 🖆 Display Top CPU-Consuming Processes

```bash
ps aux |
sort -nrk 3 |
head -n 10
```

### 🖆 Count Active System Services

```bash
systemctl list-units --type=service --state=running --no-legend |
wc -l
```

### 🖆 Search SSH Authentication Logs

On Ubuntu systems where SSH authentication logs are available through the system journal:

```bash
sudo journalctl -u ssh |
grep -i "accepted"
```

This displays log lines containing `accepted`, ignoring case.

### 🖆 Find Large Files

```bash
find "$HOME" -type f -printf '%s %p\n' |
sort -nr |
head -n 10
```

This finds files under the home directory and displays the ten largest entries by byte size.

### 🖆 Count Files by Extension

```bash
find . -type f |
awk -F. 'NF > 1 {count[$NF]++} END {
    for (extension in count) {
        print extension, count[extension]
    }
}'
```

This counts files according to their final filename extension.

The command assumes that the final dot-separated component represents the extension.

### 🖆 Find Duplicate Lines

```bash
sort users.txt |
uniq -d
```

This displays lines that appear more than once.

### 🖆 Extract IP Addresses from Network Interfaces

```bash
ip -brief address |
awk '{print $1, $3}'
```

This displays interface names and their assigned address fields.

### 🖆 Monitor Disk Usage

```bash
watch -n 5 'df -h | grep "^/dev/"'
```

This refreshes the filtered disk-usage output every five seconds.

### 🖆 Find Failed Systemd Services

```bash
systemctl --failed --no-legend |
awk '{print $1}'
```

This extracts the unit names from the failed-unit listing.

### 🖆 Search Installed Packages

```bash
dpkg-query -W -f='${binary:Package}\n' |
grep '^nginx'
```

This searches installed package names beginning with `nginx`.

### 🖆 Build a Simple Log Summary

```bash
#!/usr/bin/env bash

logfile="/var/log/syslog"

if [[ ! -r "$logfile" ]]; then
    echo "Cannot read log file."
    exit 1
fi

grep -i "error" "$logfile" |
sort |
uniq -c |
sort -nr |
head -n 10
```

This script:

* Searches for error-related lines.
* Groups identical lines.
* Counts repeated entries.
* Sorts by frequency.
* Displays the ten most frequent entries.

# 🗁 Best Practices

* Use pipes to connect commands that naturally consume and produce standard input and output.
* Prefer direct file arguments over unnecessary `cat` pipelines.
* Keep pipelines readable by placing each command on a separate line when they become long.
* Use `set -o pipefail` when pipeline failures must be detected.
* Use `PIPESTATUS` when individual command exit statuses matter.
* Use `tee` when output must be displayed and saved simultaneously.
* Use `find -print0` and `xargs -0` when processing arbitrary filenames.
* Use `"$@"` and properly quoted arguments in shell scripts.
* Understand the difference between standard output and standard error.
* Use `|&` when both output streams need to enter a pipeline in Bash.
* Avoid unnecessary subprocesses and temporary files.
* Prefer dedicated commands such as `pgrep` over fragile text searches when available.
* Validate inputs before passing them to destructive commands.
* Avoid parsing human-oriented command output when a structured or machine-readable option is available.

# 🗁 Common Mistakes

### 🖆 Assuming Pipes Forward Standard Error

Incorrect assumption:

```bash
find /root -type f | grep error
```

Permission errors are written to standard error and do not normally enter the pipe.

To include standard error:

```bash
find /root -type f 2>&1 | grep error
```

### 🖆 Using cat Unnecessarily

Unnecessary:

```bash
cat file.txt | grep "Ubuntu"
```

Better:

```bash
grep "Ubuntu" file.txt
```

This avoids an unnecessary process.

### 🖆 Ignoring Pipeline Failures

```bash
false | true
```

Without `pipefail`, the pipeline succeeds because the final command succeeds.

For failure-sensitive scripts:

```bash
set -o pipefail
```

### 🖆 Incorrect Filename Handling with xargs

Unsafe for arbitrary filenames:

```bash
find . -type f | xargs rm
```

Safer:

```bash
find . -type f -print0 | xargs -0 -r rm --
```

### 🖆 Misunderstanding uniq

Incorrect:

```bash
printf '%s\n' apple banana apple | uniq
```

`uniq` only detects adjacent duplicate lines.

Correct:

```bash
printf '%s\n' apple banana apple |
sort |
uniq
```

### 🖆 Breaking a Pipeline Across Lines Incorrectly

Incorrect:

```bash
ls
| grep txt
```

Correct:

```bash
ls |
grep txt
```

Or:

```bash
ls | \
grep txt
```

The pipe must remain connected to the preceding command.

# 🗁 Practical Exercises

### 🖆 Exercise: Sort Files

Display the entries in `/etc` alphabetically using a pipeline.

### 🖆 Exercise: Count Files

Count the number of regular files in the current directory using `find`, `wc`, and a pipe.

### 🖆 Exercise: Search Logs

Search `/var/log` for lines containing the word `error`, ignoring case.

### 🖆 Exercise: Find Duplicate Entries

Create a text file containing repeated names and use `sort` and `uniq` to identify duplicates.

### 🖆 Exercise: Process Information

Display the ten processes consuming the most memory.

### 🖆 Exercise: Disk Usage

Display filesystem usage and filter the results to show only device-backed filesystems.

### 🖆 Exercise: Save and Display Output

Use `tee` to display a directory listing while saving it to a file.

### 🖆 Exercise: Safe File Processing

Use `find -print0` and `xargs -0` to locate all `.tmp` files without breaking on unusual filenames. First display the files rather than deleting them.

### 🖆 Exercise: Pipeline Error Handling

Create a pipeline containing one failing command and one successful command. Compare its exit status before and after enabling `set -o pipefail`.

### 🖆 Exercise: Log Analysis

Build a pipeline that extracts error messages from a log file, counts repeated lines, sorts them by frequency, and displays the top five results.

# 🗁 Interview Questions

### 🖆 Fundamentals

* What is a pipe in Linux?
* What does the `|` operator do?
* How does a pipe connect two processes?
* What is the difference between standard input and standard output?
* What is an anonymous pipe?
* Can multiple commands be connected using pipes?

### 🖆 Linux Command-Line Usage

* What is the purpose of `grep`?
* What is the difference between `sort` and `uniq`?
* Why is `sort` often used before `uniq`?
* What does `wc -l` do?
* How does `awk` work with piped input?
* What is the purpose of `tee`?
* What is the purpose of `xargs`?

### 🖆 Advanced Concepts

* What is the difference between `|` and `|&`?
* Does a pipe transfer standard error automatically?
* What is `pipefail`?
* How does `PIPESTATUS` work?
* What is the difference between a pipe and process substitution?
* Why can pipeline loops execute in subshells?
* How can you process filenames containing spaces and newlines safely?
* What happens when a pipe buffer becomes full?
* How can a pipeline's exit status hide an earlier command failure?

# 🗁 References

* [GNU Bash Manual — Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)
* [GNU Bash Manual — Process Substitution](https://www.gnu.org/software/bash/manual/html_node/Process-Substitution.html)
* [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/coreutils.html)
* [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/)
* [GNU Grep Manual](https://www.gnu.org/software/grep/manual/grep.html)
* [GNU Awk Manual](https://www.gnu.org/software/gawk/manual/gawk.html)
* [Ubuntu Documentation](https://help.ubuntu.com/)
