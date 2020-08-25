# Shell脚本笔记

  * [1 常用脚本](#1-%E5%B8%B8%E7%94%A8%E8%84%9A%E6%9C%AC)
    * [1\.1 shell函数调用](#11-shell%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)
    * [1\.2 http调用脚本](#12-http%E8%B0%83%E7%94%A8%E8%84%9A%E6%9C%AC)
    * [1\.3 处理字符串](#13-%E5%A4%84%E7%90%86%E5%AD%97%E7%AC%A6%E4%B8%B2)
      * [1\.3\.1 获取字符串信息](#131-%E8%8E%B7%E5%8F%96%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%BF%A1%E6%81%AF)
      * [1\.3\.2 截取字符串](#132-%E6%88%AA%E5%8F%96%E5%AD%97%E7%AC%A6%E4%B8%B2)
      * [1\.3\.3 替换字符串](#133-%E6%9B%BF%E6%8D%A2%E5%AD%97%E7%AC%A6%E4%B8%B2)

## 1 常用脚本

### 1.1 shell函数调用

1，调用无入参函数

```shell
print_value()
{
    echo "hello world"
}

print_value

输出结果：
hello world
```



2，调用有参函数

````shell
print_value()
{
    echo "$1"
    sleep 1s
    echo "$2"
    sleep 1s
    return 0
}

print_value hello world

输出结果：
hello
world
````

- $1：表示第一个入参变量
- $2：表示第二个入参变量



3，嵌套调用函数

````shell
get_request()
{
  cat <<EOF
    {
        "key1": "test123",
        "key2": "test123"
    }
EOF
}

get_value()
{
	curl -i -H "Accept: application/json" -H "Content-Type:application/json" -H -X POST "$1" --data "$2" > /usr/local/tmp.txt
	return 0
}

get_value http://127.0.0.1/get $(get_request)
````

- 方法返回值作为入参需要用 `$()` 包裹

### 1.2 http调用脚本

1，POST方式调用http接口，并保存返回结果到文件中

```shell
get_request()
{
  cat <<EOF
    {
        "key1": "test123",
        "key2": "test123"
    }
EOF
}

curl -i -H "Accept: application/json" -H "Content-Type:application/json" -H -X POST "http://127.0.0.1/get" --data "$(get_request)" > /usr/local/tmp.txt
```

- -H：表示header
- -X：表示请求方式
- --data：表示请求json实体
- \>：表示调用结果输出到文件
- cat <<EOF ... EOF：表示输出多行字符



2，POST方式调用http接口保存返回结果到文件中，并从文件中取 `key1` 的值并替换yml文件中的 `private` 字段的值

test.yml文件：

````yaml
server:
	private: 666
	public: 777
````

脚本：

````shell
get_request()
{
  cat <<EOF
    {
        "key1": "test123",
        "key2": "test123"
    }
EOF
}

get_value_yml()
{
	curl -i -H "Accept: application/json" -H "Content-Type:application/json" -X POST "$1" --data "$(get_request)" > /usr/local/comtom/tmp.txt
    local VALUE=$(cat /usr/local/tmp.txt | sed 's/{/\n/g' | sed 's/}/\n/g' | sed 's/,/\n/g' | grep "$2" | sed 's/:/\n/g' | sed '1d' | sed 's/"//g')
    echo "========================= Insert 【$VALUE】 behind the line include 【$1】 that value is 【$VALUE】 in the 【yml】 file. ========================="
    sed -i -e "s|$3: .*|$3: $VALUE|g" /usr/local/test.yml
    sleep 2s
    return 0
}

get_value_yml http://127.0.0.1:8080/get key1 private
````

### 1.3 处理字符串

#### 1.3.1 获取字符串信息

实例：

```shell
#!/bin/bash
path='/usr/local/tomcat/logs/catalina.out'

# 获取字符串长度
echo ${#path}
```

#### 1.3.2 截取字符串

实例：

````shell
#!/bin/bash
path='/usr/local/tomcat/logs/catalina.out'

# 从右向左截取 第一个 / 后的字符串
echo ${path%/*}

# 从右向左截取 最后一个 / 后的字符串
echo ${path%%/*}

# 从左向右截取第一个 / 后的字符串
echo ${path#*/}

# 从左向右截取最后一个 / 后的字符串
echo ${path##*/}

# 从左边第6个字符串开始，往右截取至最后
echo ${path:5}

# 从左边第6个字符串开始，往右截取10个字符
echo ${path:5:10}

# 从右边第5个字符串开始，往右截取至最后
echo ${path:0-5}

# 从右边第5个字符串开始，往右截取3个字符
echo ${path:0-5:3}

# 从左边开始截取，0 表示第一个字符串
# 从右边开始截取，0-1 表示第一个字符串
````

#### 1.3.3 替换字符串

实例：

```shell
#!/bin/bash
path='usr/local/tomcat/logs/catalina.out'

# 把path里的第一个 lo 字符串，替换为 22 字符串
echo ${path/lo/22}

# 把path里的所有的 lo 字符串，替换为 22 字符串
echo ${path//lo/22}

# 匹配以 usr 开头的字符串，替换为 22 字符串
echo ${path/#usr/22}

# 匹配以 out 结尾的字符串，替换为 22 字符串
echo ${path/%out/22}
```

