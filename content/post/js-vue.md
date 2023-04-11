---
title: "Vue Development Environment"
date: 2023-04-11T17:53:32+08:00
categories:
- JavaScript
- Vue
tags:
- vue
keywords:
- js
- vue
- nvm
- npm
- yarn
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

A record about installing vue env.
<!--more-->

{{< toc >}}

## VUE ENVIROMENT

### nvm
Make 2 directories: `D:\\nvm` and `D:\\nodejs`

Download [nvm-setup.exe](https://github.com/coreybutler/nvm-windows/releases) and install.

When installing, use these 2 direcotories above.

And use it in CMD:
{{< codeblock cmd >}}
nvm install 16.14.0 -g
nvm list
nvm use 16.14.0
nvm uninstall 16.0.0
{{< /codeblock >}}


### npm
{{< codeblock cmd >}}
npm install yarn -g
{{< /codeblock >}}



### yarn
{{< tabbed-codeblock cmd >}}
<!-- tab serve -->
yarn install
yarn run serve-dev
<!-- endtab -->

<!-- tab build -->
yarn install
yarn run build-pro
<!-- endtab -->

{{< /tabbed-codeblock >}}


