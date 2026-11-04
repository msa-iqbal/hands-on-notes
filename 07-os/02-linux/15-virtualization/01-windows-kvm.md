# How to Install Windows 10/11 on Ubuntu Using KVM/QEMU

Running Windows inside Ubuntu is easy with KVM, QEMU, libvirt, and Virtual Machine Manager (virt-manager). These tools let you run Windows in a virtual machine without dual-booting.

Features include hardware virtualization, fast VirtIO storage and networking, USB passthrough, snapshots, and easy VM management.

> Example: <mark>This guide uses Windows 10</mark>. Windows 11 can also run in a VM but requires TPM 2.0 and UEFI support for Secure Boot.

Note: Windows 10 support ended on October 14, 2025. For a new VM in 2026, Windows 11 is generally the better choice.
# ☰ RESOURCES & REFERENCES

> Explore the documentation, tools, guides, and references behind the topic.

* [Install windows 10 on Ubuntu KVM](https://youtu.be/ip0HqeMK5yo)

---
# 🗁 STEP 1: KVM and QEMU Installation

**What We Are Building**

The final environment will look like this:

```text
┌──────────────────────────────────────────────────┐
│                (Ubuntu 26.04 Host)               │
│                                                  │
│  ┌────────────────────────────────────────────┐  │
│  │           (Virtual Machine Manager)        │  │
│  │                                            │  │
│  │                virt-manager                │  │
│  │                     │                      │  │
│  │                  libvirt                   │  │
│  │                     │                      │  │
│  │                    QEMU                    │  │
│  │                     │                      │  │
│  │                    KVM                     │  │
│  │                     │                      │  │
│  │       ┌────────────────────────────┐       │  │
│  │       │       Windows 10/11        │       │  │
│  │       │                            │       │  │
│  │       │  4 vCPU                    │       │  │
│  │       │  6 GB RAM                  │       │  │
│  │       │  VirtIO Disk               │       │  │
│  │       │  VirtIO Network            │       │  │
│  │       │  SPICE Display             │       │  │
│  │       │  UEFI                      │       │  │
│  │       └────────────────────────────┘       │  │
│  └────────────────────────────────────────────┘  │
│                                                  │
│                (Intel VT-x / AMD-V)              │
└──────────────────────────────────────────────────┘
```

The technologies have different responsibilities.

* **KVM** — KVM (Kernel-based Virtual Machine) is the Linux kernel's hardware virtualization mechanism.
* **QEMU** — Its provides the virtual machine process and virtual hardware.
* **libvirt** — Its provides a management layer for QEMU/KVM virtual machines.
* **virt-manager** — Virtual Machine Manager (`virt-manager`) provides a graphical interface for managing libvirt VMs.

This combination is widely used for Linux virtualization and is an excellent foundation for learning virtualization and DevOps.

Other essentials are —

* **OVMF** — Its provides UEFI firmware for the virtual machine.
* **VirtIO** — Its provides high-performance paravirtualized devices such as `Virtual disks`, `Network adapters`, `SCSI controllers`
* **SPICE** — Its provides the graphical console and guest integration features.
### ၊၊||၊ Windows 10 vs Windows 11

The basic KVM architecture works for both Windows 10 and Windows 11.

However, Windows 11 requires additional virtual hardware.

Microsoft lists requirements including:

* UEFI firmware
* Secure Boot capability
* TPM 2.0
* At least 4 GB RAM
* At least 64 GB storage
* At least two virtual processors

Therefore:

**Windows 10**

```text
UEFI
VirtIO
SPICE
No vTPM required
```

**Windows 11**

```text
UEFI
Secure Boot
TPM 2.0
VirtIO
SPICE
```

For this practical guide, Windows 10 is used because it makes the VirtIO installation process particularly easy to demonstrate.

### ၊၊||၊ Requirements (Hardware)

Recommended:

* 64-bit Intel or AMD processor
* Intel VT-x or AMD-V
* At least 8 GB RAM
* 80–100 GB free storage
* SSD recommended
* Ubuntu 26.04 LTS

For comfortable Windows virtualization:

```text
16 GB RAM or more
```

is preferable.

**Recommended VM configuration**

For a host with 16 GB RAM:

```text
CPU       : 4 vCPU
RAM       : 6 GB
Disk      : 80–100 GB
Storage   : VirtIO
Network   : VirtIO
Display   : SPICE
Firmware  : UEFI
```

These values are starting points, not strict requirements.

### ၊၊||၊ Check Hardware Virtualization

Open Terminal.

Run:

```bash
lscpu | grep -E 'Virtualization|Model name'
```

On an Intel system, you should see something similar to:

```text
Model name: Intel(R) Core(TM) i5-1135G7
Virtualization: VT-x
```

For AMD:

```text
Virtualization: AMD-V
```

### ၊၊||၊ Check whether `/dev/kvm` exists

Run:

```bash
ls -l /dev/kvm
```

You should see something similar to:

```text
crw-rw----+ 1 root kvm ... /dev/kvm
```

### ၊၊||၊ Install the KVM checker

```bash
sudo apt update
sudo apt install -y cpu-checker
```

Then:

```bash
kvm-ok
```

Expected result:

```text
INFO: /dev/kvm exists
KVM acceleration can be used
```

If KVM acceleration is unavailable, enter your computer's BIOS/UEFI and enable hardware virtualization.

For Intel systems, the option may be called:

```text
Intel Virtualization Technology
VT-x
Intel VT
```

For AMD systems:

```text
SVM
AMD-V
```

### ၊၊||၊ Install KVM/QEMU and Virtualization Packages

Ubuntu 26.04 uses newer HWE virtualization packages, so older tutorials may show slightly different package names.

Install the main components:

```bash
sudo apt update
```

```bash
sudo apt install -y \
qemu-system-x86 \
qemu-utils \
libvirt-daemon-system \
libvirt-clients \
virt-manager \
ovmf \
bridge-utils \
dnsmasq-base \
cpu-checker
```

What these packages provide -

| Package                 | Purpose                          |
| ----------------------- | -------------------------------- |
| `qemu-system-x86`       | QEMU x86 virtual machines        |
| `qemu-utils`            | QEMU disk utilities              |
| `libvirt-daemon-system` | libvirt virtualization service   |
| `libvirt-clients`       | `virsh` command-line tools       |
| `virt-manager`          | GUI virtualization manager       |
| `ovmf`                  | UEFI firmware                    |
| `bridge-utils`          | Network bridge utilities         |
| `dnsmasq-base`          | Virtual network DHCP/DNS support |
| `cpu-checker`           | KVM capability checking          |

### ၊၊||၊ What about `qemu-kvm`? (Optional)

On some Ubuntu versions, tutorials tell you to run:

```bash
sudo apt install qemu-kvm
```

On newer Ubuntu releases, `qemu-kvm` may be a virtual package provided by another QEMU package.

If Ubuntu tells you:

```text
Package qemu-kvm is a virtual package
```

that isn't necessarily an error.

For this setup, installing:

```bash
sudo apt install qemu-system-x86
```

is sufficient.

### ၊၊||၊ Configure User Permissions

Add your user to the KVM and libvirt groups:

```bash
sudo usermod -aG kvm,libvirt $USER
```

Check:

```bash
groups
```

You should eventually see:

```text
kvm
libvirt
```

#### 🔥 Important

Log out and log back in.

A reboot is also fine:

```bash
systemctl reboot -i
```

After logging back in:

```bash
groups
```

### ၊၊||၊ Configure the `libvirt` Network

libvirt normally provides a default NAT network.

Check it:

```bash
virsh net-list --all
```

You should see something like:

```text
Name      State    Autostart   Persistent
--------------------------------------------
default   active   yes         yes
```

If `default` exists but is inactive:

```bash
sudo virsh net-start default
```

Enable automatic startup:

```bash
sudo virsh net-autostart default
```

Check again:

```bash
 virsh net-list --all
```

### ၊၊||၊ How the default network works

The typical architecture is:

```plaintext
Windows VM
    │
    ▼
VirtIO Network Adapter
    │
    ▼
libvirt default network
    │
    ▼
   NAT
    │
    ▼
  Ubuntu
    │
    ▼
 Internet
```

The Windows VM does not need a physical network adapter.

### ၊၊||၊ Verify the KVM Installation

Run:

```bash
kvm-ok
```

Then:

```bash
lsmod | grep kvm
```

On Intel:

```text
kvm_intel
kvm
```

Check libvirt:

```bash
virsh list --all
```

No VMs should exist yet.

That's fine.

Check the network:

```bash
virsh net-list --all
```

You want the `default` network active.

# 🗁 STEP-2: Configuration `virt-manager`

### ၊၊||၊ Download the Windows ISO

For Windows 10, download the ISO from Microsoft's official [Windows 10 download page](https://www.microsoft.com/en-us/software-download/windows10ISO)

For Windows 11, use Microsoft's official [Windows 11 download page](https://www.microsoft.com/en-us/software-download/windows11)

Store the ISO somewhere such as:

```text
~/Downloads/
```

For example:

```text
~/Downloads/Win10_22H2.iso
```

For Windows 11:

```text
~/Downloads/Win11.iso
```

Use a legitimate Windows license/product key for activation.

### ၊၊||၊ Download `VirtIO` Drivers

Windows does not necessarily contain the VirtIO drivers required for your virtual storage and network devices.

The `virtio-win` project provides Windows drivers and guest tools for QEMU/KVM. The project maintains stable and latest ISO builds.

Download the [VirtIO Windows ISO](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win.iso) from the [official VirtIO project](https://virtio-win.github.io/Knowledge-Base/Driver-installation.html)

> Unofficial [download link](http://u.pc.cd/F7N)

Save it as:

```text
~/Downloads/virtio-win.iso
```

You should now have:

```text
~/Downloads/
├── Win10_22H2.iso
└── virtio-win.iso
```

VirtIO provides optimized virtual storage and networking drivers for Windows guests running on QEMU/KVM.

### ၊၊||၊ Create the Windows 10 Virtual Machine

Open Terminal and type:

```bash
virt-manager
```

Select:

```text
QEMU/KVM
```

Then:

```text
File
→ New Virtual Machine
```

#### STEP 1 — INSTALLATION MEDIA

The configuration method is shown in the image below:

![How to Install Windows 10/11 on Ubuntu Using KVM/QEMU](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789883567/Screenshot_From_2026-09-19_09-53-36_tyrehs.png "How to Install Windows 10/11 on Ubuntu Using KVM/QEMU")

* Select **Local install media (ISO image or CDROM)**
* Then click **Forward**

#### STEP 2 — SELECT THE WINDOWS ISO

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789884067/Screenshot_From_2026-09-19_09-54-54_scadbd.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789884067/Screenshot_From_2026-09-19_09-54-54_scadbd.png)

* Click **Browse**
* Select from `~/Downloads/Win10_22H2.iso`
* virt-manager may automatically detect: `Microsoft Windows 10`. (If it doesn't, select the appropriate Windows version manually.)
* Click **Forward**

#### STEP 3 — ALLOCATE RAM AND CPU

For a 16 GB host, a reasonable starting point is:

```text
RAM: 6144 MB
CPU: 4 vCPU
```

For an 8 GB host:

```text
RAM: 4096 MB
CPU: 2–4 vCPU
```

For a 32 GB host:

```text
RAM: 8192 MB or more
```

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885219/3_Create_new_virtual_machine_f0ruqz.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885219/3_Create_new_virtual_machine_f0ruqz.png)

* Set RAM as `4096` MB (4GB) (NB: Its depends on your host computer)
* Set CPU cores as `4` (NB: Its depends on your host computer)
* Then Click **Forward**.

#### STEP 4 — ALLOCATE STORAGE

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792466195/1._KVM-Configuration_ejx3tf.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792466195/1._KVM-Configuration_ejx3tf.png)

Create a virtual disk storage:

```text
80–100 GB
```

For a general-purpose Windows VM:

```text
100 GB
```

is a comfortable starting point if sufficient host storage is available.

The virtual disk format as `qcow2`

The default libvirt location is commonly:

```text
/var/lib/libvirt/images/
```

For example:

```text
/var/lib/libvirt/images/Windows-10.qcow2
```

#### STEP 5 — CREATE A NEW VIRTUAL MACHINE

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885405/4_Create_new_virtual_machine_zum7sh.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885405/4_Create_new_virtual_machine_zum7sh.png)

* Set name of VM OS
* Select Network

Avoid giving the VM almost all of the host's RAM.

Ubuntu still needs memory for:

* GNOME
* Browser
* VS Code
* Docker
* Background services
* QEMU
* Other applications

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885739/5_Create_new_virtual_machine_hjqipv.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885739/5_Create_new_virtual_machine_hjqipv.png)

### ၊၊||၊ Configure UEFI (Especially for `Windows 11`)

💡<mark>If you want to install Windows 10 os, you can ignore this step.</mark>

Before starting installation, enable UEFI.

In virt-manager:

```
Overview
→ Firmware
```

Select:

```
UEFI
```

This normally uses OVMF/EDK2 firmware.

For Windows 10:

```
UEFI
```

is sufficient.

For Windows 11, configure:

```
UEFI
+ Secure Boot capability
+ TPM 2.0
```

Microsoft requires TPM 2.0 and UEFI/Secure Boot capability for Windows 11.

### ၊၊||၊ Configure the Virtual Disk

#### STEP-1: CONFIGURE THE `SATA DISK-1`

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885993/6_Configure_the_Virtual_Machine_eh1mnj.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885993/6_Configure_the_Virtual_Machine_eh1mnj.png)

* Select **SATA Disk 1**
* Set **Disk bus** as `VirtIO`
* Click **Apply**

This gives Windows high-performance paravirtualized storage.

However, Windows Setup may not initially recognize the disk. That is why the VirtIO ISO must also be attached.

#### STEP-2: ATTACH THE `VirtIO` ISO

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789886621/9_Configure_the_Virtual_Machine_iaivq9.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789886621/9_Configure_the_Virtual_Machine_iaivq9.png)

In virt-manager:

* Click **Add Hardware**
* Select **Storage**
* Select as `Select or create custom storage`
* Click **Manage** and select from `~/Downloads/virtio-win-0.1.302.iso`
* Then click **Choose Volume**

Now, Which setting you have to configure that shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789887060/10_Configure_the_Virtual_Machine_elbeku.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789887060/10_Configure_the_Virtual_Machine_elbeku.png)

* Set **Device Type** as `CDROM device`
* Set **Bus type** as `SATA`
* Then click **Finish**

#### STEP-3: CHECK `SATA CDROM-1` and `SATA CDROM-2`

`SATA CDROM-1` will be configured with Windows 10 ISO.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792468435/6.1_ymv3vi.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792468435/6.1_ymv3vi.png)

`SATA CDROM-2` will be configured with `virtio-win-0.1.302.iso`.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792468435/6.2_lu1nla.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792468435/6.2_lu1nla.png)

#### STEP-4: SET BOOT SEQUENCE

Which setting you have to configure that shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789892322/12_Configure_the_Virtual_Machine_kb6fn9.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789892322/12_Configure_the_Virtual_Machine_kb6fn9.png)

* Select **Boot Options**
* Select as `Enable boot menu` and organize boot sequences as -
  * `SATA CDROM 1`
  * `VirtIO Disk 1`
* Click **Apply**

### ၊၊||၊ Configure SPICE Display

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1790075428/62_Configure_SPICE_Display_be7ss0.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1790075428/62_Configure_SPICE_Display_be7ss0.png)

* Click **Add Hardware**
* Select **Graphics**
* Set type as `Spice server`
* Then click **Finish**

For the video device, use a model supported by your current virt-manager/QEMU/Windows combination. QXL remains a common SPICE configuration; VirtIO-GPU may also be available depending on the stack and Windows driver support.

Note: If any problem faced for selecting as `Spice server`, then switched as `VNC server`
### ၊၊||၊ Configure the Network

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789886336/8_Configure_the_Virtual_Machine_pnw4bn.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789886336/8_Configure_the_Virtual_Machine_pnw4bn.png)

* Select the network interface as **NIC :9d:f3:8f**
* Set `Network source` as `Virtual network 'default': NAT`
* Set `Device model` as `virtio`
* Then click **Apply**

The final network architecture is:

```text
Internet
   |
Ubuntu
   |
libvirt_NAT
   |
VirtIO_NIC
   |
Windows
```

### ၊၊||၊ Configure the CPU

Use:

```text
CPU model:
host-passthrough
```

This exposes appropriate host CPU features to the guest.

#### 💡Important! <mark>CPU topology correction</mark>

Do **not** use:

```text
Sockets: 1
Cores: 1
Threads: 2
```

and then call the VM a 4-vCPU VM.

That configuration represents:

```text
1 × 1 × 2 = 2 vCPU
```

For 4 vCPU, if manually specifying topology, for example:

```text
Sockets: 1
Cores: 2
Threads: 2
```

gives:

```text
1 × 2 × 2 = 4 vCPU
```

However, for a normal desktop Windows VM, simply allocating 4 virtual CPUs is usually easier.

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789891839/11_Configure_the_Virtual_Machine_fvnhcf.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789891839/11_Configure_the_Virtual_Machine_fvnhcf.png)

* Select **CPUs**
* Select `Configuration` as `Copy host CPU configuration (host-passthrough)`
* Select `Manually set CPU topology` and set as Sockets: `1`, Cores: `2`, Threads: `2`
* Click **Apply**

# 🗁 STEP-3: Windows Installation

### STEP-1: START WINDOWS INSTALLATION

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789892522/13_Configure_the_Virtual_Machine_ynuklg.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789892522/13_Configure_the_Virtual_Machine_ynuklg.png)

Click **Begin Installation**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789892842/14_Windows_OS_Installation_on_Virtual_Machine_uhku9t.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789892842/14_Windows_OS_Installation_on_Virtual_Machine_uhku9t.png)

When `Allow inhibiting shortcut` diaglog box shown then click `Allow` button.

* Select **Language**
* Select **Time and currency Keyboard**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893008/15_Windows_OS_Installation_on_Virtual_Machine_n89efn.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893008/15_Windows_OS_Installation_on_Virtual_Machine_n89efn.png)

Then, click **Install now**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893232/16_Windows_OS_Installation_on_Virtual_Machine_qif4j4.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893232/16_Windows_OS_Installation_on_Virtual_Machine_qif4j4.png)

When prompted for a product key, you can select as `I don't have a product key`, if you plan to activate Windows later with a valid license.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893234/17_Windows_OS_Installation_on_Virtual_Machine_ac6pwi.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893234/17_Windows_OS_Installation_on_Virtual_Machine_ac6pwi.png)

Select the Windows edition you are licensed to use. For example, `Windows 10 Pro`

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893237/18_Windows_OS_Installation_on_Virtual_Machine_rxmvj8.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893237/18_Windows_OS_Installation_on_Virtual_Machine_rxmvj8.png)

Accept **I accept the license terms**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893238/19_Windows_OS_Installation_on_Virtual_Machine_bvh1mj.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893238/19_Windows_OS_Installation_on_Virtual_Machine_bvh1mj.png)

Select as `Custom: Install Windows only (advanced)`

### STEP-2 (Method-1): LOAD THE `VirtIO` STORAGE DRIVER

When you may reach as `Where do you want to install Windows?` and No drives were found.

This happens because Windows Setup does not have the required VirtIO storage driver loaded.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893241/20_Windows_OS_Installation_on_Virtual_Machine_qrdkdg.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893241/20_Windows_OS_Installation_on_Virtual_Machine_qrdkdg.png)

Click **Load driver**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893243/21_Windows_OS_Installation_on_Virtual_Machine_kurqpu.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893243/21_Windows_OS_Installation_on_Virtual_Machine_kurqpu.png)

Then, click **OK**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893246/22_Windows_OS_Installation_on_Virtual_Machine_hawe5x.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893246/22_Windows_OS_Installation_on_Virtual_Machine_hawe5x.png)

Select as `Red Hat VirtIO SCSI controller (E:\amd64\w10\viostor.inf)` and then click **Next**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893253/23_Windows_OS_Installation_on_Virtual_Machine_tu2fau.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893253/23_Windows_OS_Installation_on_Virtual_Machine_tu2fau.png)

Select as **Drive 0 Unallocated Space** and click **Next** option.

Windows will create the necessary partitions automatically. The installation will proceed and the VM will restart several times. Once Windows starts booting from the virtual disk, you may remove the Windows installation ISO.

💡Keep the `VirtIO ISO` attached until the required drivers have been installed.

### STEP-2 (Method-2): LOAD THE `VirtIO` STORAGE DRIVER (Recommended)

When you may reach as `Where do you want to install Windows?` and No drives were found.

This happens because Windows Setup does not have the required VirtIO storage driver loaded.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893241/20_Windows_OS_Installation_on_Virtual_Machine_qrdkdg.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893241/20_Windows_OS_Installation_on_Virtual_Machine_qrdkdg.png)

Click **Load driver**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792551593/2V_lbigef.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792551593/2V_lbigef.png)

* Then, click **Browse**
* Navigate to:

```plaintext
"CD Drive (E:) virtio-win-0.1.302" > viostor > w10 > amd64
```

* Then **Select** and click **OK**. Its select `viostor.inf` file automatically.
* Click `Red Hat VirtIO SCSI controller (E:\viostor\w10\amd64\viostor.inf)` and also click **Next**.

Windows will create the necessary partitions automatically.

The installation will proceed and the VM will restart several times.

Once Windows starts booting from the virtual disk, you may remove the Windows installation ISO.

Keep the `VirtIO ISO` attached until the required drivers have been installed.

### STEP-3: COMPLETE WINDOWS INITIAL SETUP

Complete the normal Windows setup:

* Keyboard layout
* Network
* User account
* Password/PIN
* Privacy settings

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893247/24_Windows_OS_Installation_on_Virtual_Machine_kuactk.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893247/24_Windows_OS_Installation_on_Virtual_Machine_kuactk.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893256/25_Windows_OS_Installation_on_Virtual_Machine_ufndyt.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893256/25_Windows_OS_Installation_on_Virtual_Machine_ufndyt.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893259/26_Windows_OS_Installation_on_Virtual_Machine_pk1lgf.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893259/26_Windows_OS_Installation_on_Virtual_Machine_pk1lgf.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893259/26_Windows_OS_Installation_on_Virtual_Machine_pk1lgf.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893259/26_Windows_OS_Installation_on_Virtual_Machine_pk1lgf.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893251/27_Windows_OS_Installation_on_Virtual_Machine_qkv5fa.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893251/27_Windows_OS_Installation_on_Virtual_Machine_qkv5fa.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893265/28_Windows_OS_Installation_on_Virtual_Machine_uw5uaz.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893265/28_Windows_OS_Installation_on_Virtual_Machine_uw5uaz.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893265/28_Windows_OS_Installation_on_Virtual_Machine_uw5uaz.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893265/28_Windows_OS_Installation_on_Virtual_Machine_uw5uaz.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893262/29_Windows_OS_Installation_on_Virtual_Machine_mevspw.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893262/29_Windows_OS_Installation_on_Virtual_Machine_mevspw.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893268/30_Windows_OS_Installation_on_Virtual_Machine_etxwgp.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789893268/30_Windows_OS_Installation_on_Virtual_Machine_etxwgp.png)

If you temporarily install without Internet access, you can connect the VirtIO network after installing the network driver.

# 🗁 STEP-4: Install `VirtIO` Guest Drivers

### STEP-1: NETWORK CONNECTION AND OTHERS PERIPHERALS

After reaching the Windows desktop, open:

```text
This PC
```

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554634/5V_oy3gcp.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554634/5V_oy3gcp.png)

Open the VirtIO CD-ROM.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554636/6V_tebyxa.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554636/6V_tebyxa.png)

You may find:

```text
virtio-win-gt-x64.msi
```

Install the appropriate package.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554636/7V_odaot4.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554636/7V_odaot4.png)

When these driver icon come to up, then select each for as `Entire feature will be installed on local hard drive` and click **Next**.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554635/9V_w4waqo.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792554635/9V_w4waqo.png)

When the pop-up box come to up, click **Yes**.

Restart Windows.

### STEP-2: INSTALL `SPICE Guest Agent` (DISPLAY/GRAPHICS DRIVER)

SPICE guest components are separate from the general VirtIO driver package.

The SPICE project provides Windows guest tools including SPICE-related guest components.

Download the appropriate [SPICE guest tools](https://www.spice-space.org/download/windows/spice-guest-tools/spice-guest-tools-latest.exe) from the official [SPICE](https://www.spice-space.org/download.html) download area and install them inside Windows.

> Unofficial [download link](http://u.pc.cd/F7N)

This can provide features such as:

* Better mouse integration
* Clipboard integration
* Guest-agent communication
* Improved desktop integration

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789903949/46_Install_spice-guest-tools_on_Windows_10_VM_OS_w1lbof.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789903949/46_Install_spice-guest-tools_on_Windows_10_VM_OS_w1lbof.png)

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789903953/47_Install_spice-guest-tools_on_Windows_10_VM_OS_egkkfd.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789903953/47_Install_spice-guest-tools_on_Windows_10_VM_OS_egkkfd.png)

Restart Windows after installation.

### STEP-3: VERIFY `VirtIO` DEVICES

Go to **Search box**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789897672/31_Windows_OS_Driver_Installation_on_Virtual_Machine_t66mm7.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789897672/31_Windows_OS_Driver_Installation_on_Virtual_Machine_t66mm7.png)

Open **Device Manager**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792556216/01_fgmkli.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792556216/01_fgmkli.png)

Check:

* Disk drives
* Storage controllers
* Network adapters
* Display adapters
* System devices

There should be no unexplained yellow warning icons (⚠️).

If you don't see the yellow warning icon, it means that all drivers are installed correctly and there are no driver warnings.

If you see one, right-click it:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789897674/32_Windows_OS_Driver_Installation_on_Virtual_Machine_lqyapq.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789897674/32_Windows_OS_Driver_Installation_on_Virtual_Machine_lqyapq.png)

Click **Update driver** and  then click **Browse my computer for drivers**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792557451/122_sxlwxy.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792557451/122_sxlwxy.png)

Select **VirtIO** drive or navigate to:

```text
"CD Drive (E:) virtio-win-0.1.302" > viostor > w10 > amd64
```

Click **OK**.

💡 This method is highly likely to solve this issue.

# 🖆 Run and Manage Windows-OS

### STEP-1: INSTALL `virt-viewer` ON UBUNTU

`virt-viewer` can connect to a virtual machine and display its graphical console in full-screen mode. Therefore, you can use it to view your Windows VM. This step is optional.

First, check whether `virt-viewer` is already installed.

Open the Ubuntu terminal and type:

```bash
virt-viewer
```

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789903957/49_Install_virt-viewer_on_Ubuntu_bq4zri.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789903957/49_Install_virt-viewer_on_Ubuntu_bq4zri.png)

If `virt-viewer` is not installed, install it using:

```bash
sudo apt install virt-viewer
```

### STEP-2 (Method-1): RUN WINDOWS 10/11 OS WITH KVM-QEMU

Suppose you are using Windows 10 as the guest OS.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789904613/53._Run_Windows_10_OS_with_KVM-QEMU_guk5ak.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789904613/53._Run_Windows_10_OS_with_KVM-QEMU_guk5ak.png)

Open a terminal on Ubuntu and type:

```bash
virt-manager
```

When the `Virtual Machine Manager` window opens, select `Win10` and click the **Play** button.

This will be shown as follows:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789904616/54._Run_Windows_10_OS_with_KVM-QEMU_zkuyev.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789904616/54._Run_Windows_10_OS_with_KVM-QEMU_zkuyev.png)

Now, open another terminal.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789904619/55._Run_Windows_10_OS_with_KVM-QEMU_isypll.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789904619/55._Run_Windows_10_OS_with_KVM-QEMU_isypll.png)

Type:

```bash
virt-viewer
```

When the `Choose a virtual machine` window opens, select `Win10` and click the **Connect** button.

When the **Allow inhibiting shortcuts** dialog box appears, click the **Allow** button.

Now, enjoy!

### STEP-2 (Method-2): RUN WINDOWS 10/11 OS WITH KVM-QEMU

#### START VIRTUAL MACHINE

Open terminal and type:

```bash
virsh start Windows-10
```

Here, replace `Windows-10` with the name you gave while creating the Virtual Machine.

Example:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885405/4_Create_new_virtual_machine_zum7sh.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789885405/4_Create_new_virtual_machine_zum7sh.png)

Here, you used name as `win10`. So, your terminal command have to:

```bash
virsh start win10
```

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792563281/1258_wt89i8.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792563281/1258_wt89i8.png)

**Check status**

```bash
virsh list --all
```

Open `Windows 10` with **Virt-Viewer**

```bash
virt-viewer
```

When open a pop-up box as **Choose a virtual machine**, select `win10` and click **Connect**

Now your **Windows 10 OS** will be run.

Now, enjoy!

#### SHUTDOWN VIRTUAL MACHINE

Shutdown your Windows OS

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1792569052/235_uyto7f.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1792569052/235_uyto7f.png)

Then open terminal and type:

```bash
virsh shutdown win10
```

If you have trouble shutting down Windows, you can use this command to <mark>force your virtual machine to shut down.</mark>

```bash
virsh destroy win10
```

`destroy` does not delete the VM. It immediately stops the guest, similar to physically powering off a computer.

Use graceful shutdown whenever possible.

# 🖆 Create the First Snapshot

Once Windows is completely configured, shut it down.

In virt-manager:

```text
Windows-10
→ Snapshots
→ Create
```

Name it:

```text
Clean-Windows-10-Installation
```

You can create another snapshot after installing your normal applications:

```text
Windows-10-Base-Apps
```

Snapshots are useful recovery points, but they should **not replace proper VM backups**.

# 🖆 USB Passthrough

USB devices can be passed from Ubuntu to Windows.

Examples include:

* USB flash drives
* Android phones
* USB printers
* USB adapters
* USB license dongles

### STEP-1: WHEN YOU WANT TO CONNECT USB DEVICES

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905739/57_USB_Device_Allow-Disallow_pacerc.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905739/57_USB_Device_Allow-Disallow_pacerc.png)

In **virt-manager**:

* Select **Virtual Machine**
* Select **Redirect USB device**

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905743/58_USB_Device_Allow-Disallow_fuuf1a.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905743/58_USB_Device_Allow-Disallow_fuuf1a.png)

Select the desired USB device and click.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905747/59_USB_Device_Allow-Disallow_qohbq5.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905747/59_USB_Device_Allow-Disallow_qohbq5.png)

### STEP-2: WHEN YOU WANT TO DISCONNECT / EJECT USB DEVICES

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905753/61_USB_Device_Allow-Disallow_pr2etl.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789905753/61_USB_Device_Allow-Disallow_pr2etl.png)

**Unchecked** your USB-Device and close the windows.

### ၊၊||၊ YOU CAN ALSO INSPECT USB DEVICES FROM UBUNTU

```bash
lsusb
```

Example:

```text
Bus 001 Device 003: ID 046d:c077 Logitech Mouse
```

# 🖆 File Sharing Between Ubuntu and Windows

There are two different directions.

### STEP-1: Ubuntu → Windows

If Ubuntu contains the files you want Windows to access, use Samba or another network-sharing mechanism on Ubuntu.

> 💡 Note: If you want to learn how to install and configure SMB on Linux, see [this note](06-file-sharing-with-linux.md).

Windows can then access:

```text
\\192.168.122.1\share
```

depending on the host-side network configuration.

### STEP-2: Windows → Ubuntu

If Windows contains the files you want Ubuntu to access, share the folder from Windows using SMB.

```text
Windows shared folder ➜ [---SMB---] ➜ Ubuntu Files
```

#### 🗺️ In `Windows OS`

The configuration method is shown in the image below:

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898832/35_Share_specific_folder_publicly_mvlx86.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898832/35_Share_specific_folder_publicly_mvlx86.png)

**Folder Properties** > **Sharing** > **Advanced Sharing** > Check **Share this folder** > **Permissions** > Choose **Everyone** & set permissions > **Apply** > **OK**.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898834/36_Share_specific_folder_publicly_v1uavs.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898834/36_Share_specific_folder_publicly_v1uavs.png)

**Folder Properties** > **Security** > Select **Everyone** > Set permissions > **Apply** > **OK**.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898837/37_Share_specific_folder_publicly_husdzd.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898837/37_Share_specific_folder_publicly_husdzd.png)

Go to the search box and type `Manage advanced sharing settings`.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898843/38_Share_specific_folder_publicly_jm46z4.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898843/38_Share_specific_folder_publicly_jm46z4.png)

Expand `Private (current profile)`.

* Enable `Turn on network discovery`.
* Enable `Turn on file and printer sharing`.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898844/39_Share_specific_folder_publicly_qjb4eo.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898844/39_Share_specific_folder_publicly_qjb4eo.png)

Expand `Guest or Public`.

* Enable `Turn on network discovery`.
* Enable `Turn on file and printer sharing`.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898859/40_Share_specific_folder_publicly_dxgmb1.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898859/40_Share_specific_folder_publicly_dxgmb1.png)

Expand `All Networks`.

* Enable `Turn on sharing so anyone with network access can read and write files in the Public folders`.
* Enable `Use 128-bit encryption to help protect file sharing connections (recommended)`.

For normal use, **do not disable password-protected sharing**. Using authenticated SMB access is preferable.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898846/41_Share_specific_folder_publicly_zm3c9o.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898846/41_Share_specific_folder_publicly_zm3c9o.png)

Go to the search box, type `cmd`, and select **Command Prompt**.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898861/42_Share_specific_folder_publicly_ks6khp.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898861/42_Share_specific_folder_publicly_ks6khp.png)

Type the following command in the terminal:

```powershell
ipconfig
```

Then look for the **IPv4 Address**. This is the IP address of the Windows 10 VM. For example, suppose the IP address is `192.168.122.128`.

#### 🗺️ Access from `Ubuntu`

> 💡 Note: If you want to learn how to install and configure SMB on Linux, see [this note](06-file-sharing-with-linux.md).

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898866/43_Share_specific_folder_publicly_j2d7wq.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898866/43_Share_specific_folder_publicly_j2d7wq.png)

Open **Files** on Ubuntu and go to the **Network** section. Enter:

```
smb://192.168.122.128
```

Then press **Enter**.

* After pressing **Enter**, a new **Authentication Required** window will appear.
* Select the password field and enter your Windows login password.
* Click the **Connect** button.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898851/44_Share_specific_folder_publicly_m09wqu.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898851/44_Share_specific_folder_publicly_m09wqu.png)

Select your shared folder. For example, suppose the shared folder is named `Drivers`. Open the folder.

![https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898855/45_Share_specific_folder_publicly_qlthea.png](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898855/45_Share_specific_folder_publicly_qlthea.png)

Copy or place the file you want to share into this folder. For example:

```text
spice-guest-tools-latest.exe
```

You can then access this file from Ubuntu through the Windows SMB share.

# 🖆 Backup the Virtual Machine (VM)

First shut down Windows:

```bash
virsh shutdown Windows-10
```

Check:

```bash
virsh list --all
```

Export the VM configuration:

```bash
virsh dumpxml Windows-10 > ~/Windows-10.xml
```

Back up the disk:

```bash
sudo rsync -avh --progress \
/var/lib/libvirt/images/Windows-10.qcow2 \
~/Windows-10-backup.qcow2
```

Your backup set should contain:

```text
Windows-10.xml
Windows-10.qcow2
```

For a serious backup strategy, store the backup on a **different physical disk** from the original VM.

# 🖆 Useful `virsh` Commands

**List VMs**

```bash
virsh list --all
```

**Start VM**

```bash
virsh start Windows-10
```

**Shut down VM**

```bash
virsh shutdown Windows-10
```

**Force stop**

```bash
virsh destroy Windows-10
```

**VM information**

```bash
virsh dominfo Windows-10
```

**Export VM configuration**

```bash
virsh dumpxml Windows-10
```

**List networks**

```bash
virsh net-list --all
```

**Start default network**

```bash
virsh net-start default
```

**Enable network autostart**

```bash
virsh net-autostart default
```

**Enable VM autostart**

```bash
virsh autostart Windows-10
```

**Disable VM autostart**

```bash
virsh autostart --disable Windows-10
```

# 🖆 Common Problem

### ❏❏❏  `/dev/kvm` Doesn't Exist

Check:

```bash
ls -l /dev/kvm
```

Then:

```bash
lsmod | grep kvm
```

For Intel:

```bash
sudo modprobe kvm_intel
```

For AMD:

```bash
sudo modprobe kvm_amd
```

Then:

```bash
ls -l /dev/kvm
```

If it still doesn't exist, verify that virtualization is enabled in BIOS/UEFI.

### ❏❏❏ KVM Acceleration Is Unavailable

Run:

```bash
kvm-ok
```

If KVM cannot be used:

1. Reboot.
2. Enter BIOS/UEFI.
3. Enable Intel VT-x or AMD-V/SVM.
4. Boot Ubuntu.
5. Run:

```bash
kvm-ok
```

again.

### ❏❏❏ `default` Network Is Inactive

Check:

```bash
virsh net-list --all
```

If inactive:

```bash
sudo virsh net-start default
```

Then:

```bash
sudo virsh net-autostart default
```

### ❏❏❏ Windows Says "No Drives Found"

This normally means that the VirtIO storage driver has not been loaded.

Go to:

```text
Load driver
→ Browse
→ VirtIO CD
→ viostor / vioscsi
→ Windows version
→ amd64
```

Select the appropriate `.inf` file.

The virtual disk should then appear.

### ❏❏❏ Windows Has No Internet

In **virt-manager**, check:

```text
Network source:
Virtual network 'default'
```

and:

```text
Device model:
VirtIO
```

Then verify the VirtIO network driver inside Windows.

Run in windows terminal:

```powershell
ipconfig
```

If no adapter appears, open:

```text
Device Manager
→ Network adapters
```

and install the appropriate VirtIO driver.

### ❏❏❏ Display Is Slow

Verify:

```text
Display:
SPICE
```

and make sure the appropriate Windows display/guest components are installed.

Also verify that the video device configured in virt-manager has a compatible Windows driver.

### ❏❏❏ Windows Won't Boot After Changing Disk Bus

Suppose Windows was installed using:

```text
SATA
```

and you later change the disk to:

```text
VirtIO
```

Windows may fail to boot if the VirtIO storage driver wasn't installed beforehand.

The correct sequence is:

```text
Install VirtIO driver
        ↓
Verify driver
        ↓
Shut down Windows
        ↓
Change disk to VirtIO
        ↓
Boot Windows
```

Don't change storage controllers randomly after installation.

### ❏❏❏ VM Is Extremely Slow

Check:

```text
CPU:
host-passthrough

Disk:
VirtIO

Network:
VirtIO

Firmware:
UEFI

Display:
SPICE
```

Then check host resources:

```bash
free -h
```

and:

```bash
top
```

If Ubuntu is running out of RAM or CPU, reduce the VM's allocation.

# 🖆 Recommended Configuration

For a host with 16 GB RAM:

```text
┌──────────────────────────────────┐
│           Windows VM             │
├──────────────────────────────────┤
│ RAM          6 GB                │
│ CPU          4 vCPU              │
│ CPU Model    host-passthrough    │
│ Disk         80–100 GB           │
│ Disk Format  qcow2               │
│ Disk Bus     VirtIO              │
│ Network      VirtIO              │
│ Network      libvirt NAT         │
│ Firmware     UEFI                │
│ Display      SPICE               │
└──────────────────────────────────┘
```

For Windows 11, additionally configure:

```text
TPM            2.0
Secure Boot    Enabled/Supported
```

# ☰ Final Architecture

After configuration:

```text
                         INTERNET
                            │
                            ▼
                    Ubuntu 26.04 Host
                            │
                 ┌──────────┴──────────┐
                 │                     │
              libvirt                 KVM
                 │                     │
                 └──────────┬──────────┘
                            │
                           QEMU
                            │
                   ┌────────┴────────┐
                   │                 │
                VirtIO             SPICE
                   │                 │
                   ▼                 ▼
             Windows 10/11       VM Console
                   │
        ┌──────────┼───────────┐
        │          │           │
      Disk       Network      USB
     VirtIO      VirtIO     Passthrough
```

# ☰ Conclusion

KVM/QEMU is a powerful way to run Windows inside Ubuntu without dual-booting.
