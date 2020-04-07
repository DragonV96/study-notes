# Shell脚本笔记

## 1 常用脚本

### 1.1 shell函数

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

