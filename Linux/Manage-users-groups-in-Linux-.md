Manage users groups in Linux 

  QUICK-START

groups


OUTPUT EXAMPLE ( LEE IS THE ONLY USER, SO NO OTHERS WILL APPEAR )

lee sys network floppy scanner power vboxsf realtime
video lp input audio wheel cdrom dip plugdev netdev
lpadmin bluetooth scard autologin


GROUP MANAGEMENT COMMANDS


1. GROUPADD

This command is used to create a new group.

sudo groupadd group_name


2. GROUPDEL

This command deletes an existing group.

sudo groupdel group_name


3. GROUPMOD

This command modifies an existing group, such as changing its name or GID (Group
ID).

sudo groupmod -n new_group_name old_group_name


4. GROUPS

This command displays the groups a user belongs to.

groups username


5. GPASSWD

This command is used to administer /etc/group and /etc/gshadow. It can add or
remove users from a group.

sudo gpasswd -a username group_name  # Add user to group
sudo gpasswd -d username group_name  # Remove user from group


6. ID

This command shows the user ID (UID) and group ID (GID) of a user, along with
the groups they belong to.

id username


UNDERSTANDING GROUPS

In Linux, groups are used to manage permissions and access control for multiple
users. By assigning users to groups, you can easily manage file permissions and
access rights for a set of users rather than individually.

