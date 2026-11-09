bash_functions

  # Generate random 16 character passwords
genpwd() {
    &lt; /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-16}; echo;
}

# Install pkgs using fzf
install() {
    apt-cache pkgnames | fzf --multi --preview 'apt show {1}' | xargs -r sudo apt install
}

# Remove pkgs using fzf
remove() {
    dpkg --get-selections | awk '{print $1}' | fzf --multi --preview 'apt show {1}' | xargs -r sudo apt remove --purge
}

# Show recently installed pkgs
recent() {
    dpkg-query -W --showformat='${Package}\t${Version}\t${Installed-Size}\n' | sort | tail -100
}

