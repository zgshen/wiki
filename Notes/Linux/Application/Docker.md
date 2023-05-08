### 安装

[Ubuntu 基本配置（旧）](../Ubuntu/Ubuntu%20基本配置（旧）.md)

原软件源中的 Docker 可能不是最新版本的，更新软件包索引，并且安装必要的依赖软件，来添加一个新的 HTTPS 软件源：
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```


使用下面的 curl 导入源仓库的 GPG key：
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```


将 Docker APT 软件源添加到你的系统：
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```


现在，Docker 软件源被启用了，你可以安装软件源中任何可用的 Docker 版本。

安装 Docker 最新版本，运行下面的命令
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```


非 root 用户执行 docker 命令权限不足，需要加到 docker 用户组并切换到 docker 用户组
```bash
# usermod用于修改用户账号
sudo usermod -aG docker {指定用户名}
# newgrp命令是以相同的账户名，不同的组群身份登录系统
newgrp docker

# sudo usermod -aG docker akari && newgrp docker
```

远程连接
```json
sudo vi /lib/systemd/system/docker.service
#加上 -H tcp://0.0.0.0:2375
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H fd:// --containerd=/run/containerd/containerd.sock

sudo systemctl daemon-reload // 加载docker守护线程
sudo systemctl restart docker // 重启docker
```


### 命令

```
docker verison 查看状态

docker service statrt 启动

docker images 查看镜像

docker run 镜像名 运行镜像

docker ps 查看正在运行的镜像

docker exec -it ID头几位 bash 进入运行镜像内部

exit 回到原主机

docker run -d -p 80:8080 tomcat 指定端口号运行，docker8080端口映射到主机80端口，外部访问主机ip80端口

docker stop 镜像id

docker container ls -all 列出所有镜像

docker rm -f $(docker ps -a -q) 删除镜像

docker build -t lin-vue:1.0.0 .    构建镜像，最后 . 表示Dcokerfile在当前目录

docker stop $(docker ps -a -q) // stop停止所有容器

docker rm $(docker ps -a -q) // remove删除所有容器
```


### VUE 项目部署

Docfile
```bash
FROM nginx:1.19.0
COPY default.conf /etc/nginx/conf.d/
COPY dist /usr/share/nginx/html
RUN chown -R nginx:nginx /usr/share/nginx/html
```

```bash
docker build -t lin-vue:1.0.0 .
```

说明：
- imageName 镜像名称，自定义
- version 镜像版本号，自定义
- `.` 点号代表根据同级路径下的Dockerfile构建镜像

### docker-compose

下载
https://github.com/docker/compose/releases

```bash
# nathan @ nathan-tp in ~/Downloads [15:32:37] C:1
$ sudo cp docker-compose-linux-x86_64 /usr/local/bin/docker-compose

# nathan @ nathan-tp in /usr/local/bin [15:33:05] 
$ sudo chmod +x /usr/local/bin/docker-compose

# nathan @ nathan-tp in /usr/local/bin [15:33:19] 
$ docker-compose --version
Docker Compose version v2.4.1
```


### 在IDEA的使用

先开启远程访问（systemd管理的系统，如果是upstart，则需要编辑/etc/default/docker）

```bash
sudo vi /usr/lib/systemd/system/docker.service
```

找到 ExecStart 这行，在最后面添加 -H tcp://0.0.0.0:2375，如下：

```bash
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always
```

重新加载服务然后重启

```bash
systemctl daemon-reload  
systemctl restart docker
```

idea 中 docker 设置，TCP socket 的 URL 填入链接 tcp://192.168.0.103:2375，连接成功可以在service栏看到docker容器：
![](../Assets/Pasted%20image%2020220722152822.png)

### 国内加速
```
sudo mkdir -p /etc/systemd/system/docker.service.d
vi /etc/systemd/system/docker.service.d/http-proxy.conf
```
内容
```bash
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80"
Environment="HTTPS_PROXY=https://proxy.example.com:443"
```

或者直接在终端
```bash
export https_proxy=http://127.0.0.1:10808 http_proxy=http://127.0.0.1:10808 all_proxy=socks5://127.0.0.1:10808
```


### RabbitMQ

```bash
docker pull rabbitmq

docker run -d -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_VHOST=/ -e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest --hostname ubuntuRabbit --name rabbitmq rabbitmq
```

参数说明：
```
-d：表示在后台运行容器；
-p：将容器的端口 5672（应用访问端口）和 15672 （控制台Web端口号）映射到主机中；
-e：指定环境变量：
	RABBITMQ_DEFAULT_VHOST：默认虚拟机名；
	RABBITMQ_DEFAULT_USER：默认的用户名；
	RABBITMQ_DEFAULT_PASS：默认的用户密码；
--hostname：指定主机名（RabbitMQ 的一个重要注意事项是它根据所谓的 节点名称 存储数据，默认为主机名）；
--name rabbitmq：设置容器名称；
rabbitmq：容器使用的镜像名称；
```

#### 设置docker启动的时候自动启动rabbitmq
```bash
docker update rabbitmq --restart=always
```

#### 启动Rabbit_management

方法一：
1）、先进入rabbitmq容器
```bash
docker exec -it rabbitmq /bin/bash
```
2）、再执行命令
```bash
rabbitmq-plugins enable rabbitmq_management
```

方法二：
```bash
docker exec -it rabbitmq rabbitmq-plugins enable rabbitmq_management
```

然后就能在浏览器访问控制台，地址 http://localhost:15672