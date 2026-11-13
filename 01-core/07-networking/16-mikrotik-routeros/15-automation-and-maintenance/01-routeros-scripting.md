# RouterOS Scripting

RouterOS scripting allows network administrators to automate configuration tasks, monitor network conditions, process information, and execute administrative operations.

Instead of repeatedly performing the same tasks manually, administrators can create scripts that execute multiple commands in a defined sequence.

RouterOS scripting is useful for:

* Automating routine configuration tasks.
* Monitoring network conditions.
* Managing interfaces and services.
* Generating log messages.
* Performing scheduled maintenance.
* Responding to network events.
* Managing backups and configuration exports.
* Integrating RouterOS with external management systems.

# 🗁 Understanding the RouterOS Script Environment

RouterOS includes a built-in scripting language that supports variables, expressions, conditional statements, loops, functions, and error handling.

Scripts can be executed in several ways:

* Directly from the RouterOS terminal.
* Through the `/system script` menu.
* Through the Scheduler.
* Through Netwatch event scripts.
* Through other RouterOS event-driven mechanisms.
* Through the RouterOS API.

A script consists of RouterOS commands and scripting statements executed in sequence.

Example:

```routeros
:put "Hello from MikroTik RouterOS"
```

Output:

```text
Hello from MikroTik RouterOS
```

# 🗁 Executing Commands in a Script

RouterOS commands can be executed directly inside a script.

Example:

```routeros
/system identity print
/system resource print
/interface print
```

Commands are generally executed in the order they appear.

You can also use command results in expressions and variables.

Example:

```routeros
:local routerName [/system identity get name]
:put $routerName
```

The `:local` statement creates a variable, while `:put` displays its value.

# 🗁 Understanding Comments

Comments help explain the purpose of commands and make scripts easier to maintain.

Use `#` to write a comment.

Example:

```routeros
# Display router identity
/system identity print

# Display system resources
/system resource print
```

Comments are ignored during script execution.

Use meaningful comments when scripts contain multiple operations or complex conditions.

# 🗁 Understanding Variables

Variables store values that can be reused throughout a script.

RouterOS supports local and global variables.

**Local variables**

A local variable is available within its defined scope.

Example:

```routeros
:local routerName "MikroTik-Router"
:local managementIP "192.168.10.1"

:put $routerName
:put $managementIP
```

**Global variables**

A global variable is available across script environments during the current RouterOS session.

Example:

```routeros
:global monitoringStatus "running"

:put $monitoringStatus
```

Global variables are useful when multiple scripts need to share state.

However, global variables are not a persistent storage mechanism across router reboots.

**Variable naming**

Use descriptive variable names:

```routeros
:local wanInterface "ether1"
:local lanInterface "bridge-LAN"
:local dnsServer "8.8.8.8"
```

Variable names are case-sensitive. Use consistent naming conventions to avoid mistakes.

# 🗁 Reading Configuration Values

Scripts can retrieve configuration values using the `get` command.

Example:

```routeros
:local routerName [/system identity get name]
:local routerVersion [/system resource get version]
:local cpuLoad [/system resource get cpu-load]

:put ("Router: " . $routerName)
:put ("Version: " . $routerVersion)
:put ("CPU Load: " . $cpuLoad)
```

The dot operator (`.`) joins strings and values.

This makes it possible to generate dynamic log messages and reports.

# 🗁 Understanding Data Types

RouterOS scripting supports different types of values.

Common types include:

| Type | Example |
| --- | --- |
| String | `"MikroTik"` |
| Integer | `100` |
| Boolean | `true` |
| IP address | `192.168.10.1` |
| Array | `{"ether1";"ether2"}` |
| Time | `30s` |

Example:

```routeros
:local routerName "MikroTik"
:local retryCount 5
:local monitoringEnabled true

:put $routerName
:put $retryCount
:put $monitoringEnabled
```

Understanding data types is important when comparing values, performing calculations, and passing arguments to RouterOS commands.

# 🗁 Conditional Statements

Conditional statements allow scripts to make decisions based on specific conditions.

RouterOS uses `:if` and `:else`.

Basic syntax:

```routeros
:if (condition) do={
    # Commands when condition is true
} else={
    # Commands when condition is false
}
```

Example:

```routeros
:local cpuLoad [/system resource get cpu-load]

:if ($cpuLoad > 80) do={
    :log warning "High CPU utilization detected"
} else={
    :log info "CPU utilization is within the configured threshold"
}
```

The script reads the current CPU load and generates a log message based on the condition.

# 🗁 Comparison Operators

Comparison operators evaluate relationships between values.

| Operator | Meaning |
| --- | --- |
| `=` | Equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

Example:

```routeros
:local currentLoad 75

:if ($currentLoad >= 70) do={
    :log warning "CPU load reached the threshold"
}
```

Use comparison operators to implement monitoring thresholds and configuration checks.

# 🗁 Logical Operators

Logical operators combine multiple conditions.

| Operator | Meaning |
| --- | --- |
| `&&` | Logical AND |
| ` | | ` | Logical OR |
| `!` | Logical NOT |

Example using AND:

```routeros
:local cpuLoad [/system resource get cpu-load]
:local freeMemory [/system resource get free-memory]

:if (($cpuLoad > 80) && ($freeMemory < 10000000)) do={
    :log warning "High CPU load and low free memory detected"
}
```

Example using OR:

```routeros
:local cpuLoad [/system resource get cpu-load]

:if (($cpuLoad > 80) || ($cpuLoad < 5)) do={
    :log info "CPU load is outside the expected range"
}
```

Example using NOT:

```routeros
:local monitoringEnabled false

:if (!$monitoringEnabled) do={
    :log warning "Monitoring is disabled"
}
```

Choose logical conditions carefully so that scripts behave as intended.

# 🗁 Loops

Loops repeat commands until a condition is satisfied or a collection has been processed.

RouterOS supports `:for`, `:foreach`, and `:while`.

**For loop**

A `:for` loop executes commands over a numeric range.

Example:

```routeros
:for i from=1 to=5 do={
    :put ("Iteration: " . $i)
}
```

**Foreach loop**

A `:foreach` loop processes each element in an array or collection.

Example:

```routeros
:local interfaces {"ether1";"ether2";"ether3"}

:foreach interface in=$interfaces do={
    :put $interface
}
```

**While loop**

A `:while` loop continues executing while its condition remains true.

Example:

```routeros
:local counter 1

:while ($counter <= 5) do={
    :put $counter
    :set counter ($counter + 1)
}
```

Always ensure that a while loop has a condition that eventually becomes false. Otherwise, the script may continue running indefinitely.

# 🗁 Working with Arrays

Arrays store multiple values in one variable.

Example:

```routeros
:local wanInterfaces {"ether1";"ether2";"ether3"}

:foreach interface in=$wanInterfaces do={
    :put ("WAN Interface: " . $interface)
}
```

Arrays are useful for processing:

* Interface names.
* IP addresses.
* Subnet lists.
* Router names.
* Monitoring targets.
* Configuration values.

You can also build arrays dynamically using RouterOS scripting expressions.

# 🗁 Working with RouterOS Configuration Objects

RouterOS configuration menus contain objects that can be queried and processed by scripts.

For example, retrieve interface information:

```routeros
:foreach interfaceId in=[/interface find] do={
    :local interfaceName [/interface get $interfaceId name]
    :put $interfaceName
}
```

The `find` command returns identifiers for matching configuration objects.

You can filter objects using conditions.

Example:

```routeros
:foreach interfaceId in=[/interface find where disabled=no] do={
    :local interfaceName [/interface get $interfaceId name]
    :put ("Enabled interface: " . $interfaceName)
}
```

This example processes enabled interfaces.

When working with configuration objects, use the returned identifiers instead of assuming that item numbers remain constant.

# 🗁 Creating and Executing a Script

RouterOS provides the `/system script` menu for storing reusable scripts.

Create a script:

```routeros
/system script add name=system-info source={
    :local routerName [/system identity get name]
    :local routerVersion [/system resource get version]
    :local cpuLoad [/system resource get cpu-load]

    :log info ("Router: " . $routerName)
    :log info ("RouterOS Version: " . $routerVersion)
    :log info ("CPU Load: " . $cpuLoad)
}
```

Execute the script:

```routeros
/system script run system-info
```

Display configured scripts:

```routeros
/system script print
```

Inspect a script:

```routeros
/system script print detail where name=system-info
```

Remove a script:

```routeros
/system script remove [find name=system-info]
```

Scripts stored in `/system script` can be reused by other RouterOS automation mechanisms.

# 🗁 Script Permissions and Policies

RouterOS scripts have policy settings that determine which operations they are allowed to perform.

Common policies include:

| Policy | Purpose |
| --- | --- |
| `read` | Read configuration and system information |
| `write` | Modify configuration |
| `policy` | Manage policies and permissions |
| `test` | Perform testing operations |
| `password` | Access password-related operations |
| `sensitive` | Access sensitive information |
| `reboot` | Reboot the router |
| `ftp` | Use FTP-related functionality |
| `sniff` | Use packet-sniffing functionality |
| `romon` | Use RoMON functionality |

Inspect script configuration:

```routeros
/system script print detail
```

Create a script with explicit policies:

```routeros
/system script add name=system-check \
    policy=read,test \
    source={
        :local routerName [/system identity get name]
        :put $routerName
    }
```

Grant only the permissions required by the script.

A script that modifies firewall rules, interfaces, or routing configuration requires appropriate permissions beyond read-only access.

# 🗁 Error Handling

Error handling allows scripts to respond to failures instead of stopping without a useful explanation.

RouterOS provides `:onerror` for handling errors.

Example:

```routeros
:onerror errorMessage in={
    /ping 192.168.10.1 count=3
} do={
    :log error ("Operation failed: " . $errorMessage)
}
```

The error variable contains information about the failure.

You can use error handling around operations that may fail, such as configuration changes or external connectivity checks.

Example:

```routeros
:onerror errorMessage in={
    /system identity set name="Router-Branch-01"
    :log info "Router identity updated successfully"
} do={
    :log error ("Unable to update router identity: " . $errorMessage)
}
```

Error handling does not replace validation. Check conditions before performing potentially disruptive operations.

# 🗁 Logging from Scripts

Scripts can write messages to the RouterOS system log.

RouterOS provides `:log` with different severity levels.

Common logging levels include:

* `debug`
* `info`
* `warning`
* `error`

Example:

```routeros
:log info "Automation script started"
:log warning "High CPU utilization detected"
:log error "Monitoring operation failed"
```

You can also generate dynamic messages:

```routeros
:local cpuLoad [/system resource get cpu-load]

:log info ("Current CPU load: " . $cpuLoad . "%")
```

Review the logs:

```routeros
/log print
```

Logging makes scripts easier to troubleshoot and helps administrators understand what actions were performed.

# 🗁 Using Delay in Scripts

The `:delay` command pauses script execution for a specified period.

Example:

```routeros
:log info "Starting monitoring operation"

:delay 5s

:log info "Monitoring operation continued"
```

Supported time values include:

```routeros
:delay 500ms
:delay 5s
:delay 1m
```

Delay can be useful when waiting for an operation or allowing a network service to initialize.

Avoid unnecessary delays in scripts that need to respond quickly to network events.

# 🗁 Creating Reusable Script Logic

You can organize repeated operations into reusable blocks.

For example, create a function-like local variable containing a parsed script:

```routeros
:local logRouterInfo do={
    :local routerName [/system identity get name]
    :local routerVersion [/system resource get version]

    :log info ("Router: " . $routerName)
    :log info ("Version: " . $routerVersion)
}

$logRouterInfo
```

This approach helps separate reusable logic from the main script.

For scripts shared across multiple routers, keep the logic consistent and document required variables, permissions, and dependencies.

# 🗁 Practical Example: System Resource Monitoring

This script reads CPU and memory information and records the results.

```routeros
:local routerName [/system identity get name]
:local cpuLoad [/system resource get cpu-load]
:local freeMemory [/system resource get free-memory]

:log info ("Router: " . $routerName)
:log info ("CPU Load: " . $cpuLoad . "%")
:log info ("Free Memory: " . $freeMemory . " bytes")

:if ($cpuLoad > 80) do={
    :log warning "High CPU utilization detected"
}

:if ($freeMemory < 10000000) do={
    :log warning "Low free memory detected"
}
```

Create it as a reusable script:

```routeros
/system script add name=resource-monitor \
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

        :if ($freeMemory < 10000000) do={
            :log warning "Low free memory detected"
        }
    }
```

Run it:

```routeros
/system script run resource-monitor
```

The example uses fixed thresholds for demonstration. Production thresholds should reflect the router's hardware, workload, and operational requirements.

# 🗁 Practical Example: Interface Status Monitoring

This script checks whether a specified interface is running.

```routeros
:local interfaceName "ether1"
:local interfaceId [/interface find where name=$interfaceName]

:if ([:len $interfaceId] = 0) do={
    :log error ("Interface not found: " . $interfaceName)
} else={
    :local interfaceRunning [/interface get $interfaceId running]

    :if ($interfaceRunning) do={
        :log info ($interfaceName . " is running")
    } else={
        :log warning ($interfaceName . " is not running")
    }
}
```

The script first verifies that the interface exists, then checks its running state.

This prevents the script from attempting to read a property from a nonexistent interface.

# 🗁 Practical Example: Creating a Backup Export

A script can generate a configuration export with a timestamp.

Example:

```routeros
:local routerName [/system identity get name]
:local date [/system clock get date]
:local backupName ($routerName . "-" . $date)

:log info ("Creating configuration export: " . $backupName)

/export file=$backupName
```

RouterOS filename restrictions and date formatting should be considered when using dynamic filenames.

For automated production backups, use a predictable naming convention and ensure that backup files are transferred to a separate storage location.

An export is a text representation of configuration. It is different from a binary system backup.

# 🗁 Script Debugging

Debugging helps identify syntax errors, incorrect conditions, missing variables, and permission problems.

Useful techniques include:

**Display variable values**

```routeros
:local cpuLoad [/system resource get cpu-load]
:put $cpuLoad
```

**Generate log messages**

```routeros
:log info "Reached monitoring section"
```

**Inspect script configuration**

```routeros
/system script print detail
```

**Execute the script manually**

```routeros
/system script run resource-monitor
```

**Review logs**

```routeros
/log print
```

When debugging, test smaller sections before combining them into a larger automation script.

# 🗁 RouterOS Scripting Best Practices

* Use descriptive script and variable names.
* Keep scripts small and focused.
* Add comments for complex operations.
* Validate configuration objects before accessing them.
* Use local variables unless shared state is required.
* Use global variables carefully.
* Handle expected errors with `:onerror`.
* Log important actions and failures.
* Assign only the required script policies.
* Avoid unnecessary infinite loops and delays.
* Test scripts manually before scheduling them.
* Avoid destructive operations without validation.
* Maintain backups before automating configuration changes.
* Document script dependencies and expected behavior.
* Test scripts on a lab router before production deployment.

# 🗁 Practical Scripting Lab

**Lab objectives**

* Create a RouterOS script.
* Work with local and global variables.
* Read system information.
* Use conditional statements.
* Process interface objects.
* Generate log messages.
* Handle errors.
* Execute and debug scripts.

**Create a system information script**

```routeros
/system script add name=lab-system-info \
    policy=read,write,test \
    source={
        :local routerName [/system identity get name]
        :local routerVersion [/system resource get version]
        :local cpuLoad [/system resource get cpu-load]

        :log info ("Router: " . $routerName)
        :log info ("RouterOS: " . $routerVersion)

        :if ($cpuLoad > 80) do={
            :log warning ("High CPU load: " . $cpuLoad . "%")
        } else={
            :log info ("CPU load is normal: " . $cpuLoad . "%")
        }
    }
```

**Execute the script**

```routeros
/system script run lab-system-info
```

**Review the generated logs**

```routeros
/log print
```

**Inspect the script**

```routeros
/system script print detail where name=lab-system-info
```

**Remove the lab script after testing**

```routeros
/system script remove [find name=lab-system-info]
```

**Expected outcome**

You should be able to create, execute, inspect, and remove a RouterOS script while understanding its variables, conditions, permissions, and logging behavior.

RouterOS scripting provides the foundation for more advanced automation using Scheduler, Netwatch, automated backups, notifications, and API integration.
