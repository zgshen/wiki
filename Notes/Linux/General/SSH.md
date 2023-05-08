### 1.安装配置

安装服务端

```json
sudo apt-get install openssh-server
```

ssh_config 和 sshd_config 配置文件区别：

远程管理linux系统基本上都要使用到ssh，原因很简单：telnet、FTP等传输方式是?以明文传送用户认证信息，本质上是不安全的，存在被网络窃听的危险。SSH（Secure Shell）目前较可靠，是专为远程登录会话和其他网络服务提供安全性的协议。利用SSH协议可以有效防止远程管理过程中的信息泄露问题，透过SSH可以对所有传输的数据进行加密，也能够防止DNS欺骗和IP欺骗。

ssh_config和sshd_config都是ssh服务器的配置文件，二者区别在于，前者是针对**客户端的配置文件**，后者则是针对**服务端的配置文件**。两个配置文件都允许你通过设置不同的选项来改变客户端程序的运行方式

客户机上 `ssh-keygen -t rsa` 生成密钥

Linux 上密钥位置在当前用户目录下  `.ssh` 文件夹中，Windows 上密钥位置在 `C:\Users\用户名\.ssh` 中，公钥文件为 id_rsa.pub，私钥文件为 id_rsa

将公钥文件上传到当前用户目录下 `/home/用户/.ssh/` （或者想给谁 ssh 权限放谁目录下，目录不存在先创建）

`touch ~/.ssh/authorized_keys` 创建 authorized_keys 文件

`cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys` 将公钥写入 authorized_keys 文件，注意服务器机器若也想 ssh 其他机器，.ssh 目录下要是先生成 id_rsa.pub 文件，上传的客户端公钥文件改个名，别把服务器机器自己的公钥给覆盖了。

这样就可以 ssh 免密登录了。

如果有多个公钥，换行继续添加就好了。

如果碰到错误：
```
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!

Someone could be eavesdropping on you right now (man-in-the-middle attack)!
```
可能是以前用别的公钥连过服务器，打开`C:\Users\用户名\.ssh` 下 known_hosts 文件，找到连接服务器的 host 记录删了，重新连接出现提示输入 yes 就行了，要么还是不行可能是密钥搞错了，看错误信息排查。

sshd为了安全，对属主的目录和文件权限有所要求。如果权限不对，则ssh的免密码登陆不生效。

用户目录权限为 755 或者 700，就是不能是77x。

.ssh目录权限一般为755或者700。

rsa_id.pub 及authorized_keys权限一般为644

rsa_id权限必须为600

### 2.GitHub 免密及更换远程地址

`ssh-keygen -t rsa` 一路回车

`~/.ssh` 目录下会生成 id_rsa.pub 文件

到 [https://github.com/settings/keys](https://github.com/settings/keys) 下 SSH and GPG keys 下添加 SSH keys 就好了

如果还是提示要输入帐号密码，看下仓库远程地址是不是用了 https 而不是 ssh

`git remote -v` 命令查看。如果是的话，改成 ssh 协议的就好了

```bash
git remote remove origin
git remote add origin git@github.com:username/repository name.git
```

然后原来的远程分支没了，需要重新跟踪，可以看到 `git push` 失败 #修改远程分支方式

```bash
nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git push
fatal: 当前分支 hexo-blog 没有对应的上游分支。
为推送当前分支并建立与远程上游的跟踪，使用

git push --set-upstream origin hexo-blog

nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git fetch
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
remote: Enumerating objects: 237, done.
remote: Counting objects: 100% (227/227), done.
remote: Compressing objects: 100% (84/84), done.
remote: Total 237 (delta 128), reused 175 (delta 80), pack-reused 10
接收对象中: 100% (237/237), 8.08 MiB | 750.00 KiB/s, 完成.
处理 delta 中: 100% (128/128), 完成.
来自 github.com:zgshen/zgshen.github.io
 * [新分支]            hexo-blog  -> origin/hexo-blog
 * [新分支]            master     -> origin/master
 * [新分支]            res        -> origin/res
```

`git fetch` 重新拉取远程后 `git push --set-upstream origin hexo-blog` 重新关联就好了

```bash
nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git branch -a
* hexo-blog
  master
  res
  remotes/origin/hexo-blog
  remotes/origin/master
  remotes/origin/res
nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git push origin 
fatal: 当前分支 hexo-blog 没有对应的上游分支。
为推送当前分支并建立与远程上游的跟踪，使用

    git push --set-upstream origin hexo-blog
        
nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git push --set-upstream origin hexo-blog 
枚举对象中: 9, 完成.
对象计数中: 100% (9/9), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (5/5), 完成.
写入对象中: 100% (5/5), 1.50 KiB | 382.00 KiB/s, 完成.
总共 5 （差异 4），复用 0 （差异 0）
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
To github.com:zgshen/zgshen.github.io.git
   bf96ff24..3066d9b6  hexo-blog -> hexo-blog
分支 'hexo-blog' 设置为跟踪来自 'origin' 的远程分支 'hexo-blog'。
nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git branch -a
* hexo-blog
  master
  res
  remotes/origin/hexo-blog
  remotes/origin/master
  remotes/origin/res
nathan@nathan-tp:/media/nathan/win-d/Project/gitFile/zgshen$ git push 
Everything up-to-date
```

或者使用更简单的方式（推荐）

```bash
git remote set-url origin git@github.com:username/repo.git
```

例：

```bash
# https 的
PS D:\Project\gitFile\Kaiming-English-Grammar> git remote -v
origin  https://github.com/zgshen/Kaiming-English-Grammar.git (fetch)
origin  https://github.com/zgshen/Kaiming-English-Grammar.git (push)

PS D:\Project\gitFile\Kaiming-English-Grammar> git remote set-url origin  git@github.com:zgshen/Kaiming-English-Grammar.git

# 已更换为 git 的
PS D:\Project\gitFile\Kaiming-English-Grammar> git remote -v
origin  git@github.com:zgshen/Kaiming-English-Grammar.git (fetch)
origin  git@github.com:zgshen/Kaiming-English-Grammar.git (push)
```

### 超时断开问题

通过`ssh`连上服务器后，一段时间不操作，就会自动中断

编辑sshd配置文件

```
vi /etc/ssh/sshd_config

找到

#ClientAliveInterval 0
#ClientAliveCountMax 3

修改为

ClientAliveInterval 60
ClientAliveCountMax 3
```

ClientAliveInterval指定了服务器端向客户端请求消息 的时间间隔, 默认是0, 不发送.

ClientAliveInterval 60表示每分钟发送一次, 然后客户端响应, 这样就保持长连接了.

ClientAliveCountMax, 使用默认值3即可.

ClientAliveCountMax表示服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开.

重启sshd服务

```jsx
systemctl restart sshd
```

为了增强Linux系统的安全性，我们需要在用户输入空闲一段时间后自动断开，这个操作可以由设置TMOUT值来实现。将以下字段加入到/etc/profile 中即可(对所有用户生效)

```jsx
echo $TMOUT
```

如果输出空或0表示不超时，大于0的数字n表示n秒没有收入则超时

编辑 profile 文件

```jsx
vi /etc/profile

# 添加
export TMOUT=0
```

将以上修改为0就是设置不超时，立即生效：

```jsx
source /etc/profile
```

### 用户名和邮箱地址的作用

用户名和邮箱地址是本地git客户端的一个变量，不随git库而改变。

每次commit都会用用户名和邮箱纪录。

github的contributions统计就是按邮箱来统计的。

查看用户名和邮箱地址：

$ git config [user.name](http://user.name/)

$ git config user.email

修改用户名和邮箱地址：

$ git config --global [user.name](http://user.name/) "username"

$ git config --global user.email "email"