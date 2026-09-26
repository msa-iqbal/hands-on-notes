# Android Screen Mirroring — `scrcpy`

**scrcpy** allows Android devices to be displayed and controlled from Linux through USB or Wi-Fi.

# Install

```bash
sudo apt update
sudo apt install adb scrcpy
```

Verify:

```bash
adb version
scrcpy --version
```

---

## Method-1: USB Connection

### 1. Enable Developer Options

On Android:

```text
Settings → About Phone → Build Number
```

Tap **Build Number** seven times.

### 2. Enable USB Debugging

**Settings → Developer Options → USB Debugging**

### 3. Connect Device

Connect the phone using USB.

Check:

```bash
adb devices
```

Then:

```bash
scrcpy
```

---

## Method-2: Wi-Fi Connection

Connect the phone via USB first.

### Enable TCP/IP Mode

```bash
adb tcpip 5555
```

Find the phone's IP address, then:

```bash
adb connect 192.168.1.100:5555
```

Verify:

```bash
adb devices
```

Start scrcpy:

```bash
scrcpy
```

Or:

```bash
scrcpy --tcpip=192.168.1.100
```

---

## Method-3: Android 11+ Wireless Debugging

Enable:

```text
Developer Options → Wireless Debugging
```

Pair:

```bash
adb pair IP:PAIRING_PORT
```

Example:

```bash
adb pair 192.168.1.100:36543
```

Enter the pairing code displayed on the phone.

Then connect:

```bash
adb connect 192.168.1.100:5555
```

Start:

```bash
scrcpy
```

---

# Useful scrcpy Commands

Fullscreen:

```bash
scrcpy --fullscreen
```

Turn off phone display:

```bash
scrcpy --turn-screen-off
```

Limit resolution:

```bash
scrcpy --max-size 1280
```

Disable audio:

```bash
scrcpy --no-audio
```

Keep device awake:

```bash
scrcpy --stay-awake
```

Read-only mode:

```bash
scrcpy --no-control
```

Record screen:

```bash
scrcpy --record screen.mp4
```

---

# `scrcpy` Troubleshooting

### Check Devices

```bash
adb devices -l
```

### Restart ADB

```bash
adb kill-server
adb start-server
```

### `unauthorized`

Reconnect the phone and accept the **USB debugging authorization** prompt.

### Wi-Fi Connection Failed

Check:

- Phone and PC are on the same network.
- IP address is correct.
- TCP/IP debugging is enabled.
- Port `5555` is accessible.

---

# Uninstall

```bash
sudo apt remove adb scrcpy
```

Then:

```bash
sudo apt autoremove
```
