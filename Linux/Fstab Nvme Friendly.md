Here is a modern, **NVMe-friendly** version of your `/etc/fstab` (as of 2026).

The main improvements for NVMe SSDs (which already have very low latency and high endurance):

- Add **`noatime`** (or sometimes `relatime` + `nodiratime`) → reduces unnecessary metadata writes on every read access.
- Keep using **UUIDs** (already good).
- **Do not** add `discard` in fstab → most modern distributions (Ubuntu, Fedora, Arch, etc.) recommend **periodic TRIM** via `fstrim.timer` instead, as continuous `discard` can hurt performance in many workloads.
- Root keeps `errors=remount-ro` for safety.
- `/home` and `/boot/efi` get performance-oriented options.
- `/boot/efi` (vfat) usually doesn't need much tweaking, but `noatime` helps slightly.
- Swap stays as-is.

### Recommended updated /etc/fstab

```text
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>                   <dump>  <pass>

# / was on /dev/nvme0n1p3 during installation
UUID=2452521d-2a4e-4d6e-986e-c7be2836eda2 /               ext4    defaults,noatime,commit=60,errors=remount-ro 0 1

# /boot/efi was on /dev/nvme0n1p1 during installation
UUID=F1A2-F08C  /boot/efi       vfat    umask=0077,noatime          0       1

# /home was on /dev/nvme0n1p2 during installation
UUID=56482eb2-00cc-41e2-a64f-cbc86eef1326 /home           ext4    defaults,noatime            0       2

# swap was on /dev/nvme0n1p4 during installation
UUID=3d91c045-c97b-484b-8a0a-e69cfe89894b none            swap    sw                          0       0

# 
UUID=26dd3e5e-4ab7-46ab-8d1a-9bc6dbc67699  /media/Storage  auto  nosuid,nodev,nofail,x-gvfs-show  0  2

LABEL=64USB /mnt/64USB auto nosuid,nodev,nofail,noauto,x-gvfs-show 0 0

```

### Quick explanations of changes

| Mount point | New options added              | Why?                                                                 |
|-------------|--------------------------------|----------------------------------------------------------------------|
| `/`         | `noatime,commit=60`            | No access-time writes + journal flush every 60 s (balance perf/safety) |
| `/home`     | `noatime`                      | Home has many reads; eliminates atime writes without losing much     |
| `/boot/efi` | `noatime`                      | Minor help; EFI partition has very little activity anyway            |
| All ext4    | No `discard`                   | Better to use periodic TRIM (see below)                              |

### Important additional step (TRIM)

Enable periodic TRIM if it isn't already active (most recent Ubuntu / Debian / Arch / Fedora / Pop!_OS do this by default):

```bash
sudo systemctl enable fstrim.timer
sudo systemctl start  fstrim.timer
```

Check status:

```bash
systemctl status fstrim.timer
```

This runs `fstrim` weekly — usually the best compromise for NVMe drives.

### Optional — more aggressive (if you want maximum performance)

If you don't care about access times at all (most desktop users don't):

```text
# root
UUID=... / ext4 defaults,noatime,nodiratime,commit=120,errors=remount-ro 0 1

# home
UUID=... /home ext4 defaults,noatime,nodiratime 0 2
```

`nodiratime` gives a tiny extra gain on directory-heavy workloads.

### Before & after — how to apply safely

1. Make backup:

   ```bash
   sudo cp /etc/fstab /etc/fstab.bak-$(date +%F)
   ```

2. Edit file:

   ```bash
   sudo nano /etc/fstab
   ```

   (or use vim, etc.)

3. Test the new config **without rebooting**:

   ```bash
   sudo mount -a
   ```

   If it returns no error → good. If error → fix before reboot.

4. Reboot only after it passes.

This setup gives good performance + longevity on NVMe while keeping things reliable.