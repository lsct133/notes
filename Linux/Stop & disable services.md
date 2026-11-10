Stop & disable services

  List all active and running services, use systemctl cmd:

sudo systemctl list-units --type=service --state=running

Check Open Ports and I.D Listening Services:

sudo ss -tuln

These cmds display all TCP and UDP ports currently open and waiting for
connections. For example, if you spot port 21 (used by FTP) open but you don’t
actually need FTP, it’s a clear sign you should disable that service to keep
your server secure and resource-friendly.


Common services you might not need:
 * avahi-daemon: Zero-config networking (local service discovery).
 * bluetooth.service: Bluetooth - useless on headless servers.
 * iscsi.service, iscsid.socket, iscsiuio.socket: For iSCSI network storage.
 * lvm2-monitor.service, lvm2-lvmpolld.socket: For managing LVM volumes.
 * mdmonitor.service, raid-check.timer: Related to software RAID.
 * qemu-guest-agent.service: Useful if running inside QEMU/KVM VM.
 * nfs-convert.service, nfs-client.target: For NFS (networked file systems).
 * cups.service: Printing services - likely useless on a server.
 * postfix.service: Mail server - unnecessary unless sending mail.
 * sssd.service: Connects to domains like LDAP or AD.
 * hyperv-daemon: Only needed inside Hyper-V virtual machines.
 * apport.service: Ubuntu error reporting tool.
 * zeitgeist, telepathy: Desktop logging and messaging services - not for
   servers.


Use these cmds below to disable the services listed above:



sudo systemctl disable avahi-daemon
sudo systemctl disable bluetooth
sudo systemctl disable iscsi
sudo systemctl disable iscsid.socket
sudo systemctl disable iscsiuio.socket
sudo systemctl disable lvm2-monitor
sudo systemctl disable lvm2-lvmpolld.socket
sudo systemctl disable mdmonitor
sudo systemctl disable raid-check.timer
sudo systemctl disable qemu-guest-agent
sudo systemctl disable nfs-convert
sudo systemctl disable nfs-client.target
sudo systemctl disable cups
sudo systemctl disable postfix
sudo systemctl disable sssd
sudo systemctl disable hyperv-daemons
sudo systemctl disable apport
sudo systemctl disable zeitgeist
sudo systemctl disable telepathy
sudo systemctl disable smb
sudo systemctl disable grub-btrfsd
sudo systemctl disable systemd-swap


To stop a service being triggered after disabling:

sudo systemctl mask service-name

