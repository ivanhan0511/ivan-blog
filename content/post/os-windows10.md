---
title: "What To Do After Init Windows"
date: 2023-09-12T14:24:00+08:00
categories:
- OS
- Windows
tags:
- Windows
- Init
- IDEA
- Configure
- Proxy
- VisualStudio
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

After initialization of Windows 10, here is the TODO list.

<!--more-->

{{< toc >}}


## BACKUP
---
主力开发操作系统是Windows, 如果要重装系统, 需要提前备份好本地文件

- 代码: 基本通过git有同步, 其余少量的通过copy完成
- 坚果云文档: 公司文档, 仅限文档, 其余通过本地文件存储
- 本地文档
  - 公司大文件
  - 各个项目的密钥 / 证书
  - 服务器所涉及的登录方式 / 用户名 / 密码 的文档
  - SSH密钥, 私人文件等
- 虚拟机
  - 数据库: 转储数据和数据结构, MySQL和SQLServer的所有库
  - SSH密钥
  - 证书
- IDEA
  - IDEA本身的配置通过云同步
  - IDEA开发用的每一条SQL console, 右键Refactor -> Copy file, 随Git上传
- 图片
- 视频




## BOOT DEVICE
---
- Create Windows official MediaCreationTool
- Choose boot device in BIOS 




## BASE
---
只要连接互联网, 现有的Windows10会自动联网安装驱动, 只需要等待更新/重启

趁着没有使用网络代理, 先安装基本工具

- VisualStudioPro2022  
  - Prefer English language 
  - Libs
  - Git(Will install MINGW64 env into Widnows system)
- Input method, config only `ctrl + space` to switch
- Chrome as default browser
- HHKB
- Logitech Anywhere3 and OptionPlus




## Proxy
---
v2rayN(Need DotNet6.0)

注意新安装的Windows, 要确保对时准确, 精准到1分钟以内, 否则代理建立连接失败


## VisualStudioPro2022  
---

最大化使用, C/C++, Python, Vim, Git, PowerShell

- 设置项目存储路径, Tools -> Options -> Projects and Solutions -> Locations
- Install VsVim extension
  - Check which directories that VsVim looks for this file in by using the command `:set vimrcpaths?`
  - This is typically the `HOME`, `VIM` or `USERPROFILE` directories
  - Place your `.vimrc` file in one of these directories, restart Visual Studio and VsVim will load those settings
  - You can verify which vimrc file is currently loaded in VsVim by using the command `:set vimrc?`
- ~~设置字体? 默认DejaVu Sans Mono? 避免像CMD.exe一样无法分辨小写L与数字1~~
- [Visual Studio 增加菜单来修改字符集编码](https://blog.csdn.net/qq_41868108/article/details/105750175)
- 自动补全: Enter or Tab?
  - Visual Studio 2022是使用Tab进行代码补全的, 但一般习惯回车补全的时候就需要重新设置
  - 个人选择用Tab，原因是Linux默认的补全键是Tab, Notpad++ / Nacicat 也都是Tab补全
  - 具体路径: 工具 –> 选项 –> 文本编辑器 –> C/C++ -> 高级 –> 主动提交成员列表




## IDEA
---

### Configuration
- 在各个项目中分别选择Java JDK环境, 不安装Java在裸机
- settsings(Ctrl+Alt+S) -> Editor -> Code Style -> SQL -> 将keywords设置为大写(To upper), and then`ctrl + alt + L`
- 数据库导入时, 右键某个数据库 -> Import/Export -> Restore with 'mysql' -> Path to mysql -> `C:/Program Files/MySQL/MySQL Server 8.1/bin/mysql.exe`
- 数据库导出时, 右键某个数据库 -> Import/Export -> Export with 'mysqldump' -> Path to mysqldump -> `C:/Program Files/MySQL/MySQL Server 8.1/bin/mysqldump.exe`


### Plugins
- IdeaVim
  - 在IDEA中创建`~/.ideavimrc`文件(实际创建在`C:\Users\Ivan\.ideavimrc`)
  - 只需在最后一行增加配置引用即可`source ~/.vimrc`
  - "IDEA中自动切换输入法暂时没搞定" "Windows VisualStudio的VsVim插件倒是自动有这个配置, 没琢磨明白, 有的用"
- MyBatisCodeHelperPro(貌似有官方版, 下次试试)
- Redis


### Maven
Maven 3.8.1 blocked http connection

- Do {{< hl-text red >}}NOT{{< /hl-text >}} edit this original IDEA maven settings file
  `C:\Program Files\JetBrains\IntelliJ IDEA 2022.2.1\plugins\maven\lib\maven3\conf\settings.xml`
  {{< codeblock "settings.xml" "XML" >}}
  ...
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
    <mirror>
      <id>maven-default-http-blocker</id>
      <mirrorOf>external:http:*</mirrorOf>
      <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
      <url>http://0.0.0.0/</url>
      <blocked>true</blocked>
    </mirror>
  
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
  ...
  {{< /codeblock >}}

- Find personal maven setting path in IDEA settings and DIY it `C:\Users\ivan\.m2\settings.xml` (If not exists, create this file)

  {{< codeblock "settings.xml" "XML" >}}
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 http://maven.apache.org/xsd/settings-1.2.0.xsd">
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
    -->
    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
</settings>
  {{< /codeblock >}}

- Reload pom.xml file in IDEA and automaticlly download the dependencies


### RemoteHost
Remote deploy and remote debug
{{< codeblock "cli" >}}
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar pear-admin-pro-1.11.9-SNAPSHOT.jar
{{< /codeblock >}}





## Git & SSH
---
MINGW64 Git套件

自己添加插件, 来增加额外的的Linux命令

### Hugo
- 在[HUGO的GitHub](https://github.com/gohugoio/hugo/releases)的releases中下载适用于Windows的文件压缩包
- 解压到`C:\Program Files\Git\usr\bin\`目录中
- 再打开GitBash即可使用hugo命令创建静态Blog了


### VimBundle(smartim)
{{< codeblock "VimBundle" "shell" >}}
# It's alse suitable for both Linux and Windows
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim
#:PluginInstall
{{< /codeblock >}}

Download [im-select.exe](https://github.com/daipeihust/im-select?tab=readme-ov-file#linux-1) and put it into `C:\Users\auser\.vim\bundle\smartim\plugin\'  
And set this into system env PATH


### nc

### ss



## HBuilderX
---
- 使用其npm环境? 不安装npm在裸机可以吗?  -> HBuilderX自带node.js环境, 待研究

看团队用什么吧, 也不打算深究Vue, 能调试个本地前端页面就行了

层层递进

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


### npm
{{< codeblock cmd >}}
# 遇到npm无法调用的问题, 网上有记录, 但重启解决问题
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




## DB
---
MySQL安装在Windows中也行, 安装在VMWare中也行

毕竟MySQL常年在用, 每次都开VMWare虚拟机很麻烦

如果安装在真机, 现有的安装包+配置工具已经很傻瓜式了, 安装好之后只用`MySQL 8.1 Command Line Clien`登录用于DCL(Data Control Language)管理  
然后用IDEA(Ultimate)的Import/Export 工具进行数据到导入/导出, ER图也可以导出, [各种DDL(Data Definition Language), DML(Data Manipulation Language)](https://ivanhan0511.github.io/post/sql-mysql/)都可以在IDEA中完成





## VMWare  
---
VMWare WorkStation Pro, 规划好哪些工具安装在裸机, 哪些安装在VMWare

- 切换安装路径到`D:\VirtualMachines\`盘
- 编辑 -> 首选项 -> 内存 -> 额外内存 "调整所有虚拟机内存使其适应预留的主机RAM"


### Windows
- 电源管理, 从不休眠
- 微信开发者工具
- 抖音开发者工具
- MS SQL Server + SSMS

  Init, export, import refer to[MS SQL Server Opertaions](https://ivanhan0511.github.io/post/sql-sqlserver/)


### Ubuntu Server
- ~~Redis~~
- ~~MySQL~~

  Init, export, import refer to[MySQL Opertaions](https://ivanhan0511.github.io/post/sql-mysql/)


### Ubuntu Desktop(暂时弃用)

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




## Others
---
常用的跨平台工具, 兼容iOS / macOS / Windows / Android

- Chrome
- XShell/XFtp
- ~~Postman~~ 去Postman化, 采用IDEA内置的`Scratches and Consoles`编写SQL和HTTP测试用例
- 向日葵Sunlogin
- WireShark
- Typora
- 坚果云
- Office365(Outlook, Excel, Word, PowerPoint)
- WeChat
- 腾讯会议
- 亿图Edraw
- Fiio driver
- Notepad++
- Axure RP 9 (Axure Rapid Prototyping)




## GAME
---
- Steam 可以通过内部工具迁移游戏存储目录
- EPIC 不能迁移, 重新下载
- Minecraft Bedrock Edition




## BITCOIN
---
Download Bitcoin wallet from [here](https://bitcoin.org/en/choose-your-wallet) and choose `Bitcoin Core`

Login [this website](https://www.huobi.com/en-us/login/) to trans BTC
