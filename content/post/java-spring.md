---
title: "Spring"
date: 2023-06-19T10:14:00+08:00
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

JetBrains IDEA 中如果想了解某个注解的实现, `Ctrl + Shift + F`(记得关闭输入法简体/繁体热键), 输入注解名称.java

## IOC
---
### Bean生命周期
[TODO]: 插入图片 "备注"




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




## MVC事务
