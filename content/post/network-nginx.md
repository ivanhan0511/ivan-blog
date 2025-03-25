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

nginx配置文件的各个字段解析: TODO


### nginx`location proxy_pass`后面的url 加与不加`/`的区别

Some examples

{{< tabbed-codeblock "Examples" >}}
<!-- tab one -->
# `location ^~/api` 后没有`/`, 转发网站没有`/`

location ^~/api {
    proxy_pass http://192.168.1.66:9222;
}
# 经过nginx转向之后最终的网址是: 192.168.1.66:9292/api/index.html
<!-- endtab -->


<!-- tab two -->
# `location ^~/api` 后没有`/`, 转发网站有`/`

location ^~/api {
    proxy_pass http://192.168.1.66:9292/;
}
# 经过nginx转向之后最终的网址是: 192.168.1.66:9292/index.html
<!-- endtab -->

<!-- tab three -->
# `location ^~/api/` 后有`/`, 转发网站没有`/`

location ^~/api/ {
    proxy_pass http://192.168.1.66:9292;
}
# 经过nginx转向之后最终的网址是: 192.168.1.66:9292/api/index.html
<!-- endtab -->

<!-- tab four -->
# `location ^~/api/` 后有`/`, 转发网站有`/`

location ^~/api/ {
    proxy_pass 192.168.1.66:9292/;
}
# 经过nginx转向之后最终的网址是: 192.168.1.66:9292/index.html
<!-- endtab -->
{{< /tabbed-codeblock >}}


