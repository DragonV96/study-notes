<h1 style="font-weight:bold;"><center>Java源码笔记系统总结</center></h1>

# 第一章 基础

## 1.1 String、Long 源码解析和面试题

### 1.1.1 String

​		**1. 不变性**

​		源码：

````java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}    
````

​		String类不变性的原因：

- String 被 final 修饰，所以String类不能被继承，类方法不能被覆写
- String中保存数据的是一个被final修饰的char型数组value，同时value是private权限的，String也未提供对value赋值的操作方法，所以value被赋值后，内存地址无法修改。

​		**2. 首字母大小写**

​		一般在反射逻辑中通常采用这种方法将类的首字母小写：

````java
public String getClassName(Class clazz) {
	String name = clazz.getClass().getName();
	return name.substring(0, 1).toLowerCase() + name.substring(1);
}
````

​		**3. 相等判断**

- equals（值相等判断）
- equalsIgnoreCase（值相等判断，忽略大小写）

​		equals源码：

````java
public boolean equals(Object anObject) {
    // 判断内存地址是否相同
    if (this == anObject) {
        return true;
    }
    // 待比较的对象是否是 String，如果不是 String，直接返回不相等
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        // 两个字符串的长度是否相等，不等则直接返回不相等
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            // 依次比较每个字符是否相等，若有一个不等，直接返回不相等
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
````

​		**4. 拆分和合并**

​		拆分：

​		String提供了split("str")方法，但是若有空值也会赋值给String数组的元素，如：

````java
	String a = ",a,b,";
	a.split(",");	——结果:["","a","","b"]
````

​		此时利用google开源工具包Guava可以解决：

````java
    String a =",a, ,  b  c ,";
    // Splitter 是 Guava 提供的 API 
    List<String> list = Splitter.on(',')
        .trimResults()// 去掉空格
        .omitEmptyStrings()// 去掉空值
        .splitToList(a);
    log.info("Guava 去掉空格的分割方法：{}",JSON.toJSONString(list));
    // 打印出的结果为：
    ["a","b  c"]
````

​		合并：

​		String提供了静态方法join，但是有两个局限性：

- 不支持依次join多个字符串，比如：`str.join(",", s1).join(",", s2)`最后得到的结果是是s2的值，s1的值被s2覆盖了

- 若join的是一个List对象，则无法自动过滤掉null值

  此时利用google开源工具包Guava可以解决：

````java
    // 依次 join 多个字符串，Joiner 是 Guava 提供的 API
    Joiner joiner = Joiner.on(",").skipNulls();
    String result = joiner.join("hello",null,"china");
    log.info("依次 join 多个字符串:{}",result);

    List<String> list = Lists.newArrayList(new String[]{"hello","china",null});
    log.info("自动删除 list 中空值:{}",joiner.join(list));
    
	// 输出的结果为；
    依次 join 多个字符串:hello,china
	自动删除 list 中空值:hello,china
````



### 1.1.2 Long

​		**1. 缓存**

​		Long类中实现了一种缓存机制，缓存了从-128 ~ 127之间的所有Long值，如果取这个范围内的Long值，就不会初始化，而是直接从缓存种拿，下面是缓存初始化源码（Long的静态私有内部类）：

````java
private static class LongCache {
    private LongCache(){}
    // 缓存，范围从 -128 到 127，+1 是因为有个 0
    static final Long cache[] = new Long[-(-128) + 127 + 1];

    // 容器初始化时，进行加载
    static {
        // 缓存 Long 值，注意这里是 i - 128 ，所以再拿的时候就需要 + 128
        for(int i = 0; i < cache.length; i++)
            cache[i] = new Long(i - 128);
    }
}
````

​		Long从缓存取数值的源码：

````java
    public static Long valueOf(long l) {
            final int offset = 128;
            if (l >= -128 && l <= 127) { // will cache
                return LongCache.cache[(int)l + offset];
            }
            return new Long(l);
        }
````

### 1.1.3 面试题

​		**1）为什么使用 Long 时，大家推荐多使用 valueOf 方法，少使用 parseLong 方法？**

​		答：因为 Long 本身有缓存机制，缓存了 -128 到 127 范围内的 Long，valueOf 方法会从缓 

存中去拿值，如果命中缓存，会减少资源的开销，parseLong 方法就没有这个机制。

​		**2）如何解决 String 乱码的问题？**

​		答：乱码的问题的根源主要是两个：①字符集不支持复杂汉字；②二进制进行转化时字符集不匹配，所以在 String 乱码时我们可以这么做： 

- 所有可以指定字符集的地方强制指定字符集，比如 new String 和 getBytes 这两个地方； 

- 我们应该使用 UTF-8 这种能完整支持复杂汉字的字符集。

​		**3）为什么大家都说 String 是不可变的**

​		答：主要是因为 String 和保存数据的 char 数组，都被 final 关键字所修饰，所以是不可变的。

​		**4）String 一些常用操作问题，如问如何分割、合并、替换、删除、截取等等问题** 

​		答：分割：split()；

​				合并：join()；

​				替换replace()；

​				删除replace()种的替换字符串传""即可；

​				截取：substring()。

​		**5）为什么把String设计为不可变对象**

​		答：不可变的好处：①方便使用字符串常量池，节省系统资源；

​											②避免了引用传递，是线程安全的。

​				不可变的实现原理： String 和保存数据的 char 数组，都被 final 关键字所修饰。

## 1.2 Java常用关键字理解

### 1.2.1 static

​		被static修饰的只有类变量、方法和方法块，修饰后具有静态、全局属性，同时会出现并发读写的线程安全性问题。

​		**1. static修饰变量**

​		若定义了：`public static List<String> list = new ArrayList();`这样的共享变量，如果同时被多个线程访问的话，就有线程安全的问题，有两种解决办法：

- 把线程不安全的ArrayList换成线程安全的CopyOnWriteArrayList
- 每次访问时，手动加锁		

​		**2. static修饰方法**

​		当方法被`public static`修饰时：

​		1）代表该方法和当前类时无关的，任何类都可以直接访问。

​		2）该方法内部只能调用同样被 static 修饰的方法，不能调用普通方法，（常用的 util 类里面的各种方法，用 static 修，好处就是调用特别方便。）

​		3）static 方法内部的变量在执行时是没有线程安全问题的。方法执行时，数据运行在栈里面，栈的数据每个线程都是隔离开的，所以不会有线程安全的问题，所以 util 类的各个 static 方法是可以安全使用的。

​		**3. static修饰方法块**

​		被static修饰的方法块叫做静态块，静态块在类启动之前，且静态块只能调用被static修饰的变量，且此变量必须写在静态块之前。

​		**4. static初始化顺序**

​		被 static 修饰的类变量、方法块和静态方法的初始化时机的测试 demo，如下图：

![1571142137720](assets/1571142137720.png)

​		打印结果：

​		1 父类静态变量初始化

​		2 父类静态块初始化

​		3 子类静态变量初始化

​		4 子类静态块初始化

​		5 main 方法执行

​		6 父类构造器初始化

​		7 子类构造器初始化

​		结论：①父类的静态变量和静态块比子类优先初始化；

​					②静态变量和静态块比类构造器优先初始化；

​					③被 static 修饰的方法，在类初始化的时候并不会初始化，只有当自己被调用时，才会被执行。

### 1.2.2 final



## 1.3 Arrays、Collections、Objects 常用方法源码解析

​		

#  第二章 集合

## 2.1 ArrayList 源码解析和设计思路

## 2.2 LinkedList 源码解析

## 2.3 List 源码会问哪些面试题

## 2.4 HashMap 源码解析

## 2.5 TreeMap 和 LinkedHashMap 核心源码解析

## 2.6 Map 源码会问哪些面试题

## 2.7 HashSet、TreeSet 源码解析

## 2.8 看源码对实际工作的帮助和应用

