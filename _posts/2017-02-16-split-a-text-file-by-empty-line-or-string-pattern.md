---
layout: post
title: Split a text file by empty line or string pattern
description: Split a text file by empty line or string pattern into multiple files.
tags: [Linux, awk]
---

Say we have a file as below, we want to split this file by empty line.

{% highlight bash %}
# cat infile.txt
hello
hello world

quick fox
runs
away

hello again
{% endhighlight %}

<!--more-->

Using awk, we can achieve that.

{% highlight bash %}
# awk '{print $0 > "outfile" NR}' RS='' infile.txt
# cat outfile1
hello
hello world
# cat outfile2
quick fox
runs
away
# cat outfile3
hello again
{% endhighlight %}

There is also a `csplit` command, which can do similar things.

{% highlight bash %}
# csplit --digits=2 --quiet --prefix=outfile infile.txt '/^$/+1' '{*}'
# cat outfile00
hello
hello world

# cat outfile01
quick fox
runs
away

# cat outfile02
hello again
{% endhighlight %}

`man csplit` to see specific usage of csplit. It may be a little confusing, at least for me. I can't find a way to eliminate the empty line of the outfiles.

Similarly, if we want to split infile by specific string pattern instead of empty line, we can also achieve that.

Let's try csplit first. This time, try to split infile2.txt by string "AAAA".

{% highlight bash %}
# cat infile2.txt
hello
hello world
AAAA
quick fox
runs
away
AAAA
hello again
# csplit --digits=2 --quiet --prefix=outfile infile2.txt '/^AAAA$/+1' '{*}'
# cat outfile00
hello
hello world
AAAA
# cat outfile01
quick fox
runs
away
AAAA
# cat outfile02
hello again
{% endhighlight %}

Then, we try awk.

{% highlight bash %}
# awk '{print $0 > "outfile" NR}' RS='AAAA\n' infile2.txt
# cat outfile1
hello
hello world

# cat outfile2
quick fox
runs
away

# cat outfile3
hello again

{% endhighlight %}

If you don't like the trailing empty line of all the outfiles, use "printf" instead of "print", like this.
{% highlight bash %}
# awk '{printf $0 > "outfile" NR}' RS='AAAA\n' infile2.txt
# cat outfile1
hello
hello world
# cat outfile2
quick fox
runs
away
# cat outfile3
hello again
{% endhighlight %}

In conclusion, to split file by empty line or specific string pattern, consider using awk or csplit. Personally I perfer awk, as it's more flexible. 