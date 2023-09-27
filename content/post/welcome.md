---
title: "Welcome to Ivan's Blog"
date: 2023-09-21T11:20:00+08:00
categories:
- Welcome
tags:
- Welcome
- Ivan Han
keywords:
- welcome
- ivan-blog
clearReading: true
thumbnailImage: //example.com/static/A.png
thumbnailImagePosition: top
autoThumbnailImage: false
metaAlignment: center
coverMeta: in
coverImage: //example.com/static/B.png
coverCaption: "A Beautifull Cover Image"
coverSize: full
comments: false
showTags: true
showPagination: true
showSocial: false
showDate: true
---

Some records for software, coffee, music and sports.
<!--more-->

{{< blockquote "稻盛和夫" >}}
"总有一天你会明白, 答非所问就是回答, 敬而远之就是不喜欢， 沉默不语就是拒绝， 闪烁不定就是撒谎， 冷战就是不怕失去. 有些事情真的没必要追问, 回头看看， 所有的细节都是答案."
{{< /blockquote >}}




## BLOG
- [ ] Take photos for all posts
- [ ] Write content for all posts
- [ ] Make my posts can be found on internet




## DOMAIN AND PROJECTS
---
MySQL + Redis + Springboot + Java Security + Vue3 + UniApp + WeChat Authentication一套框架

- 个人小程序, 以mall上线一个Demo
- 个人小程序, 再以mall模拟一个简化的德海项目/帐篷预定项目

### Input Validation
- [x] Normal `@Valid` and `@Validated`
- [x] DIY `@FlagValidator`


### Spring Security
- [ ] admin与security解耦, 不同客户端的认证拦截过滤
  - [ ] 用户名密码新登录, 与token登录 都用一个`jwtAuthenticationTokenFilter`, 通过`UserDetailsService`获取用户数据
    + [ ] 通过获取username最终实现定位user
    + [ ] 该token体系与微信认证是否能兼容, 如何兼容?
  - [ ] 动态权限过滤, 使用`dynamicSecurityFilter`, 通过`DynamicSecurityMetadataService`获取用户权限


### Controller
- [ ] Web admin
- [ ] App portal


### Service
- [ ] Simple service, call mapper function directly?
- [ ] Complicate service, call multiple mapper functions?


### DAO Mapper
- [x] Pure MyBatis
- [ ] MyBatis PageHelper
- [ ] org.springframework.data.domain.PageRequest? Or PearAdmin? Or mall?


### Output Convert (No BeanUtil.copy)
- [ ] Response structure


### Global exception catch
- [ ] Exception
- [ ] Log


### Cache
- [ ] Redis
- [ ] `@CacheException`


### Background Job



### API Document
- [ ] SwaggerUI


### Deployment
- [ ] DockerCompose deployment?


### Others
- [ ] ElasticSearch NativeSearchQueryBuilder


- 完成以上考察, 大范围API测试
- 最后把mall研究透彻, 并制定商城需求, 改造为可用的BtoB平台, 以及BtoC商城
- 个人订阅号, 熟悉内容的编辑/发布, 练习写作, 提高排版/拍照等后期技能
- 运营, 尝试推送文章

