---
layout: post
title: web.py入门纪要
description: web.py入门纪要，关于session, form validation, web.input, static。
tags: [python, django]
---

+ web.py自带的服务器在调试模式下，session不能正常工作，因为调试模式支持模块重载入(重载入，绝非重载。是reload,而非override)，所以reloader会载入主模块两次，因此，就会创建两个session对象。<br>为此，可以直接禁用调试模式，只要令web.config.debug = False即可。如果非要在调试模式下使用session，也有方法，我们只要把session存储在全局的数据容器中，就能避免二次创建session，如下(不要忘了重启服务器生效)：
<!--more-->
{% highlight python %}
if web.config.get('_session') is None:  
    session = web.session.Session(app, web.session.DiskStore('sessions'))  
    web.config._session = session  
else:  
    session = web.config._session 
{% endhighlight %}

---
+ 要想在模版文件里访问session，可以在创建render对象时将session传递进去，如:
{% highlight python %}
render = web.template.render('templates', base='base', globals={'context': session})  
{% endhighlight %}
只要，在模版文件中就可以访问到session了，如:
{% highlight bash %}
$if context.get('logged_in'):  
    ...  
$else:  
    ...  
{% endhighlight %}
当然，这种方法同样适用于传递其它变量。

---
+ 类似django中的login_required的装饰器，代码如下：
{% highlight python %}
def login_required(func):  
    def _loginRequiredInner(*args, **kw):  
        if session.get('logged_in'):  
            return func(*args, **kw)  
        raise web.seeother('/login/')  
    return _loginRequiredInner 
{% endhighlight %}

---
+ web.input()可以获得用户请求提交(GET/POST)的数据，是一个类字典的对象。对于多选框之类的有多个值的数据，默认情况下只返回其中一个值，如果想以列表方式获得所有值，可以提供一个空列表为初值，如web.input(fields=[]),其中fields是多选框的name。实际上这种方法还可以用于指定默认值。

---
+ Form对象的validates方法可以验证Form对象是否有效，即各个inputs是否通过了验证。只有调用了validates方法，Form对象的各个inputs才会有相应的值，否则，其值均是None。validates方法其内部默认会使用web.input()来作为数据来源去验证，如果需要指定数据来源，可以指定source参数，如：
{% highlight python %}
form = self.form()  
data = web.input(mid_page=[])  
data.mid_page = filter(None, data.mid_page)  
if form.validates(source=data):  
    self._save(form.d)  
    web.header('Content-Type','text/html; charset=utf-8', unique=True)  
    return "<strong>已保存</strong>"  
else:  
    return render.index(form)  
{% endhighlight %}

---
+ 要想在web.py自带的开发服务器中处理静态文件（如图片或css和js），可以在代码目录下创建一个文件夹命名为static，然后将需要的静态文件放到这个static文件夹里即可。也可以自己写写一个View来处理静态文件，代码如下:
{% highlight python %}
import web, os, wsgiref.util, mimetypes, datetime  
   
def serve_static(filename, mime_type=None):  
    ''''' Serves a file statically '''  
    if mime_type is None:  
        mime_type = mimetypes.guess_type(filename)[0]  
    web.header('Content-Type', '%s' % mime_type)  
    stat = os.stat(filename)  
    web.header('Content-Length', '%s' % stat.st_size)  
    web.header('Last-Modified', '%s' %  
    web.http.lastmodified(datetime.datetime.fromtimestamp(stat.st_mtime)))  
    return wsgiref.util.FileWrapper(open(filename, 'rb'), 16384)  
   
class Static(object):  
    ''''' Static file serving.'''  
    def GET(self, name):  
        return serve_static(os.path.join('mystatic', name))  
   
urls = (  
    '/mystatic/(.*)', Static  
    )  
{% endhighlight %}
