---
title: "SQL Server Init Operations"
date: 2023-08-10T17:32:00+08:00
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

## INIT
I have no project that starting with MS SQL Server  
But got a external DB in a project, so when developing, I just import the DB

- 安装完MS SQL Server + SSMS 之后, 在SSMS中通过"Windows身份验证"连接
- 在顶级目录(比如服务器或电脑名称)右键属性 -> 安全性 -> 选择"SQL Server和Windows身份验证模式"
- 在次顶级目录 -> 安全性 -> 登录名 -> 添加登录用户名, 等balabala设置
- SQL Server 配置管理器中, 允许TCP/IP连接

{{< tabbed-codeblock "init" >}}
<!-- tab cmd -->
ipconfig
<!-- endtab -->
<!-- tab SQL -->
-- Get all databases
select * from sysdatabases;
<!-- endtab -->
{{< /tabbed-codeblock >}}




## TIPs
---
### Datetime
{{< codeblock "Periods Filter" "sql" >}}
-- SQLSERVER 查询今天、昨天、本周、上周、本月、上月数据

-- DATEDIFF (datepart, startdate, enddate) 计算时间差
-- datepare值 year | quarter | month | week | day | hour | minute | second | millisecond
-- startdate 开始日期
-- enddate 结束日期
-- GetDate() 获取当前的系统日期

-- 下面例子中表名为tablename,条件字段名为inputdate
-- 查询今天
SELECT * FROM tablename where DATEDIFF(day,inputdate,GETDATE())=0

-- 查询昨天
SELECT * FROM tablename where DATEDIFF(day,inputdate,GETDATE())=1

-- 查询本周
SELECT * FROM tablename where datediff(week,inputdate,getdate())=0

-- 查询上周
SELECT * FROM tablename where datediff(week,inputdate,getdate())=1

-- 查询本月
SELECT * FROM tablename where DATEDIFF(month,inputdate,GETDATE())=0

-- 查询上月
SELECT * FROM tablename where DATEDIFF(month,inputdate,GETDATE())=1

-- 查询本年
select * from Keywords  where datediff(year, Addtime,getdate())=0
{{< /codeblock >}}


### 查询哪些表包含某个字段
{{< codeblock TableCollum sql >}}
SELECT
    a.name AS 表名,
    b.name AS 列名
FROM sys.objects a, sys.columns b
WHERE object_name(b.object_id)=a.name
    AND b.name = 'fpre'  -- 自定义搜索列名
    AND type='u'
{{< /codeblock >}}


### Others
{{< codeblock "Regex" "sql" >}}
--返回0-则为纯数字(支持正负数，小数点)
 SELECT PATINDEX('%[^0-9|.|-|+]%','2.2')--返回0

 --返回0-则为纯整数
select PATINDEX('%[^0-9]%', '2.2')--返回非0

SELECT TOP 50 PATINDEX('%[^0-9|.|-|+]%', ,FModel)
FROM t_ICItem;
{{< /codeblock >}}




## EXPORT & IMPORT
---
[TODO]: a
