---
title: "SpringBoot"
date: 2023-10-26T10:15:00+08:00
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

- 个人小程序, 以mall上线一个测试版
- 个人小程序, 再自己创建一个项目, 模拟一个真是项目




## PROJECT ARCHITECTURE
---
1. 如果项目功能足够简单, 项目比较小的话, 其实没有必要分的那么细致. 掌握设计的"度", 非常重要!!!
2. 不参与DDD(领域驱动设计)!!!

{{< alert danger >}}
DAO层
- Java代码的PO, 纯粹应对数据库结构
- 优先使用数据库关联查询, 减少连接次数
  - 做好通用抽象
  - 划分简单数据输出, 与复杂关联表格的整体输出, 都是为了服务内部传输数据
- 一对一 / 一对多 通过MyBatis的resultMap子表查询(需要DTO封装), 合理使用子表查询, eager/lazy
{{< /alert >}}


{{< alert warning >}}
DTO层
- 核心要考虑和设计的内容
  - 从业务接口着手, 脑图中列举出所有相关字段, 结合原型图, 设计前后端接口(未完)
  - 通过业务流接口, 分解模块结构, 形成数据结构
  - 以架构的合理性, 业务流的闭环, 评审业务流与数据结构是否合理
{{< /alert >}}


{{< alert success >}}
Service层
- 面对DAO, 结合MyBatisPlus(适度使用), 提供很多小而单一的服务, 以便解耦
- 面对Controller, 从模块角度考虑综合服务所应该提供的综合服务, 调用多个小服务
- 服务端要兼容多种客户端的数据交互行为, 涉及UserContext的区分等
- 服务层使用DTO通信
{{< /alert >}}


{{< alert info >}}
Controller层
- 从业务场景考虑接口设计
- 分流: 简单判断分流前端业务, 因地制宜地回复错误信息
- 输入:
  - 校验前端数据, 结合`@Validate`进行分组校验
- 输出:
  - 简单业务, *恰当地*在DTO类中依靠`@JsonView` / `@JsonIgnore`来进行简单的过滤
  - 综合业务, 根据不同的客户端的访问, 需要重新组装VO, 以便返回给前端
{{< /alert >}}




## SECURITY
---
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

### UserDetails还是个坑, 不适合目前这个项目




## CONTROLLER AND INPUT VALIDATION
---
- [ ] Web admin
- [ ] App portal
- [x] Normal `@Valid` and `@Validated`
- [x] DIY `@FlagValidator`




## SERVICE
---
- [ ] Use MaBatisPlus? How does `mall` use pure MaBatis easily to implement?
- [ ] Simple service, call mapper function directly?
- [ ] Complicate service, call multiple mapper functions?
- [ ] What is the data structure(DTO?) in nternal service?




## DAO MAPPER
---
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


### Mapper Design
- 因功能不同而查询同一个表, 很可能过滤条件维度和结果维度都不相同, 因此按照功能划分查询列表数据或查询单条数据比较合理(暂时, May 30, 2023)
  - selectOneForXxxFunction(Integer param)
  - selectListForYyyFunction(YyyRequest request)
    - request中通过分组校验参数来整合不同的请求参数, 以便Mapper层可以一条语句应对多个controller层不同条件的查询
- 复用`<sql/>`仅限于数据库表的columns字段
  - 为了在`<select/>`中看得更清晰
  - 为了MyBatisCodeHelperPro这个插件能关联`<sql/>`和`<resultMap/>`对应的字段
- 不使用联合查询, 会导致PageHelper无法正确分页, 而使用子查询


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




## Output Convert (No BeanUtil.copy)
---
- [ ] If simple, use DTO to response directly
- [ ] If complicated, transfer DTO to VO




## GLOBAL COMMON MODULE
---
- [x] CommonResult
- [ ] Exception
- [ ] Log
- [ ] Intercepter



### LOGGER
**slf4j.Logger and log4j.Logger**
{{< blockquote "LEARN SLF4J" "https://www.tutorialspoint.com/slf4j/slf4j_vs_log4j.htm#:~:text=Comparison%20SLF4J%20and%20Log4j,prefer%20one%20between%20the%20two." "SLF4J Vs Log4j">}}
Comparison SLF4J and Log4j<br/>

Unlike log4j, SLF4J (Simple Logging Facade for Java) is not an implementation of logging framework, 
it is an abstraction for all those logging frameworks in Java similar to log4J. Therefore, you cannot compare both. 
However, it is always difficult to prefer one between the two.
{{< /blockquote >}}

{{< image classes="fancybox fig-100" src="https://www.tutorialspoint.com/slf4j/images/application.jpg" thumbnail="https://www.tutorialspoint.com/slf4j/images/application.jpg" >}}




## CONNECTION POOL
---
Druid or Hikari -> PearAdminPro用的是Hikari, 也是Springboot官方选用的

Druid是淘宝选用的, 高并发的情况会适用一些





## CACHE
---
- [ ] Redis
- [ ] `@CacheException`
- MyBatis缓存




## BACKGROUND JOB
---



## API DOCUMENT
---
- [ ] Swagger UI category, description and list
- [ ] With session token




## DEPLOYMENT
---
- [ ] DockerCompose deployment in single server?




## OTHERS
---
- [ ] ElasticSearch NativeSearchQueryBuilder




## TESTING
---
- 完成以上考察, 大范围API测试




## OPERATION
---
- 个人订阅号, 熟悉内容的编辑/发布, 练习写作, 提高排版/拍照等后期技能
- 运营, 尝试推送文章




## APPENDIX
---
摘抄自网络, 不一定合理, 仅保存一些文字, 以便以后少写一些文字


### 缩写信息
- DAO: Data Access Object, 数据访问层
- PO: Persistant Object, 持久层对象. 类似数据库内的一条记录
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
