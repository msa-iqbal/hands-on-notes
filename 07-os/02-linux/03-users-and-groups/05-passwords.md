# How to Change Your Ubuntu Password Using the Terminal

# 🗁 Change your current Ubuntu password

Open Terminal with `Ctrl + Alt + T`.

Check your username:

```bash
whoami
```

Change your password:

```bash
passwd
```

Enter your current password, then enter the new password twice.

Example prompts:

```text
Current password:
New password:
Retype new password:
passwd: password updated successfully
```

This method uses your current password and may reject a weak four-digit password.

# 🗁 Set an exactly four-digit numeric password

For example, you might want a password such as `5831`. Choose your own PIN rather than copying the example.

### STEP-1: Run the command

```bash
sudo passwd "$USER"
```

This changes the password for your current account. Enter your existing sudo password when requested.

### STEP-2: Enter your new password

```bash
New password:
Retype new password:
```

Type your chosen four digits both times. The terminal will not display the digits while you type.

If successful, you will see:

```text
passwd: password updated successfully
```

This is the first method to try. On a typical Ubuntu installation, an administrator-initiated password change can bypass some password-quality checks. However, stricter local policies may still reject it.

### STEP-3: If Ubuntu rejects the password

Check the Ubuntu version:

```bash
lsb_release -a
```

Inspect the password-quality configuration:

```bash
sudo grep -E '^[[:space:]]*(minlen|enforcing|enforce_for_root|dictcheck|usercheck)' \
  /etc/security/pwquality.conf
```

If the configuration file exists, you can edit it with:

```bash
sudo nano /etc/security/pwquality.conf
```

To permit a four-character password length under `pam_pwquality`, the relevant setting is:

```text
minlen = 4
```

Save in Nano with `Ctrl + O`, press `Enter`, then exit with `Ctrl + X`.

Important: This setting only adjusts the minimum-length rule. It does not disable every password-quality check, and other PAM settings may still reject a four-digit PIN. The `pam_unix` module can also apply additional checks.

If the password remains rejected, check the exact error before changing any further authentication settings.

# 🗁 Change another user's password

If you administer multiple Ubuntu accounts, first list the local usernames:

```bash
cut -d: -f1 /etc/passwd
```

Then reset a specific user's password:

```bash
sudo passwd username
```

Replace `username` with the actual account name. For example:

```bash
sudo passwd iqbal
```

Enter the new password twice. This does not require knowing the target user's current password, but you need administrator privileges.

# 🗁 Recover a forgotten password

If you are already logged in through another administrator account, use:

```bash
sudo passwd username
```

If you cannot log in to any administrator account, try Ubuntu's recovery mode:

1. Restart the computer.
2. Open the GRUB menu. Depending on the system, press `Esc` or hold `Shift` during startup.
3. Select Advanced options for Ubuntu.
4. Choose a kernel entry marked (recovery mode).
5. Select the root shell option.
6. Remount the root filesystem as writable:

   ```bash
   mount -o remount,rw /
   ```

7. Identify the account if necessary:

   ```bash
   ls /home
   ```

8. Reset the password:

   ```bash
   passwd username
   ```

9. Restart:

   ```bash
   reboot
   ```

The recovery procedure may differ on encrypted installations or systems with customized boot configurations.

# 🗁 Check password status

Check your account:

```bash
passwd -S
```

Check another account:

```bash
sudo passwd -S username
```

Common status indicators:

| Status | Meaning                  |     |
| ------ | ------------------------ | --- |
| `P`    | A usable password is set |     |
| `L`    | The password is locked   |     |
| `NP`   | No password is set       |     |

These commands show password status, not the password itself.
# 🗁 Lock or unlock a password

Lock an account password:

```bash
sudo passwd -l username
```

Unlock it:

```bash
sudo passwd -u username
```

Locking a password does not necessarily disable other authentication methods, such as SSH keys.
# 🗁 Test the new password

Once the password has been changed:

1. Save your work.
2. Press `Super + L` to lock the screen.
3. Enter your new four-digit password.
4. Confirm that Ubuntu unlocks successfully.

Keep your current session available until you have verified that the new password works.
