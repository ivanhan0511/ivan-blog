---
title: "Deploy Pear Admin Flask"
date: 2024-01-19T11:52:32+08:00
categories:
- Python
- Deploy
tags:
- Flask
- PearAdmin
- Deploy
keywords:
- flask
- pear-admin
- deploy
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

Deploy a project which is small or flexible project, with PearAdmin Flask

<!--more-->

{{< toc >}}

## CLOUD SERVER

### SYSTEM

With Ubuntu Server 20.04 LTS as example

```shell
apt update
apt install git python3.8-dev mysql-server-8.0
```

### Domain

Create a new domain,




## WEIXIN

Log into [微信开发平台](https://mp.weixin.qq.com), Configure the new domain
into R&D settings




## DB

With MySQL as example

### Init MySQL

Please refer to [this post](sql-mysql-init.md)




## TOOLS

### ~~宝塔~~

- ~~安装宝塔云~~

    ```shell
    wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh
    ```

- ~~宝塔云, 软件商店极速安装MySQL服务~~

- ~~宝塔云, 安全组中开放3306端口~~

- ~~宝塔云, 数据库中添加数据库, 并设置外网可以访问~~

- ~~添加私有桥接docker网络~~

  ```shell
  apt install docker-compose
  docker network ls
  docker network create --driver bridge --subnet 172.18.0.0/16 --gateway 172.18.0.1 proxy-network
  docker network inspect proxy-network
  docker run -itd --name fuel_admin -p 25022:22 --network proxy-network ubuntu:20.04
  ```



- 将服务器公钥添加到Gitee的部署公钥中

- 配置nginx_reverse

- 容器内部部署

  ```shell
  apt install python3-venv iproute2 -y
  git clone <github or gitee projuect>
  cd <project>
  python3 -m venv venv
  ```

- 进入screen环境
  {{< tabbed-codeblock "activate" >}}
<!-- tab Ubuntu -->
source venv/bin/activate
<!-- endtab -->

<!-- tab Windows -->
cd venv/Scripts
activate
<!-- endtab -->

<!-- tab venv -->
# venv环境
python -m pip install -r requirements.txt
#flask init  # 初始化数据库, 如果需要
flask run
<!-- endtab -->
  {{< /tabbed-codeblock >}}

- 安装Nginx并配置

  ```shell
  cd /www/server/nginx/conf/vhost
  cat example.conf
  ```
  ```txt
  server {
      listen 80;
      server_name example.domain.com;
      location / {
      	  return 301 https://$host$request_uri;
      }
  }
  
  server {
      listen       443 ssl;
      server_name  example.domain.com;# 服务器地址或绑定域名
   
      #ssl证书的pem文件路径
      ssl_certificate /www/server/nginx/ssl/domain.com.pem;
  
      #ssl证书的key文件路径
      ssl_certificate_key /www/server/nginx/ssl/domain.com.key;
  
      ssl_session_timeout 5m;
  
      ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
      # 按照这个协议配置
      ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
  
      # 按照这个配置
      ssl_prefer_server_ciphers on;
  
      location / {  # ^~/api 表示匹配前缀为api的请求
          proxy_pass  http://127.0.0.1:5000/;  # 注：proxy_pass的结尾有/， -> 效果：会在请求时将/api/*后面的路径直接拼接到后面
    
          # proxy_set_header作用：设置发送到后端服务器(上面proxy_pass)的请求头值  
              # 【当Host设置为 $http_host 时，则不改变请求头的值;
              #   当Host设置为 $proxy_host 时，则会重新设置请求头中的Host信息;
              #   当为$host变量时，它的值在请求包含Host请求头时为Host字段的值，在请求未携带Host请求头时为虚拟主机的主域名;
              #   当为$host:$proxy_port时，即携带端口发送 ex: $host:8080 】
          proxy_set_header Host $host; 
    
          proxy_set_header X-Real-IP $remote_addr; # 在web服务器端获得用户的真实ip 需配置条件①    【 $remote_addr值 = 用户ip 】
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;# 在web服务器端获得用户的真实ip 需配置条件②	
  	      proxy_set_header X-Forwarded-Proto "https";
  	      proxy_set_header REMOTE-HOST $remote_addr;
          # proxy_set_header X-Forwarded-For $http_x_forwarded_for; # $http_x_forwarded_for变量 = X-Forwarded-For变量
      }
  
      access_log  /www/wwwlogs/example_access.log;
  }
  ```
  
- 将云服务器的证书文件放在指定位置
- 去宝塔页面点击Nginx模块的"服务"->"重载配置"



### Tinyproxy





### Lanproxy or Frp?

#### Server

- Download lanproxy  server from the [github website](https://github.com/ffay/lanproxy/releases).

- 开放端口

    - 8090: LanProxy的web配置端口
    - 4900: LanProxy对LanProxyClient的服务端口
    - <对外通信端口>: LanProxy对外的服务端口

- Start

    ```shell
    chmod 0755 [path_to_lanproxy]/bin/start.sh
    ./[path_to_lanproxy]/bin/start.sh
    ```



#### Client

下载[客户端](https://github.com/ffay/lanproxy-go-client/releases), 并使用screen保持会话

- 普通端口连接

    ```
    # mac 64位
    nohup ./client_darwin_amd64 -s SERVER_IP -p SERVER_PORT -k CLIENT_KEY &

    # linux 64位
    nohup ./client_linux_amd64 -s SERVER_IP -p SERVER_PORT -k CLIENT_KEY &

    # windows 64 位
    ./client_windows_amd64.exe -s SERVER_IP -p SERVER_PORT -k CLIENT_KEY
    ```

- SSL端口连接

    ```
    # mac 64位
    nohup ./client_darwin_amd64 -s SERVER_IP -p SERVER_SSL_PORT -k CLIENT_KEY -ssl true &

    # linux 64位
    nohup ./client_linux_amd64 -s SERVER_IP -p SERVER_SSL_PORT -k CLIENT_KEY -ssl true &

    # windows 64 位
    ./client_windows_amd64.exe -s SERVER_IP -p SERVER_SSL_PORT -k CLIENT_KEY -ssl true
    ```




## APPLICATION

### Download WeChat Cert

Download the certifications into `cert` folder in the project

### Configuration

- .flaskenv

  Checkout every line in this file

- gunicorn.conf.py

  Edit this line: `bind = '0.0.0.0:5000'`

- start.sh

  Choose a pattern to run: `production`, `development`, `testing`  
  `exec gunicorn -c gunicorn.conf.py "applications:create_app('production')"`


### Init DB

```shell
flask init
```

### Run
```shell
chmod 0755 start.sh
./start.sh
```




## REDIS

### Windows Env

```cmd
cd C:\program
redis-server redis.windows-service.conf
# 重启Redis服务
redis-cli.exe
#config set requirepass CofH2020
auth CofH2020
```


### Linux ENV

```shell
```




## OPERATION MANAGE

### 全数据库备份

- 备份整个数据库

    ```shell
    #!/usr/bin/bash

    number=365  # 保存备份个数，备份365天数据
    backup_dir=~/mysqlbackup  # 备份保存路径
    dd=`date +%Y-%m-%d_%H-%M-%S`  # 日期格式
    username=rd  # 用户名
    password=RandD268  # 密码
    database_name=fueldb  # 将要备份的数据库

    # 如果文件夹不存在则创建
    if [ ! -d $backup_dir ];
    then
        mkdir -p $backup_dir;
    fi

    # 备份
    # Example: mysqldump -u root -p123456 users > /root/mysqlbackup/users-$filename.sql
    mysqldump -u$username -p$password $database_name > $backup_dir/$database_name-$dd.sql

    #找出需要删除的备份
    delfile=`ls -l -crt $backup_dir/*.sql | awk '{print $9 }' | head -1`

    #判断现在的备份数量是否大于$number
    count=`ls -l -crt $backup_dir/*.sql | awk '{print $9 }' | wc -l`

    if [ $count -gt $number ]
    then
        #写删除文件日志
        echo "Delete $delfile" >> $backup_dir/log.txt

        #删除最早生成的备份，只保留number数量的备份
        rm $delfile
    fi
    ```

- 备份导入

    ```shell
    mysql -urd -pRandD268 fueldb < fueldb-20220118_17:08:02.sql
    ```



## PROJECT AVATAR

- 备份fuel_order表

    ```shell
    #!/usr/bin/bash

    number=1  # 保存备份个数，阿凡达中转用, 只备份1天
    backup_dir=~/avatar_backup  # 备份保存路径
    dd=`date +%Y-%m-%d_%H-%M-%S`  # 日期格式
    username=rd  # 用户名
    password=CofH2020  # 密码
    database_name=fueldb  # 将要备份的数据库
    table_name=fuel_order  # 将要备份的表


    # 如果文件夹不存在则创建
    if [ ! -d $backup_dir ];
    then
        mkdir -p $backup_dir;
    fi

    # 备份
    mysqldump -u$username -p$password $database_name $table_name > $backup_dir/$database_name.$table_name-$dd.sql

    #找出需要删除的备份
    delfile=`ls -l -crt $backup_dir/*.sql | awk '{print $9 }' | head -1`

    #判断现在的备份数量是否大于$number
    count=`ls -l -crt $backup_dir/*.sql | awk '{print $9 }' | wc -l`

    if [ $count -gt $number ]
    then
        #写删除文件日志
        echo "Delete $delfile" >> $backup_dir/log.txt

        #删除最早生成的备份，只保留number数量的备份
        rm $delfile
    fi
    ```

- Replicate a new DB with the data of table fuel_order

    ```shell
    mysql -urd -pRandD268 fueldb2 < fueldb.fuel_order-2022-01-18_17-08-02.sql
    ```


- 每天4点查询前一天的数据并插入到tmp表格

    ```sql
    # 确认使用的数据库
    use fadb;

    # Replicate fuel_order table for the first time
    #CREATE TABLE fuel_order_tmp LIKE fuel_order;

    # 清空临时表
    truncate table fuel_order_tmp;

    # 获取前一天的数据
    insert into fuel_order_tmp select * from fueldb.fuel_order where date(create_datetime) = date_sub(curdate(), INTERVAL 1 day );

    # 筛选随机X条数据(总金额的10%的条数), 并插入到fuel_fake_order表中
    # 暂且(Feb 16, 2022)人工确认随机数x
    insert into fuel_fake_order SELECT * FROM fuel_order_tmp ORDER BY rand() LIMIT 1;

    # 删除fuel_order中该随机数据
    delete from fuel_order_tmp where select

    # 更改临时表中剩余数据的时间
    update fuel_order_tmp set create_datetime = DATE_ADD(create_datetime, INTERVAL 1 DAY);
    update fuel_order_tmp set callback_datetime = DATE_ADD(callback_datetime, INTERVAL 1 DAY) where not isnull(callback_datetime);

    # 插入数据
    insert into fuel_order select * from fuel_order_tmp;
    ```

