---
title: "C++"
date: 2024-11-02T10:46:00+08:00
categories:
- C/C++
tags:
- C/C++
keywords:
- c
- c++
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

This is a custom summary and does *NOT* appear in the post.
<!--more-->

{{< toc >}}

## I. EVIRONMENT

关于 C++ IDE,
- VisualStudio is more traditional, but it's a IDE
- VSCode is more fassion, but it's a editor
- CLion is not better than VisualStudio


### A. VisualStudioPro2022

- 设置项目存储路径: Tools -> Options -> Projects and Solutions -> Locations, 指定Project location: `D:\oopt\`
- 将Tabs 改为 Spaces
- 显示行号: Tools -> Options -> Text Editor -> All Languages -> General -> choose "line numbers"
- 安装`VsVim`插件, 并增加`.vsvimrc`专用的配置文件(比自用的`.vimrc`少了`Vundle`等配置)
{{< blockquote >}}
The VsVim extension for Visual Studio supports a lot of the Vim commands that are commonly used. A Vim user should feel comfortable navigating and editing in Visual Studio using this extension. 
If you want to save your Vim settings or load your Vim settings in VsVim, you can do that from a vimrc file.

VsVim looks for a file named `.vsvimrc`, `_vsvimrc`, `.vimrc` or `_vimrc` to load Vim settings. You can put your favorite Vim settings into this file and use them with VsVim. 
You can check which directories that VsVim looks for this file in by using the command `:set vimrcpaths?`. This is typically the `HOME`, `VIM` or `USERPROFILE` directories. 
Place your vimrc file in one of these directories, restart Visual Studio and VsVim will load those settings. You can verify which vimrc file is currently loaded in VsVim by using the command `:set vimrc?`
{{< /blockquote >}}
- 设置字体? 默认DejaVu Sans Mono? 避免像CMD.exe一样无法分辨小写L与数字1(VisualStudioPro2022没有再设置了)
- [增加菜单来修改字符集编码](https://blog.csdn.net/qq_41868108/article/details/105750175): Tools -> customize... -> Commands tab -> Choose "File" and "Add command..." -> File(新弹窗) -> Advanced Save Options...
- 自动补全: Enter or Tab?
  - Visual Studio 2022是使用Tab进行代码补全的, 但一般习惯回车补全的时候就需要重新设置
  - 个人选择用Tab，原因是Linux默认的补全键是Tab, Notpad++ / Nacicat 也都是Tab补全
  - 具体路径: 工具 –> 选项 –> 文本编辑器 –> C/C++ -> 高级 –> 主动提交成员列表(Use Tab to commit...)


### B. CMake等等
        
foo-bar
Foo-bar

