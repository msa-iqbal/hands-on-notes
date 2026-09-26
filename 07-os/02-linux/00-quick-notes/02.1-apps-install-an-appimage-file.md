# How to Install an `.AppImage` File on Ubuntu

An **AppImage** is a portable Linux application format. Unlike `.deb` packages, you usually don't need to install it system-wide. You make the file executable and run it.

# Method 1 — Easiest Way: File Manager

### 1. Download the `.AppImage`

For example, suppose you downloaded:

```text
MyApplication.AppImage
```

Usually it will be in:

```text
~/Downloads
```

### 2. Make the AppImage executable

Open **Files** and go to your `Downloads` folder.

Right-click the `.AppImage` file → **Properties** → **Permissions**.

Enable:

> **Allow executing file as program**

Then close the Properties window.

### 3. Run the AppImage

Double-click the `.AppImage` file.

If Ubuntu asks what to do, select:

> **Run**

That's it. The application should launch.

---

# Method 2 — Install/Run from Terminal

This is my preferred method because it's simple and reliable.

### 1. Open Terminal

Press:

```text
Ctrl + Alt + T
```

### 2. Go to your Downloads folder

```bash
cd ~/Downloads
```

### 3. Check the AppImage filename

```bash
ls
```

For example:

```text
MyApplication.AppImage
```

### 4. Make it executable

```bash
chmod +x MyApplication.AppImage
```

### 5. Run it

```bash
./MyApplication.AppImage
```

The application should now start.

---

# Method 3 — Install the AppImage to a Permanent Location

An AppImage doesn't technically need to be "installed", but I recommend keeping applications somewhere other than `Downloads`.

For example, create an `Applications` directory:

```bash
mkdir -p ~/Applications
```

Move your AppImage:

```bash
mv ~/Downloads/MyApplication.AppImage ~/Applications/
```

Make it executable:

```bash
chmod +x ~/Applications/MyApplication.AppImage
```

Run it:

```bash
~/Applications/MyApplication.AppImage
```

You can now keep your AppImages organized like:

```text
~/Applications/
├── App1.AppImage
├── App2.AppImage
└── App3.AppImage
```

---

# Method 4 — Create an Application Menu Shortcut

If you want the AppImage to appear in Ubuntu's **Applications menu**, create a `.desktop` launcher.

For example:

```bash
nano ~/.local/share/applications/myapplication.desktop
```

Add:

```ini
[Desktop Entry]
Name=My Application
Exec=/home/YOUR_USERNAME/Applications/MyApplication.AppImage
Icon=application-x-executable
Type=Application
Categories=Utility;
Terminal=false
```

Replace:

```text
YOUR_USERNAME
```

with your actual Ubuntu username.

You can find your username with:

```bash
whoami
```

For example, if your username is `iqbal`:

```ini
Exec=/home/iqbal/Applications/MyApplication.AppImage
```

Save the file:

```text
Ctrl + O
Enter
Ctrl + X
```

Then make the launcher executable:

```bash
chmod +x ~/.local/share/applications/myapplication.desktop
```

The application should now appear in Ubuntu's application launcher.

---

# Better: Use the AppImage's Icon

Some AppImages contain their own icon, but Ubuntu doesn't automatically extract it for a `.desktop` launcher.

If you want a proper application icon, you can extract the AppImage:

```bash
./MyApplication.AppImage --appimage-extract
```

This creates:

```text
squashfs-root/
```

Look inside:

```bash
find squashfs-root -type f \( -iname "*.png" -o -iname "*.svg" \)
```

You can then use an appropriate icon in your `.desktop` file.

For example:

```ini
Icon=/home/iqbal/Applications/myapplication.png
```

---

# If You Get `Permission denied`

If you run:

```bash
./MyApplication.AppImage
```

and get:

```text
Permission denied
```

run:

```bash
chmod +x MyApplication.AppImage
```

Then:

```bash
./MyApplication.AppImage
```

---

# If You Get a FUSE Error

On some Ubuntu installations, you may see an error similar to:

```text
dlopen(): error loading libfuse.so.2
```

or:

```text
AppImages require FUSE to run
```

Install FUSE compatibility support:

```bash
sudo apt update
sudo apt install libfuse2t64
```

Then try again:

```bash
./MyApplication.AppImage
```

If your Ubuntu repository doesn't provide `libfuse2t64`, check the available package:

```bash
apt search libfuse2
```

and install the appropriate package for your Ubuntu release.

---

# How to Remove an AppImage

One advantage of AppImages is that uninstalling is extremely easy.

If you stored it in:

```text
~/Applications/
```

simply delete it:

```bash
rm ~/Applications/MyApplication.AppImage
```

If you also created a `.desktop` launcher, remove that:

```bash
rm ~/.local/share/applications/myapplication.desktop
```

That's essentially the entire uninstall process.

---

# Recommended AppImage Organization

For your Ubuntu setup, I'd recommend:

```text
Home
└── Applications
    ├── app-1.AppImage
    ├── app-2.AppImage
    ├── app-3.AppImage
    └── ...
```

Then:

```bash
mkdir -p ~/Applications
```

For every new AppImage:

```bash
mv ~/Downloads/Application.AppImage ~/Applications/
chmod +x ~/Applications/Application.AppImage
```

Run it:

```bash
~/Applications/Application.AppImage
```

---

# Quick Command — The Whole Process

If the AppImage is in `Downloads`, you can do everything in three commands:

```bash
mkdir -p ~/Applications
mv ~/Downloads/MyApplication.AppImage ~/Applications/
chmod +x ~/Applications/MyApplication.AppImage
```

Then launch:

```bash
~/Applications/MyApplication.AppImage
```

### My recommendation

For regular Ubuntu use, I'd use **`~/Applications/` + executable permission + a `.desktop` launcher**. It keeps AppImages separate from system packages while still making them behave like normal desktop applications.
