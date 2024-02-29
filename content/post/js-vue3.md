---
title: "VUE3"
date: 2023-10-26T14:13:00+08:00
categories:
- JavaScript
- VUE
tags:
- js
- vue3
- frontend
keywords:
- js
- vue3
- frontend
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

HAVE TO study some knowlege about frontend

<!--more-->

{{< toc >}}

## ENV

### nvm
Node Version Manager

nvm管理的是node, 但会同时把npm也安装好, node的版本和npm的版本是不一样的

Download [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases) and install.


And use it in CMD:
{{< codeblock cmd >}}
#nvm install 16.14.0 -g
nvm install 20.11.1 -g
#nvm uninstall 16.17.1
{{< /codeblock >}}

{{< blockquote >}}
Downloading node.js version 20.11.1 (64-bit)...
Extracting node and npm...
Complete
npm v10.2.4 installed successfully.


Installation complete. If you want to use this version, type

nvm use 20.11.1
{{< /blockquote >}}


### npm and yarn
{{< tabbed-codeblock cli >}}
<!-- tab npm -->
npm run dev
<!-- endtab -->

<!-- tab yarn -->
# 遇到npm无法调用的问题, 网上有记录, 但重启解决问题
npm install yarn -g

yarn install
yarn run serve-dev

#yarn install
#yarn run build-pro
<!-- endtab -->
{{< /tabbed-codeblock >}}



