
# Widows RDP 远程连接 Linux

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

### X11 Fowarding 

X11 图形界面转发是在 Linux 下启动 GUI 程序更便捷的一种方式。

编辑 `vi /etc/ssh/sshd_config` 把 `X11Forwarding yes` 注释去掉，重启 sshd 服务。

然后在你的 ssh 客户端软件开启 X11 转发功能，大部分 ssh 客户端都带有这个功能，比如 PuTTY 和 XShell，我现在在用的 WindTerm 都行。

最后登录 ssh，以命令启动 GUI 程序就行了。

### 输入法

用 X11 转发的时候你会发现原本在桌面环境下运行正常的中文输入法切换不出来。必须在 Linux 端安装输入法和 dbus, 然后用 dbus-launch 启动一个 dbus session, 并且让 GUI 和输入法同时得到 DBUS_SESSION_BUS_ADDRESS 的环境变量,这样输入法才能使用。

先卸掉 IBus，因为会和 fcitx 冲突，其实在安装搜狗输入法的时候官方文档就说明要卸载了。

```bash
sudo apt purge ibus
```

安装所需要的软件包 dbus-x11（实现输入法与 GUI 应用程序之间的通信，必需）、im-config （GUI 输入法配置）、fcitx （输入法框架）、fcitx-sunpinyin （智能拼音输入法） 和 fcitx-table-wubi （五笔输入法），五笔和智拼用不着就不用装了。

```bash
sudo apt install dbus-x11 fcitx im-config fcitx-sunpinyin fcitx-table-wubi
```

配置输入法，把想要输入法比如搜狗或五笔添加上，常用的移动到第一位置。

```bash
fcitx-config-gtk3
```

设置默认 GUI 输入法：
```bash
sudo im-config -n fcitx
```

修改 ~/.profile 文件：
```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export DefaultIMModule=fcitx
fcitx-autostart &>/dev/null
```

执行生效：
```bash
source ~/.profile
```

重启系统，打开 gedit 或其他可输入 GUI，Ctrl+Space 应该就能切换出你配置的输入法了。

但是实际使用还是有一些问题，比如我运行 idea，切换出搜狗，输入的时候输入法是不会跟随光标位置的，而且打字输入快了就失效只能输入英文，只能 Ctrl+Space 再切换几下回来。总结就是体验太糟糕了，X11 转发的 GUI 能不要中文输入法就尽量不要用，注释干脆全写英文得了，锻炼锻炼英文。

### 参考
- [1] [Windows10远程桌面无法连接Ubuntu](https://www.jianshu.com/p/a0e1684f00cb)
- [2] [i3 平铺窗口管理器入门](https://bynss.com/linux/760840.html)
- [3] [WSL2 中文输入法无效](https://www.v2ex.com/t/737747)
- [4] [【谷月老师讲WPS】用 Windows 11 的 WSL 安装 WPS for Linux](https://www.mdnice.com/writing/ddc2298afc224161be99573adda0f18b)