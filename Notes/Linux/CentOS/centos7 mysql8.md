# centos7 mysql8

到 [https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/) 下载 rpm 文件

**Red Hat Enterprise Linux 7 / Oracle Linux 7 (Architecture Independent), RPM Package**

```bash
# download
wget https://repo.mysql.com//mysql80-community-release-el7-4.noarch.rpm

# install
yum localinstall mysql80-community-release-el7-4.noarch.rpm
yum install mysql-community-server
```

修改配置文件，改端口和编码

```bash
[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4

[mysqld]
port = 13306
character-set-client-handshake = FALSE
character-set-server=utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'
```

启动后要先改 root 密码

```bash
# 启动
service mysqld start
# 或者
#systemctl start mysqld.service

# 看下端口
$ ss -lnp|grep mysql
u_str  LISTEN     0      128    /var/lib/mysql/mysql.sock 125315696             * 0                   users:(("mysqld",pid=11683,fd=27))
u_str  LISTEN     0      70     /var/run/mysqld/mysqlx.sock 125315692             * 0                   users:(("mysqld",pid=11683,fd=23))
u_dgr  UNCONN     0      0         * 125315687             * 8065                users:(("mysqld",pid=11683,fd=3))
tcp    LISTEN     0      128    [::]:13306              [::]:*                   users:(("mysqld",pid=11683,fd=25))
tcp    LISTEN     0      70     [::]:33060              [::]:*                   users:(("mysqld",pid=11683,fd=22))

# 看下原始 root 密码
grep 'password' /var/log/mysqld.log

# 用查到的密码登录 root
mysql -uroot -p

# 先改 root 密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '123';

# 看下编码，utf8mb4 没错
mysql> SHOW VARIABLES WHERE Variable_name LIKE 'character_set_%' OR Variable_name LIKE 'collation%';
+--------------------------+--------------------------------+
| Variable_name            | Value                          |
+--------------------------+--------------------------------+
| character_set_client     | utf8mb4                        |
| character_set_connection | utf8mb4                        |
| character_set_database   | utf8mb4                        |
| character_set_filesystem | binary                         |
| character_set_results    | utf8mb4                        |
| character_set_server     | utf8mb4                        |
| character_set_system     | utf8mb3                        |
| character_sets_dir       | /usr/share/mysql-8.0/charsets/ |
| collation_connection     | utf8mb4_unicode_ci             |
| collation_database       | utf8mb4_unicode_ci             |
| collation_server         | utf8mb4_unicode_ci             |
+--------------------------+--------------------------------+
11 rows in set (0.01 sec)
```
