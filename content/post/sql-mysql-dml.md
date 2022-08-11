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

DML: Data Manipulation Language
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
{{< /codeblock >}}

