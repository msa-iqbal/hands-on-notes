# Bash Shell in Ubuntu/Linux

Bash (Bourne Again SHell) is one of the most widely used command interpreters and scripting languages in Linux environments.

It provides an interactive command-line interface and a powerful programming environment for automating system administration, development workflows, application deployment, and server management.

This chapter focuses specifically on Bash in Ubuntu/Linux, covering its architecture, execution modes, configuration, built-in features, variables, functions, job control, debugging, and advanced capabilities.

# 🗁 Introduction to Bash

### 🖆 What Is Bash?

Bash stands for **Bourne Again SHell**.

It is a command interpreter developed by the GNU Project as a free software replacement for the traditional Bourne shell (`sh`).

Bash provides two primary capabilities:

* **Interactive shell:** Execute commands manually through a terminal.
* **Scripting language:** Automate operations using reusable shell scripts.

Bash is commonly used for:

* Linux system administration.
* Application deployment.
* Server automation.
* File and directory management.
* Process management.
* Software development workflows.
* Backup automation.
* CI/CD pipelines.
* Monitoring and maintenance.

### 🖆 Checking the Bash Version

```bash
bash --version
```

Display the version number directly:

```bash
bash -c 'echo "$BASH_VERSION"'
```

Inspect the executable:

```bash
command -v bash
```

On Ubuntu, Bash is commonly installed at:

```text
/bin/bash
```

# 🗁 Bash Architecture

### 🖆 How Bash Works

Bash acts as an intermediary between the user and the operating system.

When a command is entered, Bash interprets the command, performs the required expansions, processes redirections, and executes the requested operation.

```text
              User
                |
                v
         Terminal Emulator
                |
                v
              Bash
                |
       +--------+--------+
       |        |        |
       v        v        v
    Built-ins Functions External
                         Commands
       |        |        |
       +--------+--------+
                |
                v
          Linux Kernel
                |
                v
         System Resources
```

### 🖆 Bash Execution Responsibilities

| Component | Responsibility |
| --- | --- |
| Parser | Interprets command syntax |
| Expansion Engine | Processes variables, substitutions, and patterns |
| Command Resolver | Identifies commands and execution targets |
| Built-in Handler | Executes shell built-in commands |
| Process Manager | Creates and manages command processes |
| Job Control | Manages foreground and background jobs |
| I/O Manager | Handles input, output, and redirection |

### 🖆 Bash Process Identification

Display the current Bash process ID:

```bash
echo "$$"
```

Display the parent process ID:

```bash
echo "$PPID"
```

Inspect the running shell:

```bash
ps -p $$ -o pid,ppid,comm,args
```

The `$$` parameter identifies the current Bash process in normal Bash execution contexts.

# 🗁 Installing and Checking Bash

### 🖆 Checking Whether Bash Is Installed

```bash
command -v bash
```

Check the installed package:

```bash
dpkg -l bash
```

Display the executable version:

```bash
bash --version
```

### 🖆 Installing Bash on Ubuntu

Update package information:

```bash
sudo apt update
```

Install Bash:

```bash
sudo apt install bash
```

Ubuntu normally includes Bash by default, so a separate installation is usually unnecessary.

### 🖆 Finding Bash Executables

```bash
whereis bash
```

Inspect the executable:

```bash
ls -l /bin/bash
```

On modern Ubuntu installations, `/bin` may be a symbolic link to `/usr/bin`.

# 🗁 Starting and Running Bash

### 🖆 Starting an Interactive Bash Session

```bash
bash
```

This starts a new Bash process.

Exit the session:

```bash
exit
```

### 🖆 Executing a Single Command

Use `-c`:

```bash
bash -c 'echo "Hello from Bash"'
```

Execute multiple commands:

```bash
bash -c 'pwd; whoami; date'
```

### 🖆 Executing a Bash Script

Create a script:

```bash
nano hello.sh
```

Add:

```bash
#!/usr/bin/env bash

echo "Hello from Ubuntu!"
echo "Current user: $USER"
echo "Current directory: $PWD"
```

Run it through Bash:

```bash
bash hello.sh
```

Alternatively, make it executable:

```bash
chmod +x hello.sh
./hello.sh
```

### 🖆 Running Bash Without Reading Startup Files

Start Bash without reading normal startup configuration files:

```bash
bash --noprofile --norc
```

This is useful when troubleshooting problems caused by shell configuration.

# 🗁 Bash Execution Modes

### 🖆 Interactive Bash

An interactive shell reads commands from the user.

Start one:

```bash
bash -i
```

Check whether Bash is interactive:

```bash
case $- in
    *i*) echo "Interactive Bash" ;;
    *)   echo "Non-interactive Bash" ;;
esac
```

Interactive Bash commonly supports:

* Command history.
* Command-line editing.
* Aliases.
* Completion.
* Job control.
* Prompt customization.

### 🖆 Non-Interactive Bash

A non-interactive shell executes commands without providing an interactive prompt.

Example:

```bash
bash -c 'echo "Non-interactive execution"'
```

Execute a script:

```bash
bash script.sh
```

Common use cases:

* Automated scripts.
* Cron jobs.
* CI/CD pipelines.
* Deployment automation.
* Scheduled maintenance.

### 🖆 Login Bash

A login shell is started as part of a user login session.

Start a login Bash session:

```bash
bash --login
```

Check the current login-shell status:

```bash
shopt -q login_shell && echo "Login shell" || echo "Non-login shell"
```

### 🖆 Non-Login Bash

An interactive non-login Bash session normally reads:

```text
~/.bashrc
```

This is commonly where users configure aliases, functions, prompt settings, and interactive behavior.

### 🖆 Execution Mode Comparison

| Mode | Description |
| --- | --- |
| Interactive | Accepts commands from a user |
| Non-interactive | Executes supplied commands or scripts |
| Login | Reads login startup configuration |
| Non-login | Uses non-login startup behavior |

These modes are independent characteristics. A shell can be interactive and login, interactive and non-login, or non-interactive.

# 🗁 Bash Configuration Files

### 🖆 System-Wide Configuration

Ubuntu provides several system-level configuration files.

| File | Purpose |
| --- | --- |
| `/etc/profile` | System-wide login shell configuration |
| `/etc/bash.bashrc` | System-wide interactive Bash configuration |
| `/etc/environment` | System-wide environment variable assignments |
| `/etc/profile.d/` | Additional login configuration scripts |

### 🖆 User-Level Configuration

Important files in the user's home directory:

| File | Purpose |
| --- | --- |
| `~/.profile` | User login environment configuration |
| `~/.bash_profile` | Optional Bash login configuration |
| `~/.bash_login` | Alternative Bash login configuration |
| `~/.bashrc` | Interactive Bash configuration |
| `~/.bash_logout` | Login Bash logout commands |
| `~/.inputrc` | Readline configuration |

### 🖆 Understanding `.bashrc`

The `.bashrc` file is commonly used to customize interactive Bash sessions.

Open it:

```bash
nano ~/.bashrc
```

Example configuration:

```bash
# Custom aliases
alias ll='ls -lah'
alias gs='git status'

# Custom environment variables
export EDITOR="nano"

# Custom prompt
PS1='\u@\h:\w\$ '
```

Apply changes:

```bash
source ~/.bashrc
```

### 🖆 Understanding `.profile`

The `.profile` file is commonly used for login-session initialization.

Example:

```bash
# Add personal executable directory to PATH
if [ -d "$HOME/bin" ]; then
    PATH="$HOME/bin:$PATH"
fi

export PATH
```

Bash reads only the first existing file among `~/.bash_profile`, `~/.bash_login`, and `~/.profile` for its user-level login startup configuration.

# 🗁 Bash Startup Process

### 🖆 Login Interactive Shell

A login interactive Bash shell processes system and user login configuration.

The general startup sequence includes:

```text
/etc/profile
      |
      v
First available user login file:
~/.bash_profile
~/.bash_login
~/.profile
```

The selected user login file may explicitly load `~/.bashrc`.

### 🖆 Interactive Non-Login Shell

An interactive non-login Bash shell normally reads:

```text
/etc/bash.bashrc
       |
       v
~/.bashrc
```

### 🖆 Non-Interactive Shell

A non-interactive Bash shell normally checks the `BASH_ENV` environment variable.

If it is set, Bash expands its value and reads the referenced file before executing the script or command.

Example:

```bash
export BASH_ENV="$HOME/.bash_env"
bash script.sh
```

The referenced file can contain environment initialization required by non-interactive Bash processes.

### 🖆 Startup File Summary

| Shell Type | Startup Configuration |
| --- | --- |
| Login interactive | `/etc/profile`, first available user login file |
| Interactive non-login | `~/.bashrc` and applicable system configuration |
| Non-interactive | File specified by `BASH_ENV`, when set |
| Login logout | `~/.bash_logout`, when applicable |

# 🗁 Bash Variables

### 🖆 Creating Variables

Bash variables store values within the current shell environment.

```bash
name="Muhammad"
project="Hands-on-Notes"
version="1.0"
```

Display values:

```bash
echo "$name"
echo "$project"
```

### 🖆 Variable Naming Rules

Valid variable names generally:

* Begin with a letter or underscore.
* Contain letters, numbers, and underscores.
* Do not contain spaces.
* Are case-sensitive.

Examples:

```bash
username="admin"
APP_ENV="development"
_file_path="/tmp/example.txt"
```

### 🖆 Reading Variables

```bash
echo "$username"
printf '%s\n' "$APP_ENV"
```

Use braces when separating a variable name from adjacent characters:

```bash
project="notes"

echo "${project}_backup"
```

### 🖆 Read-Only Variables

Use `readonly` to prevent reassignment:

```bash
readonly APP_NAME="Hands-on-Notes"
```

Attempting to modify it causes an error.

### 🖆 Removing Variables

Use `unset`:

```bash
name="Muhammad"

unset name
```

Verify:

```bash
echo "$name"
```

# 🗁 Environment Variables

### 🖆 What Are Environment Variables?

Environment variables are values exported by a process and inherited by its child processes.

Create and export a variable:

```bash
export APP_ENV="development"
```

Display it:

```bash
printenv APP_ENV
```

### 🖆 Common Bash Environment Variables

| Variable | Description |
| --- | --- |
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | Configured login shell |
| `PATH` | Executable search directories |
| `PWD` | Current working directory |
| `OLDPWD` | Previous working directory |
| `LANG` | Locale setting |
| `TERM` | Terminal type |
| `EDITOR` | Preferred text editor |
| `BASH_VERSION` | Bash version |
| `BASH` | Path to the current Bash executable |
| `BASH_ENV` | Startup file used by non-interactive Bash |

### 🖆 Exporting Variables

```bash
APP_ENV="production"

export APP_ENV
```

Or in one statement:

```bash
export APP_ENV="production"
```

### 🖆 Removing Environment Variables

```bash
unset APP_ENV
```

Remove an exported variable from the current shell environment:

```bash
unset APP_ENV
```

Changes affect the current shell and its future child processes, not already-running processes.

# 🗁 Bash Built-in Commands

### 🖆 What Are Bash Built-ins?

Built-in commands are implemented directly inside Bash rather than being separate executable programs.

They can interact directly with the current shell process.

Inspect a command:

```bash
type cd
type export
type read
```

### 🖆 Common Bash Built-ins

| Built-in | Purpose |
| --- | --- |
| `cd` | Change working directory |
| `pwd` | Display working directory |
| `echo` | Display text |
| `printf` | Format output |
| `read` | Read user input |
| `export` | Export variables |
| `unset` | Remove variables |
| `readonly` | Define read-only variables |
| `alias` | Define command aliases |
| `unalias` | Remove aliases |
| `source` | Execute a file in the current shell |
| `eval` | Parse and execute a command string |
| `type` | Identify command types |
| `declare` | Declare variables and attributes |
| `local` | Declare function-local variables |
| `return` | Return from a function |
| `exit` | Exit the shell |
| `jobs` | Display shell jobs |
| `fg` | Bring a job to the foreground |
| `bg` | Resume a job in the background |
| `wait` | Wait for child processes |

### 🖆 Inspecting Built-ins

List Bash built-ins:

```bash
enable -a
```

Display information about a built-in:

```bash
help cd
```

Display help for `read`:

```bash
help read
```

# 🗁 Bash Functions

### 🖆 Defining Functions

Functions group commands into reusable units.

```bash
greet() {
    echo "Hello, Muhammad!"
}
```

Execute:

```bash
greet
```

### 🖆 Function Arguments

Bash functions support positional parameters.

```bash
greet() {
    echo "Hello, $1!"
}

greet Muhammad
```

Output:

```text
Hello, Muhammad!
```

### 🖆 Returning Values

Use `return` to provide an exit status.

```bash
check_directory() {
    if [ -d "$1" ]; then
        return 0
    else
        return 1
    fi
}

if check_directory "/tmp"; then
    echo "Directory exists"
else
    echo "Directory does not exist"
fi
```

Bash functions return integer exit statuses, not arbitrary strings. Use `printf` or command substitution when a function needs to produce textual output.

### 🖆 Local Variables

Use `local` to limit a variable's scope to a function.

```bash
greet() {
    local name="Muhammad"
    echo "Hello, $name"
}

greet
```

The `name` variable is local to the function.

### 🖆 Listing Functions

Display declared functions:

```bash
declare -F
```

Display a function definition:

```bash
declare -f greet
```

# 🗁 Aliases

### 🖆 Creating Aliases

```bash
alias ll='ls -lah'
alias gs='git status'
alias ..='cd ..'
```

### 🖆 Listing Aliases

```bash
alias
```

### 🖆 Removing Aliases

```bash
unalias gs
```

### 🖆 Persistent Aliases

Add aliases to:

```text
~/.bashrc
```

Reload:

```bash
source ~/.bashrc
```

Aliases are generally expanded in interactive shells when alias expansion is enabled. Functions are usually more suitable for reusable logic and parameterized operations.

# 🗁 Bash Arrays

### 🖆 Indexed Arrays

Bash supports indexed arrays.

Declare an array:

```bash
fruits=("Apple" "Banana" "Orange")
```

Access an element:

```bash
echo "${fruits[0]}"
```

Bash arrays use zero-based indexing.

### 🖆 Accessing All Elements

```bash
printf '%s\n' "${fruits[@]}"
```

Get the number of elements:

```bash
echo "${#fruits[@]}"
```

Get array indices:

```bash
echo "${!fruits[@]}"
```

### 🖆 Adding Elements

```bash
fruits+=("Mango")
```

Display the updated array:

```bash
printf '%s\n' "${fruits[@]}"
```

### 🖆 Associative Arrays

Associative arrays store key-value pairs.

Declare one:

```bash
declare -A user

user[name]="Muhammad"
user[role]="Developer"
user[language]="Bash"
```

Access values:

```bash
echo "${user[name]}"
echo "${user[role]}"
```

Display all keys:

```bash
echo "${!user[@]}"
```

Display all values:

```bash
echo "${user[@]}"
```

# 🗁 Bash Parameters

### 🖆 Positional Parameters

Bash provides special parameters for accessing command-line arguments.

| Parameter | Meaning |
| --- | --- |
| `$0` | Script or shell name |
| `$1` | First positional argument |
| `$2` | Second positional argument |
| `${10}` | Tenth positional argument |
| `$#` | Number of positional arguments |
| `$@` | All positional arguments |
| `$*` | All positional arguments as a single expansion, subject to quoting |
| `$?` | Exit status of the most recent command |
| `$$` | Current shell process ID |
| `$!` | PID of the most recently backgrounded asynchronous command |
| `$-` | Current shell option flags |

### 🖆 Using Positional Parameters

Create `arguments.sh`:

```bash
#!/usr/bin/env bash

echo "Script: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Argument count: $#"
```

Execute:

```bash
bash arguments.sh Ubuntu Linux
```

### 🖆 Understanding `$@` and `$*`

Quoted `"$@"` preserves each positional argument as a separate word.

Quoted `"$*"` combines all positional arguments into a single word, separated by the first character of `IFS`.

Example:

```bash
#!/usr/bin/env bash

printf 'Argument: %s\n' "$@"
```

Run:

```bash
bash arguments.sh "Ubuntu Linux" Bash
```

Output:

```text
Argument: Ubuntu Linux
Argument: Bash
```

For forwarding arguments to another command or function, `"$@"` is generally the correct choice.

# 🗁 Bash Expansion Features

### 🖆 Parameter Expansion

```bash
name="Muhammad"

echo "${name}"
echo "${name:-Guest}"
```

Provide a default value when a variable is unset or empty:

```bash
echo "${APP_ENV:-development}"
```

Provide a default only when a variable is unset:

```bash
echo "${APP_ENV- development}"
```

### 🖆 Command Substitution

Capture command output:

```bash
current_date=$(date)

echo "$current_date"
```

### 🖆 Arithmetic Expansion

```bash
a=10
b=20

result=$((a + b))

echo "$result"
```

### 🖆 Brace Expansion

```bash
echo file{1,2,3}.txt
```

Generate directory names:

```bash
mkdir -p project/{src,tests,docs}
```

### 🖆 Process Substitution

```bash
diff <(sort file1.txt) <(sort file2.txt)
```

Process substitution allows command output to be used where a filename-like argument is expected.

# 🗁 Bash Options

### 🖆 Understanding Shell Options

Bash options control shell behavior.

Display current shell options:

```bash
set -o
```

Display `shopt` options:

```bash
shopt
```

### 🖆 Enabling and Disabling Options

Enable an option:

```bash
set -o noclobber
```

Disable it:

```bash
set +o noclobber
```

The `noclobber` option prevents ordinary output redirection from overwriting an existing regular file.

### 🖆 Useful Bash Options

| Option | Purpose |
| --- | --- |
| `-e` | Exit when an unhandled command fails in applicable contexts |
| `-u` | Treat unset variable expansions as errors |
| `-x` | Display commands and expansions during execution |
| `-n` | Read commands without executing them |
| `-o pipefail` | Return a pipeline failure status when a command in the pipeline fails |

Example:

```bash
set -euo pipefail
```

These options are useful in scripts, but their behavior has important exceptions and context-dependent rules. In particular, `set -e` does not behave like a universal exception handler.

### 🖆 Using `shopt`

Enable an option:

```bash
shopt -s globstar
```

Disable it:

```bash
shopt -u globstar
```

With `globstar` enabled, `**` can match files and directories recursively during filename expansion.

Example:

```bash
ls **/*.txt
```

# 🗁 Bash Job Control

### 🖆 What Is Job Control?

Job control allows Bash to manage foreground and background jobs within an interactive shell.

Start a background process:

```bash
sleep 300 &
```

Display current jobs:

```bash
jobs
```

### 🖆 Foreground Jobs

A foreground job occupies the terminal while it executes.

Run:

```bash
sleep 60
```

Suspend it:

```text
Ctrl + Z
```

### 🖆 Background Jobs

Resume a suspended job in the background:

```bash
bg
```

Bring it back to the foreground:

```bash
fg
```

### 🖆 Job Identifiers

Display jobs with identifiers:

```bash
jobs -l
```

Bring a specific job to the foreground:

```bash
fg %1
```

Resume a specific job in the background:

```bash
bg %1
```

### 🖆 Waiting for Background Jobs

Wait for a background job:

```bash
sleep 5 &
wait
```

Wait for a specific process:

```bash
sleep 5 &
pid=$!

wait "$pid"
```

`wait` is a Bash built-in that waits for asynchronous child processes and retrieves their exit statuses.

# 🗁 Bash History

### 🖆 Displaying Command History

```bash
history
```

Display recent commands:

```bash
history 20
```

Search for commands:

```bash
history | grep docker
```

### 🖆 Reusing Commands

Execute the previous command:

```bash
!!
```

Execute a command by history number:

```bash
!15
```

Execute the most recent command beginning with a particular string:

```bash
!git
```

### 🖆 Reverse History Search

Press:

```text
Ctrl + R
```

Enter a search term to locate previous commands.

### 🖆 History Configuration

Bash history is commonly stored in:

```text
~/.bash_history
```

Inspect related variables:

```bash
echo "$HISTFILE"
echo "$HISTSIZE"
echo "$HISTFILESIZE"
```

Configure history limits:

```bash
HISTSIZE=5000
HISTFILESIZE=10000
```

# 🗁 Bash Completion

### 🖆 What Is Bash Completion?

Bash completion helps users complete commands, paths, filenames, and command-specific arguments using the keyboard.

Press:

```text
Tab
```

For example, type:

```bash
cd /usr/lo
```

Press `Tab` to complete the directory name when the match is unambiguous.

### 🖆 Installing Bash Completion on Ubuntu

Install the package:

```bash
sudo apt update
sudo apt install bash-completion
```

The package provides programmable completion support for many common commands.

### 🖆 Enabling Completion

Ubuntu commonly configures Bash completion through system-wide shell configuration.

Check whether it is available:

```bash
type _init_completion
```

Inspect the package:

```bash
dpkg -L bash-completion
```

If your shell configuration does not load it automatically, you can source the completion script from `~/.bashrc`:

```bash
if [ -r /usr/share/bash-completion/bash_completion ]; then
    source /usr/share/bash-completion/bash_completion
fi
```

Reload the configuration:

```bash
source ~/.bashrc
```

### 🖆 Programmable Completion

Bash supports custom completion functions.

A completion function can generate possible values for a command.

Example:

```bash
_complete_environment() {
    COMPREPLY=( $(compgen -W "development staging production" -- "${COMP_WORDS[COMP_CWORD]}") )
}

complete -F _complete_environment deploy
```

After defining the completion, pressing `Tab` after `deploy` can suggest the available environment names.

# 🗁 Bash Debugging

### 🖆 Syntax Checking

Check a script without executing it:

```bash
bash -n script.sh
```

### 🖆 Execution Tracing

Execute a script with tracing enabled:

```bash
bash -x script.sh
```

### 🖆 Debugging with `set -x`

Enable tracing:

```bash
set -x
```

Disable tracing:

```bash
set +x
```

### 🖆 Displaying Line Numbers

Bash provides the `LINENO` variable inside scripts and functions.

Example:

```bash
#!/usr/bin/env bash

echo "Current line: $LINENO"
```

### 🖆 Using `PS4`

The `PS4` variable controls the prefix displayed by execution tracing.

Example:

```bash
PS4='+ ${BASH_SOURCE}:${LINENO}: '

set -x

echo "Debugging Bash"

set +x
```

This makes traced command output easier to associate with the source file and line number.

# 🗁 Bash Scripting Features

### 🖆 Conditional Statements

Bash supports conditional execution using `if`, `elif`, and `else`.

```bash
#!/usr/bin/env bash

environment="production"

if [ "$environment" = "production" ]; then
    echo "Production environment"
else
    echo "Non-production environment"
fi
```

### 🖆 Loops

Bash supports `for`, `while`, and `until` loops.

Example:

```bash
for name in Ubuntu Debian Fedora; do
    echo "$name"
done
```

### 🖆 Functions

```bash
greet() {
    local name="$1"
    printf 'Hello, %s!\n' "$name"
}

greet "Muhammad"
```

### 🖆 Reading User Input

```bash
read -r -p "Enter your username: " username

printf 'Welcome, %s!\n' "$username"
```

### 🖆 Script Arguments

```bash
#!/usr/bin/env bash

echo "Script name: $0"
echo "First argument: $1"
echo "Total arguments: $#"
```

Execute:

```bash
bash script.sh Ubuntu
```

These features are explored individually in the following Shell roadmap chapters.

# 🗁 Bash vs POSIX Shell

### 🖆 What Is POSIX Shell?

POSIX defines standards for portable operating-system interfaces, including a standard shell command language.

The POSIX shell language provides a common foundation for shell scripts across Unix-like systems.

On Ubuntu, `/bin/sh` is commonly provided by `dash`, not Bash.

Check:

```bash
ls -l /bin/sh
```

### 🖆 Bash-Specific Features

Bash provides features beyond the POSIX shell language, including:

* Indexed arrays.
* Associative arrays.
* `[[ ... ]]` conditional expressions.
* `(( ... ))` arithmetic commands.
* Process substitution.
* Extended parameter expansion.
* `select` loops.
* `shopt` options.
* `mapfile` and `readarray`.
* `source` as a command (in addition to the POSIX `.` command).

### 🖆 Bash vs POSIX Shell Comparison

| Feature | Bash | POSIX Shell |
| --- | --- | --- |
| Variables | Supported | Supported |
| Functions | Supported | Supported |
| Conditions | Supported | Supported |
| Loops | Supported | Supported |
| Pipelines | Supported | Supported |
| Redirection | Supported | Supported |
| Indexed arrays | Supported | Not standardized |
| Associative arrays | Supported | Not standardized |
| `[[ ... ]]` | Supported | Not standardized |
| Process substitution | Supported | Not standardized |
| `shopt` | Supported | Not standardized |

### 🖆 Writing Portable Shell Scripts

For a POSIX-oriented script:

```sh
#!/bin/sh

name="Ubuntu"

printf 'Hello, %s\n' "$name"
```

For a Bash-specific script:

```bash
#!/usr/bin/env bash

declare -A user

user[name]="Muhammad"

echo "${user[name]}"
```

Use the interpreter that matches the language features required by the script.

# 🗁 Best Practices

* Use `#!/usr/bin/env bash` for Bash-specific scripts.
* Keep interactive configuration in `~/.bashrc`.
* Keep login environment configuration in the appropriate login startup file.
* Quote variable expansions when word splitting and filename expansion are not intended.
* Use `printf` for predictable formatted output.
* Use `local` for function-specific variables.
* Use `"$@"` when forwarding positional arguments.
* Use `bash -n` to check syntax before execution.
* Use `bash -x` when debugging.
* Use `set -o pipefail` when pipeline failure detection is required.
* Understand the behavior of `set -e` before relying on it.
* Prefer functions over complicated aliases.
* Avoid unnecessary global variables.
* Use Bash-specific features only when the script explicitly requires Bash.
* Write POSIX-compatible scripts when portability is an actual requirement.

# 🗁 Common Mistakes

### 🖆 Confusing `/bin/sh` with Bash

Ubuntu commonly uses Dash for `/bin/sh`.

A Bash-specific script should identify Bash explicitly:

```bash
#!/usr/bin/env bash
```

### 🖆 Forgetting to Export Variables

Incorrect assumption:

```bash
APP_ENV="production"
```

Child processes do not automatically inherit ordinary unexported shell variables.

Correct:

```bash
export APP_ENV="production"
```

### 🖆 Editing the Wrong Configuration File

Adding interactive aliases to `~/.profile` may not work as expected in ordinary non-login terminal sessions.

Use `~/.bashrc` for interactive Bash configuration.

### 🖆 Using Global Variables Unnecessarily

Prefer local variables inside functions:

```bash
calculate() {
    local result=100
    echo "$result"
}
```

### 🖆 Misusing `set -e`

`set -e` does not terminate a script after every possible non-zero exit status.

Its behavior depends on execution context, including conditions, pipelines, and compound commands.

Use explicit error handling when the outcome matters.

### 🖆 Assuming Bash Features Work in `/bin/sh`

Bash arrays, `[[ ... ]]`, and process substitution are not POSIX shell features.

Use the correct interpreter.

# 🗁 Practical Exercises

### 🖆 Bash Environment

* Check your Bash version.
* Locate the Bash executable.
* Display the current shell PID and parent PID.
* Identify whether your current shell is interactive.
* Identify whether it is a login shell.
* Inspect your Bash startup configuration files.

### 🖆 Variables and Functions

* Create several shell variables.
* Export an environment variable.
* Create a read-only variable.
* Write a function that accepts two arguments.
* Use local variables inside the function.
* Return a success or failure status.

### 🖆 Arrays

* Create an indexed array containing five programming languages.
* Display all array elements.
* Display the number of elements.
* Add another element.
* Create an associative array representing a user.
* Display its keys and values.

### 🖆 Bash Options and Debugging

* Display available shell options.
* Enable and disable `noclobber`.
* Enable `globstar`.
* Check a script using `bash -n`.
* Trace script execution using `bash -x`.
* Customize the `PS4` debugging prefix.

### 🖆 Job Control and History

* Start a background process.
* Display active jobs.
* Suspend a foreground process.
* Resume it using `bg`.
* Bring it back using `fg`.
* Search command history.
* Execute a previous command using history expansion.

### 🖆 Bash Completion

* Install `bash-completion`.
* Inspect its installed files.
* Test command and filename completion.
* Create a custom completion function for a command accepting `development`, `staging`, and `production`.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

* What is Bash?
* What does Bash stand for?
* What is the difference between Bash and the traditional Bourne shell?
* How does Bash execute commands?
* What is the difference between interactive and non-interactive Bash?
* What is the difference between login and non-login Bash?

### 🖆 Configuration Questions

* What is the purpose of `~/.bashrc`?
* What is the purpose of `~/.profile`?
* What is the difference between `/etc/profile` and `/etc/bash.bashrc`?
* What is the `BASH_ENV` variable?
* How does Bash process startup files?

### 🖆 Programming Questions

* What are Bash arrays?
* What is the difference between indexed and associative arrays?
* What is the purpose of `local`?
* What is the difference between `return` and `exit`?
* What is the difference between `$@` and `$*`?
* What are positional parameters?
* What is process substitution?

### 🖆 Advanced Questions

* What is the difference between Bash and POSIX shell?
* Why is `/bin/sh` commonly linked to Dash on Ubuntu?
* What does `set -euo pipefail` do?
* What is the difference between `set` and `shopt`?
* How does Bash job control work?
* What is programmable completion?
* How can you debug a Bash script?
* How does Bash handle environment variables?

# 🗁 References

* [GNU Bash Manual](https://www.gnu.org/software/bash/manual/)
* [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/index.html)
* [Bash Built-in Commands](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html)
* [Bash Shell Variables](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html)
* [Bash Arrays](https://www.gnu.org/software/bash/manual/html_node/Arrays.html)
* [Bash Startup Files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)
* [Bash Job Control](https://www.gnu.org/software/bash/manual/html_node/Job-Control.html)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
* [POSIX Shell Command Language](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/V3_chap02.html)
* [ShellCheck](https://www.shellcheck.net/)
