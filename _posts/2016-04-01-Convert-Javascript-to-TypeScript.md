---
layout: post
title: "Convert Javascript to TypeScript"
date: 2016-04-01 00:04:06
tags: Javascript Typescript
description: Convert Javascript to TypeScript
---

Currently, we got batch of javascript I wanted to convert them to typescript for better maintainence.
All javascript syntax is compatiable in Typescript.
 
So with cmd command 

{% highlight %}
    ren *.js *.ts
{% endhighlight %}  

it's very easy to convert all js to ts ideally. However, most of files I handled are long-standing so that they may contain various problems like some function is not used any more and use some broken reference...etc.

Most common problem is reference to external library,like jQuery..., Since most of Library provide their definition now, include them in the header.
