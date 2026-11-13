# Zsh Shell in Ubuntu/Linux

Zsh (Z Shell) is a powerful Unix shell designed for interactive command-line usage and scripting. It extends traditional shell capabilities with advanced command completion, globbing, customization, spelling correction, and plugin support.

Zsh is popular among developers and system administrators because of its flexible configuration system and productivity-focused interactive features.

This chapter focuses on installing, configuring, and using Zsh on Ubuntu/Linux.

# 🗁 Introduction to Zsh

### 🖆 What Is Zsh?

Zsh stands for **Z Shell**.

It is a Unix shell that combines features from traditional shells with additional capabilities for interactive command execution and scripting.

Zsh supports:

* Interactive command execution.
* Shell scripting.
* Advanced command completion.
* Command-line editing.
* Powerful filename generation.
* Advanced globbing patterns.
* Spelling correction.
* Customizable prompts.
* Plugins and themes.
* Extensive shell configuration.

### 🖆 Why Use Zsh?

Zsh offers several productivity features that make everyday terminal usage more convenient.

| Feature | Description |
| --- | --- |
| Advanced Completion | Context-aware command completion |
| Powerful Globbing | Advanced file matching patterns |
| Spelling Correction | Suggests corrections for mistyped commands |
| Prompt Customization | Flexible prompt configuration |
| Shared History | Configurable history sharing across sessions |
| Plugins | Extensible interactive functionality |
| Themes | Customizable terminal appearance |
| Command Editing | Advanced command-line editing |

### 🖆 Zsh Use Cases

Zsh is commonly used for:

* Software development.
* Linux administration.
* Interactive terminal workflows.
* Git operations.
* Development environment customization.
* Command-line productivity.
* Shell scripting.

# 🗁 Zsh Architecture

### 🖆 How Zsh Works

Zsh acts as a command interpreter between the user and the operating system.

```text
                User
                  |
                  v
           Terminal Emulator
                  |
                  v
                 Zsh
                  |
        +---------+---------+
        |         |         |
        v         v         v
     Built-ins  Functions  External
                           Commands
        |         |         |
        +---------+---------+
                  |
                  v
             Linux Kernel
                  |
                  v
            System Resources
```

Zsh interprets commands, expands variables and patterns, processes redirections, and executes built-in or external commands.

### 🖆 Checking the Current Shell

Display the configured login shell:

```bash
echo "$SHELL"
```

Display the current process:

```bash
ps -p $$ -o pid,ppid,comm,args
```

Check whether the current shell is Zsh:

```zsh
if [[ -n "$ZSH_VERSION" ]]; then
    echo "Running Zsh: $ZSH_VERSION"
fi
```

`$SHELL` normally identifies the configured login shell. It does not necessarily identify the shell currently executing commands.

# 🗁 Installing Zsh on Ubuntu

### 🖆 Updating Package Information

```bash
sudo apt update
```

### 🖆 Installing Zsh

```bash
sudo apt install zsh
```

### 🖆 Checking the Installed Version

```bash
zsh --version
```

Example output:

```text
zsh 5.9
```

The exact version depends on the Ubuntu release and configured repositories.

### 🖆 Locating the Zsh Executable

```bash
command -v zsh
```

Inspect its location:

```bash
whereis zsh
```

### 🖆 Verifying the Installation

Start Zsh:

```bash
zsh
```

Display its version:

```zsh
echo "$ZSH_VERSION"
```

Exit:

```zsh
exit
```

# 🗁 Starting Zsh

### 🖆 Starting an Interactive Zsh Session

```bash
zsh
```

This starts a new Zsh process inside the current terminal.

### 🖆 Executing a Single Command

Use `-c`:

```bash
zsh -c 'echo "Hello from Zsh"'
```

Execute multiple commands:

```bash
zsh -c 'pwd; whoami; date'
```

### 🖆 Running a Zsh Script

Create a script:

```bash
nano hello.zsh
```

Add:

```zsh
#!/usr/bin/env zsh

echo "Hello from Ubuntu!"
echo "Current user: $USER"
echo "Current directory: $PWD"
```

Execute:

```bash
zsh hello.zsh
```

Alternatively, make it executable:

```bash
chmod +x hello.zsh
./hello.zsh
```

### 🖆 Starting Zsh Without User Configuration

Start Zsh without loading normal user configuration files:

```bash
zsh -f
```

This is useful for troubleshooting configuration problems and testing Zsh without plugins or custom settings.

# 🗁 Zsh Execution Modes

### 🖆 Interactive Zsh

An interactive Zsh session accepts commands directly from the user.

Start one:

```bash
zsh -i
```

Check whether the current session is interactive:

```zsh
[[ -o interactive ]] && echo "Interactive Zsh"
```

Interactive Zsh supports:

* Command history.
* Completion.
* Prompt customization.
* Aliases.
* Command-line editing.
* Job control.
* Spelling correction.

### 🖆 Non-Interactive Zsh

A non-interactive shell executes commands or scripts without presenting an interactive prompt.

Example:

```bash
zsh -c 'echo "Non-interactive execution"'
```

Execute a script:

```bash
zsh script.zsh
```

Typical use cases include:

* Automation.
* Scheduled jobs.
* Deployment scripts.
* Build pipelines.
* System maintenance.

### 🖆 Login Zsh

A login shell is started as part of a user login session.

Start a login Zsh session:

```bash
zsh -l
```

Check login-shell status:

```zsh
[[ -o login ]] && echo "Login shell" || echo "Non-login shell"
```

### 🖆 Non-Login Zsh

A non-login shell is commonly started inside an existing terminal session.

For example:

```bash
zsh
```

An interactive non-login Zsh session reads the interactive configuration file `~/.zshrc`.

### 🖆 Execution Mode Comparison

| Mode | Description |
| --- | --- |
| Interactive | Accepts commands from the user |
| Non-interactive | Executes supplied commands or scripts |
| Login | Reads login startup configuration |
| Non-login | Uses non-login startup behavior |

# 🗁 Zsh Configuration Files

### 🖆 User Configuration Directory

Zsh configuration files are normally stored in the user's home directory.

```text
~/
├── .zshenv
├── .zprofile
├── .zshrc
├── .zlogin
└── .zlogout
```

### 🖆 Understanding `.zshenv`

The `.zshenv` file is read by every Zsh invocation, including non-interactive shells.

It is intended for environment variables and configuration required by all Zsh sessions.

Example:

```zsh
export EDITOR="nano"
export LANG="en_US.UTF-8"
```

Keep this file lightweight because it is loaded frequently.

### 🖆 Understanding `.zprofile`

The `.zprofile` file is read by login Zsh sessions.

It is commonly used for login-specific environment configuration.

Example:

```zsh
export PATH="$HOME/bin:$PATH"
export APP_ENV="development"
```

### 🖆 Understanding `.zshrc`

The `.zshrc` file is read by interactive Zsh sessions.

It is commonly used for:

* Aliases.
* Functions.
* Completion configuration.
* Prompt customization.
* Plugin initialization.
* Interactive shell options.

Open it:

```bash
nano ~/.zshrc
```

Example:

```zsh
# Aliases
alias ll='ls -lah'
alias gs='git status'

# Environment variables
export EDITOR="nano"

# Interactive options
setopt AUTO_CD
setopt HIST_IGNORE_DUPS
```

Apply changes:

```zsh
source ~/.zshrc
```

### 🖆 Understanding `.zlogin`

The `.zlogin` file is read by login shells after `.zshrc`, when the shell is interactive.

It can contain commands that should run after interactive login initialization.

### 🖆 Understanding `.zlogout`

The `.zlogout` file is processed when a login Zsh session exits.

Example:

```zsh
echo "Zsh login session ended."
```

### 🖆 Configuration File Summary

| File | Purpose |
| --- | --- |
| `~/.zshenv` | Environment configuration for every Zsh invocation |
| `~/.zprofile` | Login shell configuration before `.zshrc` |
| `~/.zshrc` | Interactive shell configuration |
| `~/.zlogin` | Login shell configuration after `.zshrc` |
| `~/.zlogout` | Login shell logout configuration |

# 🗁 Zsh Startup Process

### 🖆 Login Interactive Shell

The general startup order is:

```text
/etc/zsh/zshenv
       |
       v
~/.zshenv
       |
       v
/etc/zsh/zprofile
       |
       v
~/.zprofile
       |
       v
/etc/zsh/zshrc
       |
       v
~/.zshrc
       |
       v
/etc/zsh/zlogin
       |
       v
~/.zlogin
```

System configuration files are read when present and applicable.

### 🖆 Interactive Non-Login Shell

The general startup sequence is:

```text
/etc/zsh/zshenv
       |
       v
~/.zshenv
       |
       v
/etc/zsh/zshrc
       |
       v
~/.zshrc
```

### 🖆 Non-Interactive Shell

A non-interactive Zsh session reads:

```text
/etc/zsh/zshenv
       |
       v
~/.zshenv
```

It does not normally load `.zshrc`.

### 🖆 Login Shell Logout

When a login shell exits, Zsh processes logout configuration:

```text
/etc/zsh/zlogout
       |
       v
~/.zshlogout
```

The exact system-level files depend on the Ubuntu installation and its configuration.

# 🗁 Zsh Variables

### 🖆 Creating Variables

Zsh variables can be assigned without spaces around `=`.

```zsh
name="Muhammad"
project="Hands-on-Notes"
version="1.0"
```

Display values:

```zsh
echo "$name"
echo "$project"
```

### 🖆 Variable Naming Rules

Variable names generally:

* Begin with a letter or underscore.
* Contain letters, numbers, and underscores.
* Do not contain spaces.
* Are case-sensitive.

Examples:

```zsh
username="admin"
APP_ENV="development"
_file_path="/tmp/example.txt"
```

### 🖆 Read-Only Variables

Use `readonly`:

```zsh
readonly APP_NAME="Hands-on-Notes"
```

### 🖆 Removing Variables

Use `unset`:

```zsh
unset name
```

### 🖆 Displaying Variables

```zsh
typeset
```

Display a specific variable:

```zsh
typeset -p APP_ENV
```

# 🗁 Environment Variables

### 🖆 Exporting Variables

Use `export` to make a variable available to child processes.

```zsh
export APP_ENV="development"
```

Display it:

```zsh
printenv APP_ENV
```

### 🖆 Common Environment Variables

| Variable | Description |
| --- | --- |
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | Configured login shell |
| `PATH` | Executable search directories |
| `PWD` | Current working directory |
| `OLDPWD` | Previous working directory |
| `EDITOR` | Preferred text editor |
| `LANG` | Locale setting |
| `TERM` | Terminal type |
| `ZSH_VERSION` | Current Zsh version |
| `ZDOTDIR` | Directory containing user Zsh configuration files |

### 🖆 Configuring PATH

Add a directory to the executable search path:

```zsh
export PATH="$HOME/bin:$PATH"
```

Check the path:

```zsh
echo "$PATH"
```

Zsh also supports a special array named `path`, which corresponds to the `PATH` environment variable.

Example:

```zsh
path=("$HOME/bin" $path)
```

This array-based approach is convenient because Zsh automatically maintains the relationship between `path` and `PATH`.

# 🗁 Zsh Built-in Commands

### 🖆 Common Built-ins

| Command | Purpose |
| --- | --- |
| `cd` | Change directory |
| `pwd` | Display working directory |
| `echo` | Display text |
| `printf` | Format output |
| `read` | Read user input |
| `export` | Export variables |
| `unset` | Remove variables |
| `typeset` | Declare variables and attributes |
| `alias` | Define aliases |
| `unalias` | Remove aliases |
| `source` | Execute a configuration file in the current shell |
| `eval` | Parse and execute a command string |
| `functions` | Inspect function definitions |
| `setopt` | Enable shell options |
| `unsetopt` | Disable shell options |
| `jobs` | Display active jobs |
| `fg` | Bring a job to the foreground |
| `bg` | Resume a job in the background |
| `wait` | Wait for background processes |

### 🖆 Inspecting Commands

Identify a command:

```zsh
type cd
```

Display command resolution:

```zsh
whence -v ls
```

Display Zsh built-ins:

```zsh
builtin
```

# 🗁 Aliases and Functions

### 🖆 Creating Aliases

```zsh
alias ll='ls -lah'
alias gs='git status'
alias ..='cd ..'
```

Display aliases:

```zsh
alias
```

Remove an alias:

```zsh
unalias gs
```

### 🖆 Creating Functions

```zsh
greet() {
    echo "Hello from Zsh!"
}
```

Execute:

```zsh
greet
```

### 🖆 Function Arguments

```zsh
greet() {
    echo "Hello, $1!"
}

greet Muhammad
```

### 🖆 Local Variables

Use `local` or `typeset` inside functions to create local variables.

```zsh
greet() {
    local name="Muhammad"
    echo "Hello, $name"
}

greet
```

### 🖆 Persistent Configuration

Add aliases and functions to:

```text
~/.zshrc
```

Reload:

```zsh
source ~/.zshrc
```

# 🗁 Zsh Arrays

### 🖆 Indexed Arrays

Zsh supports indexed arrays.

```zsh
fruits=("Apple" "Banana" "Orange")
```

Unlike Bash's default indexed arrays, Zsh arrays use **one-based indexing**.

Access elements:

```zsh
echo "$fruits[1]"
echo "$fruits[2]"
```

Display all elements:

```zsh
printf '%s\n' "${fruits[@]}"
```

Get the number of elements:

```zsh
echo "${#fruits[@]}"
```

### 🖆 Adding Elements

```zsh
fruits+=("Mango")
```

Display the updated array:

```zsh
printf '%s\n' "${fruits[@]}"
```

### 🖆 Associative Arrays

Declare an associative array:

```zsh
typeset -A user

user=(
    name Muhammad
    role Developer
    language Zsh
)
```

Access values:

```zsh
echo "${user[name]}"
echo "${user[role]}"
```

Display keys:

```zsh
echo "${(k)user}"
```

Display values:

```zsh
echo "${(v)user}"
```

# 🗁 Zsh Expansion

### 🖆 Parameter Expansion

Display a variable:

```zsh
name="Muhammad"

echo "${name}"
```

Provide a default value:

```zsh
echo "${APP_ENV:-development}"
```

Display the length:

```zsh
echo "${#name}"
```

### 🖆 Command Substitution

Capture command output:

```zsh
current_date=$(date)

echo "$current_date"
```

Zsh also supports backtick command substitution:

```zsh
current_user=`whoami`
```

The `$()` form is generally preferred because it is easier to read and nest.

### 🖆 Arithmetic Expansion

```zsh
a=10
b=20

result=$((a + b))

echo "$result"
```

### 🖆 Brace Expansion

```zsh
echo file{1,2,3}.txt
```

Create directories:

```zsh
mkdir -p project/{src,tests,docs}
```

### 🖆 Tilde Expansion

```zsh
cd ~
```

Access a user's home directory:

```zsh
echo ~
```

### 🖆 Process Substitution

```zsh
diff <(sort file1.txt) <(sort file2.txt)
```

Process substitution passes command output through a filename-like interface.

# 🗁 Advanced Globbing

### 🖆 Basic Wildcards

Zsh supports wildcard patterns for filename matching.

| Pattern | Meaning |
| --- | --- |
| `*` | Matches any sequence of characters |
| `?` | Matches one character |
| `[abc]` | Matches one listed character |
| `[a-z]` | Matches a character in a range |
| `[!abc]` | Matches a character not listed |

Examples:

```zsh
ls *.txt
ls file?.txt
ls [abc]*.log
```

### 🖆 Recursive Globbing

Zsh supports recursive directory matching using `**/`.

Find Markdown files recursively:

```zsh
print -l **/*.md
```

Find files in the current directory and subdirectories:

```zsh
print -l **/*
```

### 🖆 Extended Globbing

Zsh supports advanced pattern operators.

Enable extended globbing:

```zsh
setopt EXTENDED_GLOB
```

Examples:

```zsh
print -l *.txt
print -l ^*.txt
print -l *.(txt|md)
```

These patterns can match files based on more sophisticated conditions.

### 🖆 Globbing Differences from Bash

Zsh and Bash differ in their default filename expansion behavior.

For example, unmatched wildcard patterns are normally left unchanged by Bash, whereas Zsh normally reports a `no matches found` error.

Use the following option to allow unmatched patterns to remain unchanged:

```zsh
setopt NONOMATCH
```

Alternatively, use `NULL_GLOB` when unmatched patterns should expand to nothing:

```zsh
setopt NULL_GLOB
```

# 🗁 Command Completion

### 🖆 Initializing Completion

Zsh provides a powerful completion system called `compsys`.

Initialize it in `~/.zshrc`:

```zsh
autoload -Uz compinit
compinit
```

Reload:

```zsh
source ~/.zshrc
```

### 🖆 Basic Completion

Press:

```text
Tab
```

Zsh can suggest:

* Commands.
* Files.
* Directories.
* Options.
* Command arguments.
* Users.
* Processes.
* Git-related values when appropriate completion definitions are installed.

### 🖆 Completion Menu

Enable menu-based completion:

```zsh
zstyle ':completion:*' menu select
```

Press `Tab` to display suggestions and navigate the available choices.

### 🖆 Completion Configuration

Example configuration:

```zsh
autoload -Uz compinit
compinit

zstyle ':completion:*' menu select
zstyle ':completion:*' matcher-list 'm:{a-z}={A-Za-z}'
```

The matcher configuration allows case-insensitive matching during completion.

# 🗁 Command History

### 🖆 Displaying History

```zsh
history
```

Display recent commands:

```zsh
history 20
```

Search history:

```zsh
history | grep docker
```

### 🖆 History Configuration

Zsh commonly stores history in:

```text
~/.zsh_history
```

Configure history settings in `~/.zshrc`:

```zsh
HISTFILE="$HOME/.zsh_history"
HISTSIZE=10000
SAVEHIST=10000
```

### 🖆 Useful History Options

```zsh
setopt HIST_IGNORE_DUPS
setopt HIST_IGNORE_SPACE
setopt HIST_SAVE_NO_DUPS
setopt SHARE_HISTORY
setopt APPEND_HISTORY
```

| Option | Purpose |
| --- | --- |
| `HIST_IGNORE_DUPS` | Avoid consecutive duplicate commands |
| `HIST_IGNORE_SPACE` | Ignore commands beginning with a space |
| `HIST_SAVE_NO_DUPS` | Avoid saving duplicate history entries |
| `SHARE_HISTORY` | Share history between interactive sessions |
| `APPEND_HISTORY` | Append history instead of replacing the history file |

### 🖆 Searching History

Use:

```text
Ctrl + R
```

This provides reverse incremental history searching in typical interactive configurations.

# 🗁 Zsh Options

### 🖆 Enabling Options

Use `setopt`:

```zsh
setopt AUTO_CD
setopt AUTO_PUSHD
setopt CORRECT
```

### 🖆 Disabling Options

Use `unsetopt`:

```zsh
unsetopt CORRECT
```

### 🖆 Useful Interactive Options

| Option | Purpose |
| --- | --- |
| `AUTO_CD` | Change directories by entering directory names |
| `AUTO_PUSHD` | Automatically add directories to the directory stack |
| `CORRECT` | Suggest spelling corrections for commands |
| `EXTENDED_GLOB` | Enable advanced globbing |
| `HIST_IGNORE_DUPS` | Ignore consecutive duplicate history entries |
| `INTERACTIVE_COMMENTS` | Allow comments in interactive commands |
| `NO_BEEP` | Disable terminal bell behavior |
| `SHARE_HISTORY` | Share command history between sessions |

### 🖆 Example Configuration

Add to `~/.zshrc`:

```zsh
setopt AUTO_CD
setopt AUTO_PUSHD
setopt CORRECT
setopt EXTENDED_GLOB
setopt HIST_IGNORE_DUPS
setopt INTERACTIVE_COMMENTS
setopt NO_BEEP
```

# 🗁 Prompt Customization

### 🖆 Understanding the Zsh Prompt

Zsh uses the `PROMPT` or `PS1` variable to define the primary command prompt.

Example:

```zsh
PROMPT='%n@%m:%~%# '
```

Common prompt escape sequences:

| Sequence | Meaning |
| --- | --- |
| `%n` | Current username |
| `%m` | Hostname |
| `%M` | Full hostname |
| `%~` | Current directory, abbreviated using `~` |
| `%/` | Full current directory |
| `%#` | Prompt character indicating privilege context |
| `%T` | Current time |
| `%D` | Current date |

### 🖆 Custom Prompt Example

```zsh
PROMPT='%F{green}%n@%m%f:%F{blue}%~%f %# '
```

This configures a colored prompt containing the username, hostname, and current directory.

### 🖆 Right-Side Prompt

Zsh supports a right-side prompt using `RPROMPT`.

```zsh
RPROMPT='%F{yellow}%T%f'
```

This displays the current time on the right side of the terminal prompt when the terminal supports it.

# 🗁 Plugins and Frameworks

### 🖆 What Are Zsh Plugins?

Plugins extend Zsh with additional functionality.

They can provide:

* Git shortcuts.
* Command completion.
* Syntax highlighting.
* Autosuggestions.
* Prompt enhancements.
* Development workflow utilities.

### 🖆 Oh My Zsh

<text> <escape> </escape> </text>

<Entity value="Oh My Zsh" category="software" disambig="Zsh configuration framework and plugin manager"/> is a community-driven framework for managing Zsh configuration.

Official website:

<https://ohmyz.sh/>

It provides:

* A collection of themes.
* Built-in plugins.
* Configuration helpers.
* Git integration.
* A structured configuration environment.

### 🖆 Zsh Autosuggestions

Autosuggestions display suggested commands based on command history.

Official repository:

<https://github.com/zsh-users/zsh-autosuggestions>

### 🖆 Zsh Syntax Highlighting

Syntax highlighting provides visual feedback while typing commands.

Official repository:

<https://github.com/zsh-users/zsh-syntax-highlighting>

### 🖆 Managing Plugins

Plugins are generally enabled through the Zsh configuration file.

For example, a framework-based configuration may contain:

```zsh
plugins=(
    git
    sudo
    command-not-found
)
```

The available plugins depend on the installed framework.

Avoid installing many unnecessary plugins because each plugin can increase shell startup time and configuration complexity.

# 🗁 Zsh Scripting

### 🖆 Creating a Zsh Script

Create a file:

```bash
nano system-info.zsh
```

Add:

```zsh
#!/usr/bin/env zsh

echo "System Information"

echo "User: $USER"
echo "Hostname: $(hostname)"
echo "Current directory: $PWD"
echo "Date: $(date)"
```

### 🖆 Executing the Script

Run directly through Zsh:

```bash
zsh system-info.zsh
```

Or make it executable:

```bash
chmod +x system-info.zsh
./system-info.zsh
```

### 🖆 Reading User Input

```zsh
#!/usr/bin/env zsh

read "username?Enter your username: "

echo "Welcome, $username!"
```

### 🖆 Using Arrays in Scripts

```zsh
#!/usr/bin/env zsh

languages=("Bash" "Zsh" "Python" "JavaScript")

for language in "${languages[@]}"; do
    echo "$language"
done
```

### 🖆 Checking Script Arguments

```zsh
#!/usr/bin/env zsh

echo "Script: $0"
echo "First argument: $1"
echo "Total arguments: $#"
```

Execute:

```bash
zsh script.zsh Ubuntu Linux
```

Zsh has its own scripting syntax and expansion behavior. Bash scripts should not automatically be assumed to work unchanged under Zsh.

# 🗁 Bash vs Zsh

### 🖆 Key Differences

| Feature | Bash | Zsh |
| --- | --- | --- |
| Full Name | Bourne Again SHell | Z Shell |
| Primary Strength | General-purpose shell and scripting | Interactive productivity and customization |
| Default Array Index | Zero-based | One-based |
| Associative Arrays | Supported | Supported |
| Completion | Programmable completion | Advanced completion system |
| Globbing | Standard and extended patterns | More extensive native globbing |
| Prompt Customization | `PS1` | `PROMPT`, `RPROMPT` |
| Configuration | `.bashrc`, `.profile` | `.zshrc`, `.zprofile`, `.zshenv` |
| Plugin Ecosystem | Available | Extensive ecosystem |
| Ubuntu Default Login Shell | Commonly Bash | Requires installation and selection |

### 🖆 Array Indexing Example

Bash:

```bash
fruits=("Apple" "Banana")

echo "${fruits[0]}"
```

Zsh:

```zsh
fruits=("Apple" "Banana")

echo "${fruits[1]}"
```

### 🖆 Configuration File Differences

| Purpose | Bash | Zsh |
| --- | --- | --- |
| Interactive Configuration | `~/.bashrc` | `~/.zshrc` |
| Login Configuration | `~/.profile` or `~/.bash_profile` | `~/.zprofile` |
| Environment Initialization | Login configuration and `BASH_ENV` | `~/.zshenv` |
| Logout Configuration | `~/.bash_logout` | `~/.zlogout` |

### 🖆 Which Shell Should You Use?

Choose Bash when:

* You need compatibility with common Linux environments.
* You are writing portable automation for systems that provide Bash.
* You are following existing Linux administration documentation.
* Your deployment scripts explicitly require Bash.

Choose Zsh when:

* You want advanced interactive completion.
* You prefer extensive prompt customization.
* You use advanced filename matching.
* You want a highly customizable development terminal.

Both shells are capable scripting environments. The best choice depends on compatibility requirements and workflow preferences.

# 🗁 Best Practices

* Use `#!/usr/bin/env zsh` for Zsh-specific scripts.
* Keep environment initialization in `~/.zshenv` minimal.
* Keep interactive settings in `~/.zshrc`.
* Use `.zprofile` for login-specific configuration.
* Quote variable expansions when word splitting and globbing are not intended.
* Use `printf` for predictable output.
* Use arrays for collections of arguments and values.
* Keep aliases simple and use functions for reusable logic.
* Enable only the options you understand and need.
* Keep plugin installations minimal.
* Avoid putting expensive commands in frequently loaded startup files.
* Test scripts using Zsh explicitly.
* Do not assume Bash and Zsh syntax are interchangeable.
* Keep configuration files organized and documented.

# 🗁 Common Mistakes

### 🖆 Confusing Bash and Zsh Array Indexing

Bash normally starts indexed arrays at zero, while Zsh starts them at one.

Always account for this difference when porting scripts.

### 🖆 Editing the Wrong Configuration File

Adding interactive configuration to `.zshenv` can affect non-interactive scripts.

Use `.zshrc` for interactive configuration.

### 🖆 Forgetting to Initialize Completion

Zsh completion generally requires initialization:

```zsh
autoload -Uz compinit
compinit
```

### 🖆 Unmatched Globbing Patterns

An unmatched wildcard pattern normally produces an error in Zsh.

Use `NULL_GLOB` or `NONOMATCH` when their respective behaviors are appropriate.

### 🖆 Installing Too Many Plugins

Large plugin collections can slow down startup and make configuration difficult to maintain.

Install only the functionality you actually use.

### 🖆 Assuming `$SHELL` Identifies the Current Shell

`$SHELL` commonly contains the configured login shell path.

Use process inspection or shell-specific variables to identify the currently running shell.

# 🗁 Practical Exercises

### 🖆 Installation and Configuration

* Install Zsh on Ubuntu.
* Check the installed version.
* Locate the Zsh executable.
* Start an interactive Zsh session.
* Identify whether it is a login shell.
* Inspect the Zsh startup configuration files.

### 🖆 Variables and Functions

* Create several variables.
* Export an environment variable.
* Create a function that accepts two arguments.
* Declare a local variable inside a function.
* Create and remove aliases.

### 🖆 Arrays and Expansion

* Create an indexed array containing five programming languages.
* Display its elements and length.
* Create an associative array containing user information.
* Experiment with parameter expansion.
* Use command substitution.
* Create directories using brace expansion.

### 🖆 Advanced Interactive Features

* Enable `AUTO_CD`.
* Enable `EXTENDED_GLOB`.
* Configure command completion.
* Customize the primary prompt.
* Configure a right-side prompt.
* Enable history sharing.
* Test reverse history search.

### 🖆 Plugin Management

* Install a Zsh configuration framework.
* Enable the Git plugin.
* Configure autosuggestions.
* Enable syntax highlighting.
* Compare startup performance before and after installing plugins.

### 🖆 Scripting

* Create a Zsh system-information script.
* Read input from the user.
* Process command-line arguments.
* Iterate over an array.
* Create a reusable function.
* Execute the script directly using its shebang.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

* What is Zsh?
* What are the advantages of Zsh?
* How is Zsh different from Bash?
* How do you install Zsh on Ubuntu?
* How do you identify the currently running shell?

### 🖆 Configuration Questions

* What is the purpose of `.zshrc`?
* What is the difference between `.zshenv` and `.zprofile`?
* When does Zsh load `.zlogin`?
* What is the purpose of `.zlogout`?
* What is the Zsh startup sequence?

### 🖆 Programming Questions

* How are variables declared in Zsh?
* What is the difference between indexed and associative arrays?
* How does Zsh array indexing differ from Bash?
* What is parameter expansion?
* What is process substitution?
* How do functions work in Zsh?

### 🖆 Advanced Questions

* What is Zsh's completion system?
* What is `compinit`?
* What is the difference between `setopt` and `unsetopt`?
* What is extended globbing?
* What is the purpose of `AUTO_CD`?
* What is Oh My Zsh?
* How does Zsh history sharing work?
* Why can Bash scripts behave differently when executed with Zsh?

# 🗁 References

* [Official Zsh Website](https://www.zsh.org/)
* [Zsh Documentation](https://zsh.sourceforge.io/Doc/)
* [Zsh User Guide](https://zsh.sourceforge.io/Guide/)
* [Zsh FAQ](https://zsh.sourceforge.io/FAQ/)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
* [Oh My Zsh](https://ohmyz.sh/)
* [Zsh Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
* [Zsh Syntax Highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
