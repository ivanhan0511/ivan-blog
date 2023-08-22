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




<br>

## GRAMMA STYLE

### Naming Convention

**Example 1**
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


**Example 2**

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




### Pointer
举个例子: 
`- (NSString*) foo : (NSString*) param;`

`NSString* name = (NSSting*) anyType;`

所以, 个人喜欢这种风格




## 杂记
---
### Handler
Windows中叫"句柄", Linux中叫"Socket"

- Windows中要打开一个USB设备, 会有一个句柄值, 调用系统底层API时要传这个句柄值
- Windows中要修改一个窗口, 会有一个句柄值, 调用系统底层API时要传这个句柄值
- Windows中要写一个文件, 会有一个句柄值, 调用系统底层API时要传这个句柄值
- Linux中类似, 只不过叫Socket


### Context
比如, C++中, 开线程执行某个类, 要调用线程的一个静态类, 同时要传入类的上下文, 就把类的this传进去

