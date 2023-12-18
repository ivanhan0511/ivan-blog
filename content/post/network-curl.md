---
title: "cURL"
date: 2022-06-01T16:56:35+08:00
categories:
- Network
- Tools
tags:
- cURL
keywords:
- curl
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

This is a custom summary and does **NOT** appear in the post.
<!--more-->

{{< toc >}}

在curl访问HTTPS站点时，如果没有CA证书，会提示访问缺少CA证书而失败，如下图：

curl-1.jpg



以下步骤可以实现curl带CA证书访问对应的HTTPS站点, 详情参见[证书转换]()

1. 将CA证书转换为pem格式的CA证书

以chrome浏览器为例说明：

打开HTTPS站点，点击该站点的证书图标-->【证书信息】-->【详细信息】-->【复制到文件】-->【下一步】-->选择【Base64 编码 X.509】--> 【下一步】-->点击【浏览】选择保存路径后输入文件名-->点击【下一步】完成-->【导出成功】。

2. 将导出的cert文件上传到linux系统中通过openssl将cert证书转换为.pem格式的：

openssl  x509  -in  a.cer  -out  a.pem

3. curl用法：

curl  -k  --cert  a.pem https://test.com/
