# java后端技能树笔记集合
【java开发技能树指北】涵盖java、JVM、Spring、常用框架、中间件、数据库、数据结构与算法、设计模式、git、maven、linux运维等学习笔记集合

> 打铁还需自身硬！

  * [1 java](#1-java)
    * [基础](#%E5%9F%BA%E7%A1%80)
      * [基础语法](#%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95)
      * [集合](#%E9%9B%86%E5%90%88)
      * [并发](#%E5%B9%B6%E5%8F%91)
      * [多线程](#%E5%A4%9A%E7%BA%BF%E7%A8%8B)
      * [I/O](#io)
    * [JVM](#jvm)
    * [项目](#%E9%A1%B9%E7%9B%AE)
  * [2 框架](#2-%E6%A1%86%E6%9E%B6)
    * [Spring 常用框架](#spring-%E5%B8%B8%E7%94%A8%E6%A1%86%E6%9E%B6)
      * [Spring](#spring)
      * [Spring Boot](#spring-boot)
      * [SpringMVC](#springmvc)
    * [ORM 框架](#orm-%E6%A1%86%E6%9E%B6)
    * [并发框架](#%E5%B9%B6%E5%8F%91%E6%A1%86%E6%9E%B6)
    * [搜索框架](#%E6%90%9C%E7%B4%A2%E6%A1%86%E6%9E%B6)
    * [授权/认证框架](#%E6%8E%88%E6%9D%83%E8%AE%A4%E8%AF%81%E6%A1%86%E6%9E%B6)
    * [文件系统框架](#%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E6%A1%86%E6%9E%B6)
  * [3 数据库](#3-%E6%95%B0%E6%8D%AE%E5%BA%93)
    * [MySQL](#mysql)
  * [4 中间件](#4-%E4%B8%AD%E9%97%B4%E4%BB%B6)
    * [消息队列](#%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97)
      * [RocketMQ](#rocketmq)
      * [Kafka](#kafka)
      * [RabbitMQ](#rabbitmq)
      * [ActiveMQ](#activemq)
    * [缓存中间件](#%E7%BC%93%E5%AD%98%E4%B8%AD%E9%97%B4%E4%BB%B6)
      * [Redis](#redis)
    * [搜索中间件](#%E6%90%9C%E7%B4%A2%E4%B8%AD%E9%97%B4%E4%BB%B6)
      * [Elasticsearch](#elasticsearch)
      * [Solr](#solr)
  * [5 分布式](#5-%E5%88%86%E5%B8%83%E5%BC%8F)
    * [微服务](#%E5%BE%AE%E6%9C%8D%E5%8A%A1)
    * [分布式事务](#%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A1)
    * [分布式锁](#%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81)
    * [分库分表](#%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8)
  * [6 数据结构与算法](#6-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95)
    * [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
    * [算法](#%E7%AE%97%E6%B3%95)
  * [7 计算机网络](#7-%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C)
  * [8 设计模式](#8-%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)
  * [9 Linux 运维](#9-linux-%E8%BF%90%E7%BB%B4)
    * [Linux](#linux)
    * [Docker](#docker)
    * [Kubernetes](#kubernetes)
  * [10 工具](#10-%E5%B7%A5%E5%85%B7)
    * [git](#git)
    * [Maven](#maven)
  * [11 表达式](#11-%E8%A1%A8%E8%BE%BE%E5%BC%8F)
    * [Cron 表达式](#cron-%E8%A1%A8%E8%BE%BE%E5%BC%8F)
    * [正则表达式](#%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)



## 1 java

### 基础

#### 基础语法

- [java 基础总结](https://github.com/DragonV96/study-notes/blob/master/java/基础/基础语法/java基础总结.md)

#### 集合

- [集合源码总结](https://github.com/DragonV96/study-notes/blob/master/java/基础/集合/集合源码总结.md)

#### 并发

- [java 并发编程学习笔记](https://github.com/DragonV96/study-notes/blob/master/java/基础/并发/java并发编程学习笔记.md)
- [并发集合和队列总结](https://github.com/DragonV96/study-notes/blob/master/java/基础/并发/并发集合和队列总结.md)

#### 多线程

- [多线程源码总结](https://github.com/DragonV96/study-notes/blob/master/java/基础/多线程/多线程源码总结.md)

#### I/O

### JVM

- [深入理解 JVM 学习笔记](https://github.com/DragonV96/study-notes/blob/master/java/JVM/深入理解JVM学习笔记.md)
- [生产环境性能调优笔记](https://github.com/DragonV96/study-notes/blob/master/java/JVM/生产环境性能调优笔记.md)
- [生产环境性能调优过程](https://github.com/DragonV96/study-notes/blob/master/java/JVM/生产环境性能调优过程.md)

### 项目

- [常用项目操作](https://github.com/DragonV96/study-notes/blob/master/java/项目/常用项目操作.md)

## 2 框架

### Spring 常用框架

#### Spring

- [Spring 实战笔记](https://github.com/DragonV96/study-notes/blob/master/java/Spring/Spring框架/Spring实战笔记.md)

#### Spring Boot

- [Spring 实战笔记](https://github.com/DragonV96/study-notes/blob/master/java/Spring框架/SpringBoot/Spring Boot实战笔记.md)

#### SpringMVC

- SpringMVC 实战笔记

### ORM 框架

- Mybatis
- Hibernate
- Spring Data JPA

### 并发框架

- [netty 源码分析笔记](https://github.com/DragonV96/study-notes/blob/master/框架/并发框架/Netty/netty源码分析笔记.md)
- tio
- mina
- Disruptor

### 搜索框架

- ElasticSearch
- Solr

### 授权/认证框架

- Spring Security
- Shiro
- OAuth2

### 文件系统框架

- FastDFS
- HDFS

## 3 数据库

### MySQL

- [MySQL 操作笔记](https://github.com/DragonV96/study-notes/blob/master/数据库/MySQL/MySQL操作笔记.md)
- [MySQL 实战笔记](https://github.com/DragonV96/study-notes/blob/master/数据库/MySQL/MySQL实战笔记.md)
- [MySQL 架构设计学习笔记](https://github.com/DragonV96/study-notes/blob/master/数据库/MySQL/MySQL架构设计学习笔记.md)
- [MySQL 学习笔记](https://github.com/DragonV96/study-notes/blob/master/数据库/MySQL/MySQL学习笔记.md)

## 4 中间件

### 消息队列

- [消息队列面试总结](https://github.com/DragonV96/study-notes/blob/master/面试专题/消息队列中间件/消息队列面试总结.md)

#### RocketMQ

- [RocketMQ 学习笔记](https://github.com/DragonV96/study-notes/blob/master/框架/消息队列中间件/RocketMQ/RocketMQ学习笔记.md)

#### Kafka

#### RabbitMQ

- [RabbitMQ 实操](https://github.com/DragonV96/study-notes/blob/master/框架/消息队列中间件/RabbitMQ/RabbitMQ实操.md)
- [RabbitMQ 学习笔记](https://github.com/DragonV96/study-notes/blob/master/框架/消息队列中间件/RabbitMQ/RabbitMQ学习笔记.md)

#### ActiveMQ

### 缓存中间件

- [Redis 面试总结](https://github.com/DragonV96/study-notes/blob/master/面试专题/缓存中间件/Redis面试总结.md)

#### Redis

- Redis实战笔记

### 搜索中间件

#### Elasticsearch

#### Solr

## 5 分布式

### 微服务

- [SpringCloud 学习笔记](https://github.com/DragonV96/study-notes/blob/master/java/springcloud/SpringCloud学习笔记.md)
- Dubbo

### 分布式事务

- [分布式事务总结](https://github.com/DragonV96/study-notes/blob/master/分布式/分布式事务/分布式事务总结.md)

### 分布式锁

- [分布式锁总结](https://github.com/DragonV96/study-notes/blob/master/分布式/分布式锁/分布式锁总结.md)

### 分库分表

- [分库分表总结总结](https://github.com/DragonV96/study-notes/blob/master/分布式/分库分表/分库分表总结总结.md)

## 6 数据结构与算法

### 数据结构

- 数据结构笔记

### 算法

- [经典八大排序算法](https://github.com/DragonV96/study-notes/blob/master/数据结构与算法/算法/经典八大排序算法.md)

## 7 计算机网络

- [TCP/IP 协议](https://github.com/DragonV96/study-notes/blob/master/计算机网络/TCPIP协议/TCPIP协议.md)
- UDP协议
- [HTTP 协议](https://github.com/DragonV96/study-notes/blob/master/计算机网络/HTTP协议/HTTP协议.md)
- Websocket协议
- RPC

## 8 设计模式

- [设计模式学习笔记](https://github.com/DragonV96/study-notes/blob/master/设计模式/设计模式学习笔记.md)

## 9 Linux 运维
### Linux

- [Linux 学习笔记(Cent OS7)](https://github.com/DragonV96/study-notes/blob/master/Linux运维/Linux/Linux学习笔记.md)
- [Shell 脚本笔记](https://github.com/DragonV96/study-notes/blob/master/Linux运维/Linux/Shell脚本笔记.md)

### Docker

- [Docker 命令笔记](https://github.com/DragonV96/study-notes/blob/master/Linux运维/Docker命令笔记.md)
- [Docker-compose.yml 文件参数详解](https://github.com/DragonV96/study-notes/blob/master/Linux运维/Docker/Docker-compose.yml文件参数详解.md)

### Kubernetes

## 10 工具

### git

- [git 笔记](https://github.com/DragonV96/study-notes/blob/master/工具/git/git笔记.md)

### Maven

- [Maven 学习笔记](https://github.com/DragonV96/study-notes/blob/master/工具/Maven/Maven学习笔记.md)

## 11 表达式

### Cron 表达式

- [Cron 表达式学习笔记](https://github.com/DragonV96/study-notes/blob/master/表达式/Crontab/Cron表达式学习笔记.md)

### 正则表达式

- [正则表达式大全（未验证慎用）](https://github.com/DragonV96/study-notes/blob/master/表达式/正则/正则表达式大全.md)
- [正则表达式学习笔记](https://github.com/DragonV96/study-notes/blob/master/表达式/正则/正则表达式学习笔记.md)

> 部分总结笔记来自于知名开源项目 [advanced-java](https://github.com/doocs/advanced-java)。