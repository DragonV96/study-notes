<h1 style="font-weight:bold;"><center>git学习笔记</center></h1>

# 第一章 常用命令

![1571121419790](assets/1571121419790.png)

## 1.1 配置本地ssh

​		（1）设置git的用户名和邮箱（第一次安装git的情况下）

````shell
$ git config --global user.name "glw"
$ git config --global user.email "guolongwei@comtom.cn"
````

​		（2）生成密钥

````shell
$ ssh-keygen -t rsa -C "guolongwei@comtom.cn"
````

​		（3）连续3个回车，其中第三个回车前需要输入对应的登陆密码，完成后C:\Users\Administrator\\.ssh文件夹下会生成两个文件：`id_rsa`和`id_rsa.pub`

​		（4）将`id_rsa.pub`用记事本打开，复制里面的全部内容粘贴到git服务器上的添加ssh配置中即可。



# 第二章 高级操作

## 2.1 电脑同时配置gitlab、github和gitee

### 2.1.1 生成密钥

​		（1）生成gitee密钥（指定文件名后缀防止覆盖）

````shell
ssh-keygen -t rsa -C "xxx@xx.com" -f ~/.ssh/id_rsa_gitee
````

​		（2）生成github密钥（指定文件名后缀防止覆盖）

````
ssh-keygen -t rsa -C "xxx@xx.com" -f ~/.ssh/id_rsa_github
````

​		（3）生成gitlab密钥（指定文件名后缀防止覆盖）

```
ssh-keygen -t rsa -C "xxx@xx.com" -f ~/.ssh/id_rsa_gitlab
```

​		（4）在`~/.ssh/`目录（.ssh在用户文件夹下）会分别生成`id_rsa_gitee`和`id_rsa_gitee.pub`私钥和公钥（还有github与gitlab对应私钥和公钥）。将`id_rsa_gitee.pub`中的全部内容复制粘帖到公司gitlab服务器的SSH-key的配置中。

### 2.1.2 添加config配置

​		（1）在`~/.ssh/`下添加config配置文件

````
cd ~/.ssh
touch config
````

​		（2）添加如下内容：

````
# gitlab                                                                       
Host gitlab
   Port 22
    User git
    HostName gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_gitlab
# github                                                                           
Host github
    Port 22
    User git
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_github
# gitee                                                                           
Host gitee
    Port 22
    User git
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_gitee
````

​		配置文件字段对应解释：

````
Host 
它涵盖了下面一个段的配置，我们可以通过他来替代将要连接的服务器地址。这里可以使用任意字段或通配符。 
当ssh的时候如果服务器地址能匹配上这里Host指定的值，则Host下面指定的HostName将被作为最终的服务器地址使用，并且将使用该Host字段下面配置的所有自定义配置来覆盖默认的/etc/ssh/ssh_config配置信息。 
Port 
自定义的端口。默认为22，可不配置 
User 
自定义的用户名，默认为git，可不配置 
HostName 
真正连接的服务器地址 
PreferredAuthentications 
指定优先使用哪种方式验证，支持密码和秘钥验证方式 
IdentityFile 
指定本次连接使用的密钥文件
````

### 2.1.3 配置git仓库

​		由于公司使用gitlab进行代码管理，所以将gitlab设置为`global`全局配置（可根据自身情况选择设置）。

​		示例：

​		gitlba本地仓库文件夹为`F:/gitlab`，github本地仓库文件夹为`F:/github`，gitee本地仓库文件夹为`F:/gitee`。

​		则配置如下：

​		（1）先配置gitlab（防止把gitee和github本地仓库覆盖）

````
cd F:/gitlab
git init
git config --global user.name 'xxx'
git config --global user.email 'xxx@xx.com'
````

​		（2）配置github

```
cd F:/github
git init
git config --local user.name 'xxx'
git config --local user.email 'xxx@xx.com'
```

​		（3）配置gitee

```
cd F:/gitee
git init
git config --local user.name 'xxx'
git config --local user.email 'xxx@xx.com'
```

​	