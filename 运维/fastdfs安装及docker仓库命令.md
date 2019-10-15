* 创建tracker容器：

```shell
docker run --name tracker -tid --restart=always -v /usr/local/comtom/fastdfs/tracker/data:/fastdfs/tracker/data --net=host season/fastdfs tracker
```

* 将trakcer容器中的数据复制到映射目录中

```shell
docker cp tracker:/fdfs_conf/ /usr/local/comtom/fastdfs/conf
```

* 修改storage.conf配置文件：

  storage的存储路径可以修改也可以不改直接映射就行

```shell
tracker_server=本机IP：22122
```

* 创建storage容器：

```shell
docker run --name storage -tid --restart=always -v /usr/local/comtom/fastdfs/storage/data:/fastdfs/storage/data -v /usr/local/comtom/fastdfs/storage/media:/fastdfs/store_path -v /usr/local/comtom/fastdfs/conf/fdfs_conf:/fdfs_conf --net=host -e TRACKER_SERVER:192.168.241.130:22122 -e GROUP_NAME=group1 season/fastdfs storage
```



查看fastdfs是否安装成功

````
docker logs -f -t --tail 666 storage

docker exec -it fastdfs /bin/bash

echo "Hello FastDFS!">index.html

fdfs_test /etc/fdfs/client.conf upload index.html
浏览器输入example file url访问
````

 



````
docker删除所有镜像：
docker rmi -f $(docker images -q)

docker删除所有容器：
docker rm -f $(docker ps -a -q)
````



打成tar包

````shell
# 环境
docker save -o /usr/local/comtom/dockerimages/fastdfs-nginx.tar registry.comtom.cn:2443/gd-v5/fastdfs-nginx:v5.11
docker save -o /usr/local/comtom/dockerimages/mongodb.tar registry.comtom.cn:2443/gd-v5/mongodb:4.2.0
docker save -o /usr/local/comtom/dockerimages/mysql.tar registry.comtom.cn:2443/gd-v5/mysql:5.7.26
docker save -o /usr/local/comtom/dockerimages/rabbitmq.tar registry.comtom.cn:2443/gd-v5/rabbitmq:3.8.0
docker save -o /usr/local/comtom/dockerimages/redis.tar registry.comtom.cn:2443/gd-v5/redis:5.0.6

# 大喇叭
docker save -o /usr/local/comtom/dockerimages/sys-server.tar registry.comtom.cn:2443/gd-v5/sys-server:5.0.1

# 星广播
docker save -o /usr/local/comtom/dockerimages/svr-api.tar registry.comtom.cn:2443/gd-v5/svr-api:5.0.1

# 通播星
docker save -o /usr/local/comtom/dockerimages/gbstar_gbcsa.tar registry.comtom.cn:2443/gd-v5/gbstar_gbcsa:0.1
docker save -o /usr/local/comtom/dockerimages/gbstar_gbcsc.tar registry.comtom.cn:2443/gd-v5/gbstar_gbcsc:0.1
docker save -o /usr/local/comtom/dockerimages/gbstar_tbss.tar registry.comtom.cn:2443/gd-v5/gbstar_tbss:0.1
docker save -o /usr/local/comtom/dockerimages/gbstar_tbas-service.tar registry.comtom.cn:2443/gd-v5/gbstar_tbas-service:0.1
docker save -o /usr/local/comtom/dockerimages/gbstar_tbsc-service.tar registry.comtom.cn:2443/gd-v5/gbstar_tbsc-service:0.1
docker save -o /usr/local/comtom/dockerimages/gbstar_tbapi-service.tar registry.comtom.cn:2443/gd-v5/gbstar_tbapi-service:0.1
````



修改IP

````
vi /etc/sysconfig/network-scripts/ifcfg-ens33
vim /etc/sysconfig/network-scripts/ifcfg-ens33
service network restart

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=fa736e1e-65c2-487e-ad8e-31fbf30dd36a
DEVICE=ens33
ONBOOT=yes


IPADDR=192.168.111.233
NETMASK=255.255.255.0
GATEWAY=192.168.111.254

PREFIX=24
DNS1=192.168.102.2
DNS2=114.114.114.114
ZONE=public
````





载入本地docker镜像（环境）

````
docker load -i /usr/local/comtom/dockerimages/rabbitmq.tar
docker load -i /usr/local/comtom/dockerimages/fastdfs.tar
docker load -i /usr/local/comtom/dockerimages/mongodb.tar
docker load -i /usr/local/comtom/dockerimages/redis.tar
docker load -i /usr/local/comtom/dockerimages/mysql.tar

docker load -i /usr/local/comtom/dockerimages/sys-server.tar
docker load -i /usr/local/comtom/dockerimages/svr-api.tar

docker load -i /usr/local/comtom/dockerimages/gbstar_gbcsa.tar
docker load -i /usr/local/comtom/dockerimages/gbstar_gbcsc.tar
docker load -i /usr/local/comtom/dockerimages/gbstar_tbss.tar
docker load -i /usr/local/comtom/dockerimages/gbstar_tbas-service.tar
docker load -i /usr/local/comtom/dockerimages/gbstar_tbsc-service.tar
docker load -i /usr/local/comtom/dockerimages/gbstar_tbapi-service.tar
````



````
chmod +x deploy-network.sh
./deploy-network.sh
````





构建docker镜像（环境）

````shell
docker build -t season/fastdfs:5.11 .
docker build -t mongodb:4.2.0 .
docker build -t mysql:5.7.26 .
# 大喇叭
docker build -t sys-server:5.0.1 .
# 星广播
docker build -t svr-api:5.0.1 .
````

打版本号（环境）

````shell
docker tag season/fastdfs:5.11 registry.comtom.cn:2443/gd-v5/fastdfs-nginx:v5.11
docker tag mongodb:4.2.0 registry.comtom.cn:2443/gd-v5/mongodb:4.2.0
docker tag mysql:5.7.26 registry.comtom.cn:2443/gd-v5/mysql:5.7.26
docker tag rabbitmq:management registry.comtom.cn:2443/gd-v5/rabbitmq:3.8.0
docker tag redis:5.0.6 registry.comtom.cn:2443/gd-v5/redis:5.0.6
# 大喇叭
docker tag sys-server:5.0.1 registry.comtom.cn:2443/gd-v5/sys-server:5.0.1
# 星广播
docker tag svr-api:5.0.1 registry.comtom.cn:2443/gd-v5/svr-api:5.0.1
# 通播星
docker tag gbstar_gbcsa:0.1 registry.comtom.cn:2443/gd-v5/gbstar_gbcsa:0.1
docker tag gbstar_gbcsc:0.1 registry.comtom.cn:2443/gd-v5/gbstar_gbcsc:0.1
docker tag gbstar_tbss:0.1 registry.comtom.cn:2443/gd-v5/gbstar_tbss:0.1
docker tag gbstar_tbas-service:0.1 registry.comtom.cn:2443/gd-v5/gbstar_tbas-service:0.1
docker tag gbstar_tbsc-service:0.1 registry.comtom.cn:2443/gd-v5/gbstar_tbsc-service:0.1
docker tag gbstar_tbapi-service:0.1 registry.comtom.cn:2443/gd-v5/gbstar_tbapi-service:0.1
````

推送镜像到docker仓库（环境）

````shell
docker push registry.comtom.cn:2443/gd-v5/fastdfs-nginx:v5.11
docker push registry.comtom.cn:2443/gd-v5/mongodb:4.2.0
docker push registry.comtom.cn:2443/gd-v5/mysql:5.7.26
docker push registry.comtom.cn:2443/gd-v5/rabbitmq:3.8.0
docker push registry.comtom.cn:2443/gd-v5/redis:5.0.6
# 大喇叭
docker push registry.comtom.cn:2443/gd-v5/sys-server:5.0.1
# 星广播
docker push registry.comtom.cn:2443/gd-v5/svr-api:5.0.1
# 通播星
docker push registry.comtom.cn:2443/gd-v5/gbstar_gbcsa:0.1
docker push registry.comtom.cn:2443/gd-v5/gbstar_gbcsc:0.1
docker push registry.comtom.cn:2443/gd-v5/gbstar_tbss:0.1
docker push registry.comtom.cn:2443/gd-v5/gbstar_tbas-service:0.1
docker push registry.comtom.cn:2443/gd-v5/gbstar_tbsc-service:0.1
docker push registry.comtom.cn:2443/gd-v5/gbstar_tbapi-service:0.1
````





