# ln 软连接

创建软链接

ln  -s  [源文件或目录]  [目标文件或目录]

例：

当前路径创建test 引向/var/www/test 文件夹

```bash
ln –s  /var/www/test  test
```

创建/var/test 引向/var/www/test 文件夹

```bash
ln –s  /var/www/test   /var/test
```

删除软链接

和删除普通的文件是一样的，删除都是使用rm来进行操作

例：

删除test

```bash
rm –rf test
```

修改软链接

ln –snf  [新的源文件或目录]  [目标文件或目录]

这将会修改原有的链接地址为新的地址

例如：

创建一个软链接

```bash
ln –s  /var/www/test   /var/test
```

修改指向的新路径

```bash
ln –snf  /var/www/test1   /var/test
```

**常用参数：**

```
　　-f : 链结时先将与 dist 同档名的档案删除
　　-d : 允许系统管理者硬链结自己的目录
　　-i : 在删除与 dist 同档名的档案时先进行询问
　　-n : 在进行软连结时，将 dist 视为一般的档案
　　-s : 进行软链结(symbolic link)
　　-v : 在连结之前显示其档名
　　-b : 将在链结时会被覆写或删除的档案进行备份
　　-S SUFFIX : 将备份的档案都加上 SUFFIX 的字尾
　　-V METHOD : 指定备份的方式
　　--help : 显示辅助说明
　　--version : 显示版本
```