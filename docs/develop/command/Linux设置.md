
# Linux 设置

### 系统安装

分区设置，选择空闲的分区进行分割

```
/boot   主分区 1G
/       逻辑分区 300G
/home   逻辑分区 199G
```

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

### 创建 IDEA 应用图标

编辑 `vi /usr/share/applications/idea.desktop`
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

#### 增强工具

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
sudo ./ubuntu-gdm-set-background --image /home/user/backgrounds/image.jpg
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
