---
title: "CONFIGURATION IN macOS"
date: 2023-08-11T11:05:20+08:00
categories:
- OS
- macOS
tags:
- macOS
keywords:
- config
- macos
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

Some record for configuring macOS
<!--more-->

{{< toc >}}


## BACKUP
macOS作为次要操作系统很多文件都是云存储的, 只要备份好本地的一些文件即可

- 代码: 基本通过git有同步, 其余少量的通过copy完成
- 坚果云文档: 负责与Windows同步
- iCloud: 从Windows同步过来的文档在上传到iCloud
- 本地文档
  - 各个项目的密钥 / 证书
  - SSH密钥, 私人文件等
- 图片
- 视频




## BASE
---
只要连接互联网, 现有的Windows10会自动联网安装驱动, 只需要等待更新/重启

- HHKB
- Logitech Anywhere3 and OptionPlus
- iTerm
- Chrome as default browser
- vim
  {{< tabbed-codeblock "configure" >}}
# 除了安装云存储的`.vimrc`中的Vundle, 还需要手动配置一下smartim
<!-- tab vimrc -->
# 查看macOS默认的输入法是什么, 以决定如下的配置文件中`let g:smartim_default = ''`中的内容
vim ~/.vim/bundle/smartim/plugin/im-select

> com.apple.keylayout.ABC

vim ~/.vimrc

> ...
> "call vundle#begin('~/some/path/here')
> 
> " smart switch input method automaticlly
> Plugin 'ybian/smartim'
> let g:smartim_default = 'com.apple.keylayout.ABC'
> 
> " All of your Plugins must be added before the following line
> call vundle#end() 
> ...
<!-- endtab -->

<!-- tab smartim -->
# 修改 smartim 的延迟
# 当你使用的过程中会发现，按下 ESC 之后，短暂的时间内输入法还没有切换为英文，这种卡顿让输入比较快的键盘手无法忍受
# 解决方法是在 smartim.vim 文件中添加 set timeoutlen=0
vim ~/.vim/bundle/smartim/plugin/smartim.vim
 
> augroup smartim
>   autocmd!
>   ~~set timeoutlen=0~~
>   autocmd VimLeavePre * call Smartim_SelectDefault()
>   autocmd InsertLeave * call Smartim_SelectDefault()
>   autocmd InsertEnter * call Smartim_SelectSaved()
> augroup end

# 这样问题解决。不过这会产生一个小问题，就是自定义的快捷键会失效，比如你定义了 jj 表示 ESC
# 因为没有了延迟，当你输入第二个 j 的时候，Vim 不会把它当做组合。不过不用 jj 也罢。
# 如果很纠结这个，
#   可以在函数  Smartim_SelectDefault() 的第一行添加 set timeoutlen=0
#   在函数 Smartim_SelectSaved() 的第一行添加 set timeoutlen=500
# 不过即使如此，仍会有 500ms 的延迟
<!-- endtab -->
{{< /tabbed-codeblock >}}




## Proxy
---
v2rayN or ClashX

注意对时准确, 精准到1分钟以内, 否则代理建立连接失败


## IDEA
---

### Configuration
- 在各个项目中分别选择Java JDK环境, 不安装Java在裸机
- settsings(Ctrl+Alt+S) -> Editor -> Code Style -> SQL -> 将keywords设置为大写(To upper), and then`ctrl + alt + L`
- 数据库导入时, 右键某个数据库 -> Import/Export -> Restore with 'mysql' -> Path to mysql -> `/usr/local/Cellar/mysql@8.0/bin/mysql`
- 数据库导出时, 右键某个数据库 -> Import/Export -> Export with 'mysqldump' -> Path to mysqldump -> `/usr/local/Cellar/mysql-client8.0/bin/mysqldump`


### Plugin
- IdeaVim
  - 在IDEA中创建`~/.ideavimrc`文件(实际创建在`???.ideavimrc`)
  - 只需在最后一行增加配置引用即可`source ~/.vimrc`
  - [TODO]: "IDEA中自动切换输入法暂时没搞定" "macOS iTerm2倒是通过smartim搞定了, 有的用"
- MyBatisCodeHelperPro
- Redis


### Maven
todo?


### RemoteHost
Remote deploy and remote debug
{{< codeblock "cli" >}}
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar pear-admin-pro-1.11.9-SNAPSHOT.jar
{{< /codeblock >}}




## Git & SSH
---
MINGW64 Git套件

自己添加插件, 来增加额外的的Linux命令

### HUGO
- 在[HUGO的GitHub](https://github.com/gohugoio/hugo/releases)的releases中下载适用于Windows的文件压缩包
- 解压到`C:\Program Files\Git\usr\bin\`目录中
- 再打开GitBash即可使用hugo命令创建静态Blog了

### VimBundle
{{< codeblock "Install VimBundle" "shell" >}}
# It's alse suitable for both Linux and Windows
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim
#:PluginInstall
{{< /codeblock >}}




## HBuilderX
---
- 使用其npm环境? 不安装npm在裸机可以吗?  -> HBuilderX自带node.js环境

看团队用什么吧, 也不打算深究Vue, 能调试个本地前端页面就行了




## DB
---
### MySQL
{{< codeblock "Install" "shell" >}}
brew install mysql@8.0
{{< /codeblock >}}

If forget root password:

{{< tabbed-codeblock "Install" "shell1" >}}
<!-- tab skip-grant-privileges -->
/usr/local/Cellar/mysql@8.0/bin/mysqld_safe --skip-grant-privileges &
<!-- endtab -->
{{< /tabbed-codeblock >}}


### Redis


## Others
---
常用的跨平台工具, 兼容iOS / macOS / Windows / Android

- Chrome
- Postman
- 向日葵Sunlogin
- WireShark
- Typora
- 坚果云
- Office365(Outlook, Excel, Word, PowerPoint)
- WeChat
- 腾讯会议
- 亿图
- Axure
- OmniPlan
- SwiftPlan




## GAME
---
- Minecraft Java Edition
