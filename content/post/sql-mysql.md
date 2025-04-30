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

## I. BASIC 
---
### A. Init

Refer to `https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04`

Ubuntu 24.04 as example in Win11 WSL2

{{< tabbed-codeblock examples >}}
<!-- tab shell -->
# root user
apt update
apt install mysql-server-8.0 -y
systemctl status mysql.service

mysql
<!-- endtab -->

<!--tab SQL -->
# Collect infomation
show databases;

SELECT host, user, plugin FROM mysql.user;

CREATE DATABASE `some-db` CHARACTER SET UTF8;
CREATE USER 'rd'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
GRANT ALL PRIVILEGES ON `some-db`.* TO 'rd'@'%';
# 由于 PROCESS 权限可以查看所有连接的线程和运行的 SQL，具有一定的敏感性，建议仅授予信任的内部账号或在备份服务器使用的账号
#GRANT PROCESS, LOCK TABLES, SHOW VIEW, EVENT, TRIGGER ON *.* TO 'rd'@'%';
FLUSH PRIVILEGES;

FLUSH PRIVILEGES;
<!-- endtab -->

<!--tab shell -->
# root user
vi /etc/mysql/mysql.conf.d/mysqld.cnf
# ...
# Comment this line
#bind-address = 127.0.0.1
# ...

systemctl restart mysql.service
<!-- endtab -->
{{< /tabbed-codeblock >}}


*区别*

| caching_sha2_password |  
|  mysql_native_password |


### B. Data

#### 1. Export

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


#### 2. Import

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




## II. OPERATIONS
---
|类别	|名称	|常见语句   |	功能说明|
|---    |---    |---        |---    |
|DDL	|Data Definition Language	    |CREATE, DROP, ALTER            |定义或修改数据库结构（表、索引、视图等）|
|DML	|Data Manipulation Language	    |INSERT, UPDATE, DELETE, MERGE	|操作表中的数据（增、改、删）|
|DQL	|Data Query Language	        |SELECT	                        |只查询数据，不做任何修改|
|DCL	|Data Control Language	        |GRANT, REVOKE	                |权限控制（授权与回收）|
|TCL	|Transaction Control Language	|COMMIT, ROLLBACK, SAVEPOINT	|事务管理|


### A. DCL

DCL: Data Control Language, like `GRANT`, `REVOKE`, `DENY`

#### 1. GRANT
{{< codeblock "GRANT" "SQL" >}}
GRANT ALL PRIVILEGES ON `some-db`.* TO 'rd'@'%';
FLUSH PRIVILEGES;
{{< /codeblock >}}


#### 2. REVOKE
#### 3. DENY




### B. DDL
---
DDL: Data Definition Language, like `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`, `RENAME`

#### 1. CREATE

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




#### 2. ALTER

{{< codeblock "ALTER TABLE" SQL >}}
ALTER TABLE ...
[TODO]: To be continued...
{{< /codeblock >}}



#### 3. DROP

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




#### 4. TRUNCATE

#### 5. COMMENT

#### 6. RENAME




### C. DML

DML: Data Manipulation Language, like `INSERT`, `UPDATE`, `DELETE`, `MERGE`, `CALL`, `EXPLAIN PLAN`, `LOCK TABLE`


#### 1. INSERT

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


#### 2. UPDATE
{{< codeblock "UPDATE" "sql" >}}
UPDATE <table_name> SET ...;
[TODO]: To be continued...
{{< /codeblock >}}


#### 3. DELETE

{{< codeblock "DELETE" "sql" >}}
DELETE FROM <table_name> WHERE ...;
{{< /codeblock >}}

#### 4. MERGE
#### 5. CALL
#### 6. EXPLAIN PLAN
#### 7. LOCK TABLE


#### 8. Examples

**Kill**
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




### D. DQL

DQL: Data Query Language, like `SELECT`

🤔 那为什么很多人以为 SELECT 是 DML 呢？
因为 SELECT 也是“对数据的操作”，但它不改变数据，只是“读取”。

在某些旧教材或非标准文档中，有时会把 DQL 归并到 DML 里，但这是不准确的。

✅ 正统 SQL 标准（比如 ANSI SQL）中：
SELECT = 只读查询语言，属于 DQL，与 DML 的“写操作”分开定义。


#### 1. SELECT
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


mysql> select table_schema, table_name, column_name, column_default, is_nullable, column_type, column_type, column_key, column_comment from information_schema.COLUMNS
where table_schema = 'ruoyi-vue-pro' and table_name = 'measure_pic';
+---------------+-------------+-----------------+-------------------+-------------+--------------+--------------+------------+-----------------------------------+
| TABLE_SCHEMA  | TABLE_NAME  | COLUMN_NAME     | COLUMN_DEFAULT    | IS_NULLABLE | COLUMN_TYPE  | COLUMN_TYPE  | COLUMN_KEY | COLUMN_COMMENT                    |
+---------------+-------------+-----------------+-------------------+-------------+--------------+--------------+------------+-----------------------------------+
| ruoyi-vue-pro | measure_pic | id              | NULL              | NO          | bigint       | bigint       | PRI        | 编号                              |
| ruoyi-vue-pro | measure_pic | measure_task_id | 0                 | NO          | bigint       | bigint       |            | 测量任务编号                      |
| ruoyi-vue-pro | measure_pic | pic_url         |                   | NO          | varchar(512) | varchar(512) |            | 图像地址                          |
| ruoyi-vue-pro | measure_pic | client_id       | NULL              | NO          | bigint       | bigint       |            | 客户端编号                        |
| ruoyi-vue-pro | measure_pic | category_id     | NULL              | NO          | bigint       | bigint       |            | 类别编号                          |
| ruoyi-vue-pro | measure_pic | magnification   | NULL              | NO          | int          | int          |            | 放大倍率, 是否可以删除?           |
| ruoyi-vue-pro | measure_pic | creator         |                   | YES         | varchar(64)  | varchar(64)  |            | 创建者                            |
| ruoyi-vue-pro | measure_pic | create_time     | CURRENT_TIMESTAMP | NO          | datetime     | datetime     |            | 创建时间                          |
| ruoyi-vue-pro | measure_pic | updater         |                   | YES         | varchar(64)  | varchar(64)  |            | 更新者                            |
| ruoyi-vue-pro | measure_pic | update_time     | CURRENT_TIMESTAMP | NO          | datetime     | datetime     |            | 更新时间                          |
| ruoyi-vue-pro | measure_pic | deleted         | b'0'              | NO          | bit(1)       | bit(1)       |            | 是否删除                          |
| ruoyi-vue-pro | measure_pic | tenant_id       | 0                 | NO          | bigint       | bigint       |            | 租户编号                          |
+---------------+-------------+-----------------+-------------------+-------------+--------------+--------------+------------+-----------------------------------+
12 rows in set (0.00 sec)




### E. TCL

TCL: Transaction Control Language, like `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `START TRANSACTION`


#### 1. COMMIT
#### 2. ROLLBACK
#### 3. SAVEPOINT
#### 4. START TRANSACTION




### F. VIEW

Create a SQLView to make query fast


## III. 锁
---
### A. 行锁
### B. 表锁
### C. 乐观锁
### D. 悲观锁
二、概念和用法
通常情况下，select语句是不会对数据加锁，妨碍影响其他的DML和DDL操作。同时，在多版本一致读机制的支持下，select语句也不会被其他类型语句所阻碍。
而selec... for update 语句是我们经常使用手工加锁语句。在数据库中执行select.for update,大家会发现会对数据库中的表或某些行数据进行锁表，在mysql四中，如果查询条件带有主键，会锁行数据，如果没有，会锁表。
由于InnoDB预设是Row-LevelLock，所以只有「明确」的指定主键，MySQL才会执行Row lock(只锁住被选取的资料例)，否则MySQL将会执行Table Lock(将整个资料表单给锁住)。




## IV. 驱动
---
TODO
