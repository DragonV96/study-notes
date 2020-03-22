# 日志

## 1 binlog二进制日志

### 1.1 开启binlog日志

1. 在 /etc/my.cnf 中的[mysqld]下增加如下配置

````
[mysqld]
# Server Id.数据库服务器id，用来在主从服务器中标记唯一mysql服务器
server_id=1918
# 打开binlog
log_bin=mysql-bin
# 设置bin日志格式，有ROW、STATEMENT、MIXED三种格式，根据需要设置
binlog_format=ROW
````

2. 重启mysql服务（或重启mysql的docker容器）

````
systemctl restart mysqld
或
docker restart mysql
````

### 1.2 查看binlog信息

- 查看binlog开关

```sql
show variables like 'log_bin';
```

- 查看binlog日志格式

```sql
show variables like 'binlog_format';
```

- 查看binlog相关的SQL语句

````sql
show binlog events [IN 'log_name'] [FROM pos] [LIMIT [offset,]row_count]

实例：
-- 查看第一个binlog日志
show binlog events;

-- 查看指定的binlog日志
show binlog events in 'binlog.000030';

-- 查看从指定位置开始的指定binlog日志
show binlog events in 'binlog.000030' from 922;

-- 查看从指定位置开始的指定binlog日志，限制查看的条数
show binlog events in 'binlog.000030' from 922 limit 2;

-- 查看从指定位置开始的指定binlog日志，限制查看的条数，且带有偏移
show binlog events in 'binlog.000030' from 922 limit 1,2;
````



### 1.3 管理binlog

- 查看所有binlog的日志列表

````sql
show master logs;
````

- 查看最后一个binlog日志的编号名称，及最后一个事件结束的位置（pos）

````sql
show master status;
````

- 刷新binlog，此刻开始产生一个新编号的binlog日志文件

````sql
flush logs;
````

- 清空所有的binlog日志**（此操作需谨慎）**

````sql
reset master;
````

