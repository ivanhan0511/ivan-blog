---
title: "MySQL DDL Operations"
date: 2022-08-10T14:42:36+08:00
categories:
- SQL
- MySQL
tags:
- MySQL
- DDL
keywords:
- create
- alter
- drop
- truncate
- comment
- rename
comments: true
showTags: true
showPagination: true
showSocial: true
showDate: true
---


DDL: Data Definition Language, like `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`, `RENAME`
<!--more-->

{{< toc >}}

## CREATE

### CREATE DATABASE
{{< codeblock "CREATE DATABASE" >}}
CREATE DATABASE pear-admin-pro CHARACTER SET UTF8;
{{< /codeblock >}}




### CREATE TABLE
{{< codeblock "CREATE TABLE" >}}
CREATE TABLE student (
        id INT,
        name VARCHAR(32)
        );
{{< /codeblock >}}


### CREATE INDEX

### COPY with data
```sql
create table  table_name
as   
select * from  Source_table
where   1=1;
```
### COPY without data
```sql
create table  table_name
as
select  * from
Source_table where   1 <> 1;
```


## ALTER

{{< codeblock "ALTER TABLE" >}}
ALTER TABLE ...
[TODO]: To be continued...
{{< /codeblock >}}



## DROP

### DROP TABLE

{{< codeblock "DROP TABLE" >}}
DROP TABLE <table_name>;
{{< /codeblock >}}




### DROP INDEX

## TRUNCATE

## COMMENT

## RENAME

