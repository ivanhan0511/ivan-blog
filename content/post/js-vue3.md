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

## I. ENV
---
### A. nvm
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


### B. npm and yarn
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




## II. GRAMMAR
---
### A. 杂记
**var、let、const 区别**

var定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问。

let定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问。

const用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改。


{{< codeblock vue >}}
// 块作用域
{
    var a = 1;
    let b = 2;
    const c = 3;
    // c = 4; // 报错
    var aa;
    let bb;
    // const cc; // 报错
    console.log(a); // 1
    console.log(b); // 2
    console.log(c); // 3
    console.log(aa); // undefined
    console.log(bb); // undefined
}
console.log(a); // 1
// console.log(b); // 报错
// console.log(c); // 报错

// 函数作用域
(function A() {
    var d = 5;
    let e = 6;
    const f = 7;
    console.log(d); // 5
    console.log(e); // 6  
    console.log(f); // 7 
})();
// console.log(d); // 报错
// console.log(e); // 报错
// console.log(f); // 报错
{{< /codeblock >}}

