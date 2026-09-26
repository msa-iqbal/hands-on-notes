# Boot

Practical, structured notes for understanding the **system boot process, firmware, bootloaders, kernel loading, and system initialization** across operating systems.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Boot Process|Complete system startup sequence, hardware initialization, firmware, bootloader, kernel loading, and system initialization|
|02|BIOS|Legacy BIOS, POST, firmware configuration, boot devices, boot order, and BIOS-based startup|
|03|UEFI|UEFI firmware, EFI System Partition, UEFI boot process, Secure Boot, and firmware configuration|
|04|Bootloader|Bootloader concepts, boot entries, boot configuration, bootloader stages, and operating system selection|
|05|Kernel Loading|Kernel loading, initramfs/initrd, kernel parameters, hardware initialization, and transition to the operating system|
|06|System Initialization|Initial system setup, process initialization, service startup, target/runlevel concepts, and transition to the usable system|

## Structure

The notes progress from **boot fundamentals → BIOS and UEFI → bootloaders → kernel loading → system initialization**.

The early topics establish the complete startup sequence and explain how firmware prepares the hardware for operating system startup. BIOS and UEFI cover the two major firmware approaches, including their boot mechanisms and configuration.

The later sections focus on bootloaders, loading the operating system kernel and initial filesystem, and initializing the userspace environment, services, and system processes.

Each topic is separated into focused notes for easier learning, reference, troubleshooting, and expansion.

## Focus

- System boot process
- Hardware initialization
- POST
- BIOS and legacy boot
- UEFI firmware
- EFI System Partition (ESP)
- Secure Boot
- Boot devices and boot order
- Bootloader architecture
- Boot entries and configuration
- Operating system selection
- Kernel loading
- Kernel parameters
- `initramfs` and `initrd`
- Hardware initialization
- Userspace initialization
- Initial process
- Service startup
- Targets and runlevels
- Boot troubleshooting

## Goal

> Understand how a computer progresses from power-on and firmware initialization to bootloader execution, kernel loading, and complete operating system initialization, and use this knowledge to diagnose and manage boot problems.
