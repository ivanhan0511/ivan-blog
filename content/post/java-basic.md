---
title: "Java Basic"
date: 2022-09-14T17:05:15+08:00
categories:
- Java
- Basic
tags:
- Java
keywords:
- java
- basic
clearReading: true
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

Java basic knowledge.
<!--more-->

{{< toc >}}

## THreadLocal

## JVM


## 类型对比
### String[]

### ArrayList


### int or Integer

用int还是用Integer?  
昨天例行code review时大家有讨论到int和Integer的比较和使用。 这里做个整理，发表一下个人的看法。

【int和Integer的区别】

int是java提供的8种原始类型之一，java为每个原始类型提供了封装类，Integer是int的封装类。int默认值是0，而Integer默认值是null；

int和Integer（无论是否new）比较，都为true， 因为会把Integer自动拆箱为int再去比；

Integer是引用类型，用==比较两个对象，其实比较的是它们的内存地址，所以不同的Integer对象肯定是不同的；

但是对于Integer i=*，java在编译时会将其解释成Integer i=Integer.valueOf(*)；。但是，Integer类缓存了[-128,127]之间的整数， 所以对于Integer i1=127；与Integer i2=127； 来说，i1==i2，因为这二个对象指向同一个内存单元。 而Integer i1=128；与Integer i2=128； 来说，i1==i2为false。

【各自的应用场景】

Integer默认值是null，可以区分未赋值和值为0的情况。比如未参加考试的学生和考试成绩为0的学生

加减乘除和比较运算较多，用int

容器里推荐用Integer。 对于PO实体类，如果db里int型字段允许null，则属性应定义为Integer。————默认也应当定义为包装类型，从而兼容数据为null的情况，规避NPE异常。诸如mybatis这些代码生成器生成的属性就是包装类型，我们从阿里开发规范里也可以找到类似声明———— 当然，如果系统限定db里int字段不允许null值，则也可考虑将属性定义为int。

对于应用程序里定义的枚举类型， 其值如果是整型，则最好定义为int，方便与相关的其他int值或Integer值的比较

Integer提供了一系列数据的成员和操作，如Integer.MAX_VALUE，Integer.valueOf(),Integer.compare(),compareTo(),不过一般用的比较少。建议，一般用int类型，这样一方面省去了拆装箱，另一方面也会规避数据比较时可能带来的bug。




## 泛型

{{< codeblock "A.java" "https://www.163.com" >}}
public class A {}

public static <A> List<A> asList(A... a);

public static List<A> asList(A... a);
{{< /codeblock >}}

`<T>`是申明T为泛型，以区别于类名

即：`<T>` List`<T>` 中，第一个T是告诉大家，T不是类T.class，而是泛型T
（如果只写List`<T>`则编译器以为是类T.class，如果不存在T.class类，则报错）。

案例1中A为“泛型A”，参数可以传入任何类型对象的数组；案例2则不是，其中A为"类A"，参数只能传入"类A"的对象的数组。

{{< alert info >}}
案例1中使用泛型绝不是因为要使参数可以传入任意类型，如果仅仅是这样，直接用Object就可以了。  
用泛型是因为可以使该方法的返回值成为一个指定类型的集合，这样再次使用该集合的时候就有一个明确的类型了，
这使的在将来该类型发生改变的时候编译器会报错，提醒你做相应的修改，而不是让问题暴露在运行阶段。
{{< /alert >}}


### 判断类实例中的各个属性是否有null
{{< codeblock "Utils.java" >}}
private boolean hasNull(Object obj) {
    if (obj == null) {
        return true;
    }
    final Field[] fields = ClassUtil.getDeclaredFields(obj.getClass());
    Object fieldValue = null;
    for (Field field : fields) {
        field.setAccessible(true);
        try {
            fieldValue = field.get(obj);
        } catch (Exception e) {
            e.printStackTrace();
        }
        if (null == fieldValue) {
            return true;
        }
    }
    return false;
}
{{< /codeblock >}}




## JDBC
Once a connection is obtained we can interact with the database. 
The JDBC Statement, CallableStatement, and PreparedStatement interfaces define the methods 
and properties that enable you to send SQL or PL/SQL commands and receive data from your database.

They also define methods that help bridge data type differences between Java and SQL data types used in a database.

The following table provides a summary of each interface's purpose to decide on the interface to use.

| Interfaces | Recommended Use |
| :---       | :---            |
| Statement  | Use this for general-purpose access to your database. Useful when you are using static SQL statements at runtime. The Statement interface cannot accept parameters. |
| PreparedStatement	| Use this when you plan to use the SQL statements many times. The PreparedStatement interface accepts input parameters at runtime. |
| CallableStatement	| Use this when you want to access the database stored procedures. The CallableStatement interface can also accept runtime input parameters. |


