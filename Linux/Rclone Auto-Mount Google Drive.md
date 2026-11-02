# Auto-mounting Google Drive with rclone on Debian

There are a few approaches. Here's the most reliable method using **systemd**:

---

### Prerequisites

1. Make sure rclone is configured and working:
   ```bash
   rclone ls myremote:   # test your remote
   ```

2. Create the mount point:
   ```bash
   mkdir -p ~/gdrive
   ```

3. Install fuse3 if not already present:
   ```bash
   sudo apt install fuse3
   ```
---

### Method: Systemd User Service (Recommended)

#### 1. Create the service file

```bash
mkdir -p ~/.config/systemd/user/
nano ~/.config/systemd/user/rclone-gdrive.service
```

#### 2. Paste this configuration

```ini
[Unit]
Description=rclone mount Google Drive
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
ExecStartPre=/bin/sleep 10
ExecStart=/usr/bin/rclone mount myremote: %h/gdrive \
    --vfs-cache-mode full \
    --vfs-cache-max-age 72h \
    --vfs-cache-max-size 1G \
    --log-level INFO \
    --log-file=%h/.config/rclone/rclone-mount.log
ExecStop=/bin/fusermount -uz %h/gdrive
Restart=on-failure
RestartSec=30

[Install]
WantedBy=default.target
```

> **Note:** Replace `myremote:` with your actual rclone remote name. `%h` expands to your home directory.

#### 3. Enable and start

```bash
# Reload systemd to pick up the new service
systemctl --user daemon-reload

# Enable it to start on boot
systemctl --user enable rclone-gdrive.service

# Start it now
systemctl --user start rclone-gdrive.service

# IMPORTANT: Allow user services to run without being logged in
sudo loginctl enable-linger $USER
```

#### 4. Check status

```bash
systemctl --user status rclone-gdrive.service
ls ~/gdrive
```

---

### Useful Commands

| Action | Command |
|---|---|
| Check status | `systemctl --user status rclone-gdrive` |
| View logs | `tail -f ~/.config/rclone/rclone-mount.log` |
| Stop mount | `systemctl --user stop rclone-gdrive` |
| Restart | `systemctl --user restart rclone-gdrive` |
| Disable auto-mount | `systemctl --user disable rclone-gdrive` |

---

### Troubleshooting

- **Mount hangs or fails**: Force unmount with `fusermount -uz ~/gdrive`
- **Permission denied on FUSE**: Make sure your user is in the `fuse` group:
  ```bash
  sudo usermod -aG fuse $USER
  ```
  Then log out and back in.
- **"Transport endpoint not connected"**: Unmount and remount:
  ```bash
  fusermount -uz ~/gdrive
  systemctl --user restart rclone-gdrive
  ```
