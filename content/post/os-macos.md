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
macOS��Ϊ��Ҫ����ϵͳ�ܶ��ļ������ƴ洢��, ֻҪ���ݺñ��ص�һЩ�ļ�����

- ����: ����ͨ��git��ͬ��, ����������ͨ��copy���
- ������ĵ�: ������Windowsͬ��
- iCloud: ��Windowsͬ���������ĵ����ϴ���iCloud
- �����ĵ�
  - ������Ŀ����Կ / ֤��
  - SSH��Կ, ˽���ļ���
- ͼƬ
- ��Ƶ


## BASE
---
ֻҪ���ӻ�����, ���е�Windows10���Զ�������װ����, ֻ��Ҫ�ȴ�����/����

- HHKB
- Logitech Anywhere3 and OptionPlus
- iTerm
- Chrome as default browser


## Proxy
---
v2rayN or ClashX

ע���ʱ׼ȷ, ��׼��1��������, �������������ʧ��


## IDEA
---

### Configuration
- �ڸ�����Ŀ�зֱ�ѡ��Java JDK����, ����װJava�����
- settsings(Ctrl+Alt+S) -> Editor -> Code Style -> SQL -> ��keywords����Ϊ��д(To upper), and then`ctrl + alt + L`
- ���ݿ⵼��ʱ, �Ҽ�ĳ�����ݿ� -> Import/Export -> Restore with 'mysql' -> Path to mysql -> `/usr/local/Cellar/mysql@8.0/bin/mysql`
- ���ݿ⵼��ʱ, �Ҽ�ĳ�����ݿ� -> Import/Export -> Export with 'mysqldump' -> Path to mysqldump -> `/usr/local/Cellar/mysql-client8.0/bin/mysqldump`


### Plugin
- IdeaVim
  - ��IDEA�д���`~/.ideavimrc`�ļ�(ʵ�ʴ�����`~/.ideavimrc`)
  - ������������(��ʱ����Visual Studio��Ҫ������.vimrc, ֻ��Ҫ���һ�ٲ������ü���)

    {{< codeblock ".ideavimrc" "config" >}}
set hlsearch
set incsearch

syntax on
" ���趨�ڲ���״̬�޷����˸���� Delete ��ɾ���س���
set backspace=indent,eol,start

" �޸�leader��Ϊ����
let mapleader=","
set ignorecase  " ���ô�Сд����

" Line number
"set ruler
set number
set relativenumber

" �޸�vim��������
nmap / /\v
vmap / /\v

" ȡ����������
nmap <leader>nh :noh<cr>
    {{< /codeblock >}}

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
MINGW64 Git�׼�

�Լ���Ӳ��, �����Ӷ���ĵ�Linux����

### HUGO
- ��[HUGO��GitHub](https://github.com/gohugoio/hugo/releases)��releases������������Windows���ļ�ѹ����
- ��ѹ��`C:\Program Files\Git\usr\bin\`Ŀ¼��
- �ٴ�GitBash����ʹ��hugo�������̬Blog��

### VimBundle
{{< codeblock "Install VimBundle" "shell" >}}
# It's alse suitable for both Linux and Windows
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim
#:PluginInstall
{{< /codeblock >}}




## HBuilderX
---
- ʹ����npm����? ����װnpm�����������?  -> HBuilderX�Դ�node.js����, ���о�

���Ŷ���ʲô��, Ҳ�������Vue, �ܵ��Ը�����ǰ��ҳ�������




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
���õĿ�ƽ̨����, ����iOS / macOS / Windows / Android

- Chrome
- Postman
- ���տ�Sunlogin
- WireShark
- Typora
- �����
- Office365(Outlook, Excel, Word, PowerPoint)
- WeChat
- ��Ѷ����
- ��ͼ
- Axure
- OmniPlan
- SwiftPlan




## GAME
---
- Minecraft Java Edition
