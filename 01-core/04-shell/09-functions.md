# Bash Functions in Linux

A shell function is a reusable block of commands that performs a specific task. Functions help organize Bash scripts, eliminate duplicated code, improve readability, and make complex automation easier to maintain.

In Ubuntu/Linux, Bash functions are commonly used for system administration, development workflows, automation, and command-line utilities.

# 🗁 Function Fundamentals

A function is a named group of commands that can be executed whenever needed.

Instead of repeating the same commands throughout a script, you can define them once and call the function multiple times.

### 🖆 Why Use Functions?

Functions provide several advantages:

* **Reusability:** Write logic once and execute it multiple times.
* **Maintainability:** Update logic in one place.
* **Readability:** Divide complex scripts into meaningful sections.
* **Modularity:** Separate responsibilities into smaller units.
* **Abstraction:** Hide implementation details behind descriptive function names.
* **Testing:** Make individual pieces of script logic easier to test.

### 🖆 Function Execution Flow

```text
          Bash Script
               |
               v
       Define a Function
               |
               v
       Call the Function
               |
               v
       Execute Its Commands
               |
               v
       Return to Caller
```

Defining a function does not execute its commands. The function runs only when it is called.

# 🗁 Defining Functions

Bash supports two common function declaration syntaxes.

### 🖆 Standard Function Syntax

```bash
function_name() {
    commands
}
```

Example:

```bash
greet() {
    echo "Hello, Linux!"
}
```

### 🖆 Using the `function` Keyword

```bash
function greet {
    echo "Hello, Linux!"
}
```

Both forms are valid in Bash.

The first form is commonly preferred because it is portable across more POSIX-like shells.

### 🖆 Functions with Multiple Commands

```bash
system_info() {
    echo "Hostname:"
    hostname

    echo "Kernel:"
    uname -r

    echo "Current User:"
    whoami
}
```

Each command executes when the function is called.

### 🖆 Function Names

Use descriptive names that communicate the function's purpose.

Good examples:

```bash
check_disk_usage() {
    df -h
}

create_backup() {
    echo "Creating backup..."
}

install_packages() {
    echo "Installing packages..."
}
```

Avoid names that conflict with shell builtins or existing commands unless you intentionally want to override them.

# 🗁 Calling Functions

After defining a function, call it using its name.

### 🖆 Basic Function Call

```bash
greet() {
    echo "Hello, Linux!"
}

greet
```

Output:

```text
Hello, Linux!
```

### 🖆 Calling a Function Multiple Times

```bash
greet() {
    echo "Hello, Linux!"
}

greet
greet
greet
```

Output:

```text
Hello, Linux!
Hello, Linux!
Hello, Linux!
```

### 🖆 Calling Functions in a Script

Create a file named `functions.sh`:

```bash
#!/usr/bin/env bash

show_message() {
    echo "Welcome to Bash functions."
}

show_message
```

Make it executable:

```bash
chmod +x functions.sh
```

Execute it:

```bash
./functions.sh
```

You can also run it through Bash:

```bash
bash functions.sh
```

### 🖆 Function Definition Order

Define a function before calling it.

```bash
greet() {
    echo "Hello!"
}

greet
```

Calling a function before its definition generally produces a command-not-found error because Bash has not registered the function yet.

# 🗁 Function Parameters

Bash functions can receive arguments just like shell scripts.

Arguments are accessed through special positional parameters.

### 🖆 Positional Parameters

| Parameter | Description |
| --- | --- |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `${10}` | Tenth argument |
| `$@` | All arguments as separate words |
| `$*` | All arguments as a single word when quoted |
| `$#` | Number of arguments |
| `$0` | Name of the shell or script context |

### 🖆 Function with One Argument

```bash
greet() {
    echo "Hello, $1!"
}

greet "Muhammad"
```

Output:

```text
Hello, Muhammad!
```

### 🖆 Function with Multiple Arguments

```bash
introduce() {
    echo "Name: $1"
    echo "Role: $2"
    echo "Location: $3"
}

introduce "Muhammad" "Software Engineer" "Bangladesh"
```

Output:

```text
Name: Muhammad
Role: Software Engineer
Location: Bangladesh
```

### 🖆 Accessing All Arguments

```bash
show_arguments() {
    echo "Arguments: $@"
    echo "Argument count: $#"
}

show_arguments "Linux" "Bash" "Ubuntu"
```

Output:

```text
Arguments: Linux Bash Ubuntu
Argument count: 3
```

### 🖆 Safely Handling Arguments with Spaces

Prefer `"$@"` when forwarding arguments.

```bash
print_arguments() {
    for argument in "$@"; do
        printf 'Argument: %s\n' "$argument"
    done
}

print_arguments "Hello Linux" "Bash scripting"
```

Output:

```text
Argument: Hello Linux
Argument: Bash scripting
```

Quoting `"$@"` preserves each argument as an individual value, including arguments containing spaces.

# 🗁 Local and Global Variables

Bash variables created inside functions are generally global unless declared with `local`.

### 🖆 Global Variables

A global variable can be accessed throughout the current shell environment.

```bash
name="Muhammad"

greet() {
    echo "Hello, $name"
}

greet
```

Output:

```text
Hello, Muhammad
```

Variables assigned inside functions without `local` also become available outside the function.

```bash
create_variable() {
    message="Hello from function"
}

create_variable

echo "$message"
```

Output:

```text
Hello from function
```

### 🖆 Local Variables

Use the `local` keyword to restrict a variable to the function's scope.

```bash
greet() {
    local name="Muhammad"

    echo "Hello, $name"
}

greet

echo "$name"
```

The final `echo` prints an empty line if no global `name` variable exists.

### 🖆 Local Variables with Function Parameters

```bash
calculate_total() {
    local price="$1"
    local quantity="$2"

    local total=$((price * quantity))

    echo "Total: $total"
}

calculate_total 100 5
```

Output:

```text
Total: 500
```

### 🖆 Global and Local Variables with the Same Name

A local variable can temporarily shadow a global variable.

```bash
name="Global"

show_name() {
    local name="Local"

    echo "$name"
}

show_name

echo "$name"
```

Output:

```text
Local
Global
```

### 🖆 Read-Only Local Variables

You can declare a local variable as read-only.

```bash
process_data() {
    local -r status="completed"

    echo "$status"
}

process_data
```

The `-r` option prevents reassignment of the variable.

# 🗁 Returning Values

Unlike many programming languages, Bash functions do not directly return arbitrary strings or objects through the `return` keyword.

Bash functions commonly communicate results through standard output or variables.

### 🖆 Using Standard Output

```bash
get_username() {
    echo "Muhammad"
}

username=$(get_username)

echo "$username"
```

Output:

```text
Muhammad
```

The function prints a value, and command substitution captures it.

### 🖆 Returning Numeric Values with `return`

The `return` keyword sets the function's exit status.

```bash
check_number() {
    if [[ "$1" -gt 10 ]]; then
        return 0
    else
        return 1
    fi
}

check_number 15

echo "$?"
```

Output:

```text
0
```

By convention:

* `0` indicates success.
* A non-zero value indicates failure or another status.

The exit status is limited to an unsigned 8-bit value, from `0` to `255`.

### 🖆 Using a Global Variable to Store a Result

```bash
calculate_sum() {
    result=$(("$1" + "$2"))
}

calculate_sum 10 20

echo "$result"
```

Output:

```text
30
```

This works, but it introduces global state. Prefer standard output when a function needs to produce a simple result.

### 🖆 Returning Multiple Values

You can print multiple values and capture them as text.

```bash
get_user_info() {
    printf '%s\n' "Muhammad" "Software Engineer"
}

user_info=$(get_user_info)

printf '%s\n' "$user_info"
```

For structured data, consider using arrays, formatted output, or another suitable data representation.

# 🗁 Function Exit Status

Every Bash command and function has an exit status.

A function's exit status is normally the status of its last executed command unless an explicit `return` is used.

### 🖆 Checking Function Success

```bash
create_directory() {
    mkdir -p "$1"
}

if create_directory "backups"; then
    echo "Directory created successfully."
else
    echo "Directory creation failed."
fi
```

The `if` statement checks the function's exit status.

### 🖆 Explicit Success and Failure

```bash
validate_file() {
    if [[ -f "$1" ]]; then
        echo "File exists."
        return 0
    else
        echo "File does not exist."
        return 1
    fi
}

if validate_file "config.txt"; then
    echo "Validation passed."
else
    echo "Validation failed."
fi
```

### 🖆 Capture an Exit Status

```bash
check_service() {
    systemctl is-active --quiet "$1"
}

check_service ssh

status=$?

echo "Exit status: $status"
```

Use `$?` immediately after the command whose status you want to capture because subsequent commands overwrite it.

# 🗁 Command Substitution with Functions

Command substitution captures the standard output produced by a function.

### 🖆 Basic Command Substitution

```bash
get_date() {
    date +%Y-%m-%d
}

today=$(get_date)

echo "Today: $today"
```

### 🖆 Using Function Output in Another Command

```bash
get_hostname() {
    hostname
}

echo "Server: $(get_hostname)"
```

### 🖆 Important Behavior

Command substitution runs in a subshell environment.

Consequently, variable changes made inside the function call are not available in the calling shell.

```bash
set_value() {
    result="Hello"
}

value=$(set_value)

echo "$result"
```

The variable `result` is not assigned in the parent shell by this command substitution.

To modify the current shell's variables, call the function directly instead:

```bash
set_value() {
    result="Hello"
}

set_value

echo "$result"
```

# 🗁 Default Arguments

Bash does not provide built-in default parameter declarations like many programming languages.

You can implement defaults using parameter expansion.

### 🖆 Using `${parameter:-default}`

```bash
greet() {
    local name="${1:-Guest}"

    echo "Hello, $name!"
}

greet
greet "Muhammad"
```

Output:

```text
Hello, Guest!
Hello, Muhammad!
```

The `:-` operator uses the default when the variable is unset or empty.

### 🖆 Using `${parameter-default}`

```bash
greet() {
    local name="${1-Guest}"

    echo "Hello, $name!"
}
```

The `-` operator uses the default only when the parameter is unset.

If the argument exists but is empty, it remains empty.

### 🖆 Multiple Default Arguments

```bash
create_user() {
    local username="${1:-guest}"
    local role="${2:-viewer}"
    local shell="${3:-/bin/bash}"

    echo "Username: $username"
    echo "Role: $role"
    echo "Shell: $shell"
}

create_user "developer" "admin"
```

Output:

```text
Username: developer
Role: admin
Shell: /bin/bash
```

# 🗁 Variable Number of Arguments

A function can accept any number of arguments.

This is useful when creating reusable utilities that process lists of files, packages, or values.

### 🖆 Loop Through All Arguments

```bash
show_items() {
    for item in "$@"; do
        echo "Item: $item"
    done
}

show_items "Ubuntu" "Debian" "Fedora"
```

### 🖆 Validate Argument Count

```bash
add_numbers() {
    if [[ "$#" -lt 2 ]]; then
        echo "Usage: add_numbers NUMBER1 NUMBER2" >&2
        return 1
    fi

    echo $(("$1" + "$2"))
}

add_numbers 10 20
```

### 🖆 Shift Positional Parameters

The `shift` builtin removes the first positional parameter and moves the remaining arguments one position to the left.

```bash
process_arguments() {
    while [[ "$#" -gt 0 ]]; do
        echo "Processing: $1"
        shift
    done
}

process_arguments "file1.txt" "file2.txt" "file3.txt"
```

Output:

```text
Processing: file1.txt
Processing: file2.txt
Processing: file3.txt
```

This is especially useful when implementing command-line argument parsers.

# 🗁 Recursive Functions

A recursive function calls itself.

Recursion can solve problems that naturally divide into smaller versions of the same problem.

### 🖆 Factorial Example

The factorial of a positive integer is the product of all positive integers up to that number.

```bash
factorial() {
    local number="$1"

    if (( number <= 1 )); then
        echo 1
        return
    fi

    local previous
    previous=$(factorial "$((number - 1))")

    echo $((number * previous))
}

factorial 5
```

Output:

```text
120
```

### 🖆 Recursion Fundamentals

Every recursive function needs:

* A base case that stops recursion.
* A recursive case that moves toward the base case.
* A way to combine or use the results.

Without a base case, recursion can continue until the shell reaches resource or execution limits.

For large calculations, iterative solutions are often more efficient in shell scripts.

# 🗁 Function Scope and Subshells

Bash functions normally execute in the current shell environment. However, certain execution contexts create subshells.

### 🖆 Function in the Current Shell

```bash
set_value() {
    result="Hello"
}

set_value

echo "$result"
```

Output:

```text
Hello
```

The variable remains available because the function executed in the current shell.

### 🖆 Function in a Subshell

```bash
set_value() {
    result="Hello"
}

(set_value)

echo "$result"
```

The function executes inside a subshell. Its variable assignment does not affect the parent shell.

### 🖆 Function Executed Through a Pipeline

```bash
count=0

increment() {
    count=$((count + 1))
}

increment | cat

echo "$count"
```

In a typical Bash pipeline, commands execute in subshell environments, so changes made by a pipeline component generally do not affect the parent shell.

Bash has a `lastpipe` option that can allow the final pipeline command to execute in the current shell under specific conditions, particularly when job control is disabled.

### 🖆 Exported Functions

Bash supports exporting functions to child Bash processes.

```bash
greet() {
    echo "Hello from Bash"
}

export -f greet
```

A child Bash process can then access the exported function:

```bash
bash -c 'greet'
```

Exported functions are Bash-specific and should be used deliberately when passing functions between shell processes.

# 🗁 Function Libraries

A function library is a file containing reusable functions that can be loaded into another Bash script.

Bash uses the `source` command or its `.` equivalent to execute another file in the current shell environment.

### 🖆 Create a Function Library

Create `functions.sh`:

```bash
#!/usr/bin/env bash

greet() {
    echo "Hello, $1!"
}

get_system_info() {
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
}

is_root() {
    [[ "$EUID" -eq 0 ]]
}
```

### 🖆 Load a Function Library

Create `main.sh`:

```bash
#!/usr/bin/env bash

source ./functions.sh

greet "Muhammad"

get_system_info

if is_root; then
    echo "Running as root."
else
    echo "Running as a regular user."
fi
```

Execute:

```bash
chmod +x main.sh
./main.sh
```

You can also use the equivalent syntax:

```bash
. ./functions.sh
```

### 🖆 Using an Absolute Path

For scripts that may run from different working directories, use a path relative to the script itself.

```bash
#!/usr/bin/env bash

SCRIPT_DIR="$(cd -- "$(dirname -- "${BASH_SOURCE[0]}")" && pwd)"

source "$SCRIPT_DIR/functions.sh"

greet "Muhammad"
```

This avoids depending on the directory from which the script was launched.

### 🖆 Prevent Duplicate Library Loading

A library can use a guard variable to avoid defining its functions multiple times.

```bash
if [[ -n "${MY_FUNCTIONS_LOADED:-}" ]]; then
    return 0
fi

MY_FUNCTIONS_LOADED=1

greet() {
    echo "Hello, $1!"
}
```

This pattern is useful when multiple scripts or libraries may source the same file.

# 🗁 Useful Function Techniques

### 🖆 Check Whether a Command Exists

```bash
command_exists() {
    command -v "$1" >/dev/null 2>&1
}

if command_exists git; then
    echo "Git is installed."
else
    echo "Git is not installed."
fi
```

### 🖆 Check Whether a User Is Root

```bash
require_root() {
    if [[ "$EUID" -ne 0 ]]; then
        echo "This operation requires root privileges." >&2
        return 1
    fi
}

if require_root; then
    echo "Root privileges confirmed."
fi
```

### 🖆 Check Whether a File Exists

```bash
file_exists() {
    [[ -f "$1" ]]
}

if file_exists "/etc/hosts"; then
    echo "File exists."
else
    echo "File not found."
fi
```

### 🖆 Check Disk Usage

```bash
check_disk_usage() {
    df -h /
}

check_disk_usage
```

### 🖆 Install a Package

```bash
install_package() {
    local package="${1:?Package name is required}"

    sudo apt update
    sudo apt install -y "$package"
}

install_package curl
```

### 🖆 Create a Timestamped Backup

```bash
create_backup() {
    local source="${1:?Source file is required}"

    if [[ ! -f "$source" ]]; then
        echo "File not found: $source" >&2
        return 1
    fi

    local timestamp
    timestamp=$(date +%Y%m%d_%H%M%S)

    cp -- "$source" "${source}.${timestamp}.bak"
}
```

Usage:

```bash
create_backup application.conf
```

### 🖆 Display a Formatted Message

```bash
log_message() {
    local level="$1"
    shift

    printf '[%s] [%s] %s\n' \
        "$(date '+%Y-%m-%d %H:%M:%S')" \
        "$level" \
        "$*"
}

log_message INFO "Application started"
log_message ERROR "Configuration file not found"
```

Output:

```text
[2026-11-09 09:00:00] [INFO] Application started
[2026-11-09 09:00:00] [ERROR] Configuration file not found
```

# 🗁 Best Practices

* Use descriptive function names that clearly communicate their responsibilities.
* Keep functions focused on one task.
* Prefer `local` variables to avoid unintended global state.
* Quote variable expansions, especially `"$@"` and file paths.
* Use `return` for success or failure status, not arbitrary strings.
* Use standard output to communicate calculated values.
* Send error messages to standard error using `>&2`.
* Validate function arguments before processing them.
* Use meaningful exit statuses.
* Avoid unnecessary recursion for large or deeply nested operations.
* Keep reusable functions in separate library files.
* Use `command -v` to check whether required commands exist.
* Document complex function parameters and expected behavior.
* Avoid modifying global variables unless that behavior is intentional.

# 🗁 Common Mistakes

### 🖆 Forgetting to Call a Function

Defining a function does not execute it.

```bash
greet() {
    echo "Hello"
}
```

Correct:

```bash
greet() {
    echo "Hello"
}

greet
```

### 🖆 Forgetting Local Variables

Without `local`, assignments inside a function can modify global variables.

```bash
process() {
    result="Temporary"
}
```

Prefer:

```bash
process() {
    local result="Temporary"
}
```

### 🖆 Using `return` for String Values

Incorrect:

```bash
get_name() {
    return "Muhammad"
}
```

Correct:

```bash
get_name() {
    echo "Muhammad"
}

name=$(get_name)
```

### 🖆 Forgetting to Quote Arguments

Potentially problematic:

```bash
process_file() {
    cat $1
}
```

Correct:

```bash
process_file() {
    cat -- "$1"
}
```

Quoting prevents word splitting and pathname expansion.

### 🖆 Losing the Original Exit Status

Incorrect:

```bash
some_command

echo "Command completed"

status=$?
```

The captured status belongs to `echo`, not `some_command`.

Correct:

```bash
some_command
status=$?

echo "Command completed"

echo "Exit status: $status"
```

### 🖆 Assuming Command Substitution Preserves Variable Changes

```bash
update_value() {
    result="Updated"
}

value=$(update_value)
```

Variable assignments inside command substitution do not modify the parent shell.

Call the function directly when it needs to update the current shell's variables.

# 🗁 Practical Exercises

### 🖆 Exercise: Basic Functions

Create a function named `welcome` that displays a welcome message.

Call it three times.

### 🖆 Exercise: Function Parameters

Create a function named `user_info` that accepts:

* Username
* Role
* Location

Display each value with a descriptive label.

### 🖆 Exercise: Local Variables

Create a global variable named `environment`.

Inside a function, declare another variable with the same name using `local`.

Display the value inside and outside the function.

### 🖆 Exercise: Return Status

Create a function named `check_file` that:

* Accepts a filename.
* Checks whether the file exists.
* Returns `0` when it exists.
* Returns `1` when it does not exist.
* Prints an appropriate error message to standard error when missing.

### 🖆 Exercise: Default Arguments

Create a function named `create_user` that accepts a username and role.

Use `guest` as the default username and `viewer` as the default role.

### 🖆 Exercise: Function Library

Create two files:

```text
project/
├── main.sh
└── functions.sh
```

Implement reusable functions in `functions.sh` and load them from `main.sh`.

### 🖆 Exercise: System Administration Utility

Create a Bash script with these functions:

* `show_system_info`: Display hostname and kernel information.
* `show_disk_usage`: Display filesystem usage.
* `show_memory_usage`: Display memory information.
* `check_command`: Check whether a command is installed.
* `log_message`: Display timestamped log messages.

Call all functions from the main script.

# 🗁 Interview Questions

### 🖆 Basic Questions

* What is a function in Bash?
* How do you define a function?
* How do you call a function?
* Can a Bash function accept arguments?
* What are positional parameters?
* What is the purpose of `local`?
* What happens if you define a function but never call it?

### 🖆 Intermediate Questions

* What is the difference between global and local variables?
* How do you return a value from a Bash function?
* What does the `return` keyword do?
* How do you capture a function's output?
* What is the difference between `"$@"` and `"$*"`?
* What does `shift` do?
* How can you implement default function arguments?
* What happens when a function runs inside a subshell?

### 🖆 Advanced Questions

* How does Bash determine a function's exit status?
* What is the difference between `return` and `exit`?
* How can you create reusable function libraries?
* What does `source` do?
* How can you export a function to a child Bash process?
* Why can command substitution affect variable scope?
* How does pipeline execution affect variables modified inside functions?
* How would you design a reusable Bash utility library?
* How would you handle argument validation and error reporting in a production shell script?

# 🗁 Official References

* [GNU Bash Manual — Shell Functions](https://www.gnu.org/software/bash/manual/html_node/Shell-Functions.html)
* [GNU Bash Manual — Shell Parameters](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameters.html)
* [GNU Bash Manual — Local Variables](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html)
* [GNU Bash Manual — Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)
* [GNU Bash Manual — Bash Builtins](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html)
