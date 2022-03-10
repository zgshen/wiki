
# code-server 搭建

官方的文档 [https://coder.com/docs/code-server/latest/install](https://coder.com/docs/code-server/latest/install)

[https://github.com/coder/code-server/releases](https://github.com/coder/code-server/releases) 看下发布版本。

以我的服务器是 CentOS7 为例，文档安装方法:

```
curl -fOL https://github.com/coder/code-server/releases/download/v$VERSION/code-server-$VERSION-amd64.rpm
sudo rpm -i code-server-$VERSION-amd64.rpm
sudo systemctl enable --now code-server@$USER
# Now visit http://127.0.0.1:8080. Your password is in ~/.config/code-server/config.yaml
```

$VERSION 替换成 GitHub 上要想的版本，$USER 替换成你的用户，装完就可以用 systemctl status 看下服务状态。

由于不在 Https 下 markdown 文件的预览有问题，GitHub 的 issue 说是要上 Https 才行，那就用 Nginx 配置个域名上 Https，证书用 Certbot 搞就行了。 

```
server {
    server_name  code.zkcing.com;
    location ^~ / {
        proxy_pass http://127.0.0.1:12809/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_set_header Accept-Encoding gzip;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/code.zkcing.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/code.zkcing.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
```

```java
public class LambdaTest {

    public LambdaTest() {
    }
    public LambdaTest(String str) {
        //use param str to do something
    }

    public static void interfaceTest(SingleFncInterface singleFunInterface) {
        singleFunInterface.doSomething("123");
    }

    public void simpleMenthod(String str) {
        System.out.println("simple method. str is:");
    }

    public static void staticMenthod(String str) {
        System.out.println("static menthod. str is:");
    }
}
```