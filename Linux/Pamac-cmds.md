Pamac cmds

  List of common pamac commands


BASIC CMDS:

- Update pkg database: pamac update
- Install pkg: pamac install pkg_name
- Remove pkg: pamac remove pkg_name
- Remove pkg &amp; depends: pamac remove --orphans
- Search for pkg: pamac search pkg_name
- List installed pkgs: pamac list --installed

ADVANCED CMDS:

- Upgrade all pkgs: pamac upgrade
- Show details of pkg: pamac info pkg_name
- Clean pkg cache: pamac clean
- Check orphaned pkgs: pamac orphaned
- Remove orphaned pkgs: pamac remove --orphans
- Force remove pkgs: pacman -Rd --nodeps pkg-name


OTHER USEFUL CMDS:

- List available updates: pamac list --upgradable
- Show history of transactions: pamac history
- Enable AUR: pamac config set aur.enable true
- Install pkg from AUR: pamac build pkg_name
