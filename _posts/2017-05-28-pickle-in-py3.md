---
layout: post
title: "Pickle in py3"
date: 2017-5-28 00:00:00
tags: python
description: 
---

I tried to update a app to  py3. But there is import cPickle. I import cPickle, it showed  error.

And I asked the admin if he can help install cPickle. 
But he tell me  that should use:

{% highlight python %}
     import _pickle as cPickle
{% endhighlight %}