# Hide the AppIndicator/System Tray icons

Ubuntu uses the **AppIndicator** GNOME extension for tray icons.

First check the extension:

```bash
gnome-extensions list | grep -i indicator
```

You may see something like:

```text
ubuntu-appindicators@ubuntu.com
```

Disable it:

```bash
gnome-extensions disable ubuntu-appindicators@ubuntu.com
```

Then restart GNOME Shell or log out/in.

To enable it again:

```bash
gnome-extensions enable ubuntu-appindicators@ubuntu.com
```

# If you want to hide only a specific tray icon

For example, if an application such as **Telegram, Discord, Dropbox, or another AppImage** puts an icon in the tray, disabling the entire AppIndicator extension is usually unnecessary.

Instead, check that application's settings for something like:

- **Show tray icon**
- **Minimize to tray**
- **Start minimized**
- **Show status icon**

Disable that option.
