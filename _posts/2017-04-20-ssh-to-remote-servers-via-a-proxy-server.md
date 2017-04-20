---
layout: post
title: ssh to remote servers via a proxy server
description: This article describes how to ssh to remote servers via a proxy server, and along with paramiko.
tags: [Linux, ssh, python]
---

In some strictly controlled hosts, ssh access is limitted from only a few hosts. For example, Server1 is not allowed to ssh to Server2, while Server0 does not has that limit. If we want to ssh to Server2 from Server1, we will have to use Server0 as a proxy server. 

In newer SSH versions, we could use `-J` option as below:

<!--more-->

{% highlight bash %}

$ ssh -J Server0:22 Server2

{% endhighlight %}

In older versions, `-J` is not available. In this case, we could use ProxyCommand along with the `-W` option as below:

{% highlight bash %}

$ ssh -o ProxyCommand="ssh -W %h:%p Server0" server2

{% endhighlight %}

For more detail, check this [wiki](https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Proxies_and_Jump_Hosts).

When programming in python with paramiko, here is some example code to use ProxyCommand.

{% highlight python %}
#!/usr/bin/python
sock = paramiko.ProxyCommand('ssh -W %s:%s %s' % (target_server, port, proxy_server))
ssh.connect(host, port, sock=sock)
{% endhighlight %}
