---
layout: post
title: 使用getopt命令解析shell脚本的命令行选项
description: Linux自带的外部命令getopt同样可以帮助我们解析unix风格的脚本命令行选项，同时还支持长选项
tags: [Linux, shell]
---
在之前的一篇<a href="/parse-shell-options-with-getopts-command/" title="使用getopts命令解析shell脚本的命令行选项">文章</a>中，介绍了如何利用shell内置的getopts命令来帮助我们处理shell脚本选项和参数，其缺点是只能处理短选项，无法处理长选项。下面，本文将要介绍的是getopt命令，它可以同时处理短选项和长选项。

<!--more-->
首先，getopt命令不是一个标准的unix命令，但它在大多数Linux的发行版中都自带了有，如果没有，也可以从<a href="http://software.frodo.looijaard.name/getopt/" title="getopt官网">getopt官网</a>上下载安装。

在getopt的较老版本中，存在一些bug，不大好用，在后来的版本中解决了这些问题，我们称之为getopt增强版。通过-T选项，我们可以检查当前的getopt是否为增强版，返回值为4，则表明是增强版的。

{% highlight bash %}
#getopt -T
#echo $?
4
#getopt -V
getopt (enhanced) 1.1.4
{% endhighlight %}

getopt命令与getopts命令不同，它实际上是通过将参数规范化来帮助我们处理的。具体的用法，如下面的脚本：

{% highlight bash %}
#!/bin/bash

#echo $@

#-o或--options选项后面接可接受的短选项，如ab:c::，表示可接受的短选项为-a -b -c，其中-a选项不接参数，-b选项后必须接参数，-c选项的参数为可选的
#-l或--long选项后面接可接受的长选项，用逗号分开，冒号的意义同短选项。
#-n选项后接选项解析错误时提示的脚本名字
ARGS=`getopt -o ab:c:: --long along,blong:,clong:: -n 'example.sh' -- "$@"`
if [ $? != 0 ]; then
    echo "Terminating..."
    exit 1
fi

#echo $ARGS
#将规范化后的命令行参数分配至位置参数（$1,$2,...)
eval set -- "${ARGS}"

while true
do
    case "$1" in
        -a|--along) 
            echo "Option a";
            shift
            ;;
        -b|--blong)
            echo "Option b, argument $2";
            shift 2
            ;;
        -c|--clong)
            case "$2" in
                "")
                    echo "Option c, no argument";
                    shift 2  
                    ;;
                *)
                    echo "Option c, argument $2";
                    shift 2;
                    ;;
            esac
            ;;
        --)
            shift
            break
            ;;
        *)
            echo "Internal error!"
            exit 1
            ;;
    esac
done

#处理剩余的参数
for arg in $@
do
    echo "processing $arg"
done
{% endhighlight %}

需要注意的是，像上面的-c选项，后面是可接可不接参数的，如果需要传递参数给-c选项，则必须使用如下的方式：

{% highlight bash %}
#./getopt.sh -b 123 -a -c456 file1 file2 
Option b, argument 123
Option a
Option c, argument 456
processing file1
processing file2
#./getopt.sh --blong 123 -a --clong=456 file1 file2  
Option b, argument 123
Option a
Option c, argument 456
processing file1
processing file2
{% endhighlight %}
