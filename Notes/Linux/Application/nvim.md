
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


## nvim & lua & lsp plugin

### 升级版本

上面看到Ubuntu源安装的版本较低，如果想用lsp插件这样的功能需要升级版本（Neovim>=0.5）。

升级下 neovinm：
```bash
# 添加源，stable
sudo add-apt-repository ppa:neovim-ppa/stable
# unstable 版本更高
# sudo add-apt-repository ppa:neovim-ppa/unstable

# 删除仓库
# sudo add-apt-repository -r ppa:neovim-ppa/stable

# 更新安装
sudo apt-get update
sudo apt-get install neovim

# 查看版本
nvim --version
```

把 init.vim 中的插件配置注释或删除，然后 :PlugClean 执行就删除插件了。

看下 nvim 版本：
```bash
# nathan @ nathan-tp in ~/.config/nvim [9:49:51] 
$ nvim --version
NVIM v0.4.3
Build type: Release
LuaJIT 2.1.0-beta3
Compilation: /usr/bin/cc -g -O2 -fdebug-prefix-map=/build/neovim-gOb7vg/neovim-0.4.3=. -fstack-protector-strong -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=1 -DDISABLE_LOG -Wdate-time -D_FORTIFY_SOURCE=1 -O2 -DNDEBUG -DMIN_LOG_LEVEL=3 -Wall -Wextra -pedantic -Wno-unused-parameter -Wstrict-prototypes -std=gnu99 -Wshadow -Wconversion -Wmissing-prototypes -Wimplicit-fallthrough -Wvla -fstack-protector-strong -fdiagnostics-color=always -DINCLUDE_GENERATED_DECLARATIONS -D_GNU_SOURCE -DNVIM_MSGPACK_HAS_FLOAT32 -DNVIM_UNIBI_HAS_VAR_FROM -I/build/neovim-gOb7vg/neovim-0.4.3/build/config -I/build/neovim-gOb7vg/neovim-0.4.3/src -I/usr/include -I/usr/include/lua5.1 -I/build/neovim-gOb7vg/neovim-0.4.3/build/src/nvim/auto -I/build/neovim-gOb7vg/neovim-0.4.3/build/include
Compiled by team+vim@tracker.debian.org

Features: +acl +iconv +tui
See ":help feature-compile"

   system vimrc file: "$VIM/sysinit.vim"
  fall-back for $VIM: "/usr/share/nvim"

Run :checkhealth for more info
```

### lua 脚本管理的基本配置

~/.config/nvim 下创建 init.lua 文件和 lua 文件夹。

init.lua 文件用于引入模块，比如在 lua 文件夹下创建 settings.lua，内容

```bash
# settings.lua，行号和编码等其他配置
vim.o.number = true
vim.o.encoding = "utf-8"
vim.o.fileencoding = "utf-8"
vim.o.relativenumber = true
```

init.py 引用 settings 模块
```bash
require("settings")
```

### packer 管理器

下载配置 packer 管理器
```bash
git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

packer 的使用详情看 https://github.com/wbthomason/packer.nvim 的 README.md

贴一个简单的配置：
```bash
local fn = vim.fn

-- Automatically install packer
local install_path = fn.stdpath "data" .. "/site/pack/packer/start/packer.nvim"
if fn.empty(fn.glob(install_path)) > 0 then
  PACKER_BOOTSTRAP = fn.system {
    "git",
    "clone",
    "--depth",
    "1",
    "https://github.com/wbthomason/packer.nvim",
    install_path,
  }
  print "Installing packer close and reopen Neovim..."
  vim.cmd [[packadd packer.nvim]]
end

-- Autocommand that reloads neovim whenever you save the plugins.lua file
vim.cmd [[
  augroup packer_user_config
    autocmd!
    autocmd BufWritePost plugins.lua source <afile> | PackerSync
  augroup end
]]

-- Use a protected call so we don't error out on first use
local status_ok, packer = pcall(require, "packer")
if not status_ok then
  return
end

-- Have packer use a popup window
packer.init {
  display = {
    open_fn = function()
      return require("packer.util").float { border = "rounded" }
    end,
  },
}

-- Install your plugins here
return packer.startup(function(use)
  -- My plugins here
  use "wbthomason/packer.nvim" -- Have packer manage itself
  use "vim-airline/vim-airline" -- Lean & mean status/tabline for vim that's light as air

  -- Automatically set up your configuration after cloning packer.nvim
  -- Put this at the end after all plugins
  if PACKER_BOOTSTRAP then
    require("packer").sync()
  end
end)
```

插件写在 `return packer.startup(function(use)` 下面，"wbthomason/packer.nvim" 是管理器本身，这里装一个 "vim-airline/vim-airline" 试试。

配置之后打开 init.lua，执行 :PackerInstall 就会安装插件，插件生效然后就可以看到底下的状态行变化了。


### lsp 配置

https://github.com/lxyoucan/nvim-as-java-ide

https://github.com/mfussenegger/nvim-jdtls

按 tab 补全

太折腾了，先不搞了。

## 参考

- [1][packer.nvim](https://github.com/wbthomason/packer.nvim)
- [2][nvim-as-java-ide](https://github.com/lxyoucan/nvim-as-java-ide)
- [3][nvim-jdtls](https://github.com/mfussenegger/nvim-jdtls)