---
layout: post
title: "Restore bundle installed in RoR"
date: 2016-12-24 00:00:00
tags: RoR Gem
description: Restore bundle installed RoR
---

Thse assignment is try to install GITLAB in a Linux. 

After installing bundle with "sudo -u git -H bundle install --deployment --without development test postgresql aws kerberos", postgre sql would be ignore. 

No matter how I changed Gemfile, postgre will be ignore. Even I set "sudo -u git -H bundle install" without "--without", the bundle couldn't be installed.

Eventually, there are hidden files, directory called ".bundle". so delete them and install again.

{% highlight bash %}
 
 rm -rfv .bundle

{% endhighlight %}