---
layout: post
title: "Draw Confusion Matrix with R"
date: 2016-06-09 00:04:06
tags:R Confusion-Matrix
description: Convert a confusion matrix from csv format to plot
---

confusion matrix format:
{% highlight bash %}
 1589 ,1893 ,1412,   63,  452,   17,  406,  187,  471,  203,  178, 1116,  799,   53,  100,  223 , 137 ,  31, 240 , 206 , 582 , 267 ,  59, 1750 ,  76 ,  99 ,  46 ,1150 ,  55 ,1019 
  522 ,2998 ,1920,   77,  428,    9,  379,  131,  199,  237,  145,  453,  491,   30,   60,  521 , 192 ,  28, 112 , 121 , 264 , 111 ,  84, 1461 ,  95 , 167 ,  44 , 808 ,  14 , 699 
...
{% endhighlight %}


There is  30 * 30 matrix


the last line *low = "#581845", high = "#ffc300"*  indicates that the color for highest value.

![screenshot]({{ site.baseurl | prepend:site.url}}/images/confusion-matrix.png){: .center-image }*Confusioin Matrix*
