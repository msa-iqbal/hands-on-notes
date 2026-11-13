# Scheduler

The RouterOS Scheduler is a built-in automation feature that executes scripts and commands automatically at specified times or intervals.

It allows network administrators to automate repetitive tasks without manually running scripts.

Scheduler is commonly used for:

* Periodic system monitoring.
* Automated configuration exports.
* Scheduled maintenance.
* Log management.
* Network availability checks.
* Periodic service verification.
* Executing administrative scripts.
* Running recurring network operations.

# 🗁 Understanding Scheduler

Scheduler works with configured tasks called scheduler entries.

Each entry defines when and how often a script or command should execute.

A scheduler entry commonly includes:

* `name`: Identifies the scheduled task.
* `start-date`: Defines the date when execution can begin.
* `start-time`: Defines the initial execution time.
* `interval`: Defines the recurring execution interval.
* `on-event`: Contains commands or identifies a script to execute.
* `disabled`: Enables or disables the scheduled task.
* `run-count`: Indicates how many times the task has executed.

Example execution flow:

```text
        RouterOS Scheduler
                 |
                 v
          Scheduled Time
                 |
                 v
          Execute Script
                 |
          +------+------+
          |             |
          v             v
       Success        Failure
          |             |
          v             v
       Log Event    Error Handling
```

Scheduler does not automatically determine whether a task is safe or appropriate to execute. The administrator must design the script and its execution permissions carefully.

# 🗁 Viewing Scheduler Entries

Display all scheduled tasks:

```routeros
/system/scheduler/print
```

Display detailed information:

```routeros
/system/scheduler/print detail
```

Inspect a specific entry:

```routeros
/system/scheduler/print detail where name=system-monitor
```

The output helps identify the configured execution time, interval, script, and task status.

# 🗁 Creating a One-Time Scheduled Task

A one-time task executes at a specified date and time.

Example:

```routeros
/system/scheduler/add name=one-time-task \
    start-date=nov/11/2026 \
    start-time=23:00:00 \
    interval=0s \
    on-event="/log/info \"One-time scheduled task executed\""
```

This creates a task scheduled for November 11, 2026, at 23:00.

An interval of `0s` indicates that the task is not configured for recurring execution.

Use a future date and time appropriate for the router's configured clock.

# 🗁 Creating a Recurring Scheduled Task

A recurring task executes repeatedly according to its configured interval.

Example: execute a command every five minutes.

```routeros
/system/scheduler/add name=periodic-task \
    start-time=startup \
    interval=5m \
    on-event="/log/info \"Periodic task executed\""
```

This task starts after router startup and repeats every five minutes.

Common interval examples:

| Interval | Description |
| --- | --- |
| `30s` | Every 30 seconds |
| `1m` | Every minute |
| `5m` | Every five minutes |
| `15m` | Every fifteen minutes |
| `1h` | Every hour |
| `1d` | Every day |
| `7d` | Every seven days |

Choose an interval that matches the operational requirements of the task.

# 🗁 Executing an Existing Script with Scheduler

Scheduler can execute a script stored in `/system script`.

First, create a script:

```routeros
/system script/add name=system-monitor \
    policy=read,write,test \
    source={
        :local routerName [/system identity get name]
        :local cpuLoad [/system resource get cpu-load]
        :local freeMemory [/system resource get free-memory]

        :log info ("Router: " . $routerName)
        :log info ("CPU Load: " . $cpuLoad . "%")
        :log info ("Free Memory: " . $freeMemory . " bytes")

        :if ($cpuLoad > 80) do={
            :log warning "High CPU utilization detected"
        }
    }
```

Create a scheduler entry:

```routeros
/system/scheduler/add name=run-system-monitor \
    start-time=startup \
    interval=5m \
    on-event="/system/script/run system-monitor"
```

The scheduler executes the stored script every five minutes.

Using a separate script makes the automation easier to maintain and reuse.

# 🗁 Configuring Scheduler Start Time

The `start-time` property controls the initial execution time.

For a task that should begin after startup:

```routeros
/system/scheduler/add name=startup-task \
    start-time=startup \
    interval=1h \
    on-event="/log/info \"Startup task executed\""
```

For a task that should begin at a particular time:

```routeros
/system/scheduler/add name=daily-task \
    start-date=nov/12/2026 \
    start-time=02:00:00 \
    interval=1d \
    on-event="/log/info \"Daily task executed\""
```

The recurring task begins according to its configured start date and time.

For date-sensitive scheduling, verify the router's date, time, and timezone.

# 🗁 Scheduling Daily Maintenance

Scheduler can execute maintenance scripts at a specific time each day.

Example:

```routeros
/system/script/add name=daily-maintenance \
    policy=read,write,test \
    source={
        :log info "Daily maintenance started"

        :local routerName [/system identity get name]
        :local routerVersion [/system resource get version]

        :log info ("Router: " . $routerName)
        :log info ("RouterOS version: " . $routerVersion)

        :log info "Daily maintenance completed"
    }
```

Schedule the script:

```routeros
/system/scheduler/add name=daily-maintenance-job \
    start-date=nov/12/2026 \
    start-time=02:00:00 \
    interval=1d \
    on-event="/system/script/run daily-maintenance"
```

This example runs the maintenance script daily at the configured time.

Maintenance scripts should avoid disruptive operations unless they are explicitly planned and approved.

# 🗁 Scheduling Configuration Exports

A configuration export can be generated automatically using Scheduler.

Create a script:

```routeros
/system/script/add name=daily-config-export \
    policy=read,write,test \
    source={
        :local routerName [/system identity get name]
        :local date [/system clock get date]
        :local fileName ($routerName . "-" . $date)

        :log info ("Creating configuration export: " . $fileName)

        /export file=$fileName
    }
```

Create the scheduler entry:

```routeros
/system/scheduler/add name=config-export-job \
    start-time=startup \
    interval=1d \
    on-event="/system/script/run daily-config-export"
```

This creates a configuration export every day.

An export stored on the router is not an independent backup. For better protection, transfer the generated file to a separate storage location and manage retention.

# 🗁 Scheduling Interface Monitoring

Scheduler can execute scripts that periodically inspect interface status.

Create a script:

```routeros
/system/script/add name=interface-monitor \
    policy=read,write,test \
    source={
        :local interfaceName "ether1"
        :local interfaceId [/interface/find where name=$interfaceName]

        :if ([:len $interfaceId] = 0) do={
            :log error ("Interface not found: " . $interfaceName)
        } else={
            :local running [/interface/get $interfaceId running]

            :if ($running) do={
                :log info ($interfaceName . " is running")
            } else={
                :log warning ($interfaceName . " is not running")
            }
        }
    }
```

Schedule the monitoring script:

```routeros
/system/scheduler/add name=interface-monitor-job \
    start-time=startup \
    interval=1m \
    on-event="/system/script/run interface-monitor"
```

The script checks the interface every minute and records its state.

For critical interface monitoring, consider event-driven monitoring or a dedicated monitoring platform rather than relying exclusively on periodic polling.

# 🗁 Managing Scheduler Entries

Scheduler entries can be enabled, disabled, modified, and removed.

**Disable a task**

```routeros
/system/scheduler/disable [find name=run-system-monitor]
```

**Enable a task**

```routeros
/system/scheduler/enable [find name=run-system-monitor]
```

**Modify a task interval**

```routeros
/system/scheduler/set [find name=run-system-monitor] interval=10m
```

**Modify the scheduled command**

```routeros
/system/scheduler/set [find name=run-system-monitor] \
    on-event="/system/script/run system-monitor"
```

**Remove a task**

```routeros
/system/scheduler/remove [find name=run-system-monitor]
```

Use unique task names when managing entries through scripts.

# 🗁 Scheduler Permissions and Script Policies

Scheduler entries have policy settings that determine which operations their event scripts can perform.

Inspect scheduler configuration:

```routeros
/system/scheduler/print detail
```

Example:

```routeros
/system/scheduler/add name=read-only-monitor \
    start-time=startup \
    interval=5m \
    policy=read,test \
    on-event="/system/script/run system-monitor"
```

The required permissions depend on the operations performed by the scheduled task and the execution context of the called script.

A script that modifies configuration requires appropriate write permissions.

Use the minimum privileges needed for each task. Do not grant broad policies simply to make an automation script work.

# 🗁 Checking Scheduler Execution

Use the scheduler's `run-count` property to inspect how many times a task has executed.

Example:

```routeros
/system/scheduler/print detail where name=run-system-monitor
```

Review system logs:

```routeros
/log/print
```

You can also add explicit log messages to scripts:

```routeros
:log info "Scheduled operation started"

# Scheduled operations

:log info "Scheduled operation completed"
```

Logging provides useful evidence that a script reached specific execution points.

A successful scheduler execution does not necessarily mean every command inside the script completed successfully. Use error handling and meaningful result checks when required.

# 🗁 Preventing Overlapping Scheduled Tasks

A recurring task may take longer to execute than its configured interval.

For example, a script scheduled every minute may still be running when the next execution becomes due.

For long-running tasks, use an execution lock to prevent concurrent runs.

Example:

```routeros
/system/script/add name=protected-task \
    policy=read,write,test \
    source={
        :global protectedTaskRunning

        :if ($protectedTaskRunning = true) do={
            :log warning "Previous execution is still running"
            :return
        }

        :set protectedTaskRunning true

        :onerror errorMessage in={
            :log info "Protected task started"

            :delay 5s

            :log info "Protected task completed"
        } do={
            :log error ("Protected task failed: " . $errorMessage)
        }

        :set protectedTaskRunning false
    }
```

This example demonstrates the locking concept using a global variable.

For production scripts, ensure that the lock is released even when an operation fails. A failed execution must not leave a permanent lock that prevents future runs. Use structured cleanup and carefully test error paths.

# 🗁 Scheduler and Router Time Configuration

Scheduled tasks depend on the router's system clock.

Inspect the current date and time:

```routeros
/system/clock/print
```

Inspect timezone configuration:

```routeros
/system/clock/print
```

Configure an appropriate timezone:

```routeros
/system/clock/set time-zone-name=Asia/Dhaka
```

Configure NTP client settings where supported by the RouterOS version:

```routeros
/system/ntp/client/print
```

A router with an incorrect clock may execute scheduled tasks at unexpected times.

For production environments, maintain accurate time synchronization using reliable NTP servers.

# 🗁 Scheduler Troubleshooting

If a scheduled task does not execute as expected, inspect the following areas.

**Check whether the task exists**

```routeros
/system/scheduler/print detail
```

**Check whether it is disabled**

```routeros
/system/scheduler/print where disabled=yes
```

**Check the configured date and time**

```routeros
/system/clock/print
```

**Check the execution interval**

```routeros
/system/scheduler/print detail where name=run-system-monitor
```

**Check the referenced script**

```routeros
/system/script/print detail
```

**Run the script manually**

```routeros
/system/script/run system-monitor
```

**Review system logs**

```routeros
/log/print
```

Common causes of scheduler problems include:

* Incorrect start date or time.
* Incorrect interval.
* Disabled scheduler entry.
* Missing referenced script.
* Insufficient script permissions.
* Script syntax errors.
* Incorrect router clock.
* Long-running or overlapping operations.
* Errors inside the scheduled script.

# 🗁 Scheduler Best Practices

* Use descriptive scheduler names.
* Keep scripts separate from scheduler definitions.
* Choose appropriate execution intervals.
* Synchronize the router clock.
* Assign minimum required permissions.
* Log important execution events.
* Handle errors explicitly.
* Avoid unnecessary frequent polling.
* Prevent overlapping long-running tasks.
* Test scripts manually before scheduling them.
* Monitor generated files and storage usage.
* Store backups outside the router.
* Document the purpose and dependencies of each scheduled task.
* Review scheduled tasks periodically and remove obsolete entries.

# 🗁 Practical Scheduler Lab

**Lab objectives**

* Create a reusable RouterOS script.
* Schedule automatic script execution.
* Configure a recurring interval.
* Inspect scheduler execution details.
* Review script-generated logs.
* Disable and enable a scheduled task.

**Create a monitoring script**

```routeros
/system/script/add name=lab-monitor \
    policy=read,write,test \
    source={
        :local routerName [/system identity get name]
        :local cpuLoad [/system resource get cpu-load]

        :log info ("Scheduled monitoring: " . $routerName)
        :log info ("CPU Load: " . $cpuLoad . "%")

        :if ($cpuLoad > 80) do={
            :log warning "High CPU load detected"
        }
    }
```

**Create a scheduler entry**

```routeros
/system/scheduler/add name=lab-monitor-job \
    start-time=startup \
    interval=5m \
    on-event="/system/script/run lab-monitor"
```

**Verify the scheduler configuration**

```routeros
/system/scheduler/print detail
```

**Review execution logs**

```routeros
/log/print
```

**Disable the scheduled task**

```routeros
/system/scheduler/disable [find name=lab-monitor-job]
```

**Enable it again**

```routeros
/system/scheduler/enable [find name=lab-monitor-job]
```

**Remove the lab configuration**

```routeros
/system/scheduler/remove [find name=lab-monitor-job]
/system/script/remove [find name=lab-monitor]
```

**Expected outcome**

You should be able to create a reusable script, execute it automatically at a configured interval, inspect its execution, and manage its lifecycle.

Scheduler is an essential component of RouterOS automation and provides the foundation for recurring monitoring, maintenance, and backup operations.
