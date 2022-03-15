
# Widows RDP 远程连接 Linux i3wm

环境为 Ubuntu Server 版本。

### xrdp

先安装 xrdp，顺序安装，顺序反过来或者少安装 tightvncserver 的话可能会导致远程桌面输完密码就蓝屏了。搜资料的时候还看到不过我没用，这种东西慎用吧。

```bash
sudo apt-get install tightvncserver
sudo apt-get install xrdp
```

或者用别人提供脚本自动安装 [C-NERGY.BE](http://www.c-nergy.be/products.html)，试过了也可以。

### i3wm
安装 i3wm ，xorg 是桌面环境，dmenu 是程序启动器。

```bash
sudo apt install i3 xorg dmenu
```

i3-gaps 是 i3wm 的分支，提供更多特性，上面可以换成 i3-gaps 安装。
```bash
sudo apt install i3-gaps
```

然后开始配置。执行 `vi ~/.xinitrc` 创建文件，内容：

```bash
#!/bin/sh
# /etc/X11/xinit/xinitrc
# global xinitrc file, used by all X sessions started by xinit (startx)
exec /usr/bin/i3
```

然后打开终端执行 `startx` 命令就能看到 i3wm 了。

下面是配置远程桌面的。

执行 `vi ~/.xsession` 创建文件，内容：

```bash
#!/bin/sh
exec /usr/bin/i3
```

或者直接执行 `echo i3 >~/.xsession`

服务器上终端输入 `startx` 就可以启动 i3 桌面了，Windows 也可以用 rdp 连接 i3 桌面。

因为用的 VirtualBox 虚拟机，rdp 连接后有个错误，未解决。

```
VBoxclient failed to register resizing support, rc=VERR RESOURCE_BUSY
```

### i3wm 其他配置

常用软件
- feh 图像查看器
- neofetch 系统信息显示命令行脚本
- xfce4-terminal 终端
- Conky 系统监视软件
- compton 窗口透明化的工具

```bash
sudo apt-get install feh neofetch xfce4-terminal conky-all compton
```

### 参考
- [1] [Windows10远程桌面无法连接Ubuntu](https://www.jianshu.com/p/a0e1684f00cb)
- [2] [i3 平铺窗口管理器入门](https://bynss.com/linux/760840.html)
