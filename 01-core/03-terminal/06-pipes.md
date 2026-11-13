# Pipes in Linux

Pipes are one of the most powerful features of the Linux command-line environment.

A pipe connects the standard output of one command to the standard input of another command. This allows multiple commands to work together, process data sequentially, and build complex operations from simple utilities.

Pipes are widely used in Linux administration, software development, log analysis, system monitoring, and shell scripting.

This guide covers Linux pipes, Bash pipeline behavior, practical examples, and common command-line workflows in Ubuntu.

# 🗁 Understanding Pipes

A pipe transfers data from one command to another without requiring an intermediate file.

The pipe operator is:

```bash
|
```

For example:

```bash
ls -l | less
```

This command:

* Executes `ls -l`.
* Sends its standard output into a pipe.
* Passes the received data to `less`.
* Allows you to inspect the output interactively.

Without a pipe, you might need to save the output into a temporary file and then open that file separately.

Pipes make command composition simpler and more efficient.

### 🖆 Standard Pipe Flow

```text
Command A
   |
   | Standard Output (stdout)
   v
  Pipe
   |
   | Standard Input (stdin)
   v
Command B
```

The first command produces data, and the second command consumes it.

# 🗁 Basic Pipe Syntax

The basic syntax is:

```bash
command1 | command2
```

Example:

```bash
ls | wc -l
```

Explanation:

* `ls` lists directory entries.
* `|` sends its standard output to the next command.
* `wc -l` counts the lines it receives.

The result represents the number of lines in the `ls` output. It is not necessarily the exact number of filesystem entries in every situation.

### 🖆 Another Example

Display running processes and search for a specific application:

```bash
ps aux | grep firefox
```

Here:

* `ps aux` displays process information.
* `grep firefox` filters lines containing `firefox`.

# 🗁 How Pipes Work

Linux pipes connect processes through a kernel-managed communication channel.

When a pipeline runs:

* The shell creates a pipe.
* The shell starts the participating commands.
* The first command writes data to the pipe.
* The next command reads that data from the pipe.
* The receiving command processes the data and may send its output to another pipe or destination.

Pipes are generally unidirectional.

A regular pipeline connects stdout from the left command to stdin of the right command.

### 🖆 Process Communication

Consider:

```bash
cat names.txt | sort
```

The data flow is:

```text
names.txt
    |
    v
   cat
    |
    | stdout
    v
   Pipe
    |
    | stdin
    v
  sort
    |
    v
Sorted Output
```

The shell does not need to save the intermediate output to a temporary text file.

### 🖆 Anonymous Pipes

The ordinary `|` operator creates an anonymous pipe.

It is commonly used between commands in the same pipeline.

Example:

```bash
cat access.log | grep "404"
```

Anonymous pipes are temporary communication channels managed by the operating system.

# 🗁 Chaining Multiple Commands

You can connect more than two commands in one pipeline.

Syntax:

```bash
command1 | command2 | command3
```

The output of each command becomes the input of the next command.

### 🖆 Example: Sorting and Counting

```bash
cat names.txt | sort | wc -l
```

Execution flow:

* `cat` reads the file.
* `sort` sorts its lines.
* `wc -l` counts the resulting lines.

### 🖆 Example: Finding Unique Names

```bash
cat names.txt | sort | uniq
```

This sorts the input and removes adjacent duplicate lines.

`uniq` only removes consecutive duplicate lines, which is why sorting first is useful.

### 🖆 Example: Counting Duplicate Entries

```bash
cat names.txt | sort | uniq -c
```

Example output:

```text
3 Alex
2 John
1 Maria
```

Each line shows the number of occurrences followed by the corresponding value.

### 🖆 Example: Finding the Most Frequently Used Commands

```bash
history | awk '{print $2}' | sort | uniq -c | sort -nr | head
```

Explanation:

* `history` displays command history.
* `awk` extracts the command field.
* `sort` groups identical commands.
* `uniq -c` counts occurrences.
* `sort -nr` sorts counts numerically in descending order.
* `head` displays the first ten results.

The exact history format can vary depending on shell settings and command syntax.

# 🗁 Common Commands Used with Pipes

Several Linux utilities are particularly useful in pipelines.

### 🖆 `grep` — Filtering Text

Search for matching lines:

```bash
cat application.log | grep "ERROR"
```

Case-insensitive search:

```bash
cat application.log | grep -i "error"
```

Show matching line numbers:

```bash
cat application.log | grep -n "ERROR"
```

Count matching lines:

```bash
cat application.log | grep -c "ERROR"
```

You can avoid the unnecessary `cat` process by passing the filename directly:

```bash
grep "ERROR" application.log
```

### 🖆 `sort` — Sorting Data

Sort lines alphabetically:

```bash
cat names.txt | sort
```

Sort in reverse order:

```bash
cat names.txt | sort -r
```

Sort numerically:

```bash
cat numbers.txt | sort -n
```

Sort in descending numeric order:

```bash
cat numbers.txt | sort -nr
```

### 🖆 `uniq` — Removing Repeated Lines

Remove adjacent duplicate lines:

```bash
cat names.txt | sort | uniq
```

Count repeated lines:

```bash
cat names.txt | sort | uniq -c
```

Display only repeated lines:

```bash
cat names.txt | sort | uniq -d
```

### 🖆 `wc` — Counting Data

Count lines:

```bash
cat notes.txt | wc -l
```

Count words:

```bash
cat notes.txt | wc -w
```

Count bytes:

```bash
cat notes.txt | wc -c
```

Direct alternative:

```bash
wc -l < notes.txt
```

### 🖆 `head` — First Lines

```bash
cat application.log | head -n 10
```

Display the first ten lines.

### 🖆 `tail` — Last Lines

```bash
cat application.log | tail -n 10
```

Display the last ten lines.

### 🖆 `cut` — Extracting Fields

Extract the first field from colon-separated data:

```bash
cat /etc/passwd | cut -d ':' -f 1
```

Explanation:

* `-d ':'` specifies the delimiter.
* `-f 1` selects the first field.

A simpler equivalent is:

```bash
cut -d ':' -f 1 /etc/passwd
```

### 🖆 `awk` — Processing Structured Text

Extract the first column:

```bash
ps aux | awk '{print $1}'
```

Display the username and process ID:

```bash
ps aux | awk '{print $1, $2}'
```

### 🖆 `sed` — Transforming Text

Replace a word in incoming text:

```bash
echo "Hello Linux" | sed 's/Linux/Ubuntu/'
```

Output:

```text
Hello Ubuntu
```

# 🗁 Using `tee` with Pipes

The `tee` command reads data from standard input and writes it to both standard output and one or more files.

It is useful when you want to inspect pipeline output while also saving it.

### 🖆 Basic Usage

```bash
ls -l | tee files.txt
```

The output is:

* Displayed in the terminal.
* Saved in `files.txt`.

### 🖆 Appending Output

```bash
date | tee -a dates.txt
```

The `-a` option appends the output instead of overwriting the file.

### 🖆 Using `tee` in a Longer Pipeline

```bash
cat access.log | grep "404" | tee errors.txt | wc -l
```

Execution flow:

* Read the log file.
* Filter lines containing `404`.
* Save the matching lines to `errors.txt`.
* Count the matching lines.

### 🖆 Saving Intermediate Pipeline Results

```bash
cat access.log | grep "ERROR" | tee error-matches.txt | sort | uniq -c
```

This saves the filtered error lines before continuing with sorting and counting.

# 🗁 Pipes and Standard Error

By default, the ordinary pipe operator connects only standard output.

Standard error is not automatically passed to the next command.

Consider:

```bash
find /etc /invalid-directory -name "*.conf" | grep "network"
```

Normal output from `find` is passed to `grep`, but error messages from `find` are still displayed on the terminal.

### 🖆 Redirecting Standard Error into the Pipe

Use `2>&1`:

```bash
find /etc /invalid-directory -name "*.conf" 2>&1 | grep "Permission denied"
```

This redirects standard error to the current standard output, which is connected to the pipe.

Bash also provides a shorthand operator:

```bash
find /etc /invalid-directory -name "*.conf" |& grep "Permission denied"
```

In Bash, `|&` connects both standard output and standard error to the next command.

### 🖆 Understanding the Difference

| Operator | Behavior |
| --- | --- |
| ` | ` | Connects stdout to the next command's stdin |
| ` | &` | Connects stdout and stderr to the next command's stdin in Bash |
| `2>&1` | Redirects stderr to the current stdout |

# 🗁 Pipeline Exit Status

Every Linux command normally returns an exit status.

* `0` usually indicates success.
* A non-zero value usually indicates failure or another condition.

In Bash, the exit status of a pipeline is normally the exit status of its last command.

Example:

```bash
false | true
echo $?
```

Output:

```text
0
```

The first command fails, but the last command succeeds.

### 🖆 Understanding `PIPESTATUS`

Bash provides the `PIPESTATUS` array, which stores the exit status of each command in the most recently executed foreground pipeline.

Example:

```bash
false | true | false

echo "${PIPESTATUS[@]}"
```

Output:

```text
1 0 1
```

Each value corresponds to a command in the pipeline.

Capture the statuses immediately because subsequent commands can overwrite the array:

```bash
false | true | false
statuses=("${PIPESTATUS[@]}")

printf '%s\n' "${statuses[@]}"
```

### 🖆 Using `pipefail`

Enable `pipefail`:

```bash
set -o pipefail
```

With `pipefail`, the pipeline returns the status of the rightmost command that exits with a non-zero status. If every command succeeds, the pipeline returns zero.

Example:

```bash
set -o pipefail

false | true
echo $?
```

Output:

```text
1
```

Disable it:

```bash
set +o pipefail
```

For scripts that depend on successful pipeline execution, `pipefail` can help detect failures that would otherwise be hidden by a successful final command.

# 🗁 Named Pipes (FIFOs)

A named pipe is a filesystem object that allows processes to communicate through a pipe with a name.

Unlike an ordinary anonymous pipe, a named pipe can be accessed by separate processes through its filesystem pathname.

FIFO stands for First In, First Out.

### 🖆 Creating a Named Pipe

Use `mkfifo`:

```bash
mkfifo mypipe
```

Inspect it:

```bash
ls -l mypipe
```

The first character in its permissions listing is:

```text
p
```

This identifies it as a named pipe.

### 🖆 Writing to a Named Pipe

Open one terminal and run:

```bash
cat > mypipe
```

Enter some text:

```text
Hello from Terminal One
```

### 🖆 Reading from a Named Pipe

Open another terminal in the same directory and run:

```bash
cat < mypipe
```

The reader receives the data written by the other process.

A named pipe normally blocks a reader or writer until the other side is opened, depending on the way it is accessed.

### 🖆 Removing a Named Pipe

```bash
rm mypipe
```

Removing the FIFO pathname does not automatically terminate processes that already have it open.

# 🗁 Practical Workflows

### 🖆 Analyzing Application Logs

Find all error messages:

```bash
grep -i "error" application.log
```

Count error messages:

```bash
grep -ic "error" application.log
```

Display the most recent matching errors:

```bash
grep -i "error" application.log | tail -n 10
```

Save matching errors:

```bash
grep -i "error" application.log | tee errors.txt
```

### 🖆 Finding the Largest Files

Find files and sort them by size:

```bash
find ~/Downloads -type f -printf '%s %p\n' | sort -nr | head -n 10
```

The `-printf` option is provided by GNU `find`.

The command:

* Finds regular files.
* Prints each file's size in bytes and pathname.
* Sorts the results numerically in descending order.
* Displays the first ten entries.

### 🖆 Counting Unique IP Addresses in a Log

Extract the first whitespace-separated field from a log:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

This is useful when the first field contains the client IP address.

The exact field depends on the log format.

### 🖆 Monitoring Failed SSH Login Messages

Search authentication logs for failed login messages:

```bash
grep -i "failed" /var/log/auth.log | tail -n 20
```

On systems using systemd, you can also inspect the SSH service journal:

```bash
journalctl -u ssh | grep -i "failed"
```

### 🖆 Listing Installed Packages

Display installed packages and filter for a keyword:

```bash
dpkg -l | grep nginx
```

### 🖆 Checking Running Processes

Search running processes:

```bash
ps aux | grep nginx
```

A more targeted option is:

```bash
pgrep -a nginx
```

`pgrep` avoids matching the `grep` command itself.

# 🗁 Best Practices

* Use pipes to combine simple commands into reusable workflows.
* Understand that `|` normally connects stdout only.
* Use `|&` or `2>&1` when stderr must also enter a pipeline.
* Avoid unnecessary `cat` commands when a utility accepts a filename directly.
* Use `tee` when output must be displayed and saved simultaneously.
* Use `pipefail` in Bash scripts when intermediate command failures matter.
* Capture `PIPESTATUS` immediately when individual pipeline exit codes are required.
* Use `less` to inspect long pipeline output.
* Use `head` and `tail` to limit large result sets.
* Test complex pipelines with small input samples before using production data.
* Quote variables and filenames carefully in scripts.
* Remember that pipelines normally run commands in separate processes or subshell environments, depending on the shell and pipeline context.

# 🗁 Common Mistakes

| Mistake | Explanation | Better Approach |
| --- | --- | --- |
| Assuming ` | ` transfers stderr | Ordinary pipes connect stdout only | Use `2>&1` or Bash ` | &` |
| Assuming pipeline status represents every command | Bash normally reports the last command's status | Use `set -o pipefail` or `PIPESTATUS` |
| Using `uniq` without sorting | Non-adjacent duplicates are not removed | Use `sort \| uniq` |
| Using unnecessary `cat` commands | Some commands accept filenames directly | Use `grep pattern file.txt` |
| Forgetting pipeline order | Each command receives the previous command's output | Arrange commands according to the required data flow |
| Overwriting files with `tee` | `tee` overwrites by default | Use `tee -a` to append |
| Assuming named pipes behave like regular files | FIFO operations can block until a reader or writer connects | Coordinate both processes |
| Capturing `PIPESTATUS` too late | Subsequent commands can overwrite it | Save it immediately after the pipeline |

# 🗁 Practice Exercises

### 🖆 Beginner Exercises

* List files and count the output lines using `ls` and `wc`.
* Display the contents of a text file and sort them alphabetically.
* Find a specific word in a text file using `grep`.
* Display the first ten lines of a log file.
* Display the last five lines of a file.
* Remove duplicate lines from a text file using `sort` and `uniq`.

### 🖆 Intermediate Exercises

* Count how many times each word appears in a text file.
* Extract usernames from `/etc/passwd`.
* Find the ten most frequently occurring entries in a log file.
* Use `tee` to display and save command output.
* Search for error messages and save them to a separate file.
* Find the ten largest regular files in a directory.
* Redirect both stdout and stderr into another command.

### 🖆 Advanced Exercises

* Build a pipeline that extracts IP addresses from a web server log and counts their occurrences.
* Create a named pipe and use two separate terminal sessions to exchange data.
* Write a Bash script that checks pipeline failures using `pipefail`.
* Capture the exit status of every command in a pipeline using `PIPESTATUS`.
* Build a log-analysis pipeline that filters, sorts, counts, and saves results.
* Create a pipeline that separates successful output from diagnostic messages.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is a pipe in Linux?**

A pipe is a communication mechanism that connects the standard output of one command to the standard input of another command.

**Which symbol represents a pipe?**

```bash
|
```

**What is the purpose of a pipe?**

It allows commands to exchange data directly, making it possible to build complex workflows from smaller utilities.

**What does this command do?**

```bash
ls | wc -l
```

It sends the output of `ls` to `wc -l`, which counts the resulting lines.

### 🖆 Intermediate Questions

**What is the difference between an anonymous pipe and a named pipe?**

An anonymous pipe is generally created for communication between related processes and is accessed through file descriptors. A named pipe is a filesystem object that separate processes can access through its pathname.

**Does a normal pipe transfer standard error?**

No. The ordinary `|` operator connects only standard output to the next command's standard input.

**What is the purpose of `tee`?**

`tee` duplicates incoming data to standard output and one or more files.

**Why is `sort` often used before `uniq`?**

Because `uniq` only detects adjacent repeated lines. Sorting groups identical lines together.

### 🖆 Advanced Questions

**What is a FIFO?**

FIFO stands for First In, First Out. In Linux, a FIFO is a named pipe used for inter-process communication.

**What is `pipefail` in Bash?**

It is a shell option that makes a pipeline return a non-zero status if a command in the pipeline fails. The status comes from the rightmost failing command.

**What is `PIPESTATUS`?**

It is a Bash array containing the exit statuses of the commands in the most recently executed foreground pipeline.

**Why can a named pipe block a process?**

A FIFO normally requires a reader and writer to connect. Opening it for reading or writing can block until the corresponding endpoint is opened.

**Are pipes unidirectional or bidirectional?**

Ordinary Linux pipes are unidirectional. Bidirectional communication generally requires two pipes or another IPC mechanism such as a Unix domain socket.

# 🗁 Official References

* [GNU Bash Manual — Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)
* [GNU Bash Manual — The Set Builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html)
* [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/)
* [Linux man-pages — pipe(7)](https://man7.org/linux/man-pages/man7/pipe.7.html)
* [Linux man-pages — fifo(7)](https://man7.org/linux/man-pages/man7/fifo.7.html)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
