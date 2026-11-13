# Shell Conditional Statements

Conditional statements allow shell scripts to make decisions based on conditions. They control which commands execute depending on factors such as:

* Whether a file or directory exists.
* Whether two strings match.
* Whether a number satisfies a comparison.
* Whether a command succeeds or fails.
* Whether a variable contains a particular value.
* Whether a system resource meets specific criteria.

In Ubuntu/Linux shell scripting, Bash provides several mechanisms for conditional execution, including `if`, `elif`, `else`, `case`, `test`, `[[ ]]`, and arithmetic evaluation with `(( ))`.

This chapter focuses primarily on Bash conditional statements.

# 🗁 Conditional Logic

Conditional logic evaluates an expression and determines whether a command or group of commands should execute.

In Bash, a condition is evaluated through an exit status:

| Exit status | Meaning |
| --- | --- |
| `0` | Success / true |
| Non-zero | Failure / false |

Unlike many programming languages, Bash uses the exit status of commands to determine whether a condition is true or false.

### 🖆 Basic Example

```bash
#!/usr/bin/env bash

if true; then
    echo "The condition is true."
fi
```

Output:

```text
The condition is true.
```

The `true` command returns exit status `0`, so Bash executes the commands inside the `if` block.

### 🖆 Conditional Execution Flow

```text
        Start
          |
          v
    Evaluate condition
          |
      +---+---+
      |       |
    True     False
      |       |
      v       v
 Execute    Skip or
 commands   alternative
      |       |
      +---+---+
          |
          v
         End
```

# 🗁 The if Statement

The `if` statement executes commands when a condition succeeds.

### 🖆 Syntax

```bash
if condition; then
    commands
fi
```

The structure consists of:

| Keyword | Purpose |
| --- | --- |
| `if` | Starts a conditional statement |
| `condition` | Command or expression being evaluated |
| `then` | Starts the conditional command block |
| `fi` | Ends the conditional statement |

### 🖆 Basic Example

```bash
#!/usr/bin/env bash

age=25

if [ "$age" -ge 18 ]; then
    echo "You are an adult."
fi
```

Output:

```text
You are an adult.
```

The `-ge` operator checks whether the value is greater than or equal to another number.

### 🖆 Using Multiple Commands

```bash
#!/usr/bin/env bash

if [ -d "$HOME" ]; then
    echo "Home directory exists."
    echo "Location: $HOME"
fi
```

All commands between `then` and `fi` execute when the condition succeeds.

# 🗁 The elif and else Statements

Use `else` to execute an alternative block when the condition is false.

Use `elif` to evaluate additional conditions.

### 🖆 if-else Syntax

```bash
if condition; then
    commands
else
    alternative_commands
fi
```

### 🖆 Example

```bash
#!/usr/bin/env bash

age=16

if [ "$age" -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

Output:

```text
You are a minor.
```

### 🖆 if-elif-else Syntax

```bash
if condition1; then
    commands
elif condition2; then
    commands
elif condition3; then
    commands
else
    alternative_commands
fi
```

Bash evaluates conditions from top to bottom. The first successful condition determines which block executes.

### 🖆 Example

```bash
#!/usr/bin/env bash

score=85

if [ "$score" -ge 90 ]; then
    echo "Grade: A"
elif [ "$score" -ge 80 ]; then
    echo "Grade: B"
elif [ "$score" -ge 70 ]; then
    echo "Grade: C"
else
    echo "Grade: F"
fi
```

Output:

```text
Grade: B
```

# 🗁 The test Command

The `test` command evaluates conditional expressions and returns an exit status.

Bash provides three common ways to evaluate test expressions:

```bash
test expression
```

```bash
[ expression ]
```

```bash
[[ expression ]]
```

The first two forms use the `test` command or its equivalent shell syntax.

### 🖆 Using test

```bash
#!/usr/bin/env bash

if test "$USER" = "root"; then
    echo "Running as root."
else
    echo "Not running as root."
fi
```

### 🖆 Using Single Brackets

```bash
#!/usr/bin/env bash

if [ "$USER" = "root" ]; then
    echo "Running as root."
fi
```

The single-bracket syntax requires spaces around the expression and brackets.

Correct:

```bash
[ "$name" = "Muhammad" ]
```

Incorrect:

```bash
["$name"="Muhammad"]
```

### 🖆 Comparing Conditional Syntax

| Syntax | Description |
| --- | --- |
| `test expression` | Traditional test command |
| `[ expression ]` | POSIX-compatible conditional syntax |
| `[[ expression ]]` | Bash conditional keyword with additional features |
| `(( expression ))` | Arithmetic evaluation |

For Bash scripts, `[[ ]]` is generally preferred for string comparisons, pattern matching, and compound conditions.

# 🗁 The Double Bracket Conditional

The `[[ ]]` construct is a Bash keyword that provides enhanced conditional evaluation.

Unlike `[ ]`, it supports additional operators and safer handling of many special characters.

### 🖆 Basic Syntax

```bash
if [[ condition ]]; then
    commands
fi
```

### 🖆 Example

```bash
#!/usr/bin/env bash

name="Muhammad"

if [[ "$name" == "Muhammad" ]]; then
    echo "Name matched."
fi
```

### 🖆 Important Differences Between [ ] and [[ ]]

| Feature | `[ ]` | `[[ ]]` |
| --- | --- | --- |
| POSIX compatibility | Yes | No |
| String comparison | Yes | Yes |
| Pattern matching | Limited | Yes |
| Regular expressions | No | Yes |
| Logical `&&` and ` | | ` | Inside separate commands or escaped operators | Supported directly |
| Word splitting | Can occur with unquoted expansions | Prevented for ordinary expansions |
| Pathname expansion | Can occur with unquoted expansions | Prevented for ordinary expansions |

Example using `[[ ]]`:

```bash
#!/usr/bin/env bash

filename="report.txt"

if [[ "$filename" == *.txt ]]; then
    echo "This is a text file."
fi
```

The `*.txt` expression performs pattern matching rather than requiring an exact string match.

# 🗁 String Comparisons

String comparisons evaluate text values.

Bash supports operators for equality, inequality, and empty or non-empty strings.

### 🖆 Common String Operators

| Operator | Meaning |
| --- | --- |
| `=` | Strings are equal |
| `==` | Strings are equal |
| `!=` | Strings are different |
| `-z` | String length is zero |
| `-n` | String length is non-zero |

### 🖆 Equality Comparison

```bash
#!/usr/bin/env bash

username="admin"

if [[ "$username" == "admin" ]]; then
    echo "Administrator access."
fi
```

### 🖆 Inequality Comparison

```bash
#!/usr/bin/env bash

environment="development"

if [[ "$environment" != "production" ]]; then
    echo "Development configuration is active."
fi
```

### 🖆 Checking an Empty String

```bash
#!/usr/bin/env bash

username=""

if [[ -z "$username" ]]; then
    echo "Username is empty."
fi
```

### 🖆 Checking a Non-empty String

```bash
#!/usr/bin/env bash

username="developer"

if [[ -n "$username" ]]; then
    echo "Username is provided."
fi
```

### 🖆 Comparing User Input

```bash
#!/usr/bin/env bash

read -r -p "Enter your username: " username

if [[ "$username" == "admin" ]]; then
    echo "Welcome, administrator."
else
    echo "Welcome, $username."
fi
```

# 🗁 Numeric Comparisons

Numeric comparison operators evaluate integer values.

Bash's `test` and `[[ ]]` expressions use dedicated operators for numeric comparisons.

### 🖆 Numeric Operators

| Operator | Meaning |
| --- | --- |
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |
| `-lt` | Less than |
| `-le` | Less than or equal |

### 🖆 Comparison Example

```bash
#!/usr/bin/env bash

number=50

if [[ "$number" -gt 25 ]]; then
    echo "Number is greater than 25."
fi
```

### 🖆 Multiple Numeric Conditions

```bash
#!/usr/bin/env bash

age=25

if [[ "$age" -ge 18 && "$age" -le 60 ]]; then
    echo "Age is within the permitted range."
else
    echo "Age is outside the permitted range."
fi
```

### 🖆 Important Distinction

Do not confuse string comparison with numeric comparison.

```bash
[[ "10" < "2" ]]
```

This performs a lexicographical string comparison.

For numeric comparisons, use:

```bash
[[ 10 -lt 2 ]]
```

Or use arithmetic evaluation:

```bash
(( 10 < 2 ))
```

# 🗁 File and Directory Tests

Bash provides file-test operators to check whether files, directories, and filesystem objects exist or have specific properties.

These operators are especially useful for installation scripts, backups, deployment automation, and system administration.

### 🖆 Common File Test Operators

| Operator | Meaning |
| --- | --- |
| `-e` | Path exists |
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-r` | Path is readable |
| `-w` | Path is writable |
| `-x` | Path is executable or searchable |
| `-s` | File exists and has a size greater than zero |
| `-L` | Symbolic link exists |
| `-b` | Block device exists |
| `-c` | Character device exists |
| `-p` | Named pipe exists |
| `-S` | Unix domain socket exists |
| `-nt` | First file is newer than the second |
| `-ot` | First file is older than the second |

### 🖆 Checking Whether a File Exists

```bash
#!/usr/bin/env bash

file="/etc/hosts"

if [[ -e "$file" ]]; then
    echo "Path exists."
else
    echo "Path does not exist."
fi
```

### 🖆 Checking a Regular File

```bash
#!/usr/bin/env bash

file="/etc/passwd"

if [[ -f "$file" ]]; then
    echo "Regular file exists."
fi
```

### 🖆 Checking a Directory

```bash
#!/usr/bin/env bash

directory="/var/log"

if [[ -d "$directory" ]]; then
    echo "Directory exists."
fi
```

### 🖆 Checking File Permissions

```bash
#!/usr/bin/env bash

file="/etc/hosts"

if [[ -r "$file" ]]; then
    echo "File is readable."
fi

if [[ -w "$file" ]]; then
    echo "File is writable."
fi

if [[ -x "$file" ]]; then
    echo "File is executable."
fi
```

For directories, `-x` checks whether the current process can search or traverse the directory.

### 🖆 Checking Whether a File Is Empty

```bash
#!/usr/bin/env bash

file="data.txt"

if [[ -s "$file" ]]; then
    echo "File contains data."
else
    echo "File is empty or does not exist."
fi
```

### 🖆 Comparing File Modification Times

```bash
#!/usr/bin/env bash

source="source.txt"
backup="backup.txt"

if [[ "$source" -nt "$backup" ]]; then
    echo "Source file is newer."
fi
```

# 🗁 Logical Operators

Logical operators combine multiple conditions or reverse their results.

### 🖆 Common Logical Operators

| Operator | Meaning |
| --- | --- |
| `&&` | Logical AND |
| ` | | ` | Logical OR |
| `!` | Logical NOT |

### 🖆 Logical AND

Both conditions must succeed.

```bash
#!/usr/bin/env bash

age=25
country="Bangladesh"

if [[ "$age" -ge 18 && "$country" == "Bangladesh" ]]; then
    echo "Both conditions are satisfied."
fi
```

### 🖆 Logical OR

At least one condition must succeed.

```bash
#!/usr/bin/env bash

role="admin"

if [[ "$role" == "admin" || "$role" == "manager" ]]; then
    echo "Authorized role."
fi
```

### 🖆 Logical NOT

The `!` operator reverses a condition's result.

```bash
#!/usr/bin/env bash

file="config.txt"

if [[ ! -f "$file" ]]; then
    echo "Configuration file does not exist."
fi
```

### 🖆 Combining Multiple Conditions

```bash
#!/usr/bin/env bash

username="admin"
password="secret"

if [[ "$username" == "admin" && "$password" == "secret" ]]; then
    echo "Credentials matched."
else
    echo "Invalid credentials."
fi
```

This example demonstrates conditional syntax only. Real authentication systems should use secure credential storage and verification.

### 🖆 Logical Operators in Single-Bracket Tests

With `[ ]`, use separate test commands with shell operators:

```bash
if [ "$age" -ge 18 ] && [ "$age" -le 60 ]; then
    echo "Age is within range."
fi
```

The older `-a` and `-o` operators inside `test` expressions are difficult to combine safely because of parsing and precedence issues. Prefer `&&` and `||` between separate tests, or use `[[ ]]`.

# 🗁 Pattern Matching and Regular Expressions

Bash's `[[ ]]` supports pattern matching and regular-expression comparisons.

These features are useful for validating input, matching filenames, and identifying specific text formats.

### 🖆 Wildcard Pattern Matching

The `==` and `!=` operators support shell patterns inside `[[ ]]`.

| Pattern | Meaning |
| --- | --- |
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |
| `[abc]` | Matches one listed character |
| `[0-9]` | Matches one character in the specified range |

Example:

```bash
#!/usr/bin/env bash

filename="report.pdf"

if [[ "$filename" == *.pdf ]]; then
    echo "PDF document detected."
fi
```

### 🖆 Matching Multiple Extensions

```bash
#!/usr/bin/env bash

filename="document.txt"

if [[ "$filename" == *.txt || "$filename" == *.md ]]; then
    echo "Supported document format."
fi
```

### 🖆 Regular Expression Matching

The `=~` operator evaluates a POSIX extended regular expression.

```bash
#!/usr/bin/env bash

email="developer@example.com"

if [[ "$email" =~ ^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$ ]]; then
    echo "Email format looks valid."
else
    echo "Invalid email format."
fi
```

This expression checks a basic email-shaped pattern. It does not establish that an email address actually exists.

### 🖆 Capturing Regex Groups

Bash stores matched regular-expression groups in the `BASH_REMATCH` array.

```bash
#!/usr/bin/env bash

version="Ubuntu 24.04"

if [[ "$version" =~ Ubuntu[[:space:]]+([0-9]+\.[0-9]+) ]]; then
    echo "Distribution: Ubuntu"
    echo "Version: ${BASH_REMATCH[1]}"
fi
```

Output:

```text
Distribution: Ubuntu
Version: 24.04
```

### 🖆 Quoting Regular Expressions

For predictable behavior, keep the regular-expression pattern unquoted:

```bash
if [[ "$value" =~ ^[0-9]+$ ]]; then
    echo "Only digits."
fi
```

Quoting the entire right-hand expression makes it behave as a literal string in Bash versions with modern `=~` semantics.

# 🗁 Arithmetic Conditions

Bash supports arithmetic evaluation through `(( ))`.

It is especially convenient for integer comparisons and mathematical expressions.

### 🖆 Syntax

```bash
if (( arithmetic_expression )); then
    commands
fi
```

An arithmetic expression evaluates to a numeric result:

* Non-zero means true.
* Zero means false.

### 🖆 Comparing Numbers

```bash
#!/usr/bin/env bash

number=50

if (( number > 25 )); then
    echo "Number is greater than 25."
fi
```

### 🖆 Using Arithmetic Operators

| Operator | Meaning |
| --- | --- |
| `==` | Equal |
| `!=` | Not equal |
| `>` | Greater than |
| `>=` | Greater than or equal |
| `<` | Less than |
| `<=` | Less than or equal |
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Integer division |
| `%` | Modulus |

### 🖆 Checking Even or Odd Numbers

```bash
#!/usr/bin/env bash

number=17

if (( number % 2 == 0 )); then
    echo "Even number."
else
    echo "Odd number."
fi
```

### 🖆 Arithmetic with Variables

```bash
#!/usr/bin/env bash

first=20
second=30

if (( first + second > 40 )); then
    echo "The total exceeds 40."
fi
```

Arithmetic evaluation uses integer arithmetic. It does not provide floating-point calculations.

# 🗁 The case Statement

The `case` statement selects a command block based on pattern matching.

It is particularly useful when a variable can have several possible values.

### 🖆 Syntax

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

| Keyword | Purpose |
| --- | --- |
| `case` | Starts the selection statement |
| `in` | Introduces patterns |
| `)` | Starts a pattern's command block |
| `;;` | Ends a matching block |
| `*` | Default pattern |
| `esac` | Ends the statement |

### 🖆 Basic Example

```bash
#!/usr/bin/env bash

day="Monday"

case "$day" in
    Monday)
        echo "Start of the working week."
        ;;
    Friday)
        echo "End of the working week."
        ;;
    Saturday|Sunday)
        echo "Weekend."
        ;;
    *)
        echo "Regular working day."
        ;;
esac
```

### 🖆 Multiple Patterns

Use `|` to match multiple alternatives.

```bash
#!/usr/bin/env bash

extension="jpg"

case "$extension" in
    jpg|jpeg|png|gif)
        echo "Image file."
        ;;
    pdf)
        echo "PDF document."
        ;;
    txt|md)
        echo "Text document."
        ;;
    *)
        echo "Unknown file type."
        ;;
esac
```

### 🖆 Wildcard Patterns

```bash
#!/usr/bin/env bash

filename="report.pdf"

case "$filename" in
    *.pdf)
        echo "PDF file."
        ;;
    *.jpg|*.png)
        echo "Image file."
        ;;
    *)
        echo "Other file."
        ;;
esac
```

### 🖆 Fall-through Operators

Bash supports additional case terminators:

| Terminator | Behavior |
| --- | --- |
| `;;` | Stops evaluating the case statement after the matching block |
| `;&` | Executes the next block without checking its pattern |
| `;;&` | Continues checking subsequent patterns |

Example using `;&`:

```bash
#!/usr/bin/env bash

value="start"

case "$value" in
    start)
        echo "Starting..."
        ;&
    stop)
        echo "Stopping..."
        ;;
esac
```

Output:

```text
Starting...
Stopping...
```

Example using `;;&`:

```bash
#!/usr/bin/env bash

value="apple"

case "$value" in
    a*)
        echo "Starts with a."
        ;;&
    *e)
        echo "Ends with e."
        ;;
esac
```

Output:

```text
Starts with a.
Ends with e.
```

These fall-through terminators are Bash-specific extensions.

# 🗁 Nested Conditions

Nested conditions place one conditional statement inside another.

They are useful when a decision depends on multiple levels of evaluation.

### 🖆 Basic Example

```bash
#!/usr/bin/env bash

username="admin"
role="administrator"

if [[ "$username" == "admin" ]]; then
    if [[ "$role" == "administrator" ]]; then
        echo "Full access granted."
    else
        echo "Insufficient permissions."
    fi
else
    echo "Unknown user."
fi
```

### 🖆 File and Permission Validation

```bash
#!/usr/bin/env bash

file="deploy.sh"

if [[ -e "$file" ]]; then
    if [[ -f "$file" ]]; then
        if [[ -x "$file" ]]; then
            echo "Script exists and is executable."
        else
            echo "Script exists but is not executable."
        fi
    else
        echo "Path exists but is not a regular file."
    fi
else
    echo "Script does not exist."
fi
```

For complex conditions, consider combining tests or extracting validation into functions to improve readability.

# 🗁 Checking Command Success

Every command returns an exit status.

You can use that status directly inside an `if` statement without explicitly comparing `$?`.

### 🖆 Basic Syntax

```bash
if command; then
    commands_on_success
else
    commands_on_failure
fi
```

### 🖆 Checking Whether a Command Exists

Use `command -v` to determine whether a command is available in the current environment.

```bash
#!/usr/bin/env bash

if command -v git >/dev/null 2>&1; then
    echo "Git is installed."
else
    echo "Git is not installed."
fi
```

### 🖆 Checking a Directory Listing

```bash
#!/usr/bin/env bash

if ls /var/log >/dev/null 2>&1; then
    echo "Log directory is accessible."
else
    echo "Unable to list log directory."
fi
```

### 🖆 Checking a Service

On Ubuntu systems using systemd:

```bash
#!/usr/bin/env bash

if systemctl is-active --quiet ssh; then
    echo "SSH service is running."
else
    echo "SSH service is not running."
fi
```

### 🖆 Checking a Process

```bash
#!/usr/bin/env bash

if pgrep -x nginx >/dev/null; then
    echo "Nginx process is running."
else
    echo "Nginx process is not running."
fi
```

### 🖆 Using Exit Status Explicitly

The special parameter `$?` contains the exit status of the most recently executed command.

```bash
#!/usr/bin/env bash

mkdir -p backups

if [[ "$?" -eq 0 ]]; then
    echo "Directory created successfully."
else
    echo "Directory creation failed."
fi
```

Prefer testing the command directly:

```bash
if mkdir -p backups; then
    echo "Directory created successfully."
else
    echo "Directory creation failed."
fi
```

This avoids accidentally overwriting the exit status before checking it.

# 🗁 Practical Examples

The following examples combine conditional statements with common Ubuntu/Linux administration tasks.

### 🖆 Validate User Input

```bash
#!/usr/bin/env bash

read -r -p "Enter your name: " name

if [[ -z "$name" ]]; then
    echo "Name cannot be empty."
    exit 1
fi

echo "Welcome, $name!"
```

### 🖆 Validate Numeric Input

```bash
#!/usr/bin/env bash

read -r -p "Enter your age: " age

if [[ ! "$age" =~ ^[0-9]+$ ]]; then
    echo "Please enter a valid non-negative integer."
    exit 1
fi

if (( age >= 18 )); then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

### 🖆 Check and Create a Directory

```bash
#!/usr/bin/env bash

directory="$HOME/projects"

if [[ -d "$directory" ]]; then
    echo "Directory already exists."
else
    if mkdir -p "$directory"; then
        echo "Directory created successfully."
    else
        echo "Failed to create directory."
        exit 1
    fi
fi
```

### 🖆 Check and Create a Backup

```bash
#!/usr/bin/env bash

source="important.txt"
backup="important.txt.bak"

if [[ -f "$source" ]]; then
    if cp -- "$source" "$backup"; then
        echo "Backup created: $backup"
    else
        echo "Backup operation failed."
        exit 1
    fi
else
    echo "Source file does not exist."
    exit 1
fi
```

### 🖆 Select an Environment

```bash
#!/usr/bin/env bash

environment="${1:-development}"

case "$environment" in
    development)
        echo "Loading development configuration."
        ;;
    staging)
        echo "Loading staging configuration."
        ;;
    production)
        echo "Loading production configuration."
        ;;
    *)
        echo "Invalid environment: $environment"
        exit 1
        ;;
esac
```

Run:

```bash
bash deploy.sh development
bash deploy.sh staging
bash deploy.sh production
```

### 🖆 Check Whether a Package Is Installed

```bash
#!/usr/bin/env bash

package="nginx"

if dpkg-query -W -f='${Status}' "$package" 2>/dev/null | grep -q "install ok installed"; then
    echo "$package is installed."
else
    echo "$package is not installed."
fi
```

### 🖆 Check Available Disk Space

```bash
#!/usr/bin/env bash

available=$(df -P / | awk 'NR == 2 {gsub(/%/, "", $5); print 100 - $5}')

if (( available < 20 )); then
    echo "Warning: Less than 20% disk space is available."
else
    echo "Disk space is sufficient."
fi
```

This example uses `df` to retrieve filesystem usage and `awk` to calculate available space as a percentage.

# 🗁 Best Practices

* Prefer `[[ ]]` for Bash-specific string and file conditions.
* Use `(( ))` for integer arithmetic comparisons.
* Quote variable expansions when using `[ ]` and in command arguments.
* Use meaningful variable names that communicate their purpose.
* Keep nested conditions readable and avoid unnecessary complexity.
* Prefer `&&` and `||` over the older `-a` and `-o` test operators.
* Use `case` when selecting between multiple known values.
* Check command success directly with `if command; then`.
* Validate user input before using it in arithmetic expressions.
* Use explicit error messages and meaningful exit codes.
* Use `command -v` to check command availability.
* Keep Bash-specific syntax out of scripts that must run under a POSIX shell.
* Test scripts with different input values and failure conditions.

# 🗁 Common Mistakes

### 🖆 Missing Spaces in [ ]

Incorrect:

```bash
if ["$name" = "admin"]; then
    echo "Admin."
fi
```

Correct:

```bash
if [ "$name" = "admin" ]; then
    echo "Admin."
fi
```

### 🖆 Using Assignment Instead of Comparison

Incorrect:

```bash
if [[ "$name" = "admin" ]]; then
    name="admin"
fi
```

The assignment inside the block does not compare values. Use a condition for comparison and an assignment as a separate command.

Correct:

```bash
if [[ "$name" == "admin" ]]; then
    echo "Admin user."
fi
```

### 🖆 Using String Operators for Numeric Comparisons

Incorrect:

```bash
if [[ "$age" > 18 ]]; then
    echo "Adult."
fi
```

Correct:

```bash
if (( age > 18 )); then
    echo "Adult."
fi
```

### 🖆 Forgetting fi

Incorrect:

```bash
if [[ -f "config.txt" ]]; then
    echo "Configuration found."
```

Correct:

```bash
if [[ -f "config.txt" ]]; then
    echo "Configuration found."
fi
```

### 🖆 Forgetting to Quote Variables in [ ]

Incorrect:

```bash
if [ $name = "admin" ]; then
    echo "Admin."
fi
```

If `name` is empty or contains whitespace, the expression may be parsed incorrectly.

Correct:

```bash
if [ "$name" = "admin" ]; then
    echo "Admin."
fi
```

### 🖆 Misunderstanding Arithmetic Exit Status

```bash
if (( 0 )); then
    echo "True."
else
    echo "False."
fi
```

Output:

```text
False.
```

In arithmetic evaluation, zero is false and non-zero is true.

### 🖆 Assuming set -e Handles Every Error

The `set -e` option can cause a Bash script to exit when certain commands fail. However, its behavior has exceptions, especially inside conditional tests, loops, pipelines, and compound command lists.

Do not use it as a replacement for explicit error handling.

Prefer:

```bash
if cp -- source.txt backup.txt; then
    echo "Backup completed."
else
    echo "Backup failed."
    exit 1
fi
```

# 🗁 Practical Exercises

### 🖆 Exercise: Check a Number

Write a script that:

* Accepts a number from the user.
* Checks whether it is positive, negative, or zero.
* Prints the appropriate message.

### 🖆 Exercise: Even or Odd

Write a script that accepts an integer and determines whether it is even or odd.

### 🖆 Exercise: File Validation

Write a script that accepts a file path and checks whether:

* The path exists.
* It is a regular file.
* It is readable.
* It is executable.

### 🖆 Exercise: Directory Management

Write a script that accepts a directory path and:

* Checks whether the directory exists.
* Creates it if it does not exist.
* Displays an appropriate result.

### 🖆 Exercise: Environment Selection

Write a script that accepts `development`, `staging`, or `production` as an argument and displays the selected environment configuration.

### 🖆 Exercise: Service Monitoring

Write a script that checks whether the SSH service is active using `systemctl` and displays its status.

### 🖆 Exercise: Grade Calculator

Write a script that accepts a numeric score and assigns a grade:

| Score | Grade |
| --- | --- |
| 90–100 | A |
| 80–89 | B |
| 70–79 | C |
| 60–69 | D |
| Below 60 | F |

Validate that the score is an integer between 0 and 100.

# 🗁 Interview Questions

### 🖆 Fundamentals

* What are conditional statements in Bash?
* How does Bash determine whether a condition is true or false?
* What is the purpose of `if`, `then`, `else`, `elif`, and `fi`?
* What is the difference between `[ ]` and `[[ ]]`?
* What is the purpose of the `test` command?
* Why are spaces required around `[ ]`?

### 🖆 Comparisons and Operators

* What is the difference between `=` and `-eq`?
* What is the difference between `-z` and `-n`?
* What are `-gt`, `-lt`, `-ge`, and `-le`?
* How do `&&`, `||`, and `!` work?
* What is the difference between string comparison and arithmetic comparison?
* How does Bash perform wildcard pattern matching?

### 🖆 Advanced Concepts

* What is the difference between `[[ ]]` and `(( ))`?
* How does the `=~` operator work?
* What is `BASH_REMATCH`?
* What is the purpose of the `case` statement?
* What is the difference between `;;`, `;&`, and `;;&`?
* How can you check whether a command exists?
* How can you check whether a systemd service is active?
* Why should you avoid relying exclusively on `set -e`?

# 🗁 References

* [GNU Bash Manual — Conditional Constructs](https://www.gnu.org/software/bash/manual/html_node/Conditional-Constructs.html)
* [GNU Bash Manual — Bash Conditional Expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)
* [GNU Bash Manual — Shell Arithmetic](https://www.gnu.org/software/bash/manual/html_node/Shell-Arithmetic.html)
* [GNU Bash Manual — Pattern Matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html)
* [GNU Bash Manual — Conditional Expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)
* [Ubuntu Documentation](https://help.ubuntu.com/)
