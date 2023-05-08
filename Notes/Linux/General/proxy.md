## 代理


可以用来设置环境变量的文件常用的有以下几个：

-   /etc/profile
-   /etc/profile.d/*.sh
-   /etc/bashrc
-   ~/.bash_profile
-   ~/.bashrc

.bashrc文件是用户登录时执行的脚本文件，如何你换成 zsh 的话就是 .zshrc

### 终端代理
在终端使用代理的时候我们平常可以用以下命令来让**当前**终端会话使用代理，离开当前会话就失效。
如果想要在每个终端都生效的话，需要把命令写入到 `~/.bashrc` 文件中（zsh 的话就是 .zshrc），然后执行 `source ~/.bashrc` 后新开的终端就都能走代理了。
```bash
export https_proxy=http://127.0.0.1:10808 http_proxy=http://127.0.0.1:10808 all_proxy=socks5://127.0.0.1:10808
```

### 系统代理

至于其他GUI软件，需要设置Ubuntu系统的代理。
![](../Assets/Pasted%20image%2020230221175205.png)


