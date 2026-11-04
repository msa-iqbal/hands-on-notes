# File Sharing with Linux (Samba File Sharing)

This guide explains how to share and transfer files between Ubuntu Linux and other operating systems, including Windows, macOS, and Android. It covers permanent network shares, two-way file access, troubleshooting, and secure alternatives.

For a developer's workstation, **Samba (SMB)** is recommended for setting up permanent shared folders over a local network.

**Understand the Network Setup**

![File Sharing Between Windows and Linux](https://res.cloudinary.com/du7tjwbyi/image/upload/v1791863977/File_Sharing_with_Windows_and_Linux_nao5cv.png)

The IP addresses shown in the example are for illustration only. Your computers may use different IP addresses.

- **Ubuntu → Windows:** Windows accesses a shared folder on Ubuntu.
- **Windows → Ubuntu:** Ubuntu accesses a shared folder on Windows.
- **Two-way file sharing:** Configure shared folders on both computers, or use a single shared folder with read/write permissions for both computers.

**Note:** An internet connection is not required if both computers can communicate over the same local network (LAN).

# 🗁 PART A — Ubuntu to Other Operating Systems

## STEP 1: Install Samba on Ubuntu

Open Terminal using `Ctrl + Alt + T`.

Update package information and install Samba:

```bash
sudo apt update
sudo apt install samba smbclient -y
```

Check that Samba is installed:

```bash
smbd --version
```

Enable and start the Samba service:

```bash
sudo systemctl enable --now smbd
```

Check the service status:

```bash
systemctl status smbd
```

You should see `active (running)`.

Check your Ubuntu username:

```bash
whoami
```

Find your Ubuntu IP address:

```bash
hostname -I
```

Example output:

```text
192.168.1.100
```

Use the IP address associated with your local network interface.

## STEP 2: Create a Shared Folder

Create a dedicated directory rather than sharing your entire home directory:

```bash
mkdir -p ~/Shared
```

Create a test file:

```bash
echo "Hello from Ubuntu" > ~/Shared/test.txt
```

Verify the directory contents:

```bash
ls -la ~/Shared
```

## STEP 3: Configure the Samba Share

Open the Samba configuration file:

```bash
sudo nano /etc/samba/smb.conf
```

Go to the bottom of the file and add the following configuration. Replace `YOUR_USERNAME` with the username returned by `whoami`.

```bash
[Shared]
    path = /home/YOUR_USERNAME/Shared
    browseable = yes
    read only = no
    guest ok = no
    valid users = YOUR_USERNAME
    create mask = 0644
    directory mask = 0755
```

**What these settings mean:**

| Setting          | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| `path`           | Specifies the directory being shared.                        |
| `browseable`     | Makes the share visible when browsing available shares.      |
| `read only = no` | Allows file writes, subject to Linux filesystem permissions. |
| `guest ok = no`  | Requires authenticated access.                               |
| `valid users`    | Restricts access to the specified Samba user.                |
| `create mask`    | Limits permissions assigned to newly created files.          |
| `directory mask` | Limits permissions assigned to newly created directories.    |

The `0644` file mask and `0755` directory mask are suitable for a typical single-user share. The owner can write, while other local users have read-only access.

**Save the file:** Press `Ctrl + O`, then `Enter`, and exit using `Ctrl + X`.

## STEP 4: Configure Samba Authentication

Create a Samba password for your existing Ubuntu user:

```bash
sudo smbpasswd -a "$(whoami)"
```

Enter and confirm a password when prompted. This is the password you will use when connecting from Windows.

Enable the Samba account:

```bash
sudo smbpasswd -e "$(whoami)"
```

Validate the Samba configuration:

```bash
sudo testparm
```

If the configuration is valid, restart Samba:

```bash
sudo systemctl restart smbd
```

Check the service status:

```bash
systemctl status smbd
```

You should see `active (running)`.

Test whether the shared folder is accessible locally:

```bash
smbclient //localhost/Shared -U "$(whoami)" -c ls
```

Enter your Samba password when prompted. You should see `test.txt` listed.

You can also list all available shares:

```bash
smbclient -L localhost -U "$(whoami)"
```

The `Shared` share should appear in the output.

## STEP 5: Configure the Ubuntu Firewall

Check whether UFW is active:

```bash
sudo ufw status
```

If UFW is active, allow Samba traffic only from your local subnet.

For example, if your LAN subnet is `192.168.1.0/24`, run:

```bash
sudo ufw allow from 192.168.1.0/24 to any app Samba
```

Replace `192.168.1.0/24` with your actual LAN subnet.

Check the available application profiles if needed:

```bash
sudo ufw app list
```

If the Samba application profile is unavailable, you can use an explicit LAN-restricted rule for SMB:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 445 proto tcp
```

Replace the subnet with your actual LAN subnet. This alternative allows TCP port 445 only; older NetBIOS-based SMB discovery may require additional configuration.

If UFW is inactive, you do not need to enable it solely for this tutorial.

**Security note:** Avoid exposing SMB ports 139 and 445 directly to the internet. Restrict access to your trusted local network.

## STEP 6: Access the Ubuntu Share from Windows

On your Windows computer:

1. Connect Windows and Ubuntu to the same local network.
2. Press `Win + R` to open the Run dialog.
3. Enter the following path, replacing the IP address with your actual Ubuntu IP:

    ```text
    \\192.168.1.100\Shared
    ```

4. Press `Enter`.
5. When prompted, enter your Ubuntu username and the Samba password created in Step 4.

If Windows requests a domain-qualified username, try:

```bash
YOUR_USERNAME
```

Use the Samba password, not necessarily your Ubuntu login password.

You should now see the `test.txt` file inside the shared folder.

Try creating a new file from Windows. Then check Ubuntu:

```bash
ls -la ~/Shared
```

If the new file appears, file sharing is working in both directions.

#### OPTIONAL: Map the Samba Share as a Windows Network Drive

Mapping the share makes it easier to access repeatedly.

1. Open File Explorer using `Win + E`.
2. Select **This PC**.
3. Choose **Map network drive** from the toolbar or menu.
4. Select a drive letter, such as `Z:`.
5. Enter the network path:

    ```text
    \\192.168.1.100\Shared
    ```

6. Enable **Reconnect at sign-in** if desired.
7. Click **Finish** and authenticate with your Samba credentials.

The shared folder should now appear as a network drive in File Explorer.

**Result:** Your Ubuntu `~/Shared` directory is accessible from Windows over the local network using authenticated Samba file sharing.

### ☰ TROUBLESHOOTING — Ubuntu to Windows

| Problem                                 | Solution                                                                                           |
| --------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Windows cannot access the shared folder | Verify the Ubuntu IP address, network connectivity, and Samba service status.                      |
| Access denied                           | Check the Samba account, share configuration, and Linux filesystem permissions.                    |
| Network path not found                  | Verify the network path, restart Samba with `sudo systemctl restart smbd`, and check the firewall. |
| Password rejected                       | Reset the Samba password using `sudo smbpasswd "$(whoami)"`.                                       |
| Share is not listed                     | Run `sudo testparm` and inspect `/etc/samba/smb.conf`.                                             |
| Windows keeps using the wrong password  | Remove or update the saved credentials for the Ubuntu host in Windows Credential Manager.          |
| Files cannot be created or modified     | Check Linux directory permissions and confirm `read only = no` is configured.                      |

# 🗁 PART B — Windows to Ubuntu

If the files you want to access are stored on Windows, share the folder from Windows using SMB (Server Message Block). Ubuntu can then access the shared folder over your local network.

```text
Windows Shared Folder ── SMB ──> Ubuntu Files
```

## STEP 1: Share a Folder on Windows

Locate the folder you want to share. For this example, assume the folder is named `Drivers`.

Right-click the folder and select **Properties**.

Open **Sharing** > **Advanced Sharing**.

![View the Windows Advanced Sharing screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898832/35_Share_specific_folder_publicly_mvlx86.png)

1. Check **Share this folder**.
2. Set the share name, for example, `Drivers`.
3. Click **Permissions**.
4. Select `Everyone` if you want to grant access to all users who can reach the share.
5. Configure the required permissions:

    - **Read** — View and copy files.
    - **Change** — Read, create, modify, and delete files.
    - **Full Control** — Includes changing permissions and taking ownership where permitted.

6. Click **Apply** > **OK**.

**Important:** Grant only the permissions you need. `Everyone` does not automatically bypass Windows authentication, and it does not grant filesystem permissions by itself.

## STEP 2: Configure Windows Folder Security Permissions

In the same folder's **Properties** window, open the **Security** tab.

![View the Windows share permissions screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898834/36_Share_specific_folder_publicly_v1uavs.png)

Windows requires both share permissions and NTFS security permissions to allow an operation. The effective access is determined by both.

- For read-only access, grant the appropriate read permissions.
- For copying files into the folder or modifying its contents, grant the required write or modify permissions.

You can use a dedicated Windows account instead of granting access to `Everyone`. If you choose `Everyone`, verify that its permissions are appropriate for your intended access.

Click **Apply** > **OK** when finished.

## STEP 3: Enable Network Discovery and File Sharing

Open Windows Search and type:

```text
Manage advanced sharing settings
```

![View the Windows Security permissions screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898837/37_Share_specific_folder_publicly_husdzd.png)

Open the corresponding settings page.

![View the Windows advanced sharing settings screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898843/38_Share_specific_folder_publicly_jm46z4.png)

Expand **Private networks** (or `Private (current profile)` on some Windows versions).

Enable:

- **Turn on network discovery**
- **Turn on file and printer sharing**

Use the Private network profile for your trusted home or lab network. Avoid enabling discovery on a public network.

![View the Private network settings screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898844/39_Share_specific_folder_publicly_qjb4eo.png)

You generally do not need to enable network discovery or file sharing under the **Guest or Public** profile.

## STEP 4: Configure All Networks

Expand **All networks**.

![View the All Networks settings screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898859/40_Share_specific_folder_publicly_dxgmb1.png)

For normal authenticated file sharing:

- Keep **password-protected sharing** enabled.
- Leave the Public folder sharing option disabled unless you specifically intend to share files through Windows' Public folders.
- Keep **Use 128-bit encryption to help protect file sharing connections (recommended)** enabled.

These settings allow you to share a specific folder without making it an unrestricted guest-access share.

## STEP 5: Find the Windows IP Address

Open Windows Search, type `cmd`, and launch **Command Prompt**.

![View the Windows password-protected sharing settings screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898846/41_Share_specific_folder_publicly_zm3c9o.png)

Run:

```
ipconfig
```

Find the **IPv4 Address** under the network adapter used by your Ubuntu machine's network.

![View the Windows Command Prompt screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898861/42_Share_specific_folder_publicly_ks6khp.png)

For example:

```
IPv4 Address. . . . . . . . . . . : 192.168.122.128
```

In this example, `192.168.122.128` is the Windows machine's IP address. Your actual address may differ.

If Windows is running in a virtual machine, ensure the Ubuntu machine can reach the VM's network.

## STEP 6: Access the Windows Share from Ubuntu

On Ubuntu, open **Files** and select **Other Locations** or the Network section, depending on your Ubuntu version.

![View the Ubuntu network browsing screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898866/43_Share_specific_folder_publicly_j2d7wq.png)

In the server address field, enter:

```
smb://192.168.122.128
```

Replace the example IP address with your Windows IP address, then press `Enter`.

You can also connect directly to the named share:

```
smb://192.168.122.128/Drivers
```

![View the Ubuntu SMB authentication screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898851/44_Share_specific_folder_publicly_m09wqu.png)

## STEP 7: Authenticate with Windows Credentials

An authentication dialog may appear.

Select the appropriate authentication option and enter the credentials for a Windows account that has permission to access the shared folder.

Typically:

- **Username:** Your Windows account username.
- **Password:** The password for that Windows account.
- **Domain:** Your Windows computer name or domain, if requested.

If your Windows account is linked to a Microsoft account, the required username format may differ. Use credentials accepted by Windows SMB authentication.

Click **Connect**.

## STEP 8: Open the Shared Folder and Copy Files

Once authenticated, select the shared folder, such as `Drivers`.

![View the Ubuntu shared folder screenshot](https://res.cloudinary.com/du7tjwbyi/image/upload/v1789898855/45_Share_specific_folder_publicly_qlthea.png)

You should see the files and folders that your Windows account is permitted to access.

For example, you may want to access:

```
spice-guest-tools-latest.exe
```

You can copy the file from the Windows share to a local Ubuntu directory using Files.

To copy files **from Ubuntu to Windows**, the Windows share must grant write access through both its share permissions and NTFS security permissions.

**Result:** Ubuntu can access files shared from Windows over SMB, allowing you to copy files between the two systems according to the permissions configured on Windows.

## ☰ TROUBLESHOOTING — Windows to Ubuntu

| Problem                                       | Solution                                                                            |
| --------------------------------------------- | ----------------------------------------------------------------------------------- |
| Ubuntu cannot connect to Windows              | Verify the Windows IP address, network connectivity, and Windows Firewall settings. |
| Authentication fails                          | Check the Windows username, password, and account permissions.                      |
| Shared folder is not visible                  | Verify the share name and Windows Advanced Sharing settings.                        |
| Files can be read but not copied to the share | Check both SMB share permissions and NTFS security permissions.                     |
| Windows VM is unreachable                     | Check the virtual machine's network mode and routing between Ubuntu and Windows.    |

# 🗁 PART C — Ubuntu to Android

### STEP-1: Install a file manager

Install [CX File Explorer](https://play.google.com/store/apps/details?id=com.cxinventor.file.explorer) or another Android file manager that supports SMB/LAN connections.

### STEP-2: Add your Ubuntu computer

- Connect your phone to the same Wi-Fi as Ubuntu.
- Open the file manager and find Network, LAN, or SMB.
- Add a new server or network location.
- Enter your Ubuntu IP address, e.g. `192.168.1.105`.

### STEP-3: Log in

Enter your Ubuntu username and the Samba password you created in Step 4. Open the `Shared` folder.
### ☰ TROUBLESHOOTING — Ubuntu to Android

|Problem|Solution|
|---|---|
|Cannot connect|Run `sudo systemctl status smbd`|
|Authentication failed|Run `sudo smbpasswd YOUR_USERNAME` to reset the Samba password|
|Permission denied|Check folder permissions and ownership|
|Ubuntu not discovered|Enter its IP address manually|
|Firewall blocks connection|Run `sudo ufw allow Samba` on a trusted network|

Security tip: Keep guest access disabled and only allow Samba through your trusted local network. Avoid exposing SMB port 445 directly to the internet.
