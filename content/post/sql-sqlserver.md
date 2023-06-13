---
title: "SQL Server Init Operations"
date: 2023-01-17T15:41:38+08:00
categories:
- SQL
- SQLServer
tags:
- SQLServer
keywords:
- sqlserver
- init
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

Some usual opearations for MS SQL Server.

<!--more-->

{{< toc >}}

{{< tabbed-codeblock examples >}}
<!-- tab cmd -->
ipconfig
<!-- endtab -->
<!-- tab SQL -->
-- Get all databases
select * from sysdatabases;
<!-- endtab -->
{{< /tabbed-codeblock >}}



SQLSERVER 查询今天、昨天、本周、上周、本月、上月数据

DATEDIFF (datepart, startdate, enddate) 计算时间差

datepare值 year | quarter | month | week | day | hour | minute | second | millisecond
startdate 开始日期
enddate 结束日期
GetDate() 获取当前的系统日期

下面例子中表名为tablename,条件字段名为inputdate
查询今天
SELECT * FROM tablename where DATEDIFF(day,inputdate,GETDATE())=0

查询昨天
SELECT * FROM tablename where DATEDIFF(day,inputdate,GETDATE())=1

查询本周
SELECT * FROM tablename where datediff(week,inputdate,getdate())=0

查询上周
SELECT * FROM tablename where datediff(week,inputdate,getdate())=1

查询本月
SELECT * FROM tablename where DATEDIFF(month,inputdate,GETDATE())=0

查询上月
SELECT * FROM tablename where DATEDIFF(month,inputdate,GETDATE())=1

查询本年
select * from Keywords  where datediff(year, Addtime,getdate())=0

