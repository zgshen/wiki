## Dockerfile

可参考教程 https://yeasy.gitbook.io/docker_practice/
官方文档 https://docs.docker.com/reference/

Dockerfile 文件一般放在项目根目录。

例子

```bash
FROM nginx

COPY nginx.conf /etc/nginx/nginx.conf

RUN chown -R nginx:nginx /usr/share/nginx/html

ENTRYPOINT ["nginx", "-c"] # 定参
CMD ["/etc/nginx/nginx.conf"] # 变参
```

### 指令详解

#### FROM
指定拉取镜像，不带版本号，默认下载 latest 版本，也可以指定 FROM nginx:1.19.0

#### ENV
设置环境变量
```bash
# JVM 堆内存设置
ENV JAVA_OPTS "-Xmx300M -Xms300M"
```

#### COPY
硬盘上的文件复制到容器内部路径

#### VOLUME
定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。

作用：
- 避免重要的数据，因容器重启而丢失，这是非常致命的。
- 避免容器不断变大。

格式：
```bash
VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>
```

在启动容器 docker run 的时候，我们可以通过 -v 参数修改挂载点。

#### RUN
执行命令。

```bash
RUN ["可执行文件", "参数1", "参数2"]
例如：
RUN ["./test.php", "dev", "offline"] 等价于 RUN ./test.php dev offline
```

如果有多个命令先后执行，可以用 && 符号连接起来。

```bash
RUN cd /app && ./test.sh
```

#### CMD
和 RUN 相似，也是执行命令，不同点：
- CMD 在 docker run 时运行；
- RUN 是在 docker build。

例子：
```bash
CMD cd /app && python3 -m http.server 3000 >/dev/null 2>&1
```

#### ENTRYPOINT
类似于 CMD 指令，但其不会被 docker run 的命令行参数指定的指令所覆盖，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序。

但是, 如果运行 docker run 时使用了 --entrypoint 选项，将覆盖 ENTRYPOINT 指令指定的程序。

- **优点**：在执行 docker run 的时候可以指定 ENTRYPOINT 运行所需的参数。
- **注意**：如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。

格式：
```bash
ENTRYPOINT ["<executeable>","<param1>","<param2>",...]
```

可以搭配 CMD 命令使用：一般是变参才会使用 CMD ，这里的 CMD 等于是在给 ENTRYPOINT 传参，以下示例会提到。

示例,假设已通过 Dockerfile 构建了 nginx:test 镜像：
```bash
FROM nginx

ENTRYPOINT ["nginx", "-c"] # 定参
CMD ["/etc/nginx/nginx.conf"] # 变参
```


```bash
docker run  nginx:test
# 不带参等价于
nginx -c /etc/nginx/nginx.conf


docker run  nginx:test -c /etc/nginx/new.conf
# 带参数等价于
nginx -c /etc/nginx/new.conf

```

看了另一个例子，Dockerfile
```bash
FROM ubuntu:20.04
RUN apt update && apt install curl -y
ENTRYPOINT [ "curl", "-s", "http://ipinfo.io/ip" ]
```

```bash
# 编译
sudo docker build -t queryip .
# 运行的时候就可以带上自己相加的参数了，如 -i
sudo docker run queryip -i

# 实机测试
# nathan @ VM-20-15-centos in ~/queryip [21:07:22] C:1
$ sudo docker run queryip -i
HTTP/1.1 200 OK
access-control-allow-origin: *
content-type: text/html; charset=utf-8
content-length: 13
date: Sun, 23 Oct 2022 13:08:27 GMT
x-envoy-upstream-service-time: 1
strict-transport-security: max-age=2592000; includeSubDomains
Via: 1.1 google

42.193.139.78%  
```

### 编译和运行

#### 编译
看上面查询ip的例子，在 Dockerfile 所在文件夹下执行命令，后面的 . 指的是容器的上下文环境。
```bash
sudo docker build -t queryip .
```

指定 Dockerfile 文件路径编译。
```bash
sudo docker build -f /app/Dockerfile -t queryip .
```

命令其他选项可看 https://www.runoob.com/docker/docker-build-command.html

#### 运行

直接运行，如上面的 `sudo docker run queryip -i`
```bash
# nathan @ VM-20-15-centos in ~/queryip [21:07:22] C:1
$ sudo docker run queryip -i
HTTP/1.1 200 OK
access-control-allow-origin: *
content-type: text/html; charset=utf-8
content-length: 13
date: Sun, 23 Oct 2022 13:08:27 GMT
x-envoy-upstream-service-time: 1
strict-transport-security: max-age=2592000; includeSubDomains
Via: 1.1 google

42.193.139.78% 
```

后台运行加上 -d，端口映射 -p
```bash
# 后台运行镜像 sc-py:v1，-p 后面是 宿主机ip:容器ip 的映射
sudo docker run -d -p 3000:3000 sc-py:v1
```

命令其他选项可看 https://www.runoob.com/docker/docker-run-command.html

#### 删除

删除容器
```bash
sudo docker rm 53fc978b3e85
```

删除镜像
```bash
sudo docker image rm 53fc978b3e85
```

### 推送到远程镜像库

先要登录，比如登录到腾讯云的docker镜像，用户名、密码和命名空间都是在腾讯云上设置好的。
```bash
docker login ccr.ccs.tencentyun.com --username=100022253382
```

输出
```bash
# nathan @ nathan-tp in ~/app/k8s/py-demo [14:52:03] C:130
$ docker login ccr.ccs.tencentyun.com --username=100022253382
Password: 
WARNING! Your password will be stored unencrypted in /home/nathan/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```


打个tag推送上去
```bash
# nathan @ nathan-tp in ~/app/k8s/py-demo [14:53:04] 
$ docker tag py-server:latest ccr.ccs.tencentyun.com/dev-project/py-server:latest

# nathan @ nathan-tp in ~/app/k8s/py-demo [14:56:59] C:130
$ docker push ccr.ccs.tencentyun.com/dev-project/py-server:latest
The push refers to repository [ccr.ccs.tencentyun.com/dev-project/py-server]
7ade694a8554: Pushed
3cd5b87f7a47: Pushed 
7db5fd674176: Pushed
3d1475f3fac3: Pushed 
6666686122fd: Pushed
994393dc58e7: Pushed 
latest: digest: sha256:97b6c0ad106918b01e9887bb7a5c713037a8524aac21ce6b25555726fc17d73a size: 1575
```

![](../Assets/Pasted%20image%2020221028150341.png)


## docker-compose

例子
```bash
version: '3'
services:
  server:
    build:
      context: backend
      dockerfile: Dockerfile
    volumes:
      - type: bind
        source: /opt/sqlite/data.db
        target: /app/backend/config/data.db
    ports:
      - "8080:8080"

  client:
    build:
      context: frontend
      dockerfile: Dockerfile
    ports:
      - "12880:12880"
    depends_on:
      - server
    links:
      - server
```

volumes 用 source 和 target 映射单文件
这里的 depends_on 和 links 表示 server 网络传递给 client，然后 client 就能通过 http://server:port/api/... 的方式访问 server 的服务了。