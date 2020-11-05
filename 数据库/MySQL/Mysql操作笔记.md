# MySQL操作笔记

  * [1 查询](#1-%E6%9F%A5%E8%AF%A2)
    * [1\.1 MySQL 环境变量查询](#11-mysql-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E6%9F%A5%E8%AF%A2)
      * [1\.1\.1 状态](#111-%E7%8A%B6%E6%80%81)
      * [1\.1\.2 索引](#112-%E7%B4%A2%E5%BC%95)
      * [1\.1\.3 事务](#113-%E4%BA%8B%E5%8A%A1)
      * [1\.1\.4 日志](#114-%E6%97%A5%E5%BF%97)
    * [1\.2 查询缓存](#12-%E6%9F%A5%E8%AF%A2%E7%BC%93%E5%AD%98)
  * [2 日志](#2-%E6%97%A5%E5%BF%97)
    * [2\.1 binlog 日志](#21-binlog-%E6%97%A5%E5%BF%97)
      * [2\.1\.1 开启 binlog 日志](#211-%E5%BC%80%E5%90%AF-binlog-%E6%97%A5%E5%BF%97)
      * [2\.1\.2 查看 binlog 信息](#212-%E6%9F%A5%E7%9C%8B-binlog-%E4%BF%A1%E6%81%AF)
      * [2\.1\.3 管理 binlog](#213-%E7%AE%A1%E7%90%86-binlog)
  * [5 备份与恢复](#5-%E5%A4%87%E4%BB%BD%E4%B8%8E%E6%81%A2%E5%A4%8D)
    * [5\.1 数据不丢失恢复配置](#51-%E6%95%B0%E6%8D%AE%E4%B8%8D%E4%B8%A2%E5%A4%B1%E6%81%A2%E5%A4%8D%E9%85%8D%E7%BD%AE)
  * [6 常见问题](#6-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

## 1 查询

### 1.1 MySQL 环境变量查询

#### 1.1.1 状态

- 查询用户连接状态

````sql
-- 查询用户连接状态（结果集的 Command 字段：Sleep 表示空闲，Query 表示查询）
show processlist;
````

#### 1.1.2 索引

#### 1.1.3 事务

````sql
-- 查询长事务（查找持续时间超过60s的事务）
select * from information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started))>60
````

#### 1.1.4 日志

````sql
-- 1表示每次事务的redo log都直接持久化到磁盘，保证MySQL异常重启之后数据不丢失
show variables like 'innodb_flush_log_at_trx_commit';

-- 1表示每次事务的binlog都持久化到磁盘，保证MySQL异常重启之后binlog不丢失
show variables like 'sync_binlog';
````

### 1.2 查询缓存

**1. 不使用查询缓存**

将 `query_cache_type` 设置为 `DEMAND`

**2. 按需使用查询缓存**

````sql
-- 查询语句加上 SQL_CACHE 参数
select SQL_CACHE * from T
````

## 2 日志

### 2.1 binlog 日志

#### 2.1.1 开启 binlog 日志

①在 `/etc/my.cnf` 中的 `[mysqld]` 下增加如下配置

````shell
[mysqld]
# Server Id.数据库服务器 id，用来在主从服务器中标记唯一 mysql 服务器
server_id=1918
# 打开 binlog
log_bin=mysql-bin
# 设置 binlog 志格式，有 ROW、STATEMENT、MIXED 三种格式，根据需要设置
binlog_format=ROW
````

②重启 mysql

````shell
# 重启 mysql 服务
systemctl restart mysqld

# 重启 mysql 的 docker 容器
docker restart mysql
````

#### 2.1.2 查看 binlog 信息

- 查看 binlog 开关

```sql
show variables like 'log_bin';
```

- 查看 binlog 日志格式

```sql
show variables like 'binlog_format';
```

- 查看 binlog 相关的 SQL 语句

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

#### 2.1.3 管理 binlog

- 查看所有 binlog 的日志列表

````sql
show master logs;
````

- 查看最后一个 binlog 日志的编号名称，及最后一个事件结束的位置（pos）

````sql
show master status;
````

- 刷新 binlog，此刻开始产生一个新编号的 binlog 日志文件

````sql
flush logs;
````

- 清空所有的 binlog 日志**（此操作需谨慎）**

````sql
reset master;
````

## 4 数据库规范

### 4.1 设计

### 4.2 操作

### 4.3 索引

1. 尽量使用主键查询。

原因：主键长度越小，普通索引的叶子节点就越小，普通索引占用的空间也就越小。



### 4.4 事务

1. 尽量不使用长事务。

原因：①事务在开启瞬间会生成 read-view 存储在回滚日志中，长事务会一直占用 read-view，导致回滚日志不会及时删除过旧的 read-view，而占用大量存储空间；②长事务占用锁资源，严重时会拖垮整个库。



2. 使用事务自动提交模式 `set autocommit=1` ，需要启动事务时通过显示语句的方式来启动事务。

原因：有些客户端连接框架会默认连接成功后先执行一个 `set autocommit=0` 的命令，这就导致接下来的
查询都在事务中，如果是长连接，就导致了意外的长事务。



3. 通过显示语句 `begin` 的方式来连续启动多个事务时，尽量使用 `commit work and chain` 提交事务并自动启动下一个事务。

原因：节省 `begin` 语句带来的开销，从程序开发的角度明确地知道每个语句是否处于事务中。



## 5 备份与恢复

### 5.1 数据不丢失恢复配置

**1. 双1配置**

①将 `innodb_flush_log_at_trx_commit` 参数设置为 1

表示每次事务的 redo log 都直接持久化到磁盘，保证 MySQL 异常重启之后数据不丢失（redo log 的 crash-safe 能力）。

②将 `innodb_flush_log_at_trx_commit` 参数设置为 1

表示每次事务的 binlog 都持久化到磁盘，保证 MySQL 异常重启之后 binlog 不丢失。



## 6 常见问题

问题1：如果表 T 中没有字段 k，而你执行了这个语句 `select * from T where k=1`, 那肯定是会报“不存在这个列”的错误： `Unknown column ‘k’ in ‘where clause’`。这个错误是在查询语句执行的哪个阶段报出来的呢？

答：分析器进行词法分析阶段。



问题2：在什么场景下，一天一备会比一周一备更有优势呢？或者说，它影响了这个数据库系统的哪个指标？

答：一天一备比一周一备更有优势，好处是“最长恢复时间最短”。它影响了这个数据库系统的 RTO（恢复目标时间）。



问题3：如何避免出现或处理长事务这种情况？

答：