**一、修改默认的443端口**

1.编辑文件并修改文件中的443端口为其他端口  
（1）xray-core

vim /etc/v2ray-agent/xray/conf/02_VLESS_TCP_inbounds.json

（2）v2ray-core

vim /etc/v2ray-agent/v2ray/conf/02_VLESS_TCP_inbounds.json

2.重启服务器

3.备注：修改默认端口后可能会影响某些功能，但是不影响使用，注意修改订阅的默认端口

**二、修改默认的80端口为其他端口**

1.编辑文件并修改文件中的80端口为其他端口  
修改第一个server和第三个server中的80端口，有80的地方都要修改

vim /etc/nginx/conf.d/alone.conf

2.重启nginx

systemctl restart nginx