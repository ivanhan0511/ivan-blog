---
title: "SpringBoot"
date: 2022-09-14T17:05:59+08:00
categories:
- Java
- WebFramework
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


It could be a productional framework to make my project stronger.


<!--more-->

{{< toc >}}

## 渊源
Spring
- IOC
- AOP
自动配置
双亲委派




## CORE

### IoC

### AOP

### ARCHITECTURE

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


## Java名词理解与对应

### JavaBean与Entity很像

    Strictly speaking, @Entity is not a JavaBean (the JavaBean convention requires a public no-arg constructor, 
            @Entity can have protected etc.) but they are very similar. @Entity is actually a POJO 
    (Plain Old Java Object). You can compare conventions and requirements for JavaBeans and Entity classes:


### entity, model and domain
entity(实体)
entity的意思就是实体的意思，所以也是最常用到的，entity包中的类是必须和数据库相对应的，  
比如说：数据库有个user表，字段有long类型的id，string类型的姓名，那么entity中的user类也必须是含有这两个字段的，  
且类型必须一致。不能数据库存的是long类型，user类里的属性是string类型。这样做的好处是保持实体类和数据库保持一致，  
另外，当用到hibernate或是mybatie框架来操作数据库的时候，操作这个实体类就行，写sql文之前不需要再做数据格式处理。

model(模型)
model大家不陌生，都知道是模型的意思，当用model当包名的时候，一般里面存的是实体类的模型，一般是用来给前端用的。  
比如：前端页面需要显示一个user信息，user包含姓名，性别，居住地，这些信息存在数据库的时候，姓名直接存姓名，  
但是性别和居住地一般会用数据字典的编号存到数据库，比如：111代表男，222代表女，数据库存的就是111或222，  
如果用entity的话，把111、222前端都不知道是什么玩意，就算前端知道111代表男，222代表女，写了一个js判断数据处理。  
后来数据库变动了，111代表女，222代表男，前端的js又需要重新写，很显然这样不利于维护。  
所以就需要model来解决，后台从数据库取了数据转化为前端需要的数据直接传给前端，  
前端就不需要对数据来处理，直接显示就行了。还有一种情况，数据库里面的user表字段有十个，  
包含姓名，qq，生辰八字乱七八糟的等，但是前台页面只需要显示姓名，如果把entity全部传给前台，  
无疑传了很多没用的数据。这时候model就很好的解决了这个问题，前台需要什么数据，model就包含什么数据就行了

domain(域)
domain这个包国外很多项目经常用到，字面意思是域的意思。范围有点广了，比如一个商城的项目，  
商城主要的模块就是用户，订单，商品三大模块，那么这三块数据就可以叫做三个域，domain包里就是存的就是这些数据，  
表面上这个包和entity和model包里存的数据没什么区别，其实差别还是挺大的，特别是一些大型的项目。比如一个招聘网站的项目，  
最重要的对象就是简历了，那么简历是怎么存到数据库的呢，不可能用一张表就能存的，因为简历包含基本信息和工作经验，  
项目经验，学习经验等。基本信息可以存在简历表，但是涉及到多条的就不行，因为没人知道有多少条工作经验，项目经验，  
所以必须要单独建工作经验表和项目经验表关联到简历基本信息表。但是前台页面是不关心这些的，  
前台需要的数据就是一个简历所有信息，这时就可以用到domain来处理，domain里面的类就是一个简历对象，  
包含了简历基本信息以及list的工作经验，项目经验等。这样前端只需要获取一个对象就行了，不需要同时即要获取基本信息，  
还要从基本信息里面获取工作经验关联的简历编号，然后再去获取对应的工作经验了。  
当然，如果用model的话也是可以达到domain的效果的。这个完全是看个人喜好和项目的整体架构，  
因为创建不同的package的作用本来也就是想把项目分成不同的层，便于管理和维护。如果你乐意，你可以创建entity包，  
然后在里面存图片，创建images文件夹，里面存js。你已经看懂就行，前提是如果是团队开发的话能保证别人不打你。  
这个和语言一个道理，你在200面前和英国人说：private void set(int age),  
    人家说：滚犊子；现在你这样说，人家就知道是java语言了。能被人们通用的才叫语言，你说的别人听不懂那只能算是鸟语。  
    所以开发的时候，建类建包的命名规则规范性还是很重要的。

那么三句话总结下entity、model、domain的不同：
1.entity字段必须和数据库字段一样
2.前端需要什么我们就给什么
3.domain很少用，代表一个对象模块

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


### POJO的一些参考

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





## DB



## SECURITY



## LOG




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




## DEPLOYMENT(集成/构建)

### Maven or Gradle

The maven pom.xml defines lifecycle goals and the gradle build.gradle defines tasks.

"知识体系"中关联的关于Maven的[讲解](https://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/?self)

又看了一篇[文章](https://www.zhihu.com/question/29338218), 对于两者的取舍, 还是暂且倾向于Maven的通用, 稳定, 兼容


### Maven 3.8.1
Maven 3.8.1 blocked http connection
- Find maven path and configure it, which is embedded in IDEA, like
  `C:\Program Files\JetBrains\IntelliJ IDEA 2022.2.1\plugins\maven\lib\maven3\conf\settings.xml`

  {{< codeblock "settings.xml" >}}
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



