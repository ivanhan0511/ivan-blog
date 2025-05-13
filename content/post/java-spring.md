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


## I. ENVIRONMENT
---

### A. R&D and Testing Env
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

**Additionally**, record more Java in MicroSoft Windows 10/11 as below:

- 在系统环境变量中增加一条, 以防止在不同环境下的命令行输入乱码
  - 变量名称: `JAVA_TOOL_OPTIONS`
  - 值: `-Duser.language=en`


### B. Product Env and Fixing Bugs

Install the `jdk-21_windows-x64_bin.exe` on MicroSoft Windows, or `apt install openjdk-21-jdk` on Ubuntu Service

And make sure the right `$PATH` on any kind of system

使用公司的开发服务器作为部署服务器, 与客户生产环境保持一样的数据库结构和代码打包

如果遇到生产环境的问题, 使用该环境用于验证, 备份和修复


### C. Demo Env

Install the `jdk-21_windows-x64_bin.exe` on MicroSoft Windows, or `apt install openjdk-21-jdk` on Ubuntu Service

And make sure the right `$PATH` on any kind of system

部署在云上, 使用域名, 仅作对外演示, 使用当前的生产版本




## II. PACKAGE MANAGEMENT
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









## III. JVM
---
TODO: Some basic JVM knowledge







## IV. IOC
---

### A. LifeCircle

@Configuration

https://www.cnblogs.com/java-chen-hao/p/11640177.html




## V. AOP {#chapter-2}
---
JetBrains IDEA 中如果想了解某个注解的实现, 没有太好的办法, 就像想查找一个类在哪里被反射并调用了, 试行的办法:
- 阅读大厂的源码, 文档, 了解该注解的内部实现逻辑
- 小厂的代码, 只能是全文查找类似这样的`getAnnotation(DictFormat.class)`代码


### A. HTTP 请求注解
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
- 服务端设置`Content-Type:"application/json"`并返回给前端, 前端dataType指定"json"<br>
  可以解码HTTP的body内容<br>
- 服务端设置`Content-Type:application/json`并返回给前端, 前端dataType不指定"json"<br>
  可以解码HTTP的body内容, data类型是Object<br>
- 服务端不设置`Content-Type:application/jsonResponse`并返回给前端, 前端dataType指定"json"<br>
  可以解码HTTP的body内容, data类型是Object<br>
- 服务端不设置`Content-Type:application/json`并返回给前端, 前端dataType不指定"json"<br>
  不能解码HTTP的body内容, data类型是String<br>
{{< /blockquote >}}




### B. 易混淆注解的对比

@Component @Bean @Service @Configurable, With this annotation, a class can be scaned manually or automaticlly

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




## VI. SERVLET
---



## VII. SECURITY
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




## VIII. JOB
---
Quartz




## IX. THIRD-PART HTTP API
---
RestTemplate 是从Spring3.0开始支持的一个远程HTTP请求工具，RestTemplate提供了常见的Rest服务(Rest风格、Rest架构)的客户端请求的模版，能够大大提高客户端的编写效率

多年来，Spring框架的RestTemplate一直是客户端HTTP访问的首选解决方案，它提供了同步、阻塞API以简洁的方式处理HTTP请求。然而，随着对非阻塞、反应式编程以更少的资源处理并发的需求不断增加，特别是在微服务架构中，RestTemplate已经显露出其局限性。
从Spring Framework 5开始，RestTemplate已被标记为已弃用，Spring团队推荐WebClient作为其继任者。在这篇文章中，我们将通过实际示例深入探讨RestTemplate被弃用的原因、采用WebClient的优势以及如何有效过渡。

{{< blockquote >}}
Synchronous client to perform HTTP requests, exposing a simple, template method API over underlying HTTP client libraries such as the JDK HttpURLConnection.
<br>
NOTE: As of 6.1, RestClient offers a more modern API for synchronous HTTP access. For asynchronous and streaming scenarios, consider the reactive org. springframework. web. reactive. function. client. WebClient.
{{< /blockquote >}}

传统应用: 新的RestClient

流式应用: 才考虑WebClient




## X. EVENT MONITOR
---
ApplicationEvent Monitor
module-bpm模块中有用到, 拿来主义, 上游入库后"生产"出下游库存就依靠这个监听者观察客户定制的BOM原料是否满足, 满足即可生产
[TODO]:




## XI. DAO MAPPER
---

| 层级        | 技术/概念                | 说明                             |
| --------- | -------------------- | ------------------------------ |
| **应用层**   | `@Transactional`     | Spring 提供的注解式事务管理，声明式封装        |
| **框架层**   | Spring Tx（事务管理器）     | 管理事务的开始、提交、回滚等流程               |
| **数据访问层** | JDBC / JPA / MyBatis | 实际执行 SQL 的工具，Spring Tx 会和它们协作  |
| **驱动层**   | JDBC Driver          | 具体数据库厂商实现的事务接口                 |
| **数据库层**  | MySQL / Postgres 等   | 数据真正执行事务：BEGIN、COMMIT、ROLLBACK |


### A. Transactional

@Transactional 是 Spring 提供的声明式事务语法糖；

它的本质行为是基于 JDBC 的事务控制(或者 JPA 的 EntityManager，但底层还是 JDBC)

它不属于"纯粹的 JDBC API 知识", 但必须理解 JDBC 事务机制，你才能真正理解它怎么起作用

**Spring @Transactional 的执行流程图**
{{< blockquote flow >}}
调用某个标注 @Transactional 的方法</br>
↓</br>
Spring AOP 拦截这个方法调用（通过代理）</br>
↓</br>
进入事务拦截器 TransactionInterceptor</br>
↓</br>
  ├─ 判断当前线程是否已有事务</br>
  │    ├─ 如果已有事务，则根据传播行为决定是否复用/新建</br>
  │    └─ 如果没有事务，开始新事务：</br>
  │         └─ DataSource 获取连接，setAutoCommit(false)</br>
↓</br>
执行业务方法</br>
  ├─ 方法正常返回：提交事务（conn.commit）</br>
  └─ 方法抛出异常：根据规则决定是否 rollback</br>
↓</br>
释放连接（交还给连接池）</br>
↓</br>
返回控制权
{{< /blockquote >}}

0. 图解 Spring @Transactional 方法执行的源码栈流程, 从业务代码出发：
{{< codeblock call java >}}
@Service
public class UserService {

    @Transactional
    public void createUser() {
        // 保存用户
        userRepository.save(...);
    }
}
{{< /codeblock >}}

1. 调用 userService.createUser()，其实是代理对象在执行：
{{< codeblock infact java >}}
// 原始调用
userService.createUser();

// 实际是 Spring 生成的代理对象执行：
proxy.invoke(createUser)
{{< /codeblock >}}

2. 代理对象内部执行：TransactionInterceptor.invoke(...)
{{< codeblock invode java>}}
public Object invoke(MethodInvocation invocation) throws Throwable {
    // 获取事务属性（来自 @Transactional）
    TransactionAttribute txAttr = getTransactionAttributeSource().getTransactionAttribute(...);

    // 获取事务管理器（DataSourceTransactionManager）
    PlatformTransactionManager tm = determineTransactionManager(...);

    // 创建事务（开启 conn.setAutoCommit(false)）
    TransactionStatus status = tm.getTransaction(txAttr);

    Object retVal;
    try {
        // 调用真正的方法体
        retVal = invocation.proceed();

        // 没异常 -> 提交事务
        tm.commit(status);
    } catch (Throwable ex) {
        // 有异常 -> 回滚事务
        tm.rollback(status);
        throw ex;
    }

    return retVal;
}
{{< /codeblock >}}

3. 数据库连接处理（DataSourceTransactionManager）
{{< codeblock manager java>}}
public TransactionStatus getTransaction(...) {
    // 从当前线程获取 ConnectionHolder，如果没有就新建连接
    Connection con = dataSource.getConnection();

    // 设置 autoCommit 为 false
    con.setAutoCommit(false);

    // 用 TransactionSynchronizationManager 绑定到当前线程
}
{{< /codeblock >}}

4. 提交 or 回滚阶段
{{< codeblock commit java>}}
// 正常提交：
con.commit();

// 异常回滚：
con.rollback();
{{< /codeblock >}}

5. 小结：完整调用栈路径
{{< blockquote >}}
你写的 createUser() 方法</br>
   ↓</br>
Spring AOP 代理对象（JDK / CGLIB）</br>
   ↓</br>
TransactionInterceptor.invoke()</br>
   ↓</br>
DataSourceTransactionManager.getTransaction()</br>
   ↓</br>
业务方法执行</br>
   ↓</br>
commit 或 rollback
{{< /blockquote >}}

💬 小提示
第一次执行事务方法时，Spring 会做大量准备工作，但 之后会缓存事务属性（如传播行为、异常回滚规则等），执行效率是很高的。

事务状态、连接等都存在线程局部变量中，一个线程一个事务上下文。

你用的 @Transactional 其实是调用链的“开关”，底下跑的是一整套 AOP + JDBC 的组合机制。




#### 1. Propagation

| 类型               | 名称                    | 说明 |
| ---------------- | --------------------- | -- |
| ✅ `REQUIRED`     | 有事务用当前，没有就新建（默认）      |    |
| ✅ `REQUIRES_NEW` | 始终新建事务，挂起外层事务         |    |
| ✅ `NESTED`       | 如果有事务，就嵌套一个“保存点”；否则新建 |    |
| `SUPPORTS`       | 有事务就参与，没有就非事务执行       |    |
| `NOT_SUPPORTED`  | 总是非事务执行，会挂起已有事务       |    |
| `MANDATORY`      | 必须有事务，没有就抛异常          |    |
| `NEVER`          | 必须无事务，有事务就抛异常         |    |


**1.1 REQUIRED(默认)**
{{< codeblock REQUIRED java>}}
@Transactional(propagation = Propagation.REQUIRED)
public void methodA() {
    methodB(); // methodB 也是 @Transactional（默认）
}
{{< /codeblock >}}

行为：methodB 会复用 methodA 的事务

优点：简单、效率高

缺点：如果 B 抛异常，整个 A 都回滚，容易传播太广


**1.2 REQUIRES_NEW(强制新事务)**
{{< codeblock REQUIRED_NEW java>}}
java
复制
编辑
@Transactional
public void outer() {
    saveMainData();
    try {
        saveLog(); // 新事务
    } catch (Exception e) {
        log.error("日志保存失败");
    }
}

@Transactional(propagation = Propagation.REQUIRES_NEW)
public void saveLog() {
    ...
}
{{< /codeblock >}}

行为：挂起 outer() 的事务，自己创建一个新事务

适合场景：记录操作日志、异步保存等，不影响主流程; 希望“部分提交”的子操作，即使主事务失败也不影响它


**1.3 NESTED(嵌套事务)**
{{< codeblock NESTED java>}}
@Transactional
public void parent() {
    save1(); // 插入数据1
    try {
        save2(); // 抛异常
    } catch (Exception e) {
        ...
    }
    save3();
}

@Transactional(propagation = Propagation.NESTED)
public void save2() {
    ...
}
{{< /codeblock >}}

行为：如果有事务，就创建一个“保存点”，可以局部回滚而不影响外层；如果没有事务，则等价于 REQUIRED。

底层依赖 JDBC 的 savepoint 支持(*注意：不是所有数据库 + JDBC 驱动都支持 NESTED*)


**1.4 实战中常见“坑”与场景**

a. ❌内部方法调用事务失效
{{< codeblock trap java>}}
@Transactional
public void outer() {
    this.inner(); // 不会经过代理，事务注解失效！
}

@Transactional
public void inner() {
    ...
}
{{< /codeblock >}}

解决方案：

- 用 AOP 自调用方案：将 inner() 放到别的 bean 中
- 或通过 ApplicationContext.getBean(...).inner() 调用

b. 🔥主操作成功与否, 不影响子操作提交（REQUIRES_NEW）
{{< codeblock solution java>}}
@Transactional
public void mainProcess() {
    ...
    try {
        logService.writeLog(); // 使用 REQUIRES_NEW
    } catch (Exception e) {
        log.warn("日志失败，但主流程继续");
    }
}
{{< /codeblock >}}

c. ⚠想回滚子操作, 但主事务继续（NESTED）
{{< codeblock  java>}}
@Transactional
public void saveAll() {
    ...
    try {
        subSave(); // NESTED
    } catch (Exception e) {
        log.warn("子保存失败，但父继续");
    }
}
{{< /codeblock >}}

只会 rollback 到保存点，不会影响外层事务。

**🛠 总结：如何选择事务传播行为?**

|目标	                                    |建议使用    |
|---                                        |--- |
|所有操作要么全成功，要么全失败	                |REQUIRED   |
|主操作成功与否不影响子操作提交	                |REQUIRES_NEW   |
|子操作失败只想 rollback 子操作，不影响父流程    |NESTED |
|子操作不是核心流程，可无事务执行                |NOT_SUPPORTED or SUPPORTS   |




#### 2. Isolation

以下是 MySQL 事务隔离级别的简要场景及效果区别：

- Read Uncommitted

  **场景**: 允许读取未提交的数据。  

  **脏读**: 可能发生，读取其他事务未提交的数据。  

  **不可重复读**: 可能发生，因为事务间可能修改已读取数据。  

  **幻读**: 可能发生，因为可能插入新数据。  

- Read Committed  

  **场景**: 只读取已提交的数据，避免读取未提交事务的内容。  

  **脏读**: 不会发生，保证读取的是已提交数据。  

  **不可重复读**: 可能发生，因为同一事务中重复读取时，数据可能被其他事务修改。  

  **幻读**: 可能发生，因为其他事务可能插入新记录。  

  **Tips**: 不可重复读重点在于update和delete

- Repeatable Read (MySQL默认)

  **场景**: 保证同一事务中，读取的结果是固定的，即便其他事务修改了数据。  

  **脏读**: 不会发生。  

  **不可重复读**: 不会发生，事务中数据一致。  

  **幻读**: 通过**间隙锁**避免，查询范围内的数据是固定的。  

  **Tips**: 该级别产生幻读的重点场景在于insert, 总的来说就是事务A对数据进行操作，事务B还是可以用insert插入数据的，因为使用的是行锁，这样导致的各种奇葩问题就是幻读，表现形式很多

- Serializable

  **场景**: 强制事务串行化，确保事务间完全隔离。  

  **脏读**: 不会发生。  

  **不可重复读**: 不会发生。  

  **幻读**: 不会发生，因为会对范围内的操作加锁，阻止插入或修改。  

- 总结表格

| Isolation Level  | 脏读(Dirty Read) | 不可重复读(Non Repeatable Read) | 幻读(Phantom Read) |
| ---------------- | --- | ----- | --- |
| READ UNCOMMITTED | ✅ 有 | ✅ 有   | ✅ 有 |
| READ COMMITTED   | ❌ 无 | ✅ 有   | ✅ 有 |
| REPEATABLE READ  | ❌ 无 | ❌ 无   | ✅ 有 |
| SERIALIZABLE     | ❌ 无 | ❌ 无   | ❌ 无 |

MySQL 默认是 REPEATABLE READ，但在 Spring 中默认是 DEFAULT，表示由数据库决定
{{< blockquote >}}
通常情况下，select语句是不会对数据加锁，妨碍影响其他的DML和DDL操作。同时，在多版本一致读机制的支持下，select语句也不会被其他类型语句所阻碍</br>
而select... for update 语句是我们经常使用手工加锁语句。在数据库中执行select...for update,大家会发现会对数据库中的表或某些行数据进行锁表，在mysql四中，如果查询条件带有主键，会锁行数据，如果没有，会锁表</br>
由于InnoDB预设是Row-LevelLock，所以只有「明确」的指定主键，MySQL才会执行Row lock(只锁住被选取的资料例)，否则MySQL将会执行Table Lock(将整个资料表单给锁住)。
{{< /blockquote >}}




**2.1 Isolation生命周期**

隔离级别不是你在 Java 方法中设定了就马上生效。它的生效时机是：在“开启数据库事务时”

✅ 正确做法（如确实要变更隔离级别）：
{{< codeblock isolation java >}}
@Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.SERIALIZABLE)
public void innerMethod() { ... }
{{< /codeblock >}}
这样 Spring 会挂起外部事务，开启一个新事务，才会按你的 Isolation 设置来生效！


**2.2 阅读列表文章**

[Mysql 事务隔离级别和锁的关系](https://www.cnblogs.com/zhengzhaoxiang/p/13972252.html)

> **锁防止别的事务修改或删除，GAP锁防止别的事务新增，**行锁和 GAP锁结合形成的的 Next-Key锁共同解决了 RR级别在写数据时的幻读问题。
>
> 十一、Serializable
>
> 这个级别很简单，读加共享锁，写加排他锁，读写互斥。使用的悲观锁的理论，实现简单，数据更加安全，但是并发能力非常差。如果你的业务并发的特别少或者没有并发，同时又要求数据及时可靠的话，可以使用这种模式。
>
> > 这里要吐槽一句，不要看到 select就说不会加锁了，在Serializable这个级别，还是会加锁的！

[Java面试必问的MySQL锁与事务隔离级别](https://juejin.cn/post/6920072842935533576)

[数据库事务、隔离级别和锁ACID的真实含义隔离级别和并发控制MySQL和PostgreSQL对比如何写代码](https://cloud.tencent.com/developer/article/1121737)略读




### B. JDBC

#### 1. JDBC事务的控制流程

{{< tabbed-codeblock JDBC_Spring java >}}
<!-- tab jdbc -->

Connection conn = dataSource.getConnection();
conn.setAutoCommit(false);
try {
    // 执行业务SQL
    conn.commit();
} catch (Exception e) {
    conn.rollback();
} finally {
    conn.close();
}
<!-- endtab -->
<!-- tab spring -->
@Transactional
public void doBiz() {
    userRepo.save(user);
    orderRepo.save(order);
}
// Spring 自动管理事务
<!-- endtab -->
{{< /tabbed-codeblock >}}




#### 2. 学习列表

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




## XII. UTILS
---
很多源码中总结的Utils很好用, 多学习一下


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




## XIII. MISCELLANEOUS
---

**EXCEPTION**

TryCatchFinally

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




THREAD

ThreadLocal


Thread Lock

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

