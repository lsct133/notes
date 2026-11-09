Manage  jurno logs

  1. check jurno size:
&nbsp;&nbsp;&nbsp;&nbsp;journalctl --disk-usage

2. clear all logs:
&nbsp;&nbsp;&nbsp;&nbsp;sudo journalctl --rotate
&nbsp;&nbsp;&nbsp;&nbsp;sudo journalctl --vacuum-time=1s [clears logs older than
1 sec]

3. check logs have cleared:
&nbsp;&nbsp;&nbsp;&nbsp;journalctl --disk-usage

4. remove logs older than 2 weeks:
&nbsp;&nbsp;&nbsp;&nbsp;sudo journalctl --vacuum-time=2weeks
&nbsp;&nbsp;&nbsp;&nbsp;
5. limit jurno size:
&nbsp;&nbsp;&nbsp;&nbsp;SystemMaxUse=100M [100M is changeable]

6. after changes restart jurno:
&nbsp;&nbsp;&nbsp;&nbsp;sudo systemctl restart systemd-journald
