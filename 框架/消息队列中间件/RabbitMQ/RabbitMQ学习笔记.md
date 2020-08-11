# RabbitMQ学习笔记

  * [1 RabbitMQ 简介](#1-rabbitmq-%E7%AE%80%E4%BB%8B)
  * [2 消息通信](#2-%E6%B6%88%E6%81%AF%E9%80%9A%E4%BF%A1)
    * [2\.1 核心概念](#21-%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5)
    * [2\.2 交换机](#22-%E4%BA%A4%E6%8D%A2%E6%9C%BA)
    * [2\.3 消息确认机制](#23-%E6%B6%88%E6%81%AF%E7%A1%AE%E8%AE%A4%E6%9C%BA%E5%88%B6)
  * [3 运行和管理](#3-%E8%BF%90%E8%A1%8C%E5%92%8C%E7%AE%A1%E7%90%86)

## 1 RabbitMQ 简介

## 2 消息通信

### 2.1 核心概念

- 消费者（Producer）
- 生产者（Consumer）
- 消息代理（Message Broker）
- 队列（Queue）
- 交换机（Exchange）：用来发送消息的 AMQP 实体。
- 绑定（Binding）
- 路由键（Routing Key）
- 虚拟主机（vhost）

### 2.2 交换机

**1. 分类**

- Direct exchange（直连交换机）
- Fanout exchange（扇型交换机）
- Topic exchange（主题交换机）
- Headers exchange（头交换机）

**2. 额外属性**

除交换机类型外，在声明交换机时还可以附带许多其他的属性，其中最重要的几个分别是：

- Name
- Durability （消息代理重启后，交换机是否还存在）
- Auto-delete （当所有与之绑定的消息队列都完成了对此交换机的使用后，删掉它）
- Arguments（依赖代理本身）

交换机可以有两个状态：持久（durable）、暂存（transient）。

> 持久化的交换机会在消息代理（broker）重启后依旧存在，而暂存的交换机则不会（它们需要在代理再次上线后重新被声明）。

### 2.3 消息确认机制

## 3 运行和管理