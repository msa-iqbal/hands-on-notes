# Shell Variables in Ubuntu/Linux

Variables are fundamental components of shell programming. They allow developers and system administrators to store, retrieve, modify, and manipulate data while executing commands and writing shell scripts.

In Ubuntu/Linux, Bash and Zsh provide variable management capabilities, including local variables, environment variables, arrays, special parameters, parameter expansion, and variable attributes.

This chapter focuses on shell variables using **Bash as the primary shell**, with relevant Zsh differences.

# 🗁 Introduction to Shell Variables

### 🖆 What Is a Shell Variable?

A shell variable is a named storage location that holds a value during shell execution.

Variables allow scripts to work with dynamic information instead of relying on hardcoded values.

Example:

```bash
username="Muhammad"
language="Bash"
```

Access the values:

```bash
echo "$username"
echo "$language"
```

Output:

```text
Muhammad
Bash
```

### 🖆 Why Are Variables Important?

Variables are useful for:

* Storing user input.
* Managing configuration values.
* Passing data between commands.
* Processing command-line arguments.
* Constructing file paths.
* Managing application environments.
* Performing calculations.
* Automating repetitive operations.
* Controlling script execution.

### 🖆 How Shell Variables Work

```text
       Variable Assignment
                |
                v
       username="Muhammad"
                |
                v
        Shell Variable
                |
                v
       Variable Expansion
                |
                v
       echo "$username"
                |
                v
             Output
```

# 🗁 Creating Variables

### 🖆 Basic Variable Assignment

Use the assignment operator `=`:

```bash
name="Muhammad"
age=25
language="Bash"
```

Important: Do not insert spaces around the assignment operator.

Correct:

```bash
username="admin"
```

Incorrect:

```bash
username = "admin"
```

The incorrect example is interpreted as a command named `username` with arguments rather than a variable assignment.

### 🖆 Assigning Different Values

Shell variables can contain strings and numeric text.

```bash
username="developer"
port=8080
version="1.0.0"
enabled="true"
```

Bash does not automatically assign strict data types to ordinary variables.

### 🖆 Assigning Command Output

Use command substitution:

```bash
current_date=$(date)
current_user=$(whoami)
current_directory=$(pwd)
```

Display the results:

```bash
echo "$current_date"
echo "$current_user"
echo "$current_directory"
```

### 🖆 Assigning Multiple Variables

```bash
first_name="Muhammad"
last_name="Iqbal"
role="Software Engineer"
```

Display:

```bash
printf '%s %s\n' "$first_name" "$last_name"
```

# 🗁 Variable Naming Rules

### 🖆 Valid Variable Names

Bash variable names generally follow these rules:

* Must begin with a letter or underscore.
* Can contain letters, numbers, and underscores.
* Cannot contain spaces.
* Are case-sensitive.
* Cannot contain hyphens or other punctuation.

Examples:

```bash
username="admin"
user_name="admin"
USER_NAME="admin"
_private_value="secret"
APP_PORT=8080
```

### 🖆 Invalid Variable Names

```bash
user-name="admin"
user name="admin"
2username="admin"
```

These are invalid because they violate Bash variable naming rules.

### 🖆 Case Sensitivity

Bash treats uppercase and lowercase variable names as different identifiers.

```bash
name="Muhammad"
NAME="Iqbal"

echo "$name"
echo "$NAME"
```

Output:

```text
Muhammad
Iqbal
```

### 🖆 Naming Conventions

Common conventions include:

| Convention | Example | Usage |
| --- | --- | --- |
| Lowercase | `username` | Local variables |
| Snake case | `user_name` | Readable multiword names |
| Uppercase | `APP_ENV` | Environment variables and constants |
| Underscore prefix | `_internal_value` | Internal implementation variables |

Choose descriptive names that communicate the purpose of the stored value.

# 🗁 Accessing Variables

### 🖆 Using the Dollar Sign

Use `$` to expand a variable:

```bash
name="Muhammad"

echo $name
```

### 🖆 Using Curly Braces

Curly braces explicitly identify the variable name.

```bash
name="Muhammad"

echo "${name}"
```

They are particularly useful when adding adjacent characters.

```bash
project="notes"

echo "${project}_backup"
```

Output:

```text
notes_backup
```

Without braces, Bash would interpret `$project_backup` as a different variable.

### 🖆 Using `printf`

`printf` provides predictable formatting.

```bash
name="Muhammad"
role="Developer"

printf 'Name: %s\n' "$name"
printf 'Role: %s\n' "$role"
```

### 🖆 Displaying Multiple Variables

```bash
first_name="Muhammad"
last_name="Iqbal"

echo "$first_name $last_name"
```

# 🗁 Variable Types

### 🖆 Ordinary Shell Variables

An ordinary shell variable exists in the current shell.

```bash
username="admin"
```

It is not automatically inherited by child processes.

### 🖆 Environment Variables

Environment variables are exported to child processes.

```bash
export APP_ENV="production"
```

A child process can access the variable.

### 🖆 Read-Only Variables

Read-only variables cannot be reassigned.

```bash
readonly APP_NAME="Hands-on-Notes"
```

### 🖆 Integer Variables

Bash supports integer attributes through `declare -i`.

```bash
declare -i count=10

count+=5

echo "$count"
```

Output:

```text
15
```

### 🖆 Array Variables

Arrays store multiple values under one variable name.

```bash
languages=("Bash" "Python" "JavaScript")
```

Access an element:

```bash
echo "${languages[0]}"
```

### 🖆 Variable Attribute Summary

| Type | Declaration | Purpose |
| --- | --- | --- |
| Ordinary | `name="value"` | Store a shell value |
| Exported | `export NAME="value"` | Share with child processes |
| Read-only | `readonly NAME="value"` | Prevent reassignment |
| Integer | `declare -i count=10` | Integer arithmetic |
| Indexed array | `declare -a items` | Store indexed values |
| Associative array | `declare -A config` | Store key-value pairs |

# 🗁 Environment Variables

### 🖆 What Are Environment Variables?

Environment variables are variables exported by a process so that its child processes can inherit them.

Example:

```bash
export APP_ENV="development"
```

Display:

```bash
printenv APP_ENV
```

### 🖆 Creating Environment Variables

Use `export`:

```bash
export DATABASE_HOST="localhost"
export DATABASE_PORT="5432"
export APP_ENV="development"
```

Alternatively:

```bash
DATABASE_HOST="localhost"
export DATABASE_HOST
```

### 🖆 Viewing Environment Variables

Display all exported environment variables:

```bash
printenv
```

Another option:

```bash
env
```

Display a specific variable:

```bash
printenv HOME
```

### 🖆 Environment Variables vs Shell Variables

| Feature | Shell Variable | Environment Variable |
| --- | --- | --- |
| Current shell access | Yes | Yes |
| Inherited by child processes | No, by default | Yes |
| Created using assignment | Yes | Yes |
| Export required | No | Yes |
| Common usage | Internal script data | Process configuration |

### 🖆 Demonstrating Variable Inheritance

Create an ordinary variable:

```bash
APP_NAME="Hands-on-Notes"
```

Start a child Bash process:

```bash
bash -c 'echo "${APP_NAME-unavailable}"'
```

Output:

```text
unavailable
```

Now export the variable:

```bash
export APP_NAME
```

Run the child process again:

```bash
bash -c 'echo "$APP_NAME"'
```

Output:

```text
Hands-on-Notes
```

This demonstrates how environment variables are inherited by child processes.

# 🗁 Local and Global Variables

### 🖆 Global Shell Variables

A variable declared outside a function is generally accessible throughout the current shell.

```bash
username="Muhammad"

show_user() {
    echo "$username"
}

show_user
```

### 🖆 Local Variables

Use `local` inside a Bash function to create a function-scoped variable.

```bash
greet() {
    local username="Muhammad"

    echo "Hello, $username"
}

greet
```

The local variable does not remain available after the function returns, unless another variable of the same name already exists in an outer scope.

### 🖆 Comparing Local and Global Variables

```bash
username="Developer"

show_user() {
    local username="Administrator"

    echo "Inside function: $username"
}

show_user

echo "Outside function: $username"
```

Output:

```text
Inside function: Administrator
Outside function: Developer
```

### 🖆 Declaring Variables with `declare`

The `declare` built-in creates variables and assigns attributes.

```bash
declare username="Muhammad"
declare -i age=25
declare -r APP_VERSION="1.0"
```

Inside functions, `declare` creates local variables by default.

Use `declare -g` inside a function when a global variable must be explicitly declared:

```bash
create_global() {
    declare -g APP_STATUS="ready"
}

create_global

echo "$APP_STATUS"
```

# 🗁 Read-Only Variables

### 🖆 Declaring Read-Only Variables

Use `readonly`:

```bash
readonly APP_NAME="Hands-on-Notes"
readonly API_VERSION="v1"
```

Attempting to modify a read-only variable produces an error:

```bash
APP_NAME="Another Project"
```

### 🖆 Using `declare -r`

The `declare -r` option also creates a read-only variable.

```bash
declare -r MAX_RETRIES=5
```

### 🖆 Listing Read-Only Variables

```bash
readonly -p
```

Display a specific declaration:

```bash
declare -p APP_NAME
```

Read-only variables are useful for configuration values that should not be accidentally modified during script execution.

# 🗁 Removing Variables

### 🖆 Using `unset`

The `unset` command removes a variable from the current shell.

```bash
username="Muhammad"

unset username
```

Verify:

```bash
echo "${username-unset}"
```

### 🖆 Removing Multiple Variables

```bash
first_name="Muhammad"
last_name="Iqbal"
role="Developer"

unset first_name last_name role
```

### 🖆 Removing Environment Variables

```bash
export APP_ENV="production"

unset APP_ENV
```

This removes the variable from the current shell environment.

### 🖆 Important Behavior

An exported variable can be removed using `unset`, but an already-running child process retains its own environment copy.

Also, Bash cannot unset a read-only variable.

# 🗁 Special Shell Variables

### 🖆 What Are Special Variables?

Bash provides predefined parameters containing information about the current shell, script execution, command arguments, and process state.

These variables are useful for automation and debugging.

### 🖆 Common Special Variables

| Variable | Description |
| --- | --- |
| `$0` | Script name or shell name |
| `$1`, `$2`, ... | Positional arguments |
| `$#` | Number of positional parameters |
| `$@` | All positional parameters |
| `$*` | All positional parameters as one expansion when quoted |
| `$?` | Exit status of the most recent command |
| `$$` | Current shell process ID |
| `$!` | PID of the most recently backgrounded asynchronous command |
| `$-` | Current shell option flags |
| `$_` | Last argument of the previous command, with context-dependent behavior |
| `$PPID` | Parent process ID |
| `$BASH_VERSION` | Bash version |
| `$BASH_SOURCE` | Source file information |
| `$FUNCNAME` | Current function name in function execution |
| `$LINENO` | Current source line number |
| `$RANDOM` | Pseudorandom integer |
| `$SECONDS` | Seconds since the shell started |
| `$PIPESTATUS` | Exit statuses of commands in the most recent foreground pipeline |

### 🖆 Checking the Exit Status

```bash
mkdir /tmp/bash-example

echo "$?"
```

An exit status of `0` normally indicates success.

A non-zero exit status indicates failure or another condition defined by the command.

### 🖆 Checking the Process ID

```bash
echo "Current PID: $$"
echo "Parent PID: $PPID"
```

### 🖆 Generating a Random Number

```bash
echo "$RANDOM"
```

`RANDOM` produces a pseudorandom integer in the range `0` to `32767`.

Generate a number between 1 and 100:

```bash
echo "$((RANDOM % 100 + 1))"
```

### 🖆 Measuring Script Execution Time

```bash
#!/usr/bin/env bash

echo "Starting script..."

sleep 2

echo "Elapsed seconds: $SECONDS"
```

### 🖆 Inspecting Pipeline Exit Statuses

```bash
false | true

printf 'First command: %s\n' "${PIPESTATUS[0]}"
printf 'Second command: %s\n' "${PIPESTATUS[1]}"
```

Capture `PIPESTATUS` immediately after the pipeline because subsequent commands can overwrite its contents.

# 🗁 Positional Parameters

### 🖆 What Are Positional Parameters?

Positional parameters represent arguments passed to a shell script or function.

| Parameter | Meaning |
| --- | --- |
| `$0` | Script or shell name |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `${10}` | Tenth argument |
| `$#` | Argument count |

### 🖆 Reading Script Arguments

Create `arguments.sh`:

```bash
#!/usr/bin/env bash

echo "Script: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Total arguments: $#"
```

Execute:

```bash
bash arguments.sh Ubuntu Linux
```

Output:

```text
Script: arguments.sh
First argument: Ubuntu
Second argument: Linux
Total arguments: 2
```

### 🖆 Accessing All Arguments

Use `"$@"`:

```bash
#!/usr/bin/env bash

for argument in "$@"; do
    printf 'Argument: %s\n' "$argument"
done
```

Execute:

```bash
bash arguments.sh "Ubuntu Linux" Bash Python
```

Each argument remains a separate value.

### 🖆 Understanding `$@` and `$*`

| Expression | Behavior |
| --- | --- |
| `$@` | Expands positional parameters as separate words, subject to normal splitting and globbing |
| `"$@"` | Preserves every argument as a separate word |
| `$*` | Expands positional parameters subject to normal splitting and globbing |
| `"$*"` | Combines all arguments into one word, separated by the first character of `IFS` |

Use `"$@"` when forwarding arguments to commands or functions.

### 🖆 Using `shift`

The `shift` built-in removes positional parameters from the beginning of the argument list.

Example:

```bash
#!/usr/bin/env bash

echo "First argument: $1"

shift

echo "New first argument: $1"
echo "Remaining arguments: $*"
```

Execute:

```bash
bash arguments.sh Ubuntu Linux Bash
```

Output:

```text
First argument: Ubuntu
New first argument: Linux
Remaining arguments: Linux Bash
```

### 🖆 Processing All Arguments with `shift`

```bash
#!/usr/bin/env bash

while (($# > 0)); do
    printf 'Processing: %s\n' "$1"
    shift
done
```

This is useful when processing command-line arguments sequentially.

# 🗁 Bash Arrays

### 🖆 What Are Arrays?

Arrays allow Bash scripts to store multiple values under one variable name.

Bash supports:

* Indexed arrays.
* Associative arrays.

### 🖆 Indexed Arrays

Declare an indexed array:

```bash
languages=("Bash" "Python" "JavaScript")
```

Access elements:

```bash
echo "${languages[0]}"
echo "${languages[1]}"
```

Bash indexed arrays use zero-based indexing.

### 🖆 Accessing Array Elements

Display all elements:

```bash
printf '%s\n' "${languages[@]}"
```

Display the number of elements:

```bash
echo "${#languages[@]}"
```

Display array indices:

```bash
echo "${!languages[@]}"
```

### 🖆 Adding Elements

```bash
languages+=("PHP")
```

### 🖆 Updating Elements

```bash
languages[1]="TypeScript"
```

### 🖆 Removing Elements

```bash
unset 'languages[0]'
```

### 🖆 Associative Arrays

Associative arrays store key-value pairs.

Declare:

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

### 🖆 Iterating Over Arrays

Indexed array:

```bash
languages=("Bash" "Python" "JavaScript")

for language in "${languages[@]}"; do
    echo "$language"
done
```

Associative array:

```bash
declare -A user=(
    [name]="Muhammad"
    [role]="Developer"
    [language]="Bash"
)

for key in "${!user[@]}"; do
    printf '%s: %s\n' "$key" "${user[$key]}"
done
```

# 🗁 Variable Expansion

### 🖆 What Is Variable Expansion?

Variable expansion replaces a variable reference with its stored value before command execution.

Example:

```bash
name="Muhammad"

echo "Hello, $name"
```

Output:

```text
Hello, Muhammad
```

### 🖆 Basic Expansion

```bash
APP_ENV="development"

echo "$APP_ENV"
```

### 🖆 Braced Expansion

```bash
project="notes"

echo "${project}_backup"
```

### 🖆 Preventing Expansion

Single quotes prevent variable expansion:

```bash
name="Muhammad"

echo 'Hello, $name'
```

Output:

```text
Hello, $name
```

Double quotes allow variable expansion:

```bash
echo "Hello, $name"
```

Output:

```text
Hello, Muhammad
```

### 🖆 Expansion Comparison

| Quoting | Variable Expansion | Command Substitution |
| --- | --- | --- |
| Unquoted | Yes | Yes |
| Single quotes | No | No |
| Double quotes | Yes | Yes |

Unquoted expansions can also undergo word splitting and pathname expansion. Double quoting is generally preferred when the value should remain a single argument.

# 🗁 Parameter Expansion

### 🖆 Default Value When Unset or Empty

```bash
APP_ENV=""

echo "${APP_ENV:-development}"
```

Output:

```text
development
```

The `:-` operator provides the default when the variable is unset or empty.

### 🖆 Default Value When Unset

```bash
echo "${APP_ENV-development}"
```

The `-` operator uses the default only when the variable is unset.

### 🖆 Assigning a Default Value

```bash
echo "${APP_ENV:=development}"
```

The `:=` operator assigns the default when the variable is unset or empty.

### 🖆 Assigning a Default When Unset

```bash
echo "${APP_ENV=development}"
```

The `=` operator assigns the default only when the variable is unset.

### 🖆 Displaying an Error When Unset or Empty

```bash
echo "${DATABASE_URL:?DATABASE_URL is required}"
```

This generates an error if the variable is unset or empty.

### 🖆 Alternative Value When Set

```bash
APP_ENV="production"

echo "${APP_ENV:+Environment configured}"
```

The `:+` operator expands to the alternative value when the variable is set and non-empty.

### 🖆 Parameter Expansion Summary

| Expression | Behavior |
| --- | --- |
| `${VAR:-default}` | Default when unset or empty |
| `${VAR-default}` | Default when unset |
| `${VAR:=default}` | Assign default when unset or empty |
| `${VAR=default}` | Assign default when unset |
| `${VAR:?message}` | Error when unset or empty |
| `${VAR?message}` | Error when unset |
| `${VAR:+value}` | Alternative when set and non-empty |
| `${VAR+value}` | Alternative when set |

# 🗁 Variable Substitution

### 🖆 Replacing a Substring

Bash supports pattern replacement within variables.

Replace the first matching occurrence:

```bash
text="Ubuntu Linux Ubuntu"

echo "${text/Ubuntu/Debian}"
```

Output:

```text
Debian Linux Ubuntu
```

Replace all matching occurrences:

```bash
echo "${text//Ubuntu/Debian}"
```

Output:

```text
Debian Linux Debian
```

### 🖆 Removing a Prefix

Remove the shortest matching prefix:

```bash
filename="backup.tar.gz"

echo "${filename#*.}"
```

Output:

```text
tar.gz
```

Remove the longest matching prefix:

```bash
echo "${filename##*.}"
```

Output:

```text
gz
```

### 🖆 Removing a Suffix

Remove the shortest matching suffix:

```bash
filename="backup.tar.gz"

echo "${filename%.*}"
```

Output:

```text
backup.tar
```

Remove the longest matching suffix:

```bash
echo "${filename%%.*}"
```

Output:

```text
backup
```

# 🗁 String Manipulation

### 🖆 Getting String Length

```bash
name="Muhammad"

echo "${#name}"
```

Output:

```text
8
```

### 🖆 Extracting a Substring

```bash
text="Ubuntu Linux"

echo "${text:0:6}"
```

Output:

```text
Ubuntu
```

Extract a substring starting at position 7:

```bash
echo "${text:7:5}"
```

Output:

```text
Linux
```

### 🖆 Converting to Uppercase

```bash
name="muhammad"

echo "${name^^}"
```

Output:

```text
MUHAMMAD
```

### 🖆 Converting to Lowercase

```bash
name="MUHAMMAD"

echo "${name,,}"
```

Output:

```text
muhammad
```

### 🖆 Capitalizing the First Character

```bash
name="muhammad"

echo "${name^}"
```

Output:

```text
Muhammad
```

### 🖆 Lowercasing the First Character

```bash
name="MUHAMMAD"

echo "${name,}"
```

Output:

```text
mUHAMMAD
```

# 🗁 Arithmetic Operations

### 🖆 Arithmetic Expansion

Bash supports integer arithmetic through `$((...))`.

```bash
a=10
b=5

sum=$((a + b))
difference=$((a - b))
product=$((a * b))
quotient=$((a / b))
remainder=$((a % b))
```

Display results:

```bash
echo "$sum"
echo "$difference"
echo "$product"
echo "$quotient"
echo "$remainder"
```

### 🖆 Arithmetic Operators

| Operator | Operation |
| --- | --- |
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Integer division |
| `%` | Modulus |
| `**` | Exponentiation |
| `++` | Increment |
| `--` | Decrement |

### 🖆 Increment and Decrement

```bash
count=10

((count++))

echo "$count"
```

Output:

```text
11
```

Decrement:

```bash
((count--))

echo "$count"
```

Output:

```text
10
```

### 🖆 Compound Assignment

```bash
count=10

((count += 5))
((count -= 2))
((count *= 3))

echo "$count"
```

### 🖆 Integer Limitations

Bash arithmetic uses integer values and does not provide native floating-point arithmetic.

For decimal calculations, use an external utility such as `bc`:

```bash
echo "10 / 3" | bc -l
```

# 🗁 Variable Scope

### 🖆 What Is Variable Scope?

Variable scope determines where a variable can be accessed.

Bash has several important scope-related behaviors:

* Shell-level variables.
* Environment variables.
* Function-local variables.
* Dynamically scoped function variables.

### 🖆 Function-Local Scope

```bash
#!/usr/bin/env bash

username="Muhammad"

show_user() {
    local username="Administrator"

    echo "Inside: $username"
}

show_user

echo "Outside: $username"
```

Output:

```text
Inside: Administrator
Outside: Muhammad
```

### 🖆 Dynamic Scoping in Bash

Bash uses dynamic scoping for function-local variables.

A function called by another function can access a local variable declared in the caller when that variable is visible in the active call chain.

Example:

```bash
#!/usr/bin/env bash

show_name() {
    echo "$name"
}

main() {
    local name="Muhammad"

    show_name
}

main
```

Output:

```text
Muhammad
```

This behavior differs from lexical scoping used by many general-purpose programming languages.

### 🖆 Environment Scope

Environment variables are inherited by child processes.

However, changes made inside a child process do not automatically modify the parent shell.

```bash
export APP_ENV="development"

bash -c 'APP_ENV="production"; echo "$APP_ENV"'

echo "$APP_ENV"
```

Output:

```text
production
development
```

The child process has its own environment copy.

# 🗁 Exporting Variables

### 🖆 Using `export`

```bash
export APP_ENV="production"
```

This creates or marks a variable for export to child processes.

### 🖆 Exporting Existing Variables

```bash
APP_NAME="Hands-on-Notes"

export APP_NAME
```

### 🖆 Exporting Multiple Variables

```bash
export APP_ENV="development"
export APP_PORT="8080"
export APP_DEBUG="true"
```

### 🖆 Running a Command with a Temporary Environment Variable

```bash
APP_ENV="production" bash -c 'echo "$APP_ENV"'
```

The variable is supplied to that command's environment without permanently exporting it in the parent shell.

### 🖆 Inspecting Exported Variables

```bash
export -p
```

Display one environment variable:

```bash
printenv APP_ENV
```

# 🗁 Default Environment Variables

### 🖆 Important Ubuntu/Linux Variables

| Variable | Description |
| --- | --- |
| `HOME` | User home directory |
| `USER` | Current username |
| `LOGNAME` | Login username |
| `SHELL` | Configured login shell |
| `PATH` | Command search path |
| `PWD` | Current working directory |
| `OLDPWD` | Previous working directory |
| `HOSTNAME` | Hostname when provided in the environment |
| `LANG` | Default locale |
| `LC_ALL` | Overrides locale categories |
| `TERM` | Terminal type |
| `EDITOR` | Preferred editor |
| `VISUAL` | Preferred visual editor |
| `TMPDIR` | Temporary directory preference |
| `TZ` | Time zone configuration |

### 🖆 Inspecting Environment Variables

Display a specific variable:

```bash
echo "$HOME"
echo "$USER"
echo "$PATH"
echo "$LANG"
```

Display all exported variables:

```bash
printenv
```

### 🖆 Modifying PATH

Add a directory:

```bash
export PATH="$HOME/bin:$PATH"
```

Check the result:

```bash
echo "$PATH"
```

A directory must be included in `PATH` for commands inside it to be discoverable by ordinary command lookup.

# 🗁 Bash vs Zsh Variables

### 🖆 Common Variable Features

Both Bash and Zsh support:

* Ordinary shell variables.
* Environment variables.
* Read-only variables.
* Positional parameters.
* Special parameters.
* Indexed arrays.
* Associative arrays.
* Parameter expansion.
* Arithmetic expansion.
* Function-local variables.

### 🖆 Important Differences

| Feature | Bash | Zsh |
| --- | --- | --- |
| Indexed array starting index | 0 | 1 |
| Associative arrays | `declare -A` | `typeset -A` |
| Local variables | `local` | `local` |
| Global declaration in functions | `declare -g` | `typeset -g` |
| Array length | `${#array[@]}` | `${#array[@]}` |
| Array keys | `${!array[@]}` | `${(k)array}` |
| Environment variables | `export` | `export` |
| Version variable | `BASH_VERSION` | `ZSH_VERSION` |

### 🖆 Bash Array Example

```bash
items=("Apple" "Banana")

echo "${items[0]}"
```

### 🖆 Zsh Array Example

```zsh
items=("Apple" "Banana")

echo "${items[1]}"
```

When writing portable shell scripts, account for these differences rather than assuming identical behavior.

# 🗁 Best Practices

* Use descriptive variable names.
* Follow consistent naming conventions.
* Quote variable expansions when appropriate.
* Use uppercase names for exported configuration variables when suitable.
* Use `local` for function-specific variables.
* Use `readonly` for values that must not change.
* Use `export` only when child processes need the variable.
* Use `${VAR:-default}` for optional configuration defaults.
* Use `${VAR:?message}` for required configuration values.
* Prefer `"$@"` when forwarding script arguments.
* Use arrays instead of constructing command strings.
* Use `printf` for predictable output formatting.
* Avoid unnecessary global variables.
* Validate external input before using it.
* Keep environment configuration separate from application logic.
* Remember the differences between Bash and Zsh.

### 🖆 Example: Well-Structured Variable Usage

```bash
#!/usr/bin/env bash

set -u

readonly APP_NAME="Hands-on-Notes"
APP_ENV="${APP_ENV:-development}"
APP_PORT="${APP_PORT:-8080}"

print_config() {
    local environment="$1"

    printf 'Application: %s\n' "$APP_NAME"
    printf 'Environment: %s\n' "$environment"
    printf 'Port: %s\n' "$APP_PORT"
}

print_config "$APP_ENV"
```

This example demonstrates read-only configuration, default values, local variables, and formatted output.

# 🗁 Common Mistakes

### 🖆 Adding Spaces Around Assignment Operators

Incorrect:

```bash
name = "Muhammad"
```

Correct:

```bash
name="Muhammad"
```

### 🖆 Forgetting to Quote Variables

Potentially problematic:

```bash
filename="my document.txt"

cat $filename
```

Correct:

```bash
cat "$filename"
```

### 🖆 Confusing Assignment and Comparison

Assignment:

```bash
status="success"
```

String comparison:

```bash
[[ "$status" == "success" ]]
```

### 🖆 Forgetting to Export Variables

An ordinary shell variable is not automatically inherited by child processes.

Use:

```bash
export APP_ENV="production"
```

### 🖆 Incorrect Array Indexing

Bash indexed arrays start at zero.

```bash
items=("A" "B" "C")

echo "${items[0]}"
```

### 🖆 Using Unset Variables Unexpectedly

With `set -u`, expanding an unset variable can cause an error.

Use a default:

```bash
echo "${APP_ENV:-development}"
```

### 🖆 Treating Bash Variables as Strongly Typed

Bash does not provide general-purpose strict data types.

Ordinary variables store textual values, while arithmetic contexts interpret values as integers.

# 🗁 Practical Exercises

### 🖆 Variable Fundamentals

* Create variables for your name, role, and operating system.
* Display each variable using `echo` and `printf`.
* Create a variable containing a directory path.
* Practice using braces around variable names.
* Test uppercase and lowercase variable names.

### 🖆 Environment Variables

* Create an ordinary shell variable.
* Start a child Bash process and inspect the variable.
* Export the variable.
* Start another child process and verify inheritance.
* Remove the exported variable using `unset`.

### 🖆 Variable Attributes

* Create an integer variable using `declare -i`.
* Create a read-only variable.
* Inspect variable attributes using `declare -p`.
* Try modifying a read-only variable.
* Create and remove an ordinary variable.

### 🖆 Positional Parameters

* Create a script accepting three arguments.
* Display the script name and argument count.
* Iterate over arguments using `"$@"`.
* Use `shift` to process arguments sequentially.
* Test arguments containing spaces.

### 🖆 Arrays

* Create an indexed array of programming languages.
* Add and update array elements.
* Remove an array element.
* Create an associative array representing application configuration.
* Iterate over associative array keys and values.

### 🖆 Parameter Expansion

* Provide a default value using `:-`.
* Assign a default using `:=`.
* Generate an error using `:?`.
* Replace text inside a variable.
* Extract a substring.
* Remove a filename extension.
* Convert a string to uppercase and lowercase.

### 🖆 Practical Project: Application Configuration

Create `config.sh`:

```bash
#!/usr/bin/env bash

set -u

readonly APP_NAME="Hands-on-Notes"

APP_ENV="${APP_ENV:-development}"
APP_PORT="${APP_PORT:-8080}"
APP_DEBUG="${APP_DEBUG:-false}"

print_config() {
    printf 'Application: %s\n' "$APP_NAME"
    printf 'Environment: %s\n' "$APP_ENV"
    printf 'Port: %s\n' "$APP_PORT"
    printf 'Debug: %s\n' "$APP_DEBUG"
}

print_config
```

Run:

```bash
bash config.sh
```

Override configuration:

```bash
APP_ENV="production" APP_PORT="9000" bash config.sh
```

Expected behavior:

* Default values are used when configuration is not supplied.
* Custom values override defaults.
* The application name remains read-only.
* Configuration is displayed using formatted output.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

* What is a shell variable?
* How do you create a variable in Bash?
* Why should spaces not be used around `=` during assignment?
* Are Bash variables case-sensitive?
* What is the difference between a shell variable and an environment variable?
* How do you remove a variable?

### 🖆 Environment and Scope Questions

* What does `export` do?
* How are environment variables inherited?
* What is the difference between local and global variables?
* What is dynamic scoping in Bash?
* What happens when a child process modifies an inherited environment variable?
* How do you create a read-only variable?

### 🖆 Parameter Questions

* What are positional parameters?
* What is the difference between `$@` and `$*`?
* What does `$#` represent?
* What is the purpose of `shift`?
* What does `$?` represent?
* What information is stored in `PIPESTATUS`?

### 🖆 Advanced Questions

* What is parameter expansion?
* What is the difference between `${VAR:-default}` and `${VAR-default}`?
* What is the difference between `${VAR:=default}` and `${VAR=default}`?
* How do you replace text inside a variable?
* How do you extract a substring?
* What are indexed and associative arrays?
* How does Bash integer arithmetic work?
* What is the difference between Bash and Zsh array indexing?

# 🗁 References

* [GNU Bash Manual](https://www.gnu.org/software/bash/manual/)
* [Bash Shell Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html)
* [Bash Shell Variables](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html)
* [Bash Arrays](https://www.gnu.org/software/bash/manual/html_node/Arrays.html)
* [Bash Parameter Expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)
* [Bash Shell Arithmetic](https://www.gnu.org/software/bash/manual/html_node/Shell-Arithmetic.html)
* [Zsh Documentation](https://zsh.sourceforge.io/Doc/)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
