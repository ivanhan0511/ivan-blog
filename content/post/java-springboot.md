---
title: "SpringBoot"
date: 2023-02-24T14:10:01+08:00
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


It could be a productional framework to make a software project stronger.


<!--more-->

{{< toc >}}

## CORE
To be grabbed deeply. IoC and AoP


Transactional传播生效的设置与Bean和代理有关

@DS()源于Mybatis-Plus



## PROJECT ARCHITECTURE

### Architecture
{{< alert info >}}
Controller  
- 简单分流前端业务, 因地制宜地回复错误信息
{{< /alert >}}

{{< alert success >}}
Service  
- 只管业务流的处理, 不负责数据库查询
- 可以调用其它Service, 也可以调用其它Repository
- DTO的数据转换(暂且)也在Service层处理
{{< /alert >}}

{{< alert warning >}}
DAO / Repository  
- MyBatis 接口与XML查询
- 能通过数据库查询的尽量通过数据库直接查询
{{< /alert >}}

{{< alert danger >}}
DB / Domain  
- 数据库表关联关系设计, 与Domain联动
{{< /alert >}}


### 名词解释
[项目开发中，真的有必要定义VO，BO，PO，DO，DTO这些吗？](https://blog.51cto.com/u_12302929/4811425)

自上而下的顺序:

VO: Value Object. 表现对象. 主要用于与前端直接的交互与信息传递(小规模项目, 使用DTO即可).

**DTO: Data Transfer Object. 通常是在OpenApi. 即此项目与其他外界项目交互时使用的对象.**

BO: Business Object. 业务对象(个人理解, BO和DO很像, 是一个综合多个PO的复合抽象对象).

**DO: Domain Object. 领域对象(我们在三层架构中使用的DO其实是PO)**

PO: Persistant Object. 持久层对象. 类似数据库内的一条记录

**DAO: Data Access Object.(小规模项, 使用DAO即可)**

PS: 
- POJO: Plain Old Java Object. 是PO/DO/DTO/BO/VO的统称
- 其实, 如果你的项目功能足够简单. 项目比较小的话, 其实没有必要分的那么细致. 掌握设计的“度”, 非常重要.


POJO的一些参考(阿里内部)

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




## ANNOTATION

### 常用注解的深入理解
[TODO]: To be done...

- Controller控制Headers/Content-Type/Accept
  
  [TODO]: 各种继承关系的注解, 后续补充

  - Springboot中Controller中的comsumes指定的是HTTP客户端的Content-Type, 默认application/x-www-form-urlencoded
  - Springboot中Controller中的produces指定的是HTTP客户端的Accept

http头部字段Content-Type约定请求和响应的HTTP body内容编码类型，客户端和服务端根据http头部字段Content-Type正确解码HTTP body内容。

常见的http头部Content-Type：

application/x-www-form-urlencoded
multipart/form-data
application/json
application/xml
示例说明

前端使用Content-Type:"application/json"编码http请求内容并提交给服务端；服务端使用Content-Type:"application/json"解码http请求内容。
如果不明确指定http Request头部Content-Type，将使用application/x-www-form-urlencoded; charset=UTF-8作为默认值，后端方法不能解码Content-Type:application/x-www-form-urlencoded的http Request body内容。同时也说明后端方法只能解码请求头部Content-Type为application/json的http Request body内容。

服务端使用Content-Type:"application/json"编码http响应body内容返回给前端；前端使用Content-Type:"application/json"解码http响应body内容。
服务端返回Response Content-Type:application/json，前端dataType不指定值。此时，解码http响应body内容，data类型是Object。
服务端不返回Response Content-Type:application/json，前端dataType指定值json。些时，解码http响应body内容，data类型是Object。
服务端不返回Response Content-Type:application/json，前端dataType不指定值"json"。此时，不能解码http响应body的json字符串，data类型是String。
1人点赞
网络编程


作者：番薯大佬
链接：https://www.jianshu.com/p/b7956dd875a5
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


### 易混淆注解的对比

- @Component or @Bean
With this annotation, a class can be scaned manually or automaticlly

@Bean一般是调用第三方的


- @Resource VS @Autowired

{{< blockquote "stackoverflow" "https://stackoverflow.com/questions/4093504/resource-vs-autowired" "@Resource vs @Autowired" >}}
Both @Autowired (or @Inject) and @Resource work equally well. But there is a conceptual difference or a difference in the meaning<br/><br/>
@Resource means get me a known resource by name. The name is extracted from the name of the annotated setter or field, or it is taken from the name-Parameter.<br/><br/>
@Inject or @Autowired try to wire in a suitable other component by type.<br/><br/>
So, basically these are two quite distinct concepts. Unfortunately the Spring-Implementation of @Resource has a built-in fallback, which kicks in when resolution by-name fails. In this case, it falls back to the @Autowired-kind resolution by-type.<br/>
While this fallback is convenient, IMHO it causes a lot of confusion, because people are unaware of the conceptual difference and tend to use @Resource for type-based autowiring.

{{< /blockquote >}}


- @Repository or @Mapper
DAO层的用@Repository

和@Mapper的对比还没了解到

总结
@Mapper 一定要有，否则 Mybatis 找不到 mapper。
@Repository 可有可无，可以消去依赖注入的报错信息。
@MapperScan 可以替代 @Mapper。


- SpringBoot中的@EqualsAndHashCode注解与@Data注解
https://blog.csdn.net/gdkyxy2013/article/details/104769897

- @Validate @Valid

  @Validate是org.springframework.validation.annotation.Validated 导入的

  @Valid是javax.validation.Valid 导入的

  controller类上写: @Validated
    - 如果是Bean的对象xxxRequest类限制参数, 则参数类中各自校验; 在controller类中的方法参数括号内写: @Valid
    - 如果是属性String number限制, 在controller类中的方法入参前写: @NotBlank(message = "xxx不能为空")




## DB MAPPER
Use MyBatisPLus maven and use default CRUD methods.  
But refuse to use QueryWrapper, use MyBatis' XML mapper.

[MyBatis Documents](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#Parameters)


### MultiDataSources

application.yml中设置PageHelper的 helperDialect, 兼容"mysql"和"sqlserver"两种数据库的语法


#### MultiDB & Transactional
[TODO]: 将该理论的总结归类到SQL下  

- 跨多数据源, 调用各个资源服务, 各个Service的实现类有@DS("customer")注解指定数据源
- 事务管理, 通过@DSTransactional 根据各个服务所指定的数据源进行切换
- 事务隔离(更新丢失, 脏读, 不可重复读, 幻读)
  - READ_UNCOMMITTED,它受到所有三个提到的并发副作用的影响
    + Postgres 不支持 READ_UNCOMMITTED 隔离，而是会退到 READ_COMMITED
    + Oracle 不 支持或允许 READ_UNCOMMITTED
  - READ_COMMITTED, 可防止脏读
    + Postgres、SQL Server 和 Oracle 的默认级别
  - REPEATABLE_READ, 可防止脏读和不可重复读
    + Mysql 中的默认级别
    + Oracle 不支持 REPEATABLE_READ
  - SERIALIZABLE, 最高级别的隔离

{{< alert info >}}
@DS 必须加在 @Transactional 对应的类或者方法上  
如在 mapper中加了@DS，但是 @Transactional 加在 service 方法中，此时则会获取默认的datasource
(因为在事务中已经获取了一次datasource的connection，而此时无DS注解)
{{< /alert >}}

{{< tabbed-codeblock MultiDSTransactional >}}
<!-- tab sinterface -->
public interface SomeService extends IService<Some> {}
<!-- endtab -->

<!-- tab "sImpl" -->
@Service
public class SomeServiceImpl extends ServiceImpl<SomeRepository, Some> implements SomeService {}
<!-- endtab -->

<!-- tab ointerface -->
public interface OtherService extends IService<Other> {}
<!-- endtab -->

<!-- tab oimpl -->
@Service
@DS("db2")
public class OtherServiceImpl extends ServiceImpl<OtherRepository, Other> implements OtherService {}
<!-- endtab -->

<!-- tab combo -->
public interface CombineService {
    Boolean save(CombineRequest request);
}
<!-- endtab -->

<!-- tab comboimpl -->
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


#### TODO
And how to `DynamicDataSourceContextHolder.push()` and `DynamicDataSourceContextHolder.poll()`?




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




## SECURITY
It's a deep track!

Need do really deep reseach for it.




## TIPs

### Logger

**slf4j.Logger and log4j.Logger**
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


