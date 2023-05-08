
#### 查询

在Ubuntu中，每个PPA源是单独存放在_/etc/apt/sources.list.d/_文件夹中的，进入到该文件夹，使用ls命令查询即可列出当前系统添加的PPA源。

#### 添加

```bash
sudo add-apt-repository ppa:ownername/projectname
sudo apt update
sudo apt install something
```


> 注意，添加了PPA源时，记得update一下，不然在install的时候会出现找不到安装包的情况。

#### 修改

用文本编辑器修改 `/etc/apt/sources.list.d/` 文件夹下的文件内容即可。

#### 删除

使用sudo rm命令删除 `/etc/apt/sources.listd` 文件夹中指定的PPA源文件即可。

或者使用命令 

```bash
sudo add-apt-repository --remove ppa:whatever/ppa
```