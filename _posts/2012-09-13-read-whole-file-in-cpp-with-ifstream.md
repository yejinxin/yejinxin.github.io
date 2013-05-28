---
layout: post
title: c++中ifstream一次读取整个文件
description: c++中一次读取整个文件至char*和std::string的方法。
tags: [cpp, string]
---

c++中一次读取整个文件的内容的方法：

1. 读取至char*的情况
{% highlight cpp %}
std::ifstream t;  
int length;  
t.open("file.txt");      // open input file  
t.seekg(0, std::ios::end);    // go to the end  
length = t.tellg();           // report location (this is the length)  
t.seekg(0, std::ios::beg);    // go back to the beginning  
buffer = new char[length];    // allocate memory for a buffer of appropriate dimension  
t.read(buffer, length);       // read the whole file into the buffer  
t.close();                    // close file handle  
  
// ... do stuff with buffer here ...  
{% endhighlight %}

2. 读取至std::string的情况：
<!--more-->

第一种方法：
{% highlight cpp %}
#include <string>  
#include <fstream>  
#include <streambuf>  
  
std::ifstream t("file.txt");  
std::string str((std::istreambuf_iterator<char>(t)),  
                 std::istreambuf_iterator<char>()); 
{% endhighlight %}

第二种方法：
{% highlight cpp %}
#include <string>  
#include <fstream>  
#include <sstream>  
std::ifstream t("file.txt");  
std::stringstream buffer;  
buffer << t.rdbuf();  
std::string contents(buffer.str());
{% endhighlight %}

<h4>reference</h4>
<http://stackoverflow.com/questions/2602013/read-whole-ascii-file-into-c-stdstring>

本文出自[{{ site.title }}]({{ post.url }})，转载请保留出处