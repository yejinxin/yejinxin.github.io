---
layout: post
title: 处理Vim中粘贴文本的格式问题
description: 介绍处理Vim中从外部粘贴过来的文本的格式问题的方法。
tags: [Vim]
---

在Vim插入模式下，我们可以使用操作系统的复制粘贴命令来将一些外部的文本拷贝至正在编辑的文件中。然而，有时候这样直接粘贴过来的多行文本，格式会乱掉。仔细观察，我们发现其实格式只是缩进的问题。这时候，可以有几种办法：

<!--more-->

1. Visual模式下使用`=`重新格式化：

    首先按`v`键进入Visual模式，然后通过方向键等选中粘贴过来的所有行，再按`=`键Vim就会自动重新格式化这些行的缩进了。这种方法对于粘贴过来的代码比较适用，如果粘贴的文本不是代码，并且还有特定的缩进意义的话，则不大适用。

2. 关闭自动缩进

    粘贴过来的文本缩进乱了的原因，有可能是Vim设置了"set cindent"(C风格的自动缩进)。那么，可以尝试`:set nocindent`暂时关闭自动缩进再粘贴。

3. 粘贴模式

    方法2并不总是管用，因为缩进乱了的原因还可能是Vim开启了根据文件类型自动缩进的插件("filetype plugin indent on")。其实，Vim本身提供了粘贴插入模式("`Insert (paste)`")可以很好的解决问题。

    `:set paste`使得插入模式切换为粘贴插入模式，这时候再进入插入模式后，粘贴外部的多行文本就不会有缩进问题了。粘贴完毕后，可以`:set nopaste`恢复。如果经常需要反复切换的话，还可以设置切换的快捷键，如设置`:set pastetoggle=<F2>`，可以使用F2功能键来切换paste模式。

    另外，还可以通过以下三行设置，使得快捷键切换时在左下方显示当前模式。更多可参考`:help paste`和`:help pastetoggle`。
{% highlight bash %}
nnoremap <F2> :set invpaste paste?<CR>
set pastetoggle=<F2>
set showmode
{% endhighlight %}

另外，除了Windows的`<Ctrl-C>`和`<Ctrl-V>`复制粘贴快捷键之外，还有另外一套快捷键`<Ctrl-Insert>`和`<Shift-Insert>`，这是Windows和Linux（起码在我的RedHat上如此）上都可以使用的。
