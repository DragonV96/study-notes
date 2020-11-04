# Docker命令笔记

  * [1 基本命令](#1-%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4)
    * [1\.1 Docker命令](#11-docker%E5%91%BD%E4%BB%A4)
      * [1\.1\.1 查看容器相关命令](#111-%E6%9F%A5%E7%9C%8B%E5%AE%B9%E5%99%A8%E7%9B%B8%E5%85%B3%E5%91%BD%E4%BB%A4)
      * [1\.1\.2 容器操作相关命令](#112-%E5%AE%B9%E5%99%A8%E6%93%8D%E4%BD%9C%E7%9B%B8%E5%85%B3%E5%91%BD%E4%BB%A4)
      * [1\.1\.3 删除容器及镜像](#113-%E5%88%A0%E9%99%A4%E5%AE%B9%E5%99%A8%E5%8F%8A%E9%95%9C%E5%83%8F)
      * [1\.1\.4 批量操作容器及镜像](#114-%E6%89%B9%E9%87%8F%E6%93%8D%E4%BD%9C%E5%AE%B9%E5%99%A8%E5%8F%8A%E9%95%9C%E5%83%8F)
    * [1\.2 Docker\-compose命令](#12-docker-compose%E5%91%BD%E4%BB%A4)
      * [1\.2\.1 查看容器相关命令](#121-%E6%9F%A5%E7%9C%8B%E5%AE%B9%E5%99%A8%E7%9B%B8%E5%85%B3%E5%91%BD%E4%BB%A4)
      * [1\.1\.2 容器操作相关命令](#112-%E5%AE%B9%E5%99%A8%E6%93%8D%E4%BD%9C%E7%9B%B8%E5%85%B3%E5%91%BD%E4%BB%A4-1)
      * [1\.1\.3 删除容器](#113-%E5%88%A0%E9%99%A4%E5%AE%B9%E5%99%A8)
  * [2 Docker安装镜像及使用](#2-docker%E5%AE%89%E8%A3%85%E9%95%9C%E5%83%8F%E5%8F%8A%E4%BD%BF%E7%94%A8)
    * [2\.1 安装Docker](#21-%E5%AE%89%E8%A3%85docker)
    * [2\.2 安装Docker\-compose](#22-%E5%AE%89%E8%A3%85docker-compose)
    * [2\.3 安装数据库](#23-%E5%AE%89%E8%A3%85%E6%95%B0%E6%8D%AE%E5%BA%93)
      * [2\.3\.1 MySQL](#231-mysql)
      * [2\.3\.2 MongoDB](#232-mongodb)
      * [2\.3\.3 Hbase](#233-hbase)
    * [2\.4 安装中间件](#24-%E5%AE%89%E8%A3%85%E4%B8%AD%E9%97%B4%E4%BB%B6)
      * [2\.4\.1 RabbitMQ](#241-rabbitmq)
      * [2\.4\.2 Kafka](#242-kafka)
      * [2\.4\.3 RocketMQ](#243-rocketmq)
      * [2\.4\.4 activeMQ](#244-activemq)
    * [2\.5 安装自动化工具](#25-%E5%AE%89%E8%A3%85%E8%87%AA%E5%8A%A8%E5%8C%96%E5%B7%A5%E5%85%B7)
      * [2\.5\.1 Git](#251-git)
      * [2\.5\.2 安装Jenkins](#252-%E5%AE%89%E8%A3%85jenkins)
    * [2\.6 安装Redis](#26-%E5%AE%89%E8%A3%85redis)
    * [2\.8 安装erlang](#28-%E5%AE%89%E8%A3%85erlang)
    * [2\.9 安装RabbitMQ](#29-%E5%AE%89%E8%A3%85rabbitmq)
    * [2\.10 安装ActiveMQ](#210-%E5%AE%89%E8%A3%85activemq)
    * [2\.13 安装fastDFS](#213-%E5%AE%89%E8%A3%85fastdfs)
  * [3 Docker镜像操作](#3-docker%E9%95%9C%E5%83%8F%E6%93%8D%E4%BD%9C)
    * [3\.1 打包镜像至私有仓库](#31-%E6%89%93%E5%8C%85%E9%95%9C%E5%83%8F%E8%87%B3%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93)

## 1 基本命令

### 1.1 Docker命令

#### 1.1.1 查看容器相关命令

​	1.查看docker正在运行的容器情况

````
docker ps
````

​	2.查看docker中已安装的镜像情况

```
docker images
```

​	3.查看docker指定容器日志

```
docker logs -f [容器名]
docker logs -ft --tail [n行] [容器名]

实例：
docker logs -f mysql
docker logs -ft --tail 300 mysql
```

​	4.查看容器启动命令

- 在容器外部

````
docker inspect [容器名]

实例：
docker inspect mysql
````

- 在容器内部【前提：需要容器内支持ps命令】（1号进程就是启动命令）

````
ps -fe
````

#### 1.1.2 容器操作相关命令

​	1.在容器里开启一个mysql伪终端交互

```
docker exec -it [容器名] bash

实例：
docker exec -it mysql bash
```

​	2.载入本地docker镜像

```
docker load -i [镜像文件完整路径]

实例：
docker load -i /usr/local/comtom/dockerimages/alpine-java.tar
```

​	3.保存docker镜像到本地

````
docker save -o [镜像文件完整路径及命名] [当前镜像标签名]

实例：
docker save -o /usr/local/comtom/dockerimages/rabbitmq.tar registry.comtom.cn:2443/gd-v5/rabbitmq:3.8.0
````

#### 1.1.3 删除容器及镜像

​	1.查看docker正在运行的容器情况

````
docker ps
````

![1571121219505](assets/1571121219505.png)

​	2.停止运行容器（容器名字或ID均可）

````
docker stop 1a8eb1e42729
docker stop mysql
````

​	3.删除容器（容器名字或ID均可）

````
docker rm 1a8eb1e42729
docker rm mysql

docker rm -f mysql				(强制删除容器)
````

​	4.删除镜像（ID或镜像名字均可，名称后需要加上冒号和版本号）

````
docker rmi 1a8eb1e42729
docker rmi mysql:latest

docker rmi -f mysql:latest		(强制删除镜像)
````

#### 1.1.4 批量操作容器及镜像

​	1.查看所有容器

````shell
docker ps -a
````

​	2.停止所有容器

````shell
docker stop $(docker ps -aq)
````

​	3.删除所有已停止的容器

````shell
docker stop $(docker ps -q -f status=exited)
````

​	4.删除所有容器

````shell
docker rm $(docker ps -aq)
````

​	5.删除所有镜像

````shell
docker rmi $(docker images -aq)
````

### 1.2 Docker-compose命令

#### 1.2.1 查看容器相关命令

​	1.查看docker-compose中正在运行的容器情况

```
docker-compose ps
```

​	2.查看docker-compose中已安装的镜像情况

```
docker-compose images
```

​	3.查看docker-compose日志

```
docker-compose logs -f
```

#### 1.1.2 容器操作相关命令

​	1.在容器里开启一个mysql伪终端交互

```
docker-compose exec [容器名] bash

实例：
docker-compose exec mysql bash
```

#### 1.1.3 删除容器

​	1.查看docker-compose正在运行的容器情况

```
docker-compose ps
```

![1575355859561](assets/1575355859561.png)

​	2.停止运行docker-compose容器

```
docker-compose stop
```

​	3.删除docker-compose容器

```
docker-compose rm
y
```

## 2 Docker安装镜像及使用

​	大前提：当前操作系统上已安装好Docker，可输入下面命令查看docker版本

````
docker -v
````

### 2.1 安装Docker

​	Docker 要求 CentOS 系统的内核版本高于 3.10 。

​	1.查看操作系统当前的内核版本

```
uname -r
```

​	2.更新yum包到最新

```
yum update
```

​	3.卸载旧版本（如果存在旧版本，不存在则跳过）

```
yum remove docker docker-common docker-selinux docker-engine -y
```

​	4.安装docker需要的软件，yum-util提供yum-config-manager功能，其他两个是devicemapper驱动依赖的包

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

​	5.配置yum源

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo			（阿里云）

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo			（官方源）

yum-config-manager --add-repo https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo				（中国科技大学）
```

​	6.查看所有仓库中所有docker版本，并选择特定版本进行安装

```
yum list docker-ce --showduplicates | sort -r
```

​	7.安装docker

```
yum -y install docker-ce			(默认装最新稳定版)
```

​	8.启动docker服务

```
systemctl start docker		(启动)
systemctl stop docker		(关闭,安装过程不要执行)
```

​	9.开机启动docker服务

```
systemctl enable docker		(启动)
systemctl disable docker	(关闭,安装过程不要执行)
```

​	10.查看docker版本（有client和server表示docker安装成功）

```
docker version
```

### 2.2 安装Docker-compose

​	Docker-compose 要求docker的内核版本高于所对应的docker最低版本 。

​	1.查看当前docker的内核版本

```
docker version
```

​	2.进入docker-compose的github官方release页面：[点我跳转](https://github.com/docker/compose/releases)

​	3.下载[docker-compose-Linux-x86_64](https://github.com/docker/compose/releases/download/1.25.0/docker-compose-Linux-x86_64)

​	4.上传到 `/usr/local/bin` 目录下，然后重命名为 `docker-compose`

```
mv docker-compose-Linux-x86_64 docker-compose
```

​	5.赋予 `docker-compose` 文件root权限

```
chmod +x /usr/local/bin/docker-compose
```

​	6.查看 `docker-compose` 版本

```
docker-compose version
```

### 2.3 安装数据库

#### 2.3.1 MySQL

​	1.在docker仓库中搜索mysql的镜像

```
docker search mysql
```

​	2.下载mysql镜像

```
docker pull mysql
```

​	3.运行mysql容器（运行完显示一串sha256字符）

```shell
docker run --name mysql -p 3306:3306 --restart=always -v /usr/local/mysql/conf:/etc/mysql/conf.d -v /usr/local/mysql/data:/var/lib/mysql -v /usr/local/mysql/log:/logs -e MYSQL_ROOT_PASSWORD=root --privileged=true -d registry.comtom.cn:2443/gd-v5/mysql:5.7.26 --lower_case_table_names=1

21cb89213c93d805c5bacf1028a0da7b5c5852761ba81327e6b99bb3ea89930e
```

- **-p 3306:3306**：将容器的 3306 端口映射到主机的 3306 端口。
- **-v /usr/local/mysql/conf:/etc/mysql/conf.d**：将容器的  `/etc/mysql/conf.d` 目录挂载到主机  `/usr/local/mysql/conf` 目录下 。
- **--privileged=true**：使容器内的root拥有真正的root权限。
- **-e MYSQL_ROOT_PASSWORD=root**：初始化 root 用户的密码为root。
- **-e MYSQL_ROOT_PASSWORD=root**：初始化 root 用户的密码为root。
- **--lower_case_table_names=1**：设置数据库表名为小写。

​	4.进入mysql容器

````shell
docker exec -it mysql bash
````

​	5.执行sql脚本

````shell
执行宿主机目录的指定sql脚本
docker exec -i mysql sh -c "exec mysql -uroot -proot" < /usr/local/mysql/xxx.sql
执行容器内目录的指定sql脚本
docker exec -i mysql sh -c "exec mysql -uroot -proot < /usr/local/mysql/xxx.sql"
````

​	6.退出docker中的mysql交互窗口

````shell
exit
````

#### 2.3.2 MongoDB

#### 2.3.3 Hbase

​	1.在docker仓库中搜索hbase的镜像

```
docker search hbase
```

​	2.下载hbase镜像

```
docker pull harisekhon/hbase
```

​	3.运行hbase容器（运行完显示一串sha256字符）

```shell
docker run --name hbase --net=host -h docker-hbase -d harisekhon/hbase

21cb89213c93d805c5bacf1028a0da7b5c5852761ba81327e6b99bb3ea89930e
```

- **--name hbase**：容器名字。
- **--net=host**：主机模式运行，容器和服务器共用一个网卡，即容器直接使用服务器的端口 。
- **-h docker-hbase**：设置容器的host为docker-hbase。
- **-d**：后台运行。

​	4.进入mysql容器

```shell
docker exec -it hbase bash
```

​	5.设置host

```shell
进入并编辑hosts文件
vi /etc/hosts

添加如下字符到最后一行（按a进入编辑模式）
127.0.0.1 docker-hbase

退出并保存
:wq
```

​	6.进入hbase控制台

```shell
hbase shell
```

​	7.退出docker中的mysql交互窗口

```shell
exit
```

### 2.4 安装中间件

#### 2.4.1 RabbitMQ

#### 2.4.2 Kafka

​	由于kafka依赖于zookeeper，所以必须先拉取zookeeper的docker镜像。

​	1.下载zookeeper和kafka的镜像

```
docker pull wurstmeister/zookeeper
docker pull wurstmeister/kafka
```

​	2.构建docker-compose.yml

```yaml
version: '3'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    image: wurstmeister/kafka
    depends_on: [ zookeeper ]
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 192.168.1.158
      KAFKA_CREATE_TOPICS: "merchant-template:1:1"
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
        - /usr/local/kafka/docker.sock:/var/run/docker.sock
```

- **KAFKA_ADVERTISED_HOST_NAME** 配置kafka服务器ip地址
- **KAFKA_CREATE_TOPICS** 配置kafka默认的topics
- **KAFKA_ZOOKEEPER_CONNECT** 配置zookeeper的地址和端口

​	3.在docker-compose.yml文件所在目录进行服务打包

```shell
docker-compose build
```

​	4.启动服务

```shell
docker-compose up -d
```

​	5.进入kafka伪终端

````shell
docker exec -it kafka_kafka_1 bash
````

​	6.修改kafka配置**（否则外网无法访问）**

````shell
cd /opt/kafka_2.12-2.3.0/config/
vi  server.properties

搜索advertised
/advertised

将ip:9092中的ip改为自己的服务器ip地址
advertised.listeners=PLAINTEXT://ip:9092
````

#### 2.4.3 RocketMQ

#### 2.4.4 activeMQ

### 2.5 安装自动化工具

#### 2.5.1 Git

​	1.查看是否安装过git

````
rpm -qa|grep git
````

​	显示有以`git-`+版本号的字符，表示已经装过git

​	2.若已经安装，需要先卸载（两条命令任选其一即可）

````
rpm -e --nodeps git
rpm -e git
````

​	3.

#### 2.5.2 安装Jenkins

​	1.下载Jenkins库

```
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

​	2.导入key

```
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

​	3.安装Jenkins

```
yum install -y jenkins
```

​	4.启动/停止/查看Jenkins服务（执行启动即可）

```
systemctl start jenkins				(启动activemq服务)
systemctl status jenkins				(查看activemq服务状态)
systemctl stop jenkins				(停止activemq服务)
```

​	5.因为Jenkins默认端口是8080，可能会导致端口冲突，修改Jenkins的默认端口即可

```
vim /etc/sysconfig/jenkins

直接输入（搜索关键字）：
/JENKINS_PORT

按a进入编辑模式，将JENKINS_PORT后的端口改为需要的：
JENKINS_PORT=8888

先按ESC退出编辑：
:wq			（保存更改并退出）
```

​	6.配置防火墙开放Jenkins端口

```
firewall-cmd --zone=public --add-port=8888/tcp --permanent		(开放8888端口)
firewall-cmd --reload											(重启防火墙)
```

​	7.打开浏览器访问`http://虚拟机IP地址:8888`(192.168.241.129:8888)

​	8.在服务器上查看当前的初始化密码

```
cat /var/lib/jenkins/secrets/initialAdminPassword
```



### 2.6 安装Redis

### 2.8 安装erlang

​	1.安装前置依赖

````
yum install ncurses-devel
````

​	2.解压

````
tar -zxvf otp_src_20.1.tar.gz
````

​	3.进入解压后的erlang目录，然后执行下面命令（不安装javac）

````
./configure --prefix=/usr/local/erlang20 --without-javac
````
​	4.在当前目录直接执行编译，然后再执行下面的安装命令

````
make				（编译）
make install		（安装）
===============================================
make -j 4	(四核cpu编译可采取这个方法，效率更高)
````

​	5.运行erlang，执行erlang安装路径中bin路径下的erl文件即可

````
./bin/erl
````

​	6.进入系统文件（/etc/profile），添加环境变量，并重新加载此文件

````
vim /etc/profile
按a进入编辑模式，在底部添加这句话：
export PATH=$PATH:/usr/local/erlang20/bin
先按ESC退出编辑：
:wq			（保存更改并退出）
source /etc/profile		（使profile文件修改生效）
````

### 2.9 安装RabbitMQ

​	**请装好erlang后再执行下述步骤**

​	1.安装python：

````
yum install python -y
````

​	2.安装simplejson（-y是后续询问步骤直接yes）

````
yum install xmlto -y
yum install python-simplejson -y
````

​	进行步骤3之前，要求移动到统一自定义文件夹`/usr/local/rabbitmq`下

```
mv rabbitmq_server-3.6.14 /usr/local/rabbitmq
```

​	3.先解压xz，再解压tar

````
xz -d rabbitmq-server-generic-unix-3.6.14.tar.xz
tar xf rabbitmq-server-generic-unix-3.6.14.tar
````

​	4.进入rabbitmq路径下sbin目录启动rabbitMQ server

````
./rabbitmq-server
./rabbitmq-server stop		(关闭rabbitMQ服务)
````

​	5.查看rabbitMQ server服务（默认5672端口）

````
netstat -nap|grep 5672
````

​	6.进入系统文件，添加环境变量，并重新加载此文件

```
vim /etc/profile
按a进入编辑模式，在底部添加行代码：
export PATH=$PATH:/usr/local/rabbitmq/sbin

先按ESC退出编辑：
:wq			（保存更改并退出）
source /etc/profile		（使profile文件修改生效）
```

### 2.10 安装ActiveMQ

​	1.在/usr/local目录下创建activemq文件夹（为了运维规范化，建议建立在/usr/local下）

```
mkdir /usr/local/activemq
cd /usr/local/activemq
```

​	2.将下载好的ActiveMQ通过xftp（或其他ftp工具）传到linux上的`/usr/local/activemq/`文件夹下, 下载地址：http://activemq.apache.org/activemq-5155-release.html

​	3.解压tar包到当前文件夹（activemq）下

```
tar -zxvf apache-activemq-5.15.5-bin.tar.gz
```

​	4.启动/停止/查看ActiveMQ服务，首先需要进入`apache-activemq-5.15.5`目录（当前操作可跳过）

```
cd /usr/local/activemq/apache-activemq-5.15.5	（进入目录）

bin/activemq start				(启动activemq服务)
bin/activemq status				(查看activemq服务状态)
bin/activemq stop				(停止activemq服务)
```

​	5.为了在任意地方通过`service activemq start `就可以启动activemq，配置`JAVA_HOME`地址，然后建立软连接

```
vim /usr/local/activemq/apache-activemq-5.15.5/bin/activemq
按a进入编辑模式，在第一行添加这句话：
export JAVA_HOME="/usr/local/java/jdk1.8.0_211"
先按ESC退出编辑：
:wq			（保存更改并退出）

ln -s /usr/local/activemq/apache-activemq-5.15.5/bin/activemq /etc/init.d	(建立软连接)
```

​	6.配置开机启动，可通过系统指令在任意位置进行启动/停止/查看activemq服务

````
chkconfig activemq on				(设置开机启动)

service activemq start				(启动activemq服务)
service activemq status				(查看activemq服务状态)
service activemq stop				(停止activemq服务)
````

​	7.配置防火墙开放ActiveMQ端口

````
firewall-cmd --zone=public --add-port=8161/tcp --permanent		(开放8161端口)
firewall-cmd --reload											(重启防火墙)
````

​	8.打开浏览器访问`http://虚拟机IP地址:8161`，账号密码均为`admin`

### 2.13 安装fastDFS

​	查看是否安装成功

```
查看storage日志：
docker logs -f -t --tail 666 storage

进入fastdfs容器：
docker exec -it fastdfs /bin/bash

现场写入一个html文件：
echo "Hello FastDFS!">index.html

上传此html到fastdfs：
fdfs_test /etc/fdfs/client.conf upload index.html

浏览器输入example file url访问
```

## 3 Docker镜像操作

### 3.1 打包镜像至私有仓库

​	1.登陆私有仓库，输入账号密码即可

````
docker login [地址]

实例：
docker login registry.comtom.cn:2443/gd-v5
````

​	2.构建镜像（需要提前写好Dockerfile，且需要进入到Dockerfile所在目录下执行）

```
docker build -t [镜像名]:[镜像版本号] .

实例：
docker build -t rabbitmq:management .
```

​	3.标记镜像

````
docker tag [原镜像名] [私有仓库镜像名]

实例：
docker tag rabbitmq:management registry.comtom.cn:2443/gd-v5/rabbitmq:1.0
````

​	4.推送镜像

````
docker push [私有仓库镜像名]

实例：
docker push registry.comtom.cn:2443/gd-v5/rabbitmq:1.0
````

​	5.拉取镜像

````
docker pull [私有仓库镜像名]

实例：
docker pull registry.comtom.cn:2443/gd-v5/rabbitmq:1.0
````

