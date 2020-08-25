# MySQL操作笔记

  * [1 查询](#1-%E6%9F%A5%E8%AF%A2)
    * [1\.1 MySQL环境变量查询](#11-mysql%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E6%9F%A5%E8%AF%A2)
    * [1\.2 事务相关查询](#12-%E4%BA%8B%E5%8A%A1%E7%9B%B8%E5%85%B3%E6%9F%A5%E8%AF%A2)
    * [1\.3 用户状态查询](#13-%E7%94%A8%E6%88%B7%E7%8A%B6%E6%80%81%E6%9F%A5%E8%AF%A2)
    * [1\.4 查询缓存](#14-%E6%9F%A5%E8%AF%A2%E7%BC%93%E5%AD%98)
  * [2 日志](#2-%E6%97%A5%E5%BF%97)
    * [2\.1 binlog二进制日志](#21-binlog%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%97%A5%E5%BF%97)
      * [2\.1\.1 开启binlog日志](#211-%E5%BC%80%E5%90%AFbinlog%E6%97%A5%E5%BF%97)
      * [2\.1\.2 查看binlog信息](#212-%E6%9F%A5%E7%9C%8Bbinlog%E4%BF%A1%E6%81%AF)
      * [1\.1\.3 管理binlog](#113-%E7%AE%A1%E7%90%86binlog)

## 1 查询

### 1.1 MySQL环境变量查询

````sql
-- 1表示每次事务的redo log都直接持久化到磁盘，保证MySQL异常重启之后数据不丢失
show variables like 'innodb_flush_log_at_trx_commit';

-- 1表示每次事务的binlog都持久化到磁盘，保证MySQL异常重启之后binlog不丢失
show variables like 'sync_binlog';
````

### 1.2 事务相关查询

````sql
-- 查询长事务（查找持续时间超过60s的事务）
select * from information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started))>60
````

### 1.3 用户状态查询

````sql
-- 查询用户连接状态（结果集的 Command 字段：Sleep 表示空闲，Query 表示查询）
show processlist;
````

### 1.4 查询缓存

**1. 不使用查询缓存**

将 query_cache_type 设置为 DEMAND

**2. 按需使用查询缓存**

````sql
-- 查询语句加上 SQL_CACHE 参数
select SQL_CACHE * from T
````

## 2 日志

### 2.1 binlog二进制日志

#### 2.1.1 开启binlog日志

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

#### 2.1.2 查看binlog信息

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

#### 1.1.3 管理binlog

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

