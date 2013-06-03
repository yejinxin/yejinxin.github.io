---
layout: post
title:  JQuery中区分模拟点击事件和用户点击事件
description:  JQuery中区分模拟点击事件和用户点击事件。
tags: [jquery, js]
---

使用JQuery，我们可以自定义了事件函数，如：
{% highlight js %}
$("#foo").click(function(e){  
    //do work  
});
{% endhighlight %}

我们还可以模拟点击等事件的发生，如`$("#foo").click()`
<!--more-->

有的时候我们需要区分这种模拟事件和真正的用户事件。

1. 方法一

    我们可以通过传递的参数e来判断是否是真正的用户点击，如果是用户点击事件，对象e将有clientX, clientY, pageX, pageY等属性，并且均是数字。我们也可以检查originalEvent属性，代码如下：
{% highlight js %}
$("#foo").click(function(e){  
    if(e.hasOwnProperty('originalEvent'))  
        // Probably a real click.  
    else  
        // Probably a fake click.  
});
{% endhighlight %}

2. 方法二

    我们也可以在定义事件函数的时候指定额外的参数，通过此参数来判断，详见[trigger api](http://api.jquery.com/trigger/)
{% highlight js %}
$("#foo").click(function(e, from){  
    if(from == null) from = 'User';  
    // rest of your code  
});  
$('#foo').trigger('click', ['Trigger']); 
{% endhighlight %}

<h4>reference</h4>
<http://stackoverflow.com/questions/6674669/in-jquery-how-can-i-tell-between-a-programatic-and-user-click>
