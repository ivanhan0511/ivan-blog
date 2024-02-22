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
真机系统中不再安装node.js环境, 采用HBuilderX中的node环境`& 'C:\Program Files\HBuilderX\plugins\npm\npm.cmd' ...`, Updated at Oct 30, 2023

Download [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases) and install.


And use it in CMD:
{{< codeblock cmd >}}
nvm install 16.14.0 -g
nvm list
nvm use 16.17.1
#nvm uninstall 16.17.1
{{< /codeblock >}}


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



