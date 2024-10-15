---
title: "Spring"
date: 2023-10-26T10:14:00+08:00
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

JetBrains IDEA 中如果想了解某个注解的实现, 没有太好的办法, 就像想查找一个类在哪里被反射并调用了, 试行的办法:
- 阅读大厂的源码, 文档, 了解该注解的内部实现逻辑
- 小厂的代码, 只能是全文查找类似这样的`getAnnotation(DictFormat.class)`代码



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
















## IOC
---
### Bean生命周期
[TODO]: 插入图片 "备注", 用自己的域名及云服务器做图床




## AOP
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

#### @Component @Bean @Service @Configurable
With this annotation, a class can be scaned manually or automaticlly

`@Bean`一般是调用第三方的

`@Component`与`@Service`没有区别, `@Service`还没想好, 是一个预留的注解定义

`@Configurable`是单例模式, 在启动时加载




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




## SERVLET
---




## MVC事务
---
