# Install DaVinci Resolve 21.0.4 on Ubuntu

You have the official Linux installer:

```text
DaVinci_Resolve_21.0.4_Linux.run
```

For Ubuntu, the installation is a little different from installing a `.deb` or `.AppImage`. DaVinci Resolve's Linux installer is a self-contained `.run` installer. Blackmagic currently lists **Rocky Linux 8.6/CentOS 7.3** as the minimum supported Linux environment, so Ubuntu installation can require additional compatibility work. ([Blackmagic Design](https://www.blackmagicdesign.com/event/davinciresolvedownload?utm_source=chatgpt.com "DaVinci Resolve Download"))

Below is a clean approach I'd use on Ubuntu.

---

## 1. Check Your Ubuntu Version

First, open Terminal:

```bash
Ctrl + Alt + T
```

Run:

```bash
lsb_release -a
```

or:

```bash
cat /etc/os-release
```

You should get something similar to:

```text
Distributor ID: Ubuntu
Description: Ubuntu 26.04 LTS
Release: 26.04
Codename: ...
```

---

# 2. Check Your CPU Architecture

DaVinci Resolve's Linux build is for **x86_64** systems.

Run:

```bash
uname -m
```

You want:

```text
x86_64
```

If you get:

```text
aarch64
```

the standard Linux `.run` installer isn't the appropriate build.

---

# 3. Update Ubuntu

Run:

```bash
sudo apt update
```

Then:

```bash
sudo apt upgrade
```

If Ubuntu asks whether to continue:

```text
Y
```

---

# 4. Install Basic Dependencies

Install the common libraries Resolve needs:

```bash
sudo apt install -y \
libapr1 \
libaprutil1 \
libasound2t64 \
libglib2.0-0t64 \
libglu1-mesa \
libgdk-pixbuf-2.0-0 \
libxcb-composite0 \
libxcb-cursor0 \
libxcb-damage0 \
libxcb-xinerama0 \
libxkbcommon-x11-0 \
libxss1
```

Then:

```bash
sudo apt --fix-broken install
```

And:

```bash
sudo apt autoremove
```

---

# 5. Go to Your Downloads Folder

Assuming your installer is in `Downloads`:

```bash
cd ~/Downloads
```

Check:

```bash
ls -lh
```

You should see:

```text
DaVinci_Resolve_21.0.4_Linux.run
```

---

# 6. Verify the Installer

Before executing it, check what type of file it is:

```bash
file DaVinci_Resolve_21.0.4_Linux.run
```

You should get something indicating that it is an executable/shell script or installer.

You can also check the file size:

```bash
ls -lh DaVinci_Resolve_21.0.4_Linux.run
```

A genuine Resolve installer is a large file, so an unexpectedly tiny file is a reason to re-download it.

---

# 7. Make the Installer Executable

Run:

```bash
chmod +x DaVinci_Resolve_21.0.4_Linux.run
```

Verify:

```bash
ls -l DaVinci_Resolve_21.0.4_Linux.run
```

You should see something similar to:

```text
-rwxr-xr-x
```

The `x` means the file is executable.

---

# 8. Run the Installer

Now run:

```bash
./DaVinci_Resolve_21.0.4_Linux.run
```

If the installer requires root privileges, use:

```bash
sudo ./DaVinci_Resolve_21.0.4_Linux.run
```

I would **try without `sudo` first**. If the installer itself requests administrative privileges, follow its prompt.

You should then get the DaVinci Resolve installation interface.

---

# 9. Install DaVinci Resolve

In the installer:

**Continue → Agree → Install**

The application will normally be installed under:

```text
/opt/resolve/
```

After installation, check:

```bash
ls /opt/resolve
```

You should see directories such as:

```text
bin
libs
plugins
scripts
```

---

# 10. Launch DaVinci Resolve

Try:

```bash
/opt/resolve/bin/resolve
```

If Resolve opens successfully, you're done.

You can also search your Ubuntu application menu for:

> **DaVinci Resolve**

---

# 11. Create a `resolve` Terminal Command

Instead of typing:

```bash
/opt/resolve/bin/resolve
```

every time, create a symbolic link:

```bash
sudo ln -s /opt/resolve/bin/resolve /usr/local/bin/resolve
```

Now you can simply run:

```bash
resolve
```

Much nicer.

---

# 12. Check Your GPU

This is **very important** for DaVinci Resolve.

Run:

```bash
lspci | grep -Ei 'vga|3d|display'
```

For example:

```text
NVIDIA Corporation ...
```

or:

```text
AMD/ATI ...
```

or:

```text
Intel Corporation ...
```

---

## NVIDIA GPU

Run:

```bash
nvidia-smi
```

If it works, you'll see something similar to:

```text
+-----------------------------------------------------------------------------+
| NVIDIA-SMI ...                                                             |
| GPU Name ...                                                               |
+-----------------------------------------------------------------------------+
```

If you get:

```text
command not found
```

or:

```text
NVIDIA-SMI has failed
```

your NVIDIA driver needs attention.

For Ubuntu, check available drivers:

```bash
ubuntu-drivers devices
```

Then:

```bash
ubuntu-drivers list
```

Ubuntu can recommend an appropriate driver.

---

# 13. Check OpenCL/CUDA Support

Resolve depends heavily on GPU acceleration.

For NVIDIA:

```bash
nvidia-smi
```

For AMD/Intel, you can inspect your OpenCL installation:

```bash
sudo apt install clinfo
```

Then:

```bash
clinfo
```

If OpenCL is working, you'll see information about your platform and GPU.

---

# 14. If DaVinci Resolve Doesn't Start

This is one of the most useful troubleshooting techniques.

**Don't launch it from the application menu initially.**

Run:

```bash
/opt/resolve/bin/resolve
```

Look carefully at the terminal output.

For example, you might see:

```text
error while loading shared libraries
```

or:

```text
OpenCL
```

or:

```text
CUDA
```

or:

```text
lib...
```

The exact error tells us what needs fixing.

---

# 15. If You Get a Missing Library Error

For example:

```text
error while loading shared libraries: libSomething.so
```

Find which package provides it.

First:

```bash
apt search libSomething
```

You can also use:

```bash
apt-file search libSomething.so
```

If `apt-file` isn't installed:

```bash
sudo apt install apt-file
```

Then:

```bash
sudo apt-file update
```

This is much better than randomly installing packages from internet tutorials.

---

# 16. If the Installer Says "Missing Dependencies"

This is a common issue when installing Blackmagic's Linux installer on Ubuntu because Blackmagic's officially listed Linux target is Rocky Linux/CentOS rather than Ubuntu. ([Blackmagic Design](https://www.blackmagicdesign.com/event/davinciresolvedownload?utm_source=chatgpt.com "DaVinci Resolve Download"))

You may see:

```text
Missing dependencies
```

or:

```text
The following packages are missing
```

**Don't immediately bypass the dependency check.**

Instead, copy the complete error and run:

```bash
apt policy <package-name>
```

For example:

```bash
apt policy libapr1
```

This lets us determine whether the package exists under a different Ubuntu package/version.

---

# 17. Don't Use `SKIP_PACKAGE_CHECK` Immediately

You may find Ubuntu tutorials telling you to run:

```bash
SKIP_PACKAGE_CHECK=1 ./DaVinci_Resolve_21.0.4_Linux.run
```

This bypasses Resolve's package compatibility check.

It can be useful for certain Ubuntu installations, but **don't use it as the first step**.

First determine what the installer is complaining about.

If all required libraries are actually available and the installer is merely rejecting Ubuntu's package names, then bypassing the check can make sense.

---

# 18. Create a Desktop Launcher

If Resolve doesn't automatically create an application entry, create one:

```bash
nano ~/.local/share/applications/davinci-resolve.desktop
```

Add:

```ini
[Desktop Entry]
Name=DaVinci Resolve
Comment=Professional Video Editing and Color Grading
Exec=/opt/resolve/bin/resolve
Icon=/opt/resolve/graphics/DV_Resolve.png
Terminal=false
Type=Application
Categories=AudioVideo;Video;Graphics;
StartupWMClass=resolve
```

Save:

```text
Ctrl + O
Enter
Ctrl + X
```

Then:

```bash
chmod +x ~/.local/share/applications/davinci-resolve.desktop
```

Now search Ubuntu's application launcher for:

```text
DaVinci Resolve
```

---

# 19. Launching Resolve from Terminal

You now have two options.

### Full path

```bash
/opt/resolve/bin/resolve
```

### Short command

```bash
resolve
```

I recommend the second one after creating the symlink.

---

# 20. Where DaVinci Resolve Is Installed

Unlike an AppImage:

```text
MyApp.AppImage
```

Resolve isn't simply sitting in your Downloads directory.

After installation, you'll generally have:

```text
/opt/resolve/
```

You can inspect it:

```bash
du -sh /opt/resolve
```

and:

```bash
ls -lah /opt/resolve
```

---

# 21. How to Uninstall DaVinci Resolve

First inspect the installation:

```bash
ls -lah /opt/resolve
```

Look for an uninstall mechanism.

Depending on the installer version, you may have an uninstall script/application.

You can also check:

```bash
find /opt/resolve -maxdepth 2 -iname '*uninstall*' -print
```

If an official uninstaller exists, **use it instead of manually deleting `/opt/resolve`**.

After uninstalling, if you created the `resolve` symlink:

```bash
sudo rm /usr/local/bin/resolve
```

And if you created your custom launcher:

```bash
rm ~/.local/share/applications/davinci-resolve.desktop
```

---

# 22. Don't Delete `/opt/resolve` First

Avoid doing this:

```bash
sudo rm -rf /opt/resolve
```

as your first uninstall method.

It may remove the application files, but it doesn't necessarily clean up everything created by the installer.

Use the application's provided uninstaller when available.

---

# 23. Verify the Installation

After installation, run:

```bash
ls -ld /opt/resolve
```

Then:

```bash
/opt/resolve/bin/resolve
```

If it launches, check inside Resolve:

**DaVinci Resolve → About DaVinci Resolve**

You should see:

```text
DaVinci Resolve 21.0.4
```

---

# 24. Recommended Complete Command Sequence

If your Ubuntu system is already up to date and your GPU driver is working, the basic installation is:

```bash
cd ~/Downloads
```

```bash
sudo apt update
```

```bash
sudo apt install -y \
libapr1 \
libaprutil1 \
libasound2t64 \
libglib2.0-0t64 \
libglu1-mesa \
libgdk-pixbuf-2.0-0 \
libxcb-composite0 \
libxcb-cursor0 \
libxcb-damage0 \
libxcb-xinerama0 \
libxkbcommon-x11-0 \
libxss1
```

Then:

```bash
chmod +x DaVinci_Resolve_21.0.4_Linux.run
```

Then:

```bash
./DaVinci_Resolve_21.0.4_Linux.run
```

After installation:

```bash
/opt/resolve/bin/resolve
```

And optionally:

```bash
sudo ln -s /opt/resolve/bin/resolve /usr/local/bin/resolve
```

Then:

```bash
resolve
```

---

# 25. One More Important Thing: Free vs Studio

Your filename is:

```text
DaVinci_Resolve_21.0.4_Linux.run
```

That is the **free DaVinci Resolve** installer.

The Studio installer has a different name, along the lines of:

```text
DaVinci_Resolve_Studio_21.0.4_Linux.run
```

So if you want the free version, **your current file is the correct one**.

Blackmagic's official site provides separate Resolve and Studio products/downloads. ([Blackmagic Design](https://www.blackmagicdesign.com/event/davinciresolvedownload?utm_source=chatgpt.com "DaVinci Resolve Download"))

---

## Recommended setup for your Ubuntu machine

I'd structure it like this:

```text
Ubuntu
│
├── NVIDIA/AMD/Intel GPU driver
│
├── DaVinci Resolve
│   └── /opt/resolve/
│
├── Desktop launcher
│   └── ~/.local/share/applications/
│
└── Terminal command
    └── /usr/local/bin/resolve
```

And **don't treat the `.run` file like an AppImage**. Once Resolve is installed, the `.run` installer is just the installer; the actual application lives under `/opt/resolve/`.

### If you're on Ubuntu 26.04

Because you're specifically working with Ubuntu 26.04, I would **not blindly follow older Ubuntu 22.04/24.04 DaVinci tutorials**. Blackmagic's official Linux requirement is still Rocky Linux 8.6/CentOS 7.3, and Ubuntu compatibility can change with library versions. ([Blackmagic Design](https://www.blackmagicdesign.com/event/davinciresolvedownload?utm_source=chatgpt.com "DaVinci Resolve Download"))

If the installer gives you **any error**, run:

```bash
./DaVinci_Resolve_21.0.4_Linux.run
```

and paste the **complete terminal output here**. I can then give you the exact commands for that error rather than having you install a pile of unnecessary compatibility packages.
