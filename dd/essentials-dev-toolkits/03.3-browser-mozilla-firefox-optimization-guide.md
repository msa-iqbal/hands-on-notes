# BROWSER: Mozilla Firefox Optimization Guide

Best Performance, Security & Privacy Setup for Developers and Power Users

**Last Updated:** July 31, 2026

Firefox remains one of the best browsers for users who prioritize privacy, open standards, and customization. This guide focuses on optimizing Firefox for **security, privacy, performance, and web development** without breaking website compatibility.

# 🗁 UPDATE FIREFOX FIRST

Check your current version:

```text
about:support
```

Or:

```text
about:preferences#general
```

Firefox should update automatically.

### Ubuntu / Debian

```bash
sudo apt update
sudo apt upgrade firefox
```

# 🗁 SETTINGS: GENERAL

Navigate to:

```text
about:preferences#general
```

## Performance

Uncheck:

* ⛝ Use recommended performance settings

Enable:

* 🗹 Use hardware acceleration when available

Recommended Content Process Limit:

| RAM    | Processes |
| ------ | --------- |
| 8 GB   | 4–6       |
| 16 GB  | 8         |
| 32 GB+ | 8–12      |

## Startup

Recommended:

🗹 Open previous windows and tabs (Optional)

Firefox handles session restore efficiently, so this setting is generally safe.

# 🗁 SETTINGS: PRIVACY & SECURITY

Navigate to:

```text
about:preferences#privacy
```

## Enhanced Tracking Protection

### Recommended

* 🗹 Strict

Blocks:

* Cross-site trackers
* Tracking cookies
* Cryptominers
* Fingerprinters

Benefits:

* Better privacy
* Faster page loading
* Less tracking

## HTTPS-Only Mode

Enable:

* 🗹 Enable HTTPS-Only Mode in all windows

Benefits:

* Forces encrypted connections
* Better security on public Wi-Fi
* Prevents accidental HTTP connections

## Cookies & Site Data

Recommended:

* 🗹 Clear cookies and site data when Firefox is closed _(Optional)_

For most users:

* 🗹 Keep cookies
* 🗹 Use Strict Tracking Protection

This offers a better balance between privacy and convenience.

## Firefox Data Collection

Disable:

* ⛝ Send technical and interaction data to Mozilla
* ⛝ Allow Firefox to improve features, performance, and stability between updates
* ⛝ Send daily usage ping to Mozilla

# 🗁 SETTINGS: SEARCH

Navigate to:

```text
about:preferences#search
```

Recommended search engines:

### Privacy-Focused

* DuckDuckGo
* Brave Search

### Development & Research

* Google
* Brave Search

Recommended default:

* 🗹 Brave Search

or

* 🗹 DuckDuckGo

---

# 🗁 SETTINGS: SECURITY

Navigate to:

```text
about:preferences#privacy
```

### Passwords

Enable:

* 🗹 Alert about passwords for breached websites

If using Bitwarden:

Disable:

* ⛝ Ask to save passwords

## DNS over HTTPS

Enable:

* 🗹 Max Protection

Recommended providers:

| Provider   | DNS     |
| ---------- | ------- |
| Cloudflare | 1.1.1.1 |
| NextDNS    | Custom  |
| Quad9      | 9.9.9.9 |

Recommended:

```text
Cloudflare
```

Advanced users:

```text
NextDNS
```

# 🗁 SETTINGS: FIREFOX SYNC

Navigate to:

```text
about:preferences#sync
```

Recommended:

🗹 Bookmarks
🗹 History
🗹 Settings
🗹 Tabs

Optional:

◯ Extensions

Reason:

Extensions can introduce issues across multiple devices.

# 🗁 ADVANCED PRIVACY SETTINGS

Open:

```text
about:config
```

Accept the warning.

## WebRTC Leak Protection

Search:

```text
media.peerconnection.enabled
```

Set:

```text
false
```

Useful for:

* VPN users
* Privacy-focused users

> Warning: This may break video conferencing applications such as Google Meet, Zoom Web, and Microsoft Teams.

## Telemetry

Search and disable:

```text
toolkit.telemetry.enabled
datareporting.healthreport.uploadEnabled
browser.ping-centre.telemetry
```

Set all to:

```text
false
```

## Disable Pocket

Search:

```text
extensions.pocket.enabled
```

Set:

```text
false
```

## Prefetching

Search:

```text
network.prefetch-next
```

Set:

```text
false
```

Benefits:

* Better privacy
* Reduced unnecessary requests

# 🗁 PERFORMANCE TWEAKS

## Verify Hardware Acceleration

Open:

```text
about:support
```

Look for:

```text
Compositing
```

You want:

```text
WebRender
```

or

```text
Hardware Accelerated
```

## Linux Video Acceleration

### Intel GPUs

```bash
sudo apt install intel-media-va-driver-non-free
```

### AMD GPUs

```bash
sudo apt install mesa-va-drivers
```

Verify via:

```text
about:support
```

# 🗁 EXTENSION MANAGEMENT

Open:

```text
about:addons
```

Keep extensions minimal.

## Recommended Extensions

### Security & Privacy

* [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)
* [Bitwarden](https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager/)

### Development

* [React Developer Tools](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)
* [Wappalyzer](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/)
* [JSON Viewer Pro](https://addons.mozilla.org/en-US/firefox/addon/json-viewer-pro/)
* [Lighthouse](https://addons.mozilla.org/en-US/firefox/addon/google-lighthouse/)
* [ColorZilla](https://addons.mozilla.org/en-US/firefox/addon/colorzilla/)

### Optional Privacy

* [Firefox Multi-Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/)

Benefits:

* Separate logins
* Isolated cookies
* Better privacy

# 🗁 DEVELOPER TOOLS SETUP

Open:

```text
F12
```

Recommended Developer Tools:

* 🗹 Disable HTTP Cache (while toolbox is open)
* 🗹 Responsive Design Mode

# 🗁 PROFILE MANAGEMENT

Use separate Firefox profiles.

Open:

```text
about:profiles
```

Recommended:

### Personal

* Email
* Social Media
* Banking

### Work

* GitHub
* Company Accounts
* Development Tools

### Testing

* No Extensions
* Clean Profile
* Website Debugging

# 🗁 USEFUL KEYBOARD SHORTCUTS

| Action             | Shortcut         |
| ------------------ | ---------------- |
| New Tab            | Ctrl + T         |
| Close Tab          | Ctrl + W         |
| Restore Closed Tab | Ctrl + Shift + T |
| Private Window     | Ctrl + Shift + P |
| Search Tabs        | Ctrl + Shift + A |
| Developer Tools    | F12              |

# 🗁 RECOMMENDED CONFIGURATION SUMMARY

### Security

* 🗹 HTTPS-Only Mode
* 🗹 Password Breach Alerts
* 🗹 DNS over HTTPS

### Privacy

* 🗹 Strict Tracking Protection
* 🗹 Disable Telemetry
* 🗹 Disable Pocket
* 🗹 Disable Prefetching

### Performance

* 🗹 Hardware Acceleration
* 🗹 WebRender
* 🗹 Minimal Extensions

### Developer Essentials

* 🛠 React Developer Tools
* 🛠 Wappalyzer
* 🛠 Firefox Multi-Account Containers
* 🛠 Lighthouse
* 🔐 Bitwarden
* 🛡️ uBlock Origin

### Linux

* 🗹 Hardware Video Acceleration
* 🗹 WebRender Enabled

Firefox provides some of the strongest privacy protections available in a mainstream browser. With the settings above, you get an excellent balance of **privacy, security, performance, and developer productivity** while maintaining broad website compatibility.
