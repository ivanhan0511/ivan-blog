---
title: "How to fork ruoyi-vue-pro"
date: 2024-10-25T11:22:00+08:00
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

TIP: The most really important TECH cores, like design and architecture, service for BUSINESS.

NO BEST, ONLY BETTER.


## 目录

- [I. ARCHITECTURE PRINCIPLE](#chapter-1)
- [II. BUSINESS DESIGN](#chapter-2)
- [III. TESTING](#chapter-3)
- [IV. OPERATION](#chapter-4)
- [V. COMMON](#chapter-5)




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

**可能需要大改ERP**, 原因如下:
- 定义的产品没有原料和成品之分
- 产品的barCode给Mall使用是比较恰当的(Tip: barCode可以帮助Mall区分一部手机的颜色/内存等). 而对外的商品SPU/SKU由Mall定义
- 注意, BtoC的Mall没有客户的概念, 只有C端用户而已
- 而对于BtoB的ERP, 有客户的概念, 但却缺少商品Code的定义, 比如`hf001`, `盛趣点券`等
- 另外, 结合现有业务产品线
  - 背景:
    1. 门票类, BtoC, 不依赖上游, 平台在ERP生产入库, 供Mall使用. 而Mall中因为需要绑定虚拟身份的事情, 在Mall中做旁路
    2. 手办类, BtoC, 依赖实体上游, 却(暂时)不用在系统中体现, 优先用Mall满足需求, 有进一步需求时再启用ERP支持
    3. 充值类, BtoB, 依赖上游, 对下游提供接口, 其中比如`hf001`, `盛趣点券`等自定义码, 需要DIY
  - 统筹:
    - ERP的销售订单入口, 可以专供背景`3.`, 其余两项的入口在小程序Mall
    - ERP的销售订单入口, (暂时)不太需要在Controller中抽象通用的业务订单入口(指: 通过前端传递业务类别, 以区分调用哪种业务服务)
    - ERP的产品定义, 只需旁路增加`productNo`即可, 供下游使用, 与Mall无关
    - ERP的产品(严重)依赖其`categoryId`进行区分"原料"or"成品", 供"生产监听服务"生产/组装
    - 


### A. Controller

[TODO]: 继续描述源码在前后端结合时, 互相传递的参数以及显示字段的设计思路

#### 1. Input validation

**NOT** the RESTful style

Btw, considering for the conveniece of frontend, some
Good place to convert DO/DTO to VO. Especially with java.stream

前后端传递的都是字典值, 方便服务和数据库直接存储使用

删除? 
@RequestParam("file") MultipartFile[] submissions
The files are not the request body, they are part of it and there is no built-in HttpMessageConverter that can convert the request to an array of MultiPartFile.

You can also replace HttpServletRequest with MultipartHttpServletRequest, which gives you access to the headers of the individual parts.


#### 2. Outnput convertor
前后端传递的都是字典值, 方便服务和数据库直接存储使用

前端如何显示? 如下:
- Like xxxPageRespVO, output the userId and userName at the same time, because it's bad to fetch each userName in the frontend table

- Like xxxRespVO, whether to output either the userId or userName at the same time, depends on the business. For this single object line data, frontend could fetch 
its detail by id easily

TODO: 以下这段注解主要是为了Excel导出服务的, 参考java-spring.md文章
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




### B. Service
Just work for **BUSINESS** only

基本遵循源码自动生成的代码, 在良好的数据结构设计的基础上, 很多问题都能迎刃而解. 需要额外定制的几个参考用例, 如后文


#### 1. Transactional
这里不会讲原理, 只讲注意点, 设计思路, 详细文章见java-spring.md





#### 2. Job
Quartz



#### 3. Third-part API
以下2种都是Spring官方推荐的, 都是对HttpClient的封装, 简化使用, 提高效率

- RestTemplate: Sync, 
  {{< codeblock doc java >}}
  @Deprecated(since = "6.0", forRemoval = true)
  public void setReadTimeout(int timeout) {}
  {{< /codeblock >}}
- WebClient: Async, since Sping 5.0


#### 4. ApplicationEvent Monitor
module-bpm模块中有用到, 拿来主义, 上游入库后"生产"出下游库存就依靠这个监听者观察客户定制的BOM原料是否满足, 满足即可生产
[TODO]:




### C. DAO Mapper
以下记录暂且留存, 是SpringBoot框架一路走来的思路变化
{{< blockquote >}}
**JUST** use MyBatisPLus maven and use default CRUD methods.  
But **REFUSE** to use `QueryWrapper`, use MyBatis' XML mapper.

Reason as below:
- IService和BaseMapper中"充分利用"了Java新规定中Interface可以有default的用法, 基本的CRUD确实不用重复写了
- MybatisPlus的QueryWrapper越看越像Python SQLAlchemy这类ORM, 学习成本高, 且一旦语句优化不得当, 会造成性能损失
- 性能和稳定性等不稳定因素越来越多, 比如一旦手动干预, 很多所谓的"便利"瞬间全无, 还是得依靠MyBatis
- 按照MyBatis写XML更像原生SQL, 熟练运用SQL是一件愉快的事情

**非**分布式数据库, 不推荐使用雪花算法, 使用数据库自增ID即可, 涉及到分布式了再说
{{< /blockquote >}}



#### 1. Multiple DB


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





## III. TESTING {#chapter-3}
---



## IV. OPERATION {#chapter-4}
---
运营, 软文推广, 运维, 部署, 自动化测试, 自动化接口文档等


### CI



### API Document
- [ ] Swagger UI category, description and list
- [ ] With session token




### Deployment

#### Docker
- [ ] DockerCompose deployment in single server?


#### JAR




## V. COMMON {#chapter-5}
---
理解源码的核心设计思路, 具体技术知识见java-spring.md

### A. Exception



### B. Log
**slf4j.Logger and log4j.Logger**
{{< blockquote "LEARN SLF4J" "https://www.tutorialspoint.com/slf4j/slf4j_vs_log4j.htm#:~:text=Comparison%20SLF4J%20and%20Log4j,prefer%20one%20between%20the%20two." "SLF4J Vs Log4j">}}
Comparison SLF4J and Log4j<br/>

Unlike log4j, SLF4J (Simple Logging Facade for Java) is not an implementation of logging framework, 
it is an abstraction for all those logging frameworks in Java similar to log4J. Therefore, you cannot compare both. 
However, it is always difficult to prefer one between the two.
{{< /blockquote >}}

{{< image classes="fancybox fig-100" src="https://www.tutorialspoint.com/slf4j/images/application.jpg" thumbnail="https://www.tutorialspoint.com/slf4j/images/application.jpg" >}}


### C. Cache
Redis cache

[参考该文章](https://javaguide.cn/database/redis/redis-data-structures-01.html#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF-1)

MyBatis Cache

MyBatis的一级缓存和二级缓存, 都存在可能脏读的情况, 所以一般惯用Redis做缓存
引入Redis后只需要将MyBatis配置文件中Cache 的类型定义为RedisCache


### D. StreamUtils


### E.PayLock


### F.Tenant
MyBatisPlusX


### G. Connection Pool
Druid or Hikari -> PearAdminPro用的是Hikari, 也是Springboot官方选用的
Druid是淘宝选用的, 高并发的情况会适用一些



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



