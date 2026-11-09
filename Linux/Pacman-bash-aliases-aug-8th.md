Pacman bash aliases aug 8th

  # List of common pacman commands
# Pacman commands follow this structure
# pacman [options] [operation] [pkg_name]

# Update / upgrade:
alias update='sudo pacman -Syu'  # Update system and all pkgs
alias sync='sudo pacman -Sy'     # Sync pkg databases
alias upgrade='sudo pacman -Su'  # Upgrade system but not pkg databases

# Pkg Install:
alias in='sudo pacman -S'             # Install specific pkg
alias insl='sudo pacman -U'           # Install pkg from local file
alias force='sudo pacman -S --needed' # force reinstalling a pkg (&lt;pkg_name&gt; --needed)

# Pkg Removal:
alias rem='sudo pacman -R'            # Remove a specific pkg
alias remd='sudo pacman -Rs'          # Remove pkg and depends not req
alias remn='sudo pacman -Rns'         # Remove pkg and all depends
alias clean='sudo paccache -ruk1'     # Clean cached pkgs but keep 1 copy
alias dryrun='sudo paccache -ruk1 -n' # Dry run above cmd

# Pkg Info:
alias list='pacman -Q'       # List installed pkgs
alias find='pacman -Q'       # Find a specific pkg &lt;pkg_name&gt;
alias info='pacman -Qi'      # Display info about pkg
alias orphaned='pacman -Qdt' # List orphaned pkgs (installed as depends, not req)
alias explicit='pacman -Qe'  # List explicitly installed pkgs

# Pkg Search:
alias installed='pacman -Qs' # Search for installed pkg
alias search='pacman -Ss'    # Search for pkg in repos
alias ys='yay -Ss'           # Search for pkg in yay

# Pkg Mgmt:
alias conflict='pacman -Qk'     # Check for pkg conflicts
alias mark='pacman -D --asdeps' # Mark pkg as a dependency

# AUR:
alias mkp='makepkg -s'      # Build pkg ( -s flag installs depends)
alias mkpi='sudo pacman -U' # Install (pkg_name.pkg.tar.zst)


# Additional Suggestions:
#alias clean='sudo pacman -Rns $(pacman -Qdtq)' # Remove orphaned pkgs
#alias cache_clean='sudo pacman -Sc'            # Clean pkg cache
#alias search='pacman -Qsq'                     # Search for installed pkg by name
alias top='history | awk "{ if (\$2 != \"\"){print \$2} else {print \$3} }" | sort | uniq -c | sort -nr | head -5'                        # Show top used cmds

# Locked Database:
alias nolock='sudo rm /var/lib/pacman/db.lck' # Remove file lock

# Keyring Issues:
alias init='sudo pacman-key --init'
alias populate='sudo pacman-key --populate archlinux'

# Terminal:
alias h='history'                  # Show cmd history
alias c='clear'                    # Clear terminal
alias al='alias'                   # Show aliases

# Avoid mistakes with cmds
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Fix typo's
alias cd..="cd .."
alias suda='sudo'
alias suod='sudo'
alias sudp='sudo'
alias downlaod='download'


# Get iPlayer
alias gc='get_iplayer --cache'      # Update/create all programmes cache
alias gr='get_iplayer --refresh'    # Refesh for added programmes
#alias gd='get_iplayer --pid='      # Download programmes

# Git cmds
alias gp='git pull'
alias gc='git clone'

# Journal Logs
alias jcd='journalctl --disk-usage'            # Check disk usage
alias jcl='sudo journalctl --vacuum-size=1K'   # Clean logs regardless of age
alias jurno='journalctl --user -b0 --no-pager' # display all userlogs from current boot
alias jurr='sudo journalctl -r'                # Logs in reverse-recent fisrt
#sudo journalctl -r -n 50                      # Show only last 50 entries
#sudo journalctl -r -u ssh.service             # View logs for specific service
#sudo journalctl -f -r                         # Continuous display new entries
#sudo journal -f                               # Show output of unresponsive app

# ncdu
alias ncduroot='konsole -e ncdu -r --color dark /'

# Misc
alias du='du -h --max-depth=1' # Disk usage

# Listing files
alias ll='ls -la'          # List files with details
alias la='ls -A'           # List files except hidden
alias l='ls -CF'           # List files in columns
alias ls='ls --color=auto' # Colorized output for ls
