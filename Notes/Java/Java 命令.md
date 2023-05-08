# Java 命令

ubuntu 下版本查询

```bash
# nathan @ vb-ubuntu in /etc/alternatives [14:23:13] C:2
$ ls -l /usr/bin/java
lrwxrwxrwx 1 root root 22 Nov 18 14:08 /usr/bin/java -> /etc/alternatives/java

# nathan @ vb-ubuntu in /etc/alternatives [14:23:16]
$ ls -l /etc/alternatives/java
lrwxrwxrwx 1 root root 43 Nov 18 14:08 /etc/alternatives/java -> /usr/lib/jvm/java-17-openjdk-amd64/bin/java
```

编译

`javac -cp jsoup-1.14.2.jar Task.java`

执行

`java Task`

指定 jar 目录、JVM 参数后台执行 jar 包，日志输出到 console.out 文件

java -Dloader.path="lib/" -Xms512m -Xmx1024m -jar luckdraw-*.jar > console.out 2>&1 &