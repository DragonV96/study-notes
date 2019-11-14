<h1 style="font-weight:bold;"><center>MYSQL学习笔记</center></h1>

# 第一章 sql

**SQL语句分类：**

- **DDL**（数据库定义语句）： `CREATE`，`ALTER`， `DROP`， `TRUNCATE`， `COMMENT`， `RENAME`

- **DML**（数据操纵语句）：`SELECT`， `INSERT`，`UPDATE`，`DELETE`，`DELETE`，`MERGE`，`EXPLAIN PLAN`，`LOCK TABLE`

- **DCL**（数据控制语句）： `GRANT`，`REVOKE`

- **TCL**（事务控制语句）： `COMMIT`，`ROLLBACK`，`SAVEPOINT`，`SET TRANSACTION`

![1573726522208](assets/1573726522208.png)

## 1.1 DML语法

```sql
选择：select * from table1 where 范围
插入：insert into table1(field1,field2) values(value1,value2)
删除：delete from table1 where 范围
更新：update table1 set field1=value1 where 范围
查找：select * from table1 where field1 like ’%value1%’
排序：select * from table1 order by field1,field2 [desc]
总数：select count as totalcount from table1
求和：select sum(field1) as sumvalue from table1
平均：select avg(field1) as avgvalue from table1
最大：select max(field1) as maxvalue from table1
最小：select min(field1) as minvalue from table1
```

## 1.2 DDL语法

````sql
1、创建数据库
CREATE DATABASE database-name

2、删除数据库
drop database dbname

3、备份sql server
--- 创建 备份数据的 device
USE master
EXEC sp_addumpdevice 'disk', 'testBack', 'c:\mssql7backup\MyNwind_1.dat'
--- 开始 备份
BACKUP DATABASE pubs TO testBack

4、创建新表
create table tabname(col1 type1 [not null] [primary key],col2 type2 [not null],..)

根据已有的表创建新表：
A：create table tab_new like tab_old (使用旧表创建新表)
B：create table tab_new as select col1,col2… from tab_old definition only

5、删除新表
drop table tabname

6、增加一个列
Alter table tabname add column col type
注：列增加后将不能删除。DB2中列加上后数据类型也不能改变，唯一能改变的是增加varchar类型的长度。

7、添加主键： Alter table tabname add primary key(col)
删除主键： Alter table tabname drop primary key(col)

8、创建索引：create [unique] index idxname on tabname(col….)
删除索引：drop index idxname
注：索引是不可更改的，想更改必须删除重新建。

9、创建视图：create view viewname as select statement
删除视图：drop view viewname
````



# 第二章 sql优化

## 2.1 插入优化

**1. 一条SQL语句插入多条数据。**

常用的插入语句如：

````sql
INSERT INTO insert_table (datetime, uid, content, type) VALUES ('0', 'userid_0', 'content_0', 0);INSERT INTO insert_table (datetime, uid, content, type) VALUES ('1', 'userid_1', 'content_1', 1);
````

修改成：

````sql
INSERTINTO insert_table (datetime,uid,content,type)
VALUES('0','userid_0','content_0',0),('1','userid_1','content_1',1);
````

修改后的插入操作能够提高程序的插入效率。这里第二种SQL执行效率高的主要原因是合并后日志量（MySQL的binlog和innodb的事务让日志）

减少并降低日志刷盘的数据量和频率，从而提高效率。通过合并SQL语句，同时也能减少SQL语句解析的次数，减少网络传输的IO。
这里提供一些测试对比数据，分别是进行单条数据的导入与转化成一条SQL语句进行导入，分别测试1百、1千、1万条数据记录。

![1573726930618](assets/1573726930618.png)

## 2.2 删除优化

## 2.3 修改优化

## 2.4 查询优化