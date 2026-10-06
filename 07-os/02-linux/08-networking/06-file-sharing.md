# Network & File Sharing

## Access a Windows Shared Folder

Install Samba/CIFS support:

```bash
sudo apt install samba cifs-utils
```

Open the file manager and enter:

```bash
smb://WINDOWS-PC-IP
```

Example:

```bash
smb://192.168.1.102
```

Then provide the Windows username/password when prompted.

> Keep network credentials out of this documentation.
