# Systemd 守护线程

/etc/systemd/system/nyancat.socket

```json
[Unit]
Description=Nyan Cat Telnet Server Socket

[Socket]
ListenStream=23
Accept=yes

[Install]
WantedBy=sockets.target
```

/etc/systemd/system/nyancat@.service

```json
[Unit]
Description=Nyan Cat Telnet Server
After=syslog.target auditd.service

[Service]
ExecStart=-/usr/games/bin/nyancat -t
StandardInput=socket
StandardError=syslog
IgnoreSIGPIPE=no
User=games
```

```json
# systemctl daemon-reload
# systemctl start nyancat.socket
# systemctl enable nyancat.socket
```

`telnet -t vtnt 129.204.18.28 1234`