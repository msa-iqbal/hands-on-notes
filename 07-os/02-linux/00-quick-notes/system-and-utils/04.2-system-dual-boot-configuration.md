# Dual-Boot Configuration

When Linux and Windows are installed on the same computer, **GRUB** allows you to choose which operating system starts by default.

## 1. Find the OS Menu Entry Number

Restart your computer and look at the **GRUB boot menu**.

Identify the operating system you want to start automatically and note its position in the menu.

![Dual-Boot Configuration](https://res.cloudinary.com/p2s6wtaa/image/upload/v1788516898/dual-boot-configuration-1.png)

> **Note:** GRUB menu entries are numbered starting from **`0`**, not `1`.

For example:

```text
0 → Ubuntu
1 → Advanced options for Ubuntu
2 → Windows Recovery
3 → Another OS
4 → Windows OS
```

If **Windows OS** is entry `4`, you can set it as the default boot option.

---

## 2. Edit the GRUB Configuration

Open the GRUB configuration file:

```bash
sudo nano /etc/default/grub
```

Find:

```text
GRUB_DEFAULT=0
```

Change it to:

```text
GRUB_DEFAULT=4
```

![GRUB_DEFAULT Configuration](https://res.cloudinary.com/p2s6wtaa/image/upload/v1788517850/system-dual-boot-configuration-2.png)

Save and exit **nano**:

```text
Ctrl + X
Y
Enter
```

---

## 3. Apply the GRUB Configuration

Run:

```bash
sudo update-grub
```

You should see GRUB detecting the installed operating systems, including Windows.

---

## 4. Restart the Computer

```bash
sudo systemctl reboot -i
```

After restarting, GRUB will automatically select the configured default entry.

> **Important:** The menu position can change after adding, removing, or updating operating systems. If the default entry stops pointing to the intended OS, check the GRUB menu again and update `GRUB_DEFAULT`.
