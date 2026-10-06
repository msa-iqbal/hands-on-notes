# 🗁 START USING `Flatpak`

Flatpak app store is [Flathub](https://flathub.org/en)

#### APPS LIST WHICH INSTALL BY FLATPAK

* Internet Browser: [🔥Brave](https://flathub.org/en/apps/com.brave.Browser) [Firefox](https://flathub.org/en/apps/org.mozilla.firefox) [Google Chrome](https://flathub.org/en/apps/com.google.Chrome)
* Security: [Proton VPN](https://flathub.org/en/apps/com.protonvpn.www)
* Video Player: [🔥mpv](https://flathub.org/en/apps/io.mpv.Mpv) [Cine](https://flathub.org/en/apps/io.github.diegopvlk.Cine) [VLC](https://flathub.org/en/apps/org.videolan.VLC) [Celluloid](https://flathub.org/en/apps/io.github.celluloid_player.Celluloid)
* Music Player: [Strawberry Music Player](https://flathub.org/en/apps/org.strawberrymusicplayer.strawberry)
* Download Manager: [Free Download Manager]([# Free Download Manager](https://flathub.org/en/apps/org.freedownloadmanager.Manager)
* Youtube Video Downloader: [Video Downloader](https://flathub.org/en/apps/com.github.unrud.VideoDownloader)
* Screenshot: [Flameshot](https://flathub.org/en/apps/org.flameshot.Flameshot)
* System: [CPU-X](https://flathub.org/en/apps/io.github.thetumultuousunicornofdarkness.cpu-x)
* Games: [Solitaire](https://flathub.org/en/apps/org.gnome.gitlab.wwarner.Solitaire)
---
* Office Suite: [LibreOffice](https://flathub.org/en/apps/org.libreoffice.LibreOffice) [ONLYOFFICE (MS-Office Comfortable)](https://flathub.org/en/apps/org.onlyoffice.desktopeditors)
* Email Client: [Thunderbird](https://flathub.org/en/apps/org.mozilla.thunderbird)
* Remote Desktop: [AnyDesk](https://flathub.org/en/apps/com.anydesk.Anydesk) [RustDesk](https://flathub.org/en/apps/com.rustdesk.RustDesk)
* Remote Desktop: [Remmina (VNC)](https://flathub.org/en/apps/org.remmina.Remmina) [TigerVNC Viewer](https://flathub.org/en/apps/org.tigervnc.vncviewer)
* Photo Editor: [GIMP](https://flathub.org/en/apps/org.gimp.GIMP) [Krita](https://flathub.org/en/apps/org.kde.krita) [🔥Pinta](https://flathub.org/en/apps/com.github.PintaProject.Pinta) [RawTherapee](https://flathub.org/en/apps/com.rawtherapee.RawTherapee)
* Photography: [🔥Darktable](https://flathub.org/en/apps/org.darktable.Darktable)
* Image Organization: [digiKam](https://www.digikam.org/)
* Video Editor: [Kdenlive](https://flathub.org/en/apps/org.kde.kdenlive) [Shotcut](https://flathub.org/en/apps/org.shotcut.Shotcut)
* Media Converter & Compressor: [HandBrake (Video)](https://flathub.org/en/apps/fr.handbrake.ghb) [Frame](https://flathub.org/en/apps/io.github._66HEX.Frame)
---

**DEVELOPER**

* API-Testing: [Postman](https://flathub.org/en/apps/com.getpostman.Postman)
* Code Editor: [Visual Studio Code](https://flathub.org/en/apps/com.visualstudio.code) [VSCodium](https://flathub.org/en/apps/com.vscodium.codium)
* Color Picker: [Eyedropper](https://flathub.org/en/apps/com.github.finefindus.eyedropper)
* Version-Control: [GitHub Desktop](https://flathub.org/en/apps/io.github.shiftey.Desktop)

**PRODUCTIVITY**

* Note Taking: [Obsidian](https://flathub.org/en/apps/md.obsidian.Obsidian)

**EXTRAS**

* Media-server Platform: [Jellyfin Desktop](https://flathub.org/en/apps/org.jellyfin.JellyfinDesktop)

---
#### INSTALL FLATPAK ON UBUNTU

```bash
sudo apt install flatpak
```

**Install GNOME Software Flatpak plugin**

```bash
sudo apt install gnome-software-plugin-flatpak
```

**Add the Flathub repository**

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

To complete setup, restart your system. Now all you have to do is [install apps](https://flathub.org/en)!

# 🗁 RUN `.AppImage` ON UBUNTU

#### `.AppImage` BASED APPLICATION LIST

* Media Converter & Compressor: [shutterencoder](https://www.shutterencoder.com/)

---
#### RUN `.AppImage` FILE

**Download the `.AppImage`**

For example, suppose you downloaded:

```text
MyApplication.AppImage
```

Usually it will be in:

```text
~/Downloads
```

### 2. Make the AppImage executable

Open **Files** and go to your `Downloads` folder.

Right-click the `.AppImage` file → **Properties** → **Permissions**.

Enable:

> **Allow executing file as program**

Then close the Properties window.

### 3. Run the AppImage

Double-click the `.AppImage` file.

If Ubuntu asks what to do, select:

> **Run**

That's it. The application should launch.

# 🗁 RUN APPS VIA COMMAND-LINE

* Media Converter & Compressor: [FFmpeg](https://www.ffmpeg.org/download.html)
* Video Downloader: [yt-dlp (with FFmpeg)](https://github.com/yt-dlp/yt-dlp/blob/master/README.md)
* Image Processing Suite: [ImageMagick](https://imagemagick.org)
