# 1. apt 相关命令

## 1.1. apt
### 1.1.1. apt 删除软件

apt remove 和 apt purge 的区别：

- `apt remove` 会删除软件包而保留软件的配置文件。
- `apt purge` 会同时清除软件包和软件的配置文件。

### 1.1.2. apt 搜索
`apt-cache search openjdk` 搜索查询openjdk可安装版本

### 1.1.3. apt只下载软件单不安装

```bash
sudo apt download package-name
```
### 1.1.4. apt 禁止软件更新

1. 首先，确定你想要禁止更新的软件包的名称。你可以使用 `apt list --upgradable` 命令来查看可以更新的软件包列表。
    
2. 使用 `apt-mark` 命令将软件包标记为 "hold" 状态。例如：
    
```shell
sudo apt-mark hold package_name`
```

请将 `package_name` 替换为你想要禁止更新的软件包的实际名称。

3. 确保软件包已经成功地被标记为 "hold" 状态。你可以使用以下命令检查：

```shell
apt-mark showhold
```

如果你希望解除某个软件包的 "hold" 状态，可以使用 `apt-mark unhold` 命令。例如：

```shell
sudo apt-mark unhold package_name
```

这将解除对该软件包的更新禁止。
## 1.2. apt-fast

apt 安装下载过慢的话可以使用 apt-fast 多线程并行下载，操作和 apt 一样，将命令中的 apt 替换成 apt-fast 就行。

## 1.3. 其他
## 1.4. update-alternatives 命令

`update-alternatives --display java` 查看 Java 的版本信息
