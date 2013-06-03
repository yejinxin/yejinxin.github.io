---
layout: post
title: 负数参与取模运算
description: C/Java, C++, Python语言中的负数参与取模运算的规则。
tags: [python, C++, C, 取模]
---

学习Python看到数值运算这部分，看到取模运算，原来不仅正数负数都可以取模，浮点数，甚至复数都可以取模：

对于x%y，

* 如果都是整数，则返回x/y的余数；

* 如果是浮点数，返回的是x - int(x/y)*y ;

* 如果是复数，返回的是x - int((x/y).real)*y。

不过以上这些公式貌似都只是对于操作数都是正数的情况下才满足。。。

对于负数参与取模运算，不同的语言有不同的规定：
<!--more-->

+ C/Java语言除法采用的是趋零截尾，一个规律是，<span style="color:blue">取模运算的结果的符号与第一个操作数的符号相同(或为0)</span>。

+ C++语言的截尾方式取决于特定的机器。

+ Python语言除法采用的是趋负无穷截尾，即floor除法。规律是，<span style="color:blue">取模运算的结果的符号与第二个操作数的符号相同</span>。

综上，可以总结为：

* C语言和Java中，取模运算只能是整数，先不管正负，算出结果，然后添加正负号使结果和第一个操作数同符号即可。

* Python中，取模运算还可以是浮点数和复数；

    1. 对于两个操作数同符号的情况，直接取余数或按照公式算；
	
    2. 对于两个操作数异号的情况，可以先不管符号，取余数或根据公式算出结果，如果结果是0则无需继续操作，如果不是0，则还须将结果取第二个操作数的补。（例如5%-4，先算出结果为1，不为0,则1取4的补为3，得到结果为3——还有最后一步，保证结果的符号和第二个操作数的符号相同！所以，5%-4的结果为-3。）

可以这样理解Python的取模运算：首先，规定结果和第二个操作数同号，其次x%y，如果y为正数，假设为y=4，则表示数轴对应关系为：

<p>... &nbsp; 3 &nbsp; <span style="text-decoration: underline;">0 &nbsp; 1 &nbsp; 2 &nbsp; 3</span> &nbsp;<span style="text-decoration: underline;">0 &nbsp;1 &nbsp;2 &nbsp;3 </span>&nbsp;0 &nbsp;1 ...</p>
<p>... -5 &nbsp;-4 &nbsp;-3 &nbsp;-2 &nbsp;-1 &nbsp;0 &nbsp;1 &nbsp;2 &nbsp;3 &nbsp;4 &nbsp;5 ...</p>

如果y为负数，假设y=-4，则表示数轴对应的关系为：

<p>... &nbsp; 1 &nbsp; 0 &nbsp;<span style="text-decoration: underline;"> 3 &nbsp; 2 &nbsp; 1 &nbsp;0</span> &nbsp;<span style="text-decoration: underline;">3 &nbsp;2 &nbsp;1 &nbsp;0</span> &nbsp;3 ...</p>
<p>... -5 &nbsp;-4 &nbsp;-3 &nbsp;-2 &nbsp;-1 &nbsp;0 &nbsp;1 &nbsp;2 &nbsp;3 &nbsp;4 &nbsp;5 ...</p>

那么对于C语言的取模运算，就可以这样理解了：首先，规定结果和第一个操作数同号，其次，不管正负，仍以5%4或者5%-4为例，数轴对应关系为：

<p>... &nbsp; <span style="text-decoration: underline;">1 &nbsp; 0</span> &nbsp; <span style="text-decoration: underline;">3 &nbsp; 2 &nbsp; 1 &nbsp;<strong><span style="font-family: mceinline;">0</span></strong> &nbsp;1 &nbsp;2 &nbsp;3</span> &nbsp;<span style="text-decoration: underline;">0 &nbsp;1 </span>...</p>
<p>... -5 &nbsp;-4 &nbsp;-3 &nbsp;-2 &nbsp;-1 &nbsp;0 &nbsp;1 &nbsp;2 &nbsp;3 &nbsp;4 &nbsp;5 ...</p>

o(╯□╰)o，搞得有点复杂了。。。最后，贴一个Python下的运算截图吧：

![Python取模运算截图](/img/post/modulo.jpg)

因此，在不确定具体实现的情况下，保险的做法还是自己先把操作数转换为正数，再根据需要调整。。。

本文出自[{{ site.title }}]({{ page.url }})，转载请保留出处