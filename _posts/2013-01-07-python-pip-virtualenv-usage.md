---
layout: post
title: Python的pip和virtualenv使用心得
description: Python的pip和virtualenv使用心得。
tags: [python]
---

pip可以很方便的安装、卸载和管理Python的包。

virtualenv则可以建立多个独立的虚拟环境，各个环境中拥有自己的python解释器和各自的package包，互不影响。

pip和virtualenv可以很好的协同工作，同时使用这两个工具非常方便。
<!--more-->
一般先安装pip，安装好后，`pip install virtualenv`就可以自动从网上下载并安装virtualenv了。然后`virtualenv env1`就可以创建一个名为env1的虚拟环境了，进入这个虚拟环境后，再使用`pip install`安装其它的package就只会安装到这个虚拟环境里，不会影响其它虚拟环境或系统环境。

当需要将虚拟环境env1迁移或复制到另一个虚拟环境（可能不在同一台机器上）env2时，首先仍然需要在目的机器上安装pip和virtualenv，然后采用以下方法之一安装其他的package：

1. 直接将env1里的文件全部复制到env2里，然后修改涉及路径的文件。此种方法可能正常使用，但显然不是好办法。

2. 进入原虚拟环境env1，然后执行`pip freeze > requirements.txt`将包依赖信息保存在requirements.txt文件中。然后进入目的虚拟环境env2，执行`pip install -r requirements.txt`，pip就会自动从网上下载并安装所有包。

3. pip默认会从[pypi服务器](http://pypi.python.org/simple)下载包的安装文件，如果目的机器无法连外网，则可以采用以下办法:

    1. 搭建自己的pypi服务器。专业的，可以使用第三方的软件包来搭建一个完整的pypi镜像服务器，参考<http://www.worldhello.net/2011/03/14/2357.html>。更快速的方法只需要一条命令`python -m SimpleHTTPServer`即可完成搭建服务器，具体的目录结构可参考原pypi服务器，简而言之，就是把安装文件打包放入目录即可。搭建好服务器之后，在目的虚拟环境中，就可以使用pip来安装了，命令如：`pip install -i http://127.0.0.1:8000/ -r requirements.txt`

    2. 如果你实在不想搭建pypi服务器，也有办法。首先将所有包的安装文件下载下来，可以手动下载，也可以使用pip，如`pip install -d /path/to/save/ -r requirements.txt`，然后自己修改requirements.txt文件，将每一行改成对应的包的安装文件的路径。最后在目的虚拟环境中使用pip安装，如`pip install -r requirements.txt`即可。

    3. 还有一种途径，就是pip提供的bundle选项。首先执行`pip bundle MyEnv.pybundle -r requirements.txt`，将生成一个MyEnv.pybundle文件，该文件夹包含所有包的安装文件（注意必须后缀名必须是.pybundle），默认是重新从pypi服务器下载安装文件的，如果愿意，也可以利用3.1中的方法，指定本地的pypi服务器。然后在目的虚拟环境中执行`pip install MyEnv.pybundle`即可。

4. `pip install`还有许多有用的选项，如`--download-cache=DIR`可以指定下载安装文件时缓存至DIR路径，下次需要时则直接读取缓存文件。具体选项可以执行`pip help install`得到详细信息。

5. 另外，还可以将自己的包上传至pypi服务器，分享给所有人。具体可参考<http://guide.python-distribute.org/creation.html>和<http://matrix.42qu.com/10734668>。

本文出自[{{ site.title }}]({{ post.url }})，转载请保留出处
