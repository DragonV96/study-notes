# SQL 调优实战

## 1 数据模拟

### 1.1 创建表

创建一个 user 表，并填充随机的大量数据（如1000w，大概需要3-6小时）

````sql
DROP DATABASE IF EXISTS ten_million_test;
-- 创建数据库
create database ten_million_test DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;
USE ten_million_test;

-- 指定长度，创建随机字符串
DELIMITER ;
DROP FUNCTION IF EXISTS ten_million_test.rand_string;
DELIMITER $$
CREATE FUNCTION ten_million_test.rand_string(n INT)
RETURNS VARCHAR(255)
BEGIN
DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
DECLARE return_str VARCHAR(255) DEFAULT '';
DECLARE i INT DEFAULT 0;
WHILE i < n DO
      SET return_str = concat(return_str, substring(chars_str, FLOOR(1 + RAND() * 80), 1));
      SET i = i + 1;
END WHILE;
RETURN return_str;
END $$

-- 创建随机日期
DELIMITER ;
DROP FUNCTION IF EXISTS ten_million_test.rand_date;
DELIMITER $$
CREATE FUNCTION ten_million_test.rand_date()
  RETURNS VARCHAR(255)
  BEGIN
    DECLARE nDate CHAR(10) DEFAULT '';
    SET nDate = CONCAT(2010 + FLOOR((RAND() * 8)), '-', LPAD(FLOOR(1 + (RAND() * 12)), 2, 0), '-',
                       LPAD(FLOOR(3 + (RAND() * 8)), 2, 0));
    RETURN nDate;
  END $$

-- 创建随机日期时间
DELIMITER ;
DROP FUNCTION IF EXISTS ten_million_test.rand_datetime;
DELIMITER $$
CREATE FUNCTION ten_million_test.rand_datetime()
  RETURNS VARCHAR(255)
  BEGIN
    DECLARE nDateTime CHAR(19) DEFAULT '';
    SET nDateTime = CONCAT(CONCAT(2010 + FLOOR((RAND() * 8)), '-', LPAD(FLOOR(1 + (RAND() * 12)), 2, 0), '-',
                                  LPAD(FLOOR(3 + (RAND() * 8)), 2, 0)),
                           ' ',
                           CONCAT(LPAD(FLOOR(0 + (RAND() * 23)), 2, 0), ':', LPAD(FLOOR(0 + (RAND() * 60)), 2, 0), ':',
                                  LPAD(FLOOR(0 + (RAND() * 60)), 2, 0))
    );
    RETURN nDateTime;
  END $$

-- 创建随机性别
DELIMITER ;
DROP FUNCTION IF EXISTS ten_million_test.rand_gender;
DELIMITER $$
CREATE FUNCTION ten_million_test.rand_gender()
RETURNS VARCHAR(255)
BEGIN
DECLARE chars_str VARCHAR(100) DEFAULT '男女它';
RETURN substring(chars_str, FLOOR(1 + RAND() * 3), 1);
END $$

-- 创建用户表
DELIMITER ;
CREATE TABLE IF NOT EXISTS ten_million_test.users(
`id` INT PRIMARY KEY auto_increment comment '用户表主键',
`username` VARCHAR(20) not null comment '用户名',
`email` VARCHAR(100) not null comment '邮箱',
`gender` CHAR(1) not null comment '性别',
`create_at` TIMESTAMP not null comment '创建时间',
`age` TINYINT not null comment '年龄'
) engine=innodb DEFAULT CHARSET=utf8mb4 comment = '用户表';

-- 创建插入用户表的存储过程，可执行插入的条数
DROP PROCEDURE IF EXISTS ten_million_test.insert_large_user;
DELIMITER $$
CREATE PROCEDURE ten_million_test.insert_large_user(num INT)
  BEGIN
    DECLARE sNum INT;
    SET sNum = 1;
    WHILE sNum <= num DO
      INSERT INTO ten_million_test.users(username, email, gender, create_at, age)
      VALUES (ten_million_test.rand_string(10), concat(ten_million_test.rand_string(7), '@qq.com'), ten_million_test.rand_gender(),
              ten_million_test.rand_datetime(), ROUND(RAND() * 100));
      SET sNum = sNum + 1;
    END WHILE;
END $$

-- 执行存储过程插入；插入1000w条：
CALL ten_million_test.insert_large_user(10000000);
````

## 2 调优

### 2.1 条件查询

````sql

````



### 2.2 分页查询

````sql

````

