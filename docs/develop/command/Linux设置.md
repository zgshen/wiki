
# Linux 设置

### 系统安装

分区设置，选择空闲的分区进行分割

```
/boot   主分区 1G
/       逻辑分区 300G
/home   逻辑分区 199G
```

boot 直接给个1G，太小的话升级会出问题，home 放自己的文件，分出来重装省事点。swap 分区不需要，Ubuntu 会默认创建一个2G的 swapfile 来用的。

最后注意选择 boot 所在的分区作为启动引导其设备（Device for boot loader installation）。

安装参考 [WIN10安装Ubuntu20双系统](https://blog.csdn.net/qq_33187136/article/details/113924497)

### 字体和主题设置

安装桌面配置工具
```bash
sudo apt install gnome-tweaks
```

然后打开 Tweaks 就可以设置主题（Appearance-Application）或者字体（Fonts）。

### 安装 JDK

```bash
sudo apt-get install openjdk-11-jdk openjdk-11-source
sudo apt-get install openjdk-17-jdk openjdk-17-source
```

### 创建 IDEA 和 Obsidian 应用图标

编辑 `sudo vi /usr/share/applications/idea.desktop`
```bash
[Desktop Entry]
Name=IntelliJ IDEA
Comment=IntelliJ IDEA
Exec=/home/nathan/app/idea-IC-213.6777.52/bin/idea.sh
Icon=/home/nathan/app/idea-IC-213.6777.52/bin/idea.svg
Terminal=false
Type=Application
Categories=Developer;
```

编辑 `sudo vi /usr/share/applications/obsidian.desktop`
```bash
[Desktop Entry]
Name=Obsidian
Comment=Obsidian
Exec=/home/nathan/app/obsidian/Obsidian-0.14.2.AppImage
Icon=/home/nathan/app/obsidian/favicon.ico
Terminal=false
Type=Application
Categories=Developer;

### 安装 VirtualBox

下载 deb 文件安装
```bash
sudo dpkg -i virtualbox-6.1_6.1.32-149290_Ubuntu_eoan_amd64.deb
```

然后可能会看到缺少依赖，virtualbox 也打不开

```bash
# 自动安装缺少的依赖
sudo apt-get -f -y install
# 重新再装一次
sudo dpkg -i virtualbox-6.1_6.1.32-149290_Ubuntu_eoan_amd64.deb
```

sudo apt-get -f install,是修复依赖关系(depends)的命令,就是假如你的系统上有某个package不满足依赖条件,这个命令就会自动修复,安装那个package依赖的package。

新建虚拟机的时候碰到内核错误 Kernel driver not installed (rc=-1908) ，根据报错提示，执行

```bash
nathan@nathan-tp:~/Downloads$ sudo /sbin/vboxconfig 
[sudo] password for nathan: 
vboxdrv.sh: Stopping VirtualBox services.
vboxdrv.sh: Starting VirtualBox services.
vboxdrv.sh: Building VirtualBox kernel modules.
This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.
This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.

There were problems setting up VirtualBox.  To re-start the set-up process, run
  /sbin/vboxconfig
as root.  If your system is using EFI Secure Boot you may need to sign the
kernel modules (vboxdrv, vboxnetflt, vboxnetadp, vboxpci) before you can load
them. Please see your Linux system's documentation for more information.
```

gcc make perl 没装，装上
```bash
sudo apt-get install gcc make perl
```

再执行可以了
```bash
nathan@nathan-tp:~/Downloads$ sudo /sbin/vboxconfig 
vboxdrv.sh: Stopping VirtualBox services.
vboxdrv.sh: Starting VirtualBox services.
vboxdrv.sh: Building VirtualBox kernel modules.
```

### 增强工具

Deviced-insert Guest Additions CD image 增强工具点了没反应，直接到 http://download.virtualbox.org/virtualbox/ 下载对应版本的 addition，如 VBoxGuestAdditions_6.1.2.iso，打开选择对应系统版本安装。

完毕重启后发现分辨率变高了，不再是 600*800 了。

全屏与非全屏切换：Ctrl+F, 需要注意是右手边的Ctrl键。

另外，还有其他几个可以记住：

- 切换到全屏模式：Ctrl + F
- 切换到无缝模式：Ctrl + L
- 切换到比例模式：Ctrl + C
- 显示控制菜单 ：Ctrl + Home

记住，一定是右边的 Ctrl 键，这个键也是切换回本机的功能键，先按一下右 Ctrl，再按 Alt+Tab 就回到本机应用了，而不是在虚拟机里切换应用。

这样操作后可能分辨率还是不够，可以尝试吧显存调大些 Display-Screen-Video Memory 设置为 64MB，这个要关闭虚拟机时候才能改。

或者尝试点击 View-Adjust Wnidow size，让分辨率自适应。或者点击 View-Virtual Screen1 选择想要的分辨。

### 更改登录紫色背景

手动比较麻烦，github 有人写了个脚本 [PRATAP-KUMAR](https://github.com/PRATAP-KUMAR/ubuntu-gdm-set-background)

下载解压后执行命令设置，详细见 GitHub 说明
```bash
# 背景设置为图片
sudo ./ubuntu-gdm-set-background --image /home/user/backgrounds/image.jpg
# 背景设置颜色
sudo ./ubuntu-gdm-set-background --color \#aAbBcC
```
缺的包安装
```bash
sudo apt install libglib2.0-dev-bin
```

### dock 工具

卸载自带 dock
```bash
sudo apt-get autoremove --purge gnome-shell-extension-ubuntu-dock -y
```

### Chrome 字体

Minimum 调到14，根据需要设置，最小字体能让有些字体太小的网站容易看。

Standard font 字体设置为 Noto Sans Mono CJK SC（其实就是思源黑体），可以解决有些网站中文字体大小不一的情况。

### 删除桌面快捷方式

打开 Tweaks（优化），点击 Extensions-Desktop icons 设置按钮，把不要的桌面图标取消选中就行。

### 监控插件

[Vitals](https://extensions.gnome.org/extension/1460/vitals/)
或者
[indicator-sysmonitor](https://github.com/fossfreedom/indicator-sysmonitor)

### 顶栏透明

[transparent-topbar](https://extensions.gnome.org/extension/1765/transparent-topbar/)

### 设置截图快捷键

Setting->Devices->Keyboard 打开快捷键设置，拉倒最下面加号点击添加快捷键

Name: open gnome screenshot  
Command: gnome-screenshot -i  
Shortcut: 按 Shift+Super（win）+S，就跟 Windows 下的截图一样了

### 云同步

文件云同步有 Linux 版的有 Dropbox 或者坚果云，Dropbox 是被墙的，用得少的话坚果云就够用了。坚果云在 Ubuntu 下要用源代码编译安装，deb 方式安装的打不开的，版本比较老了，地址 https://www.jianguoyun.com/s/downloads/linux

### 番茄钟

gnome 插件
[Time ++](https://extensions.gnome.org/extension/1238/time/)

### 全局去除 title bar 插件


### 禁用 swap

安装系统的时候没有设置 swap 分区，但是 Ubuntu 还是会默认在根目录下给系统创建了一个2G的 swapfile。
```bash
# nathan @ nathan-tp in ~ [20:06:59]
$ free -h
              total        used        free      shared  buff/cache   available
Mem:           31Gi       3.8Gi        22Gi       605Mi       4.5Gi        26Gi
Swap:         2.0Gi          0B       2.0Gi

# nathan @ nathan-tp in / [21:42:22] 
$ ll | grep swap
-rw-------   1 root root 2.0G Mar 25 02:11 swapfile
```

内存多的就不用开 swap 了，关闭命令：
```bash
# disable all swaps from /proc/swaps
sudo swapoff -a

# or execute this commond
sudo swapoff /swapfile 
```

没了
```bash
# nathan @ nathan-tp in ~ [21:55:17] 
$ free -h
              total        used        free      shared  buff/cache   available
Mem:           31Gi       4.5Gi        21Gi       780Mi       4.8Gi        25Gi
Swap:            0B          0B          0B
```

修改配置：
```bash
sudo vi /etc/fstab
```

注释 /swapfile 这一行;
```bash
#/swapfile                                 none            swap    sw              0       0
```

最后把 swapfile 删了：
```bash
sudo rm /swapfile
```

### 自动挂载磁盘

运行如下命令查看硬盘分区：

sudo blkid
看输出找到自己需要挂载的盘，比如这里要挂载的是 c 盘 /dev/sdb1 和 d 盘 /dev/sda2

```
/dev/sdb1: LABEL="win-c" UUID="96A8CBEBA8CBC7C7" TYPE="ntfs" PARTUUID="995606b6-6b82-7ed4-84e7-007d680f31d3"
/dev/sdb2: UUID="0C38-06FF" TYPE="vfat" PARTUUID="dc885421-8944-2273-207d-3685601c7169"
/dev/sda1: PARTLABEL="Microsoft reserved partition" PARTUUID="140f0f24-f5ff-4ea3-b95e-cd12128e0d58"
/dev/sda2: LABEL="win-d" UUID="04BECAB2BECA9C14" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="9a67d1d2-7618-4173-a9d8-d05bf45e924f"
```

修改配置文件 sudo vi /etc/fstab 最后加上

```
# Windows disks
/dev/sdb1                      /media/nathan/win-c
/dev/sda2                      /media/nathan/win-d
重启系统完事
```