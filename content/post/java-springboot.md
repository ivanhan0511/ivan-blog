---
title: "SpringBoot"
date: 2023-05-16T14:10:22+08:00
categories:
- Java
- WebFramework
tags:
- SpringBoot
- DomainDesignDrive
keywords:
- java
- springboot
- framework
- domain
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

The tec core are architecture, IoC and AoP.

But just let's focus on the architecture design for business workflow in the SpringBoot framework.

No best, only better.

<!--more-->

{{< toc >}}

## PROJECT ARCHITECTURE

{{< alert danger >}}
DAO / Repository层
- MyBatis 接口与XML查询
- 能通过数据库查询的尽量通过数据库直接查询, 一对一和一对多的关联关系也通过MyBatis的resultMap来展示
- 这一层比较薄, 只做数据库的访问, **关键要做好通用抽象**
- (仅关联DO对象, DTO的转换放在serivce层, 用BeanUtil.copyProperties来快速转换, 以便从repository层拿到的数据都是全数据)
- 善用MyBatis的数据校验来做好查询/更新/插入的冗余, 解放service层
{{< /alert >}}

{{< alert warning >}}
Domain层
- 核心要考虑和设计的内容, 优先考虑架构, 模型和业务流, 其余的展现和逻辑组合, 在其它层考虑
- 淡化PO的概念, 因为每一个Domain类都能对应一个数据库表
- 一对多的关联关系, 通过List/Set类型的字段关联, **通过增加以该Domain为主语的行为方法来表达输入输出的行为**
- 一对一的关联关系, 还是仿照数据库, 设计一个xxxID的字段, 在XML中的ResultMap进行association的关联
{{< /alert >}}

{{< alert success >}}
Service层
- Domain Design Drive下的Service层也比较薄, 一般是复杂业务对多个Domain的联合封装
- 可以调用其它Service(推荐, 如果涉及到多个数据源切换时尤其重要), 也可以调用其它Repository
- 尽量少的数据规范性校验, 更多的业务数据校验
{{< /alert >}}

{{< alert info >}}
Controller层
- 从综合业务场景考虑接口设计, 调用综合业务服务
- 校验前端数据, 使用校验分组
- 简单判断分流前端业务, 因地制宜地回复错误信息
{{< /alert >}}

{{< blockquote "分层奥义" "Ivan Han">}}
其实, 如果项目功能足够简单. 项目比较小的话, 其实没有必要分的那么细致. 掌握设计的“度”, 非常重要, 相关文章有很多<br>
1. Controller入参校验, (特别简单的除外)总要有个类封装, 叫XxxDTO也行, 叫XxxRequest也行<br>
2. Controller层输出到前端, 也总要有个封装类, 叫XxxDTO, XxxVO, XxxResponse都行<br>
3. Service层, 无论是否推崇DDD驱动, 厚与薄, 都可以充分利用Domain来传递数据, 但要在返回Controller或前端之前, 做数据转换, (尽量)不要将DO暴露给Controller<br>
4. Domain层, 无论是否推崇DDD驱动, 贫血与充血, 只要设计得当, 都可以兼容PO<br>
5. DAO层, 也利用Domain来封装SQL查询结果即可
{{< /blockquote >}}





## ANNOTATION
---

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
- 服务端使用Content-Type:"application/json"编码http响应body内容返回给前端；前端使用Content-Type:"application/json"解码http响应body内容。<br>
- 服务端返回Response Content-Type:application/json，前端dataType不指定值。此时，解码http响应body内容，data类型是Object。<br>
- 服务端不返回Response Content-Type:application/json，前端dataType指定值json。些时，解码http响应body内容，data类型是Object。<br>
- 服务端不返回Response Content-Type:application/json，前端dataType不指定值"json"。此时，不能解码http响应body的json字符串，data类型是String。<br>
{{< /blockquote >}}


### 易混淆注解的对比

#### @Component @Bean
With this annotation, a class can be scaned manually or automaticlly

@Bean一般是调用第三方的


#### @Resource @Autowired

{{< blockquote "stackoverflow" "https://stackoverflow.com/questions/4093504/resource-vs-autowired" "@Resource vs @Autowired" >}}
Both @Autowired (or @Inject) and @Resource work equally well. But there is a conceptual difference or a difference in the meaning<br/><br/>
@Resource means get me a known resource by name. The name is extracted from the name of the annotated setter or field, or it is taken from the name-Parameter.<br/><br/>
@Inject or @Autowired try to wire in a suitable other component by type.<br/><br/>
So, basically these are two quite distinct concepts. Unfortunately the Spring-Implementation of @Resource has a built-in fallback, which kicks in when resolution by-name fails. In this case, it falls back to the @Autowired-kind resolution by-type.<br/>
While this fallback is convenient, IMHO it causes a lot of confusion, because people are unaware of the conceptual difference and tend to use @Resource for type-based autowiring.

{{< /blockquote >}}


#### @Repository @Mapper
- @Mapper 一定要有，否则 Mybatis 找不到 mapper。
- @Repository 可有可无，可以消去依赖注入的报错信息。
- @MapperScan 可以替代 @Mapper。


#### @Validate @Valid
- @Validate是org.springframework.validation.annotation.Validated 导入的
- @Validate 可以分组
- @Valid是javax.validation.Valid 导入的
- @Valid 可以递归
- controller类上写: @Validated
  - 如果是Bean的对象xxxRequest类限制参数, 则参数类中各自校验; 在controller类中的方法参数括号内写: @Valid
  - 如果是属性String number限制, 在controller类中的方法入参前写: @NotBlank(message = "xxx不能为空")

PS:

@RequestParam()不那么好用, 返回信息不友好




<br>

## DB MAPPER
---
**JUST** use MyBatisPLus maven and use default CRUD methods.  
But refuse to use QueryWrapper, use MyBatis' XML mapper.

Reason as below:
- (Consent)IService和BaseMapper中"充分利用"了Java新规定中Interface可以有default的用法, 基本的CRUD确实不用重复写了
- (Dissent)MybatisPlus的QueryWrapper越看越像Python SQLAlchemy这类ORM, 学习成本高, 且一旦语句优化不得当, 会造成性能损失
- (Dissent)性能和稳定性等不稳定因素越来越多, 比如一旦手动干预, 很多所谓的"便利"瞬间全无, 还是得依靠MyBatis
- (Consent)按照MyBatis写XML更像原生SQL, 熟练运用SQL是一件愉快的事情

**非**分布式数据库, 不使用雪花算法, 使用数据库自增ID int


### Mapper Design
- 因功能不同而查询同一个表, 很可能过滤条件维度和结果维度都不相同, 因此按照功能划分查询列表数据或查询单条数据比较合理(暂时, May 30, 2023)
  - selectOneForXxxFunction(Integer param)
  - selectListForYyyFunction(YyyRequest request)
    - request中通过分组校验参数来整合不同的请求参数, 以便Mapper层可以一条语句应对多个controller层不同条件的查询
- 复用<sql/>仅涉及columns字段
  - 为了在<select/>中看得更清晰
  - 为了MyBatisCodeHelperPro这个插件能关联<sql/>和<resultMap/>对应的字段
- 不使用联合查询, 会导致PageHelper无法正确分页
- 因为<resultMap/>的功能, SQL语句中可以/也不能 用`LEFT JOIN`, 也不用增加太多where条件
  - MyBatis会做子查询继续调用<collecttion/>中的查询语句




### MultiDB & Transactional
dehai-admin项目是使用MS SQL Server 与 MySQL 两种数据库的典型例子


#### Settings
- application.yml中设置PageHelper的 helperDialect, 兼容"mysql"和"sqlserver"两种数据库的语法

- 使用多数据源时, 配置PageHelper时要注意(只有在使用application.yml格式的配置文件时会有问题):  
  {{< blockquote "application.yml" >}}
  ...
  pagehelper:
    autoRuntimeDialect: true  # 此处的配置项是驼峰, 不是IDEA自动提示的`auto-runtime-dialect: true`
  ...
  {{< /blockquote >}}
  因为如果驼峰被自动转译为横线分隔符, 会导致PageHelper切换多数据源时失效


#### 数据隔离&事务传播
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
- 事务传播, 需要时则加入传播的扩散要求  
  @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)

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




<br>

## SECURITY
---
It's a deep track!

Need do really deep reseach for it.




## LOGGER
---
**slf4j.Logger and log4j.Logger**
{{< blockquote "LEARN SLF4J" "https://www.tutorialspoint.com/slf4j/slf4j_vs_log4j.htm#:~:text=Comparison%20SLF4J%20and%20Log4j,prefer%20one%20between%20the%20two." "SLF4J Vs Log4j">}}
Comparison SLF4J and Log4j<br/>

Unlike log4j, SLF4J (Simple Logging Facade for Java) is not an implementation of logging framework, 
it is an abstraction for all those logging frameworks in Java similar to log4J. Therefore, you cannot compare both. 
However, it is always difficult to prefer one between the two.
{{< /blockquote >}}

{{< image classes="fancybox fig-100" src="https://www.tutorialspoint.com/slf4j/images/application.jpg" thumbnail="https://www.tutorialspoint.com/slf4j/images/application.jpg" >}}




<br>

## 连接池选型
---
Druid or Hikari -> PearAdminPro用的是Hikari, 也是Springboot官方选用的

Druid是淘宝选用的, 高并发的情况会适用一些




<br>

## CACHE
---
- MyBatis缓存
- Redis缓存




<br>

## INTERCEPTER
---




<br>

## DEPLOYMENT(集成/构建)
---
### Maven or Gradle

The maven pom.xml defines lifecycle goals and the gradle build.gradle defines tasks.

"知识体系"中关联的关于Maven的[讲解](https://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/?self)

又看了一篇[文章](https://www.zhihu.com/question/29338218), 对于两者的取舍, 还是暂且倾向于Maven的通用, 稳定, 兼容


### Maven 3.8.1
Maven 3.8.1 blocked http connection
- Do NOT edit this original IDEA maven settings file
  `C:\Program Files\JetBrains\IntelliJ IDEA 2022.2.1\plugins\maven\lib\maven3\conf\settings.xml`
  {{< codeblock "settings.xml" XML >}}
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

- Find personal maven setting path in IDEA settings and DIY it `C:\Users\ivan\.m2\settings.xml`  
  (If not exists, create this file)

  {{< codeblock "settings.xml" XML >}}
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




<br>

## APPENDIX
---
### 缩写信息
- DAO: Data Access Object, 数据访问层
- PO: Persistant Object, 持久层对象. 类似数据库内的一条记录<
- DO: Domain Object, 领域对象
- DTO: Data Transfer Object, 通常在OpenApi返回的对象中使用DTO
- BO: Business Object, 业务对象
- VO: Value Object, 表现对象
- POJO: Plain Old Java Object, 是PO/DO/DTO/BO/VO的统称


### 命名规范
- Service/DAO层方法命名规约
  + 获取单个对象的方法用get做前缀
  + 获取多个对象的方法用list做前缀
  + 获取统计值的方法用count做前缀
  + 插入的方法用save/insert做前缀
  + 删除的方法用remove/delete做前缀
  + 修改的方法用edit/update做前缀
- 领域模型命名规约
  + 数据对象: xxxDO, xxx即表名
  + 数据传输对象: xxxDTO, xxx即业务领域相关的名称
  + 展示对象: xxxVO, xxx一般为网页名称
  + POJO是DO/DTO/BO/VO的统称, 禁止命名成xxxPOJO

- Request和Response对象的约定[参考]
  + 复杂对象的交互必须封装成Request 和 Response与前端进行交互


## TODO
---
全局日期转换格式兼容

