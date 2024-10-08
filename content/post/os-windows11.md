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


## BACKUP
---
主力开发操作系统是Windows, 如果要重装系统, 需要提前备份好本地文件

- VisualStudio
  - TODO
- PyCharm
  - TODO
- IDEA
  - git commit and push
  - IDEA开发用的每一条SQL console, 右键Refactor -> Copy file, 存储到项目sql文件夹中随Git上传
  - Scratch代码
- HBuildX
  - No local file, all in git
- 坚果云文档: 公司文档, 仅限文档, 其余通过本地文件存储
- 本地文档
  - 公司超大文件
  - 各个项目的 密钥/证书
  - 服务器所涉及的 登录方式/用户名/密码 的文档
  - SSH密钥对
- 虚拟机
  - 数据库: 转储数据和数据结构, MySQL和SQLServer的所有库
  - SSH密钥
  - 证书
- Image
- Video
- Music




## BOOT DEVICE
---
- Create Windows official MediaCreationTool
- Choose boot device in BIOS 




## BASE
---
只要连接互联网, 现有的Windows11会自动联网安装驱动, 只需要等待更新/重启

趁着没有使用网络代理, 先安装基本工具

- VisualStudioPro2022  
  - Prefer English language 
  - Libs
  - Git?
- Input method, config only `ctrl + space` to switch
- Chrome as default browser
- HHKB
- Logitech
  - Anywhere3 and OptionPlus
  - G309 and G Hub
- 文件夹选项 -> 隐私
  - 取消勾选"显示最近使用的文件"
  - 取消勾选"显示常用文件夹"
- Active WSL2




## Proxy
---
v2rayN(Need DotNet6.0)

注意新安装的Windows, 要确保对时准确, 精准到1分钟以内, 否则代理建立连接失败




## DB
---
真机傻瓜式安装, 之后只用`MySQL 8.1 Command Line Clien`登录用于DCL(Data Control Language)管理  
然后用IDEA(Ultimate)的Import/Export 工具进行数据到导入/导出, ER图也可以导出, [各种DDL(Data Definition Language), DML(Data Manipulation Language)](https://ivanhan0511.github.io/post/sql-mysql/)都可以在IDEA中完成




## VisualStudioPro2022  
---

- 设置项目存储路径, Tools -> Options -> Projects and Solutions -> Locations
- Install VsVim extension
  - Check which directories the VsVim searchs for config files by using the command `:set vimrcpaths?`
  - This is typically the `HOME`, `VIM` or `USERPROFILE` directories
  - Place your `.vimrc` file in one of these directories, restart Visual Studio and VsVim will load those settings
  - You can verify which vimrc file is currently loaded in VsVim by using the command `:set vimrc?`
- 设置字体? 默认DejaVu Sans Mono? 避免像CMD.exe一样无法分辨小写L与数字1
- [Visual Studio 增加菜单来修改字符集编码](https://blog.csdn.net/qq_41868108/article/details/105750175)
- 自动补全: Enter or Tab?
  - Visual Studio 2022是使用Tab进行代码补全的, 但一般习惯回车补全的时候就需要重新设置
  - 个人选择用Tab，原因是Linux默认的补全键是Tab, Notpad++ / Nacicat 也都是Tab补全
  - 具体路径: 工具 –> 选项 –> 文本编辑器 –> C/C++ -> 高级 –> 主动提交成员列表




## PyCharm
---
TODO




## IDEA
---

### Configuration
- Enable `Settings Sync`
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
- RemoteHost `java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar pear-admin-pro-1.11.9-SNAPSHOT.jar`


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




## HBuilderX
---
开发环境部署, 参照[js-vue3.md](https://ivanhan0511.github.io/post/js-vue3/)




## VMWare  
---
VMWare WorkStation Pro, 规划好哪些工具安装在裸机, 哪些安装在VMWare

- 切换安装路径到`D:\VirtualMachines\`盘
- 编辑 -> 首选项 -> 内存 -> 额外内存 "调整所有虚拟机内存使其适应预留的主机RAM"


### Windows
- 电源管理, 从不休眠
- MS SQL Server + SSMS

  Init, export, import refer to[MS SQL Server Opertaions](https://ivanhan0511.github.io/post/sql-sqlserver/)


### Ubuntu Desktop(Deprecated)

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
- Postman(Deprecated)
- XShell/XFtp(转用WSL2)
  - `telnet <IP> <Port>`
- hugo(转用WSL2)
    + 在[HUGO的GitHub](https://github.com/gohugoio/hugo/releases)的releases中下载适用于Windows的文件压缩包
    + 比如放在`C:\Program Files\Git\usr\bin`中, 并添加到环境变量中
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
- Axure RP 9 (Axure Rapid Prototyping)(Deprecated, use Mock in iPad)




## BITCOIN
---
Download Bitcoin wallet from [here](https://bitcoin.org/en/choose-your-wallet) and choose `Bitcoin Core`

Login [this website](https://www.huobi.com/en-us/login/) to trans BTC
