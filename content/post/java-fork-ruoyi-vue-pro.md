---
title: "How to fork ruoyi-vue-pro"
date: 2024-09-29T14:43:47+08:00
categories:
- Java
- WebFramework
tags:
- Java
- Springboot
- ruoyi-vue-pro
keywords:
- Java
- spingboot
- ruoyi-vue-pro
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

To deploy a Springboot web framework, whatever development or production, and whatever single or distributed entity, 
[ruoyi-vue-pro](https://github.com/YunaiV/ruoyi-vue-pro) is a good project

But the stronger, the more complex. This post is the annotation for this project, by my personer opinion

<!--more-->

{{< toc >}}

The most important TECH cores are design and architecture which service for business, and then the second is tech like IoC, AoP and so on.

NO BEST, ONLY BETTER.


## 目录

- [I. ARCHITECTURE PRINCIPLE](#chapter-1)
- [II. BUSINESS DESIGN](#chapter-2)
- [III. IMPORTANT SUPPORT](#chapter-3)
- [V. OPERATION](#chapter-5)
- [VI. APPENDIX](#chapter-6)


[TODO]: 整理
### Connection Pool
Druid or Hikari -> PearAdminPro用的是Hikari, 也是Springboot官方选用的
Druid是淘宝选用的, 高并发的情况会适用一些
### Cache
### Redis
[参考该文章](https://javaguide.cn/database/redis/redis-data-structures-01.html#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF-1)
### Annotation
- [ ] `@CacheException`
### MyBatis Cache
MyBatis的一级缓存和二级缓存, 都存在可能脏读的情况, 所以一般惯用Redis做缓存
引入Redis后只需要将MyBatis配置文件中Cache 的类型定义为RedisCache
Log
### Logger
**slf4j.Logger and log4j.Logger**
{{< blockquote "LEARN SLF4J" "https://www.tutorialspoint.com/slf4j/slf4j_vs_log4j.htm#:~:text=Comparison%20SLF4J%20and%20Log4j,prefer%20one%20between%20the%20two." "SLF4J Vs Log4j">}}
Comparison SLF4J and Log4j<br/>

Unlike log4j, SLF4J (Simple Logging Facade for Java) is not an implementation of logging framework, 
it is an abstraction for all those logging frameworks in Java similar to log4J. Therefore, you cannot compare both. 
However, it is always difficult to prefer one between the two.
{{< /blockquote >}}

{{< image classes="fancybox fig-100" src="https://www.tutorialspoint.com/slf4j/images/application.jpg" thumbnail="https://www.tutorialspoint.com/slf4j/images/application.jpg" >}}
CommonResult
Redis
cache

### Beans注册, 启动顺序等



## I. ARCHITECTURE PRINCIPLE {#chapter-1}
---
So far, Sep 24, 2024
1. Reading the [docs](https://github.com/YunaiV/ruoyi-vue-pro) of ruoyi-vue-pro is the best way  

2. 如果团队比较小, 达不到上百人, 那么就不拆服务直接单体运行, 掌握设计的"度", 非常重要!!!  
   除非有动态扩缩，不同的模块由不同的团队单独维护，或者模块单独卖之类的需求，否则拆服务弊大于利

3. 单仓多项目，大厂很多项目也在用这种方式. 有些人说会发展为分布式单体的，是对maven打包不大熟吧。运维角度来看，单仓和多仓git管理的打包产物都一样的，只是管理方式不一样

   有些"伪微服务"项目，想跑起来就得clone一堆仓库，动不动出问题就是某个仓库更新了而关联模块的仓库没更新……

3. 另外, git自带子模块, 建一个总的仓库, 各个服务作为子模块版本各自管理, 也是一种兼容方案

5. 尽管代码中有DO, 但并不采用DDD(领域驱动设计)!!!




## II. BUSINESS DESIGN {#chapter-2}
---
1. Write down all the details of requirement seriously

2. Draw UML(work flow)

3. Designing the data structure and its relationship on the paper / iPad is the most important thing!

4. Record the SQL into the `./sql/mysql/`, of course, my personal choice

With the infra code-auto-generation, most backend and frontend codes can be generated. Very helpful and so handy




放在第2章节, 这主要是关于日常开发的中一些业务结构该如何设计才最优雅, 没有底层知识
### A. Controller

#### 1. Input validation

**NOT** the RESTful style

Btw, considering for the conveniece of frontend, some
Good place to convert DO/DTO to VO. Especially with java.stream


#### 2. Outnput convertor
Like xxxPageRespVO, output the userId and userName at the same time, because it's bad to fetch each userName in the frontend table

Like xxxRespVO, whether to output either the userId or userName at the same time, depends on the business. For this single object line data, frontend could fetch 
its detail by id easily

{{< codeblock java >}}
@Schema(description = "管理后台 - 商品 SPU Response VO")
@Data
@ExcelIgnoreUnannotated
public class ProductSpuRespVO {
    @Schema(description = "商品状态", requiredMode = Schema.RequiredMode.REQUIRED, example = "1")
    @ExcelProperty(value = "商品状态", converter = DictConvert.class)
    @DictFormat(DictTypeConstants.PRODUCT_SPU_STATUS)
    private Integer status;
}
{{< /codeblock >}}
TODO: 这段注解的机制, 参考java-spring.md文章



### B. Service
Just work for **BUSINESS** only

基本遵循源码自动生成的代码, 在良好的数据结构设计的基础上, 很多问题都能迎刃而解. 需要额外定制的几个参考用例, 如后文


#### 1. Transactional
[TODO]: 继续整理
- 跨多数据源, 调用各个资源服务, 各个Service的实现类有@DS("customer")注解指定数据源
- 事务管理, 通过@DSTransactional 根据各个服务所指定的数据源进行切换

**Propagation**
TODO

- 事务传播, 需要时则加入传播的扩散要求  
  @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)


**Isolation**
不同于"传播规则"是Spring提到的概念, 隔离级别这个概念是数据库事务自带的

| Isolation Level  | 脏读(Dirty Read) | 不可重复读(Non Repeatable Read) | 幻读(Phantom Read) |
| :---             | :---            | :---                           | :---              |
| Read Uncommitted | Yes             | Yes                            | Yes               |
| Read Committed   | -               | Yes                            | Yes               |
| Repeatable Read  | -               | -                              | Yes               |
| Serializable     | -               | -                              | -                 |

很多人容易搞混不可重复读和幻读，确实这两者有些相似. 但不可重复读重点在于update和delete, 而幻读的重点在于insert.

总的来说幻读就是事务A对数据进行操作，事务B还是可以用insert插入数据的，因为使用的是行锁，这样导致的各种奇葩问题就是幻读，表现形式很多，就不列举了。


**TIPs**
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




#### 2. Job
Quartz



#### 3. Third-part API

- RestTemplate: Sync
- WebClient: Async, since Sping 5.0


### C. DAO Mapper

{{< blockquote >}}
**JUST** use MyBatisPLus maven and use default CRUD methods.  
But **REFUSE** to use `QueryWrapper`, use MyBatis' XML mapper.

Reason as below:
- IService和BaseMapper中"充分利用"了Java新规定中Interface可以有default的用法, 基本的CRUD确实不用重复写了
- MybatisPlus的QueryWrapper越看越像Python SQLAlchemy这类ORM, 学习成本高, 且一旦语句优化不得当, 会造成性能损失
- 性能和稳定性等不稳定因素越来越多, 比如一旦手动干预, 很多所谓的"便利"瞬间全无, 还是得依靠MyBatis
- 按照MyBatis写XML更像原生SQL, 熟练运用SQL是一件愉快的事情
{{< /blockquote >}}

**非**分布式数据库, 不推荐使用雪花算法, 使用数据库自增ID即可, 涉及到分布式了再说



#### Multiple DB


**Settings**
- application.yml中设置PageHelper的 helperDialect, 兼容"mysql"和"sqlserver"两种数据库的语法

- 使用多数据源时, 配置PageHelper时要注意(只有在使用application.yml格式的配置文件时会有问题):  
  {{< blockquote "application.yml" >}}
  ...
  pagehelper:
    autoRuntimeDialect: true  # 此处的配置项是驼峰, 不是IDEA自动提示的`auto-runtime-dialect: true`
  ...
  {{< /blockquote >}}
  因为如果驼峰被自动转译为横线分隔符, 会导致PageHelper切换多数据源时失效







## V. OPERATION {#chapter-5}
---
运营, 软文推广, 运维, 部署, 自动化测试, 自动化接口文档等

### CI

### Monitor


### API DOCUMENT
- [ ] Swagger UI category, description and list
- [ ] With session token




### DEPLOYMENT
#### Docker
- [ ] DockerCompose deployment in single server?


#### JAR




### Others
- 个人订阅号, 熟悉内容的编辑/发布, 练习写作, 提高排版/拍照等后期技能
- 运营, 尝试推送文章




## III. IMPORTANT SUPPORT {#chapter-3}
---

### A. Security
**Configure**
这里包含初始化, 注册哪些filter


**Filter**
具体拦截, 并把相对应的且support()的provider传进去


**Provider**
提供自己的认证比对实现方法, 并且实现`support()`满足filter查找


**Handler**
增加成功与失败的handler

#### Permission

#### Protection



### B. Input
@RequestParam("file") MultipartFile[] submissions
The files are not the request body, they are part of it and there is no built-in HttpMessageConverter that can convert the request to an array of MultiPartFile.

You can also replace HttpServletRequest with MultipartHttpServletRequest, which gives you access to the headers of the individual parts.


### C. Output


### D. Exception


### E. API


### F. Log

### G. Background Job

### H. Cache


## VI. APPENDIX {#chapter-6}
---
摘抄自网络, 不一定合理, 仅保存一些文字, 以便以后少写一些文字


### 缩写信息
- DAO: Data Access Object, 数据访问层
- PO: Persistant Object, 持久层对象. 类似数据库内的一条记录
- DO: Domain Object, 领域对象
- DTO: Data Transfer Object, 通常在OpenApi返回的对象中使用DTO, 忠于表结构原始数值{"age":40}
- BO: Business Object, 业务对象
- VO: Value Object, 表现对象 (如果需要, 则转换为具体表现的内容, 例如{"age":40, "desc":"不惑之年"})
- POJO: Plain Old Java Object, 是PO/DO/DTO/BO/VO的统称


### 命名规范
- Service/DAO层方法命名规约
  + 获取单个对象的方法用get做前缀
  + 获取多个对象的方法用list做前缀
  + 获取统计值的方法用count做前缀
  + 插入的方法用save/insert做前缀
  + 删除的方法用remove/delete做前缀
  + 修改的方法用edit/update做前缀
- ~~领域模型命名规约~~
  + 数据对象: xxxDO, xxx即表名
  + 数据传输对象: xxxDTO, xxx即业务领域相关的名称
  + 展示对象: xxxVO, xxx一般为网页名称
  + POJO是DO/DTO/BO/VO的统称, 禁止命名成xxxPOJO



## 笔记

Sep 29, 2024

有点理解, 为啥代码中Enum中有各种状态, 而Dict中又有. 其实有一部分字典数据, 是为了前端而提供的, 这样前端后端就不用各自存储各个字段数值代表什么意思了


test utf-8
