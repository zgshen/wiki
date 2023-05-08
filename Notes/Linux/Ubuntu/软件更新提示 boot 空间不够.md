
### 原因

安装 Ubuntu 的时候给 /boot 分区设置的空间是 1G，长久更新累计的内核文件占满了空间

先看下当前系统内核：

```bash
# nathan @ nathan-tp in /opt [1:53:27] 
$ uname -a
Linux nathan-tp 5.19.0-35-generic #36~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Feb 17 15:17:25 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

看下安装内核版本列表：

```bash
# nathan @ nathan-tp in /opt [1:55:46] 
$ sudo dpkg --get-selections |grep linux-image
[sudo] password for nathan: 
linux-image-5.13.0-37-generic			deinstall
linux-image-5.13.0-39-generic			deinstall
linux-image-5.13.0-40-generic			deinstall
linux-image-5.13.0-41-generic			deinstall
linux-image-5.13.0-44-generic			deinstall
linux-image-5.13.0-48-generic			deinstall
linux-image-5.13.0-51-generic			deinstall
linux-image-5.13.0-52-generic			install
linux-image-5.15.0-41-generic			deinstall
linux-image-5.15.0-43-generic			deinstall
linux-image-5.15.0-46-generic			deinstall
linux-image-5.15.0-47-generic			deinstall
linux-image-5.15.0-48-generic			deinstall
linux-image-5.15.0-50-generic			deinstall
linux-image-5.15.0-52-generic			deinstall
linux-image-5.15.0-53-generic			deinstall
linux-image-5.15.0-56-generic			deinstall
linux-image-5.15.0-57-generic			deinstall
linux-image-5.15.0-58-generic			deinstall
linux-image-5.15.0-60-generic			deinstall
linux-image-5.15.0-67-generic			install
linux-image-5.15.0-69-generic			install
linux-image-5.19.0-32-generic			install
linux-image-5.19.0-35-generic			install
linux-image-5.19.0-38-generic			install
linux-image-5.8.0-43-generic			install
linux-image-generic				install
linux-image-generic-hwe-22.04			install
```

install 是安装的，deinstall 是已经卸载的。

看下 /boot 下的空间占用，可以看到几十或一百多兆的旧版内核文件。

```bash
# nathan @ nathan-tp in /boot [0:44:00] 
$ ls -lh
total 726M
-rw-r--r-- 1 root root 252K Jun 17  2022 config-5.13.0-52-generic
-rw-r--r-- 1 root root 256K Feb 22 22:01 config-5.15.0-67-generic
-rw-r--r-- 1 root root 264K Jan 30 23:44 config-5.19.0-32-generic
-rw-r--r-- 1 root root 264K Feb 17 22:31 config-5.19.0-35-generic
-rw-r--r-- 1 root root 243K Feb  5  2021 config-5.8.0-43-generic
drwx------ 5 root root 1.0K Jan  1  1970 efi
drwxr-xr-x 5 root root 4.0K Apr  1 09:05 grub
lrwxrwxrwx 1 root root   28 Mar  4 15:11 initrd.img -> initrd.img-5.15.0-67-generic
-rw-r--r-- 1 root root  96M Apr  1 09:24 initrd.img-5.13.0-52-generic
-rw-r--r-- 1 root root 163M Apr  1 09:24 initrd.img-5.15.0-67-generic
-rw-r--r-- 1 root root 173M Apr  1 09:23 initrd.img-5.19.0-32-generic
-rw-r--r-- 1 root root 174M Apr  1 09:23 initrd.img-5.19.0-35-generic
-rw-r--r-- 1 root root  38M Apr  1 09:24 initrd.img-5.8.0-43-generic
lrwxrwxrwx 1 root root   28 Mar  4 15:14 initrd.img.old -> initrd.img-5.19.0-35-generic
drwx------ 2 root root  16K Mar 25  2022 lost+found
-rw-r--r-- 1 root root 179K Feb  7  2022 memtest86+.bin
-rw-r--r-- 1 root root 181K Feb  7  2022 memtest86+.elf
-rw-r--r-- 1 root root 181K Feb  7  2022 memtest86+_multiboot.bin
-rw------- 1 root root 5.7M Jun 17  2022 System.map-5.13.0-52-generic
-rw------- 1 root root 6.0M Feb 22 22:01 System.map-5.15.0-67-generic
-rw------- 1 root root 6.2M Jan 30 23:44 System.map-5.19.0-32-generic
-rw------- 1 root root 6.2M Feb 17 22:31 System.map-5.19.0-35-generic
-rw------- 1 root root 5.3M Feb  5  2021 System.map-5.8.0-43-generic
lrwxrwxrwx 1 root root   25 Mar  4 15:11 vmlinuz -> vmlinuz-5.15.0-67-generic
-rw------- 1 root root 9.8M Jun 17  2022 vmlinuz-5.13.0-52-generic
-rw------- 1 root root  12M Feb 22 22:03 vmlinuz-5.15.0-67-generic
-rw------- 1 root root  12M Jan 30 23:46 vmlinuz-5.19.0-32-generic
-rw------- 1 root root  12M Feb 17 22:33 vmlinuz-5.19.0-35-generic
-rw-r--r-- 1 root root 9.3M Feb 10  2021 vmlinuz-5.8.0-43-generic
lrwxrwxrwx 1 root root   25 Mar  4 15:11 vmlinuz.old -> vmlinuz-5.19.0-35-generic

```

看下占用，100%使用，没空间了：
```bash
# nathan @ nathan-tp in /boot [1:48:40] 
$ df -Th
Filesystem     Type     Size  Used Avail Use% Mounted on
tmpfs          tmpfs    3.2G  2.3M  3.2G   1% /run
/dev/sda6      ext4     281G   33G  234G  13% /
tmpfs          tmpfs     16G  184M   16G   2% /dev/shm
tmpfs          tmpfs    5.0M  4.0K  5.0M   1% /run/lock
/dev/sda7      ext4     209G   87G  112G  44% /home
/dev/sda4      fuseblk  120G   80G   40G  67% /media/nathan/win-c
/dev/sda5      ext4     944M  926M     0 100% /boot
/dev/sda2      fuseblk  312G  262G   50G  85% /media/nathan/win-d
/dev/sda3      vfat     596M   34M  563M   6% /boot/efi
tmpfs          tmpfs    3.2G  208K  3.2G   1% /run/user/1000
```

### 卸载没用的旧版内核

把旧内核卸载腾出空间：

```bash
# nathan @ nathan-tp in /boot [1:38:55] C:130
$ sudo apt purge linux-image-5.8.0-43-generic
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 linux-image-generic : Depends: linux-image-5.15.0-69-generic but it is not going to be installed
 linux-modules-extra-5.15.0-69-generic : Depends: linux-image-5.15.0-69-generic but it is not going to be installed or
                                                  linux-image-unsigned-5.15.0-69-generic but it is not going to be installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

根据提示先修复：

```bash
# nathan @ nathan-tp in /boot [1:43:43] C:100
$ sudo apt --fix-broken install
Reading package lists... Done
Building dependency tree... Done
...
...
Preparing to unpack .../linux-image-5.15.0-69-generic_5.15.0-69.76_amd64.deb ...
Unpacking linux-image-5.15.0-69-generic (5.15.0-69.76) ...
dpkg: error processing archive /var/cache/apt/archives/linux-image-5.15.0-69-generic_5.15.0-69.76_amd64.deb (--unpack):
 cannot copy extracted data for './boot/vmlinuz-5.15.0-69-generic' to '/boot/vmlinuz-5.15.0-69-generic.dpkg-new': failed to write (No spac
e left on device)
No apport report written because the error message indicates a disk full error
                                                                              dpkg-deb: error: paste subprocess was killed by signal (Brok
en pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/linux-modules-5.15.0-69-generic_5.15.0-69.76_amd64.deb
 /var/cache/apt/archives/linux-image-5.15.0-69-generic_5.15.0-69.76_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

其实就是 /boot 没空间了解压没地方放了，所以可以先把一些跟要卸载无关的内核文件先移到其他地方去，腾出空间，再执行修复就成功了。
```bash
sudo mv initrd.img-5.19.0-32-generic /opt
sudo mv initrd.img-5.15.0-67-generic /opt
```


然后就可以删除了，删除后腾出空间，记得把移到别的文件夹的文件移动回来，然后再删除对应版本的文件。

```bash
sudo apt purge linux-image-5.8.0-43-generic 
...
...
sudo apt purge linux-image-5.19.0-32-generic
```

最后就可以 apt 或 apt-fast update、upgrade 更新软件了。

```bash
# 更新
sudo apt-fast update
#升级
sudo apt-fast upgrade
```

