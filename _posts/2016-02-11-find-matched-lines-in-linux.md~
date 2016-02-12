---
layout: post
title: "Find Matched lines in Linux"
date: 2016-02-12 11:04:06
tags: perl linux grep regexp
description: Find Matched lines in Linux
---

if  we need to use regular expression,
then use perl to print the result
{% highlight perl %}
perl -l -ne '/(u[A-Za-z0-9]{8}),(u[A-Za-z0-9]{8})/g && print $1.",".$2' week*.csv
{% endhighlight %}

for simpler case, we only find certain text in files 


{% highlight %}
  cat week*.csv | grep 'test' 
{% endhighlight %}


