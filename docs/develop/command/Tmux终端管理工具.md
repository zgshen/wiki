
# Tmux终端管理工具

### 安装

```bash
sudo apt install tmux -y
```

设置鼠标支持，可上下滚动查看，按 shift 鼠标选中复制复制内容，点击选中其他 tmux 窗口

```bash
tmux set mouse on
```

### 基本操作

##### session 管理

```bash
# 创建一个指定名称的 session
tmux new -s test
tmux new -s dev

# 列出所有 tmux session
tmux ls
```

关闭 session
```bash
# 关闭 test 会话
exit
# 或者
tmux kill-session -t test
# 或者按 Ctrl+D 退出当前 tmux

# 关闭服务器，所有的会话都将关闭
tmux kill-server 
```

会话切换
```bash
# 进入指定 tmux session
tmux attach -t dev

# 切换 tmux session
tmux swtich -t test
```

重命名 dev 会话名称为 nemdev
```bash
tmux rename-session -t dev newdev
```

分离会话
```bash
PREFIX d
```

prefix 键是 tmux 的前缀快捷键，默认是 ctrl+b，可在 ~/.tmux.conf 中修改，比如 `set -g prefix C-a` 将 prefix 键修改成 ctrl+a。

#### 窗口管理

拆分窗口，已经开启鼠标支持的可以拖动窗口大小

```bash
# 拆分为上下两个窗口
tmux split-window

# 拆分为左右两个窗口
tmux split-window -h
```

创建并命名窗口

```bash
# 创建窗口
prefix c
# 重命名窗口（前缀+逗号键）
prefix ,
```

在窗口之间切换

```bash
# 切换到下一个窗口（n=next）
prefix n
# 切换到上一个窗口（p=previous）
prefix p
# 切换到某个编号的窗口
prefix <num>
# 如果窗口数量超过 9 个，通过窗口名称查找（f=find）
prefix f
# 如果窗口数量超过 9 个，通过窗口列表查找（w=window）
prefix w
# 关闭一个窗口
exit 或者 prefix &
```

#### 面板管理

切割面板
```bash
# 垂直切割
prefix %
# 水平切割
prefix "
# 面板间来回切换
prefix o
# 最大化或者恢复面板
prefix z
面板布局
# 切换面板布局
prefix <space>
```

其他的参考见 [https://reishin.me/tmux/](https://reishin.me/tmux/)