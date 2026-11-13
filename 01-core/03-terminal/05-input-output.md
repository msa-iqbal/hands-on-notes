# Input and Output in Linux

Input and output (I/O) are fundamental concepts in Linux. They define how commands receive data and produce results.

Linux provides a standard I/O system that allows commands to communicate with users, files, devices, and other processes.

Understanding standard input, standard output, and standard error is essential for working effectively with the Ubuntu terminal, shell scripting, command pipelines, and automation.

# 🗁 Understanding Input and Output

Every Linux process normally starts with three standard I/O streams:

* **Standard Input (stdin):** Receives input data.
* **Standard Output (stdout):** Produces normal command output.
* **Standard Error (stderr):** Produces error and diagnostic messages.

These streams allow commands to work independently of where their input comes from or where their output goes.

For example:

```bash
cat
```

The `cat` command reads from standard input when no filename is supplied and writes the received content to standard output.

You can type text into the terminal and press `Ctrl + D` to indicate the end of input.

# 🗁 Standard Input

Standard input is the stream from which a process receives data.

Its file descriptor is:

```text
0
```

By default, standard input usually comes from the terminal.

### 🖆 Reading from the Terminal

Run:

```bash
cat
```

Enter:

```text
Hello Linux
Learning Ubuntu
```

Press `Ctrl + D` to finish.

The command reads the input and displays it through standard output.

### 🖆 Reading from a File

Many commands accept a filename as an argument:

```bash
cat notes.txt
```

Some commands can also receive a file through standard input:

```bash
sort < names.txt
```

Here, the shell connects `names.txt` to the command's standard input.

The command reads the file contents through stdin rather than receiving the filename as an ordinary argument.

### 🖆 Reading Multiple Files

The `cat` command can read multiple files:

```bash
cat file1.txt file2.txt
```

It combines their contents and writes the result to standard output.

# 🗁 Standard Output

Standard output is the stream used by a process to produce normal results.

Its file descriptor is:

```text
1
```

By default, standard output is displayed in the terminal.

### 🖆 Displaying Output with `echo`

```bash
echo "Hello Linux"
```

Display multiple lines:

```bash
echo "First line"
echo "Second line"
```

### 🖆 Displaying Output with `printf`

The `printf` command provides more control over formatting.

```bash
printf "Hello Linux\n"
```

Format variables:

```bash
name="Ubuntu"
printf "Operating System: %s\n" "$name"
```

Format numbers:

```bash
printf "Price: %.2f\n" 19.567
```

Output:

```text
Price: 19.57
```

Common format specifiers:

| Specifier | Meaning |
| --- | --- |
| `%s` | String |
| `%d` | Integer |
| `%f` | Floating-point number |
| `%.2f` | Floating-point number with two decimal places |
| `\n` | Newline |
| `\t` | Tab |

### 🖆 Displaying File Contents

Use `cat` to display a complete file:

```bash
cat notes.txt
```

Use `head` to display the beginning:

```bash
head notes.txt
```

Display the first five lines:

```bash
head -n 5 notes.txt
```

Use `tail` to display the end:

```bash
tail notes.txt
```

Display the last ten lines:

```bash
tail -n 10 notes.txt
```

Follow a file as new lines are appended:

```bash
tail -f application.log
```

This is particularly useful for monitoring application logs.

# 🗁 Standard Error

Standard error is the stream used for error messages and diagnostic information.

Its file descriptor is:

```text
2
```

For example:

```bash
cat existing.txt missing.txt
```

If `existing.txt` exists but `missing.txt` does not, the command can produce both normal output and an error message.

* The contents of `existing.txt` go to stdout.
* The error about `missing.txt` goes to stderr.

### 🖆 Separating Standard Output and Standard Error

You can redirect these streams independently.

```bash
command > output.txt 2> errors.txt
```

Here:

* `>` redirects standard output.
* `2>` redirects standard error.

Example:

```bash
find /etc /missing-directory -name "*.conf" > results.txt 2> errors.txt
```

Normal search results are written to `results.txt`, while errors are written to `errors.txt`.

### 🖆 Understanding `/dev/null`

`/dev/null` is a special device that discards data written to it.

Discard standard output:

```bash
command > /dev/null
```

Discard standard error:

```bash
command 2> /dev/null
```

Discard both:

```bash
command > /dev/null 2>&1
```

This is useful when you intentionally do not need command output or diagnostic messages.

# 🗁 File Descriptors

A file descriptor is a small non-negative integer used by a process to identify an open file or I/O resource.

Linux processes normally start with these three standard descriptors:

| File Descriptor | Name | Default Destination |
| --- | --- | --- |
| `0` | stdin | Terminal input |
| `1` | stdout | Terminal output |
| `2` | stderr | Terminal error output |

Additional file descriptors can be opened by applications and shell scripts.

### 🖆 Inspecting Standard File Descriptors

On Ubuntu, the `/proc` filesystem exposes process information.

Inspect the current shell's standard streams:

```bash
ls -l /proc/$$/fd
```

The `$$` variable represents the current shell's process ID.

Typical output may contain links similar to:

```text
0 -> /dev/pts/0
1 -> /dev/pts/0
2 -> /dev/pts/0
```

`/dev/pts/0` represents a pseudo-terminal.

The actual output depends on how the shell was started and whether its streams have been redirected.

# 🗁 Reading User Input

Shell scripts often need to receive input from users.

Bash provides the `read` builtin command.

### 🖆 Basic User Input

```bash
read name

echo "Hello, $name"
```

When executed, the script waits for the user to enter a value.

### 🖆 Using a Prompt

```bash
read -p "Enter your name: " name

echo "Hello, $name"
```

The `-p` option displays a prompt before reading input.

### 🖆 Reading Multiple Values

```bash
read -p "Enter your first and last name: " first_name last_name

echo "First Name: $first_name"
echo "Last Name: $last_name"
```

### 🖆 Reading Passwords

Use `-s` to prevent typed characters from being displayed:

```bash
read -sp "Enter password: " password
echo
echo "Password received."
```

The `-s` option is useful for interactive scripts that need to read sensitive input.

### 🖆 Reading Input with a Timeout

Use `-t` to specify a timeout in seconds:

```bash
read -t 10 -p "Enter your name: " name
```

The command waits up to ten seconds for input.

### 🖆 Reading Input into an Array

Use `-a` to split input into array elements:

```bash
read -p "Enter three names: " -a names

printf '%s\n' "${names[@]}"
```

For whitespace-sensitive or structured input, more careful parsing may be necessary.

# 🗁 Displaying Output

Linux provides multiple commands for displaying information.

### 🖆 Using `echo`

```bash
echo "Welcome to Ubuntu"
```

Display variables:

```bash
username="developer"

echo "Welcome, $username"
```

Suppress the trailing newline:

```bash
echo -n "Processing..."
```

### 🖆 Using `printf`

```bash
printf "Name: %s\nAge: %d\n" "Alex" 25
```

Unlike `echo`, `printf` provides predictable formatting behavior and is generally preferred for structured shell output.

### 🖆 Using `cat`

```bash
cat notes.txt
```

Combine multiple files:

```bash
cat file1.txt file2.txt
```

### 🖆 Using `less`

The `less` command allows you to inspect long output interactively.

```bash
less /var/log/syslog
```

Useful navigation keys:

| Key | Action |
| --- | --- |
| `Space` | Next page |
| `b` | Previous page |
| `Enter` | Next line |
| `g` | Beginning |
| `G` | End |
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Next search match |
| `q` | Quit |

# 🗁 Working with Input and Output Streams

Linux shell commands can receive input and produce output through different sources and destinations.

The shell controls how these streams are connected.

### 🖆 Command-Line Input and Output

```bash
cat notes.txt
```

The filename is passed as an argument. The command reads the file and writes its contents to stdout.

### 🖆 Input from a File

```bash
sort < names.txt
```

The shell connects `names.txt` to stdin.

### 🖆 Output to a File

```bash
date > current-date.txt
```

The shell connects stdout to `current-date.txt`.

If the file already exists, its previous contents are overwritten.

### 🖆 Append Output to a File

```bash
date >> current-date.txt
```

The output is appended instead of replacing the existing contents.

### 🖆 Combining Input and Output

```bash
sort < names.txt > sorted-names.txt
```

This command:

* Reads input from `names.txt`.
* Sorts the input.
* Writes the result to `sorted-names.txt`.

### 🖆 Separating Output Streams

```bash
find /etc /invalid -name "*.conf" > results.txt 2> errors.txt
```

This separates normal output from error messages.

Detailed redirection operators will be covered in the dedicated Redirection chapter.

# 🗁 Common I/O Commands

| Command | Purpose |
| --- | --- |
| `echo` | Display text |
| `printf` | Display formatted output |
| `read` | Read user input in Bash |
| `cat` | Read and concatenate files |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `less` | View output interactively |
| `more` | View output page by page |
| `tee` | Write input to stdout and files |
| `wc` | Count lines, words, and bytes |
| `sort` | Sort input lines |
| `uniq` | Filter adjacent repeated lines |
| `cut` | Extract fields or characters |
| `tr` | Translate or delete characters |

### 🖆 Using `tee`

The `tee` command reads from standard input and writes the received data to both standard output and one or more files.

```bash
echo "Hello Linux" | tee output.txt
```

Display the text in the terminal and save it to `output.txt`.

Append instead of overwriting:

```bash
echo "Another line" | tee -a output.txt
```

Use `tee` with another command:

```bash
date | tee current-date.txt
```

### 🖆 Using `wc`

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

Count characters:

```bash
wc -m notes.txt
```

Display all counts:

```bash
wc notes.txt
```

# 🗁 Practical Workflows

### 🖆 Collecting User Information

Create a Bash script named `user-info.sh`:

```bash
#!/usr/bin/env bash

read -p "Enter your name: " name
read -p "Enter your age: " age

printf "\nUser Information\n"
printf "Name: %s\n" "$name"
printf "Age: %s\n" "$age"
```

Run it:

```bash
chmod +x user-info.sh
./user-info.sh
```

### 🖆 Saving Command Output

Save the current date and time:

```bash
date > current-date.txt
```

Display the saved result:

```bash
cat current-date.txt
```

Append another timestamp:

```bash
date >> current-date.txt
```

### 🖆 Capturing Command Errors

Run a command and save its normal output and errors separately:

```bash
find /etc /invalid-directory -name "*.conf" > results.txt 2> errors.txt
```

Inspect the files:

```bash
cat results.txt
cat errors.txt
```

### 🖆 Monitoring Application Logs

Follow a log file:

```bash
tail -f /var/log/syslog
```

Stop monitoring:

```text
Ctrl + C
```

### 🖆 Displaying and Saving System Information

```bash
uname -a | tee system-info.txt
```

This displays system information and saves a copy to a file.

# 🗁 Best Practices

* Understand the difference between stdin, stdout, and stderr.
* Use `printf` for predictable formatted output.
* Quote shell variables when passing user input to commands.
* Separate normal output from errors when troubleshooting.
* Use `less` when inspecting large files or command output.
* Use `tee` when output needs to be displayed and saved simultaneously.
* Use meaningful filenames when saving command output.
* Be careful with `>` because it overwrites existing file contents.
* Use `>>` when appending information.
* Use `read -s` when collecting passwords interactively.
* Prefer explicit input and output handling in shell scripts.

# 🗁 Common Mistakes

| Mistake | Explanation | Better Approach |
| --- | --- | --- |
| Confusing stdout and stderr | They are separate streams | Understand file descriptors `1` and `2` |
| Using `>` when intending to append | Existing file contents are overwritten | Use `>>` |
| Forgetting to quote variables | Spaces and special characters can cause unexpected argument splitting | Use `"$variable"` |
| Using `echo` for complex formatting | Behavior can vary with escape sequences and options | Use `printf` |
| Expecting `cat file.txt` to read stdin | A filename argument tells `cat` to read that file | Run `cat` without a filename |
| Ignoring error output | A command may fail even when normal output is empty | Inspect stderr and the exit status |
| Forgetting `Ctrl + D` | Interactive commands may continue waiting for input | Send EOF with `Ctrl + D` |
| Redirecting output without checking the destination | Existing contents may be lost | Verify the filename before using `>` |

# 🗁 Practice Exercises

### 🖆 Beginner Exercises

* Display your name using `echo`.
* Display formatted information using `printf`.
* Read a username using Bash `read`.
* Display the contents of a text file using `cat`.
* Display the first five lines of a file using `head`.
* Display the last five lines using `tail`.

### 🖆 Intermediate Exercises

* Read a name and age from the user and display them.
* Read a password without displaying typed characters.
* Save the current date to a file.
* Append another date to the same file.
* Count the lines and words in a text file.
* Use `tee` to display and save command output.
* Separate normal command output from error messages.

### 🖆 Advanced Exercises

* Create an interactive Bash script that collects user information.
* Display the current date, username, and working directory using `printf`.
* Read input with a timeout.
* Capture the output and error streams of a command independently.
* Monitor a system log using `tail -f`.
* Build a script that saves diagnostic information into a report file.

# 🗁 Interview Questions

### 🖆 Basic Questions

**What is standard input?**

Standard input is the data stream from which a process receives input. Its file descriptor is `0`.

**What is standard output?**

Standard output is the stream used by a process to produce normal results. Its file descriptor is `1`.

**What is standard error?**

Standard error is the stream used for error and diagnostic messages. Its file descriptor is `2`.

**What are the default standard I/O streams connected to?**

They are normally connected to the terminal when a command is run interactively.

### 🖆 Intermediate Questions

**What is a file descriptor?**

A file descriptor is an integer used by a process to identify an open file or I/O resource.

**What is the difference between stdout and stderr?**

Stdout carries normal output, while stderr carries diagnostic and error messages. Keeping them separate makes it easier to process results and troubleshoot failures.

**What is the difference between `echo` and `printf`?**

`echo` displays text with a trailing newline by default. `printf` supports explicit formatting and provides more predictable output behavior.

**What does the `tee` command do?**

It reads from standard input and writes the data to standard output and one or more files.

### 🖆 Advanced Questions

**Can a command receive input from a file instead of the terminal?**

Yes. Shell input redirection connects a file to the command's standard input.

```bash
sort < names.txt
```

**Can stdout and stderr be redirected independently?**

Yes.

```bash
command > output.txt 2> errors.txt
```

**What is `/dev/null`?**

It is a special device that discards data written to it. It is commonly used to suppress unwanted command output.

**What is EOF?**

EOF stands for End of File. It indicates that no more input is available from a stream. In an interactive terminal, `Ctrl + D` sends EOF when appropriate.

# 🗁 Official References

* [GNU Bash Manual — Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
* [GNU Bash Manual — Bash Builtins](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html)
* [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/)
* [Linux Standard Base — Shell and Utilities](https://refspecs.linuxfoundation.org/lsb.shtml)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
