---
layout: post
title: 标量类型(scalar types)初始化
description: C++中标量类型(scalar types)初始化问题。
tags: [C++]
---

在C/C++中，以下的几种字符串指针初始化的方式都是合法的。
{% highlight cpp %}
char str1[10]="welcome";  
//char str2="welcome"; //error: invalid conversion from 'const char*' to 'char'  
char str3[10]={"welcome"};  
char *str4="welcome";  
char *str5={"welcome"};
{% endhighlight %}
<!--more-->

看起来，str3和str5貌似是会编译不过的，实则不然。

在C++11标准中，提供了与C语言一致的初始化功能。对于[标量类型](http://book.51cto.com/art/201206/341056.htm)的初始化，大括号是可选的，即`int a=0;`和`int a={0};`是等价的。

str5是指针类型，属性标量类型，因此其初始化是合法的。对于str3，是数组类型，不属于标量类型，不过编译器选择支持类似的初始化方式，或许是为了统一方便用法吧。

<h4>更多参考</h4>
[C++初始化与赋值](http://www.cnblogs.com/chio/archive/2008/10/06/1305145.html)

[C++ 11新特性](http://blog.csdn.net/doctorsc/article/details/6777849)

[scalar types 初始化](http://dhb52.blog.163.com/blog/static/74731559200783103833602/)

本文出自[{{ site.title }}]({{ page.url }})，转载请保留出处