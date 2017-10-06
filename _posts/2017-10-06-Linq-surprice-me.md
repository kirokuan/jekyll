---
layout: post
title: "LinQ surpriced me!!"
date: 2017-10-06 00:00:00
tags: C# LinQ
description: 
---

just want to murmur...

I practice LeetCode with C#

I try #373 Find K Pairs with Smallest Sums, this requires the algorithm.

But i still tried brute-force 1st.

{% highlight csharp %}
    public static IList<int[]> KSmallestPairs(int[] nums1, int[] nums2, int k)
        {
            var list = nums1.Select(t => nums2.Select(y => new int[] { t, y }))
                .SelectMany(t => t)
                .OrderBy(t => t[0] + t[1]).ToList();
                
            return list.Take(k).ToList();
        }

{% endhighlight %}

This exceed time limit

{% highlight csharp %}
    public static IList<int[]> KSmallestPairs(int[] nums1, int[] nums2, int k)
        {
            var list = nums1.Select(t => nums2.Select(y => new int[] { t, y }))
                .SelectMany(t => t)
                .OrderBy(t => t[0] + t[1]);
                
            return list.Take(k).ToList();
        }

{% endhighlight %}

This passed more than 700ms, 12-13% . 