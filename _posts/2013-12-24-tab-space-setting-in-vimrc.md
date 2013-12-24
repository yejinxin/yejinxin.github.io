---
layout: post
title: vimrc中关于tab和空格的设置
description: vimrc中关于tab和空格的设置
tags: [Vim]
---

许多情况下，我们编写代码的时候，希望使用空格来代替tab符。我们可以在vimrc中添加`set expandtab`和`set tabstop=4`来将输入的tab键转换为4个空格。

设置了"expandtab"后，如果想输入一个tab而不是空格的话，<!--more-->可以在Insert模式下，输入`<Ctrl-V><Tab>`来键入一个tab符。实际上，在Insert模式下，"`<Ctrl-V>` inserts a literal copy of next charactor"。而在Windows中，可能是`<Ctrl-Q>`而不是`<Ctrl-V>`。

如果想将文件中已经存在的tab符也转换为对应的空格的话，只需要`:retab`即可。"retab"会根据"expandtab"和"tabstop"的设置自动重新扩展tab为对应的空格。

另外，`set noexpandtab`可以关闭自动扩展tab为空格的功能。
