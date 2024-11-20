---
title: "MySQL8.0 Operations"
date: 2023-11-07T15:40:00+08:00
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
- backup
keywords:
- mysql
- init
- dcl
- ddl
- dml
- tcl
- view
- backup
---

Some usual operations examples in MySQL 8.0

<!--more-->

{{< toc >}}

## INIT
---
Refer to `https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04`

Ubuntu Server 22.04 LTS as example

{{< tabbed-codeblock examples >}}
<!-- tab shell -->
sudo apt update
sudo apt install mysql-server-8.0 -y
sudo systemctl status mysql.service

sudo mysql
<!-- endtab -->

<!--tab SQL -->
# Collect infomation
show databases;

SELECT host, user, plugin FROM mysql.user;

CREATE DATABASE `some-db` CHARACTER SET UTF8;
CREATE USER 'rd'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
GRANT ALL PRIVILEGES ON `some-db`.* TO 'rd'@'%';
FLUSH PRIVILEGES;
<!-- endtab -->

<!--tab shell -->
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
# ...
# Comment this line
#bind-address = 127.0.0.1
# ...
sudo systemctl restart mysql.service
<!-- endtab -->
{{< /tabbed-codeblock >}}


区别

| caching_sha2_password |  
|  mysql_native_password |




## DCL
---
DCL: Data Control Language, like `GRANT`, `REVOKE`, `DENY`

### GRANT
{{< codeblock "GRANT" "SQL" >}}
GRANT ALL PRIVILEGES ON `some-db`.* TO 'rd'@'%';
FLUSH PRIVILEGES;
{{< /codeblock >}}


### REVOKE
### DENY




## DDL
---
DDL: Data Definition Language, like `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`, `RENAME`

### CREATE

{{< tabbed-codeblock "CREATE" >}}
<!-- tab database -->
CREATE DATABASE pear-admin-pro CHARACTER SET UTF8;
<!-- endtab -->

<!-- tab table -->
CREATE TABLE student (
        id INT,
        name VARCHAR(32)
        );
<!-- endtab -->

<!-- tab index -->
<!-- endtab -->
{{< /tabbed-codeblock >}}


*COPY with data*
{{< tabbed-codeblock "copy with CREATE" >}}
<!-- tab with_data -->
create table table_name
as   
select * from Source_table
where 1=1;
<!-- endtab -->

<!-- tab without_data -->
create table table_name
as
select * from
Source_table where 1 <> 1;
<!-- endtab -->
{{< /tabbed-codeblock >}}




### ALTER

{{< codeblock "ALTER TABLE" SQL >}}
ALTER TABLE ...
[TODO]: To be continued...
{{< /codeblock >}}



### DROP

{{< tabbed-codeblock "DROP" >}}
<!-- tab database -->
DROP DATABASE <db_name>;
<!-- endtab -->

<!-- tab table -->
DROP TABLE <table_name>;
<!-- endtab -->

<!-- tab index -->
DROP ...;
<!-- endtab -->
{{< /tabbed-codeblock >}}




### TRUNCATE

### COMMENT

### RENAME




## DML
---
DML: Data Manipulation Language, like `INSERT`, `SELECT`, `UPDATE`, `DELETE`, `MERGE`, `CALL`, `EXPLAIN PLAN`, `LOCK TABLE`


### INSERT

{{< tabbed-codeblock "INSERT" >}}
<!-- tab single -->
INSERT INTO <table_name>...
<!-- endtab -->
<!-- tab multiple -->
INSERT INTO <table_name> (
    'c1', 'c2')
VALUES (1, 2), (3, 4)
<!-- endtab -->
{{< /tabbed-codeblock >}}


### SELECT
{{< codeblock "Periods Filter" "sql" >}}
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

-- 上季度
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




## TCL
---
TCL: Transaction Control Language, like `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `START TRANSACTION`


### COMMIT
### ROLLBACK
### SAVEPOINT
### START TRANSACTION




## VIEW
---
Create a SQLView to make query fast


## 锁
---
### 行锁
### 表锁
### 乐观锁
### 悲观锁


## EXPORT & IMPORT
---

### Export

{{< codeblock "export" "shell" >}}
# 导出整个数据库结构和数据
mysqldump -h localhost -P 3306 -urd -p123456 database > test.sql
# 导出整个数据库结构(不包含数据)
mysqldump -h localhost -P 3306 -urd -p123456 -d database > test.sql

# 导出单个数据表结构和数据
mysqldump -h localhost -P 3306 -urd -p123456 database table > test.sql
# 导出单个数据表结构(不包含数据)
mysqldump -h localhost -P 3306 -urd -p123456 -d database table > test.sql

# 说明:
# -P参数, 是大P, 且有空格, 不同于后面密码的小P，没有空格
{{< /codeblock >}}


### IMPORT

{{< tabbed-codeblock "import" >}}
<!-- tab shell -->
# 恢复到指定数据库
mysql -hhostname -uusername -ppassword databasename < test.sql

# 恢复到指定数据库中的表
mysql -hhostname -uusername -ppassword databasename tablename < test.sql
<!-- endtab -->
<!-- tab sql -->
use some_db;
source /data/test.sql;
<!-- endtab -->
{{< /tabbed-codeblock >}}


