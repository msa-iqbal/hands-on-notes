# Shell Redirection in Linux

Shell redirection allows you to control where commands receive input and where they send output. Instead of displaying everything in the terminal, you can redirect output to files, read input from files, suppress errors, combine output streams, and manage file descriptors.

In Ubuntu/Linux, Bash provides several redirection operators that make command-line workflows and shell scripts more powerful.

# 🗁 Standard Streams

Every Linux process normally starts with three standard communication streams.

| Stream | File Descriptor | Purpose |
| --- | ---: | --- |
| Standard Input (`stdin`) | `0` | Receives input |
| Standard Output (`stdout`) | `1` | Displays normal output |
| Standard Error (`stderr`) | `2` | Displays error messages |

By default:

* `stdin` reads from the keyboard.
* `stdout` displays output in the terminal.
* `stderr` displays error messages in the terminal.

### 🖆 Standard Input

Standard input provides data to a command.

```bash
cat
```

The command waits for keyboard input. Type some text and press `Ctrl + D` to finish.

### 🖆 Standard Output

Standard output carries normal command results.

```bash
ls -l
```

The directory listing is written to standard output.

### 🖆 Standard Error

Standard error carries diagnostic and error messages.

```bash
ls /directory-that-does-not-exist
```

The error message is written to standard error.

# 🗁 File Descriptors

A file descriptor is a non-negative integer that identifies an open file or I/O resource associated with a process.

Linux uses file descriptors to manage communication between processes and resources.

### 🖆 Standard File Descriptors

```text
0  -> Standard Input
1  -> Standard Output
2  -> Standard Error
```

You can inspect a process's open file descriptors through the `/proc` filesystem.

```bash
ls -l /proc/self/fd
```

You can also inspect the current shell's file descriptors:

```bash
ls -l /proc/$$/fd
```

Here, `$$` represents the current shell's process ID.

### 🖆 File Descriptor Redirection Syntax

General syntax:

```bash
command [n]operator target
```

Where:

* `command` is the command to execute.
* `n` is an optional file descriptor number.
* `operator` specifies the redirection operation.
* `target` identifies a file or another file descriptor.

Examples:

```bash
echo "Hello" > output.txt
```

```bash
ls /missing-directory 2> errors.log
```

# 🗁 Output Redirection

Output redirection sends standard output to a file instead of displaying it in the terminal.

### 🖆 Redirect Output with `>`

The `>` operator creates a file if it does not exist or overwrites it if it already exists.

```bash
echo "Hello Linux" > output.txt
```

Read the file:

```bash
cat output.txt
```

Output:

```text
Hello Linux
```

Another example:

```bash
ls -l > directory-list.txt
```

The directory listing is saved to `directory-list.txt`.

### 🖆 Append Output with `>>`

The `>>` operator appends output to the end of a file without removing existing content.

```bash
echo "First line" > output.txt
echo "Second line" >> output.txt
echo "Third line" >> output.txt
```

Read the result:

```bash
cat output.txt
```

Output:

```text
First line
Second line
Third line
```

### 🖆 Explicit Standard Output Redirection

You can explicitly specify file descriptor `1`.

```bash
echo "Hello Linux" 1> output.txt
```

This is equivalent to:

```bash
echo "Hello Linux" > output.txt
```

# 🗁 Input Redirection

Input redirection provides data to a command from a file instead of the keyboard.

### 🖆 Redirect Input with `<`

The `<` operator redirects a file into standard input.

```bash
cat < input.txt
```

This is equivalent in displayed output to:

```bash
cat input.txt
```

However, the two commands use different input mechanisms:

* `cat input.txt` passes the filename as a command-line argument.
* `cat < input.txt` connects the file to standard input.

### 🖆 Using Input Redirection with Other Commands

Count the number of lines:

```bash
wc -l < input.txt
```

Search for a word:

```bash
grep "Linux" < input.txt
```

Sort file contents:

```bash
sort < input.txt
```

Read a list of values:

```bash
while IFS= read -r line; do
    echo "Value: $line"
done < input.txt
```

The loop reads each line from `input.txt`.

# 🗁 Redirecting Standard Error

Standard error uses file descriptor `2`. You can redirect it independently from standard output.

### 🖆 Redirect Errors with `2>`

```bash
ls /missing-directory 2> errors.log
```

The error message is saved to `errors.log`.

Normal output remains connected to the terminal.

### 🖆 Append Errors with `2>>`

```bash
ls /missing-directory 2>> errors.log
```

This appends new error messages to the existing file.

### 🖆 Suppress Error Messages

Linux provides `/dev/null`, a special device that discards everything written to it.

```bash
ls /missing-directory 2> /dev/null
```

The command still executes, but its error message is discarded.

### 🖆 Suppress Standard Output

```bash
command > /dev/null
```

This discards normal output while leaving standard error visible.

Example:

```bash
find /etc -name "*.conf" > /dev/null
```

### 🖆 Suppress Both Standard Output and Error

```bash
command > /dev/null 2>&1
```

This sends standard output to `/dev/null` and then redirects standard error to the same destination.

# 🗁 Redirecting Standard Output and Error

Sometimes you need to capture both normal output and error messages in the same file.

### 🖆 Redirect Both Streams with `> file 2>&1`

```bash
command > output.log 2>&1
```

This performs two operations:

* `> output.log` redirects standard output to `output.log`.
* `2>&1` redirects standard error to wherever standard output currently points.

Example:

```bash
ls /etc /missing-directory > result.log 2>&1
```

Both the directory listing and error message are saved in `result.log`.

### 🖆 Bash Shorthand with `&>`

Bash provides a shorthand for redirecting both standard output and standard error.

```bash
command &> output.log
```

This is equivalent to:

```bash
command > output.log 2>&1
```

Example:

```bash
find /etc -name "*.conf" &> search.log
```

### 🖆 Append Both Streams with `&>>`

The Bash `&>>` operator appends both standard output and standard error to a file.

```bash
command &>> output.log
```

Example:

```bash
date &>> system.log
```

This appends the date and time to `system.log`.

Equivalent form:

```bash
command >> output.log 2>&1
```

# 🗁 Redirection Order

Redirections are processed from left to right. Their order can change where output goes.

This is particularly important when duplicating file descriptors.

### 🖆 Correct Redirection Order

```bash
command > output.log 2>&1
```

Execution:

* Standard output is redirected to `output.log`.
* Standard error is redirected to the current destination of standard output.
* Both streams go to `output.log`.

### 🖆 Different Redirection Order

```bash
command 2>&1 > output.log
```

Execution:

* Standard error is redirected to the current destination of standard output, usually the terminal.
* Standard output is redirected to `output.log`.
* Standard error remains connected to the terminal.

Therefore, standard output and standard error do not both go to the file.

### 🖆 Practical Comparison

| Command | Standard Output | Standard Error |
| --- | --- | --- |
| `command > file` | File | Terminal |
| `command 2> file` | Terminal | File |
| `command > file 2>&1` | File | File |
| `command 2>&1 > file` | File | Original stdout destination |
| `command &> file` | File | File |
| `command > /dev/null 2>&1` | Discarded | Discarded |

Understanding redirection order is essential when writing reliable shell scripts.

# 🗁 Here Documents

A here-document provides multiple lines of input directly inside a shell command.

It uses the `<<` operator.

### 🖆 Basic Here Document

```bash
cat << EOF
Hello Linux
Welcome to Bash
Learning shell redirection
EOF
```

Output:

```text
Hello Linux
Welcome to Bash
Learning shell redirection
```

The shell sends the text between the delimiters to the command's standard input.

### 🖆 Writing Multiple Lines to a File

```bash
cat << EOF > message.txt
Hello Linux
Welcome to Bash
This is a here-document example.
EOF
```

Read the file:

```bash
cat message.txt
```

### 🖆 Variable Expansion in Here Documents

An unquoted delimiter allows parameter expansion, command substitution, and arithmetic expansion.

```bash
name="Muhammad"

cat << EOF
Hello, $name
Today is $(date)
Two plus three is $((2 + 3))
EOF
```

The shell expands the expressions before passing the input to the command.

### 🖆 Prevent Variable Expansion

Quote the delimiter to prevent expansions.

```bash
name="Muhammad"

cat << 'EOF'
Hello, $name
Today is $(date)
EOF
```

Output:

```text
Hello, $name
Today is $(date)
```

The content is passed literally.

### 🖆 Indented Here Documents with `<<-`

The `<<-` operator removes leading tab characters from lines in the here-document.

```bash
cat <<- EOF
 Hello Linux
 Welcome to Bash
 Here-document with tabs
EOF
```

Important:

* Only leading tab characters are removed.
* Ordinary spaces are not removed.
* Tabs are required for indentation removal.

This is useful when writing readable shell scripts.

# 🗁 Here Strings

A here-string provides a single string as standard input to a command.

It uses the Bash `<<<` operator.

### 🖆 Basic Here String

```bash
grep "Linux" <<< "Linux is a powerful operating system"
```

Output:

```text
Linux is a powerful operating system
```

### 🖆 Using Variables

```bash
name="Muhammad"

grep "Muhammad" <<< "$name"
```

### 🖆 Using Here Strings with Other Commands

Count words:

```bash
wc -w <<< "Linux shell redirection"
```

Convert text to uppercase:

```bash
tr '[:lower:]' '[:upper:]' <<< "hello linux"
```

Output:

```text
HELLO LINUX
```

A here-string automatically adds a trailing newline to the supplied string.

# 🗁 File Descriptor Management

Bash allows you to create, duplicate, close, and manage additional file descriptors.

### 🖆 Create a Custom File Descriptor

You can use file descriptor `3` or another available descriptor for custom input/output operations.

```bash
echo "Custom output" 3> custom.log
```

This command opens `custom.log` on file descriptor `3`, but `echo` writes to standard output, not descriptor `3`.

To write to descriptor `3`:

```bash
echo "Custom output" >&3 3> custom.log
```

### 🖆 Duplicate File Descriptors

The syntax:

```bash
n>&m
```

duplicates output file descriptor `m` onto descriptor `n`.

Example:

```bash
command 3>&1
```

This makes descriptor `3` refer to the same output destination as standard output.

For input descriptors, use:

```bash
n<&m
```

### 🖆 Close a File Descriptor

Use `>&-` to close an output file descriptor.

```bash
command 3>&-
```

Use `<&-` to close an input file descriptor.

```bash
command 3<&-
```

### 🖆 Open a File for Reading and Writing

The `<>` operator opens a file for both reading and writing without truncating its existing contents.

```bash
exec 3<> data.txt
```

You can now use descriptor `3` for input and output operations.

```bash
read -r line <&3
printf '%s\n' "$line" >&3
```

The exact position in the file depends on the operations performed through the descriptor.

### 🖆 Persistent Redirection with `exec`

The Bash `exec` builtin can change the current shell's file descriptors.

Example:

```bash
exec > application.log 2>&1
```

After this command, subsequent standard output and standard error from the current shell are written to `application.log`.

This is useful for logging shell scripts.

To restore the original terminal output, save the original descriptor before redirecting:

```bash
exec 3>&1 4>&2

exec > application.log 2>&1

echo "This goes to the log file."

exec 1>&3 2>&4

echo "This goes to the terminal."
```

Descriptor `3` preserves the original standard output, and descriptor `4` preserves the original standard error.

# 🗁 Special Files

Linux exposes several special files that can be used with redirection.

### 🖆 `/dev/null`

Discards all data written to it.

```bash
echo "Discard this message" > /dev/null
```

Common uses:

```bash
command > /dev/null
command 2> /dev/null
command > /dev/null 2>&1
```

### 🖆 `/dev/zero`

Provides a continuous stream of zero bytes when read.

```bash
head -c 10 /dev/zero | od -An -t x1
```

Output:

```text
00 00 00 00 00 00 00 00 00 00
```

### 🖆 `/dev/stdin`

Represents the current process's standard input.

```bash
cat /dev/stdin
```

### 🖆 `/dev/stdout`

Represents the current process's standard output.

```bash
echo "Hello" > /dev/stdout
```

### 🖆 `/dev/stderr`

Represents the current process's standard error.

```bash
echo "An error occurred" > /dev/stderr
```

### 🖆 `/dev/fd`

Provides access to open file descriptors.

For example:

```bash
ls -l /dev/fd
```

On Linux, `/dev/fd` commonly provides access through `/proc/self/fd`.

# 🗁 Redirection with Pipelines

Pipes and redirection are related but serve different purposes.

* Redirection connects a command's input or output to a file or file descriptor.
* A pipe connects one command's standard output to another command's standard input.

### 🖆 Pipe with Output Redirection

```bash
ps aux | grep nginx > nginx-processes.txt
```

Execution:

* `ps aux` produces process information.
* `grep nginx` filters the output.
* `>` saves the filtered result to a file.

### 🖆 Redirect Standard Error in a Pipeline

```bash
find /etc -name "*.conf" 2> errors.log | grep "network"
```

Here:

* `find` sends standard output through the pipe.
* `find` sends standard error to `errors.log`.
* `grep` receives the output from `find`.

### 🖆 Pipe Both Standard Output and Standard Error

```bash
command 2>&1 | grep "error"
```

Both standard output and standard error are redirected into the pipe.

Bash also provides:

```bash
command |& grep "error"
```

The `|&` operator connects both standard output and standard error to the next command.

For more details about pipelines, see `04-SHELL/07-pipes.md`.

# 🗁 Practical Examples

### 🖆 Save Command Output to a File

```bash
ls -lah > directory-list.txt
```

### 🖆 Save Command Output and Errors

```bash
find /etc -name "*.conf" > search.log 2>&1
```

### 🖆 Append Application Logs

```bash
echo "Application started: $(date)" >> application.log
```

### 🖆 Suppress Unnecessary Errors

```bash
find /etc -name "*.conf" 2> /dev/null
```

### 🖆 Read Input from a File

```bash
sort < names.txt
```

### 🖆 Create a Configuration File with a Here Document

```bash
cat << 'EOF' > app.conf
APP_NAME=MyApplication
APP_ENV=development
APP_DEBUG=true
EOF
```

The quoted delimiter prevents variable expansion.

### 🖆 Log an Entire Script

```bash
#!/usr/bin/env bash

exec > application.log 2>&1

echo "Application started: $(date)"

echo "Checking system information..."
uname -a

echo "Application completed: $(date)"
```

All subsequent standard output and standard error are saved to `application.log`.

### 🖆 Log Output Without Losing Terminal Visibility

Use `tee` when you want to display output in the terminal and save it to a file.

```bash
command 2>&1 | tee application.log
```

Append instead of overwriting:

```bash
command 2>&1 | tee -a application.log
```

### 🖆 Separate Standard Output and Standard Error

```bash
command > output.log 2> errors.log
```

This keeps normal output and error messages in separate files.

### 🖆 Redirect Input and Output Together

```bash
sort < names.txt > sorted-names.txt
```

The command reads from `names.txt` and writes its sorted result to `sorted-names.txt`.

# 🗁 Best Practices

* Use `>` when you intentionally want to overwrite a file.
* Use `>>` when you want to preserve existing content.
* Redirect standard error separately when diagnostic information matters.
* Use `2>&1` after the intended standard output redirection when combining streams.
* Use `/dev/null` to suppress output intentionally, not to hide errors during debugging.
* Quote here-document delimiters when literal content is required.
* Use `exec` carefully because it changes file descriptors for the current shell.
* Prefer descriptive filenames for logs and generated output.
* Use `tee` when output should be both visible and recorded.
* Understand redirection order before combining multiple file descriptors.

# 🗁 Common Mistakes

### 🖆 Confusing `>` and `>>`

Incorrect when you intend to append:

```bash
echo "New entry" > application.log
```

This overwrites the existing file.

Correct:

```bash
echo "New entry" >> application.log
```

### 🖆 Redirecting Error Output to the Wrong Destination

Incorrect when the goal is to capture both streams:

```bash
command 2>&1 > output.log
```

Correct:

```bash
command > output.log 2>&1
```

### 🖆 Forgetting That `>` Truncates Files

```bash
command > important.txt
```

This truncates `important.txt` before executing the command.

Use append redirection when existing content must be preserved.

### 🖆 Expecting a Pipe to Capture Standard Error

```bash
command | grep "error"
```

A normal pipe captures standard output, not standard error.

To include both streams:

```bash
command 2>&1 | grep "error"
```

### 🖆 Using Spaces Incorrectly Around Redirection

Correct:

```bash
echo "Hello" > output.txt
```

Avoid placing a space between the descriptor number and its redirection operator:

```bash
command 2 > errors.log
```

This is interpreted as redirecting standard output to a file named `2`, then passing `>` and `errors.log` as arguments to the command.

Correct:

```bash
command 2> errors.log
```

### 🖆 Using Spaces Instead of Tabs with `<<-`

The `<<-` operator strips leading tabs, not ordinary spaces. Use actual tab characters when relying on this behavior.

# 🗁 Practical Exercises

### 🖆 Exercise: Basic Output Redirection

* Create a file named `system-info.txt`.
* Save the output of `uname -a` into it.
* Append the output of `date`.
* Display the complete file.

### 🖆 Exercise: Input Redirection

* Create a file containing at least five names.
* Sort the names using input redirection.
* Count the number of lines using `wc -l`.

### 🖆 Exercise: Error Redirection

* Run a command against a nonexistent directory.
* Redirect its error output to `errors.log`.
* Append another error message to the same file.
* Display the file.

### 🖆 Exercise: Combined Output

* Execute a command that produces both normal output and an error.
* Redirect both streams into `combined.log`.
* Verify that both messages are present.

### 🖆 Exercise: Here Document

* Create a file named `server.conf`.
* Use a here-document to write several configuration values.
* Use a quoted delimiter to prevent variable expansion.
* Display the generated file.

### 🖆 Exercise: Shell Script Logging

Create `system-check.sh`:

```bash
#!/usr/bin/env bash

exec > system-check.log 2>&1

echo "System check started: $(date)"

echo "Hostname:"
hostname

echo "Kernel:"
uname -r

echo "Disk usage:"
df -h

echo "System check completed: $(date)"
```

Make it executable:

```bash
chmod +x system-check.sh
```

Run it:

```bash
./system-check.sh
```

Inspect the log:

```bash
cat system-check.log
```

# 🗁 Interview Questions

### 🖆 Basic Questions

* What is shell redirection?
* What are standard input, standard output, and standard error?
* What are file descriptors?
* What is the difference between `>` and `>>`?
* What is the purpose of `<`?
* What does `/dev/null` do?

### 🖆 Intermediate Questions

* What is the difference between `2>` and `2>>`?
* How do you redirect both standard output and standard error?
* What is the difference between `command > file 2>&1` and `command 2>&1 > file`?
* What is a here-document?
* What is a here-string?
* What is the difference between `<< EOF` and `<< 'EOF'`?
* How does `2>&1` work?
* What is the purpose of `tee`?

### 🖆 Advanced Questions

* How does Bash process multiple redirections?
* What happens when you redirect a file descriptor that is already connected to another destination?
* How can you create and close custom file descriptors?
* What is the difference between `&>` and `> file 2>&1`?
* How does `exec` affect shell file descriptors?
* How can you preserve terminal output while redirecting script output to a log file?
* How do pipes and redirection interact?
* Why does a normal pipeline not capture standard error?

# 🗁 Official References

* [GNU Bash Manual — Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
* [GNU Bash Manual — Here Documents](https://www.gnu.org/software/bash/manual/html_node/Here-Documents.html)
* [GNU Bash Manual — Pipelines](https://www.gnu.org/software/bash/manual/html_node/Pipelines.html)
* [Linux Manual — `dup(2)`](https://man7.org/linux/man-pages/man2/dup.2.html)
* [Linux Manual — `open(2)`](https://man7.org/linux/man-pages/man2/open.2.html)
* [Linux Manual — `proc(5)`](https://man7.org/linux/man-pages/man5/proc.5.html)
