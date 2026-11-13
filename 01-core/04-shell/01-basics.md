# Shell Basics in Ubuntu/Linux

Shell fundamentals provide the foundation for understanding how commands are interpreted, executed, and combined in Ubuntu/Linux.

Bash (Bourne Again SHell) is the primary shell used throughout this chapter. You will learn how to interact with Bash, execute commands, work with shell syntax, understand command types, manage exit statuses, and use essential shell features.

This knowledge is important for Linux administration, software development, automation, and shell scripting.

# 🗁 Introduction to Bash

### 🖆 What Is Bash?

Bash stands for **Bourne Again SHell**.

It is a command-line interpreter and scripting language widely used in Ubuntu/Linux environments.

Bash allows users to:

* Execute Linux commands.
* Manage files and directories.
* Work with environment variables.
* Manage processes and jobs.
* Combine commands using pipelines.
* Redirect input and output.
* Create functions and aliases.
* Automate tasks using shell scripts.

Check the installed Bash version:

```bash
bash --version
```

Example output:

```text
GNU bash, version 5.x
```

The exact version depends on your Ubuntu release.

### 🖆 Bash as a Command Interpreter

Bash accepts commands, interprets their syntax, performs expansions, and executes the requested operations.

Example:

```bash
echo "Hello, Ubuntu!"
```

Output:

```text
Hello, Ubuntu!
```

Here:

* `echo` is the command.
* `"Hello, Ubuntu!"` is the argument.
* Bash interprets the command and executes it.

# 🗁 Starting and Exiting Bash

### 🖆 Starting a Bash Session

Start a new Bash session:

```bash
bash
```

Display the current Bash version:

```bash
bash --version
```

Display the current shell process:

```bash
ps -p $$ -o pid,ppid,comm,args
```

### 🖆 Executing a Command with Bash

Use the `-c` option to execute a command string:

```bash
bash -c 'echo "Hello from Bash"'
```

Execute multiple commands:

```bash
bash -c 'pwd; date; whoami'
```

### 🖆 Exiting Bash

Use:

```bash
exit
```

Or press:

```text
Ctrl + D
```

Both normally terminate the current interactive Bash session.

### 🖆 Nested Bash Sessions

You can start Bash from an existing Bash session.

```bash
bash
```

This creates another shell process.

Display the current shell PID:

```bash
echo $$
```

Exit the nested shell:

```bash
exit
```

You return to the previous shell session.

# 🗁 Understanding the Shell Prompt

### 🖆 What Is a Shell Prompt?

A shell prompt is the text displayed when Bash is ready to receive a command.

A typical Ubuntu prompt looks like:

```text
username@hostname:~/projects$
```

The components represent:

| Component | Description |
| --- | --- |
| `username` | Current user |
| `hostname` | System hostname |
| `~` | User's home directory |
| `projects` | Current directory |
| `$` | Regular-user prompt indicator |

A root shell commonly uses `#` as its prompt indicator.

### 🖆 Understanding the Home Directory Symbol

The tilde (`~`) represents the current user's home directory.

Check your home directory:

```bash
echo "$HOME"
```

Navigate to it:

```bash
cd ~
```

Or simply:

```bash
cd
```

### 🖆 Understanding the Current Directory

Display the current working directory:

```bash
pwd
```

Example:

```text
/home/username/projects
```

### 🖆 Customizing the Bash Prompt

The `PS1` variable controls the primary Bash prompt.

Display the current prompt configuration:

```bash
echo "$PS1"
```

Temporarily customize it:

```bash
PS1='\u@\h:\w\$ '
```

Common prompt escape sequences:

| Sequence | Meaning |
| --- | --- |
| `\u` | Current username |
| `\h` | Hostname |
| `\H` | Full hostname |
| `\w` | Current working directory |
| `\W` | Current directory name |
| `\d` | Date |
| `\t` | Time |
| `\A` | Time in 24-hour HH:MM format |
| `\$` | `$` for regular users, `#` for root |

To make a prompt configuration persistent, add it to `~/.bashrc`.

# 🗁 Executing Commands

### 🖆 Running a Command

Enter a command and press `Enter`.

```bash
date
```

Display the current user:

```bash
whoami
```

Display the system hostname:

```bash
hostname
```

Display the current directory:

```bash
pwd
```

### 🖆 Executing Multiple Commands

Use a semicolon to separate commands:

```bash
pwd; date; whoami
```

Bash executes each command sequentially, regardless of the previous command's exit status.

### 🖆 Conditional Execution with `&&`

Execute the next command only when the previous command succeeds.

```bash
mkdir project && cd project
```

If directory creation succeeds, Bash executes `cd`.

### 🖆 Conditional Execution with `||`

Execute the next command only when the previous command fails.

```bash
cd /invalid/path || echo "Directory not found"
```

This is useful for basic error handling.

### 🖆 Combining Conditional Operators

```bash
mkdir project && cd project || echo "Operation failed"
```

Be aware that `&&` and `||` are left-associative and have equal precedence. For more complex logic, use parentheses or explicit `if` statements to make the intended behavior clear.

# 🗁 Command Structure

### 🖆 General Command Syntax

A typical Linux command follows this structure:

```text
command [options] [arguments]
```

Example:

```bash
ls -lah /var/log
```

| Component | Value | Description |
| --- | --- | --- |
| Command | `ls` | Lists directory contents |
| Options | `-lah` | Modifies command behavior |
| Argument | `/var/log` | Specifies the target directory |

### 🖆 Command Name

The command name identifies the operation to execute.

Examples:

```bash
ls
pwd
mkdir
touch
grep
```

### 🖆 Command Options

Options modify how a command behaves.

Short options commonly use a single hyphen:

```bash
ls -l
```

Long options commonly use two hyphens:

```bash
ls --all
```

Some short options can be combined:

```bash
ls -lah
```

Equivalent individual options:

```bash
ls -l -a -h
```

Whether options can be combined depends on the command.

### 🖆 Command Arguments

Arguments provide information to a command.

Example:

```bash
mkdir projects
```

Here, `projects` is the argument specifying the directory name.

Multiple arguments:

```bash
mkdir frontend backend database
```

# 🗁 Command Types

### 🖆 Built-in Commands

Built-in commands are implemented directly inside Bash.

Examples:

```bash
cd
echo
read
export
alias
type
history
jobs
```

Inspect a built-in:

```bash
type cd
```

Output:

```text
cd is a shell builtin
```

Built-ins can interact directly with the current shell environment.

For example:

```bash
cd /tmp
```

Changes the working directory of the current shell.

### 🖆 External Commands

External commands are separate executable programs installed on the system.

Examples:

```bash
ls
grep
find
curl
git
python3
```

Locate an external command:

```bash
command -v git
```

Inspect its type:

```bash
type git
```

### 🖆 Shell Functions

Functions are reusable command sequences defined within a shell.

Example:

```bash
greet() {
    echo "Hello, $1!"
}
```

Execute the function:

```bash
greet Muhammad
```

Output:

```text
Hello, Muhammad!
```

Inspect a function:

```bash
type greet
```

### 🖆 Aliases

Aliases provide alternative names or shortcuts for commands.

Example:

```bash
alias ll='ls -lah'
```

Execute:

```bash
ll
```

Inspect an alias:

```bash
alias ll
```

### 🖆 Comparing Command Types

| Type | Description | Example |
| --- | --- | --- |
| Built-in | Implemented inside Bash | `cd` |
| External | Separate executable | `grep` |
| Function | User-defined command sequence | `greet()` |
| Alias | Alternative command name | `ll` |

# 🗁 Command Resolution

### 🖆 How Bash Finds Commands

When Bash receives a command, it determines how to execute it.

Command resolution involves shell features such as:

* Aliases.
* Reserved words.
* Functions.
* Shell built-ins.
* External commands found through `PATH`.

The exact resolution rules include context-dependent details, especially for aliases and reserved words.

### 🖆 Using `type`

The `type` command identifies how Bash interprets a command.

```bash
type cd
type ls
type ll
type grep
```

For more detailed information:

```bash
type -a ls
```

This displays all known matches, including aliases, functions, built-ins, and executables where applicable.

### 🖆 Using `command -v`

Check how a command is resolved:

```bash
command -v python3
command -v git
command -v cd
```

This is useful in scripts because it is a shell built-in.

### 🖆 Using `command`

The `command` built-in executes a command without searching for an alias or shell function with that name.

Example:

```bash
command ls
```

This is useful when you want to bypass an alias or function.

### 🖆 Understanding PATH

Bash uses the `PATH` environment variable to locate external executable commands.

Display it:

```bash
echo "$PATH"
```

Display each directory separately:

```bash
printf '%s\n' "$PATH" | tr ':' '\n'
```

Locate a command:

```bash
command -v git
```

# 🗁 Command Arguments and Options

### 🖆 Short Options

Short options typically begin with one hyphen.

```bash
ls -l
ls -a
ls -h
```

Some commands support combined short options:

```bash
ls -lah
```

### 🖆 Long Options

Long options generally begin with two hyphens.

```bash
ls --all
ls --human-readable
ls --recursive
```

### 🖆 Options with Values

Some options accept values.

Example:

```bash
head -n 5 /etc/passwd
```

Here, `5` specifies the number of lines to display.

Some commands also support the `--option=value` syntax:

```bash
grep --color=always "root" /etc/passwd
```

The exact syntax depends on the command.

### 🖆 Getting Command Help

Use the `--help` option:

```bash
ls --help
```

Read the manual:

```bash
man ls
```

Search for a manual page:

```bash
man -k network
```

Exit a manual page using:

```text
q
```

# 🗁 Quoting and Escaping

### 🖆 Why Quoting Matters

Quoting controls how Bash interprets characters and expands expressions.

It is especially important when working with:

* Spaces in filenames.
* Variables.
* Wildcards.
* Special characters.
* Command substitutions.

### 🖆 Single Quotes

Single quotes preserve the literal meaning of almost every character inside them.

```bash
echo 'Hello $USER'
```

Output:

```text
Hello $USER
```

Variables are not expanded inside single quotes.

### 🖆 Double Quotes

Double quotes preserve most characters literally while allowing selected expansions.

```bash
echo "Hello $USER"
```

Output:

```text
Hello username
```

Variable expansion works inside double quotes.

Command substitution also works:

```bash
echo "Today is $(date)"
```

### 🖆 Unquoted Text

Unquoted text allows Bash to perform expansions, word splitting, and filename expansion.

Example:

```bash
name="my project"

echo $name
```

The value may undergo word splitting and be treated as separate arguments.

Prefer:

```bash
echo "$name"
```

### 🖆 Escaping Characters

The backslash (`\`) escapes the special meaning of the following character in contexts where it is active.

Example:

```bash
echo Hello\ World
```

Output:

```text
Hello World
```

Escape a dollar sign:

```bash
echo "\$HOME"
```

Output:

```text
$HOME
```

### 🖆 Quoting Comparison

| Syntax | Behavior |
| --- | --- |
| `'text'` | Preserves literal characters |
| `"text"` | Allows selected expansions |
| `\character` | Escapes a character |
| `text` without quotes | Allows normal shell expansions and word splitting |

# 🗁 Command Separators and Operators

### 🖆 Semicolon (`;`)

Separates commands and executes them sequentially.

```bash
echo "First"; echo "Second"
```

### 🖆 Logical AND (`&&`)

Executes the next command only when the previous command succeeds.

```bash
mkdir project && echo "Directory created"
```

### 🖆 Logical OR (`||`)

Executes the next command only when the previous command fails.

```bash
cd /missing || echo "Unable to change directory"
```

### 🖆 Background Operator (`&`)

Starts a command asynchronously.

```bash
sleep 100 &
```

The shell returns control without waiting for the command to finish.

### 🖆 Pipe Operator (`|`)

Passes standard output from one command to another command's standard input.

```bash
ps aux | grep nginx
```

### 🖆 Grouping Commands

Use parentheses to execute commands in a subshell:

```bash
(
    cd /tmp
    pwd
)
```

Use braces to execute a group of commands in the current shell:

```bash
{
    echo "Starting"
    date
    echo "Finished"
}
```

# 🗁 Exit Status

### 🖆 What Is an Exit Status?

An exit status is an integer returned by a command to indicate its result.

By convention:

* `0` indicates success.
* A non-zero value indicates failure or another command-specific condition.

### 🖆 Accessing the Exit Status

Use `$?`:

```bash
ls /etc
echo $?
```

A successful command normally returns:

```text
0
```

Check a failed command:

```bash
ls /invalid-directory
echo $?
```

### 🖆 Using Exit Status in Conditions

```bash
if grep -q "root" /etc/passwd; then
    echo "Root account found"
else
    echo "Root account not found"
fi
```

The `if` statement evaluates the command's exit status.

### 🖆 Understanding Common Exit Status Values

| Status | Meaning |
| ---: | --- |
| `0` | Success |
| `1` | Commonly used for a general failure or unsuccessful search |
| `2` | Commonly used for invalid usage or syntax errors |
| `126` | Command found but cannot be executed |
| `127` | Command not found |
| `128 + N` | Common convention for termination by signal number `N` |

Exact meanings depend on the command and shell.

### 🖆 Exiting with a Specific Status

Use the `exit` command:

```bash
exit 0
```

Or:

```bash
exit 1
```

Inside a script, this communicates the script's result to its calling process.

# 🗁 Working with Shell Built-ins

### 🖆 The `cd` Command

Change the current directory:

```bash
cd /var/log
```

Navigate to the home directory:

```bash
cd ~
```

Return to the previous directory:

```bash
cd -
```

### 🖆 The `pwd` Command

Display the current working directory:

```bash
pwd
```

Display the logical working directory:

```bash
pwd -L
```

Display the physical working directory:

```bash
pwd -P
```

### 🖆 The `echo` Command

Display text:

```bash
echo "Hello Linux"
```

Display a variable:

```bash
echo "$HOME"
```

For more predictable output formatting in scripts, consider using `printf`.

### 🖆 The `printf` Command

Format and display output:

```bash
printf "Name: %s\n" "Muhammad"
```

Display numbers:

```bash
printf "Total: %d\n" 100
```

Display multiple values:

```bash
printf "%s - %s\n" "Ubuntu" "Linux"
```

### 🖆 The `read` Command

Read input from the user:

```bash
read -r -p "Enter your name: " name

echo "Hello, $name"
```

The `-r` option prevents backslash interpretation.

### 🖆 The `export` Command

Export a variable to child processes:

```bash
export APP_ENV="development"
```

Inspect it:

```bash
printenv APP_ENV
```

### 🖆 The `unset` Command

Remove a shell variable:

```bash
name="Muhammad"

unset name
```

Verify:

```bash
echo "$name"
```

# 🗁 Aliases

### 🖆 What Is an Alias?

An alias creates a shortcut for a command or command sequence.

Define an alias:

```bash
alias ll='ls -lah'
```

Execute it:

```bash
ll
```

### 🖆 Listing Aliases

Display all currently defined aliases:

```bash
alias
```

Inspect a specific alias:

```bash
alias ll
```

### 🖆 Removing an Alias

```bash
unalias ll
```

Remove all aliases:

```bash
unalias -a
```

### 🖆 Making Aliases Persistent

Add aliases to:

```text
~/.bashrc
```

Example:

```bash
alias ll='ls -lah'
alias la='ls -A'
alias gs='git status'
alias ..='cd ..'
```

Reload the configuration:

```bash
source ~/.bashrc
```

Aliases are primarily intended for interactive use. For reusable script logic, prefer functions or executable scripts.

# 🗁 Command History

### 🖆 Displaying History

```bash
history
```

Display the latest entries:

```bash
history 10
```

Search history:

```bash
history | grep git
```

### 🖆 Reusing Previous Commands

Execute the previous command:

```bash
!!
```

Execute a command by its history number:

```bash
!25
```

Execute the most recent command beginning with `git`:

```bash
!git
```

### 🖆 Searching History Interactively

Press:

```text
Ctrl + R
```

Enter a search term.

Press `Ctrl + R` again to find an earlier match.

### 🖆 History Configuration

Bash commonly stores persistent history in:

```text
~/.bash_history
```

Inspect history-related variables:

```bash
echo "$HISTFILE"
echo "$HISTSIZE"
echo "$HISTFILESIZE"
```

Modify the in-memory history size:

```bash
HISTSIZE=5000
```

Modify the history file size:

```bash
HISTFILESIZE=10000
```

These settings can be added to `~/.bashrc` to persist across sessions.

# 🗁 Shell Expansion

### 🖆 Parameter Expansion

Bash replaces variable references with their values.

```bash
name="Muhammad"

echo "$name"
echo "${name}"
```

Use braces when additional characters follow the variable name:

```bash
project="notes"

echo "${project}_backup"
```

### 🖆 Command Substitution

Capture the output of a command:

```bash
current_date=$(date)

echo "$current_date"
```

### 🖆 Arithmetic Expansion

Perform arithmetic operations:

```bash
a=10
b=20

result=$((a + b))

echo "$result"
```

### 🖆 Filename Expansion

Match filenames using wildcard patterns:

```bash
ls *.txt
```

Other examples:

```bash
ls file?.txt
ls report[123].txt
```

### 🖆 Brace Expansion

Generate multiple values:

```bash
echo file{1,2,3}.txt
```

Create multiple directories:

```bash
mkdir -p project/{src,tests,docs}
```

### 🖆 Process Substitution

Bash supports process substitution, which exposes command output or input through a filename-like interface.

Example:

```bash
diff <(sort file1.txt) <(sort file2.txt)
```

This compares the sorted contents of two files without creating intermediate sorted files.

# 🗁 Subshells and Command Groups

### 🖆 What Is a Subshell?

A subshell is a separate shell execution environment created by the current shell.

Example:

```bash
(
    name="Linux"
    echo "$name"
)
```

Variables created inside this subshell do not modify the parent shell's variables.

### 🖆 Creating a Subshell

Use parentheses:

```bash
(
    cd /tmp
    pwd
)
```

After execution, the parent shell remains in its original directory.

### 🖆 Command Groups

Use braces to group commands in the current shell:

```bash
{
    name="Ubuntu"
    echo "$name"
}
```

The variable remains available in the current shell after the group executes.

### 🖆 Comparing Subshells and Command Groups

| Subshell `( )` | Command Group `{ }` |
| --- | --- |
| Runs in a separate shell environment | Runs in the current shell |
| Variable changes do not affect parent shell | Variable changes affect current shell |
| Requires parentheses | Requires braces and command separators |
| Useful for isolation | Useful for grouping operations |

# 🗁 Debugging Bash Commands

### 🖆 Checking Command Syntax

Use Bash's syntax-checking option:

```bash
bash -n script.sh
```

This checks syntax without executing the script.

### 🖆 Tracing Command Execution

Use:

```bash
bash -x script.sh
```

This displays commands as Bash executes them.

### 🖆 Enabling Debugging Inside a Script

Enable execution tracing:

```bash
set -x
```

Disable it:

```bash
set +x
```

Example:

```bash
#!/usr/bin/env bash

set -x

name="Ubuntu"
echo "Hello, $name"

set +x
```

### 🖆 Inspecting Command Resolution

```bash
type ls
type cd
command -v python3
```

These commands help identify aliases, functions, built-ins, and executable paths.

### 🖆 Checking Exit Status

```bash
some_command

status=$?

echo "Exit status: $status"
```

Capture the status immediately after the command because subsequent commands overwrite `$?`.

# 🗁 Best Practices

* Use Bash consistently when writing Bash-specific commands and scripts.
* Understand the difference between built-ins, functions, aliases, and external commands.
* Quote variable expansions when word splitting and filename expansion are not intended.
* Prefer `printf` over `echo` when predictable formatting is important.
* Use `command -v` to check whether a command is available.
* Use meaningful variable and function names.
* Check exit statuses when handling command failures.
* Use `&&` and `||` for simple conditional execution.
* Use explicit `if` statements for complex decision-making.
* Keep interactive aliases in `~/.bashrc`.
* Use functions or scripts for reusable automation.
* Use `bash -n` before executing modified scripts.
* Use `bash -x` when debugging unexpected behavior.
* Avoid unnecessary command chaining when it reduces readability.

# 🗁 Common Mistakes

### 🖆 Adding Spaces Around Variable Assignment

Incorrect:

```bash
name = "Ubuntu"
```

Correct:

```bash
name="Ubuntu"
```

Bash interprets spaces as command separators, so variable assignments must not contain spaces around `=`.

### 🖆 Forgetting to Quote Variables

Potentially problematic:

```bash
name="My Project"

mkdir $name
```

Correct:

```bash
mkdir "$name"
```

Quoting prevents unintended word splitting and filename expansion.

### 🖆 Confusing Shell Variables with Environment Variables

A variable does not automatically become part of the environment inherited by child processes.

Incorrect assumption:

```bash
APP_ENV="development"
```

Export it when child processes need access:

```bash
export APP_ENV="development"
```

### 🖆 Assuming Every Command Is External

Some commands are implemented directly inside Bash.

Check command types:

```bash
type cd
type echo
type ls
```

### 🖆 Using Single Quotes When Variable Expansion Is Required

```bash
name="Ubuntu"

echo '$name'
```

This prints the literal text `$name`.

Use:

```bash
echo "$name"
```

### 🖆 Assuming `&&` and `||` Have Different Precedence

They have equal precedence and are evaluated from left to right.

Use parentheses or explicit conditions when combining them in complex expressions.

# 🗁 Practice Exercises

### 🖆 Basic Bash Operations

* Display the Bash version.
* Start a nested Bash session.
* Display the current PID and PPID.
* Exit the nested shell.
* Display your username and hostname.
* Customize your Bash prompt temporarily.

### 🖆 Command Structure

* Execute commands with short options.
* Execute commands with long options.
* Pass multiple arguments to `mkdir`.
* Inspect commands using `type`.
* Locate executables using `command -v`.

### 🖆 Quoting and Variables

* Create a variable containing spaces.
* Print it using double quotes.
* Compare single-quoted and double-quoted output.
* Escape a dollar sign.
* Perform command substitution.
* Perform arithmetic expansion.

### 🖆 Command Execution

* Execute multiple commands using `;`.
* Use `&&` for conditional execution.
* Use `||` for basic error handling.
* Group commands with parentheses.
* Group commands with braces.
* Inspect command exit statuses.

### 🖆 Aliases and History

* Create an alias named `ll`.
* Display all aliases.
* Remove an alias.
* Add persistent aliases to `~/.bashrc`.
* Search command history.
* Execute the previous command using `!!`.

### 🖆 Debugging

* Create a simple Bash script.
* Check its syntax using `bash -n`.
* Execute it with `bash -x`.
* Enable and disable tracing inside the script.
* Capture and display an exit status.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

* What is Bash?
* What is the difference between Bash and a terminal?
* What is a shell prompt?
* What is the purpose of the `PS1` variable?
* What is the difference between `bash` and `bash -c`?
* What is a nested shell session?

### 🖆 Command Execution Questions

* What is the general structure of a Linux command?
* What is the difference between command options and arguments?
* What is the difference between short and long options?
* How does Bash resolve commands?
* What is the difference between built-in and external commands?
* What is the purpose of `command -v`?
* What is the difference between `;`, `&&`, and `||`?

### 🖆 Quoting and Expansion Questions

* What is the difference between single quotes and double quotes?
* What is the purpose of escaping characters?
* What is parameter expansion?
* What is command substitution?
* What is arithmetic expansion?
* What is filename expansion?
* What is brace expansion?
* What is process substitution?

### 🖆 Advanced Questions

* What is an exit status?
* What does exit status `127` mean?
* What is the difference between a shell function and an alias?
* What is the difference between a subshell and a command group?
* What is the purpose of `set -x`?
* How do you check Bash script syntax without executing it?
* Why should variables usually be quoted?
* How does Bash history work?

# 🗁 References

* [GNU Bash Manual](https://www.gnu.org/software/bash/manual/)
* [Bash Reference Manual — Basic Shell Features](https://www.gnu.org/software/bash/manual/html_node/Basic-Shell-Features.html)
* [Bash Reference Manual — Shell Commands](https://www.gnu.org/software/bash/manual/html_node/Shell-Commands.html)
* [Bash Reference Manual — Shell Expansions](https://www.gnu.org/software/bash/manual/html_node/Shell-Expansions.html)
* [Bash Reference Manual — Command Execution](https://www.gnu.org/software/bash/manual/html_node/Command-Execution.html)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
* [Linux Manual Pages](https://man7.org/linux/man-pages/)
