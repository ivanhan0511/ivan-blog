---
title: "MySQL8.0 Operations"
date: 2023-06-02T16:49:00+08:00
categories:
- SQL
- MySQL
tags:
- MySQL8.0
- init
- DCL
- DDL
- DML
- TCL
- VIEW
keywords:
- mysql
- init
- dcl
- ddl
- dml
- tcl
- view
---

Some usual operations examples in MySQL 8.0

<!--more-->

{{< toc >}}

## INIT
Refer to `https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04`

Ubuntu Server 20.04 LTS as example which is in a Cloud Server

{{< tabbed-codeblock examples >}}
<!-- tab ServerShell -->
sudo apt update
sudo apt install mysql-server-8.0 -y
#sudo apt install mysql-server -y
sudo systemctl status mysql.service

sudo mysql
# or
#mysql -u root -p  # Use the system user-password
<!-- endtab -->
<!-- tab DockerShell -->
docker run -p 3306:3306 --name mysql-rd -e MYSQL_ROOT_PASSWORD=XXXXXX -d mysql:8.0
mysql -h 127.0.0.1 -u root -p
<!-- endtab -->
<!--tab SQL -->
# Collect infomation
show databases;

SELECT host, user FROM mysql.user;

SET GLOBAL validate_password.policy=LOW;

CREATE DATABASE `some-db` CHARACTER SET UTF8;
CREATE USER 'rd'@'%' IDENTIFIED BY 'your_password';
#CREATE USER 'rd'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES on `some-db`.* TO 'rd'@'%';
#GRANT ALL PRIVILEGES ON *.* TO 'rd'@'%' WITH GRANT OPTION;  # This is deprecated!
#GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'rd'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
<!-- endtab -->
{{< /tabbed-codeblock >}}



区别

| caching_sha2_password |  
|  mysql_native_password |




<br>

## DCL
DCL: Data Control Language, like `GRANT`, `REVOKE`, `DENY`

### GRANT
### REVOKE
### DENY




<br>

## DDL
DDL: Data Definition Language, like `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`, `RENAME`

### CREATE

#### CREATE DATABASE
{{< codeblock "CREATE DATABASE" >}}
CREATE DATABASE pear-admin-pro CHARACTER SET UTF8;
{{< /codeblock >}}


#### CREATE TABLE
{{< codeblock "CREATE TABLE" >}}
CREATE TABLE student (
        id INT,
        name VARCHAR(32)
        );
{{< /codeblock >}}


#### CREATE INDEX

#### COPY with data
```sql
create table  table_name
as   
select * from  Source_table
where   1=1;
```
#### COPY without data
```sql
create table  table_name
as
select  * from
Source_table where   1 <> 1;
```


### ALTER

{{< codeblock "ALTER TABLE" >}}
ALTER TABLE ...
[TODO]: To be continued...
{{< /codeblock >}}



### DROP

#### DROP TABLE

{{< codeblock "DROP TABLE" >}}
DROP TABLE <table_name>;
{{< /codeblock >}}


#### DROP INDEX


### TRUNCATE

### COMMENT

### RENAME




<br>

## DML
DML: Data Manipulation Language, like `INSERT`, `SELECT`, `UPDATE`, `DELETE`, `MERGE`, `CALL`, `EXPLAIN PLAN`, `LOCK TABLE`


### INSERT

{{< codeblock "INSERT" "sql" >}}
INSERT INTO <table_name>...
[TODO]: To be continued...
{{< /codeblock >}}


### SELECT
{{< codeblock "SELECT" "sql" >}}
SELECT * FROM <table_name>;
{{< /codeblock >}}

#### The Time Filter
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


### UPDATE
{{< codeblock "UPDATE" "sql" >}}
UPDATE <table_name> SET ...;
[TODO]: To be continued...
{{< /codeblock >}}


### DELETE

{{< codeblock "DELETE" "sql" >}}
DELETE FROM <table_name> WHERE ...;
{{< /codeblock >}}

### MERGE
### CALL
### EXPLAIN PLAN
### LOCK TABLE


### EXAMPLES
#### Kill
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




### JOIN

![Joins](https://upload.wikimedia.org/wikipedia/commons/9/9d/SQL_Joins.svg)




<br>

## TCL
TCL: Transaction Control Language, like `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `START TRANSACTION`

{{< toc >}}

### COMMIT
### ROLLBACK
### SAVEPOINT
### START TRANSACTION




<br>

## VIEW
Create a SQLView to make query fast


## 锁
行锁, 表锁, 乐观锁, 悲观锁
