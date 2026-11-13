# Terminal Shortcuts in Ubuntu/Linux

Terminal shortcuts improve productivity by reducing repetitive typing and making command-line operations faster.

In Ubuntu/Linux, terminal keyboard shortcuts are commonly provided by the terminal emulator and the active shell. Bash also provides its own command-line editing shortcuts through the Readline library.

This chapter focuses on Ubuntu Terminal, Bash, command-line navigation, text editing, process control, and command history.

# 🗁 Understanding Terminal Shortcuts

### 🖆 What Are Terminal Shortcuts?

Terminal shortcuts are keyboard combinations or key sequences that execute common actions without requiring additional commands.

For example:

* `Ctrl + C` interrupts a running foreground command.
* `Ctrl + L` clears the visible terminal screen.
* `Ctrl + R` searches command history.
* `Ctrl + A` moves the cursor to the beginning of the command line.
* `Ctrl + E` moves the cursor to the end of the command line.

### 🖆 Terminal Emulator vs Shell Shortcuts

Not every shortcut is handled by the same component.

| Component | Responsibility |
| --- | --- |
| Terminal Emulator | Terminal windows, tabs, copy, paste, and display |
| Bash | Command-line interaction and job control |
| Readline | Editing and navigating the command line |
| Linux Kernel | Processing terminal signals and input events |

For example, `Ctrl + C` is interpreted by the terminal's line discipline as an interrupt character in the usual terminal configuration. The resulting `SIGINT` is sent to the foreground process group.

Bash command-line editing shortcuts are generally provided by GNU Readline.

# 🗁 Basic Terminal Shortcuts

### 🖆 Essential Shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl + C` | Interrupt the foreground process |
| `Ctrl + D` | Send EOF or exit an interactive shell when appropriate |
| `Ctrl + L` | Clear the visible terminal screen |
| `Ctrl + Z` | Suspend the foreground process |
| `Ctrl + S` | Pause terminal output in terminals using software flow control |
| `Ctrl + Q` | Resume terminal output |
| `Ctrl + U` | Delete text before the cursor |
| `Ctrl + K` | Delete text after the cursor |
| `Ctrl + W` | Delete the previous word |
| `Ctrl + Y` | Paste the most recently killed text |
| `Ctrl + R` | Search command history backward |

### 🖆 Interrupting a Command

Use:

```text
Ctrl + C
```

Example:

```bash
ping google.com
```

Press `Ctrl + C` to interrupt the command.

This normally sends `SIGINT` to the foreground process group.

### 🖆 Exiting an Interactive Shell

Use:

```text
Ctrl + D
```

When entered at an empty Bash prompt, this signals end-of-file (EOF) and normally exits the shell.

It can also indicate EOF to commands reading from standard input.

### 🖆 Clearing the Terminal Screen

Use:

```text
Ctrl + L
```

Equivalent command:

```bash
clear
```

This clears the visible terminal display. It does not delete command history or terminal scrollback.

# 🗁 Command-Line Navigation

### 🖆 Moving the Cursor

| Shortcut | Action |
| --- | --- |
| `Ctrl + A` | Move to the beginning of the line |
| `Ctrl + E` | Move to the end of the line |
| `Alt + B` | Move backward one word |
| `Alt + F` | Move forward one word |
| `Ctrl + B` | Move backward one character |
| `Ctrl + F` | Move forward one character |

Example command:

```bash
sudo apt install nginx
```

Use `Ctrl + A` to move to the beginning of the command.

Use `Ctrl + E` to move to the end.

### 🖆 Moving Between Words

Given this command:

```bash
mkdir /home/user/projects/my-app
```

* `Alt + B` moves backward by one word.
* `Alt + F` moves forward by one word.

Depending on the terminal emulator and keyboard configuration, `Alt` combinations may require pressing `Esc` first.

For example, `Esc` followed by `B` performs the backward-word operation in Bash Readline.

### 🖆 Moving Through Command History

| Shortcut | Action |
| --- | --- |
| `Up Arrow` | Previous command |
| `Down Arrow` | Next command |
| `Ctrl + P` | Previous history entry |
| `Ctrl + N` | Next history entry |

These shortcuts allow you to reuse previously executed commands without typing them again.

# 🗁 Command-Line Editing

### 🖆 Deleting Text

| Shortcut | Action |
| --- | --- |
| `Backspace` | Delete the character before the cursor |
| `Delete` | Delete the character under the cursor |
| `Ctrl + H` | Delete the previous character |
| `Ctrl + D` | Delete the character under the cursor when the line is non-empty |
| `Ctrl + W` | Delete the previous word |
| `Alt + D` | Delete the next word |
| `Ctrl + U` | Delete from the cursor to the beginning of the line |
| `Ctrl + K` | Delete from the cursor to the end of the line |

Example:

```bash
sudo apt install apache2
```

Place the cursor after `install` and press `Ctrl + K` to remove the remaining text.

### 🖆 Copying and Pasting Within the Command Line

Bash Readline provides its own kill-and-yank operations.

| Shortcut | Action |
| --- | --- |
| `Ctrl + K` | Cut text from cursor to line end |
| `Ctrl + U` | Cut text from cursor to line beginning |
| `Ctrl + W` | Cut the previous word |
| `Alt + D` | Cut the next word |
| `Ctrl + Y` | Paste the most recently killed text |

Example:

```bash
sudo apt install nginx
```

* Move the cursor to the beginning of `nginx`.
* Press `Alt + D` to remove the next word.
* Move to the desired position.
* Press `Ctrl + Y` to restore the removed text.

These operations use the Readline kill ring, not the desktop clipboard.

# 🗁 Command History

### 🖆 Searching Command History

Use:

```text
Ctrl + R
```

This starts an incremental reverse search through Bash history.

Example:

```text
(reverse-i-search)`apt': sudo apt install nginx
```

Type a keyword to search previous commands.

Press `Ctrl + R` again to search further backward.

Press `Enter` to execute the displayed command, or use an editing key to bring it back to the prompt for modification.

### 🖆 Searching Forward

During an active reverse search, use:

```text
Ctrl + S
```

to search forward in history, provided terminal flow control is not intercepting the shortcut.

If `Ctrl + S` pauses terminal output, disable software flow control or use another Readline search method.

### 🖆 Reusing Previous Commands

Execute the previous command:

```bash
!!
```

Execute a command by its history number:

```bash
!25
```

Execute the most recent command beginning with a specific string:

```bash
!sudo
```

Display command history:

```bash
history
```

Search history:

```bash
history | grep apt
```

### 🖆 Editing the Previous Command

Use:

```text
Ctrl + P
```

to retrieve the previous command.

Modify it using Readline shortcuts before pressing `Enter`.

You can also use:

```bash
fc
```

to edit a previous command using the configured editor.

# 🗁 Terminal Screen Management

### 🖆 Clear the Terminal

```text
Ctrl + L
```

Or:

```bash
clear
```

### 🖆 Scroll Through Terminal Output

In Ubuntu's graphical terminal emulator, common scrolling shortcuts include:

| Shortcut | Action |
| --- | --- |
| `Shift + Page Up` | Scroll upward |
| `Shift + Page Down` | Scroll downward |
| `Ctrl + Shift + C` | Copy selected terminal text |
| `Ctrl + Shift + V` | Paste text into the terminal |

These shortcuts are commonly used in GNOME Terminal. Other terminal emulators may use different key bindings.

### 🖆 Open a New Terminal Window or Tab

Common GNOME Terminal shortcuts:

| Shortcut | Action |
| --- | --- |
| `Ctrl + Shift + T` | Open a new tab |
| `Ctrl + Shift + N` | Open a new terminal window |
| `Ctrl + Page Up` | Switch to the previous tab |
| `Ctrl + Page Down` | Switch to the next tab |
| `Alt + 1` through `Alt + 9` | Switch to a numbered tab |

### 🖆 Detaching Terminal Output from the Screen

To inspect a long command without flooding the terminal, use a pager:

```bash
ps aux | less
```

Useful `less` shortcuts:

| Shortcut | Action |
| --- | --- |
| `Space` | Next page |
| `b` | Previous page |
| `g` | Beginning of output |
| `G` | End of output |
| `/keyword` | Search forward |
| `n` | Next search result |
| `q` | Quit |

# 🗁 Process Control Shortcuts

### 🖆 Interrupt a Foreground Process

```text
Ctrl + C
```

Sends `SIGINT` to the foreground process group under normal terminal settings.

### 🖆 Suspend a Foreground Process

```text
Ctrl + Z
```

Normally sends `SIGTSTP` to the foreground process group.

Resume the suspended process in the background:

```bash
bg
```

Bring it back to the foreground:

```bash
fg
```

### 🖆 Pause Terminal Output

```text
Ctrl + S
```

In terminals with software flow control enabled, this pauses output.

Resume output:

```text
Ctrl + Q
```

If the terminal appears frozen after pressing `Ctrl + S`, press `Ctrl + Q`.

### 🖆 Display Background Jobs

```bash
jobs
```

Display jobs with process IDs:

```bash
jobs -l
```

# 🗁 Tab Completion

### 🖆 Completing Commands and Paths

Press:

```text
Tab
```

Bash attempts to complete the current command, filename, directory, or other supported word.

Example:

```bash
cd /var/lo
```

Press `Tab` to complete the path if it is unambiguous.

### 🖆 Listing Available Completions

If multiple matches exist, press `Tab` twice to display available completions in the standard Bash configuration.

Example:

```bash
cd /usr/bi
```

Press `Tab` twice to see matching paths.

### 🖆 Completing Commands

Start typing a command:

```bash
sys
```

Press `Tab` twice to display matching command names, depending on the installed commands and Bash completion configuration.

### 🖆 Completing Package Names

Ubuntu commonly includes Bash completion support for many commands.

For example:

```bash
sudo apt install ng
```

Press `Tab` to request possible completions.

Package-name completion depends on the installed completion configuration and available package metadata.

# 🗁 Useful Bash Keyboard Shortcuts

### 🖆 Frequently Used Readline Shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl + A` | Beginning of line |
| `Ctrl + E` | End of line |
| `Ctrl + B` | Backward character |
| `Ctrl + F` | Forward character |
| `Alt + B` | Backward word |
| `Alt + F` | Forward word |
| `Ctrl + U` | Kill to beginning of line |
| `Ctrl + K` | Kill to end of line |
| `Ctrl + W` | Kill previous word |
| `Alt + D` | Kill next word |
| `Ctrl + Y` | Yank killed text |
| `Ctrl + L` | Clear screen |
| `Ctrl + R` | Reverse history search |
| `Ctrl + P` | Previous history entry |
| `Ctrl + N` | Next history entry |
| `Ctrl + T` | Transpose characters |
| `Alt + T` | Transpose words |
| `Ctrl + _` | Undo the previous editing operation |

### 🖆 Character Transposition

The `Ctrl + T` shortcut swaps the character immediately before the cursor with the character at the cursor, subject to the current cursor position.

For example, if you accidentally type:

```text
gti
```

You can use `Ctrl + T` to correct the character order when the cursor is positioned appropriately.

### 🖆 Word Transposition

`Alt + T` swaps the previous word with the word at the cursor position.

This can help correct word order without deleting and retyping the command.

# 🗁 Customizing Bash Shortcuts

### 🖆 Readline Configuration

Bash uses GNU Readline for command-line editing.

Its configuration file is:

```text
~/.inputrc
```

You can define custom key bindings in this file.

Example:

```bash
nano ~/.inputrc
```

Add:

```text
"\C-x\C-e": beginning-of-line
"\C-x\C-r": reverse-search-history
```

These examples bind key sequences to Readline editing functions.

Reload the configuration by restarting Bash or using:

```bash
bind -f ~/.inputrc
```

### 🖆 Changing the Editing Mode

Readline supports Emacs-style and Vi-style editing modes.

Set Emacs mode:

```bash
set -o emacs
```

Set Vi mode:

```bash
set -o vi
```

To make Vi mode the default for future Bash sessions, add the following to `~/.bashrc`:

```bash
set -o vi
```

Reload the configuration:

```bash
source ~/.bashrc
```

### 🖆 Inspecting Existing Key Bindings

Display Readline key bindings:

```bash
bind -P
```

Search for a specific editing function:

```bash
bind -P | grep beginning-of-line
```

Display the current Readline editing mode:

```bash
set -o | grep emacs
set -o | grep vi
```

# 🗁 Best Practices

* Learn the most frequently used shortcuts before memorizing the entire list.
* Use `Ctrl + R` to reuse complex commands.
* Use `Ctrl + A` and `Ctrl + E` to navigate long commands quickly.
* Use `Ctrl + U` and `Ctrl + K` to remove unwanted command-line text.
* Use `Tab` completion to reduce typing mistakes.
* Use `Ctrl + C` to interrupt commands when appropriate.
* Understand the difference between interrupting and suspending a process.
* Remember that `Ctrl + D` can send EOF to an interactive command.
* Use `Ctrl + Q` if terminal output has been paused by `Ctrl + S`.
* Customize Readline only when a shortcut provides a genuine productivity benefit.
* Be aware that terminal emulator shortcuts and shell shortcuts may overlap.

# 🗁 Practice Exercises

### 🖆 Basic Shortcuts

* Open Ubuntu Terminal.
* Run a command and interrupt it using `Ctrl + C`.
* Clear the screen using `Ctrl + L`.
* Exit an interactive Bash session using `Ctrl + D`.

### 🖆 Command-Line Navigation

* Type a long command.
* Move to its beginning using `Ctrl + A`.
* Move to its end using `Ctrl + E`.
* Navigate by words using `Alt + B` and `Alt + F`.
* Delete text using `Ctrl + W`, `Ctrl + U`, and `Ctrl + K`.

### 🖆 Command History

* Execute several commands.
* Retrieve the previous command using the Up Arrow.
* Search history using `Ctrl + R`.
* Execute the previous command using `!!`.
* Search command history using `history | grep`.

### 🖆 Process Control

* Start a foreground `sleep` command.
* Suspend it using `Ctrl + Z`.
* Resume it using `bg`.
* Bring it back using `fg`.
* Interrupt it using `Ctrl + C`.

### 🖆 Bash Customization

* Inspect existing Readline key bindings.
* Create a `~/.inputrc` file.
* Add a custom key binding.
* Reload the configuration.
* Switch between Emacs and Vi editing modes.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

* What are terminal keyboard shortcuts?
* What is the difference between a terminal emulator and a shell?
* What does `Ctrl + C` do?
* What does `Ctrl + D` do?
* What is the purpose of `Ctrl + L`?
* What does `Ctrl + Z` do?

### 🖆 Bash and Readline Questions

* What is GNU Readline?
* What is the purpose of the `~/.inputrc` file?
* What is the difference between `Ctrl + U` and `Ctrl + K`?
* What is the purpose of `Ctrl + R`?
* What is the difference between Emacs and Vi editing modes in Bash?
* How can you inspect existing Readline key bindings?

### 🖆 Process Control Questions

* What is the difference between `Ctrl + C` and `Ctrl + Z`?
* What is the relationship between `Ctrl + Z`, `bg`, and `fg`?
* What happens when `Ctrl + S` is pressed in a terminal with software flow control enabled?
* How can you resume terminal output after it has been paused?

# 🗁 References

* [GNU Bash Manual](https://www.gnu.org/software/bash/manual/)
* [GNU Bash Manual — Readline Interaction](https://www.gnu.org/software/bash/manual/html_node/Readline-Interaction.html)
* [GNU Readline Manual](https://tiswww.case.edu/php/chet/readline/readline.html)
* [GNU Bash Manual — Readline Init File](https://www.gnu.org/software/bash/manual/html_node/Readline-Init-File.html)
* [Ubuntu Documentation](https://documentation.ubuntu.com/)
* [GNOME Terminal Help](https://help.gnome.org/users/gnome-terminal/stable/)
