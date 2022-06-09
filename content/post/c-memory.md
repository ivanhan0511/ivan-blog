---
title: "C Memory"
date: 2022-06-09T16:55:06+08:00
categories:
- C/C++
- Memory
tags:
- C Memory
keywords:
- c
- c++
- memory
clearReading: true
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

The core in many compute languages is memory.

There are some records about memory.

<!--more-->

{{< toc >}}

## BASIC

一、预备知识—程序的内存分配
一个由C/C++编译的程序占用的内存分为以下几个部分
1、栈区（stack）— 由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其
操作方式类似于数据结构中的栈。
2、堆区（heap） — 一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回
收 。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表，呵呵。
3、全局区（静态区）（static）—，全局变量和静态变量的存储是放在一块的，初始化的
全局变量和静态变量在一块区域， 未初始化的全局变量和未初始化的静态变量在相邻的另
一块区域。 - 程序结束后由系统释放。
4、文字常量区 —常量字符串就是放在这里的。 程序结束后由系统释放
5、程序代码区—存放函数体的二进制代码。
————————————————
版权声明：本文为CSDN博主「luckystar_sai」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33573235/article/details/88920069


