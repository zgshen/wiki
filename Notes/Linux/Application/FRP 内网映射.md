
项目GitHub地址 https://github.com/fatedier/frp

项目文档地址 https://gofrp.org/

### 配置

frpc.ini  客户端配置

frps.ini  服务端配置

### 映射一个内网应用例子

#### 客户端

server_addr 公网服务器ip，custom_domains自定义域名或公网服务器ip

frpc.ini配置
```ini
[common]
server_addr = 42.193.139.xx
server_port = 7123

[spring]
type = http
local_ip = 127.0.0.1
local_port = 8000
remote_port = 12193
custom_domains = 42.193.139.xx
```

frps.ini配置
```ini
[common]
bind_port = 7123
vhost_http_port = 12193
```

服务端指定配置启动：./frps -c frps.ini
访问 http://42.193.139.78:12193 即可访问到客户端内网8000端口上的服务。




https://github.com/zgshen/lin/blob/master/lin-config-spring/lin-config-client-dev.yml
https://github.com/zgshen/lin/master/lin-config-spring/lin-config-client-dev.yml