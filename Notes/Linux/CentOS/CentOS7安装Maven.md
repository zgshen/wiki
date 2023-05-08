# CentOS7安装Maven

## **1、下载**

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
```

也可以在浏览器去[maven官网](http://maven.apache.org/download.cgi)下载需要的版本，这里安装的是二进制包，所以选择“-bin.tar.gz”结尾的包

## **2、解压**

```bash
tar -xf apache-maven-3.6.3-bin.tar.gz -C /usr/local/
mv /usr/local/apache-maven-3.6.3/ maven3.6

```

## **3、加入环境变量**

在/etc/profile文件最下方加入新的一行`export PATH=$PATH:/usr/local/maven3.6/bin`

添加完后，执行`source /etc/profile`,让配置生效

验证：

执行`which mvn`显示/usr/local/maven3.6/bin/mvn就说明配置成功了

## **4、JAVA环境**

运行maven需要Java环境----系统安装有jdk，并且在系统中配置了JAVA_HOME。

## 5**、修改配置文件**

因为之后需要用Jenkins构建项目,所以最好是指定下依赖的下载位置

```bash
mkdir /usr/local/maven3.6/repository
vi /usr/local/maven3.6/conf/settings.xml
复制代码
```

添加指定位置

```bash
#指定下载位置
<localRepository>/usr/local/maven/repository</localRepository>
#设定阿里镜像
<mirror>
     <id>alimaven</id>
     <name>aliyun maven</name>
     <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
     <mirrorOf>central</mirrorOf>
</mirror> 

```