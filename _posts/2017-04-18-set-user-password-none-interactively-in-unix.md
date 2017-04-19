---
layout: post
title: set user password none-interactively in unix
description: This article describes how to set user password none-interactively in unix
tags: [unix, shell]
---

For system administrators, it's a common task to set user's password. Often we hope that we could do that without user interaction. In Linux or AIX, typically, we would write a shell script like this:   

{% highlight bash %}

# echo 'username:password' | chpasswd 
#
# echo 'password' | passwd --stdin username

{% endhighlight %}

However, if you were in a HPUX system, you will find neither `chpasswd` nor `passwd --stdin` is available. So, how can we set user password in HPUX shell script? 

<!--more-->

After searching around, I got a perl script to generate encrypted password.

{% highlight bash %}
# cat mypwgen
#!/usr/bin/perl -l
die "One arg expected\n" unless @ARGV;
print crypt(
$ARGV[0],
join( '',
( '.', '/', 0 .. 9, 'A' .. 'Z', 'a' .. 'z' )[ rand 64, rand 64 ] )
);
1;
{% endhighlight %}

...run as:
{% highlight bash %}
# ./mypwgen plaintextpw
{% endhighlight %}

...the output will be an encrypted password suitable for use with `useradd` and `usermod`.

{% highlight bash %}
# usermod -F -p ENCRYPTED username
{% endhighlight %}

The perl script above is too obscure for me, though it do work. More detailed information could be found by `man 3 crypt`. After reading that, you should know what's going on.

To generate encrypted password, the key is to call the `crypt(key, salt)` function. Here is my own `mypwgen` of python version, which I think is more readable.

{% highlight python %}
#!/usr/bin/python
import string
from random import choice
from crypt import crypt
chars=string.ascii_letters + string.digits + './'
print crypt(sys.argv[1], choice(chars)+choice(chars))
{% endhighlight %}

Alternatively, there is another quick and dirty choice.

{% highlight bash %}
# ENCRYPTED=`openssl passwd -crypt -salt ST PASSWORD`
# usermod -Fp $ENCRYPTED username
{% endhighlight %}

All the methods above can be easily adopted to Linux/AIX too. By the way, `usermod -p` can also be used to recover password, if you have a backup.
