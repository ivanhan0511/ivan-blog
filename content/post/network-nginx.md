---
title: "Nginx Configuration"
date: 2023-02-21T15:06:52+08:00
categories:
- Network
- Nginx
tags:
- Nginx
- Config
- CORS
keywords:
- nginx
- config
- cors
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

Nginx config
<!--more-->

{{< toc >}}
## nginx location proxy_pass 后面的url 加于不加 / 的区别

即proxy_pass后的 / 会截掉location后跟的地址.

{{< codeblock "dev.conf" >}}
server {
    listen 80;
    server_name dev.xxx.com;
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen       80;
    server_name  dev.xxx.com;

    location ^~ /api {
        proxy_pass http://192.168.1.66:9292;

        # proxy_set_header作用：设置发送到后端服务器(上面proxy_pass)的请求头值
        # 当Host设置为 $http_host 时，则不改变请求头的值;
        # 当Host设置为 $proxy_host 时，则会重新设置请求头中的Host信息;
        # 当为$host变量时，它的值在请求包含Host请求头时为Host字段的值，在请求未携带Host请求头时为虚拟主机的主域名;
        # 当为$host:$proxy_port时，即携带端口发送 ex: $host:8080 

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;  # 在web服务器端获得用户的真实ip 需配置条件① $remote_addr值 = 用户ip
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  # 在web服务器端获得用户的真实ip 需配置条件②
        proxy_set_header REMOTE-HOST $remote_addr;
        # proxy_set_header X-Forwarded-For $http_x_forwarded_for;  # $http_x_forwarded_for变量 = X-Forwarded-For变量
    }

    location / {
        root /var/wwww/dev;
        try_files $uri $uri/ /index.html;
        index index.html index.htm;
    }
    
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}
{{< /codeblock >}}

Some examples

{{< tabbed-codeblock "Examples" >}}
<!-- tab one -->
#location ^~/api后没有`/`, 转发网站没有`/`
location ^~/api {
    proxy_pass http://192.168.1.66:9222;
}
#经过nginx转向之后最终的网址是: 192.168.1.66:9292/api/index.html
<!-- endtab -->


<!-- tab two -->
#location ^~/api后没有`/`, 转发网站有`/`
location ^~/api {
    proxy_pass http://192.168.1.66:9292/;
}
#经过nginx转向之后最终的网址是: 192.168.1.66:9292/index.html
<!-- endtab -->

<!-- tab three -->
#location ^~/api/后有`/`, 转发网站没有`/`
location ^~/api/ {
    proxy_pass http://192.168.1.66:9292;
}
#经过nginx转向之后最终的网址是: 192.168.1.66:9292/api/index.html
<!-- endtab -->

<!-- tab four -->
#location ^~/api/后有`/`, 转发网站有`/`
location ^~/api/ {
    proxy_pass 192.168.1.66:9292/;
}
#经过nginx转向之后最终的网址是: 192.168.1.66:9292/index.html
<!-- endtab -->
{{< /tabbed-codeblock >}}


