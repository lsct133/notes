For modern NVMe drives running **ext4** on EndeavourOS, the primary optimisations focus on eliminating unnecessary metadata write cycles and tuning journal commitment while avoiding continuous synchronous discard (which degrades drive throughput).

  ### Recommended Optimised `/etc/fstab`

Code snippet

```
# <file system>                            <mount point>          <type>  <options>                                                <dump>  <pass>
UUID=F0FC-9006                             /efi                    vfat    fmask=0137,dmask=0027                                      0       2
UUID=69b792ee-abc9-4c69-b2d3-41783bafe399  /                       ext4    noatime,commit=60                                          0       1
UUID=84114690-f44d-4270-8593-adffc384e851  /home                   ext4    noatime,commit=60                                          0       2
tmpfs                                      /tmp                    tmpfs   defaults,noatime,mode=1777                                 0       0
UUID=26dd3e5e-4ab7-46ab-8d1a-9bc6dbc67699  /run/media/lee/Storage  auto    nosuid,nodev,nofail,noatime,x-gvfs-show                    0       2
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