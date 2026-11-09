Optimise Debian 13

  ENABLE ADDITIONAL REPOSITORY AREAS

Debian’s package repositories are organised into separate areas (or components)
like main contrib non-free non-free-firmware, etc.
 * non-free contains packages whose licences are not FOSS
 * contrib contains dependencies of non-free packages

contrib and non-free are not enabled on a Debian system since they contain
non-FOSS packages. You can enable the non-free areas to get access to additional
packages such as nvidia-drivers, aac-enc, faac, etc

Edit /etc/apt/sources.list as shown below:

deb http://deb.debian.org/debian/ trixie main contrib non-free non-free-firmware
deb http://security.debian.org/ trixie-security main contrib non-free non-free-firmware
deb http://deb.debian.org/debian/ trixie-updates main contrib non-free non-free-firmware
deb http://deb.debian.org/debian/ trixie-backports main contrib non-free non-free-firmware

#deb-src http://deb.debian.org/debian/ trixie main contrib non-free non-free-firmware
#deb-src http://security.debian.org/ trixie-security main contrib non-free non-free-firmware
#deb-src http://deb.debian.org/debian/ trixie-updates main contrib non-free non-free-firmware
#deb-src http://deb.debian.org/debian/ trixie-backports main contrib non-free non-free-firmware


ADD SBIN FOLDERS TO YOUR PATH

If you try to reboot your Debian system by typing reboot in the terminal you
will get the following error:

Debian has a long-standing tradition of separating system administration tools
from regular user programs. Regular users don’t have /usr/sbin in their
environment’s PATH variable. This means that your terminal doesn’t know where to
find the executable file for the reboot command when you type reboot in a
terminal. You need to type the full path to make it work.

This can be inconvenient when you are the only user on your computer and wish to
run administrative tools without jumping through hoops. Add a single line at the
end of your bashrc file (~/.bashrc) to add the sbin folders to your PATH.

export PATH="${PATH}:/usr/local/sbin:/usr/sbin:/sbin"


ADD YOURSELF TO SYSTEM GROUPS

Debian adds new users to a limited set of system groups in order to restrict
their privileges. This is good for security, but inconvenient when you are the
only user on your computer.

You can check the groups you belong to using the groups command:

If you are the admin for your computer, you can add yourself to some optional
groups to make your life easier.

sudo usermod -aG adm,crontab,input,kvm,lpadmin,render,staff $USER

What these groups do:

GROUP       PURPOSE
adm         Access log files in /var/log and system monitoring tasks
            without needing sudo
crontab     Running the crontab command to view, create, and edit
            scheduled jobs
input       Changing some input settings (mouse and keyboard) without
            needing sudo
kvm         For running VMs using Virt-Manager and KVM
lpadmin     Manage printers and print jobs without needing sudo
render      Give access to a rendering device like graphic cards
staff       Add and modify files in /usr/local without needing sudo
vboxsf      Accessing shared folders from inside the guest OS in
            VirtualBox VMs
vboxusers   USB pass-through (connecting USB drives to VirtualBox VMs)


USE CLOUDFLARE DNS FOR QUICKER WEB BROWSING

A DNS server acts like a phone book for the internet. When you type a website
address like google.com into your browser, your computer doesn’t know where to
find it. It first sends a request to a DNS server, asking for the website’s
numerical IP address (e.g., 142.250.191.46). The DNS server then provides that
address, allowing your computer to connect to the correct web server and load
the page.

An installation of Debian 13, by default, uses DNS servers of your local ISP
which are usually very slow. Google
[https://developers.google.com/speed/public-dns] and Cloudflare
[https://www.cloudflare.com/learning/dns/what-is-1.1.1.1/] provide public DNS
servers that are much faster.

A fast DNS server can make your internet feel quicker because it reduces the
time it takes to find the website’s address before any content can even begin to
load.

To use Cloudflare DNS servers on your system, edit the file
/etc/systemd/resolved.conf and add 2 lines for DNS and FallbackDNS under the
section [Resolve].

[Resolve]
DNS=1.1.1.1#cloudflare-dns.com 1.0.0.1#cloudflare-dns.com 2606:4700:4700::1111#cloudflare-dns.com 2606:4700:4700::1001#cloudflare-dns.com
FallbackDNS=8.8.8.8#dns.google 8.8.4.4#dns.google 2001:4860:4860::8888#dns.google 2001:4860:4860::8844#dns.google

Restart the service systemd-resolved to apply the changes:

sudo systemctl restart systemd-resolved

Check dns in use

cat /etc/resolv.conf
or
systemd-resolve --status
or
nmcli dev show | grep DNS


SWITCH TO LOCAL MIRRORS FOR FASTER PACKAGES DOWNLOADS

Debian’s update servers have mirrors worldwide, so choosing one closer to your
geographical location can lead to much faster package downloads. Switching to a
local mirror will let you maximize your internet speed and get updates as
quickly as possible.

There are many ways to select and use the fastest mirrors. We will install and
use Nala.

Nala is a prettier frontend for APT that makes it easier to install packages
from the command line. It has the same syntax as APT. Run the command given
below to install Nala from its official repository.

Run the command sudo nala fetch to search for the fastest mirrors in your
geographic region.

When prompted for a choice enter 1 2 (one-space-two) and hit Enter. It will use
the first two fastest mirrors in your region.


ENABLE SNAP AND FLATPAK

Debian is a stable distribution where the software in each release remains
unchanged throughout its 5-year lifespan. Packages do not receive major updates
or new features - only bugfixes and security patches.

While this is great for having a stable and reliable system, it is not so great
when you want to use the latest and greatest software. That’s where Snap and
Flatpak come to the rescue.

Run the following commands to add support for Snap and Flatpak packages:

# snap
sudo apt install snapd

# flatpak
sudo apt install flatpak
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

You can search and install software using syntax similar to Apt.

sudo snap search mission-center
sudo snap install mission-center

I prefer Snaps over Flatpaks since they take up less disk space. Each snap
package is an XZ-compressed archive that is mounted to a loop device and used
directly, instead of unpacking the files and wasting disk space.


INSTALL GNOME SOFTWARE CENTER

Linux lacks a good FOSS GUI for browsing and installing Flatpak and Snap
packages (not considering paid apps like Appy [https://teejeetech.com/appy/]
which can do all this and more). GNOME Software is the closest option available.

Run the following commands to install the Snap and Flatpak plugins for GNOME
software.

sudo apt install gnome-software
sudo apt install gnome-software-plugin-flatpak
sudo apt install gnome-software-plugin-snap

The first time you open GNOME Software, you’ll need to wait a minute for it to
download package information and app icons. Once the initial download is
complete, your experience will be much smoother.

GNOME Software installs software updates in offline mode when you shutdown your
computer. This is similar to how updates work on Windows and can be extremely
annoying as a Linux user.

Disable offline updates with the following command:

dconf load -f / &lt;&lt;EOM
[org/gnome/software]
allow-updates=false
EOM

This will also hide the Updates tab in GNOME Software’s main window.


USE SYNAPTIC FOR APT PACKAGES

Synaptic is a classic GUI tool for managing Apt packages. It helps you browse
through the entire catalog of packages in your Debian or Ubuntu repository.

Install Synaptic with the following command.

sudo apt install synaptic

Add a shortcut to your desktop for the next time that you want to install or
remove packages.

--------------------------------------------------------------------------------

SEE ALSO

 * Asmi Linux 13 Debian Edition [https://teejeetech.com/2025/08/09/asmi_13/]
 * GeanyPad - Use Geany as a simple text editor
   [https://teejeetech.com/2025/07/12/geanypad/]

--------------------------------------------------------------------------------

Clipped from Optimizing Debian 13 Desktop
[https://teejeetech.com/2025/08/14/debian_13_tips/]

