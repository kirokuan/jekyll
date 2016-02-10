---
layout: post
title: "Extend Enum to use Description Attribute"
date: 2016-02-09 03:04:06
tags: csharp Enum
description: how to extend enum method to make it can use description from attribute
---

Define a Attribute with string property
{% highlight csharp %}
    public class DescriptionAttribute : Attribute
    {
        public string Description;

        public DescriptionAttribute(string desp){

            Description = desp;
        }
    }
{% endhighlight %}

Extend the method of enum, so that all enum can use method GetDescription
{% highlight csharp %}
    public static class ExtendEnum
    {
        public static string GetDescription(this Enum e)
        {
            if (e == null) return null;
            var memInfo = e.GetType().GetMember(e.ToString());
            var attributes = memInfo[0].GetCustomAttributes(typeof(DescriptionAttribute), false).Where(a => a is DescriptionAttribute).Cast<DescriptionAttribute>().ToList();
            if (attributes.Any())
                return attributes[0].Description;
            return e.ToString();
        }
        public static T ParseFromDescription<T>(this Enum e, string description)
        {
            var typeT = typeof(T);
            var enumList = Enum.GetValues(typeT).Cast<T>();
            foreach (var x in from x in enumList
                              let memInfo = typeT.GetMember(x.ToString())
                              from m in memInfo
                              let attributes = memInfo[0].GetCustomAttributes(typeof		(DescriptionAttribute), false)
                .Where(a => a is DescriptionAttribute)
                .Cast<DescriptionAttribute>()
                .ToList()
                              where attributes.Any() && attributes[0].Description == description
                              select x)
            {
                return x;
            }
            return (T)
                Enum.Parse(typeT, description, true);
        }
    }
{% endhighlight %}

So we can use in any enum  GetDescription()
if DescriptionAttribute is not present, then it use Enum.ToString() as default description.
and if we need to convert string to enum we can use ParseFromDescription<T>().

