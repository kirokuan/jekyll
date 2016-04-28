---
layout: post
title: "Use Git in Window"
date: 2016-04-28 00:04:06
tags: Git
description: Use Git in Window
---

Although I used official GUI long ago, I still think it confusing. I'd rather use the command line...

Recently, I found [Tortoise Git](https://tortoisegit.org/) quite useful. 

When initializing a project, just click "Create repo here" upon right click, and just click "ok" twice.

If the project should be pushed to remote server, right click-> push

At first time push, it prompts the dialog to ask you fill the information it needs
In Git/Remote, You can set up all remote reposity.

there is a easy description about how the credential works in Git.

In Git Choose "Edit Systemwide GitConfig", and add the following in the file

{% highlight bash %}
[user]
  name=yourname
  email=yourmail
{% endhighlight %}  