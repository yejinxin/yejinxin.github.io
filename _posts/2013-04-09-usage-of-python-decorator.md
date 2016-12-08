---
layout: post
title: Python装饰器(decorator)用法
description: 带参数和不带参数的Python装饰器的用法。
tags: [python]
---

普通的不带参数的装饰器，写法如下：
{% highlight python %}
def debug(func):  
    def wrapper(*args, **kwargs):  
        print 'debug start'  
        ret = func(*args, **kwargs)  
        print 'debug end'  
        return ret  
    return wrapper  
  
@debug  
def foo(arg1, arg2):  
    print 'foo', arg1, arg2 
{% endhighlight %}

带参数的装饰器，写法如下：
<!--more-->
{% highlight python %}
def debug(level=1):  
    def wrapper(func):  
        def inner_wrapper(*args, **kwargs):  
            print 'debug start(level=%s)' % level  
            ret = func(*args, **kwargs)  
            print 'debug end(level=%s)' % level  
            return ret  
        return inner_wrapper  
    return wrapper  
  
@debug(3)  
def foo1(arg1, arg2):  
    print 'foo1', arg1, arg2  
  
@debug()  
def foo1(arg1, arg2):  
    print 'foo2', arg1, arg2
{% endhighlight %}
