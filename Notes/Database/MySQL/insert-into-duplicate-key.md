
假设上面语句中字段a的属性为唯一unique。
```sql
INSERT INTO TableName(a,b,c) 
VALUES(1,2,3)
ON DUPLICATE KEY
UPDATE c=c+1;
```


当在 insert 语句末尾指定了 on duplicate key update 语句时，如果新插入的新数据中 a列 的值已经在数据库中存在，则会执行后面的 update 语句，这时相当于执行了下面语句：
```sql
UPDATE TableName SET c=c+1 WHERE a=1;
```
反之，则执行正常的 insert 语句。

这里需要注意的是：如果行作为新记录被插入，则受影响行的值为1；如果原有的记录被更新，则受影响行的值为2。

参考资料：[http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html](http://www.php1.cn/)

下面来看一下同时插入多行记录的情况：

字段a被定义为UNIQUE，并且原数据库表table中已存在记录(2,2,9)和(3,2,1)，如果插入记录的a值与原有记录重复，则更新原有记录，否则插入新行：
```sql
INSERT INTO TABLE (a,b,c) VALUES 
(1,2,3),
(2,5,7),
(3,3,6),
(4,8,2)
ON DUPLICATE KEY UPDATE b=VALUES(b);
```

以上SQL语句的执行，发现(2,5,7)中的a与原有记录(2,2,9)发生唯一值冲突，则执行ON DUPLICATE KEY UPDATE，将原有记录(2,2,9)更新成(2,5,9)，将(3,2,1)更新成(3,3,1)，插入新记录(1,2,3)和(4,8,2)  。
  
注意：ON DUPLICATE KEY UPDATE只是MySQL的特有语法，并不是SQL标准语法！