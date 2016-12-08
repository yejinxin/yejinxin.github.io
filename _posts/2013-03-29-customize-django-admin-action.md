---
layout: post
title: 自定义django的admin后台action
description: 自定义django的admin后台action的方法，包括带中间页面的action。
tags: [python, django]
---

django的admin后台管理系统中自带了一个批量删除所选对象的action。
<img src="https://docs.djangoproject.com/en/1.1/_images/user_actions.png" alt="django admin action"></img>

我们还可以添加自定义的action来实现其它类似的功能，如批量修改某个字段的功能。简单的，例如将文章批量标记为已发布的action，如下:
<!--more-->
{% highlight python %}
from django.contrib import admin  
from myapp.models import Article  
  
def make_published(modeladmin, request, queryset):  
    queryset.update(status='p')   
make_published.short_description = "Mark selected stories as published"  #这里的短描述是action下拉框中显示的描述  
  
class ArticleAdmin(admin.ModelAdmin):  
    list_display = ['title', 'status']  
    ordering = ['title']  
    actions = [make_published]  
  
admin.site.register(Article, ArticleAdmin)  
{% endhighlight %}

上面例子中的action比较简单，执行action时也不需要用户输入，实际上更常见的action是需要用户的输入或选择的。例如一个model A中有个外键foreign key关联另一个model B，我希望能有一个action可以批量更改A关联的B对象。对于这种情况，django官方文档中推荐的做法是重定向至另一个View中，并将所需的item id等作为GET query参数传递过去，在另一个View中处理复杂的逻辑，如让用户输入等。
注意到默认的删除action也是需要用户输入的（确认或取消操作），也有另外的页面，但是这个页面的url确实和原先的共用的，也就是说没有完全像文档中推荐的那样有另外一个View。是怎么做到的呢？
直接上代码吧。
{% highlight python %}
from django.contrib import admin, messages  
from django import forms  
from myApp.models import DataSrc  

class CaseAdmin(admin.ModelAdmin):  
  form = CaseForm  
  actions = ['update_data_src']  
  
  class data_src_form(forms.forms.Form):  
    _selected_action = forms.CharField(widget=forms.MultipleHiddenInput)  
    data_src = forms.ModelChoiceField(DataSrc.objects)  
  

  
def update_data_src(modeladmin, request, queryset):  
  form = None  
  if 'cancel' in request.POST:  
    modeladmin.message_user(request, u'已取消')  
    return  
  elif 'data_src' in request.POST:  
    form = modeladmin.data_src_form(request.POST)  
    if form.is_valid():  
      data_src = form.cleaned_data['data_src']  
      for case in queryset:  
        case.data_src = data_src  
        case.save()  
      modeladmin.message_user(request, "%s successfully updated." % queryset.count())  
      return HttpResponseRedirect(request.get_full_path())  
    else:  
      messages.warning(request, u"请选择数据源")  
      form = None  
  
  if not form:  
    form  = modeladmin.data_src_form(initial={'_selected_action': request.POST.getlist(admin.ACTION_CHECKBOX_NAME)})  
  return render_to_response('batch_update.html',  
                                  {'objs': queryset, 'form': form, 'path':request.get_full_path(), 'action': 'update_data_src', 'title': u'批量修改数据源为'},  
                                  context_instance=RequestContext(request)  
        )  
  
update_data_src.short_description = u'批量修改 数据源'
{% endhighlight %}

batch_update.html如下：

{% highlight html %}
{% raw %}
{% extends "admin/base_site.html" %}  

{% block content %}  
    <form method="post" action="{{ path }}">  
        {% csrf_token %}  
        {{ form }}  
        <p>  
            <input type="hidden" name="action" value="{{ action }}" />  
            <input type="submit" name="cancel" value="取消" />  
            <input type="submit" value="确定"/>  
        </p>  
    </form>  
    <p>将批量修改以下所有对象</p>  
    <ul>  
        {% for obj in objs %}  
            <li>{{ obj }}</li>  
        {% endfor %}  
    </ul>  
{% endblock %} 
{% endraw %}
{% endhighlight %}

简而言之，就是在action中，首先返回一个用户输入（选择）的页面，此页面包含一个form，此form将submit至原先的url，form中包含_selected_action为用户已选择的id，以及action为用户选择的action名（即模拟原先的页面中form表单中的必须元素）。这时候用户再submit时，可识别出用户已经选择了，此时再执行想要的批量操作即可。

<h4>reference：</h4>

[http://www.hoboes.com/Mimsy/hacks/django-actions-their-own-intermediate-page/django](http://www.hoboes.com/Mimsy/hacks/django-actions-their-own-intermediate-page/django)

[django官方文档](https://docs.djangoproject.com/en/1.1/ref/contrib/admin/actions/)
