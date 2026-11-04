# Hide/Show Running-App Indicators (Dots) and Highlight Shadows in the Ubuntu Dock

## ၊၊||၊ Hide or show indicator dots

Hide the dots while keeping running applications visible:

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock custom-theme-customize-running-dots true
gsettings set org.gnome.shell.extensions.dash-to-dock custom-theme-running-dots-color '#00000000'
gsettings set org.gnome.shell.extensions.dash-to-dock custom-theme-running-dots-border-width 0
```

To restore the default indicator appearance:

```bash
gsettings reset org.gnome.shell.extensions.dash-to-dock custom-theme-running-dots-color
gsettings reset org.gnome.shell.extensions.dash-to-dock custom-theme-running-dots-border-width
gsettings reset org.gnome.shell.extensions.dash-to-dock custom-theme-customize-running-dots
```

## ၊၊||၊ Hide or show the running-app highlight shadow

Ubuntu Dock also has a setting for the backlight effect on running applications:

```bash
# Hide the running-app highlight/backlight
gsettings set org.gnome.shell.extensions.dash-to-dock unity-backlit-items false
```

Restore it:

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock unity-backlit-items true
```

This controls the backlight effect when supported by the active Dock theme.

## ၊၊||၊ Hide/Show running applications from the Dock

If you want to hide the running-app icons rather than just their indicators, run:

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock show-running false
```

If you want to show the running-app icons rather than just their indicators, run:

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock show-running true
```

## ၊၊||၊ Restore the default Dock settings

Open Terminal (`Ctrl + Alt + T`) and run:

```bash
dconf reset -f /org/gnome/shell/extensions/dash-to-dock/
```

This resets all settings under the Ubuntu Dock/Dash-to-Dock configuration path, including any custom appearance settings.

**Restart the Dock**

Log out of Ubuntu and log back in. This is the most reliable way to reload the desktop shell and apply the restored settings.
