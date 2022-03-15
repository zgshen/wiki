
# Linux磁盘相关


### vgdisplay 

vgdisplay 命令显示 LVM 卷组的信息。如果不指定”卷组”参数，则分别显示所有卷组的属性。

```
pvscan [-As] [vg]
```

选项：
- -A：仅显示活动卷组的属性
- -s：使用短格式输出的信息


参数：

vg：要显示属性的卷组名称,示例：

显示存在的卷组vg1000的属性
```
vgdisplay vg1000
```

显示所有卷组信息：
```
vgdisplay
```

### 磁盘扩容/缩减

用 lvresize 命令给磁盘扩容或缩减，用 resize2fs 执行变更。

lvresize 命令进行磁盘扩容或缩减：
```
lvextend -L 10G /dev/mapper/ubuntu--vg-ubuntu--lv      //增大或减小至19G
lvextend -L +10G /dev/mapper/ubuntu--vg-ubuntu--lv     //增加10G
lvreduce -L -10G /dev/mapper/ubuntu--vg-ubuntu--lv     //减小10G
lvresize -l  +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv   //按百分比扩
```

resize2fs 执行变更：
```
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

**例子**

```bash
$ df -Th
Filesystem                        Type      Size  Used Avail Use% Mounted on
udev                              devtmpfs  1.9G     0  1.9G   0% /dev
tmpfs                             tmpfs     394M   12M  383M   3% /run
/dev/mapper/ubuntu--vg-ubuntu--lv ext4       20G   20G     0 100% /
tmpfs                             tmpfs     2.0G     0  2.0G   0% /dev/shm
tmpfs                             tmpfs     5.0M  4.0K  5.0M   1% /run/lock
tmpfs                             tmpfs     2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sda2                         ext4      976M  206M  704M  23% /boot
share                             vboxsf    932G  264G  668G  29% /home/nathan/windows
tmpfs                             tmpfs     394M  8.0K  394M   1% /run/user/124
tmpfs                             tmpfs     394M  4.0K  394M   1% /run/user/1000

$ df /tmp -h
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv   20G   20G     0 100% /
```

/dev/mapper/ubuntu--vg-ubuntu--lv 已经没空闲空间了。

查看卷信息：
```bash
root@vb-ubuntu:/home/nathan# vgdisplay
  --- Volume group ---
  VG Name               ubuntu-vg
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <39.00 GiB
  PE Size               4.00 MiB
  Total PE              9983
  Alloc PE / Size       5120 / 20.00 GiB
  Free  PE / Size       4863 / <19.00 GiB
  VG UUID               UIaujX-muWO-u1L0-R674-suW1-ApCY-UJhNvH
```

Free 还有剩余可以使用，均10G来用。
```bash
root@vb-ubuntu:/home/nathan# lvresize -L +10G /dev/mapper/ubuntu--vg-ubuntu--lv
  Size of logical volume ubuntu-vg/ubuntu-lv changed from 20.00 GiB (5120 extents) to 30.00 GiB (7680 extents).
  Logical volume ubuntu-vg/ubuntu-lv successfully resized.
root@vb-ubuntu:/home/nathan# resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
resize2fs 1.45.5 (07-Jan-2020)
Filesystem at /dev/mapper/ubuntu--vg-ubuntu--lv is mounted on /; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 4
The filesystem on /dev/mapper/ubuntu--vg-ubuntu--lv is now 7864320 (4k) blocks long.
```

再看看：
```bash
root@vb-ubuntu:/home/nathan# vgdisplay
  --- Volume group ---
  VG Name               ubuntu-vg
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <39.00 GiB
  PE Size               4.00 MiB
  Total PE              9983
  Alloc PE / Size       7680 / 30.00 GiB
  Free  PE / Size       2303 / <9.00 GiB
  VG UUID               UIaujX-muWO-u1L0-R674-suW1-ApCY-UJhNvH

root@vb-ubuntu:/home/nathan# df -Thl
Filesystem                        Type      Size  Used Avail Use% Mounted on
udev                              devtmpfs  1.9G     0  1.9G   0% /dev
tmpfs                             tmpfs     394M   12M  383M   3% /run
/dev/mapper/ubuntu--vg-ubuntu--lv ext4       30G   20G  8.5G  70% /
tmpfs                             tmpfs     2.0G     0  2.0G   0% /dev/shm
tmpfs                             tmpfs     5.0M  4.0K  5.0M   1% /run/lock
tmpfs                             tmpfs     2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sda2                         ext4      976M  206M  704M  23% /boot
share                             vboxsf    932G  264G  668G  29% /home/nathan/windows
tmpfs                             tmpfs     394M  8.0K  394M   1% /run/user/124
tmpfs                             tmpfs     394M  4.0K  394M   1% /run/user/1000
```

### df 查看磁盘空间大小的命

df 命令用于查看磁盘分区上的磁盘空间，包括使用了多少，还剩多少，默认单位是KB.

```bash
df -Thl
```

参数：
```bash
-a或--all：包含全部的文件系统；
--block-size=<区块大小>：以指定的区块大小来显示区块数目；
-h或--human-readable：以可读性较高的方式来显示信息；
-H或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
-i或--inodes：显示inode的信息；
-k或--kilobytes：指定区块大小为1024字节；
-l或--local：仅显示本地端的文件系统；
-m或--megabytes：指定区块大小为1048576字节；
--no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
-P或--portability：使用POSIX的输出格式；
--sync：在取得磁盘使用信息前，先执行sync指令；
-t<文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
-T或--print-type：显示文件系统的类型；
-x<文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
--help：显示帮助；
--version：显示版本信息。
```

### du 查看文件和目录大小的命令

du 用来查看文件和目录大小用的，和 df 略有区别。

如要看 /home 目录的总大小，可以用以下命令：

```bash
du -sh /home
```

或者进到 /home 目录后直接执行：
```
# -s参数就是查看总大小（区别于查看其中每个目录的大小）
du -sh
```

查看 /hoome 目录下各个子目录的大小，包括子目录的子目录，但不包含 /home 下文件：
```bash
du -h
```

查看 /home 目录下各个子目录的大小，不包括子目录的子目录：
```bash
du -sh *
```

du 命令参数：
```
-a或-all 显示目录中个别文件的大小。
-b或-bytes 显示目录或文件大小时，以byte为单位。
-c或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-k或--kilobytes 以KB(1024bytes)为单位输出。
-m或--megabytes 以MB为单位输出。
-s或--summarize 仅显示总计，只列出最后加总的值。
-h或--human-readable 以K，M，G为单位，提高信息的可读性。
-x或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-L<符号链接>或--dereference<符号链接> 显示选项中所指定符号链接的源文件大小。
-S或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
-X<文件>或--exclude-from=<文件> 在<文件>指定目录或文件。
--exclude=<目录或文件> 略过指定的目录或文件。
-D或--dereference-args 显示指定符号链接的源文件大小。
-H或--si 与-h参数相同，但是K，M，G是以1000为换算单位。
-l或--count-links 重复计算硬件链接的文件。
```

### sort 排序

df du 命令可以和 sort 一起使用实现排序。

将文件夹中的文件按大小排序：
```bash
du -a|sort -rn
```

-r参数代表反向排序, -n代表按照数字排序，只认数字不认单位，单位是默认的KB，所以这个命令不能用 du -ah，这会使排序结果出现2M小于100K的情况。

sort命令参数：
```
-b：忽略每行前面开始出的空格字符；
-c：检查文件是否已经按照顺序排序；
-d：排序时，处理英文字母、数字及空格字符外，忽略其他的字符；
-f：排序时，将小写字母视为大写字母；
-i：排序时，除了040至176之间的ASCII字符外，忽略其他的字符；
-m：将几个排序号的文件进行合并；
-M：将前面3个字母依照月份的缩写进行排序；
-n：依照数值的大小排序；
-o<输出文件>：将排序后的结果存入制定的文件；
-r：以相反的顺序来排序；
-t<分隔字符>：指定排序时所用的栏位分隔字符；
+<起始栏位>-<结束栏位>：以指定的栏位来排序，范围由起始栏位到结束栏位的前一栏位。
```


### 参考
- [1] [vgdisplay 显示卷组详细信息](https://www.kancloud.cn/xjdnw/linux/2534988)
- [2] [Linux中查看磁盘大小、文件大小、排序方法小结](https://blog.csdn.net/lkforce/article/details/80917306)
- [3] [如何扩大ubuntu的ubuntu--vg-ubuntu--lv空间](https://zhuanlan.zhihu.com/p/359959580)
