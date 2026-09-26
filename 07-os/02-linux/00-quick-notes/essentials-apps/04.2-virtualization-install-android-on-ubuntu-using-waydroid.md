# Waydroid

Waydroid runs Android applications inside Linux.

Reference Guide: <https://docs.waydro.id/usage/install-on-desktops>

### Install Dependencies

```bash
sudo apt install curl ca-certificates -y
```

### Add Repository

```bash
curl -s https://repo.waydro.id | sudo bash
```

### Install

```bash
sudo apt install waydroid -y
```

### Enable Waydroid Container

```bash
sudo systemctl enable --now waydroid-container
```

### Restart Waydroid

```bash
sudo systemctl restart waydroid-container
waydroid session stop
waydroid session start
```

### Set Resolution

Example — 1600×900:

```bash
waydroid prop set persist.waydroid.width 1600
waydroid prop set persist.waydroid.height 900
```

Full HD:

```bash
waydroid prop set persist.waydroid.width 1920
waydroid prop set persist.waydroid.height 1080
```
