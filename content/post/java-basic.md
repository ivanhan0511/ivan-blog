---
title: "Java Basic"
date: 2022-09-14T17:05:15+08:00
categories:
- Java
- Basic
tags:
- Java
keywords:
- java
- basic
clearReading: true
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

Java basic knowledge.
<!--more-->

{{< toc >}}

## BASIC

### 类型对比
String[]

ArrayList




## 泛型

{{< codeblock "A.java" "https://www.163.com" >}}
public class A {}

public static <A> List<A> asList(A... a);

public static List<A> asList(A... a);
{{< /codeblock >}}

<T>是申明T为泛型，以区别于类名

即：<T> List<T> 中，第一个T是告诉大家，T不是类T.class，而是泛型T
（如果只写List<T>则编译器以为是类T.class，如果不存在T.class类，则报错）。

案例1中A为“泛型A”，参数可以传入任何类型对象的数组；案例2则不是，其中A为"类A"，参数只能传入"类A"的对象的数组。

{{< alert info >}}
案例1中使用泛型绝不是因为要使参数可以传入任意类型，如果仅仅是这样，直接用Object就可以了。
用泛型是因为可以使该方法的返回值成为一个指定类型的集合，这样再次使用该集合的时候就有一个明确的类型了，
这使的在将来该类型发生改变的时候编译器会报错，提醒你做相应的修改，而不是让问题暴露在运行阶段。
{{< /alert >}}

