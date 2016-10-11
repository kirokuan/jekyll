---
layout: post
title: "simple Threading in Linux"
date: 2016-09-01 00:00:00
tags: Linux shell
description: simple script to enable threading in Linux 
---

test is a script we want to execute with input parameter 1~52
{% highlight bash %}
 
#!/bin/bash 
    for ((i=1;i<=20;i++));
    do
        (for((j=$i;j<=52;j+=20));
            do test $j
        done )&

    done

{% endhighlight %}

This started 20 threads and when 1 is finished, the next is executed