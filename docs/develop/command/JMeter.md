# JMeter 压测工具

### 配置

`sudo vi /etc/profile` 最下面添加：

```bash
export JAVA_HOME=/home/nathan/app/jdk/jdk1.8.0_221
export JRE_HOME=/home/nathan/app/jdk/jdk1.8.0_221/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
export PATH=/home/nathan/app/apache-jmeter-5.4.3/bin:$PATH
```

JMeter 在高版本 JDK 有些问题，比如我本机环境 JDK17 无法保存测试脚本，所以设置下用 JDK8 跑。

`source /etc/profile` 让配置生效。但是 profile 是在系统启动的时候只加载一次的，source 命令只是让修改后 profile 只在当前终端生效，要让全局生效只能重启系统。

要让当前 shell 生效，编辑 `vi ~/.zshrc`，如果是 bash 那就是编辑 `vi ~/.bashrc`，同样添加以上内容。

`source  ~/.zshrc` 新开终端窗口就都生效了。

### 压测

Don't use GUI mode for load testing !, only for Test creation and Test debugging.

For load testing, use CLI Mode (was NON GUI):

jmeter -n -t [jmx file] -l [results file] -e -o [Path to web report folder] & increase Java Heap to meet your test requirements:

Modify current env variable HEAP="-Xms1g -Xmx1g -XX:MaxMetaspaceSize=256m" in the jmeter batch file. Check : https://jmeter.apache.org/usermanual/best-practices.html

JMeter 启动信息已经说得很清楚了，GUI 只用来编写脚本，不要用来最负载测试。

用命令做压测例子：

```bash
jmeter -n -t local-java-sb.jmx -l res.jtl -e -o report
```

local-java-sb.jmx 是编写的脚本，res.jtl 是取样器日志，生成的报告放在 report 文件夹下，html 文件，可以用浏览器打开。

