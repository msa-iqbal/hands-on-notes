# Dropbox: Remove Dropbox from Windows 10 Explorer and Taskbar

**Updated**: 2026-08-11

This guide explains how to:

- Remove Dropbox from the **right-click (context) menu**
- Remove the **Dropbox folder** from File Explorer's navigation pane
- Hide the **Dropbox system tray icon**
- Keep Dropbox installed while removing unwanted Windows integrations

> **Before You Begin**
>
> It is highly recommended to create a **System Restore Point** and **export any registry keys** before making changes. This allows you to restore the original settings if needed.

# Disable Dropbox Context Menu

## Method 1: Remove Dropbox Shell Extension via Registry

### Step 1: Open Registry Editor

Press **Win + R**, type:

```text
regedit
```

### Step 2: Check the following registry locations

```text
HKEY_CLASSES_ROOT\AllFilesystemObjects\shellex\ContextMenuHandlers
```

```text
HKEY_CLASSES_ROOT\Directory\shellex\ContextMenuHandlers
```

```text
HKEY_CLASSES_ROOT\*\shellex\ContextMenuHandlers
```

Look for keys similar to:

```text
Dropbox
DropboxExt
```

### Step 3: Backup the registry key

- Right-click the Dropbox-related key.
- Select **Export**.
- Save the `.reg` file somewhere safe.

### Step 4: Delete the key

After creating the backup, delete the Dropbox-related registry key.

### Step 5: Check approved shell extensions

Navigate to:

```text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Shell Extensions\Approved
```

If Dropbox entries exist, you may remove them as well after exporting a backup.

### Step 6: Restart Windows Explorer

Open **Command Prompt** and run:

```cmd
taskkill /f /im explorer.exe
start explorer.exe
```

> **Note**
>
> Recent Dropbox versions may recreate these registry entries after an update.

## Method 2: Remove Dropbox Explorer Extension DLLs (Recommended)

This method prevents Dropbox from loading its Explorer shell extension.

### Step 1: Open the Dropbox installation folder

Navigate to:

```text
C:\Program Files (x86)\Dropbox\Client
```

Delete files similar to:

```text
DropboxExt.96.0.dll
DropboxExt64.96.0.dll
```

> The version number may differ depending on your Dropbox release.

### Step 2: Remove duplicate DLLs

Navigate to:

```text
C:\Program Files (x86)\Dropbox\Client\PackageAssets
```

Delete the following files if present:

```text
DropboxExt.96.0.dll
DropboxExt64.96.0.dll
```

Restart Windows Explorer or reboot your computer.

> **Note**
>
> Dropbox updates may restore these files automatically.

---

# Remove Dropbox from File Explorer Navigation Pane

### Step 1: Open Registry Editor

Press **Win + R**, type:

```text
regedit
```

### Step 2: Navigate to

```text
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace
```

You may find a Dropbox namespace key similar to:

```text
{E31EA727-12ED-4702-820C-4B6445F28E1A}
```

or another Dropbox CLSID, such as:

```text
{B135C697-4943-4362-99B0-C779E08012C3}
```

### Step 3: Backup the key

Right-click the Dropbox-related key and choose **Export**.

### Step 4: Delete the key

Delete the Dropbox namespace key.

### Step 5: Restart Windows Explorer

```cmd
taskkill /f /im explorer.exe
start explorer.exe
```

Dropbox will no longer appear in the File Explorer navigation pane.

---

# Hide the Dropbox System Tray Icon

If you want to prevent Dropbox from displaying its custom tray icon, you can remove the icon resources.

Navigate to:

```text
C:\Program Files (x86)\Dropbox\Client\<version>\images\
```

For example:

```text
C:\Program Files (x86)\Dropbox\Client\262.4.3183\images\
```

Delete the following folder:

```text
03_Tray_Icon
```

Restart Dropbox or restart Windows.

> **Note**
>
> Dropbox updates may restore the folder automatically.

---

# Restore the Changes

If you exported the registry keys before deleting them:

1. Double-click the exported `.reg` file.
2. Confirm the import.
3. Restart Windows Explorer.

If you deleted DLL files or folders, restore them from your backup or reinstall Dropbox.

---

# Notes

- These methods have been tested on **Windows 10**.
- Dropbox updates may restore Explorer integration, requiring you to repeat these steps.
- Removing the Explorer shell extension does **not** affect Dropbox synchronization.
- Removing the navigation pane entry does **not** delete your Dropbox files.
- Keeping backups of deleted registry keys and files is strongly recommended.
