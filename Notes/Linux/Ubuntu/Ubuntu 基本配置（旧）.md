# Ubuntu 基本配置（旧）

上手先设置下 vi 吧，Ubuntu 默认的 vi 使用方向键和退格键有问题，可以通过修改配置或安装 full 版本的 vi 解决。

编辑 /etc/vim 下的 vimrc.tiny 文件 `sudo vi /etc/vim/vimrc.tiny`将 set compatible 改成 set nocompatible ，然后添加 set backspace=2 即可解决。

或者卸载旧版 vi 装新的

```bash
sudo apt-get remove vim-common
sudo apt-get install vim-common
```

### 1. VS Code 问题

1."Visual Studio Code is unable to watch for file changes in this large workspace" (error ENOSPC)

```bash
cat /proc/sys/fs/inotify/max_user_watches
```

The limit can be increased to its maximum by editing `/etc/sysctl.conf` (except on Arch Linux, read below) and adding this line to the end of the file:

```bash
fs.inotify.max_user_watches=524288
```

The new value can then be loaded in by running `sudo sysctl -p`.

### 2. 梯子设置

下载

[https://github.com/Qv2ray/Qv2ray](https://github.com/Qv2ray/Qv2ray)                deb file

[https://github.com/v2ray/v2ray-core/](https://github.com/v2ray/v2ray-core/releases/)            v2ray-linux-64.zip

	```bash
unzip v2ray-linux-64.zip
sudo apt install  Qv2ray.....deb
```

点击分组，添加订阅，输入订阅地址更新订阅

首选项-内核设置，填入 v2ray 解压文件的路径

V2Ray  核心可执行文件路径 `/home/nathan/app/v2ray/v2ray` 

V2Ray 资源目录 `/home/nathan/app/v2ray/` 

### 3. 输入法设置

**IBus（IDEA 下输入框有问题）**

安装 rime `sudo apt-get install ibus-rime`

安装输入法方案，有很多，双拼全拼还是五笔自己选一个 [https://github.com/rime/home/wiki/RimeWithIBus#ubuntu](https://github.com/rime/home/wiki/RimeWithIBus#ubuntu)

`sudo apt-get install librime-data-pinyin-simp`

另一种方法推荐使用 [/plum/](https://github.com/rime/plum) 安裝最新版本

创建配置文件 `vi ~/.config/ibus/rime/default.custom.yaml`

```jsx
patch:
  "menu/page_size": 9 #每页候选词个数
  "ascii_composer/switch_key/Shift_L": commit_code #左shift提交字母
  "style/color_scheme": dota_2
```

创建配置文件 `vi ~/.config/ibus/rime/build/ibus_rime.yaml`

```bash
# 内容如下
style:
  horizontal: true #横向候选框
```

重新部署即可生效

或者重启 ibus-deamon，执行 `ibus restart`

其他设置看官方文档 [https://github.com/rime/home/wiki/CustomizationGuide](https://github.com/rime/home/wiki/CustomizationGuide)

扩充词库

找找下载别人的词库文件，格式类似 luna_pinyin.chat.dict.yaml，以 dict.yml 结尾

新建编辑 luna_pinyin.extended.dict.yaml 文件，import_tables 下的列表就是词库列表名

```bash
# Rime dictionary
# encoding: utf-8

---
name: luna_pinyin.extended
version: "2021.08.12"
sort: by_weight
use_preset_vocabulary: true
import_tables:
  - luna_pinyin
  - luna_pinyin.extra_hanzi
```

创建 luna_pinyin_simp.custom.yaml，translator/dictionary 设 luna_pinyin.extended

```bash
patch:
  switches:
    - name: ascii_mode
      reset: 1                                          # 默认英文
      states: ["中", "英"]
    - name: full_shape
      reset: 0                                          # 默认半角
      states: ["半", "全"]
    - name: zh_simp
      reset: 1                                          # 预设简体
      states: ["繁", "简"]
    - name: ascii_punct
      states: ["。，", "．，"]
    - options: [utf8, gbk]                              # 字符集
      reset: 1                                          # 默认 GBK
      states: ["utf8", "gbk"]

  "engine/filters/@next": charset_filter@gbk            # 默认 GBK
  "engine/translators/@next": reverse_lookup_translator

  translator:
    dictionary: luna_pinyin.extended
    prism: luna_pinyin_simp

  "speller/algebra/@before 0": xform/^([b-df-hj-np-tv-z])$/$1_/

  punctuator:                                           # 符号快速输入和部分符号的快速上屏
    import_preset: symbols
    full_shape:
      "\\": "、"
    half_shape:
      "#": "#"
      "`": "`"
      "~": "~"
      "@": "@"
      "=": "="
      "/": ["/", "÷"]
      '\': "、"
      "'": {pair: ["「", "」"]}
      "[": ["【", "["]
      "]": ["】", "]"]
      "$": ["¥", "$", "€", "£", "¢", "¤"]
      "<": ["《", "〈", "«", "<"]
      ">": ["》", "〉", "»", ">"]

  recognizer:
    patterns:
      email: "^[A-Za-z][-_.0-9A-Za-z]*@.*$"
      uppercase: "[A-Z][-_+.'0-9A-Za-z]*$"
      url: "^(www[.]|https?:|ftp[.:]|mailto:|file:).*$|^[a-z]+[.].+$"
      punct: "^/([a-z]+|[0-9]0?)$"
      reverse_lookup: "`[a-z]*'?$"
```

然后重新部署

**Fcitx**

与 IBus 的部署类似，安装

```bash
sudo apt-get install fcitx-rime
```

在 `sudo apt-get install fcitx-rime` 下创建默认配置文件 default.custom.yaml

```bash
patch:
  menu/page_size: 9 #每页候选词个数
  ascii_composer/switch_key/Shift_L: commit_code #左shift提交字母
```

词库的配置与 IBus 也一样，将 luna_pinyin.extended.dict.yaml 、luna_pinyin_simp.custom.yaml 和词库文件复制过来重新部署就是了。如果有问题就从最简单的一个词库文件配起，慢慢排查问题。

fcitx 皮肤导入路径 /usr/share/fcitx/skin，直接将皮肤文件夹丢在这里就行了，比如我用这个 [https://github.com/henices/rime](https://github.com/henices/rime)  ，还可以自己修改字体和配置 

```bash
vi usr/share/fcitx/skin/mac-gray/fcitx_skin.conf
#输入栏字体和颜色设置
[SkinFont]
FontSize=14
MenuFontSize=11
RespectDPI=True
TipColor=25 120 200
InputColor=25 120 200
IndexColor=25 120 200
FirstCandColor=240 50 50
UserPhraseColor=25 120 200
CodeColor=0 0 0
OtherColor=105 105 105
ActiveMenuColor=255 255 255
InactiveMenuColor=0 0 0
```

配置参考 [https://www.cnblogs.com/maicss/p/14376127.html](https://www.cnblogs.com/maicss/p/14376127.html)

### 4. 其他常用软件安装

**亮度调节**

```bash
sudo add-apt-repository ppa:apandada1/brightness-controller
sudo apt update
sudo apt install brightness-controller
brightness-controller
```

**资源监控**

```bash
# 添加软件源的命令
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor && sudo apt update
# 如果需要删除该软件源可以使用
sudo add-apt-repository -r ppa:fossfreedom/indicator-sysmonitor

# 安装 indicator-sysmonitor
sudo apt install indicator-sysmonitor -y
```

打开 System Monitor Indicator，Perference - General - Run on startup 开机启动

Advanced 中设置自定义格式  men{men}  net{net}

**番茄钟**

```bash

sudo apt install gnome-shell-pomodoro
```

### 5. 设置恢复

Ubunut20.02 桌面启动器 gonme 恢复默认设置，包括桌面布局、图标和系统应用（如输入法和终端等）都会恢复为默认设置

```bash
dconf reset -f /org/gnome/
```

### 6. 中文文件夹改为英文

打开终端，在终端中输入命令:

`export LANG=en_US`

`xdg-user-dirs-gtk-update`

跳出对话框询问是否将目录转化为英文路径,同意并关闭.在终端中输入命令:

`export LANG=zh_CN`

关闭终端,并重起.下次进入系统,系统会提示是否把转化好的目录改回中文.选择不再提示,并取消修改.主目录的中文转英文就完成了~

### 7. 挂载 Windows 系统的硬盘

由于以前的工程全在 Windows 系统下，想在 Ubuntu 直接打开编辑，发现 IDEA 一直报错

Unable to save settings: Failed to save settings. Please restart IntelliJ IDEA ，找了下问题挂载的硬盘权限是没问题了，但就是不能创建文件和编辑文件。

重新挂载硬盘 

```bash
nathan@nathan-tp:/media/nathan$ sudo mount /dev/sda2 /media/nathan/testd
The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Falling back to read-only mount because the NTFS partition is in an
unsafe state. Please resume and shutdown Windows fully (no hibernation
or fast restarting.)
Could not mount read-write, trying read-only
ntfs-3g-mount: failed to access mountpoint /media/nathan/testd: 没有那个文件或目录
```

Windows 盘的原因，unclean 啥的，没正常关机导致的。好吧前一天开的 Windows 睡觉时休眠，早上打开的时候不是进入 Windows ，也没多想就没管进 Ubuntu了，确实没正常关机。网上看到别人也有相似问题 [解决 Linux 挂载 NTFS 分区只读不能写的问题](https://cloud.tencent.com/developer/article/1520766)。

重启进入 Windows 再正常重启进入 Ubuntu 就行了。

**设置开机自动挂载**

运行如下命令查看Windows分区：

```bash
sudo blkid
```

看输出找到自己需要挂载的盘

```
/dev/sdb1: LABEL="win-c" UUID="96A8CBEBA8CBC7C7" TYPE="ntfs" PARTUUID="995606b6-6b82-7ed4-84e7-007d680f31d3"
/dev/sdb2: UUID="0C38-06FF" TYPE="vfat" PARTUUID="dc885421-8944-2273-207d-3685601c7169"
/dev/sda1: PARTLABEL="Microsoft reserved partition" PARTUUID="140f0f24-f5ff-4ea3-b95e-cd12128e0d58"
/dev/sda2: LABEL="win-d" UUID="04BECAB2BECA9C14" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="9a67d1d2-7618-4173-a9d8-d05bf45e924f"
```

修改配置文件 `sudo vi /etc/fstab` 最后加上

```bash
# Windows disks
/dev/sdb1                                                /media/nathan/win-c
/dev/sda2                                                /media/nathan/win-d
```

重启系统完事

### 8. 禁止休眠和唤醒

**休眠问题**

笔记本盖子关闭外接显示器的时候，一关闭显示器系统就休眠了， 不想休眠可以通过编辑 logind.conf 配置来设置

```bash
sudo vi /etc/systemd/logind.conf
```

修改其中的 HandleLidSwitch 配置为  ignore 后重启机器

```bash
# poweroff 关闭盖子时关闭计算机
# hibernate 关闭盖子时计算机休眠
# suspend 关闭盖子时暂停计算机
# ignore 不执行任何操作
#HandleLidSwitch=suspend
HandleLidSwitch=ignore
```

**usb 设备唤醒**

查看所有 usb 设备的电源唤醒状态

```bash
nathan@nathan-tp:~$ grep . /sys/bus/usb/devices/*/power/wakeup
/sys/bus/usb/devices/1-1.3/power/wakeup:enabled
/sys/bus/usb/devices/1-1.4/power/wakeup:disabled
/sys/bus/usb/devices/1-1/power/wakeup:disabled
/sys/bus/usb/devices/1-6/power/wakeup:disabled
/sys/bus/usb/devices/1-7/power/wakeup:disabled
/sys/bus/usb/devices/2-1/power/wakeup:disabled
/sys/bus/usb/devices/usb1/power/wakeup:disabled
/sys/bus/usb/devices/usb2/power/wakeup:disabled
```

`lsusb` 查看外接设备 

```bash
nathan@nathan-tp:~$ lsusb
Bus 002 Device 005: ID 0424:5744 Microchip Technology, Inc. (formerly SMSC) Hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 007: ID 04f2:b541 Chicony Electronics Co., Ltd Integrated Camera
Bus 001 Device 005: ID 8087:0a2b Intel Corp. 
Bus 001 Device 003: ID 138a:0090 Validity Sensors, Inc. VFS7500 Touch Fingerprint Sensor
Bus 001 Device 016: ID 046d:c084 Logitech, Inc. G203 Gaming Mouse
Bus 001 Device 015: ID 1c4f:0002 SiGma Micro Keyboard TRACER Gamma Ivory
Bus 001 Device 014: ID 0424:2744 Microchip Technology, Inc. (formerly SMSC) Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

用这个也可以看到

```bash
nathan@nathan-tp:~$ grep . /sys/bus/usb/devices/*/product
/sys/bus/usb/devices/1-1.3/product:USB Keyboard
/sys/bus/usb/devices/1-1.4/product:G102 Prodigy Gaming Mouse
/sys/bus/usb/devices/1-1/product:USB2744
/sys/bus/usb/devices/1-8/product:Integrated Camera
/sys/bus/usb/devices/2-1/product:USB5744
/sys/bus/usb/devices/usb1/product:xHCI Host Controller
/sys/bus/usb/devices/usb2/product:xHCI Host Controller
```

两个 usb 都是 disabled，全都改成 enabled 得了

用管理员修改 wakeup 值为 enabled

```bash
echo 'enabled' > /sys/bus/usb/devices/usb1/power/wakeup
echo 'enabled' > /sys/bus/usb/devices/usb2/power/wakeup
```

### 9. Docker

原软件源中的 Docker 可能不是最新版本的

更新软件包索引，并且安装必要的依赖软件，来添加一个新的 HTTPS 软件源：

`sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common`

使用下面的 `curl` 导入源仓库的 GPG key：

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

将 Docker APT 软件源添加到你的系统：

`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

现在，Docker 软件源被启用了，你可以安装软件源中任何可用的 Docker 版本。

安装 Docker 最新版本，运行下面的命令

`sudo apt update`

`sudo apt install docker-ce docker-ce-cli containerd.io`

非 root 用户执行 docker 命令权限不足，需要加到 docker 用户组并切换到 docker 用户组

`sudo usermod -aG docker {指定用户名}`

`newgrp docker`

远程连接

```json
sudo vi /lib/systemd/system/docker.service
#加上 -H tcp://0.0.0.0:2375
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H fd:// --containerd=/run/containerd/containerd.sock

sudo systemctl daemon-reload // 加载docker守护线程
sudo systemctl restart docker // 重启docker
```

### 10. Jdk

安装 openjdk8，顺便把 source 也安装上，在 idea 配置下路径就能看到源文件了，不然只能看到 idea 反编译的 class 文件

```bash
sudo apt-get install openjdk-8-jdk
sudo apt-get install openjdk-8-source
```

### 11、修改更新源

备份/etc/apt/sources.list

备份

cp /etc/apt/sources.list /etc/apt/sources.list.bak

在/etc/apt/sources.list文件前面添加如下条目

```jsx
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

[删除软件](删除软件.md)
