#### 1.停止服务

立即停止服务

```
nginx -s stop
```

systemctl停止
```
systemctl stop nginx.service
```

killall方法杀死进程
```
killall nginx
```

#### 2.启动Nginx

直接启动 `nginx`

systemctl命令启动 `systemctl start nginx.service`

#### 3.查看启动记录
```
ps aux|grep nginx
```

#### 4.重启Nginx服务
```
systemctl restart nginx.service
```

#### 5.重新载入配置文件
```
nginx -s reload
```

#### 6.查看端口
```
netstat -tlnp
```


#### 7. 用户静态目录问题

```
server {
	listen  3000;
	listen  [::]:3000;
	root  /home/nathan/app;
}
```

root设置为在用户目录下的静态目录，访问3000端口页面，会提示没权限，即没有 /home/nathan/app 下的 index.html 的权限。

参考 https://stackoverflow.com/a/46083622

```
# 把用户加到nginx用户组中
sudo usermod -a -G your_user nginx
# 需要加上执行权限，即710第二位的1
chmod 710 /home/your_user 
```

#### 8. 测试配置文件

-c <path_to_config>：使用指定的配置文件而不是 conf 目录下的 nginx.conf 。  
-t：测试配置文件是否正确，在运行时需要重新加载配置的时候，此命令非常重要，用来检测所修改的配置文件是否有语法错误。

```bash
sudo nginx -tc /etc/nginx/nginx.conf
```
