
### 服务端

#### 安装 x11vnc 和 lightdm

```
sudo apt install x11vnc
sudo apt install lightdm
```

安装 lightdm 过程会让选择 display manger，选择 lightdm 就行。

设置密码，根据提示输入， sudo 会在 root/.vnc 下生成 passwd 文件，不加 sudo 则是在用户目录 username/.vnc 下生成 passwd 文件。

```
sudo x11vnc -storepasswd
```

#### 设置 vnc 服务

编辑
```bash
sudo vi /etc/systemd/system/x11vnc.service
```

粘贴内容，home 目录后换成自己的用户
```bash
[Unit]
Description=x11vnc (Remote access)
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -auth guess -display :0 -rfbauth /home/akari/.vnc/passwd -rfbport 5900 -forever -loop -noxdamage -repeat -shared -capslock -nomodtweak
ExecStop=/bin/kill -TERM $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
KillMode=control-group
Restart=on-failure

[Install]
WantedBy=graphical.target

```

#### 启动

安装了 lightdm 需要重启系统，然后就可以启动 vnc 了

``` bash
# 重新加载配置
sudo systemctl daemon-reload

# 启动
sudo systemctl start x11vnc.service 

# 查询状态
sudo systemctl status x11vnc.service
```

查询状态输出如下为正常

```
akari@akari-pc:~$ systemctl status x11vnc.service
\u25cf x11vnc.service - x11vnc (Remote access)
     Loaded: loaded (/etc/systemd/system/x11vnc.service; disabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-10-17 12:58:08 CST; 2h 24min ago
   Main PID: 2447 (x11vnc)
      Tasks: 2 (limit: 19041)
     Memory: 16.1M
     CGroup: /system.slice/x11vnc.service
             \u251c\u25002447 /usr/bin/x11vnc -auth guess -display :0 -rfbauth /home/akari/.vnc/passwd -rfbport 5900 -forever -l>
             \u2514\u25002448 /usr/bin/x11vnc -auth guess -display :0 -rfbauth /home/akari/.vnc/passwd -rfbport 5900 -forever -l>

10\u6708 17 13:05:59 akari-pc x11vnc[2448]: 17/10/2022 13:05:59 Enabling ServerIdentity protocol extension for client 192.>
10\u6708 17 13:05:59 akari-pc x11vnc[2448]: 17/10/2022 13:05:59 Using tight encoding for client 192.168.0.103
10\u6708 17 13:06:01 akari-pc x11vnc[2448]: 17/10/2022 13:06:01 client_set_net: 192.168.0.103  0.5578
10\u6708 17 13:06:07 akari-pc x11vnc[2448]: 17/10/2022 13:06:07 created selwin: 0x4400040
10\u6708 17 13:06:07 akari-pc x11vnc[2448]: 17/10/2022 13:06:07 called initialize_xfixes()
10\u6708 17 13:06:10 akari-pc x11vnc[2448]: 17/10/2022 13:06:10 client 4 network rate 1876.0 KB/sec (29792.4 eff KB/sec)
10\u6708 17 13:06:10 akari-pc x11vnc[2448]: 17/10/2022 13:06:10 client 4 latency:  7.6 ms
10\u6708 17 13:06:10 akari-pc x11vnc[2448]: 17/10/2022 13:06:10 dt1: 0.0142, dt2: 0.0362 dt3: 0.0076 bytes: 87531
10\u6708 17 13:06:10 akari-pc x11vnc[2448]: 17/10/2022 13:06:10 link_rate: LR_LAN - 7 ms, 1876 KB/s
10\u6708 17 15:00:20 akari-pc x11vnc[2448]: 17/10/2022 15:00:20 3*Alt_L, calling: refresh_screen(0)
```


### 客户端

Remmina 或 VNC Viewer

安装 Remmina

```bash
sudo apt install remmina -y
```

选择 Remmina VNC Plugin 协议，输入 ip、用户名和密码就可连接了。