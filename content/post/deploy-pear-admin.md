---
title: "Deploy Pear Admin"
date: 2022-05-25T15:39:32+08:00
categories:
- Deploy
- Flask
tags:
- Flask
- PearAdmin
keywords:
- deploy
- flask
- pear-admin
clearReading: true
thumbnailImage: images/E.jpg
thumbnailImagePosition: left
autoThumbnailImage: yes
metaAlignment: center
coverImage: images/E.jpg
coverCaption: "A beautiful image"
coverMeta: out
coverSize: full
coverImage: image-2.png
comments: true
showTags: true
showPagination: true
showSocial: true
showDate: true
---

Deploy a project of PearAdmin Flask
<!--more-->

{{< toc >}}


```shell
apt update
apt install git python3.8-dev mysql-server-8.0
```

## MySQL
方法1： 用SET PASSWORD命令
首先登录MySQL。
格式：mysql> set password for 用户名@localhost = password(‘新密码’);
例子：mysql> set password for root@localhost = password(‘123’);

方法2：用mysqladmin
格式：mysqladmin -u用户名 -p旧密码 password 新密码
例子：mysqladmin -uroot -p123456 password 123



show grants for 'test1'@'localhost;
create user 'test1'@'localhost' identified by '密码';
Mysql8.0默认采用caching-sha2-password加密，目前好多客户端不支持，可改为mysql_native_password;
create user 'test'@'%' identified with mysql_native_password BY '密码';
Alter user 'test1'@'localhost' identified by '新密码';

flush privileges;

grant all privileges on *.* to 'test1'@'localhost' with grant option;
flush privileges;
show grants for 'test1'@'localhost;
revoke all privileges on *.* from 'test1'@'localhost';
drop user 'test1'@'localhost';


sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
bind comment
