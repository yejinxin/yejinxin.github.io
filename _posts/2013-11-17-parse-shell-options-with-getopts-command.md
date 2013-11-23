---
layout: post
title: 使用getopts命令解析shell脚本的命令行选项
description: shell的内建命令getopts可以帮助我们解析unix风格的脚本命令行选项
tags: [Linux, shell]
---
标准的Unix命令一般都提供很多选项，使用者通过命令行提供具体的选项和参数，格式如下:

`command -options parameters filename`

getopts是shell内建的一个命令，它可以帮助我们处理命令行选项，使得我们的脚本也可以与unix命令保持风格一致。getopts的用法格式为：

`getopts option_string variable`

具体的例子，直接上脚本：

<!--more-->

{% highlight bash %}
#!/bin/bash 
QUIET=
VERBOSE=
DEVICE=
LOGFILE=/tmp/default

usage()
{
    echo "Usage: `basename $0` [-qv] [-l LOGFILE] -d DEVICE input_file [input_file2...]"
    exit 1
}

[ $# -eq 0 ] && usage

#option_string以冒号开头表示屏蔽脚本的系统提示错误，自己处理错误提示。
#后面接合法的单字母选项，选项后若有冒号，则表示该选项必须接具体的参数
while getopts :qvd:l: OPTION
do
    case $OPTION in
        q)
            QUIET=y
            ;;
        v)
            VERBOSE=y
            ;;
        d)
            DEVICE=$OPTARG        #$OPTARG为特殊变量，表示选项的具体参数
            ;;
        l)
            LOGFILE=$OPTARG
            ;;
        \?)                       #如果出现错误，则解析为?
            usage
            ;;
    esac
done

#$OPTIND为特殊变量，表示第几个选项，初始值为1
shift $(($OPTIND - 1))      #除了选项之外，该脚本必须接至少一个参数
if [ $# -eq 0 ]; then
    usage
fi

if [ -z "$DEVICE" ]; then   #该脚本必须提供-d选项
    echo "You must specify DEVICE with -d option"
    exit
fi


echo "you chose the following options.."
echo "Quiet=$QUIET VERBOSE=$VERBOSE DEVICE=$DEVICE LOGFILE=$LOGFILE"

for file in $@          #依次处理剩余的参数
do
    echo "Processing $file"
done
{% endhighlight %}

以上是getopts命令的用法例子，可以看到，getopts命令式不支持长选项的。需要注意的是，还有另外一个Linux命令getopt，它可以支持长选项，但不是内置的命令，unix版本和Linux版本的用法也不一样，用法见<a href="/parse-shell-options-with-getopt-command/" title="getopt">另一篇文章</a>。
