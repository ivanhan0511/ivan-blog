---
title: "Java Basic"
date: 2022-10-26T14:05:15+08:00
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

## TYPE
---
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
---
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






## THREAD
---
### ThreadLocal


### Thread Lock
一、ReentrantLock是用来做什么的？
二、ReentrantLock的实现原理是什么？
三、ReentrantLock对比于Synchronized有哪些优缺点？
四、ReentrantLock的简单使用


一、ReentrantLock是用来做什么的？
这个类是JUC工具包中对线程安全问题提供的一种解决方案，它主要是用来给对象上锁，保证同一时间这能有一个线程在访问当前对象。这样处理是为了防止如果一个线程对某个公共变量进行了改变，而其它线程读取时读出来的是原有数据导致脏读的问题。

二、ReentrantLock的实现原理是什么？
ReentrantLock主要是通过同步队列和CAS机制来实现的，它实现的过程中主要包含下面几个属性：

status：锁状态，0表示没有线程获取锁，1表示已有线程获取锁
exclusiveOwnerThread：当前持有锁的线程
Node：节点，是ReentrantLock内部维持的一个双向链表（同步阻塞队列）的基本构成
具体流程

1、上锁更新锁状态：当A线程持有锁时，会通过CAS将status状态置为1，并将A线程自身存入exclusiveOwnerThread属性当中；
2、线程入队：而后线程B通过CAS获取锁时发现无法获取锁，此时就会获取node信息，但是由于node双向链表是null所以会通过CAS来创建一个双向链表的head对象，之后再把线程B封装成的Node节点通过尾插法接入双向链表的尾部；
3、线程阻塞：入队完成后再调用park方法进行阻塞；
4、释放锁并更新锁状态：当A线程释放锁时会将status属性重置为0，且把exclusiveOwnerThread置为null；
5、唤醒线程：A线程释放锁完毕后会调用unpark方法来唤醒双向链表中下一节点的线程B;

三、ReentrantLock对比于Synchronized有哪些优缺点？
1、ReentrantLock的锁状态是可见的，而Synchronized的锁状态是不可见的；
2、ReentrantLock是JDK层面的实现，而Synchronized是JVM层面的实现；
3、ReentrantLock需要手动释放锁，而Synchronized不需要手动释放锁；
4、ReentrantLock可以是非公平锁也可以是公平锁，而Synchronized只能是非公平锁；
5、ReentrantLock是可被中断的，而Synchronized是不可悲中断的；
6、Synchronized在特定条件下是后来的线程先获取锁，而ReentrantLock是先来的线程先获取锁；
四、ReentrantLock的简单使用


[另一篇](https://blog.csdn.net/sqL520lT/article/details/122096431)




## JDBC
---
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




## JVM
---




## TOOLS
---
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

### 兼容多种时间格式的输入
{{< tabbed-codeblock "DateForamtUtil" "java" >}}
<!-- tab Util -->
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.HashMap;
import java.util.regex.Pattern;

/**
 * author : toby
 * e-mail : 13128826083@163.com
 * time : 2018/2/2
 * desc :
 */
public class DateFormatUtil {

    /**
     * 格式化时间
     *
     * @param dateStr
     * @return
     */
    public static String dateRegFormat(String dateStr) {
        HashMap<String, String> dateRegFormat = new HashMap<>();
        // 2014年3月12日 13时5分34秒，2014-03-12 12:05:34，2014/3/12 12:5:34
        dateRegFormat.put("^\\d{4}\\D+\\d{1,2}\\D+\\d{1,2}\\D+\\d{1,2}\\D+\\d{1,2}\\D+\\d{1,2}\\D*$", "yyyy-MM-dd-HH-mm-ss");

        // 2014-03-12 12:05，2014-3-12 12:5
        dateRegFormat.put("^\\d{4}\\D+\\d{1,2}\\D+\\d{1,2}\\D+\\d{1,2}\\D+\\d{1,2}\\D*$", "yyyy-MM-dd-HH-mm");

        // 2014-03-12
        dateRegFormat.put("^\\d{4}\\D+\\d{1,2}\\D+\\d{1,2}$", "yyyy-MM-dd");

        DateFormat formatter1 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        DateFormat formatter2;
        String dateReplace;
        String strSuccess = "";
        try {
            for (String key : dateRegFormat.keySet()) {
                if (Pattern.compile(key).matcher(dateStr).matches()) {
                    formatter2 = new SimpleDateFormat(dateRegFormat.get(key));
                    dateReplace = dateStr.replaceAll("\\D+", "-");
                    strSuccess = formatter1.format(formatter2.parse(dateReplace));
                    break;
                }
            }
        } catch (Exception e) {
            System.err.println("---------日期格式无效-----------" + dateStr);
            throw new Exception("日期格式无效");
        } finally {
            return strSuccess;
        }
    }
}
<!-- endtab -->

<!-- tab call -->
private void testDateFormat() {
    String[] dateStrArray = new String[]{
        "2018-03-12 12:05:34",
        "2018-03-12 12:05",
        "2018-3-12 12:5:34",
        "2018-3-12 12:5",
        "2018-3-12",
        "2018-03-12",
        "2018/03/12 12:05:34",
        "2018/3/12 12:5:34",
        "2018/03/12 12:05",
        "2018/3/12 12:5",
        "2018/03/12",
        "2018年3月12日 13时5分34秒",
        "2018年3月12日 13时5分",
        "2018年03月12日 13时05分34秒"
    };
    for (int i = 0; i < dateStrArray.length; i++) {
        Log.d("----index---->" + i, DateFormatUtil.dateRegFormat(dateStrArray[i]));
    }
}
<!-- endtab -->
{{< /tabbed-codeblock >}}
