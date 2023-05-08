	
### 编辑器

General-setting Editor

custom，输入框填 gedit（Ubuntu 自带的文本编辑器）


### 自定义规则


Setting-Profiles-Parsers 点击 edit，

添加规则，多个订阅的话规则要分别添加，如下：

```yml
parsers: # array
  - url: https://example1.com
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX,bing.com,DIRECT
  - url: https://example2.com
    yaml:
      prepend-rules:
        - DOMAIN-SUFFIX,bing.com,DIRECT
```

或者在 Profiles 右键对应订阅，点击 Rules 添加规则，不过在这里添加的规则在订阅更新后会丢失，所以用上面的方法就行了。

chrome 配合 SwitchyOmega 一起使用切换代理比较方便，但是流量最终还是会被 clash 管理，导致有些chrome代理是无效的，在 clash 判断一遍后又走直连。这里也同样可以用规则设置 chrome 的流量由 SwitchyOmega 管理哪些走代理，哪些不走。

同样在 Profiles 右键对应订阅，点击 Rules 添加规则，content 填 chrome，type 选择 PROCESS-NAME，Proxy or Policy 选择你的代理线路。

这样下来，SwitchyOmega 判断流量直连的直接通过，代理的转给 clash， clash 看到 chrome 来的流量不再做判断，直接全部走代理。


### 安装 service mode 问题

参考 https://github.com/Fndroid/clash_for_windows_pkg/issues/3464

**[Fndroid](https://github.com/Fndroid)** commented [on Sep 19, 2022](https://github.com/Fndroid/clash_for_windows_pkg/issues/3464#issuecomment-1250788591)

[@amir-valizadeh](https://github.com/amir-valizadeh) steps to install Service Mode manually:

1.  copy folder `APP HOME PATH/resources/static/files/linux/[x64|arm64]/service` to `Home Directory`(open from General) folder
2.  create file `/usr/lib/systemd/system/clash-core-service.service` with content:
    
    ```bash
    [Unit]
    Description=Clash core service created by Clash for Windows
    After=network-online.target nftables.service iptabels.service
    
    [Service]
    Type=simple
    ExecStart=<path to Home Directory/service/clash-core-service>
    Restart=always
    RestartSec=5
    
    [Install]
    WantedBy=multi-user.target
    ```
    
**you have to change the `ExecStart` path to `Home Directory/service/clash-core-service`!!!**

3.  run commands:
    
```
sudo systemctl enable clash-core-service
```
    
    
```
sudo systemctl start clash-core-service
```
