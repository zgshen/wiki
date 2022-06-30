
# neovim

安装
```bash
sudo apt install -y neovim
```

配置，参考YouTube上老哥的 https://github.com/NeuralNine/config-files/blob/master/init.vim

视频教程 https://www.youtube.com/watch?v=JWReY93Vl6g&t=1120s

安装 [vim-plug](https://github.com/junegunn/vim-plug)，以便它在启动时自动加载：

```bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

不过现在更推荐vim-plug换成了packer，使用lua来管理配置，更加灵活。

体验参考知乎 https://www.zhihu.com/question/445290918

不过这么折腾，我为什么不直接用vscode+vim插件来开发呢？



