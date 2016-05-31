---
layout: post
title: "Damn... Powershell"
date: 2016-04-28 00:04:06
tags: Powershell
description: though about using Powershell
---

Recently, for managing batch of servers, I started to use Powershell. It seemed it's like funcational language more. It also support some commonly used shortcut ,like >, %, ?.

Why it easier than bat is that it seemed that it is more structured and also can call lib from .net framework.

What makes me crazy is that "echo" always creates a newline, and it's considered as returned value. 
{% highlight bash %}
1..3| echo $_
{% endhighlight %}

It turned out to be 
{% highlight bash %}
1
2
3
{% endhighlight %}

for returning "1 2 3" in 1 line, if I only want it show in screen output, then just use

{% highlight bash %}
1..3| write-host $_ -NoNewline
{% endhighlight %}

if I want it to return in 1 line,then I need 1 more variable.
{% highlight bash %}
$p=""
1..3| $p+=$_
echo $p 
{% endhighlight %}

It's much easier to learn than Bat, especially for .net developers. However, there is always somthing confusing.

Like, I tried to use 
{% highlight bash %}
if ($a > 0) {
    ...
} 
{% endhighlight %}
 
 it always return false, since ">" doesn't mean not greater in bash..., but this is not syntactic error.
 
 {% highlight bash %}
if ($a -gt 0) {
    ...
} 
{% endhighlight %}

