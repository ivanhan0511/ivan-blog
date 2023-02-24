---
title: "API Design"
date: 2023-02-23T12:52:00+08:00
categories:
- Miscellaneous
- API
tags:
- API
- Design
keywords:
- api
- design
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

## 登录/认证/签名
接口明确规定HEADER内容, 例如Content-Type, Accept等

- 用户ID之类 用String(Java Long 会在JavaScript中溢出)
- Double数据 用String
- Data String等 用String
- Data类 用Object
- List列表中的类型 用Object
- Boolean类 最好用0, 1

以确保加密/签名时的精度和非常规字符不会丢失/变形




## 普通业务接口

### 数据类型
Integer, Double, String, List[], Boolean等等, 均可

该怎么用怎么用
