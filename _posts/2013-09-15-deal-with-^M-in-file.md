---
layout: post
title: 删除文件中的^M符号
description: 在Linux中删除windows文件里的^M符号
tags: [Linux]
---

Windows和Linux的文本文件换行方式不同，有时候将windows的文本文件上传到Linux中，会出现一些问题。

Linux下`cat -A filename`或`cat -v filename`就可以看到Windows文件中多出的`^M`符号。

下面的方法可以去除`^M`。注意：以下命令中的`^M`都是通过ctrl+v然后ctrl+m来输入的。
<!--more-->

1. 使用sed：
    `sed -i 's/^M//g' filename`

2. 使用tr，其中`\r`可用`^M`或`\015`代替
    `tr -d "\r" < filename > newfilename`
	
3. 使用dos2unix
    `dos2unix filename`

4. 在vi中使用替换命令
    `:%s/^M//g`
