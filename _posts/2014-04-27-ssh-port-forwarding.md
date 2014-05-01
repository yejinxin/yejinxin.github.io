---
layout: post
title: ssh端口转发
description: 通过ssh端口转发加密数据通信或突破防火墙限制，包括本地端口转发和远程端口转发。
tags: [ssh, Linux]
---

我们知道`ssh -X server`可以将远程服务器的GUI画面转发到本地，实际上除了转发X协议之外，ssh还可以建立任意的端口转发。

<!--more-->

ssh的端口转发实际上是建立了一个安全的ssh隧道，原本通信的数据通过这个ssh隧道来传输，从而保证了数据在传输中的安全。这种端口转发，也可以绕开防火墙对某些端口的限制。

例如在server1上执行`ssh -L 4000:localhost:23 server2`，就可以与server2建立ssh连接，同时将本机(server1)的4000端口的请求通过server2转发到localhost（也就是server2）的23端口。也就是说，在server1上，`telnet 127.0.0.1 4000`访问本机的4000端口就访问到了server2的telnet服务了。
在这里，`-L`选项表示建立本地端口转发，而其中的`localhost`可以换成任何其它server2可以访问得到的地址。

与本地端口转发对应的是`-R`远程端口转发，如果你从server1上不能通过ssh连接到server2，只能从server2中ssh连接到server1，但仍然希望实现上文中的效果，也就是在server1中通过ssh隧道访问server2的telnet服务，那么就需要使用远程端口转发了。
在server2上执行`ssh -R 4000:localhost:23 server1`，即将远端(server1)的4000端口通过自己(server2)转发到localhost的23端口。

此外，上文中本地端口转发和远程端口转发创建的4000端口，默认是只监听127.0.0.1的，也就是只有本机可以访问，使用`-g`选项可以创建监听0.0.0.0的端口，也就是外部也可以访问。
`-N`选项表示不进入远程shell，`-f`选项表示后台运行。

<h4>reference</h4>
<http://www.ibm.com/developerworks/cn/linux/l-cn-sshforward/>
