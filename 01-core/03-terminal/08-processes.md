# Process Management in Ubuntu/Linux

Process management is the practice of monitoring, controlling, and managing running programs in an operating system.

In Ubuntu/Linux, every running program operates as one or more processes. Each process has a unique Process ID (PID), consumes system resources, and has a lifecycle managed by the Linux kernel.

The terminal provides several commands for inspecting processes, starting background tasks, managing jobs, sending signals, adjusting scheduling priority, and troubleshooting unresponsive applications.

# 🗁 Understanding Processes

### 🖆 What Is a Process?

A process is an instance of a program that is currently executing.

A program is a collection of instructions stored on disk, while a process is the running instance of those instructions.

For example:

```bash
nano notes.txt
```

When you execute this command, Linux starts a process for the Nano text editor.

A process typically contains:

* Process ID (PID).
* Parent Process ID (PPID).
* Process state.
* Memory address space.
* CPU scheduling information.
* Open file descriptors.
* Environment variables.
* Security credentials.
* Execution context.

### 🖆 Program vs Process

| Program | Process |
| --- | --- |
| Stored executable or application code | Running instance of a program |
| Exists on storage | Exists as an executing task |
| Does not necessarily consume CPU | May consume CPU |
| Can be started multiple times | Each instance has its own PID |
| Passive | Active |

For example, starting two Nano instances creates two separate processes, even though both execute the same program.

### 🖆 Process Hierarchy

Linux processes generally form a parent-child hierarchy.

A process can create child processes using system calls such as `fork()` and `clone()`. The `exec()` family of functions can replace a process's program image with another program.

Example:

```text
systemd (PID 1)
    |
    ├── sshd
    |    └── sshd
    |         └── bash
    |              ├── nano
    |              └── sleep
    |
    ├── cron
    |
    └── system services
```

The exact hierarchy depends on the running system.

# 🗁 Process Identifiers

### 🖆 Process ID (PID)

Every process has a unique Process ID (PID) while it exists.

Display the current shell's PID:

```bash
echo $$
```

Display the PID of the most recently started background process:

```bash
sleep 100 &
echo $!
```

Here:

* `$$` represents the current Bash process ID.
* `$!` represents the PID of the most recently started asynchronous process.

### 🖆 Parent Process ID (PPID)

A Parent Process ID identifies the process that created another process.

Display the current shell's parent PID:

```bash
echo $PPID
```

Display PID and PPID information:

```bash
ps -o pid,ppid,cmd -p $$
```

### 🖆 User ID (UID)

Each process runs with user credentials that determine its access permissions.

Display process ownership:

```bash
ps -eo pid,user,group,cmd
```

### 🖆 Process Groups and Sessions

Linux organizes related processes into process groups and sessions.

* **Process Group:** A collection of related processes, commonly used for job control.
* **Session:** A collection of process groups associated with a login session or terminal.
* **Foreground Process Group:** The process group currently receiving terminal input.
* **Background Process Group:** A process group running without controlling terminal input.

Inspect process group and session information:

```bash
ps -o pid,ppid,pgid,sid,tpgid,stat,cmd -p $$
```

| Field | Meaning |
| --- | --- |
| PID | Process ID |
| PPID | Parent Process ID |
| PGID | Process Group ID |
| SID | Session ID |
| TPGID | Foreground Process Group ID |
| STAT | Process state |

# 🗁 Process States

### 🖆 Common Linux Process States

Linux uses process states to describe what a process is currently doing.

| State | Meaning | Description |
| --- | --- | --- |
| R | Running or runnable | Executing or waiting for CPU time |
| S | Interruptible sleep | Waiting for an event and can respond to signals |
| D | Uninterruptible sleep | Usually waiting for kernel-level I/O |
| T | Stopped | Suspended by a signal or debugger |
| t | Tracing stop | Stopped during tracing |
| Z | Zombie | Exited but not yet reaped by its parent |
| I | Idle | Idle kernel thread |

Inspect process states:

```bash
ps -eo pid,stat,comm
```

Example:

```text
PID   STAT  COMMAND
1     Ss    systemd
820   S     bash
1200  R     top
1400  T     nano
1600  Z     example
```

The additional characters in `STAT`, such as `s`, `l`, and `+`, provide further information about session leadership, multithreading, and foreground process groups.

### 🖆 Running and Sleeping Processes

A process in the `R` state is either executing on a CPU or ready to execute.

A process in the `S` state is waiting for an event, such as user input or a timer.

Sleeping is normal behavior. A process does not need to continuously consume CPU resources.

### 🖆 Stopped Processes

A stopped process is temporarily suspended.

For example:

```bash
sleep 300
```

Press:

```text
Ctrl + Z
```

The shell suspends the foreground job.

Inspect it:

```bash
jobs
```

Resume it in the foreground:

```bash
fg
```

### 🖆 Zombie Processes

A zombie is a process that has finished execution but whose parent has not yet collected its exit status.

A zombie:

* Has already terminated.
* Does not execute instructions.
* Does not consume normal running-process resources.
* Retains a process-table entry until reaped by its parent.

Inspect zombie processes:

```bash
ps -eo pid,ppid,stat,cmd | awk '$3 ~ /^Z/'
```

A zombie cannot be terminated with `kill` because it has already exited. The parent must collect its exit status, usually through `wait()`. If the parent exits, the zombie is typically adopted and reaped by an appropriate system process.

### 🖆 Uninterruptible Sleep

A process in the `D` state is usually waiting for a kernel operation, often related to storage or another I/O operation.

Inspect such processes:

```bash
ps -eo pid,stat,wchan:32,cmd | awk '$2 ~ /^D/'
```

Sending `SIGKILL` does not immediately remove a process in this state. The process must first return from the uninterruptible kernel wait.

# 🗁 Viewing Processes

### 🖆 The `ps` Command

The `ps` command displays information about processes.

Basic usage:

```bash
ps
```

Display processes associated with the current terminal:

```bash
ps -f
```

Display all processes using BSD-style options:

```bash
ps aux
```

Display all processes using Unix-style options:

```bash
ps -ef
```

The two common formats differ in their option syntax and output columns.

### 🖆 Understanding `ps aux`

```bash
ps aux
```

Important columns:

| Column | Description |
| --- | --- |
| USER | Process owner |
| PID | Process ID |
| %CPU | CPU utilization |
| %MEM | Memory utilization |
| VSZ | Virtual memory size |
| RSS | Resident memory size |
| TTY | Controlling terminal |
| STAT | Process state |
| START | Process start time |
| TIME | Accumulated CPU time |
| COMMAND | Executed command |

### 🖆 Finding a Process with `pgrep`

The `pgrep` command searches for processes by name or pattern.

```bash
pgrep bash
```

Display matching PIDs with process names:

```bash
pgrep -a bash
```

Search using a full command line:

```bash
pgrep -af "python3 app.py"
```

### 🖆 Finding a Process with `pidof`

The `pidof` command returns the PIDs of running instances of a specified program.

```bash
pidof bash
```

It is useful when the executable name is known.

### 🖆 Viewing Process Trees with `pstree`

The `pstree` command displays processes in a hierarchical tree.

Install it if necessary:

```bash
sudo apt install psmisc
```

Display the complete process tree:

```bash
pstree
```

Display PIDs:

```bash
pstree -p
```

Display the process tree for the current user:

```bash
pstree -u
```

# 🗁 Monitoring Processes

### 🖆 The `top` Command

The `top` command provides a real-time view of system processes and resource consumption.

```bash
top
```

It displays:

* System uptime.
* Load averages.
* CPU utilization.
* Memory utilization.
* Swap utilization.
* Running and sleeping processes.
* Process IDs.
* CPU and memory usage per process.

Useful interactive controls:

| Key | Action |
| --- | --- |
| `q` | Quit |
| `k` | Send a signal to a process |
| `M` | Sort by memory usage |
| `P` | Sort by CPU usage |
| `1` | Toggle individual CPU core statistics |
| `r` | Change process priority |
| `h` | Display help |

### 🖆 The `htop` Command

`htop` is an interactive process-monitoring tool with a more convenient interface than `top`.

Install it:

```bash
sudo apt update
sudo apt install htop
```

Run:

```bash
htop
```

It provides:

* Interactive process selection.
* CPU and memory visualization.
* Process-tree view.
* Process filtering and searching.
* Convenient signal and priority controls.

### 🖆 Monitoring System Load

Display system uptime and load averages:

```bash
uptime
```

Display memory information:

```bash
free -h
```

Display CPU and memory information for selected processes:

```bash
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head
```

Sort by memory usage:

```bash
ps -eo pid,comm,%mem --sort=-%mem | head
```

Display process-specific resource information:

```bash
ps -p 1234 -o pid,ppid,stat,%cpu,%mem,rss,vsz,cmd
```

Replace `1234` with the actual PID.

# 🗁 Starting Background Processes

### 🖆 Foreground Processes

A foreground process runs attached to the terminal's foreground process group.

Example:

```bash
sleep 60
```

The terminal remains occupied until the command completes or is interrupted.

### 🖆 Background Processes

Use the `&` operator to start a command asynchronously.

```bash
sleep 300 &
```

The shell returns control immediately.

Display the background process PID:

```bash
echo $!
```

Start multiple background processes:

```bash
sleep 100 &
sleep 200 &
sleep 300 &
```

### 🖆 Redirecting Background Output

Background processes can still write to the terminal unless their output is redirected.

```bash
sleep 300 > sleep.log 2>&1 &
```

This redirects standard output and standard error to `sleep.log`.

A more practical example:

```bash
python3 app.py > app.log 2>&1 &
```

Inspect the output:

```bash
tail -f app.log
```

### 🖆 Using `nohup`

The `nohup` command runs a command with the `SIGHUP` signal ignored.

```bash
nohup sleep 300 &
```

By default, terminal output is written to `nohup.out` when it would otherwise go to the terminal.

For an application:

```bash
nohup python3 app.py > app.log 2>&1 &
```

Important distinction:

* `nohup` prevents termination caused by `SIGHUP`.
* `nohup` does not automatically place a command in the background.
* The `&` operator starts it asynchronously.

### 🖆 Using `disown`

`disown` is a Bash built-in used to remove jobs from the shell's job table or prevent them from receiving `SIGHUP`.

Start a background job:

```bash
sleep 300 &
```

List jobs:

```bash
jobs
```

Remove the most recent job from the job table:

```bash
disown
```

Remove a specific job:

```bash
disown %1
```

Mark a job so the shell does not send it `SIGHUP`:

```bash
disown -h %1
```

Unlike `nohup`, `disown` is a shell job-control operation. It does not change the process's signal disposition in the same way that `nohup` does.

# 🗁 Managing Shell Jobs

### 🖆 The `jobs` Command

The `jobs` command displays background and suspended jobs belonging to the current shell.

```bash
sleep 300 &
sleep 400 &
```

Display jobs:

```bash
jobs
```

Include process IDs:

```bash
jobs -l
```

Example:

```text
[1]-  1234 Running    sleep 300 &
[2]+  1235 Running    sleep 400 &
```

Job identifiers such as `%1` and `%2` are shell job specifications, not process IDs.

### 🖆 Moving a Job to the Foreground

Use `fg` to bring a background or suspended job into the foreground.

```bash
fg %1
```

If no job is specified, Bash selects its current job.

### 🖆 Moving a Job to the Background

Use `bg` to resume a suspended job in the background.

Start a command:

```bash
sleep 300
```

Suspend it with:

```text
Ctrl + Z
```

Resume it in the background:

```bash
bg %1
```

### 🖆 Suspending a Foreground Job

Press:

```text
Ctrl + Z
```

This normally sends `SIGTSTP` to the foreground process group and suspends the job.

Resume it:

```bash
fg
```

Or resume it in the background:

```bash
bg
```

# 🗁 Process Signals

### 🖆 What Are Signals?

Signals are software notifications sent to processes to communicate events or request specific actions.

Linux provides signals for process control, error handling, and inter-process communication.

List available signal names:

```bash
kill -l
```

Common signals:

| Signal | Number | Description |
| --- | ---: | --- |
| SIGHUP | 1 | Hangup; commonly associated with terminal disconnection |
| SIGINT | 2 | Interrupt from the terminal |
| SIGQUIT | 3 | Quit and potentially produce a core dump |
| SIGKILL | 9 | Forcefully terminate a process |
| SIGTERM | 15 | Request process termination |
| SIGSTOP | 19 on common architectures | Stop a process |
| SIGCONT | 18 on common architectures | Continue a stopped process |
| SIGTSTP | 20 on common architectures | Terminal stop request |

Signal numbers for some signals can vary by architecture. Prefer signal names when writing portable commands.

### 🖆 Sending Signals with `kill`

Despite its name, `kill` sends signals to processes. It does not always terminate them.

Send the default signal, `SIGTERM`:

```bash
kill 1234
```

Explicitly send `SIGTERM`:

```bash
kill -TERM 1234
```

Send `SIGINT`:

```bash
kill -INT 1234
```

Stop a process:

```bash
kill -STOP 1234
```

Continue a stopped process:

```bash
kill -CONT 1234
```

Replace `1234` with the target PID.

### 🖆 Signal Handling

A process can catch, handle, or ignore many signals.

However, two important signals cannot be caught, blocked, or ignored:

* `SIGKILL`
* `SIGSTOP`

`SIGTERM` is generally preferred for normal termination because applications can handle it and perform cleanup operations.

### 🖆 Sending Signals with `pkill`

The `pkill` command sends signals to processes selected by name or other matching criteria.

Terminate processes matching a name:

```bash
pkill firefox
```

Send a specific signal:

```bash
pkill -TERM firefox
```

Match a full command line:

```bash
pkill -f "python3 app.py"
```

Target processes belonging to a particular user:

```bash
pkill -u username firefox
```

Use process-name and user filters carefully to avoid affecting unrelated applications.

### 🖆 Sending Signals with `killall`

The `killall` command sends signals to processes selected by process name.

```bash
killall firefox
```

Send `SIGTERM` explicitly:

```bash
killall -TERM firefox
```

Force termination:

```bash
killall -KILL firefox
```

On Ubuntu, `killall` is commonly provided by the `psmisc` package. Its behavior and implementation can differ across operating systems.

# 🗁 Terminating Processes

### 🖆 Graceful Termination

The recommended first approach is to request normal termination.

Find the process:

```bash
pgrep -af "python3 app.py"
```

Send `SIGTERM`:

```bash
kill -TERM 1234
```

Check whether it has exited:

```bash
ps -p 1234
```

A process may need time to finish its current operation and clean up resources.

### 🖆 Forceful Termination

If a process does not respond to `SIGTERM`, `SIGKILL` can be used as a last resort.

```bash
kill -KILL 1234
```

The shorter equivalent is:

```bash
kill -9 1234
```

`SIGKILL` cannot be handled by the target process, so the application cannot perform its normal cleanup.

### 🖆 Terminating a Process by Name

```bash
pkill -TERM firefox
```

Or:

```bash
killall -TERM firefox
```

Before terminating processes by name, verify which instances match:

```bash
pgrep -a firefox
```

### 🖆 Understanding Process Termination

A typical process termination workflow is:

```text
Identify process
      |
      v
Inspect PID and ownership
      |
      v
Send SIGTERM
      |
      v
Wait and verify
      |
      +---- Process exited ----> Complete
      |
      v
Still running?
      |
      v
Investigate the process state
      |
      v
Consider SIGKILL if appropriate
```

A process in the `D` state may remain present even after receiving `SIGKILL`, until its uninterruptible kernel operation completes.

# 🗁 Process Priority

### 🖆 Understanding Nice Values

Linux uses scheduling policies to determine how CPU time is distributed among runnable processes.

For normal scheduling, the nice value influences a process's scheduling priority.

The nice value ranges from:

| Nice Value | Relative Priority |
| ---: | --- |
| -20 | Highest priority |
| 0 | Default priority |
| +19 | Lowest priority |

A lower nice value generally gives a process a greater scheduling preference relative to other normal-priority processes.

### 🖆 Starting a Process with `nice`

Start a process with a nice value of `10`:

```bash
nice -n 10 sleep 300
```

Start a process with a lower nice value:

```bash
sudo nice -n -10 sleep 300
```

Increasing the nice value reduces the process's scheduling preference. Lowering it generally requires elevated privileges.

### 🖆 Changing Priority with `renice`

Change the nice value of an existing process:

```bash
renice 10 -p 1234
```

Set a higher scheduling preference:

```bash
sudo renice -10 -p 1234
```

Inspect process priority:

```bash
ps -o pid,ni,pri,cmd -p 1234
```

Important fields:

* `NI`: Nice value.
* `PRI`: Kernel-reported scheduling priority.

Nice values are not direct CPU percentages or guaranteed execution times.

# 🗁 Waiting for Processes

### 🖆 The `wait` Built-in

Bash provides the `wait` built-in to wait for background processes started by the current shell.

Example:

```bash
sleep 5 &
pid=$!

wait "$pid"

echo "Process completed"
```

The shell waits until the specified process finishes.

### 🖆 Waiting for All Background Jobs

```bash
sleep 3 &
sleep 5 &

wait

echo "All background jobs completed"
```

Without a PID, `wait` waits for applicable background jobs started by the current shell.

### 🖆 Checking Exit Status

The exit status of a completed process is available through `$?`.

```bash
sleep 2 &
pid=$!

wait "$pid"

echo "Exit status: $?"
```

A successful command generally returns `0`. A non-zero status usually indicates an error or another condition defined by the command.

### 🖆 Waiting for Any Background Process

Bash supports `wait -n`, which waits for the next applicable background job to finish.

```bash
sleep 5 &
sleep 2 &
sleep 4 &

wait -n

echo "One background process has completed"
```

This is useful when coordinating multiple asynchronous tasks.

# 🗁 The Linux `/proc` Filesystem

### 🖆 What Is `/proc`?

`/proc` is a virtual filesystem that exposes information about running processes and kernel state.

It does not behave like an ordinary directory stored on disk. Its contents are generated by the kernel.

List its contents:

```bash
ls /proc
```

Each numeric directory typically represents a process ID.

Inspect a process:

```bash
ls /proc/1234
```

### 🖆 Process Information Files

Important files inside `/proc/PID/` include:

| File | Description |
| --- | --- |
| `status` | Process state, IDs, memory and other information |
| `cmdline` | Command-line arguments |
| `comm` | Process command name |
| `stat` | Detailed process statistics |
| `environ` | Process environment variables |
| `fd/` | Open file descriptors |
| `cwd` | Current working directory |
| `exe` | Executable file reference |

### 🖆 Inspecting Process Status

```bash
cat /proc/1234/status
```

Extract important fields:

```bash
grep -E '^(Name|State|Pid|PPid|Uid|Gid|VmRSS|VmSize):' /proc/1234/status
```

### 🖆 Inspecting Command-Line Arguments

```bash
cat /proc/1234/cmdline
```

The arguments are separated by null bytes, so `tr` can make them easier to read:

```bash
tr '\0' ' ' < /proc/1234/cmdline
```

### 🖆 Inspecting Open File Descriptors

```bash
ls -l /proc/1234/fd
```

This directory contains references to files, sockets, pipes, and other resources opened by the process.

### 🖆 Inspecting the Working Directory

```bash
readlink /proc/1234/cwd
```

Inspect the executable:

```bash
readlink /proc/1234/exe
```

Access to some `/proc` information may be restricted by user permissions or system security settings.

# 🗁 Managing System Services

### 🖆 Processes vs System Services

A process is a running instance of a program.

A system service is a background system component managed by a service manager, commonly `systemd` on Ubuntu.

A service may create one or more processes.

### 🖆 Checking Service Status

Inspect a service:

```bash
systemctl status ssh
```

Check whether it is active:

```bash
systemctl is-active ssh
```

List running services:

```bash
systemctl list-units --type=service --state=running
```

### 🖆 Viewing Service Logs

Use `journalctl` to inspect logs collected by the systemd journal.

```bash
journalctl -u ssh
```

Display recent logs:

```bash
journalctl -u ssh -n 50
```

Follow new log entries:

```bash
journalctl -u ssh -f
```

### 🖆 Starting and Stopping Services

Start a service:

```bash
sudo systemctl start ssh
```

Stop a service:

```bash
sudo systemctl stop ssh
```

Restart a service:

```bash
sudo systemctl restart ssh
```

These commands manage system services, not ordinary foreground or background shell jobs.

# 🗁 Practical Workflows

### 🖆 Find a Process Consuming High CPU

Display the processes consuming the most CPU:

```bash
ps -eo pid,ppid,comm,%cpu,%mem --sort=-%cpu | head -n 10
```

Inspect a particular process:

```bash
ps -p 1234 -o pid,ppid,stat,%cpu,%mem,cmd
```

If necessary, inspect it interactively:

```bash
top -p 1234
```

### 🖆 Find a Process Consuming High Memory

```bash
ps -eo pid,comm,%mem,rss --sort=-%mem | head -n 10
```

Remember that RSS represents resident memory, while VSZ represents virtual memory size.

### 🖆 Run a Long-Running Command in the Background

```bash
nohup python3 app.py > app.log 2>&1 &
```

Save its PID:

```bash
echo $! > app.pid
```

Monitor the output:

```bash
tail -f app.log
```

Terminate it gracefully later:

```bash
kill -TERM "$(cat app.pid)"
```

### 🖆 Find and Terminate an Unresponsive Application

Find matching processes:

```bash
pgrep -af firefox
```

Inspect their state:

```bash
ps -p 1234 -o pid,ppid,stat,cmd
```

Request termination:

```bash
kill -TERM 1234
```

If it remains unresponsive and is not waiting in an uninterruptible state, consider:

```bash
kill -KILL 1234
```

### 🖆 Run Multiple Commands Concurrently

```bash
sleep 3 &
sleep 5 &
sleep 2 &

wait

echo "All tasks completed"
```

This demonstrates basic concurrent execution and synchronization in Bash.

# 🗁 Best Practices

* Identify a process by PID and command before terminating it.
* Prefer `SIGTERM` over `SIGKILL`.
* Use `SIGKILL` only when graceful termination fails and forceful termination is appropriate.
* Avoid terminating processes using broad name patterns without inspecting the matches.
* Use `top` or `htop` to investigate resource consumption.
* Use `ps` for process inspection and scripting.
* Redirect output from long-running background tasks to log files.
* Use `nohup` when a command must ignore terminal hangup signals.
* Use `systemctl` to manage systemd services.
* Understand the difference between PIDs, job IDs, process groups, and sessions.
* Investigate zombie processes by examining their parent processes.
* Avoid changing nice values without understanding their scheduling implications.
* Use `/proc` for detailed process inspection.

# 🗁 Common Mistakes

### 🖆 Confusing PID with Job ID

A PID identifies a system process, while a job ID identifies a job managed by the current shell.

```bash
sleep 300 &
jobs -l
```

The job specification might be `%1`, while the process might have PID `1234`.

### 🖆 Assuming `kill` Always Terminates a Process

The `kill` command sends a signal. The target process may handle or ignore signals such as `SIGTERM`.

### 🖆 Using `kill -9` Immediately

`SIGKILL` prevents the application from performing normal cleanup. Always try graceful termination first.

### 🖆 Assuming `&` Makes a Process Persistent

The `&` operator only starts a job asynchronously. It does not guarantee that the process will survive terminal disconnection.

### 🖆 Confusing `nohup` with Background Execution

`nohup` ignores `SIGHUP`; it does not automatically background a process.

Use:

```bash
nohup command > output.log 2>&1 &
```

### 🖆 Trying to Kill a Zombie Process

A zombie has already exited. Investigate its parent process instead of repeatedly sending signals to the zombie PID.

### 🖆 Assuming High CPU Usage Always Means a Problem

Some workloads legitimately require substantial CPU resources. Investigate the application's expected behavior before terminating it.

# 🗁 Practice Exercises

### 🖆 Basic Process Inspection

* Display all processes belonging to your current terminal.
* Display all processes on the system.
* Find the PID of your current Bash shell.
* Find the parent PID of your current shell.
* Display the process tree.

### 🖆 Background Process Management

* Start a `sleep` command in the background.
* Display its PID.
* List the shell's background jobs.
* Bring the job to the foreground.
* Suspend it using `Ctrl + Z`.
* Resume it using `bg`.
* Wait for it to complete.

### 🖆 Signal Management

* Start a long-running `sleep` process.
* Send it `SIGTERM`.
* Start another `sleep` process.
* Stop it using `SIGSTOP`.
* Continue it using `SIGCONT`.
* Terminate it using `SIGTERM`.

### 🖆 Process Monitoring

* Open `top`.
* Sort processes by CPU usage.
* Sort processes by memory usage.
* Install and explore `htop`.
* Identify the processes consuming the most memory.
* Inspect a process using `/proc/PID/status`.

### 🖆 Priority Management

* Start a process with `nice -n 10`.
* Inspect its nice value using `ps`.
* Change its priority using `renice`.
* Compare the reported scheduling information.

### 🖆 Background Application Workflow

* Run a long-running command using `nohup`.
* Redirect its output to a log file.
* Store its PID in a file.
* Monitor its log output.
* Terminate it gracefully using the stored PID.

# 🗁 Interview Questions

### 🖆 Fundamental Questions

* What is a process in Linux?
* What is the difference between a program and a process?
* What is a PID?
* What is a PPID?
* What is a process hierarchy?
* What is the difference between a process and a thread?
* What are process groups and sessions?

### 🖆 Process State Questions

* What are the common Linux process states?
* What is a zombie process?
* What is an orphan process?
* Can a zombie process be killed?
* What does the `D` process state represent?
* What is the difference between running and sleeping processes?

### 🖆 Process Management Questions

* What is the difference between `ps aux` and `ps -ef`?
* What is the difference between `pgrep` and `pidof`?
* What is the difference between `kill` and `killall`?
* What is the difference between `SIGTERM` and `SIGKILL`?
* What is the purpose of the `nohup` command?
* What is the difference between `nohup` and `disown`?
* What is the difference between a PID and a shell job ID?

### 🖆 Advanced Questions

* What are nice values in Linux?
* What is the purpose of `renice`?
* How does the Bash `wait` command work?
* What information is available in `/proc/PID/`?
* Why might a process remain in the `D` state after receiving `SIGKILL`?
* What is the difference between managing a process with `kill` and managing a service with `systemctl`?
* How would you investigate a process consuming excessive CPU or memory?

# 🗁 References

* [Linux Manual Pages](https://man7.org/linux/man-pages/)
* [ps — Linux Manual](https://man7.org/linux/man-pages/man1/ps.1.html)
* [proc — Linux Manual](https://man7.org/linux/man-pages/man5/proc.5.html)
* [kill — Linux Manual](https://man7.org/linux/man-pages/man1/kill.1.html)
* [GNU Bash Manual — Job Control](https://www.gnu.org/software/bash/manual/html_node/Job-Control.html)
* [GNU Bash Manual — Job Control Builtins](https://www.gnu.org/software/bash/manual/html_node/Job-Control-Builtins.html)
* [GNU Coreutils Manual — nohup](https://www.gnu.org/software/coreutils/manual/html_node/nohup-invocation.html)
* [Ubuntu Server Documentation](https://documentation.ubuntu.com/server/)
