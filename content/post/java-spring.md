---
title: "Spring"
date: 2024-10-26T11:22:00+08:00
categories:
- Java
- WebFramework
tags:
- Java
- Spring
keywords:
- java
- spring
- bean
- ioc
- aop
clearReading: true
#thumbnailImage: //example.com/static/A.png
thumbnailImage: image-1.png
thumbnailImagePosition: bottom
autoThumbnailImage: yes
metaAlignment: center
#coverImage: //example.com/static/B.png
coverImage: image-2.png
coverCaption: "A beautiful image"
coverMeta: out
coverSize: full
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

This is a custom summary and does *NOT* appear in the post.
<!--more-->

{{< toc >}}

站在巨人的肩膀上, 多看看[廖雪峰手写Sping](https://www.liaoxuefeng.com/wiki/1539348902182944)以便了解Spring的核心逻辑思想

自己只记录一些自己觉得有用的小Tip

TODO @Ivan : 合并Java Basic和Java Spring, 预留Maven板块, 整理源码以及maven包整体定制迁移问题

## 目录

- [I. ENVIRONMENT](#chapter-1)
- [II. PACKAGE MANAGEMENT](#chapter-2)
- [III. TESTING](#chapter-3)
- [IV. OPERATION](#chapter-4)
- [V. COMMON](#chapter-5)




## I. ENVIRONMENT {#chapter-1}

### A. R&D Env
Use IDE of "InteliJ IDEA", and **download JDK wihin IDEA**

#### 1. Backup and Sync
同步之后, 大部分的配置和插件都可以同步下来, 以下仅作备考

**Configuration**
- Enable Backup and Sync -> `Enable Backup and Sync`
- **在各个项目中分别选择Java JDK环境, 不安装Java在裸机**
- Settsings -> Editor -> Code Style
  - General tab, 选择 Line separator: Unix and macOS(\n)
  - SQL -> General, 将`keywords` 和 `Built-in types`设置为大写(To upper)


**Plugins**
- IdeaVim
  - 在IDEA中创建`~/.ideavimrc`文件(实际创建在`C:\Users\Ivan\.ideavimrc`)
  - 只需在最后一行增加配置引用即可`source //wsl.localhost/Ubuntu/root/.vimrc`, 已经可以使用WSL中的配置文件了
  - 配置完, 需要退出并重新打开IDEA
  - IDEA的vim插件, 从插入模式退出到普通模式后, 自动切换为英文输入法, 暂时没搞定, Windows VisualStudio的VsVim插件倒是自动有这个配置, 暂时没琢磨明白
- ~~MyBatisCodeHelperPro(貌似有官方版, 下次试试)~~
- Redis
- RemoteHost `java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar pear-admin-pro-1.11.9-SNAPSHOT.jar`


### B. Product Env

Install the `jdk-21_windows-x64_bin.exe` on MicroSoft Windows, or `apt install openjdk-21-jdk` on Ubuntu Service

And make sure the right `$PATH` on any kind of system

**Additionally**, record more Java in MicroSoft Windows 10/11 as below:

- 在系统环境变量中增加一条, 以防止在不同环境下的命令行输入乱码
  - 变量名称: `JAVA_TOOL_OPTIONS`
  - 值: `-Duser.language=en`





## II. PACKAGE MANAGEMENT {#chapter-2}
---

### A. First of All

Maven 3.8.1 blocked http connection

- Do {{< hl-text red >}}NOT{{< /hl-text >}} edit this original IDEA maven settings file
  `C:\Program Files\JetBrains\IntelliJ IDEA 2022.2.1\plugins\maven\lib\maven3\conf\settings.xml`
  {{< codeblock "settings.xml" "XML" >}}
  ...
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    <mirror>
      <id>maven-default-http-blocker</id>
      <mirrorOf>external:http:*</mirrorOf>
      <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
      <url>http://0.0.0.0/</url>
      <blocked>true</blocked>
    </mirror>
  </mirrors>
  ...
  {{< /codeblock >}}

- Find personal maven setting path in IDEA settings and DIY it `C:\Users\ivan\.m2\settings.xml` (If not exists, create this file)

  {{< codeblock "settings.xml" "XML" >}}
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 http://maven.apache.org/xsd/settings-1.2.0.xsd">
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
    -->
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
</settings>
  {{< /codeblock >}}

- Reload pom.xml file in IDEA and automaticlly download the dependencies

### B. How to use maven





## 泛型Tips
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




## EXCEPTION
---

### TryCatchFinally

首先看个例子：

{{< tabbed-codeblock tryCatchFinally >}}
<!-- tab return2 -->
public class TestReturn {
    public int test() {
        int x  = 1;
        try {
            return ++x;
        } catch(Exception e) {

        } finally {
            ++x;
        }

        return x;
    }

    public static void main(String[] args) {
        TestReturn t = new TestReturn();
        int result = t.test();
        System.out.println(result);
    }
}

// 输出结果是2
<!-- endtab -->

<!-- tab return3 -->
public class TestReturn {
    public int test() {
        int x  = 1;
        try {
            return ++x;
        } catch(Exception e) {

        } finally {
            return ++x;
        }
    }

    public static void main(String[] args) {
        TestReturn t = new TestReturn();
        int result = t.test();
        System.out.println(result);
    }
}

// 输出结果是3
<!-- endtab -->
{{< /tabbed-codeblock >}}

查阅资料得到下面的说法：

当try语句退出时肯定会执行finally语句。这确保了即使发了一个意想不到的异常也会执行finally语句块。
但是finally的用处不仅是用来处理异常——它可以让程序员不会因为return、continue、或者break语句而忽略了清理代码。
把清理代码放在finally语句块里是一个很好的做法，即便可能不会有异常发生也要这样做。

{{< alert warning >}}
注意，当try或者catch的代码在运行的时候，JVM退出了。那么finally语句块就不会执行。同样，如果线程在运行try或者catch的代码时被中断了或者被杀死了(killed)，那么finally语句可能也不会执行了，即使整个运用还会继续执行。
{{< /alert >}}

说明：如果在try语句里有return语句，finally语句还是会执行。它会在把控制权转移到该方法的调用者或者构造器前执行finally语句。也就是说，使用return语句把控制权转移给其他的方法前会执行finally语句。


**那么结果为什么是3不是2呢?**

当执行到`return ++x;`时，jvm在执行完 ++x 后会在局部变量表里另外分配一个空间来保存当前x的值。  
现在还没把值返回给y，而是继续执行finally语句里的语句。等执行完后再把之前保存的值**(是2, 不是x)**返回给y。所以就有了y是2不是3的情况。

其实这里还有一点要注意的是，如果你在finally里也用了return语句，比如`return +xx;`, 那么y会是3。  
因为规范规定了，当try和finally里都有return时，会忽略try的return，而使用finally的return。

原文链接：https://blog.csdn.net/wujingjing_crystal/article/details/52495189




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
5、ReentrantLock是可被中断的，而Synchronized是不可被中断的；
6、Synchronized在特定条件下是后来的线程先获取锁，而ReentrantLock是先来的线程先获取锁；
四、ReentrantLock的简单使用


[示例](https://blog.csdn.net/sqL520lT/article/details/122096431)
[ReentrantLock原理](https://www.cnblogs.com/huanshilang/p/16872660.html)



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

<!-- tab Invoke -->
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




## SNOWFLAKE
---

Tmp record

自己的项目中已经不再使用, 但作为算法案例, 临时记录于此

前面文章在谈论分布式唯一ID生成的时候，有提到雪花算法，这一次，我们详细点讲解，只讲它。
SnowFlake算法

据国家大气研究中心的查尔斯·奈特称，一般的雪花大约由10^19个水分子组成。在雪花形成过程中，会形成不同的结构分支，所以说大自然中不存在两片完全一样的雪花，每一片雪花都拥有自己漂亮独特的形状。雪花算法表示生成的id如雪花般独一无二。
snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。

核心思想：分布式，唯一。
算法具体介绍
雪花算法是 64 位 的二进制，一共包含了四部分：

1位是符号位，也就是最高位，始终是0，没有任何意义，因为要是唯一计算机二进制补码中就是负数，0才是正数。
41位是时间戳，具体到毫秒，41位的二进制可以使用69年，因为时间理论上永恒递增，所以根据这个排序是可以的。
10位是机器标识，可以全部用作机器ID，也可以用来标识机房ID + 机器ID，10位最多可以表示1024台机器。
12位是计数序列号，也就是同一台机器上同一时间，理论上还可以同时生成不同的ID，12位的序列号能够区分出4096个ID。


优化
由于41位是时间戳，我们的时间计算是从1970年开始的，只能使用69年，为了不浪费，其实我们可以用时间的相对值，也就是以项目开始的时间为基准时间，往后可以使用69年。获取唯一ID的服务，对处理速度要求比较高，所以我们全部使用位运算以及位移操作，获取当前时间可以使用System.currentTimeMillis()。
时间回拨问题
在获取时间的时候，可能会出现时间回拨的问题，什么是时间回拨问题呢？就是服务器上的时间突然倒退到之前的时间。

人为原因，把系统环境的时间改了。
有时候不同的机器上需要同步时间，可能不同机器之间存在误差，那么可能会出现时间回拨问题。

解决方案

回拨时间小的时候，不生成 ID，循环等待到时间点到达。
上面的方案只适合时钟回拨较小的，如果间隔过大，阻塞等待，肯定是不可取的，因此要么超过一定大小的回拨直接报错，拒绝服务，或者有一种方案是利用拓展位，回拨之后在拓展位上加1就可以了，这样ID依然可以保持唯一。但是这个要求我们提前预留出位数，要么从机器id中，要么从序列号中，腾出一定的位，在时间回拨的时候，这个位置 +1。

由于时间回拨导致的生产重复的ID的问题，其实百度和美团都有自己的解决方案了，有兴趣可以去看看，下面不是它们官网文档的信息：

百度UIDGenerator：github.com/baidu/uid-g…

UidGenerator是Java实现的, 基于Snowflake算法的唯一ID生成器。UidGenerator以组件形式工作在应用项目中, 支持自定义workerId位数和初始化策略, 从而适用于docker等虚拟化环境下实例自动重启、漂移等场景。 在实现上, UidGenerator通过借用未来时间来解决sequence天然存在的并发限制; 采用RingBuffer来缓存已生成的UID, 并行化UID的生产和消费, 同时对CacheLine补齐，避免了由RingBuffer带来的硬件级「伪共享」问题. 最终单机QPS可达600万。


美团Leaf:tech.meituan.com/2019/03/07/…

leaf-segment 方案

优化：双buffer + 预分配
容灾：Mysql DB 一主两从，异地机房，半同步方式
缺点：如果用segment号段式方案：id是递增，可计算的，不适用于订单ID生成场景，比如竞对在两天中午12点分别下单，通过订单id号相减就能大致计算出公司一天的订单量，这个是不能忍受的。


leaf-snowflake方案

使用Zookeeper持久顺序节点的特性自动对snowflake节点配置workerID

1.启动Leaf-snowflake服务，连接Zookeeper，在leaf_forever父节点下检查自己是否已经注册过（是否有该顺序子节点）。
2.如果有注册过直接取回自己的workerID（zk顺序节点生成的int类型ID号），启动服务。
3.如果没有注册过，就在该父节点下面创建一个持久顺序节点，创建成功后取回顺序号当做自己的workerID号，启动服务。


缓存workerID，减少第三方组件的依赖
由于强依赖时钟，对时间的要求比较敏感，在机器工作时NTP同步也会造成秒级别的回退，建议可以直接关闭NTP同步。要么在时钟回拨的时候直接不提供服务直接返回ERROR_CODE，等时钟追上即可。或者做一层重试，然后上报报警系统，更或者是发现有时钟回拨之后自动摘除本身节点并报警





{{< codeblock "snowflake" "java" >}}
public class SnowFlake {

    // 数据中心(机房) id
    private long datacenterId;
    // 机器ID
    private long workerId;
    // 同一时间的序列
    private long sequence;

    public SnowFlake(long workerId, long datacenterId) {
        this(workerId, datacenterId, 0);
    }

    public SnowFlake(long workerId, long datacenterId, long sequence) {
        // 合法判断
        if (workerId > maxWorkerId || workerId < 0) {
            throw new IllegalArgumentException(String.format("worker Id can't be greater than %d or less than 0", maxWorkerId));
        }
        if (datacenterId > maxDatacenterId || datacenterId < 0) {
            throw new IllegalArgumentException(String.format("datacenter Id can't be greater than %d or less than 0", maxDatacenterId));
        }
        System.out.printf("worker starting. timestamp left shift %d, datacenter id bits %d, worker id bits %d, sequence bits %d, workerid %d",
                timestampLeftShift, datacenterIdBits, workerIdBits, sequenceBits, workerId);

        this.workerId = workerId;
        this.datacenterId = datacenterId;
        this.sequence = sequence;
    }

    // 开始时间戳（2021-10-16 22:03:32）
    private long twepoch = 1634393012000L;

    // 机房号，的ID所占的位数 5个bit 最大:11111(2进制)--> 31(10进制)
    private long datacenterIdBits = 5L;

    // 机器ID所占的位数 5个bit 最大:11111(2进制)--> 31(10进制)
    private long workerIdBits = 5L;

    // 5 bit最多只能有31个数字，就是说机器id最多只能是32以内
    private long maxWorkerId = -1L ^ (-1L << workerIdBits);

    // 5 bit最多只能有31个数字，机房id最多只能是32以内
    private long maxDatacenterId = -1L ^ (-1L << datacenterIdBits);

    // 同一时间的序列所占的位数 12个bit 111111111111 = 4095  最多就是同一毫秒生成4096个
    private long sequenceBits = 12L;

    // workerId的偏移量
    private long workerIdShift = sequenceBits;

    // datacenterId的偏移量
    private long datacenterIdShift = sequenceBits + workerIdBits;

    // timestampLeft的偏移量
    private long timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

    // 序列号掩码 4095 (0b111111111111=0xfff=4095)
    // 用于序号的与运算，保证序号最大值在0-4095之间
    private long sequenceMask = -1L ^ (-1L << sequenceBits);

    // 最近一次时间戳
    private long lastTimestamp = -1L;


    // 获取机器ID
    public long getWorkerId() {
        return workerId;
    }


    // 获取机房ID
    public long getDatacenterId() {
        return datacenterId;
    }


    // 获取最新一次获取的时间戳
    public long getLastTimestamp() {
        return lastTimestamp;
    }


    // 获取下一个随机的ID
    public synchronized long nextId() {
        // 获取当前时间戳，单位毫秒
        long timestamp = timeGen();

        if (timestamp < lastTimestamp) {
            System.err.printf("clock is moving backwards.  Rejecting requests until %d.", lastTimestamp);
            throw new RuntimeException(String.format("Clock moved backwards.  Refusing to generate id for %d milliseconds",
                    lastTimestamp - timestamp));
        }

        // 去重
        if (lastTimestamp == timestamp) {

            sequence = (sequence + 1) & sequenceMask;

            // sequence序列大于4095
            if (sequence == 0) {
                // 调用到下一个时间戳的方法
                timestamp = tilNextMillis(lastTimestamp);
            }
        } else {
            // 如果是当前时间的第一次获取，那么就置为0
            sequence = 0;
        }

        // 记录上一次的时间戳
        lastTimestamp = timestamp;

        // 偏移计算
        return ((timestamp - twepoch) << timestampLeftShift) |
                (datacenterId << datacenterIdShift) |
                (workerId << workerIdShift) |
                sequence;
    }

    private long tilNextMillis(long lastTimestamp) {
        // 获取最新时间戳
        long timestamp = timeGen();
        // 如果发现最新的时间戳小于或者等于序列号已经超4095的那个时间戳
        while (timestamp <= lastTimestamp) {
            // 不符合则继续
            timestamp = timeGen();
        }
        return timestamp;
    }

    private long timeGen() {
        return System.currentTimeMillis();
    }

    public static void main(String[] args) {
        SnowFlake worker = new SnowFlake(1, 1);
        long timer = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
            worker.nextId();
        }
        System.out.println(System.currentTimeMillis());
        System.out.println(System.currentTimeMillis() - timer);
    }
}
{{< /codeblock >}}



问题分析
1. 第一位为什么不使用?
在计算机的表示中，第一位是符号位，0表示整数，第一位如果是1则表示负数，我们用的ID默认就是正数，所以默认就是0，那么这一位默认就没有意义。
2.机器位怎么用？
机器位或者机房位，一共10 bit，如果全部表示机器，那么可以表示1024台机器，如果拆分，5 bit 表示机房，5bit表示机房里面的机器，那么可以有32个机房，每个机房可以用32台机器。
3. twepoch表示什么？
由于时间戳只能用69年，我们的计时又是从1970年开始的，所以这个twepoch表示从项目开始的时间，用生成ID的时间减去twepoch作为时间戳，可以使用更久。
4. -1L ^ (-1L << x) 表示什么？
表示 x 位二进制可以表示多少个数值，假设x为3：
在计算机中，第一位是符号位，负数的反码是除了符号位，1变0，0变1, 而补码则是反码+1：
txt复制代码-1L 原码：1000 0001
-1L 反码：1111 1110
-1L 补码：1111 1111

从上面的结果可以知道，-1L其实在二进制里面其实就是全部为1,那么 -1L 左移动 3位，其实得到 1111 1000，也就是最后3位是0，再与-1L异或计算之后，其实得到的，就是后面3位全是1。-1L ^ (-1L << x) 表示的其实就是x位全是1的值，也就是x位的二进制能表示的最大数值。
5.时间戳比较
在获取时间戳小于上一次获取的时间戳的时候，不能生成ID，而是继续循环，直到生成可用的ID，这里没有使用拓展位防止时钟回拨。
6.前端直接使用发生精度丢失
如果前端直接使用服务端生成的long 类型 id，会发生精度丢失的问题，因为 JS 中Number是16位的（指的是十进制的数字），而雪花算法计算出来最长的数字是19位的，这个时候需要用 String 作为中间转换，输出到前端即可。
秦怀の观点
雪花算法其实是依赖于时间的一致性的，如果时间回拨，就可能有问题，一般使用拓展位解决。而只能使用69年这个时间限制，其实可以根据自己的需要，把时间戳的位数设置得更多一点，比如42位可以用139年，但是很多公司首先得活下来。当然雪花算法也不是银弹，它也有缺点，在单机上递增，而多台机器只是大致递增趋势，并不是严格递增的。
没有最好的设计方案，只有合适和不合适的方案。
【作者简介】：
秦怀，公众号【秦怀杂货店】作者，技术之路不在一时，山高水长，纵使缓慢，驰而不息。个人写作方向：Java源码解析，JDBC，Mybatis，Spring，redis，分布式，剑指Offer，LeetCode等，认真写好每一篇文章，不喜欢标题党，不喜欢花里胡哨，大多写系列文章，不能保证我写的都完全正确，但是我保证所写的均经过实践或者查找资料。遗漏或者错误之处，还望指正。

作者：秦怀杂货店
链接：https://juejin.cn/post/7030825216162922510
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 目录

- [I. IOC](#chapter-1)
- [II. AOP](#chapter-2)
- [III. SERVLET](#chapter-3)
- [IV. SECURITY](#chapter-4)
- [V. TRANSACTIONAL](#chapter-5)
- [VI. JOB](#chapter-6)
- [VII. THIRD-PART HTTP API](#chapter-7)
- [VIII. EVENT MONITOR](#chapter-8)
- [IX. DAO MAPPER](#chapter-9)
- [X. UTILS](#chapter-10)
- [XI. MISCELLANEOUS](#chapter-11)




## I. IOC {#chapter-1}
---

### A. LifeCircle-@Configuration

https://www.cnblogs.com/java-chen-hao/p/11640177.html




## II. AOP {#chapter-2}
---
JetBrains IDEA 中如果想了解某个注解的实现, 没有太好的办法, 就像想查找一个类在哪里被反射并调用了, 试行的办法:
- 阅读大厂的源码, 文档, 了解该注解的内部实现逻辑
- 小厂的代码, 只能是全文查找类似这样的`getAnnotation(DictFormat.class)`代码


### 常用注解的深入理解
- [TODO]: 各种继承关系的注解, 类似RestController, 包含了ResponseBody等, 后续补充
- Controller中设置Headers, 指定Content-Type/Accept
  - Springboot中Controller中的comsumes所指定的是HTTP客户端的Content-Type的内容, 默认application/x-www-form-urlencoded
  - Springboot中Controller中的produces所指定的是HTTP客户端的Accept的内容

{{< blockquote "HTTP回顾" "来自于网络" >}}
常见的http头部Content-Type:<br>
- application/x-www-form-urlencoded<br>
- multipart/form-data<br>
- application/json<br>
- application/xml<br>
<br>
前后组合如下:<br>
- 服务端使用   Content-Type:"application/json"编码http响应body内容返回给前端；前端使用Content-Type:"application/json"解码http响应body内容。<br>
- 服务端返回   Response Content-Type:application/json，前端dataType 不指定值 "json". 此时，解码http响应body内容，   data类型是Object。<br>
- 服务端不返回 Response Content-Type:application/json，前端dataType 指定值   "json". 些时，解码http响应body内容，   data类型是Object。<br>
- 服务端不返回 Response Content-Type:application/json，前端dataType 不指定值 "json". 此时，不能解码http响应body内容，data类型是String。<br>
{{< /blockquote >}}




### 易混淆注解的对比

#### @Component @Bean @Service @Configurable
With this annotation, a class can be scaned manually or automaticlly

`@Bean`, 

`@Component`与`@Service`没有区别, `@Service`还没想好, 是一个预留的注解定义

`@Configurable`是单例模式, 在启动时加载




**@Resource @Autowired**

{{< blockquote "stackoverflow" "https://stackoverflow.com/questions/4093504/resource-vs-autowired" "@Resource vs @Autowired" >}}
Both @Autowired (or @Inject) and @Resource work equally well. But there is a conceptual difference or a difference in the meaning<br/><br/>
@Resource means get me a known resource by name. The name is extracted from the name of the annotated setter or field, or it is taken from the name-Parameter.<br/><br/>
@Inject or @Autowired try to wire in a suitable other component by type.<br/><br/>
So, basically these are two quite distinct concepts. Unfortunately the Spring-Implementation of @Resource has a built-in fallback, which kicks in when resolution by-name fails. In this case, it falls back to the @Autowired-kind resolution by-type.<br/>
While this fallback is convenient, IMHO it causes a lot of confusion, because people are unaware of the conceptual difference and tend to use @Resource for type-based autowiring.

{{< /blockquote >}}


**@Repository @Mapper**
- @Mapper 一定要有，否则 Mybatis 找不到 mapper。
- @Repository 可有可无，可以消去依赖注入的报错信息。
- @MapperScan 可以替代 @Mapper。


**@Validate @Valid**
- @Validate是org.springframework.validation.annotation.Validated 导入的
- @Validate 可以分组
- @Valid是javax.validation.Valid 导入的
- @Valid 可以递归
- controller类上写: @Validated
  - 如果是Bean的对象xxxRequest类限制参数, 则参数类中各自校验; 在controller类中的方法参数括号内写: @Valid
  - 如果是属性String number限制, 在controller类中的方法入参前写: @NotBlank(message = "xxx不能为空")

PS:

@RequestParam()不那么好用, 返回信息不友好




## III. SERVLET {#chapter-3}
---



## IV. SECURITY {#chapter-4}
---

以下大概描述的是Java Security的主要组件结构, 即使遗忘了实现细节, 掌握其结构和机制, 就容易理解得多

**Configure**
这里包含初始化, 注册哪些filter


**Filter**
具体拦截, 并把相对应的且support()的provider传进去


**Provider**
提供自己的认证比对实现方法, 并且实现`support()`满足filter查找


**Handler**
增加成功与失败的handler




## V. TRANSACTIONAL {#chapter-5}
---

### A. Propagation

- 事务传播, 需要时则加入传播的扩散要求  
  @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)


### B. Isolation

以下是 MySQL 事务隔离级别的简要场景及效果区别：

#### 1. Read Uncommitted
**场景**: 允许读取未提交的数据。  
**效果**:  
- **脏读**: 可能发生，读取其他事务未提交的数据。  
- **不可重复读**: 可能发生，因为事务间可能修改已读取数据。  
- **幻读**: 可能发生，因为可能插入新数据。  

### 2. Read Committed  
**场景**: 只读取已提交的数据，避免读取未提交事务的内容。  
**效果**:  
- **脏读**: 不会发生，保证读取的是已提交数据。  
- **不可重复读**: 可能发生，因为同一事务中重复读取时，数据可能被其他事务修改。  
- **幻读**: 可能发生，因为其他事务可能插入新记录。  

**Tips**

不可重复读重点在于update和delete


#### 3. Repeatable Read (MySQL默认)
**场景**: 保证同一事务中，读取的结果是固定的，即便其他事务修改了数据。  
**效果**:  
- **脏读**: 不会发生。  
- **不可重复读**: 不会发生，事务中数据一致。  
- **幻读**: 通过**间隙锁**避免，查询范围内的数据是固定的。  

**Tips**

该级别产生幻读的重点场景在于insert, 总的来说就是事务A对数据进行操作，事务B还是可以用insert插入数据的，因为使用的是行锁，这样导致的各种奇葩问题就是幻读，表现形式很多
TODO: MySQL的间隙锁在开源代码中是否已经默认被应用


#### 4. Serializable
**场景**: 强制事务串行化，确保事务间完全隔离。  
**效果**:  
- **脏读**: 不会发生。  
- **不可重复读**: 不会发生。  
- **幻读**: 不会发生，因为会对范围内的操作加锁，阻止插入或修改。  


#### 总结表格

| Isolation Level  | 脏读(Dirty Read) | 不可重复读(Non Repeatable Read) | 幻读(Phantom Read) |
|------------------|------|------------|-------|
| Read Uncommitted | ✅   | ✅         | ✅    |
| Read Committed   | ❌   | ✅         | ✅    |
| Repeatable Read  | ❌   | ❌         | ❌    |
| Serializable     | ❌   | ❌         | ❌    |


[Mysql 事务隔离级别和锁的关系](https://www.cnblogs.com/zhengzhaoxiang/p/13972252.html)

> **锁防止别的事务修改或删除，GAP锁防止别的事务新增，**行锁和 GAP锁结合形成的的 Next-Key锁共同解决了 RR级别在写数据时的幻读问题。
>
> ### 十一、Serializable
>
> 这个级别很简单，读加共享锁，写加排他锁，读写互斥。使用的悲观锁的理论，实现简单，数据更加安全，但是并发能力非常差。如果你的业务并发的特别少或者没有并发，同时又要求数据及时可靠的话，可以使用这种模式。
>
> > 这里要吐槽一句，不要看到 select就说不会加锁了，在Serializable这个级别，还是会加锁的！



[Java面试必问的MySQL锁与事务隔离级别](https://juejin.cn/post/6920072842935533576)

> ### ReadView
>
> 如果数据库隔离级别是未提交读（READ UNCOMMITTED），那么读取版本链中最新版本的记录即可。如果是是串行化（SERIALIZABLE），事务之间是加锁执行的，不存在读不一致的问题。但是如果是已提交读（READ COMMITTED）或者可重复读（REPEATABLE READ），就需要遍历版本链中的每一条记录，判断该条记录是否对当前事务可见，直到找到为止(遍历完还没找到就说明记录不存在)。InnoDB通过ReadView实现了这个功能。ReadView中主要包含以下4个内容：
>
> #### 1.m_ids
>
> 表示在生成ReadView时当前系统中活跃的读写事务的事务id列表。
>
> #### 2.min_trx_id
>
> 表示在生成ReadView时当前系统中活跃的读写事务中最小的事务id，也就是m_ids中的最小值。
>
> #### 3.max_trx_id
>
> 表示生成ReadView时系统中应该分配给下一个事务的id值。
>
> #### 4.creator_trx_id
>
> 表示生成该ReadView事务的事务id。
>
> 有了ReadView之后，我们可以基于以下步骤判断某个版本的记录是否对当前事务可见。
>
> 1.如果被访问版本的trx_id属性值与ReadView中的creator_trx_id值相同，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。
>
> 2.如果被访问版本的trx_id属性值小于ReadView中的min_trx_id值，表明生成该版本的事务在当前事务生成ReadView前已经提交，所以该版本可以被当前事务访问。
>
> 3.如果被访问版本的trx_id属性值大于或等于ReadView中的max_trx_id值，表明生成该版本的事务在当前事务生成ReadView后才开启，所以该版本不可以被当前事务访问。
>
> 4.如果被访问版本的trx_id属性值在ReadView的min_trx_id和max_trx_id之间，那就需要判断一下trx_id属性值是不是在m_ids列表中，如果在，说明创建ReadView时生成该版本的事务还是活跃的，该版本不可以被访问；如果不在，说明创建ReadView时生成该版本的事务已经被提交，该版本可以被访问。
>
> 在MySQL中，READ COMMITTED和REPEATABLE READ隔离级别的的一个非常大的区别就是它们生成ReadView的时机不同。READ COMMITTED在每次读取数据前都会生成一个ReadView，这样就能保证每次都能读到其它事务已提交的数据。REPEATABLE READ 只在第一次读取数据时生成一个ReadView，这样就能保证后续读取的结果完全一致。
>
> 作者：IT中心
> 链接：https://juejin.cn/post/6920072842935533576
> 来源：稀土掘金
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



[数据库事务、隔离级别和锁ACID的真实含义隔离级别和并发控制MySQL和PostgreSQL对比如何写代码](https://cloud.tencent.com/developer/article/1121737)略读




### C. 多数据源

{{< alert info >}}
@DS 必须加在 @Transactional 对应的类或者方法上  
如在 mapper中加了@DS，但是 @Transactional 加在 service 方法中，此时则会获取默认的datasource
(因为在事务中已经获取了一次datasource的connection，而此时无DS注解)
{{< /alert >}}

{{< tabbed-codeblock MultiDSTransactional >}}
<!-- tab sInterface -->
public interface SomeService extends IService<Some> {}
<!-- endtab -->

<!-- tab "sImpl" -->
@Service
public class SomeServiceImpl extends ServiceImpl<SomeRepository, Some> implements SomeService {}
<!-- endtab -->

<!-- tab oInterface -->
public interface OtherService extends IService<Other> {}
<!-- endtab -->

<!-- tab oImpl -->
@Service
@DS("db2")
public class OtherServiceImpl extends ServiceImpl<OtherRepository, Other> implements OtherService {}
<!-- endtab -->

<!-- tab combo -->
public interface CombineService {
    Boolean save(CombineRequest request);
}
<!-- endtab -->

<!-- tab comboImpl -->
@Service
public class CombineServiceImpl implements CombineService {
    @Resource
    private SomeService someService;

    @Resource
    private OtherService otherService;

    @Override
    @DSTransactional
    public Boolean save(CombineRequest request) {
        Boolean r1 = saveSome(request);
        Boolean r2 = saveOthers(request);
        return r1 && r2;
    }

    @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)
    protected Boolean saveOthers(CombineRequest request) {
        List<Other> others = methodToGetOthers(request);
        return otherService.saveBatch(others);
    }

    private Boolean saveSome(CombineRequest request) {
        Some some = methodToGetSome(request);
        return someService.save(some);
    }
}
<!-- endtab -->
{{< /tabbed-codeblock >}}


[TODO]:
MyBatis的手动切换, 后续再深入学习  
And how to `DynamicDataSourceContextHolder.push()` and `DynamicDataSourceContextHolder.poll()`?



## VI. JOB {#chapter-6}
---
Quartz




## VII. THIRD-PART HTTP API {#chapter-7}
---
RestTemplate 是从Spring3.0开始支持的一个远程HTTP请求工具，RestTemplate提供了常见的Rest服务(Rest风格、Rest架构)的客户端请求的模版，能够大大提高客户端的编写效率

多年来，Spring框架的RestTemplate一直是客户端HTTP访问的首选解决方案，它提供了同步、阻塞API以简洁的方式处理HTTP请求。然而，随着对非阻塞、反应式编程以更少的资源处理并发的需求不断增加，特别是在微服务架构中，RestTemplate已经显露出其局限性。
从Spring Framework 5开始，RestTemplate已被标记为已弃用，Spring团队推荐WebClient作为其继任者。在这篇文章中，我们将通过实际示例深入探讨RestTemplate被弃用的原因、采用WebClient的优势以及如何有效过渡。

{{< blockquote >}}
Synchronous client to perform HTTP requests, exposing a simple, template method API over underlying HTTP client libraries such as the JDK HttpURLConnection.

NOTE: As of 6.1, RestClient offers a more modern API for synchronous HTTP access. For asynchronous and streaming scenarios, consider the reactive org. springframework. web. reactive. function. client. WebClient.
{{< /blockquote >}}

传统应用: 新的RestClient

流式应用: 才考虑WebClient



## VIII. EVENT MONITOR {#chapter-8}
---
ApplicationEvent Monitor
module-bpm模块中有用到, 拿来主义, 上游入库后"生产"出下游库存就依靠这个监听者观察客户定制的BOM原料是否满足, 满足即可生产
[TODO]:




## IX. DAO MAPPER {#chapter-9}
---




## X. UTILS {#chapter-10}
---
很多源码中总结的Utils很好用, 多学习一下



## XI. MISCELLANEOUS {#chapter-11}
---

