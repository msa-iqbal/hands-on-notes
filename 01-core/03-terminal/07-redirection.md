# Redirection in Linux

Redirection is a fundamental feature of the Linux shell that controls where commands receive input and send output.

By default, commands normally read input from the terminal and display their output in the terminal. Redirection allows you to change these destinations.

You can use redirection to:

* Save command output to files.
* Append output to existing files.
* Read input from files.
* Separate normal output from error messages.
* Discard unwanted output.
* Combine multiple input and output streams.
* Build automated workflows in Bash scripts.

This guide explains the redirection operators available in Ubuntu/Linux, with practical examples using Bash.

# 🗁 Understanding Redirection

Redirection changes the default input or output destination of a command.

Bash provides several operators for controlling standard input, standard output, and standard error.

For example:

```bash
ls > files.txt
```

Instead of displaying the directory listing in the terminal, Bash writes the command's standard output into `files.txt`.

The general syntax is:

```bash
command [redirection-operator] destination
```

Redirection is handled by the shell before the command executes.

# 🗁 Standard File Descriptors

Linux processes normally start with three standard file descriptors.

| File Descriptor | Name | Default Destination |
| --- | --- | --- |
| `0` | Standard Input (stdin) | Terminal input |
| `1` | Standard Output (stdout) | Terminal output |
| `2` | Standard Error (stderr) | Terminal error output |

These descriptors can be redirected independently or combined.

# 🗁 Output Redirection

The `>` operator redirects standard output to a file.

### 🖆 Basic Output Redirection

```bash
echo "Hello Linux" > output.txt
```

This creates `output.txt` if it does not exist and writes the text into it.

### 🖆 Redirecting Command Output

```bash
ls -l > files.txt
```

Save the current date:

```bash
date > current-date.txt
```

Save system information:

```bash
uname -a > system-info.txt
```

### 🖆 Overwriting Existing Files

The `>` operator normally truncates an existing regular file before writing new output.

Example:

```bash
echo "New content" > notes.txt
```

If `notes.txt` already contains data, its previous contents are replaced.

Be careful when redirecting output to important files.

### 🖆 Creating an Empty File

You can use output redirection without a command:

```bash
> empty.txt
```

If the file exists, it is truncated to zero bytes.

# 🗁 Appending Output

The `>>` operator appends standard output to a file without replacing its existing contents.

### 🖆 Basic Append Operation

```bash
echo "First line" > notes.txt
echo "Second line" >> notes.txt
```

The resulting file contains:

```text
First line
Second line
```

### 🖆 Appending Command Output

Append the current date:

```bash
date >> activity.log
```

Append system information:

```bash
uname -a >> system-info.txt
```

### 🖆 Difference Between `>` and `>>`

| Operator | Behavior |
| --- | --- |
| `>` | Creates or truncates the destination before writing |
| `>>` | Creates the destination if needed and appends output |

Use `>` when replacing file contents is intended and `>>` when preserving existing contents is important.

# 🗁 Input Redirection

The `<` operator redirects a file into a command's standard input.

### 🖆 Basic Input Redirection

```bash
sort < names.txt
```

Bash opens `names.txt` and connects it to the command's standard input.

The `sort` command reads the input and displays the sorted result.

### 🖆 Counting Lines

```bash
wc -l < notes.txt
```

This passes the contents of `notes.txt` through standard input.

Unlike `wc -l notes.txt`, this form normally prints only the count, without the filename.

### 🖆 Comparing Input Methods

Read a file using a filename argument:

```bash
sort names.txt
```

Read the same file through standard input:

```bash
sort < names.txt
```

Both sort the file contents, but the shell handles the input differently.

# 🗁 Error Redirection

Standard error uses file descriptor `2`.

The `2>` operator redirects standard error to a file.

### 🖆 Basic Error Redirection

```bash
cat missing.txt 2> errors.txt
```

If the file does not exist, the error message is written to `errors.txt`.

### 🖆 Saving Errors Separately

```bash
find /etc /invalid-directory -name "*.conf" 2> errors.txt
```

Normal search results remain in the terminal, while error messages are saved in `errors.txt`.

### 🖆 Appending Error Messages

Use `2>>` to append errors:

```bash
find /etc /invalid-directory -name "*.conf" 2>> errors.txt
```

Existing error records are preserved.

### 🖆 Redirecting Only Standard Error

```bash
command 2> errors.log
```

This changes the destination of stderr without changing stdout.

# 🗁 Redirecting Both Output Streams

Sometimes you need to save normal output and error messages together.

Bash supports several ways to do this.

### 🖆 Redirecting Both Streams to One File

```bash
command > output.log 2>&1
```

Explanation:

* `> output.log` redirects stdout to the file.
* `2>&1` redirects stderr to the current destination of stdout.

Both streams are written to `output.log`.

### 🖆 Using the `&>` Operator

Bash provides a shorter form:

```bash
command &> output.log
```

This redirects both stdout and stderr to the same destination.

The append form is:

```bash
command &>> output.log
```

### 🖆 Comparing Common Operators

| Operator | Purpose |
| --- | --- |
| `> file` | Redirect stdout |
| `>> file` | Append stdout |
| `< file` | Redirect stdin |
| `2> file` | Redirect stderr |
| `2>> file` | Append stderr |
| `> file 2>&1` | Redirect stdout and stderr to one file |
| `&> file` | Bash shorthand for redirecting both streams |
| `&>> file` | Append both streams in Bash |

# 🗁 Discarding Output with `/dev/null`

`/dev/null` is a special Linux device that discards data written to it.

It is commonly called the null device.

### 🖆 Discarding Standard Output

```bash
command > /dev/null
```

Normal output is discarded.

Example:

```bash
ls -la > /dev/null
```

### 🖆 Discarding Standard Error

```bash
command 2> /dev/null
```

Error messages are discarded.

Example:

```bash
cat missing.txt 2> /dev/null
```

### 🖆 Discarding Both Streams

```bash
command > /dev/null 2>&1
```

Or, in Bash:

```bash
command &> /dev/null
```

This is useful when command output is intentionally unnecessary.

### 🖆 Keeping Errors While Discarding Normal Output

```bash
command > /dev/null
```

Only stdout is discarded. Error messages remain visible.

This is useful when you want a quiet command but still need to see failures.

# 🗁 Here Documents

A here document provides multiple lines of input directly to a command.

The Bash syntax uses `<<` followed by a delimiter.

### 🖆 Basic Here Document

```bash
cat << EOF
Hello Linux
Learning Ubuntu
Working with Bash
EOF
```

Bash passes the text between the delimiters to the command's standard input.

### 🖆 Writing Multiple Lines to a File

```bash
cat << EOF > notes.txt
Linux is an operating system.
Ubuntu is a Linux distribution.
Bash is a command-line shell.
EOF
```

The output is saved in `notes.txt`.

### 🖆 Variable Expansion in Here Documents

An unquoted delimiter allows parameter expansion and command substitution.

```bash
name="Ubuntu"

cat << EOF
Welcome to $name
Current directory: $(pwd)
EOF
```

Bash expands `$name` and executes `pwd` before passing the resulting text to `cat`.

### 🖆 Preventing Variable Expansion

Quote the delimiter:

```bash
name="Ubuntu"

cat << 'EOF'
Welcome to $name
Current directory: $(pwd)
EOF
```

The variables and command substitutions remain literal text.

### 🖆 Using `<<-` to Remove Leading Tabs

The `<<-` operator removes leading tab characters from the document's lines.

```bash
cat <<- EOF
 Hello Linux
 Learning Bash
EOF
```

It removes leading tabs, not ordinary spaces.

# 🗁 Here Strings

A here string provides a single string as standard input to a command.

Bash uses the `<<<` operator.

### 🖆 Basic Here String

```bash
grep "Linux" <<< "Learning Linux terminal commands"
```

The string is passed to `grep` as input.

### 🖆 Using Variables

```bash
name="Ubuntu"

tr '[:lower:]' '[:upper:]' <<< "$name"
```

Output:

```text
UBUNTU
```

### 🖆 Comparing Here Documents and Here Strings

| Feature | Here Document `<<` | Here String `<<<` |
| --- | --- | --- |
| Input | Multiple lines | One string |
| Syntax | `command << EOF` | `command <<< "text"` |
| Common use | Multiline input | Simple string input |
| Shell support | Bash and other shells | Bash and some other shells |

# 🗁 File Descriptor Duplication

Bash allows you to duplicate file descriptors so multiple streams can share a destination.

### 🖆 Understanding `2>&1`

```bash
command > output.log 2>&1
```

This means:

* Redirect stdout to `output.log`.
* Make stderr use the same destination currently assigned to stdout.

The order matters because `2>&1` duplicates the current stdout destination.

### 🖆 Redirecting Both Streams to the Terminal

```bash
command > /dev/tty 2>&1
```

When a controlling terminal is available, this directs both streams to that terminal.

### 🖆 Using a Custom File Descriptor

Bash allows custom file descriptors.

Open a file for writing using descriptor `3`:

```bash
exec 3> output.txt
```

Write to descriptor `3`:

```bash
echo "Hello from file descriptor 3" >&3
```

Close the descriptor:

```bash
exec 3>&-
```

Inspect the result:

```bash
cat output.txt
```

Custom descriptors are useful in more advanced shell scripts.

# 🗁 Redirection Order

Redirection operators are processed from left to right.

This is particularly important when redirecting stdout and stderr.

### 🖆 Correct Order: Both Streams to One File

```bash
command > output.log 2>&1
```

Execution:

* stdout is redirected to `output.log`.
* stderr is redirected to the current stdout destination, which is now `output.log`.

Both streams go to the file.

### 🖆 Different Order: Only Stderr Goes to the File

```bash
command 2>&1 > output.log
```

Execution:

* stderr is redirected to the current stdout destination, which is still the terminal.
* stdout is then redirected to `output.log`.

Result:

* stdout goes to `output.log`.
* stderr remains connected to the terminal.

### 🖆 Understanding the Difference

| Command | stdout | stderr |
| --- | --- | --- |
| `command > output.log 2>&1` | File | File |
| `command 2>&1 > output.log` | File | Original stdout destination |
| `command > output.log 2> errors.log` | Output file | Error file |
| `command &> output.log` | Output file | Output file |

Understanding redirection order prevents unexpected output destinations.

# 🗁 Practical Workflows

### 🖆 Saving System Information

```bash
{
    echo "System Information"
    date
    uname -a
    whoami
    pwd
} > system-report.txt
```

The braces group multiple commands. Their combined standard output is redirected to one file.

### 🖆 Creating a Command Log

```bash
{
    echo "Command started"
    date
    ls -la
    echo "Command completed"
} >> activity.log
```

The output is appended to `activity.log`.

### 🖆 Separating Application Output and Errors

```bash
application-command > application.log 2> application-errors.log
```

Normal output and errors are stored separately.

### 🖆 Creating a Configuration File

```bash
cat << 'EOF' > app.conf
APP_NAME=MyApplication
APP_ENV=development
APP_DEBUG=false
EOF
```

The quoted delimiter prevents shell expansion inside the document.

### 🖆 Suppressing Unwanted Output

```bash
sudo apt update > /dev/null
```

This discards stdout but leaves stderr visible.

To discard both streams:

```bash
sudo apt update > /dev/null 2>&1
```

### 🖆 Collecting Diagnostic Information

```bash
{
    echo "===== Date ====="
    date

    echo "===== Hostname ====="
    hostname

    echo "===== Disk Usage ====="
    df -h

    echo "===== Memory Usage ====="
    free -h
} > diagnostics.txt 2>&1
```

Both normal output and errors from the grouped commands are directed into `diagnostics.txt`.

# 🗁 Best Practices

* Understand the difference between stdin, stdout, and stderr.
* Use `>` only when replacing existing file contents is intended.
* Use `>>` when appending information.
* Separate stdout and stderr when troubleshooting applications.
* Use `2>&1` when both streams must share a destination.
* Pay attention to redirection order.
* Use `/dev/null` only when output is intentionally unnecessary.
* Use quoted here-document delimiters when shell expansion is not wanted.
* Use here documents for readable multiline input in scripts.
* Use meaningful filenames for logs and reports.
* Prefer explicit redirection in scripts so output destinations are clear.
* Avoid hiding error messages when they are important for diagnosing failures.

# 🗁 Common Mistakes

| Mistake | Explanation | Better Approach |
| --- | --- | --- |
| Using `>` instead of `>>` | Existing contents are overwritten | Use `>>` to append |
| Using `2>` to redirect normal output | `2>` redirects stderr only | Use `>` for stdout |
| Assuming `>` redirects errors too | It normally redirects stdout only | Use `2>&1` or `&>` |
| Incorrect `2>&1` ordering | Descriptor duplication uses the current stdout destination | Use `> file 2>&1` when both streams should go to one file |
| Forgetting that `<<` needs a delimiter | The shell waits for the terminating delimiter | Use the same delimiter to close the document |
| Expecting quoted here documents to expand variables | Quoting the delimiter disables expansion | Use an unquoted delimiter when expansion is required |
| Sending everything to `/dev/null` | Important diagnostic information may be lost | Suppress only the stream you do not need |
| Redirecting to a protected location without permissions | The shell may fail to open the destination file | Use an appropriate privileged editing or writing method |

# 🗁 Practice Exercises

### 🖆 Beginner Exercises

* Save the output of `date` to `date.txt`.
* Append another timestamp to the same file.
* Save the output of `ls -la` to `files.txt`.
* Read a text file using input redirection.
* Redirect an error message into `errors.txt`.
* Discard normal output using `/dev/null`.

### 🖆 Intermediate Exercises

* Redirect stdout and stderr into separate files.
* Redirect both stdout and stderr into one file.
* Create a multiline text file using a here document.
* Use a quoted here-document delimiter to prevent variable expansion.
* Use a here string with `grep`.
* Create a system information report using grouped commands.

### 🖆 Advanced Exercises

* Create a Bash script that records execution output and errors separately.
* Use a custom file descriptor to write diagnostic messages.
* Demonstrate the difference between `command > file 2>&1` and `command 2>&1 > file`.
* Create a configuration file using a here document.
* Build a diagnostic report that includes date, hostname, disk usage, and memory information.
* Create a script that appends execution timestamps to a log file.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is redirection in Linux?**

Redirection changes where a command receives input or sends output.

**What does the `>` operator do?**

It redirects standard output to a file, creating the file if necessary and normally truncating it if it already exists.

**What is the difference between `>` and `>>`?**

`>` replaces existing file contents, while `>>` appends output to the end of a file.

**What does the `<` operator do?**

It connects a file to a command's standard input.

### 🖆 Intermediate Questions

**What does `2>` mean?**

It redirects standard error, which uses file descriptor `2`.

**What does `2>&1` mean?**

It duplicates the current standard output destination for standard error.

**What is `/dev/null`?**

It is a special device that discards data written to it.

**What is a here document?**

A here document provides multiline input to a command through a shell redirection operator.

**What is a here string?**

A here string provides a single string as standard input using Bash's `<<<` operator.

### 🖆 Advanced Questions

**Why does redirection order matter?**

Because Bash processes redirections from left to right. File descriptor duplication uses the destination assigned at that point.

**What is the difference between `command > file 2>&1` and `command 2>&1 > file`?**

In the first form, both stdout and stderr go to the file. In the second, stderr retains the original stdout destination while stdout goes to the file.

**What is the purpose of a custom file descriptor?**

It provides an additional independent I/O channel that a shell script can use for logging, input, or output.

**Does redirection happen before a command executes?**

Yes. The shell processes redirections and prepares the required file descriptors before executing the command. If the shell cannot establish a required redirection, the command may not run.

# 🗁 Official References

* [GNU Bash Manual — Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
* [GNU Bash Manual — Here Documents](https://www.gnu.org/software/bash/manual/html_node/Here-Documents.html)
* [GNU Bash Manual — Here Strings](https://www.gnu.org/software/bash/manual/html_node/Here-Strings.html)
* [GNU Bash Manual — Duplicating File Descriptors](https://www.gnu.org/software/bash/manual/html_node/Moving-File-Descriptors.html)
* [Linux man-pages Project](https://www.kernel.org/doc/man-pages/)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
