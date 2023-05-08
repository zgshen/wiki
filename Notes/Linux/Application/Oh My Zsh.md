### Oh My Zsh 安装

先安装 zsh

centos    `yum install -y zsh`

ubunut   `sudo apt install zsh -y`

安装 oh my zsh，不同用户都要安装一次

```
#方法一：wget方式自动化安装oh my zsh：
$ wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

#方法二：
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

#官网上的另外一种写法
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

#方法三：当然也可以通过git下载
$ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

git clone https://ghproxy.com/https://github.com/robbyrussell/oh-my-zsh ~/.oh-my-zsh
```

`sudo apt-get install zsh`

vi ~/.zshrc   修改主题

```json
#ZSH_THEME="robbyrussell"
ZSH_THEME="ys"
```

echo $SHELL  查看默认 shell

chsh -s /bin/zsh   设置 zsh 为默认 shell

**ubuntu 下没看到生效的话 log out 重新登录应该就可以了。**

### 让其他用户使用zsh

如果使用`wt`用户安装配置了`oh-my-zsh`，其他用户想要使用相同的主题和配置，可以参考`https://askubuntu.com/questions/521469/oh-my-zsh-for-the-root-and-for-all-user`
这里介绍一种更简单的方法（亲测有效）：
比如让`root`用户使用和`wt`用户相同的配置：

```bash
sudo ln -s $HOME/.oh-my-zsh           /root/.oh-my-zsh
sudo ln -s $HOME/.zshrc               /root/.zshrc
```

切换到`root`用户，命令`zsh`,即可看到`zsh`的主题和`wt`用户的一样了。如果提示

`/root/.zshrc:119: command not found: pyenv
/root/.zshrc:120: command not found: pyenv`

再创建`.pyenv`的软连接即可。

`sudo ln -s $HOME/.pyenv    /root/.pyenv`

这样做的缺点是`root`用户的所有配置都和`wt`用户的一致，不能个性化。修改一个，其他用户的也会变。
如果要个性化，可以用

`sudo cp -r /home/wt/.oh-my-zsh    /root
sudo cp -r /home/wt/.zshrc    /root`

