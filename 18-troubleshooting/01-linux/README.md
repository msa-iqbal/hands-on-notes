# Linux

Practical, structured **Linux troubleshooting guides** for diagnosing boot failures, package problems, permissions, storage, filesystems, networking, services, resource usage, and application crashes.

## Contents

| #   | Topic                        | What You'll Learn                                                                                                                                        |
| --- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 01  | Boot Failure                 | Identify common boot problems, inspect boot errors, check system logs, diagnose filesystem and service issues, and restore normal system startup         |
| 02  | Package Installation Failure | Diagnose package-manager errors, dependency conflicts, repository problems, broken packages, and failed package installations                            |
| 03  | Permission Denied            | Diagnose file, directory, command, and resource permission problems, inspect ownership and permissions, and apply appropriate fixes                      |
| 04  | Disk Space Full              | Identify filesystem and partition usage, locate large files and directories, clean unnecessary data, and recover available disk space                    |
| 05  | Disk Not Detected            | Identify missing disks, inspect hardware and kernel detection, verify device nodes, check partition information, and diagnose storage problems           |
| 06  | Mount Failure                | Diagnose filesystem mount errors, inspect devices and mount points, verify filesystem types and options, and resolve mounting problems                   |
| 07  | Filesystem Errors            | Identify filesystem problems, inspect system logs, check filesystem health, safely perform filesystem checks, and recover from common errors             |
| 08  | Network Not Working          | Diagnose network-interface, connectivity, routing, and configuration problems using system and network diagnostic tools                                  |
| 09  | DNS Not Working              | Diagnose DNS-resolution failures, inspect resolver configuration, test DNS servers, identify local configuration problems, and restore name resolution   |
| 10  | Service Not Starting         | Diagnose failed services, inspect service status and logs, identify configuration or dependency problems, and restore service operation                  |
| 11  | systemd Service Failure      | Diagnose systemd unit failures, inspect journal logs, verify unit configuration, analyze dependencies, and correct service startup problems              |
| 12  | High CPU Usage               | Identify processes consuming CPU, inspect process behavior, analyze system load, diagnose resource-intensive workloads, and reduce unnecessary CPU usage |
| 13  | High Memory Usage            | Identify memory-consuming processes, inspect RAM and swap usage, diagnose memory pressure, and resolve excessive memory consumption                      |
| 14  | Application Crash            | Diagnose crashed applications, inspect logs and core information, identify configuration or dependency problems, and verify application recovery         |

## Structure

The guides progress from **boot and package problems → permissions and storage → filesystems → networking and DNS → services and systemd → CPU and memory → application crashes**.

The first sections focus on fundamental system failures that can prevent Linux from starting correctly or installing required software. Permission and disk-space guides then address common operational problems involving access control and filesystem capacity.

Disk detection, mounting, and filesystem-error guides provide a structured approach to diagnosing storage problems. These guides cover device detection, partitions, mount points, filesystem types, health checks, and safe recovery procedures.

Networking and DNS troubleshooting then focus on connectivity and name-resolution problems. Service and systemd guides address applications and background services that fail to start, stop unexpectedly, or encounter configuration and dependency problems.

The final guides focus on system-resource and application-level problems. CPU and memory troubleshooting helps identify resource pressure and problematic processes, while application-crash troubleshooting provides a structured approach to logs, dependencies, configuration, and recovery.

Each troubleshooting guide follows a **problem-oriented workflow** covering symptoms, possible causes, diagnosis, solution, verification, prevention, and related issues.

## Focus

- Linux boot and package-management problems
- File permissions and access control
- Disk space, disk detection, and mounting
- Filesystem errors and recovery
- Network connectivity and DNS resolution
- Service and systemd failures
- CPU and memory usage
- Application crashes and recovery
- Logs, diagnostics, root-cause analysis, and verification

## Goal

> Build a practical Linux troubleshooting knowledge base that makes common system, storage, networking, service, resource, and application problems easier to identify, diagnose, resolve, verify, and prevent.
