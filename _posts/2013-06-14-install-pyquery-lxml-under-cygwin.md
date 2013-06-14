---
layout: post
title: cygwin下安装pyquery和lxml
description: cygwin下安装pyquery和lxml的解决方法。
tags: [python, xml, cygwin]
---

[pyquery](https://pypi.python.org/pypi/pyquery)是一个可以让你使用类似jquery的语法来查询和操作xml的python模块，其内部使用[lxml](http://lxml.de/)来操作xml和html。

可以使用`pip install pyquery`或者`easy_install pyquery`来安装pyquery模块，然而，在cygwin环境下，可能会因为lxml或其它相关的库没有正确安装而出现一些错误。

错误信息可能会提示请确保libxml2和libxslt开发环境库已正确安装。因此，需要用cygwin的setup.exe将以下包安装上：

<!--more-->

libxml2, libxml2-devel, libxslt, libxslt-devel, python-libxml2, python-libxslt


<h4>reference</h4>
<http://anythingsimple.blogspot.com/2010/04/install-lxml-on-cygwin.html>
