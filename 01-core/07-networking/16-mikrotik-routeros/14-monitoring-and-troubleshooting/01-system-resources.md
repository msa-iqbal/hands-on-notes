# System Resources

System resource monitoring in MikroTik RouterOS provides visibility into the router's hardware utilization, operating condition, and available capacity.

Monitoring system resources helps network administrators identify performance bottlenecks, detect abnormal resource consumption, plan hardware upgrades, and troubleshoot unexpected router behavior.

RouterOS provides several commands and monitoring facilities for inspecting CPU usage, memory, storage, uptime, temperature, voltage, and other hardware information.

# 🗁 Understanding System Resources

The primary system resource information is available through:

```routeros
/system/resource/print
```

Example output:

```routeros
uptime: 2d 04:15:32
version: 7.x
build-time: ...
factory-software: ...
free-memory: 450.0MiB
total-memory: 512.0MiB
cpu: ARM
cpu-count: 4
cpu-frequency: 1400MHz
cpu-load: 12%
free-hdd-space: 90.0MiB
total-hdd-space: 128.0MiB
architecture-name: arm
board-name: ...
platform: MikroTik
```

The exact fields depend on the router model and RouterOS version.

Important resource fields:

| Resource | Description |
| --- | --- |
| `uptime` | Time elapsed since the router was last started |
| `version` | Installed RouterOS version |
| `free-memory` | Currently available RAM |
| `total-memory` | Total installed RAM |
| `cpu` | CPU architecture or processor type |
| `cpu-count` | Number of CPU cores |
| `cpu-frequency` | CPU operating frequency |
| `cpu-load` | Current CPU utilization percentage |
| `free-hdd-space` | Available storage space |
| `total-hdd-space` | Total storage capacity |
| `architecture-name` | RouterOS hardware architecture |
| `board-name` | Hardware board model |
| `platform` | Hardware platform manufacturer or type |

# 🗁 Monitoring CPU Usage

CPU utilization indicates how much processing capacity the router is currently using.

Run:

```routeros
/system/resource/print
```

Check the `cpu-load` field.

For continuously refreshed CPU information:

```routeros
/system/resource/monitor
```

The monitor command displays changing resource values until interrupted.

Press `Ctrl+C` to stop monitoring.

High CPU utilization may be caused by:

* Large numbers of firewall rules.
* Heavy connection tracking activity.
* Complex Mangle rules.
* PCC load balancing.
* VPN encryption and decryption.
* Queue processing and bandwidth management.
* Routing protocol calculations.
* High packet-per-second traffic.
* Excessive logging.
* Traffic inspection and packet sniffing.

A router can experience high CPU usage even when bandwidth consumption is relatively low. Packet-per-second processing, small packets, and complex firewall processing can create substantial CPU load.

# 🗁 Monitoring CPU Usage Per Core

On multi-core routers, overall CPU utilization does not always reveal how processing is distributed across individual cores.

Use:

```routeros
/tool/profile
```

The profiler displays CPU consumption by RouterOS processes and services.

Example output:

```routeros
NAME                    CPU
ethernet                2%
firewall                5%
networking              3%
management              1%
routing                 0%
queuing                 1%
```

The actual process names and values depend on the device and workload.

To inspect CPU usage by individual cores, use the profiler's available core selection options:

```routeros
/tool/profile cpu=all
```

Where supported, individual CPU cores can also be selected:

```routeros
/tool/profile cpu=0
```

```routeros
/tool/profile cpu=1
```

The profiler is useful for identifying which RouterOS subsystem is consuming processing resources.

For example:

* High `firewall` usage may indicate expensive filtering, NAT, or connection tracking operations.
* High `queuing` usage may indicate intensive queue processing.
* High `networking` usage may indicate packet-processing overhead.
* High `routing` usage may indicate routing-related processing.

Profiler output is a diagnostic snapshot. Compare results under normal and peak traffic conditions to identify recurring bottlenecks.

# 🗁 Monitoring Memory Usage

RAM is used by RouterOS services, connection tracking, routing tables, queues, firewall processing, and other system components.

Run:

```routeros
/system/resource/print
```

Check:

* `total-memory`
* `free-memory`

Memory usage can be estimated using:

```text
Used Memory = Total Memory - Free Memory
```

Memory utilization percentage can be calculated using:

```text
Memory Usage (%) = (Used Memory / Total Memory) × 100
```

For example, if a router has 512 MiB of RAM and 128 MiB is free:

```text
Used Memory = 512 - 128
            = 384 MiB

Memory Usage = (384 / 512) × 100
             = 75%
```

A high memory utilization percentage is not automatically a problem. RouterOS may use available memory for internal operations and caching.

Investigate memory usage when:

* Free memory decreases continuously.
* Router services become unstable.
* Configuration changes fail because of insufficient memory.
* The router becomes unresponsive.
* Memory consumption increases unexpectedly after enabling a service.

# 🗁 Monitoring Storage

Storage contains RouterOS system files, configuration backups, logs, packages, and other stored data.

Inspect storage information using:

```routeros
/system/resource/print
```

Check:

* `free-hdd-space`
* `total-hdd-space`

List available files:

```routeros
/file/print
```

Inspecting stored files helps identify unnecessary backups, large log files, and other files consuming storage.

Storage management considerations:

* Remove obsolete backup files when they are no longer required.
* Avoid storing excessive logs on limited internal storage.
* Keep sufficient free space for system operations and updates.
* Store important backups on an external management system.
* Avoid repeatedly writing unnecessary data to flash storage.

Not every MikroTik device exposes storage in the same way. Some models use flash storage, while others have different storage capabilities.

# 🗁 Monitoring System Temperature

Temperature monitoring helps identify overheating conditions that may affect device stability and hardware reliability.

Where supported, inspect hardware health information using:

```routeros
/system/health/print
```

To continuously monitor available health sensors:

```routeros
/system/health/monitor
```

Depending on the hardware model, available information may include:

* CPU temperature.
* Board temperature.
* Voltage.
* Fan speed.
* Other hardware sensor readings.

Not every device exposes all these values.

Temperature thresholds are hardware-specific. Use the manufacturer's specifications for the particular MikroTik model rather than applying one universal temperature limit.

Possible causes of excessive temperature include:

* Poor ventilation.
* High ambient temperature.
* Direct sunlight.
* Dust accumulation.
* Faulty cooling components.
* Installation inside an enclosed cabinet without adequate airflow.

# 🗁 Monitoring Voltage and Power

Voltage monitoring can help identify unstable power conditions.

Run:

```routeros
/system/health/print
```

If the device exposes voltage sensors, RouterOS displays their current readings.

Unstable power can cause:

* Unexpected router restarts.
* Interface instability.
* Device malfunction.
* Hardware damage over time.

For devices powered through PoE, inspect the power source, cable quality, connector condition, and compatibility with the device's supported input voltage.

The available voltage and power information depends on the hardware model.

# 🗁 Checking System Identity and Router Information

System identity helps administrators distinguish routers in multi-device environments.

Display the configured identity:

```routeros
/system/identity/print
```

Change the identity:

```routeros
/system/identity/set name=CORE-Router
```

Inspect the router's board information:

```routeros
/system/routerboard/print
```

Where supported, this command provides information such as:

* RouterBOARD model.
* Current firmware version.
* Factory firmware version.
* RouterBOARD serial number.

To inspect available RouterOS packages:

```routeros
/system/package/print
```

These commands help administrators identify the device, installed software, and firmware information during maintenance and troubleshooting.

# 🗁 Monitoring Uptime and Restart History

Uptime indicates how long the router has been running since its most recent restart.

Check uptime:

```routeros
/system/resource/print
```

The `uptime` field shows the elapsed operating time.

Inspect system logs for restart-related events:

```routeros
/log/print
```

Filter logs for relevant messages:

```routeros
/log/print where topics~"system"
```

RouterOS logging can help identify events associated with:

* Manual reboots.
* Power interruptions.
* System shutdowns.
* RouterOS upgrades.
* Configuration changes.
* Hardware or software problems.

A short uptime does not necessarily indicate a failure. It may be the result of planned maintenance, a software upgrade, or an administrator-initiated reboot.

# 🗁 Monitoring Resource Usage with WinBox

WinBox provides a graphical interface for monitoring system resources.

Open WinBox and connect to the router.

Navigate to:

**System → Resources**

This window displays information such as:

* CPU utilization.
* Memory usage.
* Free and total storage.
* Router uptime.
* RouterOS version.
* Hardware architecture.
* CPU information.

For additional hardware information, navigate to:

**System → Health**

The available fields depend on the connected device.

# 🗁 Best Practices for Resource Monitoring

Follow these practices when managing MikroTik routers:

* Establish baseline CPU, memory, and storage usage during normal operation.
* Monitor resource utilization during peak traffic periods.
* Investigate sustained high CPU utilization instead of relying on isolated readings.
* Use `/tool/profile` to identify resource-intensive RouterOS processes.
* Monitor available hardware health sensors where supported.
* Keep sufficient free storage for maintenance and upgrades.
* Review system logs when unexpected restarts occur.
* Avoid excessive debug logging on production routers.
* Compare resource utilization before and after major configuration changes.
* Monitor resource trends over time using an external monitoring system.

# 🗁 Practical Resource Monitoring Checklist

Use the following commands as a basic system resource inspection routine:

```routeros
/system/resource/print
```

```routeros
/system/resource/monitor
```

```routeros
/tool/profile
```

```routeros
/system/health/print
```

```routeros
/system/identity/print
```

```routeros
/system/routerboard/print
```

```routeros
/system/package/print
```

```routeros
/log/print
```

Some commands and output fields depend on the device model and RouterOS version.

# 🗁 Troubleshooting Resource Problems

| Problem | Possible Cause | Investigation |
| --- | --- | --- |
| High CPU usage | Firewall, queues, routing, or packet processing | `/tool/profile` |
| Low free memory | High connection count or resource-intensive services | `/system/resource/print` |
| Low storage | Excessive backups, logs, or stored files | `/file/print` |
| High temperature | Poor ventilation or excessive ambient temperature | `/system/health/print` |
| Unexpected restart | Power interruption, manual reboot, or system failure | `/log/print` |
| Unresponsive router | CPU saturation, memory pressure, or excessive traffic | Resource monitor and profiler |

Resource monitoring should be performed regularly, especially on routers providing critical services such as PPPoE, NAT, firewalling, VPNs, routing, and bandwidth management.
