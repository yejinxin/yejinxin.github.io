---
layout: post
title: Linux中pam_cracklib.so的minlen和credit参数
description: Linux中PAM的pam_cracklib.so库，设置密码最小长度的参数minlen和lcredit/ucredit/dcredit/ocredit参数的理解。
tags: [Linux]
---

Linux中的PAM(Pluggable Authentication Modules)包含很多有用的模块，其中pam_cracklib.so模块可以配置密码长度复杂度的需求。一般需要同时配置/etc/pam.d/目录中的system-auth和password-auth文件，例如下面

<!--more-->

{% highlight bash %}
password   requisite  pam_cracklib.so  try_first_pass  retry=3  type=  ocredit=2 minlen=10
{% endhighlight %}

限定密码长度主要涉及minlen参数,以及ucredit lcredit dcredit ocredit这四个credit参数，分别表示大写字符、小写字符、数字、其它字符的额外credit值。

这里的minlen=10实际上表示最小分数为10，而不是简单的最小长度为10。密码每有一个任意字符会有一分，另外，ucredit/lcredit/dcredit/ocredit默认值均为1，表示密码中四种字符的类别数，每多一种，就会得到额外的一分。

在这里，`ocredit=2 minlen=10`，也就是说，如果密码全是其它字符的话，最少需要minlen - ocredit = 10-2 = 8位；若密码包含其它字符和小写字符，最少需要minlen - ocredit - lcredit = 10-2-1 = 7位字符，以此类推。

另外ucredit/lcredit/dcredit/ocredit参数的值如果为负数，例如dcredit=-2，则表示密码中最少需要2位数字。

另外，除了密码长度之外，pam_cracklib.so库默认还会做其它方面的简单检查，并且库代码里写死了密码最小长度不能小于6.

<h4>reference</h4>
<http://www.deer-run.com/~hal/sysadmin/pam_cracklib.html>
