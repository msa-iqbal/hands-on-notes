# PERSONALIZE: “GNOME” Desktop Environment (DE)

The easiest way is to use **Ubuntu's package manager for fonts/icons** and install the rest with one script.
### Step 1: Install fonts, icons, and Tweaks

```bash
sudo apt update

sudo apt install -y \
  papirus-icon-theme \
  fonts-inter \
  fonts-jetbrains-mono \
  gnome-tweaks \
  git
```

### Step 2: Install Orchis Theme

```bash
git clone <https://github.com/vinceliuice/Orchis-theme.git>
cd Orchis-theme
./install.sh
```

### Step 3: Install Bibata Cursor

```bash
git clone <https://github.com/ful1e5/Bibata_Cursor.git>
cd Bibata_Cursor
sudo ./build.sh -t ice
```

If that fails, use:

```bash
sudo apt install bibata-cursor-theme
```

(if available in your Ubuntu version).

### Step 4: Open GNOME Tweaks

```bash
gnome-tweaks
```

Go to **Appearance** and select:

- **Applications:** Orchis-Dark
- **Icons:** Papirus-Dark
- **Cursor:** Bibata-Modern-Ice

Go to **Fonts** and select:

- Interface Text → Inter 11
- Document Text → Inter 11
- Monospace Text → JetBrains Mono 12
