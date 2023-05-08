
#### 更新
拿到服务器后第一件事，先更新软件源（root登录）。
```bash
apt update -y
```

#### 安装sudo
```bash
apt install sudo
```

#### 创建sudo用户
参考 [创建用户](../General/创建用户.md)

#### ssh登录
在本地将公钥传到服务器上：
```bash
scp id_rsa.pub akari@216.xx.190.103:/home/akari/
```

服务器上看到后写进~/.ssh/authorized_keys
```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

#### 命令别名
比如设置ll命令
```bash
alias ll='ls -l'
```

安装Xray# 八合一共存脚本+伪装站点 https://github.com/mack-a/v2ray-agent

```bash
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```
按照提示输入域名和uuid，最后成功会输出配置

开启bbr（或者使用上面脚本后输入vasma命令按选项安装）
```bash
wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh
chmod 755 /opt/bbr.sh
/opt/bbr.sh 
```

查看信息 
```bash
---------- System Information ----------
 OS      : Debian GNU/Linux 10
 Arch    : x86_64 (64 Bit)
 Kernel  : 4.19.0-6-amd64
----------------------------------------
 Automatically enable TCP BBR script

 URL: https://teddysun.com/489.html
----------------------------------------

Press any key to start...or Press Ctrl+C to cancel

[Info] The kernel version is greater than 4.9, directly setting TCP BBR...
[Info] Setting TCP BBR completed...
root@racknerd-e6af81:~# uname -r
4.19.0-6-amd64
root@racknerd-e6af81:~#  lsmod | grep bbr
tcp_bbr                20480  9


root@racknerd-e6af81:~#  sysctl net.ipv4.tcp_available_congestion_control
net.ipv4.tcp_available_congestion_control = reno cubic bbr
```


#### clash配置

参考 
https://haoel.github.io/
https://github.com/Hackl0us/SS-Rule-Snippet/wiki/clash(X)

下载 https://cdn.jsdelivr.net/gh/Hackl0us/SS-Rule-Snippet@master/LAZY_RULES/clash.yaml 并编辑

