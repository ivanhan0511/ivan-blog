---
title: "C Basic"
date: 2022-05-24T16:51:56+08:00
categories:
- C/C++
- Basic
tags:
- C/C++
- Gramma
keywords:
- c
- c++
---

Hello, world!\n

Some basic knowledge or principles of C/C++

<!--more-->

## C

### Gramma Style

#### `*`
话说在 Objective－C 里声明指针变量时，星号(`*`) 该放到哪个位置，是紧贴变量类型，紧贴变量名还是放它们之间两边用空格，或者全挤在一起？到底还是在思考 C/C++ 中指针变量的声明风格，因为 Objective－C 是 C 的超集。

纯粹讲 Objective－C 的代码风格，我觉得 Google 的 Google Objective-C Style Guide 非常有指导意义。转回来看 Objective－C 声明指针变量时什么风格好些，下面四种都符合语法：


。看显然，第四种风格是最不好看的，就像有人写 SQL 喜欢 “select*from table1" 一样
。第一种风格看似没有倾向性，谁也不依靠，但还是要否定它，仍然未向我们传达较明确的含义。

至于中间两种风格，即 * 是靠向变量类型还是靠向变量名，各有千秋，可以继续参考 Google C++ Style Guide 中关于 Pointer 的规范。其中否定了第一种写法，而且大约是没有提第四种写法的必要，第四种更是不可取的。对于中间两种都能接受，只是说要在你的项目中保持一致的风格。

我为什么会对指针变量的声明想那么多呢，来说说我的理解：

第二种写法，即 NSString* name2; 可以很直观的理解为变量类型就是 NSString* 这么一个指针类型，变量名为 name2，这于我们普通变量的声明方式是吻合的，因为接下来使用变量也是不用带 * 的 name2，而不是使用 *name2。这很好理解，就如 BOOL flag  = YES; flag = NO; 一样自然。

而且类型与*号一体表示类型还体现在参数或方法的返回型或是强制转型的情况，那是没理由把 * 号与类型拆分开的，如

- (NSString*) foo : (NSString*) param;
NSString* name = (NSSting*) anyType;

第三种写法该如何理解呢？*号标记在变量名上，类型应该说是 NSString 的指针类型，反正我是觉得不好理解，不够自然。而唯一能说得通这么写的理由是在一行中声明多个指针变量时：


* 号紧贴变量名的方式好像只有在一行中同时声明多个指针变量时才通得过去，然而很多语言的规范都不推荐在一行中声明多个变量，所以似乎这种写法存在的理由也不够充分。

最后我还是觉得第二种像 NSString* name1;  的写法比较写意，并且也非常合乎规范，我就较喜欢这咱风格，可偏偏很多教材里热衷于第三种，即 NSString *name1 这样的风格。

你呢？也许本身就无足轻重！


## C++

null

