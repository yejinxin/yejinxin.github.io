---
layout: post
title: Shell中实现类似Python中的字符串相乘
description: 在Shell环境中实现类似Python中的字符串相乘
tags: [shell]
---

在Python中，字符串和数字可以相乘，表示n个字符串相连，如`"-"*20`表示包含20个折线的字符串。在shell中，要想实现类似的结果，可以有如下方法：

<!--more-->

1. `seq -s "sep" 100 | tr -d "[:digit:]"`，实现"sep"*100
2. `printf "%100s" | tr " " =`，注意tr只能替换单字符，所以这种方法只适用于单字符
3. `printf 'sep%.0s' {1..100}`,这里，sep%.0s表示不管实际传什么参数printf都直接打印sep，{1..100}展开后相当于打印100次
4. `str=$( printf "%100s" ); echo ${str// /sep}`，其中，`${str// /sep}`表示替换str中所有的空格为sep 
