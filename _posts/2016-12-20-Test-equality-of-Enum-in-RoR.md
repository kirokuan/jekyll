---
layout: post
title: "Test Equality of Enum in ROR"
date: 2016-12-20 00:00:00
tags: Enum RoR 
description: how to test equality in RoR
---

According to the [post](http://www.justinweiss.com/articles/search-and-filter-rails-models-without-bloating-your-controller/), we can create a function in model.

{% highlight ruby %}

class Item < ActiveRecord::Base
  enum status: [:unknown,:Pending, :Active, :Waiting,:Delete,:Reject,:Finish]    
  scope :status, -> (status) { where status: status }
end

{% endhighlight %}

While status is enum, i tried several way to make it work

in Controller,

{% highlight ruby %}

    @items.status(1) # (o)
    @items.status(:Pending) #(x) without compiling error
    @items.status("Pending") #(x) without compiling error
    @items.status(Item.statuses[:Pending]) # (o)

{% endhighlight %}
Make sure it's status**es**[:Pending] not status

None of them contribute runtime error, but it's always false. It seemed strange...


in View, use string to compare

{% highlight ruby %}

    <% if item.status == "Pending"%> # (o)
    <% if item.status == 1%> # (x)
    <% if item.status == :Pending%> # (x)
    <% if item.status == Item.statuses[:Pending]%> # (x)
    <% if item.Pending? %> # (o)
    <% if item.status.Pending? %> # (x) runtime error
    
{% endhighlight %}

My speculation is that it converts property into string for rendering.
