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


## 目录

- [I. IOC](#chapter-1)
- [II. AOP](#chapter-2)
- [III. SERVLET](#chapter-3)
- [IV. SECURITY](#chapter-4)
- [V. TRANSACTIONAL](#chapter-5)
- [VI. JOB](#chapter-6)
- [VII. THIRD-PART HTTP API](#chapter-7)
- [VIII. EVENT MONITOR](#chapter-8)
- [IX. DAO MAPPER](#chapter-9)
- [X. UTILS](#chapter-10)
- [XI. MISCELLANEOUS](#chapter-11)




## I. IOC {#chapter-1}
---

### A. LifeCircle-@Configuration

https://www.cnblogs.com/java-chen-hao/p/11640177.html




## II. AOP {#chapter-2}
---
JetBrains IDEA 中如果想了解某个注解的实现, 没有太好的办法, 就像想查找一个类在哪里被反射并调用了, 试行的办法:
- 阅读大厂的源码, 文档, 了解该注解的内部实现逻辑
- 小厂的代码, 只能是全文查找类似这样的`getAnnotation(DictFormat.class)`代码


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
- 服务端使用   Content-Type:"application/json"编码http响应body内容返回给前端；前端使用Content-Type:"application/json"解码http响应body内容。<br>
- 服务端返回   Response Content-Type:application/json，前端dataType 不指定值 "json". 此时，解码http响应body内容，   data类型是Object。<br>
- 服务端不返回 Response Content-Type:application/json，前端dataType 指定值   "json". 些时，解码http响应body内容，   data类型是Object。<br>
- 服务端不返回 Response Content-Type:application/json，前端dataType 不指定值 "json". 此时，不能解码http响应body内容，data类型是String。<br>
{{< /blockquote >}}




### 易混淆注解的对比

#### @Component @Bean @Service @Configurable
With this annotation, a class can be scaned manually or automaticlly

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




## III. SERVLET {#chapter-3}
---



## IV. SECURITY {#chapter-4}
---
**Configure**
这里包含初始化, 注册哪些filter


**Filter**
具体拦截, 并把相对应的且support()的provider传进去


**Provider**
提供自己的认证比对实现方法, 并且实现`support()`满足filter查找


**Handler**
增加成功与失败的handler




## V. TRANSACTIONAL {#chapter-5}
---

### A. Propagation

- 事务传播, 需要时则加入传播的扩散要求  
  @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)


### B. Isolation

以下是 MySQL 事务隔离级别的简要场景及效果区别：

#### 1. Read Uncommitted
**场景**: 允许读取未提交的数据。  
**效果**:  
- **脏读**: 可能发生，读取其他事务未提交的数据。  
- **不可重复读**: 可能发生，因为事务间可能修改已读取数据。  
- **幻读**: 可能发生，因为可能插入新数据。  

### 2. Read Committed  
**场景**: 只读取已提交的数据，避免读取未提交事务的内容。  
**效果**:  
- **脏读**: 不会发生，保证读取的是已提交数据。  
- **不可重复读**: 可能发生，因为同一事务中重复读取时，数据可能被其他事务修改。  
- **幻读**: 可能发生，因为其他事务可能插入新记录。  

**Tips**

不可重复读重点在于update和delete


#### 3. Repeatable Read (MySQL默认)
**场景**: 保证同一事务中，读取的结果是固定的，即便其他事务修改了数据。  
**效果**:  
- **脏读**: 不会发生。  
- **不可重复读**: 不会发生，事务中数据一致。  
- **幻读**: 通过**间隙锁**避免，查询范围内的数据是固定的。  

**Tips**

该级别产生幻读的重点场景在于insert, 总的来说就是事务A对数据进行操作，事务B还是可以用insert插入数据的，因为使用的是行锁，这样导致的各种奇葩问题就是幻读，表现形式很多
TODO: MySQL的间隙锁在开源代码中是否已经默认被应用


#### 4. Serializable
**场景**: 强制事务串行化，确保事务间完全隔离。  
**效果**:  
- **脏读**: 不会发生。  
- **不可重复读**: 不会发生。  
- **幻读**: 不会发生，因为会对范围内的操作加锁，阻止插入或修改。  

#### 总结表格

| Isolation Level  | 脏读(Dirty Read) | 不可重复读(Non Repeatable Read) | 幻读(Phantom Read) |
|------------------|------|------------|-------|
| Read Uncommitted | ✅   | ✅         | ✅    |
| Read Committed   | ❌   | ✅         | ✅    |
| Repeatable Read  | ❌   | ❌         | ❌    |
| Serializable     | ❌   | ❌         | ❌    |


### C. 多数据源

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



## VI. JOB {#chapter-6}
---
Quartz



## VII. THIRD-PART HTTP API {#chapter-7}
---
RestTemplate 是从Spring3.0开始支持的一个远程HTTP请求工具，RestTemplate提供了常见的Rest服务(Rest风格、Rest架构)的客户端请求的模版，能够大大提高客户端的编写效率

多年来，Spring框架的RestTemplate一直是客户端HTTP访问的首选解决方案，它提供了同步、阻塞API以简洁的方式处理HTTP请求。然而，随着对非阻塞、反应式编程以更少的资源处理并发的需求不断增加，特别是在微服务架构中，RestTemplate已经显露出其局限性。
从Spring Framework 5开始，RestTemplate已被标记为已弃用，Spring团队推荐WebClient作为其继任者。在这篇文章中，我们将通过实际示例深入探讨RestTemplate被弃用的原因、采用WebClient的优势以及如何有效过渡。

{{< blockquote >}}
Synchronous client to perform HTTP requests, exposing a simple, template method API over underlying HTTP client libraries such as the JDK HttpURLConnection.

NOTE: As of 6.1, RestClient offers a more modern API for synchronous HTTP access. For asynchronous and streaming scenarios, consider the reactive org. springframework. web. reactive. function. client. WebClient.
{{< /blockquote >}}




## VIII. EVENT MONITOR {#chapter-8}
---
ApplicationEvent Monitor
module-bpm模块中有用到, 拿来主义, 上游入库后"生产"出下游库存就依靠这个监听者观察客户定制的BOM原料是否满足, 满足即可生产
[TODO]:




## IX. DAO MAPPER {#chapter-9}
---




## X. UTILS {#chapter-10}
---




## XI. MISCELLANEOUS {#chapter-11}
---

