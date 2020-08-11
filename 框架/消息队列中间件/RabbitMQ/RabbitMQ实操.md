# RabbitMQ实操

  * [1\. 常用命令](#1-%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
    * [1\.1 vhost操作](#11-vhost%E6%93%8D%E4%BD%9C)
    * [1\.2 用户操作](#12-%E7%94%A8%E6%88%B7%E6%93%8D%E4%BD%9C)
    * [1\.3 给用户赋权限](#13-%E7%BB%99%E7%94%A8%E6%88%B7%E8%B5%8B%E6%9D%83%E9%99%90)
  * [2\. docker下命令](#2-docker%E4%B8%8B%E5%91%BD%E4%BB%A4)
    * [2\.1 docker下vhost操作](#21-docker%E4%B8%8Bvhost%E6%93%8D%E4%BD%9C)
    * [2\.2 docker下用户操作](#22-docker%E4%B8%8B%E7%94%A8%E6%88%B7%E6%93%8D%E4%BD%9C)
    * [2\.3 docker下给用户赋权限](#23-docker%E4%B8%8B%E7%BB%99%E7%94%A8%E6%88%B7%E8%B5%8B%E6%9D%83%E9%99%90)

## 1. 常用命令

### 1.1 vhost操作

````shell
# 添加vhost
rabbitmqctl add_vhost /testhost

# 列出vhost
rabbitmqctl list_vhosts

# 删除vhost
rabbitmqctl delete_vhost /testhost
````

### 1.2 用户操作

````shell
# 添加用户  rabbitmqctl add_user {username} {password}
rabbitmqctl add_user admin admin

# 修改用户密码 rabbitmqctl change_password {username} {newpassword}
rabbitmqctl change_password admin 666

# 验证用户密码
rabbitmqctl authenticate_user admin 666

# 删除用户
rabbitmqctl delete_user admin

# 列出用户
rabbitmqctl list_users

# 给用户设置标签 none management monitoring administrator 多个用,分隔
# rabbitmqctl set_user_tags {username} {tag ...}
rabbitmqctl set_user_tags admin administrator
````

### 1.3 给用户赋权限

````shell
# rabbitmqctl set_permissions [-p host] {user} {conf} {write} {read}
# vhost 授予用户访问权限的vhost名称 默认 /
# user 可以访问指定vhost的用户名
# conf 一个用于匹配用户在那些资源上拥有可配置的正则表达式
# write 一个用于匹配用户在那些资源上拥有可写的正则表达式
# read 一个用于匹配用户在那些资源上拥有可读的正则表达式

# 授予admin用户可访问虚拟主机testhost，并在所有的资源上具备可配置、可写及可读的权限
rabbitmqctl set_permissions -p /testhost admin ".*" ".*" ".*"

# 授予admin用户可访问虚拟主机testhost2，在以queue开头的资源上具备可配置权限、并在所有的资源上可写及可读的权限
rabbitmqctl set_permissions -p /testhost2 admin "^queue.*" ".*" ".*"

# 清除权限
rabbitmqctl clear_permissions -p /testhost admin

# 虚拟主机的权限
rabbitmqctl list_permissions -p /testhost

# 用户权限
rabbitmqctl list_user_permissions admin
````

## 2. docker下命令

### 2.1 docker下vhost操作

```shell
# 添加vhost
docker exec -i rabbitmq sh -c 'rabbitmqctl add_vhost /testhost'

# 列出vhost
docker exec -i rabbitmq sh -c 'rabbitmqctl list_vhosts'

# 删除vhost
docker exec -i rabbitmq sh -c 'rabbitmqctl delete_vhost /testhost'
```

### 2.2 docker下用户操作

```shell
# 添加用户  rabbitmqctl add_user {username} {password}
docker exec -i rabbitmq sh -c 'rabbitmqctl add_user admin admin'

# 修改用户密码 rabbitmqctl change_password {username} {newpassword}
docker exec -i rabbitmq sh -c 'rabbitmqctl change_password admin 666'

# 验证用户密码
docker exec -i rabbitmq sh -c 'rabbitmqctl authenticate_user admin 666'

# 删除用户
docker exec -i rabbitmq sh -c 'rabbitmqctl delete_user admin'

# 列出用户
docker exec -i rabbitmq sh -c 'rabbitmqctl list_users'

# 给用户设置标签 none management monitoring administrator 多个用,分隔
# rabbitmqctl set_user_tags {username} {tag ...}
docker exec -i rabbitmq sh -c 'rabbitmqctl set_user_tags admin administrator'
```

### 2.3 docker下给用户赋权限

```shell
# rabbitmqctl set_permissions [-p host] {user} {conf} {write} {read}
# vhost 授予用户访问权限的vhost名称 默认 /
# user 可以访问指定vhost的用户名
# conf 一个用于匹配用户在那些资源上拥有可配置的正则表达式
# write 一个用于匹配用户在那些资源上拥有可写的正则表达式
# read 一个用于匹配用户在那些资源上拥有可读的正则表达式

# 授予admin用户可访问虚拟主机testhost，并在所有的资源上具备可配置、可写及可读的权限
docker exec -i rabbitmq sh -c 'rabbitmqctl set_permissions -p /testhost admin ".*" ".*" ".*"'

# 授予admin用户可访问虚拟主机testhost2，在以queue开头的资源上具备可配置权限、并在所有的资源上可写及可读的权限
docker exec -i rabbitmq sh -c 'rabbitmqctl set_permissions -p /testhost2 admin "^queue.*" ".*" ".*"'

# 清除权限
docker exec -i rabbitmq sh -c 'rabbitmqctl clear_permissions -p /testhost admin'

# 虚拟主机的权限
docker exec -i rabbitmq sh -c 'rabbitmqctl list_permissions -p /testhost'

# 用户权限
docker exec -i rabbitmq sh -c 'rabbitmqctl list_user_permissions admin'
```

