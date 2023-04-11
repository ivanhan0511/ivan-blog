---
title: "MySQL DML Operations"
date: 2022-08-10T14:42:36+08:00
categories:
- SQL
- MySQL
tags:
- MySQL
- DML
keywords:
- insert
- select
- update
- delete
- merge
- call
- explain plan
- lock table
comments: true
showTags: true
showPagination: true
showSocial: true
showDate: true
---

DML: Data Manipulation Language, like `INSERT`, `SELECT`, `UPDATE`, `DELETE`, 
    `MERGE`, `CALL`, `EXPLAIN PLAN`, `LOCK TABLE`
<!--more-->

{{< toc >}}

## INSERT

{{< codeblock "INSERT" "sql" >}}
INSERT INTO <table_name>...
[TODO]: To be continued...
{{< /codeblock >}}




## SELECT
{{< codeblock "SELECT" "sql" >}}
SELECT * FROM <table_name>;
{{< /codeblock >}}

### The Time Filter
{{< codeblock "Periods" sql >}}
-- 今天
SELECT * FROM 表名 WHERE TO_DAYS(时间字段名) = TO_DAYS(NOW());

-- 昨天
SELECT * FROM 表名 WHERE TO_DAYS(NOW()) - TO_DAYS(时间字段名) <= 1;

-- 本周
SELECT * FROM 表名 WHERE YEARWEEK(DATE_FORMAT(时间字段名,'%Y-%m-%d')) = YEARWEEK(NOW());

-- 上周
SELECT * FROM 表名 WHERE YEARWEEK(DATE_FORMAT(时间字段名,'%Y-%m-%d')) = YEARWEEK(NOW())-1;

-- 近7天
SELECT * FROM 表名 WHERE DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= DATE(时间字段名);

-- 近30天
SELECT * FROM 表名 WHERE DATE_SUB(CURDATE(), INTERVAL 30 DAY) <= DATE(时间字段名);

-- 本月
SELECT * FROM 表名 WHERE DATE_FORMAT(时间字段名,'%Y%m') = DATE_FORMAT(CURDATE(),'%Y%m');

-- 上月
SELECT * FROM 表名 WHERE PERIOD_DIFF(DATE_FORMAT(NOW(),'%Y%m'),DATE_FORMAT(时间字段名,'%Y%m')) = 1;
SELECT * FROM 表名 WHERE DATE_FORMAT(时间字段名,'%Y%m') = DATE_FORMAT(CURDATE(),'%Y%m') ;
SELECT * FROM 表名 WHERE WEEKOFYEAR(FROM_UNIXTIME(时间字段名,'%y-%m-%d')) = WEEKOFYEAR(NOW());
SELECT * FROM 表名 WHERE MONTH(FROM_UNIXTIME(时间字段名,'%y-%m-%d')) = MONTH(NOW());
SELECT * FROM 表名 WHERE YEAR(FROM_UNIXTIME(时间字段名,'%y-%m-%d')) = YEAR(NOW()) AND MONTH(FROM_UNIXTIME(时间字段名,'%y-%m-%d')) = MONTH(NOW())；

-- 近6个月
SELECT * FROM 表名 WHERE 时间字段名 BETWEEN DATE_SUB(NOW(),INTERVAL 6 MONTH) AND NOW();

-- 本季度
SELECT * FROM 表名 WHERE QUARTER(时间字段名) = QUARTER(NOW());

--- 上季度
SELECT * FROM 表名 WHERE QUARTER(时间字段名) = QUARTER(DATE_SUB(NOW(),INTERVAL 1 QUARTER));

-- 本年
SELECT * FROM 表名 WHERE YEAR(时间字段名)=YEAR(NOW());

-- 去年
SELECT * FROM 表名 WHERE YEAR(时间字段名) = YEAR(DATE_SUB(NOW(),INTERVAL 1 YEAR));
{{< /codeblock >}}


## UPDATE
{{< codeblock "UPDATE" "sql" >}}
UPDATE <table_name> SET ...;
[TODO]: To be continued...
{{< /codeblock >}}




## DELETE

{{< codeblock "DELETE" "sql" >}}
DELETE FROM <table_name> WHERE ...;
{{< /codeblock >}}

## MERGE
## CALL
## EXPLAIN PLAN
## LOCK TABLE


## EXAMPLES
### Kill
数据库锁死的问题解决
{{< codeblock "Kill lock" "sql" >}}
select *
from information_schema.PROCESSLIST;

select *
from information_schema.INNODB_TRX;

select A.trx_started, B.*
from information_schema.INNODB_TRX A
left join (select * from information_schema.PROCESSLIST) B on A.trx_mysql_thread_id = B.ID;

kill 12345;
{{< /codeblock >}}




## JOIN

![Joins](https://upload.wikimedia.org/wikipedia/commons/9/9d/SQL_Joins.svg)


