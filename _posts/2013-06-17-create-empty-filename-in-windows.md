---
layout: post
title: Windows下创建只有扩展名(.pypirc)的文件
description: 在Windows下创建没有文件主名只有扩展名，例如".pypirc"这样的文件。
tags: [windows]
---

在Windows操作系统中，文件名由文件主名和扩展名组成，扩展名用于区分文件类型，主名和扩展名中间用圆点隔开。而在Unix和Linux系统中，则不存在扩展名的说法，文件名可以随意更改而不影响文件类型。

在*nix系统中，有不少文件是只有“扩展名”而没有“文件主名”的，例如`.bashrc`、`.gitconfig`等。有时候在Windows系统中也需要这样的文件，例如你想在Windows系统中发布package到其它的[pypi服务器](https://pypi.python.org/pypi)，需要有一个[.pypirc](http://docs.python.org/2/distutils/packageindex.html#pypirc)文件。

<!--more-->

+ 实际上，windows系统是支持这样的文件的，可以在记事本或其它编辑器里创建，保存的时候保存类型选择为“所有文件(*.*)”即可。

+ 但是，在资源管理器中是无法将一个已有的文件重命名为只有“扩展名”的文件的，会提示“必须键入文件名”错误。

    + 在这种情况下， 可以切换到cmd命令提示符，使用rename命令来改名，不会有错误提示。

    + 或者在资源管理器中，将文件重命名为想要更改的名字再加一个圆点。例如，想要更改为`.pypirc`，则右键重命名为`.pypirc.`，确认后最后面的多余的圆点会自动消失。


<h4>reference</h4>
<http://serverfault.com/questions/22626/rename-files-to-empty-filename-in-windows-vista>
