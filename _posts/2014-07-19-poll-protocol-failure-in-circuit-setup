---
layout: post
title: 记一个rsh报错的解决办法
description: Linux下大并发执行时，rsh报错poll: protocol failure in circuit setup，解决办法是修改xinetd的设置。
tags: [Linux]
---

问题现象：执行到`rsh -l user1 192.168.0.2 "pwd"`命令时报错，报错提示“poll: protocol failure in circuit setup”，但是只有在大并发执行的情况下才会发生。

分析及解决办法：

<!--more-->

根据问题现象，可以推测是系统或服务做了某些资源使用方面的限制。rsh（remote shell）是一个由超级守护进程xinetd代理的服务，查找相关的man帮助并尝试后，发现是xinetd的两个默认设置导致的。

修改/etc/xinetd.conf全局设置，或者，更合理地，应该修改/etc/xinetd.d/rsh配置文件，添加两行配置，然后重启xinetd服务即可。

{% highlight bash %}
    cps = 5000 30       #每秒最多接受5000个连接，超过后禁用服务30秒
    per_source = 5000   #单个IP来源每秒最多接受5000个连接
{% endhighlight %}
