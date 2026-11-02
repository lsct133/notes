**Updated Guide: Minimal Debian 13 (Trixie) + GNOME on Wayland (with Bluetooth)**

Here is the revised version with Bluetooth support included.

### 1. Installation (Netinstall)

Same as before:
- Use the **Debian 13 netinstall ISO**.
- In **Software selection**, uncheck everything except **Standard system utilities**.

### 2. Base System Update

```bash
apt update && apt full-upgrade -y
```

### 3. Install Minimal GNOME + Bluetooth

Install the core GNOME packages and explicitly add Bluetooth support:

```bash
apt install --no-install-recommends \
    gnome-core \
    bluez \
    gnome-bluetooth
```

This gives you:
- GNOME Shell + Mutter (Wayland)
- GDM3
- Nautilus
- Full Bluetooth support with GUI integration in GNOME Settings

### 4. Enable Bluetooth Service

```bash
systemctl enable --now bluetooth
```

### 5. Reboot and Verify Wayland

```bash
reboot
```

At the login screen, select **GNOME** (Wayland) using the gear icon.

Verify:

```bash
echo $XDG_SESSION_TYPE   # should show: wayland
bluetoothctl show        # should show your Bluetooth adapter
```

### 6. Disable Other Unnecessary Services

Now that Bluetooth is wanted, we only disable the other non-essential services:

```bash
systemctl disable --now \
    avahi-daemon \
    cups \
    cups-browsed \
    ModemManager \
    wpa_supplicant \
    smartd \
    packagekit \
    2>/dev/null || true

systemctl mask avahi-daemon cups cups-browsed ModemManager
```

### 7. Install a Minimal Terminal (Recommended)

```bash
apt install --no-install-recommends gnome-terminal
# or
apt install --no-install-recommends ptyxis
```

### 8. Final Cleanup

```bash
apt autoremove --purge -y
apt clean
```

### Summary of Changes

| Item                    | Status     | Reason |
|-------------------------|------------|--------|
| `bluez`                 | Installed  | Core Bluetooth daemon |
| `gnome-bluetooth`       | Installed  | GNOME Settings integration |
| `bluetooth` service     | Enabled    | Required for functionality |
| Avahi / Printing / etc. | Disabled   | Kept minimal |
| `--no-install-recommends` | Used    | Prevents bloat |

You now have a very lean GNOME Wayland system that includes working Bluetooth while keeping everything else as minimal as possible.