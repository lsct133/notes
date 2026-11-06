For modern NVMe drives running **ext4** on EndeavourOS, the primary optimizations focus on eliminating unnecessary metadata write cycles and tuning journal commitment while avoiding continuous synchronous discard (which degrades drive throughput).

  

### Recommended Optimized `/etc/fstab`

Code snippet

```
# <file system>                            <mount point>            <type>  <options>                                                   <dump>  <pass>
UUID=F0FC-9006                             /efi                     vfat    fmask=0137,dmask=0027                                       0       2
UUID=69b792ee-abc9-4c69-b2d3-41783bafe399 /                        ext4    noatime,commit=60                                           0       1
UUID=84114690-f44d-4270-8593-adffc384e851 /home                    ext4    noatime,commit=60                                           0       2
tmpfs                                      /tmp                     tmpfs   defaults,noatime,mode=1777                                  0       0
UUID=26dd3e5e-4ab7-46ab-8d1a-9bc6dbc67699 /run/media/lee/Storage   auto    nosuid,nodev,nofail,noatime,x-gvfs-show                     0       2
```

### What Was Changed & Why

1.  **Retained `noatime` (and added it to `Storage`):**
    
      
    -   Disables writing access timestamps every time a file or directory is read, cutting down flash write wear significantly.
        
          
        
2.  **Added `commit=60` to ext4 partitions:**
    
      
    -   Ext4 flushes dirty metadata and data to disk every 5 seconds by default. Raising this to `60` batches journal updates, substantially decreasing write amplification on flash memory during background tasks.
        
          
        
3.  **Deliberately omitted `discard`:**
    
      
    -   Modern NVMe firmware handles TRIM far better in batches than via continuous inline discard (`discard` mount option), which can introduce I/O latency spikes on file deletions. Instead, enable periodic batch trimming via systemd:
        
          
        
        Bash
        
        ```
        sudo systemctl enable --now fstrim.timer
        ```
        
        _Verification:_ Running `systemctl is-active fstrim.timer` should output `active`.
        
          
        
4.  **Kept default `data=ordered`:**
    
      
    -   While `data=writeback` offers slightly higher throughput, `ordered` ensures file data is written before metadata is committed, protecting against corrupted zero-length files in the event of an unexpected crash or power cut.
        
          
        

### Applying and Verifying the Changes

To test the configuration without rebooting or risking a failed boot:

  

1.  Edit `/etc/fstab` with root privileges:
    
      
    
    Bash
    
    ```
    sudo nano /etc/fstab
    ```
    
2.  Test mount points for syntax errors:
    
      
    
    Bash
    
    ```
    sudo mount -a
    ```
    
    _Verification:_ No output should be returned. If errors or warnings appear, fix them before rebooting.
    
      
    
3.  Reload systemd daemon to pick up the updated fstab:
    
    Bash
    
    ```
    sudo systemctl daemon-reload
    ```


---

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