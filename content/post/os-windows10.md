---
title: "What to Do after Init Windows"
date: 2023-02-15T15:16:59+08:00
categories:
- OS
- Windows
tags:
- Windows
- Init
- myOS
keywords:
- windows
- init
- config
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

After initialization of Windows, here is the TODO list.

<!--more-->

{{< toc >}}

## R&D
按先后顺序排序


VisualStudioPro  
最大化使用, C, Python, JavaScript, Vim
- 设置字体? 默认DejaVu Sans Mono? 避免像CMD.exe一样无法分辨小写L与数字1
- [Visual Studio 2019 修改字符集编码](https://blog.csdn.net/qq_41868108/article/details/105750175)
- 自动补全: Enter or Tab?
  - Visual Studio 2019是使用Tab进行代码补全的, 但一般习惯回车补全的时候就需要重新设置
  - 个人选择用Tab，原因是Linux默认的补全键是Tab, Notpad++ / Nacicat 也都是Tab补全
  - 具体路径: 工具 –> 选项 –> 文本编辑器 –> C/C++ -> 高级 –> 主动提交成员列表
- Install VsVim extension -> 超级好用, 可以随插入模式和普通模式的切换而自动切换输入法
- Both English and Chinese, prefer English
- 可能会提供git环境
- 使用其Python环境, 不安装Python在裸机
- 使用其npm环境? 不安装npm在裸机可以吗?  -> 貌似不行
- 是否能兼容调用MSYS2的一些命令而不用安装MSYS2?


~~CLion or VisualStudioPro ?~~  
目前有点倾向VisualStudioPro, 可以写C/C++, 还可以写Python, JavaScript, 何乐而不为呢?
Java就交给JetBrains IDEA就可以了


~~PyCharm or VisualStudioPro ?~~


IDEA
- git? 验证VisualStudioPro是否会安装git
- git? 验证MSYS2是否会安装git
- 使用其Java环境, 不安装Java在裸机


MSYS2 or Cygwin?  
先安装VisualStudioPro, 看看是否会提供git环境  
不行再试试MSYS2, 而不用安装Git For Bash  
~~MinGW32, MinGW64, WSL~~

~~HBuilder or WebStorm or VS Code ?~~  
~~npm?~~

~~Notepad++~~
- 可以使用VisualStudioPro代替


VMWare  
规划好哪些工具安装在裸机, 哪些安装在VMWare

- 切换安装路径到D:\盘
- 纯净Win10最新版, 快照
- 银行软件
- 各种Linux
  - PHP
  - Redis
  - ...
- 折腾环境/演示环境


Typora
- 需要看目录时, 例如接口文档中上来来回滚动, 还是需要Typora的目录
- 所见即所得的格式校验, PDF输出, 字体兼容, 还是需要Typora

XShell/XFtp

Navicat 还是最终DB的解决方案?  
MySQL Workbench和SSMS这俩工具太难用了, 还是Navicat吧

MySQL Server~~ + (MySQL Workbench)~~  
`MySQL Workbench -> Preferences -> SQL Editor -> QueryEditor -> use UPPERCASE keywords on completion`

SQL Server~~ + (SSMS)~~

向日葵Sunlogin

WireShark

微信开发者工具

hugo创建静态博客

Postman

Inno Setup Compiler(可选)




<br>

## BASIC APP
常用的跨平台工具, 兼容手机 / macOS / Windows

- Input method, ctrl+space to switch
- Chrome as default browser
- HHKB
- Logitech Anywhere3 and OptionPlus
- v2ray
- 坚果云
- Office365
- WeChat
- 腾讯会议
- 亿图
- Fiio driver
- EMail客户端




<br>

## GAME
Steam

EPIC

