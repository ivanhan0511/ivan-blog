---
title: "SpringBoot"
date: 2022-09-14T17:05:59+08:00
categories:
- Java
- Web Frame
tags:
- SpringBoot
keywords:
- java
- springboot
- web frame
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

I like this framework more and more.

It could be a productional framework to make my project stronger.


<!--more-->

{{< toc >}}

[知识体系](https://github.com/YunaiV/SpringBoot-Labs#spring-boot-%E4%B8%93%E6%A0%8F)

## 渊源
Spring
- IOC
- AOP
自动配置
双亲委派




## CORE

### IoC

### AOP




## ARCHITECTURE

三层架构
经典的三层架构主要是Dao/Service/Controller层这三层. 相应的, 对应这3层的对象为DO/BO/VO对象.

POJO: Plain Old Java Object. 是DO/DTO/BO/VO的统称

PO: Persistant Object. 持久层对象. 类似数据库内的一条记录

DO: Domain Object. 领域对象. 我们在三层架构中使用的DO其实是PO

BO: Business Object. 业务对象.

VO: Value Object. 表现对象. 主要用于与前端直接的交互与信息传递.

DTO: Data Transfer Object. 通常是在OpenApi. 即此项目与其他外界项目交互时使用的对象.

DAO: Data Access Object. 相当于Python的CRUD

SPI: Service Provider Interface. 常见的 SPI 有 JDBC、JNDI、JAXP 等，这些SPI的接口由核心类库提供，却由第三方实现

PS: 最后插一句. 其实, 如果你的项目功能足够简单. 项目比较小的话, 其实没有必要分的那么细致. 掌握设计的“度”, 非常重要.

阿里开发手册的部分约定
[参考]Domain对象各层命名约定:

- Service/DAO层方法命名规约
  + 获取单个对象的方法用get做前缀
  + 获取多个对象的方法用list做前缀
  + 获取统计值的方法用count做前缀
  + 插入的方法用save/insert做前缀
  + 删除的方法用remove/delete做前缀
  + 修改的方法用update做前缀
- 领域模型命名规约
  + 数据对象: xxxDO, xxx即表名
  + 数据传输对象: xxxDTO, xxx即业务领域相关的名称
  + 展示对象: xxxVO, xxx一般为网页名称
  + POJO是DO/DTO/BO/VO的统称, 禁止命名成xxxPOJO

[参考]Request和Response对象的约定

即复杂对象的交互必须封装成Request 和 Response与前端进行交互


个人理解:  
POJO中一般不允许有业务逻辑方法, 不能带有connection之类的方法


## Java名词理解与对应

JavaBean与Entity很像
    Strictly speaking, @Entity is not a JavaBean (the JavaBean convention requires a public no-arg constructor, 
            @Entity can have protected etc.) but they are very similar. @Entity is actually a POJO 
    (Plain Old Java Object). You can compare conventions and requirements for JavaBeans and Entity classes:



## ANNOTATION

### @Component or @Bean
With this annotation, a class can be scaned manually or automaticlly

@Bean一般是调用第三方的


### @Resource VS @Autowired

{{< blockquote "stackoverflow" "https://stackoverflow.com/questions/4093504/resource-vs-autowired" "@Resource vs @Autowired" >}}
Both @Autowired (or @Inject) and @Resource work equally well. But there is a conceptual difference or a difference in the meaning<br/><br/>
@Resource means get me a known resource by name. The name is extracted from the name of the annotated setter or field, or it is taken from the name-Parameter.<br/><br/>
@Inject or @Autowired try to wire in a suitable other component by type.<br/><br/>
So, basically these are two quite distinct concepts. Unfortunately the Spring-Implementation of @Resource has a built-in fallback, which kicks in when resolution by-name fails. In this case, it falls back to the @Autowired-kind resolution by-type.<br/>
While this fallback is convenient, IMHO it causes a lot of confusion, because people are unaware of the conceptual difference and tend to use @Resource for type-based autowiring.

{{< /blockquote >}}


### @Repository or @Mapper
DAO层的用@Repository

和@Mapper的对比还没了解到

总结
@Mapper 一定要有，否则 Mybatis 找不到 mapper。
@Repository 可有可无，可以消去依赖注入的报错信息。
@MapperScan 可以替代 @Mapper。




## 集成/构建

### Maven or Gradle

The maven pom.xml defines lifecycle goals and the gradle build.gradle defines tasks.

"知识体系"中关联的关于Maven的[讲解](https://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/?self)

又看了一篇[文章](https://www.zhihu.com/question/29338218), 对于两者的取舍, 还是暂且倾向于Maven的通用, 稳定, 兼容


### Maven 3.8.1
Maven 3.8.1 blocked http connection
- Find maven path, which is embedded in IDEA, like
  `C:\Program Files\JetBrains\IntelliJ IDEA 2022.2.1\plugins\maven\lib\maven3\conf`
- Edit `settings.xml`

  {{< codeblock "conf.xml" >}}
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
  
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
  ...
  {{< /codeblock >}}

- Reload pom.xml file in IDEA and automaticlly download the dependencies


## TIPs

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


### slf4j.Logger and log4j.Logger
{{< blockquote "LEARN SLF4J" "https://www.tutorialspoint.com/slf4j/slf4j_vs_log4j.htm#:~:text=Comparison%20SLF4J%20and%20Log4j,prefer%20one%20between%20the%20two." "SLF4J Vs Log4j">}}
Comparison SLF4J and Log4j<br/>

Unlike log4j, SLF4J (Simple Logging Facade for Java) is not an implementation of logging framework, 
it is an abstraction for all those logging frameworks in Java similar to log4J. Therefore, you cannot compare both. 
However, it is always difficult to prefer one between the two.
{{< /blockquote >}}

{{< image classes="fancybox fig-100" src="https://www.tutorialspoint.com/slf4j/images/application.jpg" thumbnail="https://www.tutorialspoint.com/slf4j/images/application.jpg" >}}


### application.yml
dependency
springboot自己的依赖: spring-boot-starter-xxx
第三方的以来: xxx-spring-boot-starter



