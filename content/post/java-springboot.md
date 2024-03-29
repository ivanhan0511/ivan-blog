﻿---
title: "SpringBoot"
date: 2024-01-23T10:18:00+08:00
categories:
- Java
- WebFramework
tags:
- SpringBoot
- TraditionalDesign
keywords:
- java
- springboot
- framework
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

The most important TECH cores are design and architecture which service for business, and then the second are IoC and AoP.

NO BEST, ONLY BETTER.

<!--more-->

{{< toc >}}
## 目录

- [I. ARCHITECTURE PRINCIPLE](#chapter-1)
- [II. BOOT LOADER](#chapter-2)
- [III. IMPORTANT SUPPORT](#chapter-3)
- [IV. BUSINESS DESIGN](#chapter-4)
- [V. OPERATION](#chapter-5)
- [VI. APPENDIX](#chapter-6)




## I. ARCHITECTURE PRINCIPLE {#chapter-1}
---
So far, Feb 23, 2024

1. 借鉴[ruoyi-vue-pro](https://github.com/YunaiV/ruoyi-vue-pro)

2. 如果团队比较小, 达不到上百人, 那么就不拆服务直接单体运行, 掌握设计的"度", 非常重要!!!

   除非有动态扩缩，不同的模块由不同的团队单独维护，或者模块单独卖之类的需求，否则拆服务弊大于利

3. 单仓多项目，大厂很多项目也在用这种方式. 有些人说会发展为分布式单体的，是对maven打包不大熟吧。运维角度来看，单仓和多仓git管理的打包产物都一样的，只是管理方式不一样

   有些"伪微服务"项目，想跑起来就得clone一堆仓库，动不动出问题就是某个仓库更新了而关联模块的仓库没更新……

3. 另外, git自带子模块, 建一个总的仓库, 各个服务作为子模块版本各自管理, 也是一种兼容方案

5. 不参与DDD(领域驱动设计)!!!




## II. BOOT LOADER {#chapter-2}
---
弄懂maven加载,
IoC
AoP
Exception
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


### Multiple modules
参考[这篇文章](https://www.cnblogs.com/yangyongjie/p/16895043.html)来组织多模块


### Beans注册, 启动顺序等




## III. IMPORTANT SUPPORT {#chapter-3}
---

### Security
TODO
### mall
- [ ] admin与security解耦, 不同客户端的认证拦截过滤
  - [ ] 用户名密码新登录, 与token登录 都用一个`jwtAuthenticationTokenFilter`, 通过`UserDetailsService`获取用户数据
    + [ ] 通过获取username最终实现定位user
    + [ ] 该token体系与微信认证是否能兼容, 如何兼容?
  - [ ] 动态权限过滤, 使用`dynamicSecurityFilter`, 通过`DynamicSecurityMetadataService`获取用户权限


### Configure
这里包含初始化, 注册哪些filter


### Filter
具体拦截, 并把相对应的且support()的provider传进去


### Provider
提供自己的认证比对实现方法, 并且实现`support()`满足filter查找


### Handler
增加成功与失败的handler


### Miscellaneous
- 还涉及到与微信服务器交换opencode, 交换session, 再让用户授权手机号并最终拿到OpenId 等一系列动作
- 还要设置缓存, 清空与微信服务器交换时中间状态的cache

UserDetails还是个坑, 不适合目前这个项目




## IV. BUSINESS DESIGN {#chapter-4}
---
@RequestBody MultipartFile[] submissions
should be

@RequestParam("file") MultipartFile[] submissions
The files are not the request body, they are part of it and there is no built-in HttpMessageConverter that can convert the request to an array of MultiPartFile.

You can also replace HttpServletRequest with MultipartHttpServletRequest, which gives you access to the headers of the individual parts.

### Controller and input validation
- [ ] Web admin
- [ ] App portal
- [x] Normal `@Valid` and `@Validated`
- [x] DIY `@FlagValidator`




### Service
- [ ] Use MaBatisPlus? How does `mall` use pure MaBatis easily to implement?
- [ ] Simple service, call mapper function directly?
- [ ] Complicate service, call multiple mapper functions?
- [ ] What is the data structure(DTO?) in nternal service?




### DAO Mapper
- [ ] Pure MyBatis or MyBatisPlus?
- [ ] MyBatis PageHelper detail
- [ ] org.springframework.data.domain.PageRequest? Or PearAdmin? Or mall?

**JUST** use MyBatisPLus maven and use default CRUD methods.  
But **REFUSE** to use `QueryWrapper`, use MyBatis' XML mapper.

Reason as below:
- IService和BaseMapper中"充分利用"了Java新规定中Interface可以有default的用法, 基本的CRUD确实不用重复写了
- MybatisPlus的QueryWrapper越看越像Python SQLAlchemy这类ORM, 学习成本高, 且一旦语句优化不得当, 会造成性能损失
- 性能和稳定性等不稳定因素越来越多, 比如一旦手动干预, 很多所谓的"便利"瞬间全无, 还是得依靠MyBatis
- 按照MyBatis写XML更像原生SQL, 熟练运用SQL是一件愉快的事情

**非**分布式数据库, 不推荐使用雪花算法, 使用数据库自增ID即可


#### Mapper Design
- 因功能不同而查询同一个表, 很可能过滤条件维度和结果维度都不相同, 因此按照功能划分查询列表数据或查询单条数据比较合理(暂时, May 30, 2023)
  - selectOneForXxxFunction(Integer param)
  - selectListForYyyFunction(YyyRequest request)
    - request中通过分组校验参数来整合不同的请求参数, 以便Mapper层可以一条语句应对多个controller层不同条件的查询
- 复用`<sql/>`仅限于数据库表的columns字段
  - 为了在`<select/>`中看得更清晰
  - 为了MyBatisCodeHelperPro这个插件能关联`<sql/>`和`<resultMap/>`对应的字段
- 不使用联合查询, 会导致PageHelper无法正确分页, 而使用子查询


### Multiple DB
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


### Transactional
- 跨多数据源, 调用各个资源服务, 各个Service的实现类有@DS("customer")注解指定数据源
- 事务管理, 通过@DSTransactional 根据各个服务所指定的数据源进行切换

#### Propagation
TODO

- 事务传播, 需要时则加入传播的扩散要求  
  @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)


#### Isolation
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




### Background Job
#### Quartz





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




## VI. APPENDIX {#chapter-6}
---
摘抄自网络, 不一定合理, 仅保存一些文字, 以便以后少写一些文字


### 缩写信息
- DAO: Data Access Object, 数据访问层
- PO: Persistant Object, 持久层对象. 类似数据库内的一条记录
- DO: Domain Object, 领域对象 (不参与)
- DTO: Data Transfer Object, 通常在OpenApi返回的对象中使用DTO, 忠于表结构原始数值{"age":40}
- BO: Business Object, 业务对象(暂且不用)
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


