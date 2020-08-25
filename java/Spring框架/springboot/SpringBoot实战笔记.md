# Spring Boot实战笔记

## 1 Spring Boot入门

### 1.1 Spring Boot核心

- 自动配置：Spring Boot 自动提供 Spring 应用程序的JPA、Thymeleaf模板、安全和Spring MVC等相关配置
- 起步依赖：特殊的 Maven 依赖和 Gradle 依赖，利用传递依赖解析，把常用库聚合在一起，组成几个为特定功能而定制的依赖
- 命令行界面（Spring Boot CLI）：无需传统项目构建，只需专注于应用代码就能完成完整的应用程序
- Actuator：提供在运行时检视应用程序内部情况的特性







## 第一章 Spring基础

## 1.1 Spring概述

**1.1.1 Spring的简史**

​	Spring的发展过程：

- 第一阶段：xml配置（Spring 1.x 时代）
- 第二阶段：注解配置（Spring 2.x 时代）
- 第三阶段：Java配置（Spring 3.x 时代~至今）

**1.1.2 Spring概述**

​	Spring框架是一个轻量级的企业级开发的一站式解决方案。

​	解决方案：基于Spring解决Java EE开发的所有问题。

​	Spring框架主要提供IOC容器、AOP、数据访问、Web开发、消息、测试等相关技术的支持。

​	每一个被Spring管理的POJO（Plain Old Java Object，无任何限制的普通对象）都称为Bean，IOC容器则用来初始化对象，解决对象间的依赖管理和对象的使用。

​	**1. Spring的模块**

​	![](.\img\1-1.png)

​	（1）核心容器（Core Container）

- Spring-Core：核心工具类，Spring其他模块大量使用到了Spring-Core；
- Spring-Beans：Spring定义Bean的支持；
- Spring-Context：运行时Spring容器；
- Spring-Context-Support：Spring容器对第三方包的集成支持；
- Spring-Expression：使用表达式语言字运行时查询和操作对象。

​	（2）AOP

- Spring-AOP：基于代理的AOP支持；
- Spring-Aspects：基于AspectJ的AOP支持。

​	（3）消息（Messaging）

- Spring-Messaging：对消息架构和协议的支持。

​	（4）Web

- Spring-Wen：提供基础的Web集成的功能，在Web项目中提供Spring的容器；
- Spring-Webmvc：提供基于Servlet的SpringMVC；
- Spring-WebSocket：提供WebSocket功能；
- Spring-Webmvc-Porltet：提供Portlet环境支持。

​	（5）数据访问/集成（Data Acess/Integration）

- Spring-JDBC：提供以JDBC访问数据库的支持；
- Spring-TX：提供编程式和声明式的事务支持；
- Spring-ORM：提供对对象/关系映射技术的支持；
- Spring-OXM：提供对对象/xml映射技术的支持；
- Spring-JMS：提供对JMS的支持。

​	**2. Spring的生态**

- Spring Boot：使用默认开发配置来实现快速开发。
- Spring XD：用来简化大数据应用开发。
- Spring Cloud：为分布式系统开发提供工具集合。
- Spring Data：对主流的关系型和NoSQL数据库的支持。
- Spring Integration：通过消息机制对企业集成模式（EIP）的支持。
- Spring Batch：简化及优化大量数据的批处理操作。
- Spring Security：通过认证和授权保护应用。
- Spring HATEOAS：基于HATEOAS原则简化REST服务开发。
- Spring Social：与社交网络API（如Facebook、新浪微博等）的集成。
- Spring AMQP：对基于AMQP的消息的支持。
- Spring Mobile：提供对手机设备检测的功能，给不同的设备返回不同的页面的支持。
- Spring for Android：主要提供在Android上消费RESTful API的功能。
- Spring Web Flow：基于Spring MVC提供基于向导流程式的Web应用开发。
- Spring Web Services：提供了基于协议有限的SOAP/Web服务。
- Spring LDAP：简化使用LDAP开发。
- Spring Session：提供一个API及实现来管理用用户会话信息。

## 1.2 Spring项目快速搭建

​	目前主流的项目构建工具有：Maven和Gradle，其中Ant已经很少了。

**1.2.1 Maven**

​	Apache Maven是一个软件项目管理工具。基于项目对象模型（Project Object Model，POM）的概念，Maven可用来管理项目的依赖、编译、文档等信息。

​	Maven管理项目时，所有项目依赖的jar包全部放在.m2文件夹下或者是用户自定义的文件夹下，集中管理，节省空间，管理方便。

**1.2.2 Maven的pom.xml**

​	Maven的pom.xml用来管理项目的依赖以及项目的编译等功能。

​	**1. dependencies元素**

​	<dependencies></dependencies>，此元素包含多个项目依赖需要使用的<dependency></dependency>。

​	**2. dependency元素**

​	<dependency></dependency>内部通过groupId、artifactId确定唯一的依赖，这三个可理解为三个坐标：

- groupId：组织的唯一标识；
- artifactId：项目的唯一标识；
- version：项目的版本。

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.1.5.RELEASE</version>
</dependency>
```

​	**3. 变量定义**

​	变量定义：<properties></properties>可定义变量在dependency中引用。

```xml
<properties>
    <spring.version>4.1.5.RELEASE</spring.version>
</properties>
    
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
</dependency>
```

​	**4. 编译插件**

​	编译插件中可设置Java的编译级别：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.1</version>
            <configuration>
                <source>1.7</source>
                <target>1.7</target>
                <encoding>UTF8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

​	**5. Maven运作方式**

​	Maven会自动根据dependency中的依赖配置，直接下载依赖包到Maven本地仓库中。

​	Maven中心库网址：http://mvnrepository.com

​	打包Maven中心库中没有的jar包到Maven本地仓库命令（Oracle驱动例子）：

```
mvn install:install-file -DgroupId=com.oracle "-DartifactId=ojdbc14" "-Dversion=10.2.0.2.0" "-Dpackaging=jar" "Dfile=D:\ojdbc14.jar"
```

## 1.3 Spring基础配置

​	Spring框架**四大原则**：

​	1）使用POJO进行轻量级和最小侵入式开发；

​	2）通过依赖注入和基于接口编程实现松耦合；

​	3）通过AOP和默认习惯进行声明式编程；

​	4）使用AOP和模板（template）减少模式化代码。

​	**Spring所有功能的设计和实现都是基于此四大原则的。**

**1.3.1 依赖注入**

​	控制反转（Inversion of Control，IOC）和依赖注入（Dependency Injection，DI）在Spring环境下是等同的概念。控制反转是通过依赖注入实现的。

​	依赖注入：**容器负责创建对象和维护对象之间的依赖关系，而不是通过对象本身负责自己的创建和解决自己的依赖。**

​	目的：解耦合。（如：新建的类需要具备某项功能，继承一个父类，子类将与父类耦合；组合另外一个类则使耦合度大大降低。）

​	Spring IOC容器（ApplicationContext）负责创建Bean，并通过容器将功能类Bean注入到你需要的Bean中。

​	Spring实现Bean的创建和注入有三种方式：

- xml注解
- Java配置
- groovy配置

​	这三种方式称为配置元数据（元数据即描述数据的数据，元素据不具备任何可执行能力，只能被外界代码解析后进行操作）。

​	Spring容器解析这些配置元数据进行Bean初始化、配置和管理依赖。

​	声明Bean的注解：

- @Component组件，没有明确的角色使用；
- @Service在业务逻辑层（service层）使用；
- @Repository在数据访问层（dao层）使用；
- @Controller在展现层（MVC→SpringMVC）使用。

​	注入Bean的注解：

- @Autowired：Spring提供的注解；
- @Inject：JSR-330提供的注解；
- @Resource：JSR-250提供的注解。

​	@Autowired、@Inject、@Resource可注解在set方法上或者属性上，注解在属性上可使代码更少、层次更清晰。

**Spring IOC原理拆解图**

![1583046882582](assets/1583046882582.png)

**Spring MVC原理拆解图**

![1583047634233](assets/1583047634233.png)

