# Bash Scripting in Linux

Bash scripting is the process of writing a sequence of shell commands in a file so they can be executed together as a program.

Bash scripts are widely used in Ubuntu/Linux for system administration, automation, application deployment, backups, monitoring, software installation, and development workflows.

A Bash script can combine commands, variables, conditions, loops, functions, input/output operations, and error handling to automate repetitive tasks.

# 🗁 Bash Scripting Fundamentals

A Bash script is a text file containing commands interpreted by the Bash shell.

Instead of manually executing commands one by one, you can place them in a script and execute them together.

### 🖆 Why Use Bash Scripts?

* **Automation:** Automate repetitive Linux tasks.
* **System administration:** Manage users, permissions, processes, and services.
* **Deployment:** Automate application installation and deployment.
* **Monitoring:** Collect system information and monitor resources.
* **Backup:** Automate file and directory backups.
* **Development:** Simplify development workflows and project setup.
* **Integration:** Connect multiple Linux utilities into one workflow.

### 🖆 Bash Script Execution Flow

```text
        Bash Script
             |
             v
      Bash Interpreter
             |
             v
       Read Commands
             |
             v
      Execute Commands
             |
             v
      Produce Results
             |
             v
       Exit Status
```

### 🖆 Bash vs. Shell

A shell is a command interpreter that provides an interface between users and the operating system.

Bash is one specific shell implementation.

| Shell | Description |
| --- | --- |
| Bash | Bourne Again Shell |
| Zsh | Z Shell |
| Dash | Lightweight POSIX-oriented shell |
| Fish | Friendly Interactive Shell |
| Ksh | KornShell |

This chapter focuses on Bash scripting in Ubuntu/Linux.

# 🗁 Creating a Bash Script

A Bash script is usually saved with the `.sh` extension.

The extension is a naming convention, not a requirement.

### 🖆 Create a Script File

Create a directory for your scripts:

```bash
mkdir -p ~/scripts
```

Navigate into it:

```bash
cd ~/scripts
```

Create a script:

```bash
touch hello.sh
```

Open it with a terminal editor:

```bash
nano hello.sh
```

### 🖆 Write Your First Script

Add the following content:

```bash
#!/usr/bin/env bash

echo "Hello, Linux!"
echo "Welcome to Bash scripting."
```

Save the file.

### 🖆 Understand the Script

| Statement | Purpose |
| --- | --- |
| `#!/usr/bin/env bash` | Selects the Bash interpreter |
| `echo` | Prints text |
| `#` | Starts a comment outside quoted strings |

The script contains two commands that execute sequentially.

# 🗁 The Shebang

The shebang is a special first line that identifies the interpreter used to execute a script directly.

### 🖆 Shebang Syntax

```bash
#!/path/to/interpreter
```

For Bash:

```bash
#!/usr/bin/env bash
```

This asks the environment to locate Bash through the current `PATH`.

### 🖆 Alternative Shebang

Ubuntu commonly provides Bash at:

```bash
#!/bin/bash
```

Both forms are valid.

The `/usr/bin/env` form is useful when the interpreter may be installed in different locations.

### 🖆 Why the Shebang Matters

When you execute a script directly:

```bash
./hello.sh
```

The operating system uses the shebang to determine which interpreter should execute it.

Without a valid shebang, direct execution may fail or use an unintended interpreter.

### 🖆 Bash Script vs. POSIX Shell Script

A Bash script can use Bash-specific features such as:

* Indexed and associative arrays.
* `[[ ... ]]` conditional expressions.
* `(( ... ))` arithmetic evaluation.
* Here strings (`<<<`).
* Process substitution.
* Extended parameter expansion.

A script using these features should explicitly select Bash rather than assuming `/bin/sh` supports them.

# 🗁 Executing Bash Scripts

There are several ways to execute a Bash script.

### 🖆 Execute with Bash

```bash
bash hello.sh
```

Bash reads and executes the file.

This does not require the executable permission bit.

### 🖆 Execute with the Shebang

First, make the script executable:

```bash
chmod +x hello.sh
```

Then execute it:

```bash
./hello.sh
```

The operating system uses the script's shebang.

### 🖆 Execute with an Absolute Path

```bash
/home/user/scripts/hello.sh
```

This works when the script has executable permission and a valid shebang.

### 🖆 Execute a Script from Another Directory

```bash
~/scripts/hello.sh
```

The `~` character represents the current user's home directory.

### 🖆 Execute a Script in the Current Shell

Use `source`:

```bash
source hello.sh
```

Or:

```bash
. hello.sh
```

Unlike normal script execution, `source` executes the script in the current shell environment.

This means variable assignments, function definitions, and other shell changes can remain available after execution.

# 🗁 Comments

Comments explain what a script does and why particular operations are performed.

Bash ignores comments during normal execution.

### 🖆 Single-Line Comments

Use `#` to write comments.

```bash
#!/usr/bin/env bash

# Display a welcome message
echo "Hello, Linux!"
```

### 🖆 Multiple-Line Comments

Bash does not provide a dedicated block-comment syntax.

Use multiple single-line comments:

```bash
# This script displays
# system information
# on Ubuntu Linux.

echo "System Information"
```

### 🖆 Documentation Comments

Add a description at the beginning of complex scripts.

```bash
#!/usr/bin/env bash

# Script: system-info.sh
# Purpose: Display basic system information.
# Platform: Ubuntu Linux
# Shell: Bash

hostname
uname -r
```

# 🗁 Variables and Constants

Variables store values that can be reused throughout a script.

### 🖆 Declare Variables

```bash
username="Muhammad"
role="Software Engineer"
```

Access variables using `$`:

```bash
echo "$username"
echo "$role"
```

Important: Do not put spaces around the assignment operator.

Correct:

```bash
name="Linux"
```

Incorrect:

```bash
name = "Linux"
```

### 🖆 Environment Variables

Environment variables provide configuration values to processes.

Common examples:

| Variable | Purpose |
| --- | --- |
| `$HOME` | User's home directory |
| `$USER` | Current username |
| `$PATH` | Executable search paths |
| `$PWD` | Current working directory |
| `$SHELL` | User's configured login shell |
| `$HOSTNAME` | System hostname |

Example:

```bash
echo "User: $USER"
echo "Home: $HOME"
echo "Path: $PATH"
```

### 🖆 Export Variables

Use `export` to make a variable available to child processes.

```bash
APP_ENV="development"

export APP_ENV
```

Or:

```bash
export APP_ENV="development"
```

### 🖆 Read-Only Variables

Use `readonly` to prevent reassignment.

```bash
readonly APP_NAME="MyApplication"
```

Attempting to modify a read-only variable produces an error.

### 🖆 Script Variables vs. Environment Variables

| Feature | Shell Variable | Environment Variable |
| --- | --- | --- |
| Available in current shell | Yes | Yes |
| Automatically inherited by child processes | No | Yes |
| Created through assignment | Yes | Yes |
| Common use | Internal script state | Process configuration |

# 🗁 User Input

Bash provides the `read` builtin to receive input from users.

### 🖆 Basic User Input

```bash
#!/usr/bin/env bash

echo "Enter your name:"

read -r username

echo "Hello, $username!"
```

The `-r` option prevents backslash interpretation.

### 🖆 Input with a Prompt

```bash
read -r -p "Enter your username: " username

echo "Welcome, $username!"
```

### 🖆 Hidden Input

Use `-s` to prevent input from being displayed.

```bash
read -r -s -p "Enter password: " password

echo
echo "Password received."
```

### 🖆 Input with a Timeout

Use `-t` to specify a timeout in seconds.

```bash
if read -r -t 10 -p "Enter your name: " username; then
    echo "Hello, $username!"
else
    echo "Input timed out."
fi
```

### 🖆 Input Validation

```bash
read -r -p "Enter your age: " age

if [[ "$age" =~ ^[0-9]+$ ]]; then
    echo "Your age is $age."
else
    echo "Please enter a valid number."
fi
```

# 🗁 Command-Line Arguments

Command-line arguments allow users to pass values when executing a script.

### 🖆 Positional Parameters

| Parameter | Description |
| --- | --- |
| `$0` | Script name or invocation path |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `${10}` | Tenth argument |
| `$#` | Number of arguments |
| `"$@"` | All arguments, preserved individually |
| `"$*"` | All arguments combined into one word |
| `$?` | Exit status of the last command |

### 🖆 Access Arguments

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
bash arguments.sh Linux Bash
```

Output:

```text
Script: arguments.sh
First argument: Linux
Second argument: Bash
Argument count: 2
```

### 🖆 Process All Arguments

```bash
#!/usr/bin/env bash

for argument in "$@"; do
    echo "Argument: $argument"
done
```

Execute:

```bash
bash arguments.sh "Ubuntu Linux" "Bash scripting" Git
```

Each argument remains a separate value.

### 🖆 Shift Arguments

The `shift` builtin removes the first positional argument.

```bash
#!/usr/bin/env bash

while [[ "$#" -gt 0 ]]; do
    echo "Processing: $1"
    shift
done
```

This is useful for processing command-line arguments sequentially.

### 🖆 Validate Required Arguments

```bash
#!/usr/bin/env bash

if [[ "$#" -lt 1 ]]; then
    echo "Usage: $0 FILENAME" >&2
    exit 1
fi

filename="$1"

echo "Processing: $filename"
```

### 🖆 Parse Named Options

For more complex scripts, use `while` and `case`.

```bash
#!/usr/bin/env bash

username=""
environment="development"

while [[ "$#" -gt 0 ]]; do
    case "$1" in
        --user)
            username="${2:?Missing username}"
            shift 2
            ;;
        --env)
            environment="${2:?Missing environment}"
            shift 2
            ;;
        -h|--help)
            echo "Usage: $0 --user NAME [--env ENVIRONMENT]"
            exit 0
            ;;
        *)
            echo "Unknown option: $1" >&2
            exit 1
            ;;
    esac
done

echo "Username: $username"
echo "Environment: $environment"
```

Execute:

```bash
./arguments.sh --user muhammad --env production
```

# 🗁 Conditions

Conditional statements allow scripts to make decisions based on expressions and command results.

### 🖆 `if` Statement

```bash
if [[ condition ]]; then
    commands
fi
```

Example:

```bash
#!/usr/bin/env bash

age=25

if [[ "$age" -ge 18 ]]; then
    echo "Adult"
fi
```

### 🖆 `if-else` Statement

```bash
if [[ condition ]]; then
    commands
else
    commands
fi
```

Example:

```bash
if [[ -f "config.txt" ]]; then
    echo "Configuration file exists."
else
    echo "Configuration file not found."
fi
```

### 🖆 `if-elif-else` Statement

```bash
if [[ condition1 ]]; then
    commands
elif [[ condition2 ]]; then
    commands
else
    commands
fi
```

Example:

```bash
score=85

if [[ "$score" -ge 90 ]]; then
    echo "Grade A"
elif [[ "$score" -ge 80 ]]; then
    echo "Grade B"
else
    echo "Grade C"
fi
```

### 🖆 `case` Statement

```bash
case "$variable" in
    pattern1)
        commands
        ;;
    pattern2)
        commands
        ;;
    *)
        default_commands
        ;;
esac
```

Example:

```bash
read -r -p "Enter an option: " option

case "$option" in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    restart)
        echo "Restarting service..."
        ;;
    *)
        echo "Invalid option."
        ;;
esac
```

For more details, see `04-SHELL/05-conditions.md`.

# 🗁 Loops

Loops execute commands repeatedly.

### 🖆 `for` Loop

```bash
for item in Linux Bash Ubuntu; do
    echo "$item"
done
```

### 🖆 C-Style `for` Loop

```bash
for ((i = 1; i <= 5; i++)); do
    echo "Number: $i"
done
```

### 🖆 `while` Loop

```bash
counter=1

while [[ "$counter" -le 5 ]]; do
    echo "$counter"
    ((counter++))
done
```

### 🖆 `until` Loop

```bash
counter=1

until [[ "$counter" -gt 5 ]]; do
    echo "$counter"
    ((counter++))
done
```

### 🖆 Break and Continue

```bash
for ((i = 1; i <= 10; i++)); do
    if (( i == 3 )); then
        continue
    fi

    if (( i == 8 )); then
        break
    fi

    echo "$i"
done
```

For more details, see `04-SHELL/06-loops.md`.

# 🗁 Functions

Functions divide scripts into reusable logical units.

### 🖆 Define a Function

```bash
greet() {
    echo "Hello, Linux!"
}
```

Call it:

```bash
greet
```

### 🖆 Function with Arguments

```bash
greet() {
    echo "Hello, $1!"
}

greet "Muhammad"
```

### 🖆 Function with Exit Status

```bash
check_file() {
    [[ -f "$1" ]]
}

if check_file "config.txt"; then
    echo "File exists."
else
    echo "File not found."
fi
```

### 🖆 Function with Local Variables

```bash
calculate_sum() {
    local first="$1"
    local second="$2"

    echo $((first + second))
}

result=$(calculate_sum 10 20)

echo "Result: $result"
```

For more details, see `04-SHELL/09-functions.md`.

# 🗁 Arrays

Bash supports indexed and associative arrays.

### 🖆 Indexed Arrays

Declare an array:

```bash
languages=("Bash" "Python" "JavaScript")
```

Access an element:

```bash
echo "${languages[0]}"
```

Output:

```text
Bash
```

Access all elements:

```bash
echo "${languages[@]}"
```

Get the array length:

```bash
echo "${#languages[@]}"
```

Add an element:

```bash
languages+=("TypeScript")
```

Loop through an array:

```bash
for language in "${languages[@]}"; do
    echo "$language"
done
```

### 🖆 Associative Arrays

Associative arrays use named keys instead of numeric indexes.

Declare one:

```bash
declare -A user

user[name]="Muhammad"
user[role]="Software Engineer"
user[location]="Bangladesh"
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

Associative arrays require Bash 4 or later, which is available in standard Ubuntu Bash installations.

# 🗁 String Operations

Bash provides parameter expansion and other builtins for manipulating strings.

### 🖆 String Length

```bash
text="Hello Linux"

echo "${#text}"
```

### 🖆 Extract a Substring

```bash
text="Hello Linux"

echo "${text:0:5}"
```

Output:

```text
Hello
```

### 🖆 Replace Text

```bash
text="Hello Linux"

echo "${text/Linux/Ubuntu}"
```

Output:

```text
Hello Ubuntu
```

### 🖆 Remove a Prefix

```bash
filename="application.log"

echo "${filename#*.}"
```

Output:

```text
log
```

### 🖆 Convert Case

```bash
text="hello linux"

echo "${text^^}"
```

Output:

```text
HELLO LINUX
```

Convert to lowercase:

```bash
text="HELLO LINUX"

echo "${text,,}"
```

These case-conversion expansions are Bash features.

# 🗁 Arithmetic Operations

Bash supports integer arithmetic through arithmetic expansion and arithmetic evaluation.

### 🖆 Arithmetic Expansion

```bash
first=10
second=5

sum=$((first + second))
difference=$((first - second))
product=$((first * second))
division=$((first / second))
remainder=$((first % second))

echo "$sum"
echo "$difference"
echo "$product"
echo "$division"
echo "$remainder"
```

### 🖆 Arithmetic Evaluation

```bash
counter=0

((counter++))

echo "$counter"
```

### 🖆 Comparison

```bash
first=10
second=20

if (( first < second )); then
    echo "First is smaller."
fi
```

Bash arithmetic uses integer values. For floating-point calculations, use an appropriate utility such as `awk` or `bc`.

Example:

```bash
echo "scale=2; 10 / 3" | bc
```

# 🗁 Command Substitution

Command substitution captures the output of a command and stores it in a variable or uses it directly in another command.

### 🖆 Modern Syntax

```bash
current_date=$(date)

echo "$current_date"
```

### 🖆 Command Substitution in a Variable

```bash
hostname_value=$(hostname)
kernel_version=$(uname -r)

echo "Hostname: $hostname_value"
echo "Kernel: $kernel_version"
```

### 🖆 Command Substitution in Strings

```bash
echo "Current user: $(whoami)"
echo "Current directory: $(pwd)"
```

### 🖆 Avoid Legacy Backticks

Older syntax:

```bash
current_date=`date`
```

Preferred syntax:

```bash
current_date=$(date)
```

The modern syntax is easier to nest and read.

# 🗁 Exit Status and Error Handling

Exit statuses indicate whether a command or script completed successfully.

By convention:

* `0` means success.
* A non-zero value indicates failure or another status.

### 🖆 Check Exit Status

```bash
mkdir -p backups

if [[ "$?" -eq 0 ]]; then
    echo "Directory created."
else
    echo "Directory creation failed."
fi
```

A clearer approach is to place the command directly inside the condition:

```bash
if mkdir -p backups; then
    echo "Directory created."
else
    echo "Directory creation failed."
fi
```

### 🖆 Exit a Script

Use `exit` to terminate the script with a status.

```bash
if [[ ! -f "config.txt" ]]; then
    echo "Configuration file not found." >&2
    exit 1
fi

echo "Configuration file found."
exit 0
```

### 🖆 Enable Strict Mode

A common starting point for Bash scripts is:

```bash
set -Eeuo pipefail
```

The options provide:

| Option | Behavior |
| --- | --- |
| `-e` | Exits in many cases when a command fails |
| `-E` | Inherits `ERR` traps in functions, command substitutions, and subshell environments |
| `-u` | Treats unset variables as errors in most expansions |
| `-o pipefail` | Makes a pipeline return a failure status if a command in it fails |

Important: `set -e` has context-dependent behavior. It does not guarantee that every failed command immediately terminates a script. Commands used in conditions, parts of certain logical lists, and other contexts have special rules.

### 🖆 Handle Errors with `trap`

The `trap` builtin executes a command when a specified signal or shell event occurs.

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

cleanup() {
    echo "Cleaning up temporary resources..."
}

handle_error() {
    local status=$?

    echo "Script failed with status: $status" >&2
}

trap cleanup EXIT
trap handle_error ERR

echo "Starting script..."

false

echo "This line will not execute."
```

The `EXIT` trap runs when the shell exits. The `ERR` trap runs when a command fails in a context where Bash would trigger it.

### 🖆 Cleanup Temporary Files

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

temp_file=$(mktemp)

cleanup() {
    rm -f -- "$temp_file"
}

trap cleanup EXIT

echo "Temporary data" > "$temp_file"

cat "$temp_file"
```

This ensures that the temporary file is removed when the script exits normally or through many failure paths.

### 🖆 Validate Dependencies

```bash
#!/usr/bin/env bash

require_command() {
    if ! command -v "$1" >/dev/null 2>&1; then
        echo "Required command not found: $1" >&2
        return 1
    fi
}

require_command curl
require_command git

echo "All required commands are available."
```

# 🗁 Debugging Bash Scripts

Debugging helps identify syntax errors, incorrect assumptions, unexpected values, and command failures.

### 🖆 Syntax Checking

Use `bash -n` to check syntax without executing the script.

```bash
bash -n script.sh
```

### 🖆 Execution Tracing

Use `bash -x` to display commands as Bash executes them.

```bash
bash -x script.sh
```

### 🖆 Enable Debugging Inside a Script

```bash
#!/usr/bin/env bash

set -x

name="Muhammad"

echo "Hello, $name"

set +x
```

`set -x` enables execution tracing, and `set +x` disables it.

### 🖆 Print Variable Values

```bash
username="Muhammad"

printf 'DEBUG: username=%s\n' "$username" >&2
```

Sending debugging information to standard error keeps it separate from normal output.

### 🖆 Use ShellCheck

ShellCheck is a static analysis tool for finding common shell scripting problems.

Install it on Ubuntu:

```bash
sudo apt update
sudo apt install shellcheck
```

Analyze a script:

```bash
shellcheck script.sh
```

ShellCheck can identify issues involving:

* Quoting and word splitting.
* Incorrect variable references.
* Unused variables.
* Suspicious command substitutions.
* Portability problems.
* Common Bash scripting mistakes.

### 🖆 Debugging Tools Summary

| Command | Purpose |
| --- | --- |
| `bash -n script.sh` | Check syntax |
| `bash -x script.sh` | Trace execution |
| `set -x` | Enable tracing |
| `set +x` | Disable tracing |
| `shellcheck script.sh` | Perform static analysis |
| `printf` | Display diagnostic values |

# 🗁 Script Organization

A well-organized script separates configuration, functions, validation, and execution logic.

### 🖆 Recommended Script Structure

```bash
#!/usr/bin/env bash

# Script description

set -Eeuo pipefail

# Configuration
APP_NAME="MyApplication"

# Functions
log_message() {
    printf '[%s] %s\n' "$(date '+%F %T')" "$*"
}

check_dependencies() {
    command -v curl >/dev/null 2>&1
}

main() {
    log_message "Starting $APP_NAME"

    if ! check_dependencies; then
        log_message "Required dependencies are missing."
        return 1
    fi

    log_message "All dependencies are available."
}

# Entry point
main "$@"
```

### 🖆 Why Use a `main` Function?

A `main` function separates script initialization from the main execution flow.

Benefits:

* Makes the entry point explicit.
* Keeps top-level code organized.
* Improves readability.
* Makes functions easier to reuse and test.
* Simplifies script maintenance.

### 🖆 Recommended Project Structure

For a larger Bash project:

```text
my-project/
├── README.md
├── bin/
│   └── application.sh
├── config/
│   └── application.conf
├── lib/
│   ├── common.sh
│   └── logging.sh
├── scripts/
│   └── backup.sh
├── logs/
└── tests/
    └── application-test.sh
```

Keep generated logs and temporary files outside source-controlled locations when appropriate.

# 🗁 Practical Bash Scripts

The following examples demonstrate real-world Ubuntu/Linux automation tasks.

### 🖆 Script: Display System Information

Create `system-info.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

echo "===== SYSTEM INFORMATION ====="

echo "Hostname:"
hostname

echo
echo "Current User:"
whoami

echo
echo "Kernel:"
uname -r

echo
echo "Operating System:"
source /etc/os-release
echo "$PRETTY_NAME"

echo
echo "Uptime:"
uptime -p

echo
echo "Memory Usage:"
free -h

echo
echo "Disk Usage:"
df -h /
```

Execute:

```bash
chmod +x system-info.sh
./system-info.sh
```

### 🖆 Script: Create a Backup

Create `backup.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

source_dir="${1:?Usage: $0 SOURCE_DIRECTORY}"

if [[ ! -d "$source_dir" ]]; then
    echo "Directory not found: $source_dir" >&2
    exit 1
fi

backup_dir="$HOME/backups"
timestamp=$(date +%Y%m%d_%H%M%S)

mkdir -p "$backup_dir"

archive="$backup_dir/backup_$timestamp.tar.gz"

tar -czf "$archive" -C "$(dirname "$source_dir")" "$(basename "$source_dir")"

echo "Backup created: $archive"
```

Execute:

```bash
./backup.sh ~/Documents
```

This creates a compressed archive inside `~/backups`.

### 🖆 Script: Check Disk Usage

Create `disk-check.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

threshold=80

usage=$(df -P / | awk 'NR == 2 {gsub(/%/, "", $5); print $5}')

echo "Root filesystem usage: ${usage}%"

if (( usage >= threshold )); then
    echo "WARNING: Disk usage has reached the configured threshold." >&2
    exit 1
else
    echo "Disk usage is within the configured threshold."
fi
```

Execute:

```bash
chmod +x disk-check.sh
./disk-check.sh
```

### 🖆 Script: Check a Service

Create `service-check.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

service="${1:?Usage: $0 SERVICE_NAME}"

if systemctl is-active --quiet "$service"; then
    echo "$service is running."
else
    echo "$service is not running."
    exit 1
fi
```

Execute:

```bash
./service-check.sh ssh
```

### 🖆 Script: Install Multiple Packages

Create `install-packages.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

packages=(
    curl
    git
    wget
    unzip
    tree
)

echo "Updating package lists..."

sudo apt update

echo "Installing packages..."

sudo apt install -y "${packages[@]}"

echo "Installation completed."
```

Execute:

```bash
chmod +x install-packages.sh
./install-packages.sh
```

### 🖆 Script: Interactive Menu

Create `menu.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

show_menu() {
    echo
    echo "===== SYSTEM MENU ====="
    echo "1. Show date"
    echo "2. Show hostname"
    echo "3. Show disk usage"
    echo "4. Show memory usage"
    echo "5. Exit"
}

while true; do
    show_menu

    read -r -p "Choose an option: " choice

    case "$choice" in
        1)
            date
            ;;
        2)
            hostname
            ;;
        3)
            df -h
            ;;
        4)
            free -h
            ;;
        5)
            echo "Goodbye!"
            break
            ;;
        *)
            echo "Invalid option."
            ;;
    esac
done
```

Execute:

```bash
chmod +x menu.sh
./menu.sh
```

### 🖆 Script: Monitor a Process

Create `process-check.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

process="${1:?Usage: $0 PROCESS_NAME}"

if pgrep -x "$process" >/dev/null; then
    echo "$process is running."
else
    echo "$process is not running."
fi
```

Execute:

```bash
./process-check.sh nginx
```

The `pgrep` command searches for processes by name.

### 🖆 Script: Count Files in a Directory

Create `count-files.sh`:

```bash
#!/usr/bin/env bash

set -Eeuo pipefail

directory="${1:-.}"

if [[ ! -d "$directory" ]]; then
    echo "Directory not found: $directory" >&2
    exit 1
fi

count=$(find "$directory" -maxdepth 1 -type f | wc -l)

echo "Files in $directory: $count"
```

Execute:

```bash
./count-files.sh ~/Documents
```

# 🗁 Scheduling Bash Scripts

Linux provides scheduling tools that allow scripts to execute automatically.

### 🖆 Schedule with Cron

Cron executes commands according to a predefined schedule.

Open the current user's crontab:

```bash
crontab -e
```

Cron expression format:

```text
minute hour day-of-month month day-of-week command
```

Example: Execute a backup script every day at 2:00 AM.

```cron
0 2 * * * /home/user/scripts/backup.sh
```

Example: Execute a script every five minutes.

```cron
*/5 * * * * /home/user/scripts/monitor.sh
```

Example: Execute a script every Monday at 8:00 AM.

```cron
0 8 * * 1 /home/user/scripts/weekly-report.sh
```

List scheduled jobs:

```bash
crontab -l
```

Remove the current user's crontab:

```bash
crontab -r
```

### 🖆 Redirect Cron Output to Logs

```cron
0 2 * * * /home/user/scripts/backup.sh >> /home/user/logs/backup.log 2>&1
```

This appends both standard output and standard error to the log file.

### 🖆 Schedule with Systemd Timers

Systemd timers provide another way to schedule scripts.

They are useful when you need service integration, dependency management, logging through the journal, and more flexible scheduling.

Create a service unit:

`/etc/systemd/system/backup.service`

```ini
[Unit]
Description=Daily Backup Script

[Service]
Type=oneshot
ExecStart=/home/user/scripts/backup.sh
```

Create a timer unit:

`/etc/systemd/system/backup.timer`

```ini
[Unit]
Description=Run Backup Daily

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable and start the timer:

```bash
sudo systemctl enable --now backup.timer
```

Check its status:

```bash
systemctl status backup.timer
```

List active timers:

```bash
systemctl list-timers
```

View service logs:

```bash
journalctl -u backup.service
```

# 🗁 Best Practices

* Always use an appropriate Bash shebang, such as `#!/usr/bin/env bash`.
* Use `set -Eeuo pipefail` as a starting point, then account for its behavior in your script's control flow.
* Quote variable expansions, especially paths and user-provided arguments.
* Use `"$@"` when forwarding command-line arguments.
* Validate inputs before performing operations.
* Use functions to organize reusable logic.
* Prefer `local` variables inside functions.
* Use meaningful exit statuses.
* Send error messages to standard error.
* Check whether required commands are available.
* Use `mktemp` for temporary files instead of predictable filenames.
* Register cleanup actions with `trap`.
* Use ShellCheck to detect common scripting problems.
* Test scripts with sample inputs before using them on important data.
* Use absolute paths in scheduled jobs because cron has a limited environment.
* Avoid unnecessary use of `sudo` inside scripts.
* Keep configuration separate from application logic.
* Use descriptive names for variables, functions, and files.
* Document complex operations and assumptions.
* Avoid storing passwords or sensitive credentials directly in scripts.
* Keep scripts readable and maintainable.

# 🗁 Common Mistakes

### 🖆 Missing Execute Permission

Trying to execute a script without executable permission:

```bash
./script.sh
```

You may receive a permission error.

Fix:

```bash
chmod +x script.sh
```

Alternatively, execute it with Bash:

```bash
bash script.sh
```

### 🖆 Incorrect Variable Assignment

Incorrect:

```bash
username = "Muhammad"
```

Correct:

```bash
username="Muhammad"
```

### 🖆 Unquoted Variables

Potentially problematic:

```bash
rm -r $directory
```

Correct:

```bash
rm -r -- "$directory"
```

Quoting protects paths containing spaces and prevents unintended word splitting and pathname expansion.

### 🖆 Using the Wrong Interpreter

A Bash-specific script should not be executed with `sh`:

```bash
sh script.sh
```

Use:

```bash
bash script.sh
```

Or execute it directly with a correct Bash shebang:

```bash
./script.sh
```

### 🖆 Ignoring Command Failures

Potentially problematic:

```bash
mkdir backups
cp files.txt backups/
```

If the directory creation fails, the copy operation may also fail.

Use explicit checks:

```bash
if mkdir -p backups; then
    cp files.txt backups/
else
    echo "Could not create backup directory." >&2
    exit 1
fi
```

### 🖆 Incorrect Argument Handling

Avoid:

```bash
for item in $@; do
    echo "$item"
done
```

Use:

```bash
for item in "$@"; do
    echo "$item"
done
```

### 🖆 Assuming the Current Directory

Cron jobs and other automation tools may execute scripts from a different working directory.

Avoid relying on relative paths for important files.

Resolve paths relative to the script when appropriate:

```bash
SCRIPT_DIR="$(cd -- "$(dirname -- "${BASH_SOURCE[0]}")" && pwd)"
```

### 🖆 Ignoring Exit Status in Pipelines

Without `pipefail`, a pipeline normally reports the exit status of its final command.

Use:

```bash
set -o pipefail
```

when failures in earlier pipeline commands must also be detected.

# 🗁 Practical Projects

Use these projects to combine Bash concepts into complete automation utilities.

### 🖆 Project: Linux System Information Tool

Build a script that displays:

* Hostname.
* Current user.
* Operating system.
* Kernel version.
* CPU information.
* Memory usage.
* Disk usage.
* System uptime.
* Network interfaces.

Requirements:

* Organize each section into a function.
* Use formatted output.
* Handle missing commands gracefully.
* Save an optional report to a file.

### 🖆 Project: Automated Backup Manager

Build a backup utility that:

* Accepts a source directory.
* Creates compressed `.tar.gz` archives.
* Uses timestamps in archive names.
* Stores backups in a configurable directory.
* Validates source paths.
* Logs backup results.
* Removes old backups according to a configurable retention period.

Suggested commands:

```bash
tar
gzip
find
date
mkdir
```

### 🖆 Project: Service Monitoring Tool

Build a service monitoring script that:

* Accepts one or more service names.
* Checks service status using `systemctl`.
* Reports running and inactive services.
* Writes results to a log file.
* Returns a non-zero exit status if any required service is inactive.

### 🖆 Project: Package Installation Manager

Build an installation utility that:

* Accepts package names as arguments.
* Checks whether packages are already installed.
* Updates package metadata.
* Installs missing packages.
* Reports installation results.
* Handles installation failures.

Use Ubuntu's `apt` package manager.

### 🖆 Project: Interactive User Management Tool

Build a menu-driven utility that can:

* Create a Linux user.
* Display user information.
* Check whether a user exists.
* Add a user to a group.
* Display available groups.
* Exit the program.

Use Linux commands such as:

```bash
useradd
id
getent
usermod
groupadd
```

### 🖆 Project: Log Monitoring Utility

Build a script that:

* Accepts a log file path.
* Searches for error messages.
* Counts matching entries.
* Displays the latest matching entries.
* Saves a filtered report.
* Handles missing or unreadable files.

Suggested commands:

```bash
grep
awk
wc
tail
date
```

# 🗁 Interview Questions

### 🖆 Basic Questions

* What is Bash scripting?
* What is a shebang?
* How do you create a Bash script?
* How do you execute a Bash script?
* What is the purpose of `chmod +x`?
* How do you declare variables in Bash?
* How do you receive user input?
* What are positional parameters?
* How do you write comments?
* What is the purpose of `echo` and `printf`?

### 🖆 Intermediate Questions

* What is the difference between `source` and executing a script?
* What is the difference between a shell variable and an environment variable?
* How do you pass arguments to a Bash script?
* What is the difference between `"$@"` and `"$*"`?
* How do you define and call functions?
* How do you work with arrays?
* What is command substitution?
* How do you handle errors in Bash?
* What does `set -euo pipefail` do?
* What is the purpose of `trap`?
* How do you debug a Bash script?
* What is ShellCheck?

### 🖆 Advanced Questions

* How does the operating system determine which interpreter executes a script?
* What is the difference between Bash and POSIX shell scripting?
* How does command substitution affect variable scope?
* How do pipelines affect subshell execution?
* How would you implement a command-line argument parser?
* How would you design reusable Bash function libraries?
* How do you handle temporary files safely?
* How would you implement robust logging and error handling?
* What is the difference between cron and systemd timers?
* How would you troubleshoot a script that works interactively but fails under cron?
* How would you make a backup script reliable and repeatable?
* What techniques would you use to make a Bash script maintainable?

# 🗁 Official References

* [GNU Bash Manual](https://www.gnu.org/software/bash/manual/bash.html)
* [GNU Bash Reference Manual — Shell Scripts](https://www.gnu.org/software/bash/manual/html_node/Shell-Scripts.html)
* [GNU Bash Reference Manual — Shell Functions](https://www.gnu.org/software/bash/manual/html_node/Shell-Functions.html)
* [GNU Bash Reference Manual — Conditional Constructs](https://www.gnu.org/software/bash/manual/html_node/Conditional-Constructs.html)
* [GNU Bash Reference Manual — Shell Expansions](https://www.gnu.org/software/bash/manual/html_node/Shell-Expansions.html)
* [ShellCheck Documentation](https://www.shellcheck.net/)
* [Ubuntu Server Documentation](https://documentation.ubuntu.com/server/)
* [Linux Manual — `crontab(5)`](https://man7.org/linux/man-pages/man5/crontab.5.html)
* [Systemd Timer Documentation](https://www.freedesktop.org/software/systemd/man/latest/systemd.timer.html)
