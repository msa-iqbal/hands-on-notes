# How to Change Your Ubuntu Password Using the Terminal

Changing your user password in Ubuntu is simple and can be done entirely from the Terminal. The standard `passwd` command allows you to change your password without opening any graphical settings.

This guide covers:

- Changing your current Ubuntu password
- Changing another user's password
- Setting a short 4-digit numeric password
- Understanding Ubuntu's password-quality policy
- Troubleshooting rejected passwords
- Checking password configuration
- Reverting password-policy changes
- Useful password-management commands

---

## 1. Change Your Current Ubuntu Password

The easiest way to change your password is with the `passwd` command.

Open Terminal and run:

```bash
passwd
```

Ubuntu will ask for your current password:

```text
Current password:
```

Enter your existing password.

Then enter your new password:

```text
New password:
```

Finally, enter it again:

```text
Retype new password:
```

If everything is successful, you will see:

```text
passwd: password updated successfully
```

### Example

```bash
passwd
```

```text
Current password:
New password:
Retype new password:
passwd: password updated successfully
```

> When entering a password in the Terminal, nothing appears on the screen—not even `*` characters. This is normal.

---

# 2. Change Another User's Password

If you have `sudo` privileges, you can change another user's password.

Use:

```bash
sudo passwd username
```

For example:

```bash
sudo passwd iqbal
```

You will be asked to enter your current `sudo` password and then specify the new password for that user.

You do not need to know the user's existing password when using `sudo passwd`.

---

# 3. Can Ubuntu Use a 4-Digit Password?

Ubuntu uses password-quality rules to prevent extremely short or weak passwords.

For example, a password such as:

```text
1234
```

may be rejected with a message similar to:

```text
BAD PASSWORD: The password is shorter than 8 characters
```

or:

```text
BAD PASSWORD: The password is too simple
```

The exact message depends on the password-quality configuration installed on your system.

A 4-digit numeric password has only:

```text
0000–9999
```

which means there are only **10,000 possible combinations**.

Therefore, a 4-digit number should generally be treated as a **PIN**, not as a strong Linux account password.

---

# 4. Check Ubuntu's Password Policy

Before changing the password policy, check whether `pam_pwquality` is being used.

Run:

```bash
grep -R "pam_pwquality" /etc/pam.d/
```

You may see something similar to:

```text
/etc/pam.d/common-password:password requisite pam_pwquality.so
```

You can also inspect the configuration:

```bash
sudo cat /etc/security/pwquality.conf
```

Look for settings such as:

```text
minlen = 8
```

The `minlen` option controls the minimum password length.

---

# 5. Allowing a 4-Digit Password

If you specifically want Ubuntu to accept a 4-digit numeric password, you need to modify the password-quality configuration.

First, create a backup:

```bash
sudo cp /etc/security/pwquality.conf \
/etc/security/pwquality.conf.backup
```

Then open the configuration:

```bash
sudo nano /etc/security/pwquality.conf
```

Look for:

```text
minlen = 8
```

and change it to:

```text
minlen = 4
```

Save the file:

```text
Ctrl + X
Y
Enter
```

Then try:

```bash
passwd
```

and enter your 4-digit password.

---

# 6. What If Ubuntu Still Rejects the 4-Digit Password?

Changing `minlen` does not necessarily mean that every 4-digit password will be accepted.

Password-quality rules can also check:

- Repeated characters

- Sequential numbers

- Dictionary words

- Character classes

- Password similarity

- Other complexity requirements

For example:

```text
1234
1111
0000
```

are extremely weak passwords and may still be rejected depending on the configured policy.

Check the complete configuration:

```bash
sudo cat /etc/security/pwquality.conf
```

Also inspect the PAM configuration:

```bash
grep -R "pam_pwquality" /etc/pam.d/
```

---

# 7. Test Your Password

After changing the password, you can test authentication with:

```bash
sudo -v
```

Enter the new password when prompted.

If authentication succeeds, your password is working correctly.

You can also open a new Terminal session and test:

```bash
su - username
```

For example:

```bash
su - iqbal
```

Enter the user's new password when prompted.

---

# 8. Reset the Password Policy

If you temporarily changed the password policy and want to restore the previous configuration, restore the backup:

```bash
sudo cp /etc/security/pwquality.conf.backup \
/etc/security/pwquality.conf
```

Then verify:

```bash
sudo cat /etc/security/pwquality.conf
```

---

# 9. Useful Password Commands

### Change Your Password

```bash
passwd
```

### Change Another User's Password

```bash
sudo passwd username
```

### Lock a User Account

```bash
sudo passwd -l username
```

### Unlock a User Account

```bash
sudo passwd -u username
```

### Check Password Status

```bash
sudo passwd -S username
```

Example:

```bash
sudo passwd -S iqbal
```

### Check Password Expiration Information

```bash
sudo chage -l username
```

Example:

```bash
sudo chage -l iqbal
```

---

# 10. Change the Root Password

Ubuntu normally keeps the root account locked and expects administrators to use `sudo`.

If you intentionally need to set a root password:

```bash
sudo passwd root
```

You can then switch to the root account with:

```bash
su -
```

However, for normal Ubuntu administration, using `sudo` is generally preferable.

---

# 11. Password vs PIN

It is useful to distinguish between a **password** and a **PIN**.

### Password

A strong password might look like:

```text
Linux@2026!Ubuntu
```

or, preferably, a longer unique passphrase.

### PIN

A PIN might look like:

```text
4827
```

A 4-digit PIN is convenient, but it has only 10,000 possible combinations.

If your objective is simply **quick local login**, investigate whether your desktop/login environment supports a PIN-like authentication mechanism rather than weakening your Linux account password.

---

# 12. Recommended Approach

For a normal Ubuntu workstation, I recommend keeping the Linux account password reasonably strong.

For example:

```text
MyUbuntuWorkstation!2026
```

Even better, use a unique password generated and stored by a password manager.

If you specifically want a short code because you dislike typing a long password during login, consider configuring the **login/authentication mechanism** separately instead of making the underlying Unix account password only four digits.

---

# 13. Quick Reference

|Task|Command|
|---|---|
|Change current password|`passwd`|
|Change another user's password|`sudo passwd username`|
|Change root password|`sudo passwd root`|
|Lock account|`sudo passwd -l username`|
|Unlock account|`sudo passwd -u username`|
|Check password status|`sudo passwd -S username`|
|Check expiration|`sudo chage -l username`|
|Check password policy|`sudo cat /etc/security/pwquality.conf`|
|Check PAM configuration|`grep -R "pam_pwquality" /etc/pam.d/`|

---

# Conclusion

Ubuntu makes password management straightforward from the command line. For most users, the main command you need is:

```bash
passwd
```

For administrators managing another account:

```bash
sudo passwd username
```

If Ubuntu rejects a short password, its configured password-quality policy is enforcing minimum requirements. The policy can be inspected through:

```bash
sudo cat /etc/security/pwquality.conf
```

Although it is technically possible to configure Ubuntu to accept a 4-digit password, a four-digit numeric value provides very limited protection. It is better suited to a **PIN-style authentication experience** than to the primary password protecting a Linux user account.
