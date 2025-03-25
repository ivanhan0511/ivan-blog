---
title: "Customize My OS"
date: 2025-01-19T10:54:00+08:00
categories:
- OS
tags:
- Windows
- macOS
keywords:
- windows
- macOS
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

After initialization of a Windows 11 or a macOS, here is the TODO list.

<!--more-->

{{< toc >}}

## I. BACKUP
---

如果要重装系统, 需要提前备份好本地文件

- PyCharm/IDEA
  - git commit and push
  - resources中的.yaml文件均需要保存, 且用于隔离不同生产环境的已经固化的.yaml谨慎修改
  - Database Consoles中的SQL代码, 分别右键`Refactor -> Copy File...`, 在开发过后补资料阶段, 梳理后存储到项目sql文件夹中随Git上传
  - Scratches中的一些pojo代码, 酌情备份
- VisualStudioPro2022
  - git commit and push
- VSCode
  - git commit and push
  - 据说可以通过vue-cli创建uni-app工程?
- 坚果云文档: 仅限文档, 其余通过本地文件存储
- 本地文档
  - 公司超大文件
  - 各个项目的 密钥/证书
  - 服务器所涉及的 登录方式/用户名/密码 等文档
- `C:\Users\ASUS\.vsvimrc`专为VisualStudio配置的(VSCode不需要深入配置和研究)
- Remove Passkey from such as Github or anyelse website
- WSL2
  - `~/.ssh/id_ed25519*`, `~/.ssh/config`
  - .vimrc(坚果云有备份)
  - .bashrc(暂时没有必要备份, 重装后, 配置`apssh`即可)
  - .gitconfig
- Proxy文件夹
- 虚拟机
  - Finance
  - 其它特种数据?
- Image
- Video
- Music
- Create a bootable USB by Microsoft official `MediaCreationTool`




## II. BASE
---

只要连接互联网, 现有的Windows11会自动联网安装驱动, 只需要等待更新/重启

常用的跨平台工具, 兼容iOS / macOS / Windows / Android

- Input method, config only `ctrl + space` to switch(微软输入法与IDEA关于重命名变量的热键冲突已经被修复)
- Chrome as default browser, 下载工具, 查询密码, 都需要它, 优先安装
- HHKB
- Logitech G703 and G Hub
- 文件夹选项 -> 隐私
  - 取消勾选"显示最近使用的文件"
  - 取消勾选"显示常用文件夹"
- v2rayN(Need DotNet6.0) 注意新安装的Windows, 要确保对时准确, 精准到1分钟以内, 否则代理建立连接失败
- hugo in WSL2
  - ~~在[HUGO的GitHub](https://github.com/gohugoio/hugo/releases)的releases中下载适用于Windows的文件压缩包~~
  - ~~比如放在`C:\Program Files\Git\usr\bin`中, 并添加到环境变量中~~
- 向日葵Sunlogin
- WireShark
- Typora
- 坚果云
- Office365(Outlook, Excel, Word, PowerPoint)
- WeChat
- 腾讯会议
- 亿图Edraw
- [Fiio driver](https://www.fiio.com/Driver_Download)
- Notepad++
  - 确认 字符集/字体/缩进/换行符(Unix LF), 深色模式随系统
- ~~Axure RP 9(Axure Rapid Prototyping)~~(Deprecated, use Mock in iPad)





## III. WSL2
---

If use Windows11, I like WSL2 so much

If use macOS, natively with terminal


### A. Active WSL2

- Install Ubuntu, configure Ubuntu as default and configure color themes
- Autostart WSL2 with Win11 boot up
- SSH key-pairs, config file
- `.gitconfig` file
- `C:\User\Ivan\.wslconfig`?


### B. Vim

- Put `.vimrc` into `/root`目录(Dec 04, 2024, 新安装的Ubuntu默认使用的是root用户)
- Install bundle, which is recorded on the top of `.vimr` file


### C. Mysql

参见sql-mysql.md


### D. Redis

参见sql-mysql.md




## IV. IDE
---

关于开发语言/开发环境, 暂时都在物理机上安装和管理

- Java, 使用IDEA管理JDK环境, 存放在`C:\Users\Ivan\.jdks`
- Python, 使用Anaconda管理Python环境, 安装在`C:\Users\anaconda3`, PyCharm选择并调用
- C++, Microsoft Visual Studio Professional 2022 (64-bit)
  - 编译器: 目前(Feb 13, 2025)首选MSVC, 不折腾, 能学习, 能运行, 能实战就行
- JavaScript, 使用nvm管理nodejs环境, 安装在`C:\Users\Ivan\AppData\Roaming\nvm`, VSCode通过`Remote - WSL`插件连接到WSL, 通过命令行操作`npm run dev`等


### A. Jetbrains (PyCharm/IDEA)

参见python-basic.md/java-basic.md中步骤

另外, 配置好WSL2之后, PyCharm/IDEA中的git可以自动检测到`Auto-detected: \\wsl.localhost\Ubuntu\usr\bin\git`


### B. VisualStudio Pro 2022

VisualStudio Pro 2022, 64位的IDE

- hugo, 参见README.md
- C++, 参见c-plus-plus.md


### C. VS Code

轻度使用VS Code就好了, 暂时不要过度使用

- Vue3, 参见js-vue3.md



## V. VirtualBox
---

Try V-box this time(Dec 03, 2024) ?
- Finance镜像: Download Bitcoin wallet from [here](https://bitcoin.org/en/choose-your-wallet) and choose `Bitcoin Core`
- Login [this website](https://www.huobi.com/en-us/login/) to trans BTC
- 百度云盘
- 迅雷


修复
欢迎咨询社区！我是Chen Pondsi。

图上看到可能是Windows11，
建议打开设置，时间和语言，输入，高级键盘设置，取消勾选“允许我为每个应用窗口使用不同的输入法”。

然后鼠标右键单击开始按钮（微软图标的按钮）→"Windows PowerShell(I)（管理员)(A ）”→输入：
（WIndows11中可能显示Windows 终端（管理员））
sfc /SCANNOW
（按下Enter键）
Dism /Online /Cleanup-Image /ScanHealth
（按下Enter键）
Dism /Online /Cleanup-Image /CheckHealth
（按下Enter键）
DISM /Online /Cleanup-image /RestoreHealth
（按下Enter键）
完成后重启电脑，再次输入：
sfc /SCANNOW
（按下Enter键）

注意！请务必遮挡个人信息（电子邮件/电话/姓名等）。（若是没有，请忽略）
48小时内没有新的回复，我将无法再收到回复提醒。
