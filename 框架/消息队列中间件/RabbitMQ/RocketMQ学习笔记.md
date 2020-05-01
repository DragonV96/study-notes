<h1 style="font-weight:bold;"><center>RocketMQ学习笔记</center></h1>

# 1 RocketMQ简介

## 1.1  RocketMQ特性

- 支持集群模型、负载均衡、水平扩展
- 上亿级别的消息堆积能力
- 采用零拷贝原理、顺序写盘、随机读
- 丰富的API使用（顺序消息、异步消息、同步消息）
- 代码优秀，底层通信框架采用Netty NIO框架
- NameServer 代替 Zookeeper
- 强调计群无单点，可扩展，任意一点高可用，水平可扩展
- 消息失败重试机制、消息可查询

## 1.2 概念模型

- Producer：消息生产者，负责生产消息，一般由业务系统负责产生消息。

- Consumer：消息消费者，负责消费消息，一般是后台系统负责异步消费。
  - Push Consumer：需要向Consumer对象注册监听。
  - Pull Consumer：需要主动请求Broker拉取消息。
- Producer Group：生产者集合，一般用于发送一类消息
- Consumer Group：消费者集合，一般用于接受一类消息进行消费
- Broker：MQ消息服务（中转角色，用于消息存储与生产消费转发）

## 1.3 源码包结构

- rocketmq-broker 主要的业务逻辑，消息收发，主从同步，pagecache
- rocketmq-client 客户端接口，比如生产者和消费者
- rocketmq-example 示例，比如生产者和消费者
- rocketmq-common 公用数据结构等
- rocketmq-distribution 编译模块，编译输出等
- rocketmq-filter 进行Brocker过滤不感兴趣的消息传输，减少带宽
- rocketmq-logappender、rocketmq-logging 日志相关
- rocketmq-namesrv Namesrv服务，用于服务协调
- rocketmq-openmessaging 对外提供服务
- rocketmq-remoting 远程调用接口，封装Netty底层通信
- rocketmq-srvutil 提供一些公用的工具方法，比如解析命令行参数
- rocketmq-store 消息存储
- rocketmq-test、rocketmq-example 测试及使用范例
- rocketmq-tools 管理工具，如mqadmin工具