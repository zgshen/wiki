# git

## 1. 提交注释类型

 **Type**

Must be one of the following:

- **build**: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- **ci**: Changes to our CI configuration files and scripts (example scopes: Circle, BrowserStack, SauceLabs)
- **docs**: Documentation only changes
- **feat**: A new feature
- **fix**: A bug fix
- **perf**: A code change that improves performance
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **test**: Adding missing tests or correcting existing tests

feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动

## 2.代理

**解决方法一**

```bash
只对 github.com 
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080 
取消代理 
git config --global --unset http.https://github.com.proxy
```

**解决方法二**

```bash
分辨需要设置的代理
HTTP 形式：
git clone https://github.com/owner/git.git

SSH 形式：
git clone git@github.com:owner/git.git

一、HTTP 形式
走 HTTP 代理
git config --global http.proxy "http://127.0.0.1:8080"
git config --global https.proxy "http://127.0.0.1:8080"
走 socks5 代理（如 Shadowsocks）
git config --global http.proxy "socks5://127.0.0.1:1080"
git config --global https.proxy "socks5://127.0.0.1:1080"
取消设置
git config --global --unset http.proxy
git config --global --unset https.proxy
二、SSH 形式
修改 ~/.ssh/config 文件（不存在则新建）：

# 必须是 github.com
Host github.com
   HostName github.com
   User git
   # 走 HTTP 代理
   # ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=8080
   # 走 socks5 代理（如 Shadowsocks）
   # ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
```

**注意** 

```bash
# 如果是 ssh 协议，需要添加下列配置到 ~/.ssh/config，代理协议为 socks5
Host github.com
	ProxyCommand nc -X 5 -x 127.0.0.1:10809 %h %p
# 或者使用 connect 命令建立 socket 连接 http://www.patthoyts.tk/blog/using-git-with-socks-proxy.html
```

以上是在 Linux 下的配置，在 Windows 下没有 nc 命令，但安装 git bash 后可以有 connect 命令，配置如下

```bash
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    User zgshen
ProxyCommand "D:/Program Files/Git/mingw64/bin/connect.exe" -H 127.0.0.1:10809 %h %p
#ProxyCommand nc -v -x 127.0.0.1:10809 %h %p
```

## 3. 查看提交历史

```bash
git log --pretty=format:"%h - %an, %ar : %s" -5
```

## 4. 修改默认编辑器

git 的默认编辑器是 nano，少用不习惯的话可改为 vi

```bash
git config core.editor vim
```

## 5. 修改提交历史用户

```bash
git rebase -i HEAD~9   #最新的 9 个提交
```

将提交前面的  pick 改为 edit，保存退出，会有类似提示，如果是合并的提交就不是，可选择跳过

```bash
停止在 78611caa... fix
您现在可以修补这个提交，使用

  git commit --amend 

当您对变更感到满意，执行

  git rebase --continue
```

使用以下命令来修改提交用户

```bash
git commit --amend --author="zgshen <zguishen@foxmail.com>"
```

continue 继续修改直到结束

```bash
git rebase --continue
```

## 6. 免密登录

客户机上 `ssh-keygen -t rsa` 生成密钥

Linux 上密钥位置在当前用户目录下  `.ssh` 文件夹中，Windows 上密钥位置在 `C:\Users\用户名\.ssh` 中，公钥文件为 id_rsa.pub，私钥文件为 id_rsa

将公钥文件上传到当前用户目录下 `/home/用户/.ssh/` （或者想给谁 ssh 权限放谁目录下，目录不存在先创建）

`touch ~/.ssh/authorized_keys` 创建 authorized_keys 文件

`cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys` 将公钥写入 authorized_keys 文件，注意服务器机器若也想 ssh 其他机器，.ssh 目录下要是先生成 id_rsa.pub 文件，上传的客户端公钥文件改个名，别把服务器机器自己的公钥给覆盖了。

这样就可以 ssh 免密登录了。

如果有多个公钥，换行继续添加就好了

碰到错误

IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!

Someone could be eavesdropping on you right now (man-in-the-middle attack)!

可能是以前用别的公钥连过服务器，打开`C:\Users\用户名\.ssh` 下 known_hosts 文件，找到连接服务器的 host 记录删了，重新连接出现提示输入 yes 就行了，要么还是不行可能是密钥搞错了，看错误信息排查。

## 7. GitHub 免密及更换远程地址

`ssh-keygen -t rsa` 一路回车

`~/.ssh` 目录下会生成 id_rsa.pub 文件

到 [https://github.com/settings/keys](https://github.com/settings/keys) 下 SSH and GPG keys 下添加 SSH keys 就好了

如果还是提示要输入帐号密码，看下仓库远程地址是不是用了 https 而不是 ssh

`git remote -v` 命令查看。如果是的话，改成 ssh 协议的就好了

```bash
git remote remove origin
git remote add origin git@github.com:username/repository name.git
```

然后原来的远程分支没了，需要重新跟踪，可以看到 `git push` 失败

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

## 8. 登录远程服务器

如下设置

```bash
Host tx-light
    HostName 42.193.139.xx
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    User nathan
```

然后就可以按以下命令登录了

```bash
ssh tx-light
```