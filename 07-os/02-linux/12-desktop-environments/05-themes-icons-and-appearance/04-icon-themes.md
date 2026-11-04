# 🗁 ICON PACKS

Ubuntu lets you change your desktop icons to make your system look more modern and attractive. Here are five popular icon packs and how to install them.
### Papirus
Colorful, modern icons with excellent application and folder coverage. [GitHub Repository](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme)

```bash
sudo apt update
sudo apt install papirus-icon-theme
```

### Yaru — (Default)
Ubuntu's original icon theme. Choose it if you like the default appearance. [GitHub Repository](https://github.com/ubuntu/yaru)

```bash
sudo apt install yaru-theme-icon
```

### Tela
Beautiful, flat icons with bright colors and clean designs. [GitHub Repository](https://github.com/vinceliuice/Tela-icon-theme)

```bash
sudo apt install git
git clone https://github.com/vinceliuice/Tela-icon-theme.git
cd Tela-icon-theme
./install.sh
cd ..
rm -rf Tela-icon-theme
```

### Colloid 🔥
Modern icons that look great with a matching desktop theme. [GitHub Repository](https://github.com/vinceliuice/Colloid-icon-theme)

```bash
git clone --depth=1 https://github.com/vinceliuice/Colloid-icon-theme.git
cd Colloid-icon-theme
./install.sh
cd ..
rm -rf Colloid-icon-theme
```

### WhiteSur
A macOS-inspired theme with matching icons and desktop styling. [GitHub Repository](https://github.com/vinceliuice/WhiteSur-gtk-theme)

```bash
git clone --depth=1 https://github.com/vinceliuice/WhiteSur-icon-theme.git
cd WhiteSur-icon-theme
./install.sh
cd ..
rm -rf WhiteSur-icon-theme
```

# 🗁 HOW TO CHANGE ICONS

After installing an icon pack:

1. Install GNOME Tweaks:

   ```bash
   sudo apt install gnome-tweaks
   ```

2. Open **Tweaks**.
3. Select **Appearance**.
4. Find **Icons**.
5. Choose your installed icon theme.
