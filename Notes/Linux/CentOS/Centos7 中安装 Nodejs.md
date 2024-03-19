# Centos7 中安装 Nodejs

[Centos7 中安装 Nodejs_kong7828675的博客-CSDN博客]<[https://blog.csdn.net/kong7828675/article/details/108824617](https://blog.csdn.net/kong7828675/article/details/108824617)>

### **一 下载**

下载地址：

[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

![https://img-blog.csdnimg.cn/20200927113026265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tvbmc3ODI4Njc1,size_16,color_FFFFFF,t_70#pic_center](https://img-blog.csdnimg.cn/20200927113026265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tvbmc3ODI4Njc1,size_16,color_FFFFFF,t_70#pic_center)

### **二 解压/安装**

将文件放入 /usr/local 下，进行解压，并重命名文件夹：

```
cd /usr/local
tar -xvf node-v12.18.3-linux-x64.tar
mv node-v12.18.3-linux-x64 nodejs
123
```

### **三 配置**

全局引用 / 配置软连接

```
ln -s /usr/local/nodejs/bin/npm /usr/local/bin
ln -s /usr/local/nodejs/bin/node /usr/local/bin
12
```

注：/usr/local/nodejs/bin/npm 此地址为安装的nodejs的文件的绝对地址下的npm和node所在位置，根据每个人的情况不同。

### **四 查看**

```
node -v
npm -v
```

### **五 升级更新**

安装 n 包

```shell
npm install -g n
```

你需要全局安装此包，因为它在根目录管理 Node 版本。

安装新版本的 Node

```shell
n lts
n latest
```

上面两个命令安装长期支持和最新版本的 Node.js。

**删除以前安装的版本**

```shell
n prune
```

此命令会删除以前安装的版本的缓存版本，只保留最新安装的版本。

