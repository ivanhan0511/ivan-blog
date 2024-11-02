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

## I. IDE
---
同时用 vim 、CLion 、vscode 开发 C++用了挺长一段时间，分享一下自己的经验。
vim：用起来最顺手的，会有一点学习曲线陡峭的问题。vim 相对来说是投入产出比最高的了，一次学习，终生受益。VIM 对各种 C++项目的适应性是最好的，使用 YouCompleteMe 配合 clangd ，无论是 cmake 项目还是 makefile 项目，统统生成 compile_commands.json 之后就可以无缝衔接了。 同时配合上 gtags-cscope 之后，基本的 Find References 也比较丝滑了。 至于 easymotion 、multicursor 都是 vim 上必备的了。可以看看 skywind3000 讲解如何使用 vim 搭建 C++开发环境的，他应该主要是 Windows 环境比较多，在 Mac 上也类似的，但要讲兼容性，还是整个 ubuntu 的 vm 比较容易一些。

vscode：clangd + microsoft cpp ，这个具体可以看 clangd 官方网站的介绍的 best pracitce( https://clangd.llvm.org/installation)，里面对于主流的编辑器如何配置都有详细介绍。 vscode 安装 vim 插件之后，自带了 easymotion 等一些比较好用的 vim 插件，相对来说比 JetBrains 家的 IdeaVim 要更加全面一些，但是实际用起来似乎没有 IdeaVim 那么稳定。vscode 的好处是可定制性非常强，和 vim 一样对于 cmake 和 makefile 项目都能比较好的支持，代码提示、静态代码检查这些基于 clangd 干的事也都能干得好。 通过精心配置之后，vscode 也可以做到几乎完全用键盘操作了。 如果用普通鼠标的话，vscode 的滚动比例需要调整一下，不然在 mac 下鼠标滚动非常神经质，用 trackpad 和 magic mouse 的话是感觉不到这个滚动问题的。
vscode 的最大优势个人认为是 remote 模式确实做得非常棒，比 Clion 要好很多很多很多。。。。。， 但是 vscode 时不时会有一些小问题，比如撤销编辑这个操作，cmd+z 和 vim 里的 u ，感觉有冲突，总是不小心就撤销错了。

clion： 它其实也是基于 clangd 来搞的，相对来说更加开箱即用得多，几乎是 0 配置就可用了，代码编辑这个功能本身做的个人认为比 vscode 要流畅丝滑很多，插件的成熟度比 vscode 高一些，但是数量少很多，容易有一些功能就不好实现。但是也有一些比如 multi-highlight 这种好用的插件，vscode 没有对应的。clion 的代码索引做得会比较好用一些，跟 idea 那个体验有一拼了，如果项目是完全用 cmake 管理，没有 extenral_project 的话，那么 clion 的体验是非常棒的。
clion 的缺点很明显也很要命，首先是 remote development 用起来一言难尽，与 vscode 的 remote 相比简直连 beta 都算不上，即使把服务器上 clion 的内存堆大小开到 12G 以上，也没什么实质的提升，稍微大一点的项目，动不动就索引失效了，然后卡半天建索引，体验真的一言难尽。
clion 的另一个缺点就是只能对 cmake 管理的项目有比较好的体验，一旦这个项目混合了 cmake 和 makefile ，或者 cmake 里面有 external_project ，那代码索引的能力就非常捉急了。
忍了很久 clion 的这些问题，最后放弃使用了。 有些 C++项目在 Mac 上比较难搞，所以 remote 是刚需。


总体来说，vim 有一定的学习曲线，需要投入一些时间，效率提升地反馈没有那么快。但是还是建议一定要学习和使用 vim ，因为即使 vscode 、clion 这些都在安装 vim 插件之后才有更高的效率。 另外配置 vim 插件的过程中，你能够对 c++开发过程中的很多细节有更好的理解，比如 clangd 的代码提示怎么才能生成，clang-tidy 是怎么回事，clang-format 又是怎么回事，分别是怎么配置的，vim 和 git 怎么集成的，git 内部的数据结构大概是什么样的，等等。
这样在使用 vscode 、clion 的时候，对于很多问题就会有更加透彻的理解，而不是完全只能当成一个黑盒使用。

最后，现在主力使用的是 vscode （ 70%时间），辅助使用 vim （ 25%时间），很少使用 clion （ 5%时间）。vscode 上同时使用 clangd 、clang-format （为了格式化 proto ，clangd 自身好像没法 format proto 文件）、microsoft cpp 、vim 等等插件，平时主要是用 vscode 的 remote 模式，代码和环境都放服务器上，ubuntu 、centos 、rocky 都有，虚拟机、物理机、Docker Container 都有，总体来说体验是比较好的。
vim 效率跟 vscode 差不多，由于 vscode 偶尔有些小毛病，vim 甚至可能还要效率更高，但是 vim 的主要问题是 YouCompeteMe 这个插件在一些老的系统上编译起来比较麻烦，每个机器都去配置环境也是个挺浪费时间的事，vscode 就省心很多，自动安装就行了。

啰啰嗦嗦说了很多，希望对 OP 能有帮助：）


