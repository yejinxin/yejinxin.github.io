---
layout: post
title: 虚拟机关机后自动关闭宿主机
description: 在Linux和windows中，实现在某些进程结束后，自动关机的命令
tags: [windows, Linux]
---

在Linux中，`shutdown`命令可以方便地实现定时关机的任务，要想让系统在某些后台进程结束后再自动关机也不难：
{% highlight bash %}
watch 'ps -p 2421 || shutdown -h now && exit'
{% endhighlight %}

<!--more-->
其中，2421为要等待的进程ID，使用`watch`命令就可以每隔2秒执行引号中的命令，这比使用crontab要简单。

而在windows中，要想实现进程结束后关机，就略微麻烦了——可以将下面的命令保存为一个.bat文件执行：
{% highlight bat %}
@echo off
:LOOP1
TASKLIST /FI "PID eq 2421"|findstr 2421 
if %errorlevel% NEQ 0 (
    shutdown /s
    pause
)
ping 127.0.0.1 -n 3 -w 1000 > nul
goto LOOP1
{% endhighlight %}

其中，`ping 127.0.0.1 -n 2 -w 1000 > nul`是用于延时2秒,相当于Linux中的`sleep 2`命令。

回到本文的题目，有时候在虚拟机中执行的后台任务耗时较久，想在任务执行完毕后关闭虚拟机(Linux)和宿主机(windows)。

这种情况下，首先，在Linux使用上文的命令设置任务完成后自动关机；然后，需要设置自动关闭windows，以VMware Workstation 9.0为例，可以发现，在虚拟机对应的本地进程是"vmware-vmx.exe"，虚拟机关机完成后，该进程也结束。因此，只需要使用任务管理器或`tasklist`命令确定"vmware-vmx.exe"进程的进程号后，按照上文中的方法即可实现。

