---
title: "Python Basic"
date: 2024-01-18T15:26:10+08:00
categories:
- Python
- Basic
tags:
- Python
keywords:
- python
---

Some basic records of Python
<!--more-->

## I. ENVIRONMENT
---

### A. Backup and Sync
同步之后, 大部分的配置和插件都可以同步下来, 以下仅作备考

#### 1. Configuration
- Enable Backup and Sync -> `Enable Backup and Sync`
- 在各个项目中分别选择Anaconda的环境, 不安装Python在裸机
- Settsings -> Editor -> Code Style -> SQL -> General, 将`keywords` 和 `Built-in types`设置为大写(To upper)


#### 2. Plugins
- IdeaVim
  - 在IDEA中找到(无则创建)`~/.ideavimrc`文件(在`C:\Users\Ivan\.ideavimrc`可以找到)
  - 只需在最后一行增加配置引用即可`source //wsl.localhost/Ubuntu/root/.vimrc`, 已经可以使用WSL中的配置文件了
  - 配置完, 需要退出并重新打开PyCharm
  - IDEA的vim插件, 从插入模式退出到普通模式后, 自动切换为英文输入法, 暂时没搞定, Windows VisualStudio的VsVim插件倒是自动有这个配置, 暂时没琢磨明白
- Redis


### B. Anaconda
