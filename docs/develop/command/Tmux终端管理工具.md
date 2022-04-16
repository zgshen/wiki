
# Tmux终端管理工具

安装

```bash
sudo apt install tmux -y
```

设置鼠标支持，可上下滚动查看，按 shift 鼠标选中复制复制内容，点击选中其他 tmux 窗口

```bash
tmux set mouse on
```

基本操作
```bash
# 创建一个指定名称的 session
tmux new -s test
tmux new -s dev

# 列出所有 tmux session
tmux ls
```

按 Ctrl+D 退出当前 tmux
```bash
# 进入指定 tmux session
tmux attach -t dev

# 切换 tmux session
tmux swtich -t test
```

拆分窗口，已经开启鼠标支持的可以拖动窗口大小

```bash
# 拆分为上下两个窗口
tmux split-window

# 拆分为左右两个窗口
tmux split-window -h
```

重命名 dev 会话名称为 nemdev
```bash
tmux rename-session -t dev newdev
```

关闭 session
```bash
# 关闭 test 会话
tmux kill-session -t test
# 关闭服务器，所有的会话都将关闭
tmux kill-server 
```