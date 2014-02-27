---
layout: post
title: linux/AIX/HP-UX查看CPU/内存/硬盘/操作系统版本等信息
description: linux/AIX/HP-UX环境下查看CPU/内存/硬盘/操作系统版本等信息的命令
tags: [Linux, unix]
---

查看机器的CPU个数、内存大小、磁盘空间、操作系统版本信息是常见的系统管理任务，下面总结记录了在Linux、AIX、HP-UX环境下执行这些任务的命令和方法：

<!--more-->

1. Linux

    `lscpu`查看CPU信息，`free`/`top`查看内存，`df`查看文件系统信息，`lsblk`查看block设备信息。
    
    操作系统的版本信息，可以查看/etc目录下的issue或lsb-release或redhat-release文件，也可以使用命令查询`lsb_release -a`。

    查看CPU和内存也可以查看对应的文件，如
    
    `grep processor /proc/cpuinfo | wc -l`查看CPU个数

    `grep MemTotal /proc/meminfo | awk '{print $2}'`查看内存大小

2. AIX

    `prtconf`/`nmon`命令可以查看系统配置信息，包括物理CPU和内存等信息。
    
    `bindprocessor -q`查看逻辑CPU，`pmcycles -m`查看逻辑CPU和频率。`vmstat`/`topas`也可查看逻辑CPU和内存信息。
    
    `df`/`lsfs`/`lsvg`/`lslv`等命令可以查看磁盘和文件系统信息，也可查看/etc/filesystems文件。
    
    `oslevel -s`查看操作系统版本。

3. HP-UX

    `print_manifest`命令可以查看系统的各种信息，包括系统型号、机器序列号、处理器类型、处理器数目、CPU类型、总内存大小、内置硬盘数、挂接存储信息、IO信息、安装的软件、网络信息、文件系统信息、内核信息。
    
    `ioscan`可查看CPU信息，`swapinfo`可查看内存和交换区信息。
    
    `lanscan`查看网卡信息，`ifconfig lan0`查看lan0网卡的ip等详细信息。
    
    `bdf`命令检查文件系统的使用情况。
    
    `cat /etc/issue`可以查看操作系统版本。
