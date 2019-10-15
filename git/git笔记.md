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