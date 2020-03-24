# Redis总结

## 1 缓存

缓存指数据交换的缓冲区。

### 1.1 缓存作用

- **高性能**：预加载复杂、耗时且极少修改的查询，用户体验丝滑流畅
- **高并发**：内存中key-value式操作，单机支撑并发量达到几万十几万，是mysql的几十倍（因为缓存走内存，mysql走磁盘I/O）

### 1.2 缓存带来的问题

- 缓存与数据库双写不一致
- 缓存雪崩、缓存穿透、缓存击穿
- 缓存并发竞争

## 2 Redis基础概念

### 2.1 Redis与Memcached区别

| 区别     | Redis                                                        | Memcached              |
| -------- | ------------------------------------------------------------ | ---------------------- |
| 数据结构 | 支持string、hash、list、set、sorted set、bitmap、hyperloglog和geospatial等8种数据结构(5.0引入了stream) | 只支持String和整数     |
| 线程模型 | 单线程处理                                                   | 多线程处理             |
| 性能对比 | 100k以上数据性能较低，100k以下性能较高                       | 与其相反               |
| 部署模式 | 原生支持集群模式                                             | 原生仅支持单机部署模式 |
| 同步方式 | 支持主从同步                                                 | 不支持复制             |

### 2.2 Redis数据结构

**1. 基础数据结构**

- String

最简单的KV存储（效率比Memcached高）

> value也可以存数字，如：数字类型（数字类型可用Long表示时）

````
set name JeasonBourne
````

- hash

类似map的数据结构

> 可存储结构化数据的同时只修改其中的部分属性，如：一个对象（对象中不能嵌套对象）

````
hset person name JeasonBourne
hset person age 25
hset person sex man
hget person age
````

````
person = {
	"name": "JeasonBourne",
	"age": 25,
	"sex": "man"
}
````

- list

有序列表

> 可通过list存储列表型的数据结构，如存储粉丝列表、文章评论列表等

> 可通过lrange命令实现高性能分页，如微博下拉刷新的分页操作

````shell
# 0表示开始位置，-1表示结束位置，结束位置为-1时，表示列表的最后一个位置，即查看所有
lrange pageList 0 -1
````

- set
- sorted set

**2. 高级数据结构**

### 2.3 Redis线程模型

