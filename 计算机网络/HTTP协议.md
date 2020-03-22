- [1 基础概念](#1-基础概念)
  - [1.1 URI与URL](#11-URI与URL)
  - [1.2 Cookie](#12-Cookie)
  - [1.3 Session](#13-Session)
- [2 HTTP请求](#2-HTTP请求)
  - [2.1 HTTP请求报文](#21-HTTP请求报文)
  - [2.2 HTTP/1.1请求方法](#22-HTTP/1.1请求方法)
- [3 HTTP响应](#3-HTTP响应)
  - [3.1 HTTP响应报文](#31-HTTP响应报文)
  - [3.2 HTTP响应状态码](#32-HTTP响应状态码)

# HTTP协议

## 1 基础概念

http协议是一种无状态协议。

### 1.1 URI与URL

- URL 是统一资源定位符（如网页地址）
- URI 是统一资源标识符（如视频下载地址）

### 1.2 Cookie

### 1.3 Session

## 2 HTTP请求

### 2.1 HTTP请求报文

- 报文首部
  - 请求行：方法、URI、HTTP版本
  - 首部字段
    - 请求首部字段
      	Accept：用户代理能处理的媒体类型及其相对优先级
        	Authorization：用户代理的认证信息
        	Host（必填）：明确指出请求的服务器群下的具体主机名
        	User-Agent：浏览器种类及操作系统信息
    - 通用首部字段
    - 实体首部字段
      - Content-Type：实体主体内对象的媒体类型
  - 其他
- 空行（CR + LF）
- 报文主体数据

### 2.2 HTTP/1.1请求方法

- GET：请求访问已被URI识别的资源（获取资源）
- POST：传输实体主体
- PUT：传输文件
- HEAD：获得报文首部
- DELETE：请求URI删除指定的资源（删除文件）
- OPTIONS：查询针对请求URI指定的资源支持的方法（询问支持的方法）
- TRACE：是让 Web 服务器端将之前的请求通信环回给客户端（追踪路径）
- CONNECT：主要使用 SSL（Secure Sockets Layer，安全套接 层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容 加 密后经网络隧道传输。（要求用隧道协议连接代理）

## 3 HTTP响应

### 3.1 HTTP响应报文

- 报文首部

  - 请求行：HTTP版本、状态码

  - 首部字段

    - 响应首部字段

      ​	Accept-Ranges：告知客户端服务器能否处理范围请求，以指定获取服务器端某个部分的资源

    - 通用首部字段

    - 实体首部字段

      - Content-Type：实体主体内对象的媒体类型

  - 其他

- 空行（CR + LF）

- 报文主体数据

### 3.2 HTTP响应状态码

- 1XX
  - 类别：Infomational（信息性状态码）
  - 原因：接收的请求正在处理
- 2XX
  - 类别：Success（成功状态码）
  - 原因：请求正常处理完毕
- 3XX
  - 类别：Redirection（重定向状态码）
  - 原因：需要进行附加操作以完成请求
- 4XX
  - 类别：Client Error（客户端错误状态码）
  - 原因：服务器无法处理请求
- 5XX
  - 类别：Server Error（服务器错误状态码）
  - 原因：服务器处理请求出错