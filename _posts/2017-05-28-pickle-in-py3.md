---
layout: post
title: "Set Proxy for docker service"
date: 2017-2-20 00:00:00
tags: docker proxy
description: 
---

I encountered the problem how to set up proxy for docker while it's pulling.

It seemed the way in Linux and Windows are different.

{% highlight bash %}
[Environment]::SetEnvironmentVariable("HTTP_PROXY", "http://username:password@proxy:port/", [EnvironmentVariableTarget]::Machine)
Restart-Service docker
{% endhighlight %}


in Linux, the proxy can be set @ /etc/systemd/system/docker.service.d/http-proxy.conf

{% highlight bash %}
[Service]
Environment="HTTP_PROXY=http://address:port/"
Environment="HTTPS_PROXY=https://address:port/"
{% endhighlight %}

then restart the docker

{% highlight bash %}
sudo systemctl restart docker
{% endhighlight %}
