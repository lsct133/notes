To build a truly lean Debian 13 (Trixie) system with GNOME on Wayland, you must avoid Debian’s default metapackages (like `gnome` or `gnome-core`). These pull in a massive amount of background services (Tracker, Evolution Data Server, Avahi, ModemManager) and unneeded applications.

Here is the step-by-step guide to achieving an ultra-minimal GNOME Wayland desktop.

---

### Step 1: Base System Installation

Since Debian 13 (Trixie) is currently the "Testing" branch (until mid-2025), you will need the Testing Netinst ISO.

1. Download the **Debian Testing Netinst ISO** from the official Debian website.
2. Boot the installer and proceed with a normal installation.
3. **CRITICAL STEP:** When you reach the **Software Selection (tasksel)** screen, **UNCHECK EVERYTHING**.
    - Uncheck "Debian desktop environment".
    - Uncheck "GNOME".
    - Keep "Standard system utilities" checked (recommended) or uncheck it for absolute minimalism.
4. Finish the installation and reboot. You will boot into a plain text TTY terminal.

---

### Step 2: Configure APT to Ignore "Recommends"

Debian’s package manager installs "Recommended" packages by default. This is the #1 cause of bloat. We need to turn this off before installing the GUI.

1. Log in to the TTY with your `root` user (or use `sudo` if you set up a standard user without a root password).
2. Create an APT configuration file to disable recommended packages:
    
    Bash
    
    ```
    echo 'APT::Install-Recommends "false";' > /etc/apt/apt.conf.d/99-no-recommends
    echo 'APT::Install-Suggests "false";' >> /etc/apt/apt.conf.d/99-no-recommends
    ```
    
3. Update your repositories:
    
    Bash
    
    ```
    apt update
    ```
    

---

### Step 3: Install the Minimal GNOME Wayland Stack

Now we install only the absolute bare minimum required for GNOME to function on Wayland.

Run the following command (as root):

Bash

```
apt install gdm3 gnome-shell gnome-session gnome-control-center nautilus gnome-console
```

_What this does:_

- `gdm3`: The GNOME Display Manager (required for proper GNOME Wayland integration and lock screens).
- `gnome-shell` & `gnome-session`: The actual Wayland desktop and compositor (Mutter).
- `gnome-control-center`: The settings app.
- `nautilus`: File manager.
- `gnome-console`: A very lightweight terminal (replaces the older `gnome-terminal`).

---

### Step 4: Install Essential Hardware & System Utilities

Because we turned off "Recommends", your system doesn't know how to handle networking, audio, or power yet. Let's add the leanest modern backends.

Bash

```
apt install network-manager pipewire pipewire-pulse wireplumber power-profiles-daemon udisks2 polkitd
```

_What this does:_

- `network-manager`: Allows GNOME Settings to manage your internet.
- `pipewire` stack: The modern, lightweight audio/video routing system (replaces PulseAudio).
- `power-profiles-daemon`: Enables the Power Saver / Balanced / Performance toggles in the GNOME quick settings menu.
- `udisks2` & `polkitd`: Allows you (a normal user) to click and mount USB drives in Nautilus without needing root passwords.

Enable NetworkManager to start on boot:

Bash

```
systemctl enable NetworkManager
```

---

### Step 5: Prevent Heavy Background Services (Tracker)

Even with a minimal install, GNOME relies on `tracker` (a file indexing service) for Nautilus search. Tracker runs in the background, consuming CPU and disk I/O.

Since you cannot uninstall it without breaking Nautilus, you can **mask** it so it never runs:

Log in as your standard user (not root) or switch to them (`su - yourusername`), and run:

Bash

```
systemctl --user mask tracker-extract-3.service tracker-miner-fs-3.service tracker-miner-rss-3.service tracker-writeback-3.service tracker-xdg-portal-3.service
```

_(Note: This means searching inside Nautilus will only search filenames in the current folder, rather than full-text indexing your whole drive—which is exactly what you want for a lean system)._

---

### Step 6: Install Core Fonts & Themes

Because we skipped the metapackages, GNOME might look broken (missing icons/fonts). Install the defaults:

Bash

```
apt install fonts-cantarell adwaita-icon-theme xwayland
```

_(Note: `xwayland` is included here because while your desktop is pure Wayland, some third-party apps—like Discord, older games, or certain Electron apps—will completely fail to launch without XWayland running seamlessly in the background)._

---

### Step 7: Verify and Reboot

Before restarting, ensure `gdm3` (the login screen) is set to start automatically:

Bash

```
systemctl enable gdm3
```

Now, reboot the system:

Bash

```
reboot
```

---

### Post-Install: What you have now

Upon rebooting, you will be greeted by GDM. Log in.

- **Total RAM Usage:** Usually between 600MB and 800MB on boot.
- **Running Services:** You will have no GNOME Software center checking for updates in the background, no Evolution data server syncing calendars, no Avahi broadcasting to the local network, and no Tracker indexing your files.
- **Installed Apps:** You will literally only have Settings, Terminal, and Files.

**Adding Software:**  
Whenever you install software moving forward (like Firefox), APT will continue to ignore "Recommended" bloatware.

Bash

```
apt install firefox-esr
```

_Tip: If an application you install ever acts weird or is missing a feature, check its package page on Debian packages. You may need to manually install one of its "Recommended" packages that you actually need._