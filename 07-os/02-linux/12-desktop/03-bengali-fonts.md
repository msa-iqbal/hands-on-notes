# How to Fix Broken Bengali Fonts in Google Chrome and Brave on Ubuntu

If Bengali text appears broken, distorted, or incorrectly rendered in **Google Chrome** or **Brave Browser** on Ubuntu, the problem is usually related to missing Bengali Unicode fonts, font configuration, or a corrupted font cache.

For example, Bengali text may appear like this:

> বাংলা ভাষা আমার মাতৃভাষা।

Instead of rendering correctly, characters might appear disconnected, replaced with boxes, or displayed with an inappropriate font.

This guide explains how to diagnose and fix Bengali font rendering problems on Ubuntu.

## Why Does Bengali Text Look Broken?

Bengali is a complex Unicode script that requires proper font and text-shaping support. Browser rendering depends on several components working together:

- Bengali Unicode fonts
- Fontconfig
- HarfBuzz text shaping
- Browser font configuration
- The system font cache

If an appropriate Bengali font isn't installed or the font cache becomes inconsistent, Chrome- or Chromium-based browsers may render Bengali incorrectly.

Since both **Google Chrome** and **Brave** are Chromium-based browsers, the same underlying font configuration problem can affect both.

### 1. Install Bengali Unicode Fonts

The easiest solution is to install high-quality Bengali fonts from Ubuntu's package repositories.

Open Terminal and run:

```bash
sudo apt update
```

Then install Bengali-compatible fonts:

```bash
sudo apt install fonts-noto-core fonts-noto-extra fonts-lohit-beng-bengali
```

These packages provide fonts that support Bengali and many other Unicode scripts.

In particular, **Noto Sans Bengali** and **Noto Serif Bengali** are good choices for modern web browsing.

### 2. Rebuild the Font Cache

After installing the fonts, rebuild Ubuntu's font cache:

```bash
fc-cache -f -v
```

This may take a little while.

Once the command finishes, verify that the Bengali fonts are available.

Run:

```bash
fc-list :lang=bn family
```

You should see Bengali-compatible fonts such as:

```text
Noto Sans Bengali
Noto Serif Bengali
Lohit Bengali
```

If these fonts appear in the output, Ubuntu can detect the installed Bengali fonts.

### 3. Configure Bengali Fonts in Google Chrome

Open Google Chrome and enter this address in the address bar:

```text
chrome://settings/fonts
```

You can configure the browser's font preferences there.

For Bengali-heavy websites, a good configuration is:

|Font Setting|Recommended Font|
|---|---|
|Standard font|Noto Sans Bengali|
|Serif font|Noto Serif Bengali|
|Sans-serif font|Noto Sans Bengali|
|Fixed-width font|Noto Sans Mono|

You don't necessarily have to change every option. In many cases, simply installing the fonts and restarting Chrome is enough.

### 4. Configure Bengali Fonts in Brave

Brave uses the Chromium rendering engine, so its font configuration is very similar.

Open:

```text
brave://settings/fonts
```

Then configure the fonts similarly:

- **Standard:** Noto Sans Bengali
- **Serif:** Noto Serif Bengali
- **Sans-serif:** Noto Sans Bengali
- **Fixed-width:** Noto Sans Mono

After changing the settings, completely restart Brave.

### 5. Clear the Font Cache

If Bengali text is still broken, the browser or system may be using an outdated font cache.

Remove the user's Fontconfig cache:

```bash
rm -rf ~/.cache/fontconfig
```

Then rebuild the cache:

```bash
fc-cache -f -v
```

Restart Chrome and Brave afterward.

### 6. Completely Restart the Browsers

Sometimes closing the browser window isn't enough because background browser processes may still be running.

You can terminate Chrome with:

```bash
pkill chrome
```

And Brave with:

```bash
pkill brave
```

Then start the browsers again.

### 7. Test Bengali Rendering

After completing the steps above, open a Bengali website or any page containing Bengali Unicode text.

You can also test with this sentence:

```text
বাংলা আমার মাতৃভাষা। আমি বাংলায় লিখতে এবং পড়তে ভালোবাসি।
```

The text should appear with properly connected Bengali glyphs.

### 8. If the Problem Exists Only in Chrome and Brave

If Bengali works correctly in applications such as Firefox, LibreOffice, or your Linux desktop but remains broken in Chrome and Brave, the issue is probably specific to Chromium's font configuration or rendering environment.

Because Chrome and Brave share Chromium's rendering stack, a problem affecting one can often affect the other.

In that situation, check:

```bash
fc-match sans-serif:lang=bn
```

and:

```bash
fc-match serif:lang=bn
```

These commands show which fonts Ubuntu selects for Bengali text.

Ideally, the output should point to a Bengali-capable font such as:

```text
NotoSansBengali-Regular.ttf
```

or:

```text
NotoSerifBengali-Regular.ttf
```

# Recommended Quick Fix

For most Ubuntu installations, the following is enough:

```bash
sudo apt update
sudo apt install fonts-noto-core fonts-noto-extra fonts-lohit-beng-bengali
fc-cache -f -v
```

Then restart Chrome and Brave.

If the problem remains:

```bash
rm -rf ~/.cache/fontconfig
fc-cache -f -v
```

Finally, restart the browsers again.

# Conclusion

Broken Bengali fonts in Chrome and Brave on Ubuntu are usually not caused by the Bengali website itself. The problem is commonly related to **font availability, font selection, or font caching**.

Installing proper Bengali Unicode fonts such as **Noto Sans Bengali** and **Noto Serif Bengali**, rebuilding Fontconfig's cache, and restarting Chromium-based browsers generally resolves the issue.

The most important commands to remember are:

```bash
sudo apt install fonts-noto-core fonts-noto-extra fonts-lohit-beng-bengali
fc-cache -f -v
fc-list :lang=bn family
```

For a clean Ubuntu setup, **Noto Sans Bengali** is an excellent default choice for everyday Bengali web content.
