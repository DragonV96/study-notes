# IO 总结

## 1 IO 流

### 1.1 概念

**1. I/O 概念**

I/O 是机器获取和交换信息的主要渠道。

**2. 流**

1）概念

在计算机中，流是一种信息的转换。

> 流是完成 I/O 操作的主要方式。

2）分类

- 输入流（InputStream）：机器或者应用程序接收外界的信息
- 输出流（OutputStream）：从机器或者应用程序向外输出的信息

合称为输入 / 输出流（I/O Streams）。

**3. IO 原理**

机器间或程序间在进行信息交换或者数据交换时，先将对象或数据转换为某种形式的流，再通过流的传输，到达指定机器或程序后，再将流转换为对象数据。

### 1.2  Java 的 IO

**1. IO 操作类**

`java.io` 包下有 4个 IO 操作的基本类（抽象类）：

- 字节流：`InputStream`、`OutputStream`
- 字符流：`Reader`、`Writer`

![image-20210318231736553](assets/image-20210318231736553.png)

**2.字节流**

字节流的子类，以功能划分：

- 文件的读写操作：`FileInputStream`、`FileOutputStream`
- 字节数组的读写操作：`ByteArrayInputStream`、`ByteArrayOutputStream`
- 普通字符串的读写操作：`BufferedInputStream`、`BufferedOutputStream`

![image-20210318231723397](assets/image-20210318231723397.png)

**3. 字符流**

1）分类

字符流的子类，以功能划分：

- 文件的读写操作：`FileReader`、`FileWriter`
- 字符数组的读写操作：`CharArrayReader`、`CharArrayWriter`
- 字节流的读写操作：`InputStreamReader`、`OutputStreamWriter`

![image-20210318232224828](assets/image-20210318232224828.png)

2）需要 IO 字符流的原因

数据读写过程中，字符到字节必须经过转码，此过程非常耗时，如果对编码类型不确定，则容易乱码，所以 IO 流提供了一个直接操作字符的接口，方便代码对字符进行流操作。

## 2 IO 模型









































