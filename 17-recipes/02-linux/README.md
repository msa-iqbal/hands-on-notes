# Linux

Practical, step-by-step **Linux recipes** for managing disks, users, services, SSH, firewalls, network shares, scheduled tasks, disk space, and system backups.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Mount Disk|Identify disks and partitions, create mount points, mount filesystems, verify mounts, and troubleshoot common mounting issues|
|02|Mount Disk at Boot|Configure persistent filesystem mounts using `/etc/fstab`, UUIDs, mount options, and boot-time verification|
|03|Create systemd Service|Create, configure, start, stop, enable, monitor, and troubleshoot custom systemd services|
|04|Create Linux User|Create and manage users, configure groups, set passwords, assign permissions, and manage user accounts|
|05|Configure SSH Server|Install and configure OpenSSH, manage SSH access, configure authentication, secure the server, and troubleshoot connections|
|06|Configure Firewall|Configure firewall rules, allow or deny network traffic, manage ports, verify rules, and apply basic host-level network security|
|07|Setup Samba Share|Install and configure Samba, create shared directories, manage access permissions, configure users, and connect to network shares|
|08|Setup Cron Job|Create scheduled tasks with cron, configure crontab entries, manage execution schedules, capture output, and troubleshoot jobs|
|09|Manage Disk Space|Inspect filesystem usage, identify large files and directories, clean unnecessary data, and monitor available disk space|
|10|Create Backup Script|Create automated backup scripts, archive files, manage backup destinations, schedule backups, verify results, and handle failures|

## Structure

The recipes progress from **disk management → persistent mounts → system services → user management → SSH → firewall → Samba → scheduled tasks → disk-space management → backups**.

The first recipes focus on Linux storage management, including mounting disks and configuring filesystems to mount automatically at boot. These provide the foundation for managing local storage and persistent filesystem configuration.

The next sections cover core system administration tasks such as creating users, configuring systemd services, securing remote access with SSH, and managing host-level firewall rules.

Samba then introduces network file sharing between Linux and other systems, while cron provides a practical method for scheduling recurring administrative tasks.

The final recipes focus on system maintenance and reliability through disk-space management and automated backup scripts. Together, these procedures cover common Linux administration tasks that can be reproduced across systems.

Each recipe follows an **action-oriented workflow** with commands, configuration, expected results, verification steps, troubleshooting guidance, and important safety considerations.

## Focus

- Disk mounting and filesystem management
- Persistent mounts with `/etc/fstab`
- systemd service management
- Linux users, groups, and permissions
- SSH server configuration and security
- Firewall configuration and network access
- Samba network file sharing
- Cron scheduling and automated tasks
- Disk-space management and cleanup
- Backup scripting and verification

## Goal

> Build a practical Linux recipe knowledge base that makes common system administration, storage, networking, service management, automation, maintenance, and backup procedures easy to follow, verify, troubleshoot, and reproduce.
