---
title: "C Miscellaneous"
date: 2022-06-09T21:32:20+08:00
categories:
- C/C++
- Miscellaneous
tags:
- C
- Naming Convention
- Pointer *
keywords:
- c
- naming convention
- pointer *
---

Hello, world!\n

Some basic knowledge or principles of C/C++

<!--more-->

## NAMING CONVENTION

### Example 1
- 类型大写首字母驼峰
- 变量和函数名小写下划线
- 常量大写下划线
- 驼峰和下划线不混用
- 需要 export 的类型和函数加对应的驼峰/小写下划线前缀前缀
- 到处都有人人都知道的, 可以缩写, 后面部分尽量不要缩写

个人喜欢用下划线, 也是赞同一个观点: 因为阅读需要空间  
其实驼峰和下划线, 纯看个人信仰

Web 语言也推荐下划线. 因为RFC4343 规定了域名是不分大小写的, 
url 的其他部分就各家有各家的玩法, 那请求参数还是用下划线的比较保险. 
然后你在语言里用驼峰的命名, 又天天处理各种请求参数, 就... 乱了...

作者：luikore
链接：https://www.zhihu.com/question/31498049/answer/120351303
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


### Example 2

`camelCase`

`PascalCase`

`underscore_case`

`_private_underscore_case`

`UPPERCASE_UNDERSCORE`

不要用匈牙利命名法，那是为没有IDE而且不面向对象的时代准备的，你在C++里面遇到的大部分是对象，拿什么前缀都是白费。

camel还是下划线仅仅是一个信仰的差异，Python推荐用下划线是因为有`_`开头是私有的约定，
对Java和C++这样有private和protected的功能的其实没啥用，
而且我到现在仍然觉得下划线比camel丑……
但终究来说，下划线还是camel是个信仰问题。

其他的其实Python的约定挺合理的，类用PascalCase，全局常量用全部大写的下划线分割。
局部变量尽量用比较短的表述。其他语言也都可以参考（而且实际上本来就是参考其他语言的规范制定的）

作者：灵剑
链接：https://www.zhihu.com/question/31498049/answer/120343046
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。




## GRAMMA STYLE

### Pointer

话说在 Objective－C 里声明指针变量时，星号(`*`) 该放到哪个位置，是紧贴变量类型，紧贴变量名还是放它们之间两边用空格，或者全挤在一起？到底还是在思考 C/C++ 中指针变量的声明风格，因为 Objective－C 是 C 的超集。

纯粹讲 Objective－C 的代码风格，我觉得 Google 的 Google Objective-C Style Guide 非常有指导意义。转回来看 Objective－C 声明指针变量时什么风格好些，下面四种都符合语法：


。看显然，第四种风格是最不好看的，就像有人写 SQL 喜欢 `select*from table1` 一样
。第一种风格看似没有倾向性，谁也不依靠，但还是要否定它，仍然未向我们传达较明确的含义。

至于中间两种风格，即 * 是靠向变量类型还是靠向变量名，各有千秋，可以继续参考 Google C++ Style Guide 中关于 Pointer 的规范。其中否定了第一种写法，而且大约是没有提第四种写法的必要，第四种更是不可取的。对于中间两种都能接受，只是说要在你的项目中保持一致的风格。

我为什么会对指针变量的声明想那么多呢，来说说我的理解：

第二种写法，即 NSString* name2; 可以很直观的理解为变量类型就是 NSString* 这么一个指针类型，变量名为 name2，这于我们普通变量的声明方式是吻合的，因为接下来使用变量也是不用带 * 的 name2，而不是使用 *name2。这很好理解，就如 BOOL flag  = YES; flag = NO; 一样自然。

而且类型与*号一体表示类型还体现在参数或方法的返回型或是强制转型的情况，那是没理由把 * 号与类型拆分开的，如

- (NSString*) foo : (NSString*) param;
NSString* name = (NSSting*) anyType;

第三种写法该如何理解呢？`*`号标记在变量名上，类型应该说是 NSString 的指针类型，反正我是觉得不好理解，不够自然。而唯一能说得通这么写的理由是在一行中声明多个指针变量时：


* 号紧贴变量名的方式好像只有在一行中同时声明多个指针变量时才通得过去，然而很多语言的规范都不推荐在一行中声明多个变量，所以似乎这种写法存在的理由也不够充分。

最后我还是觉得第二种像 `NSString* name1;`  的写法比较写意，并且也非常合乎规范，我就较喜欢这咱风格，可偏偏很多教材里热衷于第三种，即 NSString *name1 这样的风格。

你呢？也许本身就无足轻重！



## COMPILE

http://yuanfentiank789.github.io/2018/09/10/GCC%E4%B8%8ECmake%E7%9A%84%E5%85%B3%E7%B3%BB/

1. gcc是GNU Compiler Collection（就是GNU编译器套件），也可以简单认为是编译器，它可以编译很多种编程语言（括C、C++、Objective-C、Fortran、Java等等）。
3. 但是当你的程序包含很多个源文件时，用gcc命令逐个去编译时，你就很容易混乱而且工作量大
4. 所以出现了make工具，make工具可以看成是一个智能的批处理工具，它本身并没有编译和链接的功能，而是用类似于批处理的方式—通过调用makefile文件中用户指定的命令来进行编译和链接的。
5. makefile是什么？简单的说就像一首歌的乐谱，make工具就像指挥家，指挥家根据乐谱指挥整个乐团怎么样演奏，make工具就根据makefile中的命令进行编译和链接的。
6. makefile命令中就包含了调用gcc（也可以是别的编译器）去编译某个源文件的命令。
7. makefile在一些简单的工程完全可以人工手下，但是当工程非常大的时候，手写makefile也是非常麻烦的，如果换了个平台makefile又要重新修改。
8. 这时候就出现了Cmake这个工具，cmake就可以更加简单的生成makefile文件给上面那个make用。当然cmake还有其他功能，就是可以跨平台生成对应平台能用的makefile，你不用再自己去修改了。
9. 可是cmake根据什么生成makefile呢？它又要根据一个叫CMakeLists.txt文件（学名：组态档）去生成makefile。
10. 到最后CMakeLists.txt文件谁写啊？亲，是你自己手写的。
11. 当然如果你用IDE，类似VS这些一般它都能帮你弄好了，你只需要按一下那个三角形

CMake ———> makefile （包含调用gcc的命令）————> make工具 —————> 编译链接源文件

CMake 根据CMakeLists.txt生成乐谱（makefiles） make工具根据乐谱对过个源文件进行 编译和链接即可。


