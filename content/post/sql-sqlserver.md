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



SQLSERVER ��ѯ���졢���졢���ܡ����ܡ����¡���������

DATEDIFF (datepart, startdate, enddate) ����ʱ���

datepareֵ year | quarter | month | week | day | hour | minute | second | millisecond
startdate ��ʼ����
enddate ��������
GetDate() ��ȡ��ǰ��ϵͳ����

���������б���Ϊtablename,�����ֶ���Ϊinputdate
��ѯ����
SELECT * FROM tablename where DATEDIFF(day,inputdate,GETDATE())=0

��ѯ����
SELECT * FROM tablename where DATEDIFF(day,inputdate,GETDATE())=1

��ѯ����
SELECT * FROM tablename where datediff(week,inputdate,getdate())=0

��ѯ����
SELECT * FROM tablename where datediff(week,inputdate,getdate())=1

��ѯ����
SELECT * FROM tablename where DATEDIFF(month,inputdate,GETDATE())=0

��ѯ����
SELECT * FROM tablename where DATEDIFF(month,inputdate,GETDATE())=1

��ѯ����
select * from Keywords  where datediff(year, Addtime,getdate())=0

