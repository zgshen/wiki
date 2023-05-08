
## Git 彻底删除某个commit的方法

如果因为一些原因，需要删除某个错误的 `commit`，而且需要干净的操作，彻底让其消失，不留痕迹，该如何操作？

> 我向仓库提交了一个大文件，大约 300M，push 失败（因为 git 最大能提交 100M 文件），删除本地文件不行，尝试过修改配置文件，解除 git 只能提交小于 100M 文件的限制，但是未起作用。只能通过删除包含提交此文件的 commit 解决。

废话少说，直奔主题。

1.首先输入如下命令查看历史提交的 commit：

```javascript
git log
```


重要的是记下要删除的 commit 的上一条 commit 的 commit号。如下，如果要删除 commit 1df9b1382cf8ab4efb544162cc62e9437849b5f7，需要上一个 commit号 ：2d8bd0533beb31a86e93aeca1dd1da8edbb9cbe7
```
commit 1df9b1382cf8ab4efb544162cc62e9437849b5f7 (HEAD -> master, origin/master)
Author: zgshen <zguishen@foxmail.com>
Date:   Fri Mar 3 00:17:11 2023 +0800

    fix

commit 2d8bd0533beb31a86e93aeca1dd1da8edbb9cbe7
Author: zgshen <zguishen@foxmail.com>
Date:   Wed Feb 22 22:08:35 2023 +0800

    fix

commit 1f44abfd6174f3113855d6541098d5c09d14c426
Author: zgshen <zguishen@foxmail.com>
Date:   Tue Feb 21 23:12:50 2023 +0800

    fix
```

2.然后执行如下的命令：

```javascript
git rebase -i commit号

# git rebase -i 2d8bd0533beb31a86e93aeca1dd1da8edbb9cbe7
```


会出现如下界面：
```
pick 1df9b13 fix

# Rebase 2d8bd05..1df9b13 onto 2d8bd05 (1 command)
#
# Commands:
# p, pick <commit> = use commit
...
...
```


3.然后将要删除的 commit号 的前缀 `pick` 改为 `drop` ，保存，会提示 Successfully rebased and updated refs/heads/master.。

4.然后可以通过如下命令再次查看是否已经删除：

```javascript
git log
```


5.最后通过如下命令将现在的状态推送到远程仓库即可：

```javascript
git push origin HEAD -force
```


来自 https://cloud.tencent.com/developer/article/1511875