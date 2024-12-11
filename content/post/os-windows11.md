---
title: "DIY My Windows"
date: 2024-09-30T10:39:00+08:00
categories:
- OS
- Windows
tags:
- Windows
- Init
- VisualStudio
- IDEA
- Configuration
- Proxy
- HBuildX
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

After initialization of Windows 11, here is the TODO list.

<!--more-->

{{< toc >}}

## I. BACKUP

主力开发操作系统是Windows, 如果要重装系统, 需要提前备份好本地文件

- PyCharm/IDEA
  - git commit and push
  - IDEA开发用的每一条SQL console, 右键Refactor -> Copy file, 存储到项目sql文件夹中随Git上传
  - Scratch代码
- ~~HBuildX~~改用VSCode
  - git commit and push
  - 据说可以通过vue-cli创建uni-app工程? 不太喜欢uni-app
- 坚果云文档: 仅限文档, 其余通过本地文件存储
- 本地文档
  - 公司超大文件
  - 各个项目的 密钥/证书
  - 服务器所涉及的 登录方式/用户名/密码 等文档
- Terminal
  - SSH密钥对, config
  - .vimrc(暂时没有必要备份, 重装后, 配置`.ideavimrc`和`.vscodevimrc`即可), vim bundle
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
- ~~XShell/XFtp `telnet <IP> <Port>`~~ (转用WSL2)
- ~~hugo~~(转用WSL2)
  - 在[HUGO的GitHub](https://github.com/gohugoio/hugo/releases)的releases中下载适用于Windows的文件压缩包
  - 比如放在`C:\Program Files\Git\usr\bin`中, 并添加到环境变量中
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
- ~~Axure RP 9(Axure Rapid Prototyping)~~(Deprecated, use Mock in iPad)





## III. WSL2

### A. Active WSL2

- Install Ubuntu, configure Ubuntu as default and configure color themes
- Autostart WSL2 with Win11 boot up
- SSH key-pairs, config file
- `.gitconfig` file
- `C:\User\Ivan\.wslconfig`?


### B. Vim

- Put `.vimrc` into `/root`目录(Dec 04, 2024, 新安装的Ubuntu默认使用的是root用户)
- Install bundle, which is recorded on the top


### C. Mysql

参见sql-mysql.md


### D. Redis

参见sql-mysql.md




## IV. IDE

关于开发语言/开发环境, 暂时都在物理机上安装和管理

- Java, 使用IDEA管理JDK环境, 存放在`C:\Users\Ivan\.jdks`
- Python, 使用Anaconda管理Python环境, 安装在`C:\Users\anaconda3`, PyCharm选择并调用
- C++, TODO @Ivan
- JavaScript, 使用nvm管理nodejs环境, 安装在`C:\Users\Ivan\AppData\Roaming\nvm`, VSCode通过`Remote - WSL`插件连接到WSL, 通过命令行操作`npm run dev`等


### A. Jetbrains PyCharm/IDEA

参见python-basic.md/java-basic.md中步骤

另外, 配置好WSL2之后, PyCharm/IDEA中的git可以自动检测到`Auto-detected: \\wsl.localhost\Ubuntu\usr\bin\git`


### B. VisualStudio Pro 2022 / VS Code

暂不使用Visual Studio, 多尝试VS Code

- hugo, 参见README.md
- C++, 参见c-plus-plus.md
- Vue3, 参见js-vue3.md




## V. VMWare  ?

Try V-box this time(Dec 03, 2024) ?
网上银行, BitCoin, 百度云盘 安装在哪里?

VMWare WorkStation Pro, 规划好哪些工具安装在裸机, 哪些安装在VMWare

- 切换安装路径到`D:\VirtualMachines\`盘
- 编辑 -> 首选项 -> 内存 -> 额外内存 "调整所有虚拟机内存使其适应预留的主机RAM"


### A. Windows

- Base镜像: 电源管理, 从不休眠
- MS SQL Server + SSMS

  Init, export, import refer to[MS SQL Server Opertaions](https://ivanhan0511.github.io/post/sql-sqlserver/)

- RD镜像: 开发试验用, 或安装垃圾软件等
- Finance镜像: Download Bitcoin wallet from [here](https://bitcoin.org/en/choose-your-wallet) and choose `Bitcoin Core`

Login [this website](https://www.huobi.com/en-us/login/) to trans BTC


### B. Ubuntu Desktop(Deprecated)

Refer to [this post](https://blog.csdn.net/weixin_43862116/article/details/107731631)挂载host主机的文件夹到Ubuntu挂在点, 用于共享

{{< codeblock "sharedfolder" "sh" >}}
vmware-hgfsclient
# WinDownloads

sudo mkdir /mnt/hgfs/downloads
sudo mount -t fuse.vmhgfs-fuse .host:WinDownloads /mnt/hgfs/downloads/ -o allow_other
#sudo umount -a fuse.vmhgfs-fuse .host:WinDownloads /mnt/hgfs/downloads/
sudo vi /etc/fstab
{{< /codeblock >}}

`.host:WinDownloads /mnt/hgfs/downloads/ fuse.vmhgfs-fuse allow_other 0 0`
