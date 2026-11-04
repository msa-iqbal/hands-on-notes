# 🗁 Apply GNOME Themes in Ubuntu

### METHOD 1: Apply a Theme Using GNOME Tweaks

GNOME Tweaks allows you to change GTK application themes easily.

**STEP 1: INSTALL GNOME TWEAKS**

Open Terminal and run:

```bash
sudo apt update
sudo apt install -y gnome-tweaks
```

**STEP 2: INSTALL YOUR THEME**

Download your preferred GTK theme from a trusted source, such as GitHub, and follow its installation instructions.

For themes that provide an installation script, open the extracted or cloned theme directory and run the corresponding installer as instructed by the developer.

**STEP 3: APPLY YOUR THEME**

1. Open **GNOME Tweaks**.
2. Go to **Appearance**.
3. Under **Applications**, select your installed theme.

**💡Important:** GTK4 and Libadwaita applications may not fully follow third-party themes. Some applications, such as newer GNOME apps, may retain their default appearance.
### METHOD 2: Apply a Theme Using GNOME Extension Manager

The **User Themes** extension allows you to apply custom GNOME Shell themes, particularly to the top bar, Activities overview, and other Shell interface elements.

**STEP 1: INSTALL EXTENSION MANAGER**

```bash
sudo apt update
sudo apt install -y gnome-shell-extension-manager
```

**STEP 2: INSTALL THE USER THEMES EXTENSION**

Open the following page:

[https://extensions.gnome.org/extension/19/user-themes/](https://extensions.gnome.org/extension/19/user-themes/)

Install the extension if it supports your GNOME version. Alternatively, open **Extension Manager**, search for **User Themes**, and install it there.

**STEP 3: APPLY YOUR SHELL THEME**

1. Open **GNOME Tweaks**.
2. Go to **Appearance**.
3. Under **Shell**, select your installed GNOME Shell theme.

If the **Shell** option is unavailable, ensure that the User Themes extension is installed and enabled.

**💡Important:** GNOME Shell themes are different from GTK application themes. You may need to install both to achieve a consistent desktop appearance.

### METHOD 3: Manage GTK4 Themes Using GTK4 Theme Manager 🔥

GTK4 Theme Manager is a graphical utility for managing compatible GTK4 themes. Depending on the tool's version, it may help you preview, install, or link themes without manually moving theme directories.

**STEP 1: INSTALL GTK4 THEME MANAGER**

Follow the installation instructions provided by the specific GTK4 Theme Manager project you use. If it is hosted on GitHub, check its README for the latest dependencies and installation commands.

GNOME Theme Manager is a graphical tool for managing GTK4 themes. It helps you preview and apply themes easily.

Open Terminal and run:

```bash
sudo apt update
sudo apt install git python3-gi libglib2.0-dev meson gcc python3-dev libxcursor-dev libgdk-pixbuf2.0-dev
```

**Download the source code**

Clone the project's GitHub repository:

```bash
git clone <repository-url>
cd Gnome-Theme-Manager
```

Replace `<repository-url>` with the project's actual GitHub repository URL.

The repository's README uses `./script.sh build` and `./script.sh install` for this version of GTK4 Theme Manager.

```bash
git clone https://github.com/aleritty/gtk4-theme-manager.git
cd gtk4-theme-manager
./script.sh build
./script.sh install
```

**STEP 2: OPEN GTK4 THEME MANAGER**

Open the Ubuntu Applications menu and search for GNOME Theme Manager.
You can now use it to manage your GTK4 themes.

**STEP 3: INSTALL AND APPLY A THEME**

1. Select a compatible GTK4 theme.
2. Follow the application's instructions to install or link it.
3. Apply the theme using the available controls.
4. Restart the affected application if necessary.

**💡Important:** GTK4 Theme Manager is not a universal theme switcher. GTK4 and Libadwaita applications may ignore third-party themes, and the manager cannot guarantee that every application will use the same appearance.
