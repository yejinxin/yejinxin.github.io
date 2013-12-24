---
layout: post
title: 将文件中的tab转换为space空格
description: Linux中将文件中的tab转换为space空格的几种方法。
tags: [Linux]
---

在之前的一篇[文章](/tab-space-setting-in-vimrc)中提到了在Vim中将tab符转换为空格的方法，其实，和[处理文件中的`^M`的方法](/deal-with-^M-in-file)类似，在Linux中还有其它的方法可以将文件中的tab和space相互转换。

<!--more-->

1. 使用sed：

    `sed -i 's/^I/    /g' filename`，其中`^I`是在命令行中输入`<Ctrl-V><Tab>`来键入的，此方法将所有tab替换为4个空格。

2. 在vi中使用替换命令

    `:%s/^I/    /g`，同样是输入`<Ctrl-V><Tab>`来键入`^I`，同样将所有tab替换为4个空格。

3. 使用expand和unexpand命令
{% highlight bash %}
expand -t 4 filename > newfile    #将文件中的tab扩展为4个空格。
unexpand -t 4 filename > newfile  #将文件中的空格还原为tab。
{% endhighlight %}
