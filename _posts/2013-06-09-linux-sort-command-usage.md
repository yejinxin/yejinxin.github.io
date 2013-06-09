---
layout: post
title: Linux中sort命令用法
description: Linux中sort命令的相关用法。
tags: [Linux, 排序]
---

sort命令时Linux中一个非常有用的命令，它常见的参数和意义如下：

+ `-c` `--check` 只检查文件是否已排序，而不进行排序。

+ `-m` `--merge` 合并已经排好序的文件。

+ `-o` `--output=FILE` 将排序结果保存到指定文件中，通常用于将结果保存到原文件中（这种情况用输出重定向不能完成）。

<!--more-->

+ `-s` `--stable` sort命令默认是不稳定的排序，此选项使排序结果稳定。

+ `-u` `--unique` 对排序键值相同的行只保留一行。默认情况下，`sort | uniq`等效于`sort -u`，但是在指定了排序key时则不同。

+ `-b` `--ignore-leading-blanks` 忽略前导空格。

+ `-R` `--random-sort` 随机排序，每次运行的结果均不同。

+ `-n` `--numeric` 按字符串的数值来比较。

+ `-r` `--reverse` 倒序排序。

+ `-t` `--field-separator=SEP` 设置字段分隔符，默认的分隔符是non-blank to blank transition。

+ `-f` `--ignore-case` 开启此选项则忽略大小写进行比较。

    然而，有可能在Linux中，默认情况下已经是忽略大小写的排序了，这是因为[locale](http://wiki.ubuntu.org.cn/Locale)的原因。sort命令的帮助中有如下警告：

    <pre>*** WARNING *** The locale specified by the  environment  affects  sort
       order.  Set LC_ALL=C to get the traditional sort order that uses native
       byte values.</pre>

    sort命令会根据当前的locale来决定一些字符串的比较结果，这样，在不同的机器中，即使输入数据相同，也有可能因为locale不同而导致排序结果不同。因此，如果排序结果不符合预期的话，不妨`echo $LC_ALL`看看是否为C，如果不是，可以执行`export LC_ALL=C`。

+ `-k` `--key=KEYDEF` 指定排序key，格式为F[.C][OPTS][,F[.C][OPTS]]。

    例如`sort -t: -k2,2n -k4,4nr input.txt`表示以冒号为分隔符，首先以数值方式比较第二列，如果相同，再以逆序数值方式比较第四列，列数是从1开始的。而`sort -k2.1,2.2 input.txt`表示比较第二列的前两个字符来排序。

    key的结束位置默认为行尾，因此`sort -k2 input.txt`表示的是比较第二列开始到行尾的字符串，而不仅仅是第二列。因此，如果需要，尽量定义多个key，而不是一个跨列的key。

<h4>reference</h4>
<http://www.skorks.com/2010/05/sort-files-like-a-master-with-the-linux-sort-command-bash/>
