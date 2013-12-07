---
layout: post
title: Bash和Ksh中查找和执行历史命令
description: 在Linux和Unix的Bash和Ksh两种shell中，如何搜索和执行历史命令
tags: [Linux, shell]
---

在Linux的Bash环境中，可以比较方便的搜索和执行最近输入过的命令，其实，在Unix的Ksh环境中，也可以做到，不过方法有所不同。总结这两种shell环境下相关的历史命令操作如下：

<!--more-->

{% highlight text %}
#Bash
history          #显示历史命令
history 10       #显示最近的10条历史命令
!!               #显示并执行上一条历史命令
!27              #显示并执行第27条历史命令
!ps              #显示并执行上一条以ps开头的命令
!ps:p            #显示上一条以ps开头的命令
                 #ctrl + R 可以反向搜索包含特定字符串的历史命令，重复输入可以继续向前搜索
{% endhighlight %}

{% highlight text %}
#Ksh
history              #显示历史命令
history 1000 1050    #显示第1000到1050条的历史命令
history 1000         #显示第1000以后的历史命令
history -50          #显示最近的50条历史命令
r                    #显示并执行上一条历史命令
r 27                 #显示并执行第27条历史命令
r ps                 #显示并执行上一条以ps开头的命令
{% endhighlight %}

另外，在Ksh中，按`ESC+\`同样可以补全文件名。而按`ESC+K`则可以显示上一条命令，这时候，可以继续按K或J前后翻滚历史命令，甚至可以使用类似vi里的一些命令如`x`删除、`r`替换等操作。
