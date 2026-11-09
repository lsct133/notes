Firefox lowering ram use

  > I've contemplated switching to Chrome given the rampant memory use of Firefox
> on Linux with my 64GB RAM laptop triggering the OOM daemon routinely due to
> excessive memory use with Firefox.

My solution on a machine with way less RAM was to run Firefox as a systemd user
service. You can create a new one with e.g. systemctl --user edit --full --force
firefox and fill in something like

[Unit]
Description=Firefox service

[Service]
# add Environment lines to your liking, e.g.
Environment=DISPLAY=:0
Environment=GTK_THEME=Adwaita:dark
Environment=MOZ_ENABLE_WAYLAND=1
ExecStart=/usr/bin/firefox
# tweak when firefox gets the OOM-killer treatment
MemoryMax=3G

[Install]
# set the target you want, or just use default.target
WantedBy=graphical.target

at that point you'll have some more control over when the OOM-killer is
triggered and what it hits. You can do the same thing for other apps that have a
tendency to eat too much memory [https://www.linuxatemyram.com/].