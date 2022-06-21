---
title: "MySQL8.0 Init Operations"
date: 2022-05-24T02:58:26+08:00
categories:
- SQL
- MySQL
tags:
- MySQL8.0
keywords:
- mysql
- init
---

Init operations of MySQL 8.0
<!--more-->

{{< toc >}}

## In a normal server

`https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04`

Ubuntu Server 20.04 LTS as example  
Tencent Cloud Server

```shell
sudo apt update
sudo apt install mysql-server-8.0 -y
#sudo apt install mysql-server
sudo systemctl status mysql.service

sudo mysql
# or
#mysql -u root -p  # Use the system user-password
```
```sql
SET GLOBAL validate_password.policy=LOW;
#CREATE DATABASE xxxdb charset=utf8;
CREATE USER 'rd'@'%' IDENTIFIED BY 'password';
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'rd'@'%' WITH GRANT OPTION;
#GRANT ALL PRIVILEGES ON *.* TO 'rd'@'%' WITH GRANT OPTION;  # This is deprecated!
FLUSH PRIVILEGES;
exit
```




## In a Docker

```shell
docker run -p 3306:3306 --name mysql-rd -e MYSQL_ROOT_PASSWORD=RandD268 -d mysql:8.0
mysql -h 127.0.0.1 -u root -p
```

```sql
CREATE DATABASE xxxdb charset=utf8;
CREATE USER 'rd'@'%' IDENTIFIED BY 'password';
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'rd'@'%' WITH GRANT OPTION;
#GRANT ALL PRIVILEGES on xxxdb.* TO 'rd'@'%';
FLUSH PRIVILEGES;
```

