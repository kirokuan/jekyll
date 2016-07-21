---
layout: post
title: "Deploy RoR project to Heroku"
date: 2016-07-20 00:04:06
tags: Heroku RoR Deployment Git
description: deploy project to RoR and make it run
---

Recently I worked for a RoR project. 

{% highlight bash %}
    heroku login
    heroku create app
{% endhighlight %}


heroku create will ask you the login and password, and then "create app" will create a new reposity in heroku.

And its name is generated randomly, for example: testproject

{% highlight bash %}
    cd /path_to_app/
    heroku git:remote -a testproject
{% endhighlight %}

and if we want to verify if the remote is added, use

{% highlight bash %}
    git remote -v
{% endhighlight %}

here should be
{% highlight bash %}
    heroku  https://git.heroku.com/testproject.git (fetch)
    heroku  https://git.heroku.com/testproject.git (push)
{% endhighlight %}

use 
{% highlight bash %}
    git push heroku master
{% endhighlight %}
it will push code to Heroku. But I encountered a problem,that the terminal kept asking me the  authentication.

I tried to key the username and password I used, but it failed. Finally, I found that
key
{% highlight bash %}
    heroku auth:token
{% endhighlight %}

and what it shows is password,and username is left as empty.

{% highlight bash %}
    heroku run rake db:migrate
{% endhighlight %}


