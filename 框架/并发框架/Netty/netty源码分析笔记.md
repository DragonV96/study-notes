# netty源码分析笔记

## 1 Netty基本组件

### Netty基本组件概念

​		**NioEventLoop**：开启两种类型的线程，一个是监听客户端的连接，另一个是处理客户端的读写。

​		**Channel**：对每一条连接的封装，可以调用其API对数据进行读写。

​		**Pipeline**：对数据读写的一条逻辑处理链。

​		**ChannelHandler**：对Channel中的数据进行逻辑处理的处理类。

​		**ByteBuf**：对数据读写的操作类#。

## 2 Netty的启动过程

### 2.1 创建服务端Channel

**创建服务端Channel流程：**

​		bind()							  								（用户代码入口）

​	→initAndRegister()*（进入函数内部）*		 （初始化并注册）

​	→newChannel()*（进入函数内部）*				（创建服务端channel）

**反射创建服务端Channel：**

​		newSocket()										  （通过jdk来创建底层jdk channel）

​	→NioServerSocketChannelConfig()	（tcp参数配置类）

​	→AbstractNioChannel()

​	→configureBlocking(false)*（进入函数内部）*	 （关闭阻塞模式）

​	→AbstractChannel()*（进入函数内部）*				（创建id，unsafe，pipeline）

### 2.2 初始化服务端Channel

### 2.3 注册selector

### 2.4 端口绑定

