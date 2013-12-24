---
layout: post
title: Vim中显示不可见字符
description: Vim中如何显示和隐藏不可见字符的设置。
tags: [Vim]
---

在Linux中，`cat -A file`可以把文件中的所有可见的和不可见的字符都显示出来，在Vim中，如何将不可见字符也显示出来呢？当然，如果只是想在Vim中查看的话，可以这样`:%!cat -A`在Vim中调用cat转换显示。这样的做法不便于编辑，其实Vim本身是可以设置显示不可见字符的。

<!--more-->
只需要`:set invlist`即可以将不可见的字符显示出来，例如，会以`^I`表示一个tab符，`$`表示一个回车符等。

或者，你还可以自己定义不可见字符的显示方式：

{% highlight bash %}
set listchars=eol:$,tab:>-,trail:~,extends:>,precedes:<
set list
{% endhighlight %}

最后，`:set nolist`可以回到正常的模式。
