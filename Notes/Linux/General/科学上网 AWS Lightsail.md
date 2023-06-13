前两天尝试了 Azure 的免费试用功能，搭建了一个 Linux 机器，不过免费的200刀是有使用限制的，搞不好要被 Azure 反撸，所以就把订阅取消了。恰好看到 AWS 这边的 lightsail 实例看起来更划算，最低配3.5刀每月1T流量，前三月免费，不过是仅限新号首次开机，这些云服务商套路都挺多的，多关注账单，有问题赶紧停掉。

### 开通

先注册 AWS，需要绑银行卡，AWS lightsail 的入口 [https://lightsail.aws.amazon.com/](https://lightsail.aws.amazon.com/)，新建vps，区域一般选近的新加坡或日本，选3.5刀前三月免费那个就够用了，创建个密钥用于登录，把密钥下载下来。

下载到本机的密钥放到当前用户 `~/.ssh` 目录下，编辑该目录下的 config 文件，贴上配置：

```bash
# 创建的是Debian系统机器，这里host名随便写的
Host aws-debian
    HostName ${aws机器ip}
    IdentityFile ~/.ssh/sgssh.pem
    # aws默认给你创建了一个admin用户
    User admin
```

然后就可以 `ssh aws-debian` 登录服务器了。

在 AWS lightsail 后台，点击竖向三点里面的管理，看到联网选项可以设置防火墙增删端口。

### 安装 REALITY

嫌麻烦用就用别人做好的脚本（怕有问题就先看下脚本内容），参考 [https://www.v2ray-agent.com/archives/1680104902581](https://www.v2ray-agent.com/archives/1680104902581) 的第二种安装方式，比较简单，域名默认就行，端口指定一个，记得到后台防火墙添加上。

REALITY 现在有很多客服端都支持了:
- iOS：[Shadowrocket](https://apps.apple.com/ca/app/shadowrocket/id932747118)
- Android：[v2rayNG](https://github.com/2dust/v2rayNG)
- PC：[Clash Verge](https://github.com/zzzgydi/clash-verge)

### 开启 BBR

```bash
wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh
chmod 755 /opt/bbr.sh
/opt/bbr.sh 
```

脚了来自 [https://teddysun.com/489.html](https://teddysun.com/489.html)