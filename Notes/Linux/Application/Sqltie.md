
#### 导入执行 sql 的脚本

```bash
#!/bin/bash  
# sudo ./db.sh  
mkdir /opt/sqlite  
SQL_FILE=db.sql  
DB_FILE=/opt/sqlite/data.db  
sqlite3 -init ${SQL_FILE} ${DB_FILE}
```

#### 退出 sqlite 终端

.help可打印出帮助文档

.quit就可以退出sqlite3，回来shell界面
