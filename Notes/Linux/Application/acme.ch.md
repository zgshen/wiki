#### Nginx 配置

先看Nginx配置，如果是重定向的需要设置一下㔿转发 /.well-known/acme-challenge/

```bash
server {
    listen       80;
    server_name  ai.zguishen.com;

    location ^~ /.well-known/acme-challenge/ {
        default_type "text/plain";
        allow all;
        root /var/www/ai.zguishen.com/;
    }

    location ^~ / {
        proxy_pass http://127.0.0.1:8888/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

```

#### acme.sh 脚本

项目地址 https://github.com/acmesh-official/acme.sh/ 

生成证书：
```bash
# 设置下邮箱
./acme.sh --register-account -m example@gmail.com

# 生成证书，-w 内容为 Nginx 中配置的 root 位置，命令输出有错误的话可以加上 --debug 看日志
./acme.sh  --issue -d ai.zguishen.com -w /var/www/ai.zguishen.com/
```

安装证书
```bash
./acme.sh --install-cert -d ai.zguishen.com \
--key-file       /etc/nginx/conf.d/ai.zguishen.com.key.pem  \
--fullchain-file /etc/nginx/conf.d/ai.zguishen.com.pem \
--reloadcmd     "nginx -s reload"
```

修改 Nginx conf

```bash
server {
    listen       80;
    server_name  ai.zguishen.com;

    location ^~ /.well-known/acme-challenge/ {
        default_type "text/plain";
        allow all;
        root /var/www/ai.zguishen.com/;
    }

    location ^~ / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl;
    server_name  ai.zguishen.com;

    ssl_certificate   /etc/nginx/conf.d/ai.zguishen.com.pem;
    ssl_certificate_key  /etc/nginx/conf.d/ai.zguishen.com.key.pem;

    location ^~ / {
        proxy_pass http://127.0.0.1:8888/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

重新加载 Nginx `nginx -s reload`，如果怕不生效的话就先停了 `nginx -s stop` 再启动  `nginx -c /etc/nginx/nginx.conf`
