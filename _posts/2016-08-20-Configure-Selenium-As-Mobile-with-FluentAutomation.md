---
layout: post
title: "Configure Selenium  as Mobile With Chrome  Driver and FlentAutomation"
date: 2016-08-09 00:00:00
tags: FluentAutomation mobile ChromeDriver Selenium C# Test
description: How to configure with ChromeDriver wrapped with FluentAutomation
---

So far, FluentAutomation seemeed to not support the Ipad/Android natually.
With 

{% highlight csharp %}
 SeleniumWebDriver.Bootstrap(
                     SeleniumWebDriver.Browser.Andorid // or IPad
                    );
{% endhighlight %}

It throws the NotImplementationException, so I want to use Chrome to simulate the way the web site seen in mobilee

With FluentAutomation, Wwe can easily configure the selenium driver by adding in base constructor

{% highlight csharp %}

     SeleniumWebDriver.Bootstrap(
                    new Uri("http://localhost:9515/"),
                    SeleniumWebDriver.Browser.Chrome
                    );
{% endhighlight %}

It seemed quite easy if we can access ChromeDriver/

{% highlight csharp %}

    ChromeOptions chromeCapabilities = new ChromeOptions();         
    chromeCapabilities.EnableMobileEmulation("Apple iPhone 6");    
    IWebDriver driver = new ChromeDriver(chromeCapabilities);

{% endhighlight %}

But FluentAutomation also provide extra constructor

![screenshot]({{ site.baseurl | prepend:site.url}}/images/2016-08-21_102922.png){: .center-image }*constructor of bootstrap*

Although the type is not obvious, but it adds dictionary to ChromeOption

{% highlight csharp %}

    var mobileEmulation = new Dictionary<string,object>();
            mobileEmulation.Add("deviceName", "Google Nexus 5");// can donfigure different device
           
            var mobileOptions = new Dictionary<string,object>();
            mobileOptions.Add("mobileEmulation", mobileEmulation);
            var cap=new Dictionary<string,object>()
            {
                {ChromeOptions.Capability,mobileOptions}
            };
            SeleniumWebDriver.Bootstrap(
                new Uri("http://localhost:9515/"),
                SeleniumWebDriver.Browser.Chrome, cap
            );
{% endhighlight %}

with the Settings, we also can configure the width and height we want.
