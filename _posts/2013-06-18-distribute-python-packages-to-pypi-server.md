---
layout: post
title: 发布python的包至pypi服务器
description: 发布自己的python代码到pypi服务器上，使得所有人都能使用pip或easy_install来安装使用。
tags: [python]
---
使用pip或easy_install可以管理和安装python的package包，实际上它们都是从[pypi服务器](https://pypi.python.org/pypi)中搜索和下载package的。目前在pypi服务器上，有超过三万多个package，同时还允许我们将自己的代码也上传发布到服务器上。这样，世界上的所有人都能使用pip或easy_install来下载使用我们的代码了。

具体步骤如下：

<!--more-->

<ul>
    <li>
    <p>首先创建项目文件和setup文件。</p>
    <p>目录文件结构如下：</p>
{% highlight bash %}
project/
    simpletest/
        __init__.py
        test.py
    setup.py
{% endhighlight %}
    <p>假设项目文件只有一个simpletest包，里面有一个test.py文件。</p>
    <p>创建的setup.py文件格式大致如下，其中，install_requires字段可以列出依赖的包信息，用户使用pip或easy_install安装时会自动下载依赖的包。详细的格式参考<a href="http://docs.python.org/2/distutils/setupscript.html">文档</a>。</p>
{% highlight python %}
from setuptools import setup, find_packages

setup(
    name = 'simpletest',
    version = '0.0.1',
    keywords = ('simple', 'test'),
    description = 'just a simple test',
    license = 'MIT License',
    install_requires = ['simplejson>=1.1'],

    author = 'yjx',
    author_email = 'not@all.com',
    
    packages = find_packages(),
    platforms = 'any',
)
{% endhighlight %}
    </li>
    <li>
    <p>然后将代码打包。</p>
    <p>打包只需要执行<code>python setup.py xxx</code>命令即可，其中xxx是打包格式的选项，如下：</p>
{% highlight bash %}
# 以下所有生成文件将在当前路径下 dist 目录中
python setup.py bdist_egg # 生成easy_install支持的格式 
python setup.py sdist     # 生成pip支持的格式，下文以此为例
{% endhighlight %}
    </li>
    <li>
    <p>发布到pypi。</p>
    <p>发布到<a href="https://pypi.python.org/pypi">pypi</a>首先需要<a href="https://pypi.python.org/pypi?%3Aaction=register_form">注册</a>一个账号，然后进行如下两步：</p>
    <ol>
        <li>注册package。输入<code>python setup.py register</code>。</li>
        <li>上传文件。输入<code>python setup.py sdist upload</code>。</li>
    </ol>
    </li>
    <li>
    <p>安装测试</p>
    <p>上传成功后，就可以使用pip来下载安装了。</p>
    <p>另外，pypi还有一个<a href="https://testpypi.python.org/pypi">测试服务器</a>，可以在这个测试服务器上做测试，测试的时候需要给命令指定额外的"-r"或"-i"选项，如<code>python setup.py register -r "https://testpypi.python.org/pypi"</code>, <code>python setup.py sdist upload -r "https://testpypi.python.org/pypi"</code>, <code>pip install -i "https://testpypi.python.org/pypi" simpletest</code>。</p>
    <p>发布到测试服务器的时候，建议在linux或cygwin中发布，如果是在windows中，参考<a href="http://docs.python.org/2/distutils/packageindex.html#the-pypirc-file">文档</a>，需要生成.pypirc文件，参考<a href="{{ site.url }}/create-empty-filename-in-windows/">另一篇博文</a>。</p>
    </li>
</ul>


<h4>reference</h4>
<http://liluo.org/blog/2012/08/how-to-create-python-egg/>

<http://blog.jkey.lu/2013/04/11/create-python-egg/>

<http://docs.python.org/2/distutils/index.html>