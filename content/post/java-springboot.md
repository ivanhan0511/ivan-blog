---
title: "SpringBoot"
date: 2022-08-25T15:02:14+08:00
categories:
- Java
- Web Frame
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

This is a custom summary and does *NOT* appear in the post.
<!--more-->

{{< toc >}}


## Java名词理解与对应

DTO(Data Transfer Object) 相当于Schema
DAO(Data Access Object) 相当于CRUD
Entity 相当于数据库中的表, Flask中的Model
JavaBean与Entity很像
    Strictly speaking, @Entity is not a JavaBean (the JavaBean convention requires a public no-arg constructor, @Entity can have protected etc.) but they are very similar. @Entity is actually a POJO (Plain Old Java Object). You can compare conventions and requirements for JavaBeans and Entity classes:


## 框架内置

Spring的核心包括2个概念：控制反转(IOC)和面向切面(AOP)。

现在来总结一下，经过改变前后到底发生了什么，

上图展示的很明确，就是控制权的反转，之前主动权在业务层，每次用户提出需求业务层就需要跟着做出改变，现在我们把主动权交给了用户，它传进什么，就得到什么样的结果，这样业务代码就不用跟着改变了。

这就是IOC（控制反转）的核心思想。



controller中包含具体API





## 注解Annotation
@Value
https://www.baeldung.com/spring-value-annotation
In this article, we examined the various possibilities of using the @Value annotation with simple properties defined in the file, with system properties, and with properties calculated with SpEL expressions.

@Service
@Transactional
@Autowired




## 集成/构建

### Maven or Gradle

The maven pom.xml defines lifecycle goals and the gradle build.gradle defines tasks.

又看了一篇[文章](https://www.zhihu.com/question/29338218), 对于两者的取舍, 还是暂且倾向于Maven的通用, 稳定, 兼容

