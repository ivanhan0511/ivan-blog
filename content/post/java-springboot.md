---
title: "SpringBoot"
date: 2023-09-21T11:20:00+08:00
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

The most important tec core are design and architecture, and the second are IoC and AoP.

No best, only better.

<!--more-->

{{< toc >}}

## PROJECT ARCHITECTURE (Deprecated)

其实, 如果项目功能足够简单, 项目比较小的话, 其实没有必要分的那么细致. 掌握设计的"度", 非常重要!

{{< alert danger >}}
DAO / Repository层
- ~~(仅关联DO对象, DTO的转换放在serivce层, 用BeanUtil.copyProperties来快速转换, 以便从repository层拿到的数据都是全数据)~~
- 能通过数据库查询的尽量通过数据库直接查询, **关键要做好通用抽象**
- 一对一 / 一对多 通过MyBatis的resultMap子表查询, 合理使用子表查询, eager/lazy, 或者接口上分开查询数据详情等
{{< /alert >}}


{{< alert warning >}}
Domain层
- 不研究不参与DDD(领域驱动设计) ! ! !
- 核心要考虑和设计的内容, 优先考虑架构, 模型和业务流, 其余的展现和逻辑组合, 在其它层考虑
- 淡化PO的概念, 因为每一个Domain类都能对应一个数据库表
- 一对多的关联关系, 通过List/Set类型的字段关联, ~~通过增加以该Domain为主语的行为方法来表达输入输出的行为~~
- 一对一的关联关系, 还是仿照数据库, 设计一个xxxID的字段, 在XML中的ResultMap进行association的关联
{{< /alert >}}


{{< alert success >}}
Service层
- 不研究不参与DDD(领域驱动设计) ! ! !
- 从模块角度考虑服务所应该提供的功能
- ~~Domain Design Drive下的Service层也比较薄, 一般是复杂业务对多个Domain的联合封装~~
- 服务端要兼容多种客户端的数据交互行为, 涉及userContext的区分等
{{< /alert >}}


{{< alert info >}}
Controller层
- 从业务场景考虑接口设计
- 分流: 简单判断分流前端业务, 因地制宜地回复错误信息
- 输入:
  - 校验前端数据, 使用requestDTO配合`@Validate`分组校验
- 输出:
  - 简单业务, *恰当地*在Domain类中依靠`@JsonView` / `@JsonIgnore`来进行简单的过滤
  - 综合业务, 需要重新组装responseDTO数据
{{< /alert >}}




## DB MAPPER
---
**JUST** use MyBatisPLus maven and use default CRUD methods.  
But refuse to use QueryWrapper, use MyBatis' XML mapper.

Reason as below:
- (Consent)IService和BaseMapper中"充分利用"了Java新规定中Interface可以有default的用法, 基本的CRUD确实不用重复写了
- (Dissent)MybatisPlus的QueryWrapper越看越像Python SQLAlchemy这类ORM, 学习成本高, 且一旦语句优化不得当, 会造成性能损失
- (Dissent)性能和稳定性等不稳定因素越来越多, 比如一旦手动干预, 很多所谓的"便利"瞬间全无, 还是得依靠MyBatis
- (Consent)按照MyBatis写XML更像原生SQL, 熟练运用SQL是一件愉快的事情

**非**分布式数据库, 不推荐使用雪花算法, 使用数据库自增ID int


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




## SECURITY
---
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





## 连接池选型
---
Druid or Hikari -> PearAdminPro用的是Hikari, 也是Springboot官方选用的

Druid是淘宝选用的, 高并发的情况会适用一些





## CACHE
---
- MyBatis缓存
- Redis缓存





## INTERCEPTER
---



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


## TODO
全局日期转换格式兼容

