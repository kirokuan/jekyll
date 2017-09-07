---
layout: post
title: "LinQ Property"
date: 2017-8-29 00:00:00
tags: C# LinQ
description: 
---

I encountered a bug that index in anounymous type would be wrong and greater than expected.

{% highlight csharp %}
 public static IList<int> Maximize(IList<int> numbers, int swapTimes)
        {
            var i = 0;
            var rankingNum=numbers
                .Select(n => new
                {
                    index = i++,
                    num = n
                })
                .OrderByDescending(t => t.num)//.ToArray();
            ...
             for (var x=0;x< rankingNum.Count() && times>0;x++)
            {
               ...

            }
            return newNumbers;
        }
{% endhighlight %}

And in each iteration, index of rankingNum keep increasing. This is due to LinQ property.

So Just add ToList/ToArray while declaration, it value is assigned and kept. 
