## EndevOS MacBook Air Install Guide

**Target device:** Apple MacBook Air 13-inch (2017), MacBookAir7,2 / A1466, MQD32LL/A, 8 GB RAM

This guide covers installing EndeavourOS and configuring Wi-Fi, Bluetooth, the trackpad, keyboard keys and backlight, and keyboard-backlight idle behavior.

> **Note:** Hardware support and package names can change. Check commands and AUR packages against current EndeavourOS/Arch documentation before relying on them.

---

## Table of Contents

- [1. Install EndeavourOS](#1-install-endeavouros)
- [2. Wi-Fi: Broadcom `wl` Driver](#2-wi-fi-broadcom-wl-driver)
- [3. Bluetooth Firmware](#3-bluetooth-firmware)
- [4. MacBook Air Optimisations](#4-macbook-air-optimisations)
  - [Function-key behavior](#function-key-behavior)
  - [Battery and power profiles](#battery-and-power-profiles)
  - [FaceTime HD camera](#facetime-hd-camera)
- [5. Trackpad Gestures and Natural Scrolling](#5-trackpad-gestures-and-natural-scrolling)
  - [X11 libinput configuration](#x11-libinput-configuration)
  - [Desktop environment settings](#desktop-environment-settings)
  - [Multi-finger gestures](#multi-finger-gestures)
- [6. Map Brightness and Volume Keys](#6-map-brightness-and-volume-keys)
  - [Set function-key mode](#set-function-key-mode)
  - [Install control utilities](#install-control-utilities)
  - [Desktop environment key-bindings](#desktop-environment-key-bindings)
- [7. Allow Non-Root Keyboard-Backlight Control](#7-allow-non-root-keyboard-backlight-control)
- [8. Restore Keyboard-Backlight Level on Boot](#8-restore-keyboard-backlight-level-on-boot)
- [9. Set an Idle Timeout for the Keyboard Backlight](#9-set-an-idle-timeout-for-the-keyboard-backlight)
  - [Wayland approach using `swayidle`](#wayland-approach-using-swayidle)
  - [X11 approach using `xprintidle`](#x11-approach-using-xprintidle)
- [10. Fade the Backlight Out Smoothly](#10-fade-the-backlight-out-smoothly)
- [11. Fade Out Slowly, Restore Instantly](#11-fade-out-slowly-restore-instantly)
- [12. Dim to a 10% Floor Instead of Turning Off](#12-dim-to-a-10-floor-instead-of-turning-off)
- [13. Use Different Idle Levels on AC Power and Battery](#13-use-different-idle-levels-on-ac-power-and-battery)
- [14. Keep the Backlight Off Below 20% Battery](#14-keep-the-backlight-off-below-20-battery)
- [15. Notify When the Backlight Is Disabled for Low Battery](#15-notify-when-the-backlight-is-disabled-for-low-battery)

---

## 1. Install EndeavourOS

1. Plug in the Ventoy USB drive, power on the MacBook Air, and hold **Option (⌥)** until Apple Startup Manager appears.
2. Select the EFI boot drive and launch the EndeavourOS ISO.
3. If Wi-Fi is unavailable in the live environment, connect your phone by USB and enable USB tethering, or use a USB Ethernet adapter.
4. Launch Calamares from the Welcome app and complete the installation.
5. Consider installing the LTS kernel and its headers alongside the standard kernel for additional kernel-module compatibility.
6. Reboot after installation.

## 2. Wi-Fi: Broadcom `wl` Driver

The guide identifies the wireless card as a Broadcom BCM4360 and recommends the proprietary `broadcom-wl-dkms` module.

If Wi-Fi is not detected after installation, connect using USB tethering or Ethernet and install the driver and kernel headers:

```bash
sudo pacman -Syu linux-headers linux-lts-headers dkms broadcom-wl-dkms
```

Remove potentially conflicting modules and load `wl`:

```bash
sudo modprobe -r b43 b44 b43legacy ssb brcmsmac bcma
sudo modprobe wl
```

Check for a wireless interface:

```bash
nmcli device
ip link
```

## 3. Bluetooth Firmware

If Bluetooth reports that no adapters are found, install the Bluetooth utilities and the firmware package referenced in the guide:

```bash
yay -S bluez bluez-utils broadcom-bt-firmware
```

Enable and start Bluetooth:

```bash
sudo systemctl enable --now bluetooth.service
```

Check the controller:

```bash
bluetoothctl show
```

## 4. MacBook Air Optimisations

### Function-key behavior

To make the top row act as media keys by default, create a module configuration file:

```bash
echo "options hid_apple fnmode=2" | sudo tee /etc/modprobe.d/hid_apple.conf
```

Use `fnmode=1` if you prefer standard F1–F12 behavior by default.

### Battery and power profiles

Install and enable TLP:

```bash
sudo pacman -S tlp tlp-rdw
sudo systemctl enable --now tlp.service
```

### FaceTime HD camera

The guide says the built-in camera may require additional packages:

```bash
yay -S facetimehd-dkms facetimehd-firmware
sudo modprobe facetimehd
```

## 5. Trackpad Gestures and Natural Scrolling

### X11 libinput configuration

Create or edit the configuration file:

```bash
sudo nano /etc/X11/xorg.conf.d/30-touchpad.conf
```

Add:

```text
Section "InputClass"
    Identifier "Apple Wireless/Internal Touchpad"
    MatchIsTouchpad "on"
    Driver "libinput"
    Option "Tapping" "on"
    Option "NaturalScrolling" "true"
    Option "TappingButtonMap" "lrm"
    Option "ClickMethod" "clickfinger"
    Option "DisableWhileTyping" "true"
    Option "ScrollMethod" "twofinger"
    Option "AccelSpeed" "0.15"
EndSection
```

Save and exit. Log out and back in, or check detected devices with:

```bash
libinput list-devices
```

### Desktop environment settings

- **KDE Plasma:** Open **System Settings → Mouse & Touchpad → Touchpad**. Enable natural scrolling and tap-to-click; configure two-finger right-click as desired.
- **GNOME:** Open **Settings → Mouse & Touchpad**. Enable natural scrolling, tap-to-click, and two-finger scrolling.
- **Wayland:** The X11 configuration above does not apply. Use the desktop environment’s settings.

### Multi-finger gestures

For X11 or lightweight desktop environments, the guide suggests Touchégg:

```bash
yay -S touchegg touche
sudo systemctl enable --now touchegg.service
```

Use the Touché app to configure gestures such as workspace switching or an overview gesture. GNOME and KDE Plasma on Wayland may provide multi-finger gestures natively.

## 6. Map Brightness and Volume Keys

### Set function-key mode

Create the `hid_apple` configuration:

```bash
echo "options hid_apple fnmode=2" | sudo tee /etc/modprobe.d/hid_apple.conf
```

Rebuild the initramfs using the tool appropriate to the installation:

```bash
sudo dracut-rebuild
```

Or, if the system uses mkinitcpio:

```bash
sudo mkinitcpio -P
```

To apply the setting immediately, if the module is loaded:

```bash
echo 2 | sudo tee /sys/module/hid_apple/parameters/fnmode
```

With `fnmode=2`, the top row acts as media keys by default; hold **Fn** for F1–F12.

### Install control utilities

```bash
sudo pacman -S brightnessctl playerctl pamixer
```

Test keyboard-backlight control:

```bash
brightnessctl --device='smc::kbd_backlight' set 50%
```

### Desktop environment key-bindings

KDE Plasma and GNOME may map the media keys automatically. Typical mappings on this model are:

- **F1/F2:** Screen brightness down/up
- **F5/F6:** Keyboard backlight down/up
- **F10:** Mute
- **F11/F12:** Volume down/up

In a window manager, add appropriate bindings. Example for i3 or Sway:

```text
# Screen brightness
bindsym XF86MonBrightnessDown exec brightnessctl set 5%-
bindsym XF86MonBrightnessUp exec brightnessctl set 5%+

# Keyboard backlight
bindsym XF86KbdBrightnessDown exec brightnessctl --device='smc::kbd_backlight' set 10%-
bindsym XF86KbdBrightnessUp exec brightnessctl --device='smc::kbd_backlight' set 10%+

# Audio
bindsym XF86AudioRaiseVolume exec wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
bindsym XF86AudioLowerVolume exec wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bindsym XF86AudioMute exec wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle

# Media playback
bindsym XF86AudioPlay exec playerctl play-pause
bindsym XF86AudioNext exec playerctl next
bindsym XF86AudioPrev exec playerctl previous
```

## 7. Allow Non-Root Keyboard-Backlight Control

Add your user to the `video` group:

```bash
sudo usermod -aG video "$USER"
```

Log out and back in for the group change to take effect.

Create a udev rule:

```bash
sudo nano /etc/udev/rules.d/90-macbook-backlight.rules
```

Add:

```text
# Allow users in video group to control screen backlight
ACTION=="add", SUBSYSTEM=="backlight", RUN+="/bin/chgrp video $sys$devpath/brightness", RUN+="/bin/chmod 0664 $sys$devpath/brightness"

# Allow users in video group to control Apple SMC keyboard backlight
ACTION=="add", SUBSYSTEM=="leds", KERNEL=="*::kbd_backlight", RUN+="/bin/chgrp video $sys$devpath/brightness", RUN+="/bin/chmod 0664 $sys$devpath/brightness"
```

Reload the rules and trigger device events:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger --subsystem-match=leds --subsystem-match=backlight
```

Check permissions:

```bash
ls -l /sys/class/leds/smc::kbd_backlight/brightness
```

Test without `sudo`:

```bash
brightnessctl --device='smc::kbd_backlight' set 30%
```

## 8. Restore Keyboard-Backlight Level on Boot

Systemd may already restore brightness for supported devices. Check the service and saved state:

```bash
sudo systemctl status 'systemd-backlight@leds:smc::kbd_backlight.service'
cat /var/lib/systemd/backlight/leds:smc::kbd_backlight
```

If automatic restoration does not work, the guide proposes a custom script.

Create the script:

```bash
sudo nano /usr/local/bin/restore-kbd-backlight.sh
```

Add:

```bash
#!/bin/bash

DEVICE="/sys/class/leds/smc::kbd_backlight/brightness"
STATE_FILE="/var/lib/systemd/backlight/macbook_kbd_brightness"

if [ "$1" = "save" ]; then
    if [ -f "$DEVICE" ]; then
        cat "$DEVICE" > "$STATE_FILE"
    fi
elif [ "$1" = "restore" ]; then
    if [ -f "$STATE_FILE" ] && [ -f "$DEVICE" ]; then
        cat "$STATE_FILE" > "$DEVICE"
    fi
fi
```

Make it executable:

```bash
sudo chmod +x /usr/local/bin/restore-kbd-backlight.sh
```

Create a systemd service:

```bash
sudo nano /etc/systemd/system/macbook-kbd-backlight.service
```

Add:

```ini
[Unit]
Description=Save and Restore MacBook Keyboard Backlight
After=systemd-modules-load.service
DefaultDependencies=no
Before=shutdown.target

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/bin/restore-kbd-backlight.sh restore
ExecStop=/usr/local/bin/restore-kbd-backlight.sh save

[Install]
WantedBy=multi-user.target
```

Enable it:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now macbook-kbd-backlight.service
```

## 9. Set an Idle Timeout for the Keyboard Backlight

### Wayland approach using `swayidle`

Install `swayidle`:

```bash
sudo pacman -S swayidle
```

Create a helper script:

```bash
mkdir -p ~/.local/bin
nano ~/.local/bin/kbd-backlight-idle.sh
```

Add:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)
        if [ "$CURRENT" -gt 0 ]; then
            echo "$CURRENT" > "$STATE_FILE"
            brightnessctl --device="$DEVICE" set 0
        fi
        ;;
    resume)
        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            brightnessctl --device="$DEVICE" set "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

Make it executable:

```bash
chmod +x ~/.local/bin/kbd-backlight-idle.sh
```

Create a user service:

```bash
mkdir -p ~/.config/systemd/user
nano ~/.config/systemd/user/kbd-idle.service
```

Add:

```ini
[Unit]
Description=MacBook Keyboard Backlight Idle Timeout
PartOf=graphical-session.target

[Service]
Type=simple
ExecStart=/usr/bin/swayidle -w \
    timeout 30 "%h/.local/bin/kbd-backlight-idle.sh dim" \
    resume "%h/.local/bin/kbd-backlight-idle.sh resume"
Restart=on-failure

[Install]
WantedBy=graphical-session.target
```

Enable it:

```bash
systemctl --user daemon-reload
systemctl --user enable --now kbd-idle.service
```

### X11 approach using `xprintidle`

Install `xprintidle`:

```bash
sudo pacman -S xprintidle
```

Create a script:

```bash
nano ~/.local/bin/x11-kbd-idle.sh
```

Add:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
TIMEOUT=30000 # 30 seconds in milliseconds
DIMMED=0
SAVED_LEVEL=0

while true; do
    IDLE_TIME=$(xprintidle)

    if [ "$IDLE_TIME" -ge "$TIMEOUT" ] && [ "$DIMMED" -eq 0 ]; then
        SAVED_LEVEL=$(brightnessctl --device="$DEVICE" get)
        if [ "$SAVED_LEVEL" -gt 0 ]; then
            brightnessctl --device="$DEVICE" set 0
            DIMMED=1
        fi
    elif [ "$IDLE_TIME" -lt "$TIMEOUT" ] && [ "$DIMMED" -eq 1 ]; then
        brightnessctl --device="$DEVICE" set "$SAVED_LEVEL"
        DIMMED=0
    fi

    sleep 1
done
```

Make it executable and configure it to start with your desktop:

```bash
chmod +x ~/.local/bin/x11-kbd-idle.sh
```

- **i3:** Add `exec --no-startup-id ~/.local/bin/x11-kbd-idle.sh` to `~/.config/i3/config`.
- **XFCE:** Add the script under **Settings → Session and Startup → Application Autostart**.

## 10. Fade the Backlight Out Smoothly

Replace the idle helper script with a stepped fade:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"
STEPS=20
SLEEP_INTERVAL=0.03

fade() {
    local start=$1
    local end=$2
    local diff=$((end - start))

    for i in $(seq 1 "$STEPS"); do
        local current=$((start + (diff * i / STEPS)))
        brightnessctl -q --device="$DEVICE" set "$current"
        sleep "$SLEEP_INTERVAL"
    done

    brightnessctl -q --device="$DEVICE" set "$end"
}

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)
        if [ "$CURRENT" -gt 0 ]; then
            echo "$CURRENT" > "$STATE_FILE"
            fade "$CURRENT" 0
        fi
        ;;
    resume)
        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            fade 0 "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

Make it executable:

```bash
chmod +x ~/.local/bin/kbd-backlight-idle.sh
```

The guide suggests `STEPS=15` and `SLEEP_INTERVAL=0.02` for a faster fade, or `STEPS=30` and `SLEEP_INTERVAL=0.04` for a slower one.

## 11. Fade Out Slowly, Restore Instantly

Use a fade function only for dimming; restore brightness with one command:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"
STEPS=25
SLEEP_INTERVAL=0.03

fade_out() {
    local start=$1
    local end=0
    local diff=$((end - start))

    for i in $(seq 1 "$STEPS"); do
        local current=$((start + (diff * i / STEPS)))
        brightnessctl -q --device="$DEVICE" set "$current"
        sleep "$SLEEP_INTERVAL"
    done

    brightnessctl -q --device="$DEVICE" set 0
}

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)
        if [ "$CURRENT" -gt 0 ]; then
            echo "$CURRENT" > "$STATE_FILE"
            fade_out "$CURRENT"
        fi
        ;;
    resume)
        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            brightnessctl -q --device="$DEVICE" set "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

## 12. Dim to a 10% Floor Instead of Turning Off

This version reads the hardware maximum brightness and calculates a 10% floor:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"
STEPS=25
SLEEP_INTERVAL=0.03

MAX_BRIGHTNESS=$(brightnessctl --device="$DEVICE" max)
TARGET_FLOOR=$((MAX_BRIGHTNESS / 10))

fade_to_floor() {
    local start=$1
    local end=$TARGET_FLOOR
    local diff=$((end - start))

    for i in $(seq 1 "$STEPS"); do
        local current=$((start + (diff * i / STEPS)))
        brightnessctl -q --device="$DEVICE" set "$current"
        sleep "$SLEEP_INTERVAL"
    done

    brightnessctl -q --device="$DEVICE" set "$end"
}

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)
        if [ "$CURRENT" -gt "$TARGET_FLOOR" ]; then
            echo "$CURRENT" > "$STATE_FILE"
            fade_to_floor "$CURRENT"
        fi
        ;;
    resume)
        pkill -f "kbd-backlight-idle.sh dim" 2>/dev/null
        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            brightnessctl -q --device="$DEVICE" set "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

Make it executable:

```bash
chmod +x ~/.local/bin/kbd-backlight-idle.sh
```

## 13. Use Different Idle Levels on AC Power and Battery

The following version uses a 10% floor while plugged in and turns the backlight off on battery:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"
STEPS=25
SLEEP_INTERVAL=0.03

MAX_BRIGHTNESS=$(brightnessctl --device="$DEVICE" max)

if grep -qs 1 /sys/class/power_supply/*/online; then
    TARGET_FLOOR=$((MAX_BRIGHTNESS / 10))
else
    TARGET_FLOOR=0
fi

fade_to_floor() {
    local start=$1
    local end=$2
    local diff=$((end - start))

    for i in $(seq 1 "$STEPS"); do
        local current=$((start + (diff * i / STEPS)))
        brightnessctl -q --device="$DEVICE" set "$current"
        sleep "$SLEEP_INTERVAL"
    done

    brightnessctl -q --device="$DEVICE" set "$end"
}

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)
        if [ "$CURRENT" -gt "$TARGET_FLOOR" ]; then
            echo "$CURRENT" > "$STATE_FILE"
            fade_to_floor "$CURRENT" "$TARGET_FLOOR"
        fi
        ;;
    resume)
        pkill -f "kbd-backlight-idle.sh dim" 2>/dev/null
        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            brightnessctl -q --device="$DEVICE" set "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

## 14. Keep the Backlight Off Below 20% Battery

This version turns the backlight off when running on battery at or below the configured threshold:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"
STEPS=25
SLEEP_INTERVAL=0.03
LOW_BATTERY_THRESHOLD=20

is_low_battery() {
    # AC power overrides the low-battery restriction.
    if grep -qs 1 /sys/class/power_supply/*/online; then
        return 1
    fi

    local capacity
    capacity=$(cat /sys/class/power_supply/BAT*/capacity 2>/dev/null | head -n1)

    if [ -n "$capacity" ] && [ "$capacity" -le "$LOW_BATTERY_THRESHOLD" ]; then
        return 0
    fi

    return 1
}

MAX_BRIGHTNESS=$(brightnessctl --device="$DEVICE" max)

if grep -qs 1 /sys/class/power_supply/*/online; then
    TARGET_FLOOR=$((MAX_BRIGHTNESS / 10))
else
    TARGET_FLOOR=0
fi

fade_to_floor() {
    local start=$1
    local end=$2
    local diff=$((end - start))

    for i in $(seq 1 "$STEPS"); do
        local current=$((start + (diff * i / STEPS)))
        brightnessctl -q --device="$DEVICE" set "$current"
        sleep "$SLEEP_INTERVAL"
    done

    brightnessctl -q --device="$DEVICE" set "$end"
}

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)

        if is_low_battery; then
            brightnessctl -q --device="$DEVICE" set 0
            exit 0
        fi

        if [ "$CURRENT" -gt "$TARGET_FLOOR" ]; then
            echo "$CURRENT" > "$STATE_FILE"
            fade_to_floor "$CURRENT" "$TARGET_FLOOR"
        fi
        ;;
    resume)
        pkill -f "kbd-backlight-idle.sh dim" 2>/dev/null

        if is_low_battery; then
            brightnessctl -q --device="$DEVICE" set 0
            exit 0
        fi

        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            brightnessctl -q --device="$DEVICE" set "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

The guide suggests checking the battery interface with:

```bash
cat /sys/class/power_supply/BAT*/capacity
```

## 15. Notify When the Backlight Is Disabled for Low Battery

Install `notify-send`:

```bash
sudo pacman -S --needed libnotify
```

Test notifications:

```bash
notify-send "Test" "Notification test working"
```

The following script adds a flag file so the notification is sent once until the low-battery condition clears or AC power is restored:

```bash
#!/bin/bash

DEVICE="smc::kbd_backlight"
STATE_FILE="${XDG_RUNTIME_DIR}/kbd_backlight_idle_level"
NOTIFY_FLAG="${XDG_RUNTIME_DIR}/kbd_backlight_low_notified"
STEPS=25
SLEEP_INTERVAL=0.03
LOW_BATTERY_THRESHOLD=20

notify_low_battery() {
    if [ ! -f "$NOTIFY_FLAG" ]; then
        local cap=$1
        export DBUS_SESSION_BUS_ADDRESS="${DBUS_SESSION_BUS_ADDRESS:-unix:path=/run/user/$(id -u)/bus}"

        notify-send \
            --urgency=normal \
            --icon=battery-caution \
            --app-name="Power Management" \
            "Keyboard Backlight Disabled" \
            "Battery is at ${cap}%. Backlight turned off to conserve power."

        touch "$NOTIFY_FLAG"
    fi
}

is_low_battery() {
    if grep -qs 1 /sys/class/power_supply/*/online; then
        rm -f "$NOTIFY_FLAG"
        return 1
    fi

    local capacity
    capacity=$(cat /sys/class/power_supply/BAT*/capacity 2>/dev/null | head -n1)

    if [ -n "$capacity" ] && [ "$capacity" -le "$LOW_BATTERY_THRESHOLD" ]; then
        notify_low_battery "$capacity"
        return 0
    fi

    rm -f "$NOTIFY_FLAG"
    return 1
}

MAX_BRIGHTNESS=$(brightnessctl --device="$DEVICE" max)

if grep -qs 1 /sys/class/power_supply/*/online; then
    TARGET_FLOOR=$((MAX_BRIGHTNESS / 10))
else
    TARGET_FLOOR=0
fi

fade_to_floor() {
    local start=$1
    local end=$2
    local diff=$((end - start))

    for i in $(seq 1 "$STEPS"); do
        local current=$((start + (diff * i / STEPS)))
        brightnessctl -q --device="$DEVICE" set "$current"
        sleep "$SLEEP_INTERVAL"
    done

    brightnessctl -q --device="$DEVICE" set "$end"
}

case "$1" in
    dim)
        CURRENT=$(brightnessctl --device="$DEVICE" get)

        if is_low_battery; then
            brightnessctl -q --device="$DEVICE" set 0
            exit 0
        fi

        if [ "$CURRENT" -gt "$TARGET_FLOOR" ]; then
            echo "$CURRENT" > "$STATE_FILE"
            fade_to_floor "$CURRENT" "$TARGET_FLOOR"
        fi
        ;;
    resume)
        pkill -f "kbd-backlight-idle.sh dim" 2>/dev/null

        if is_low_battery; then
            brightnessctl -q --device="$DEVICE" set 0
            exit 0
        fi

        if [ -f "$STATE_FILE" ]; then
            SAVED=$(cat "$STATE_FILE")
            brightnessctl -q --device="$DEVICE" set "$SAVED"
            rm -f "$STATE_FILE"
        fi
        ;;
esac
```

Make the script executable:

```bash
chmod +x ~/.local/bin/kbd-backlight-idle.sh
```

To test the notification without draining the battery, temporarily raise `LOW_BATTERY_THRESHOLD` in the script, remove the notification flag, and run the script while unplugged:

```bash
rm -f "${XDG_RUNTIME_DIR}/kbd_backlight_low_notified"
~/.local/bin/kbd-backlight-idle.sh dim
```

Restore `LOW_BATTERY_THRESHOLD=20` after testing.