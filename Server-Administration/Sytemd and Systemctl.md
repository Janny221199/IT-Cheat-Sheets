
- `systemctl start XXX` start service XXX (e.g. httpd)
- `systemctl stop XXX` stop service XXX (e.g. httpd)
- `systemctl enable XXX` enable startup of service XXX on system startup
- `systemctl disable XXX` disable startup of service XXX on system startup

# Create custom systemctl service

1. have en executable program e.g. python
2. create a `XXX.service` file in `/etc/systemd/system` with a `[Service]` Tag and specify the start command (`ExecStart=...`) -> you can also specify description, documentation url, restart policy, pre run commands and dependencies etc.
3.  reload systemctl `systemctl daemon-reload`
4. start (`systemctl start XXX`) or enable it on system startup (`systemctl enable XXX`)