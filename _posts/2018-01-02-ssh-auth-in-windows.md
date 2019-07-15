---
layout: post
title: "Use ssh auth in windows environment"
date: 2018-01-02 00:00:00
tags: windows git bash ssh
description: 
---

Recently, I started to work on new project which use some private repo in github installed with npm.
Those dependacies are using "git+ssh://git@github.com:<user-account>/<repository-name>".

It seemed ssh authentication is necessary certainly. In Linux, just call key-gen and copy the content of public key and it works. However, in Windows, there is no keygen. 

Click "Start" and find git bash, and run it, and then we can run 

{% highlight json %}

ssh-keygen -t rsa -C "your@email.com"

{% endhighlight %}

then it created the 2 files id_rsa and id_rsa.pub in currrent folder.
We should let the git know where to find the private key. It can be configued in /etc/ssh/ssh_config

But... in windows, where is /etc/ssh/? actually, I can't find its physical path, so I can only use vim to edit it with 

{% highlight bash %}

vi /etc/ssh/ssh_config

{% endhighlight %}

and add 

{% highlight bash %}

Host * 
    IdentityFile /c/Users/<username>/.ssh/id_rsa

{% endhighlight %}

`/c/Users/<username>/.ssh/id_rsa` is `C:\Users\<username>\.ssh\id_rsa`

then try to run 

git ls-remote -h -t ssh://git@github.com/<user-account>/<repository-name>

then console prompt to ask to enter the password.
![screenshot]({{ site.baseurl | prepend:site.url}}/images/id_rsa.png){: .center-image }*prompt to enter the password*

