---
title: "Nginx Configurations"
date: 2023-02-21T15:06:52+08:00
categories:
- Network
- Nginx
tags:
- Nginx
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

This is a custom summary and does *NOT* appear in the post.
<!--more-->

{{< toc >}}

server {
    listen       80;
    server_name  www.a.com;
    access_log  logs/test.access.log;
    # 匹配以/apis/开头的请求
    location ^~ /apis/ {
        proxy_pass http://www.b.com/;  #注意域名后有一个/
    }
    location / {
        root html/a;
        index index.html index.htm;
    }
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}

没错就是proxy_pass后面的地址加一个/

加/的情况，访问www.a.com/apis/login.json会得到www.b.com/login.json,使用b文件夹作为/目录来访问

不加/的情况,访问www.b.com/apis/login.json会得到www.b.com/apis/login.json,使用b文件下的apis作为根目录进行访问

