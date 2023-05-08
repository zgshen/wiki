
# 平铺式桌面管理器 i3wm 

i3wm 配置比较复杂，可以用现成调教好的桌面，比如 Regolith

### 安装

Ubuntu 下的安装 [https://regolith-linux.org/docs/getting-started/install/](https://regolith-linux.org/docs/getting-started/install/)


### 其他配置

status bar 加内存剩余显示。

```bash
# install
sudo apt install i3xrocks-memory

# refresh
regolith-look refresh
```

~~安装 i3lock 锁屏。~~

```bash
sudo apt install i3lock
```

直接 i3lcok 就可以锁屏，`i3lock -i (YOUR_PNG)` 设置锁屏壁纸。

设置快捷键锁屏，加上配置：
```bash
bindsym $mod+Tab exec i3lock # Win+Tab 锁屏
```

### 修改配置

要修改默认的一些配置文件，可以把配置文件复制到当前用户目录下再修改。

比如 i3 config: /etc/regolith/i3/config 到 ~/.config/regolith/i3/config

还有 i3 bar status indicator: /usr/share/i3xrocks 到 ~/.config/regolith/i3xrocks

其他组件也都一样，或者可以干脆把整个 /etc/regolith 文件夹拷贝到 ~/.config/regolith 更直接。

修改样式的一般用 `regolith-look refresh` 就能看到效果了。

### 快捷键

几个常用的：
```
win+shift+? 帮助
win+tab 切换工作空间
win+数字 切换到指定工作空间，不存在就创建
win+t 切换窗口布局
win+space 应用管理
win+shift+f 浮动窗口切换
win+ctrl+m 后台暂存窗口
win+ctrl+a 切换出暂存窗口
```

regolith 快捷键 [https://regolith-linux.org/docs/reference/keybindings/](https://regolith-linux.org/docs/reference/keybindings/)

连锁屏休眠啥都有了，上面的 i3lock 根本不用安装了。

链接打开太慢了，下面是复制过来的，方便看。

  All default key mappings provided in Regolith
---

| Action                                                                             | Key Binding                    |
| ---------------------------------------------------------------------------------- | ------------------------------ |
| [ Launch - Application ](#LaunchApplication)                                       | `⊞ Win` Space                  |
| [ Launch - Browser ](#LaunchBrowser)                                               | `⊞ Win` `Shift` Enter          |
| [ Launch - Command ](#LaunchCommand)                                               | `⊞ Win` `Shift` Space          |
| [ Launch - File Browser ](#LaunchFileBrowser)                                      | `⊞ Win` `Shift` n              |
| [ Launch - File Search ](#LaunchFileSearch)                                        | `⊞ Win` `Alt` Space            |
| [ Launch - Notification Viewer ](#LaunchNotificationViewer)                        | `⊞ Win` n                      |
| [ Launch - Terminal ](#LaunchTerminal)                                             | `⊞ Win` Enter                  |
| [ Launch - This Dialog ](#LaunchThisDialog)                                        | `⊞ Win` `Shift` ?              |
| [ Modify - Bluetooth Settings ](#ModifyBluetoothSettings)                          | `⊞ Win` b                      |
| [ Modify - Carry Window to Workspace 1 - 10](#ModifyCarryWindowtoWorkspace1-10)    | `⊞ Win` `Alt` 0..9             |
| [ Modify - Carry Window to Workspace 11 - 19 ](#ModifyCarryWindowtoWorkspace11-19) | `⊞ Win` `Alt` `Ctrl` 1..9      |
| [ Modify - Containing Workspace ](#ModifyContainingWorkspace)                      | `⊞ Win` `Ctrl` `Shift` ↑ ↓ ← → |
| [ Modify - Display Settings ](#ModifyDisplaySettings)                              | `⊞ Win` d                      |
| [ Modify - Load Window Layout ](#ModifyLoadWindowLayout)                           | `⊞ Win` .                      |
| [ Modify - Move Window to Workspace 1 - 10 ](#ModifyMoveWindowtoWorkspace1-10)     | `⊞ Win` `Shift` 0..9           |
| [ Modify - Move Window to Workspace 11 - 19](#ModifyMoveWindowtoWorkspace11-19)    | `⊞ Win` `Ctrl` `Shift` 1..9    |
| [ Modify - Move to Scratchpad ](#ModifyMovetoScratchpad)                           | `⊞ Win` `Ctrl` m               |
| [ Modify - Next Window Orientation ](#ModifyNextWindowOrientation)                 | `⊞ Win` Backspace              |
| [ Modify - Save Window Layout ](#ModifySaveWindowLayout)                           | `⊞ Win` ,                      |
| [ Modify - Settings ](#ModifySettings)                                             | `⊞ Win` c                      |
| [ Modify - Tile/Float Focus Toggle ](#ModifyTile/FloatFocusToggle)                 | `⊞ Win` `Shift` t              |
| [ Modify - Toggle Bar ](#ModifyToggleBar)                                          | `⊞ Win` i                      |
| [ Modify - Wifi Settings ](#ModifyWifiSettings)                                    | `⊞ Win` w                      |
| [ Modify - Window Floating Toggle ](#ModifyWindowFloatingToggle)                   | `⊞ Win` `Shift` f              |
| [ Modify - Window Fullscreen Toggle ](#ModifyWindowFullscreenToggle)               | `⊞ Win` f                      |
| [ Modify - Window Layout Mode ](#ModifyWindowLayoutMode)                           | `⊞ Win` t                      |
| [ Modify - Window Position ](#ModifyWindowPosition)                                | `⊞ Win` `Shift` k j h l        |
| [ Modify - Window Position ](#ModifyWindowPosition)                                | `⊞ Win` `Shift` ↑ ↓ ← →        |
| [ Navigate - Next Workspace ](#NavigateNextWorkspace)                              | `⊞ Win` Tab                    |
| [ Navigate - Next Workspace ](#NavigateNextWorkspace)                              | `⊞ Win` `Alt` →                |
| [ Navigate - Previous Workspace ](#NavigatePreviousWorkspace)                      | `⊞ Win` `Alt` ←                |
| [ Navigate - Previous Workspace ](#NavigatePreviousWorkspace)                      | `⊞ Win` `Shift` Tab            |
| [ Navigate - Relative Window ](#NavigateRelativeWindow)                            | `⊞ Win` k j h l                |
| [ Navigate - Relative Window ](#NavigateRelativeWindow)                            | `⊞ Win` ↑ ↓ ← →                |
| [ Navigate - Scratchpad ](#NavigateScratchpad)                                     | `⊞ Win` `Ctrl` a               |
| [ Navigate - Window by Name ](#NavigateWindowbyName)                               | `⊞ Win` `Ctrl` Space           |
| [ Navigate - Workspace 11 - 19 ](#NavigateWorkspace11-19)                          | `⊞ Win` `Ctrl` 1..9            |
| [ Navigate - Workspaces 1-10 ](#NavigateWorkspaces1-10)                            | `⊞ Win` 0..9                   |
| [ Resize - Enter Resize Mode ](#ResizeEnterResizeMode)                             | `⊞ Win` r                      |
| [ Session - Exit App ](#SessionExitApp)                                            | `⊞ Win` `Shift` q              |
| [ Session - Lock Screen ](#SessionLockScreen)                                      | `⊞ Win` Escape                 |
| [ Session - Logout ](#SessionLogout)                                               | `⊞ Win` `Shift` e              |
| [ Session - Power Down ](#SessionPowerDown)                                        | `⊞ Win` `Shift` p              |
| [ Session - Reboot ](#SessionReboot)                                               | `⊞ Win` `Shift` b              |
| [ Session - Refresh Session ](#SessionRefreshSession)                              | `⊞ Win` `Shift` r              |
| [ Session - Reload i3 Config ](#SessionReloadi3Config)                             | `⊞ Win` `Shift` c              |
| [ Session - Restart i3 ](#SessionRestarti3)                                        | `⊞ Win` `Ctrl` r               |
| [ Session - Sleep ](#SessionSleep)                                                 | `⊞ Win` `Shift` s              |
| [ Session - Terminate App ](#SessionTerminateApp)                                  | `⊞ Win` `Alt` q                |