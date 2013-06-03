---
layout: post
title: 递归遍历文件夹脚本
description: 递归遍历文件夹脚本，python和shell版本。
tags: [python, shell]
---

递归遍历某个文件夹下的所有文件是一项很常见的操作，总结通过shell和Python来递归遍历文件夹的脚本如下：
<!--more-->

+ shell版本
    {% highlight bash %}
#!/bin/bash  

function walk()  
{  
  for file in `ls $1`  
  do  
    local path=$1"/"$file  
    if [ -d $path ]  
     then  
      echo "DIR $path"  
      walk $path  
    else  
      echo "FILE $path"  
    fi  
  done  
}  

if [ $# -ne 1 ]  
then  
  echo "USAGE: $0 TOP_DIR"  
else  
  walk $1  
fi  
    {% endhighlight %}
---
+ python version 1, using os.listdir
    {% highlight python %}
def os_list_dir(top_dir):  
  for file in os.listdir(top_dir):  
    file_path = os.path.abspath(os.path.join(top_dir, file))  
    if os.path.isfile(file_path):  
      print 'FILE', file_path  
    elif os.path.isdir(file_path):  
      print 'DIR', file_path  
      os_list_dir(file_path)
    {% endhighlight %}

---
+ python version 2, using os.walk
    {% highlight python %}
def os_walk(top_dir):  
  for parent, dirnames, filenames in os.walk(top_dir):  
    for filename in filenames:  
      print 'FILE', os.path.abspath(os.path.join(parent, filename))  
    for dirname in dirnames:  
      print 'DIR', os.path.abspath(os.path.join(parent, dirname))  
    #del dirnames[:]  #uncomment this line to not walk recursively 
    {% endhighlight %}

---
+ python version 3, using os.path.walk
{% highlight python %}
def os_path_walk(top_dir):  
  def print_name(arg, dirname, files):  
    for file in files:  
      file_path = os.path.abspath(os.path.join(dirname, file))  
      if os.path.isfile(file_path):  
        print 'FILE', file_path  
      elif os.path.isdir(file_path):  
        print 'DIR', file_path  
    #del files[:] #uncomment this line to not walk recursively  
   
  os.path.walk(top_dir, print_name, None)
{% endhighlight %}

本文出自[{{ site.title }}]({{ page.url }})，转载请保留出处