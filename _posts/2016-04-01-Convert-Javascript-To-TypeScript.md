---
layout: post
title: "Convert Javascript to TypeScript"
date: 2016-04-01 11:04:06
tags: Javascript Typescript
description: Convert Javascript to TypeScript
---

Currently, we got batch of javascript I wanted to convert them to typescript for better maintainence.
All javascript syntax is compatiable in Typescript.
 

So with cmd command 

{% highlight %}
    ren *.js *.ts
{% endhighlight %}  

it's very easy to convert all js to ts ideally. However, most of files I handled are long-standing so that they may contain various problems like some function is not used any more and use some broken reference...etc.

Most common problem is reference to external library,like jQuery..., Since most of Library provide their definition now, include them in the header.

{% highlight javascript %}
    /// <reference path="defition/jquery.d.ts" />
{% endhighlight javascript %}

Even though the definition file is absent, use command to generate the definition 
{% highlight %}
    tsc -d file
{% endhighlight %}  

For the global object, if tconfig is defined (that mean the whole folder is considered as a project),  variable can be used in other files. 
For the global object that is not defined in the file in project,can define them in the window interface

{% highlight javascript %}
    interface Window {
        $:Jquery
    }
{% endhighlight javascript %}

It takes a lot of effort to clear up the codebase to make it can be compiled.
{% highlight javascript %}

    function test(a){...} 
    test(); ///typescript alert error

    var x={a:"bbb"}
    if(x.bb){ //// conpile error, since the property should be defined before used
        ...
    }
{% endhighlight javascript %}

So to make it compatible with old code and runnable before all javascript is converted to typescript, the workaround is to avoid to use the keyword like export/require...since once use these keywords, compiler will convert the code with RequireJS style.