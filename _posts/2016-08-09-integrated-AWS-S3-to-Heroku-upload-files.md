---
layout: post
title: "Integrated AWS S3 to RoR application in Heroku to upload files"
date: 2016-08-09 00:00:00
tags: Heroku RoR Deployment AWS 
description: deploy project to RoR and make it run
---

Recently, not until i tested my new application in heroku environment, I found that whenevet I pushed somthing to Heroku, the all uploaded files disapeared. 

It's very shocking, since I have ignored uploads folders, but it also makes sense that whenever you push , the whole server is clear and build again.

The [official tutorial](https://devcenter.heroku.com/articles/direct-to-s3-image-uploads-in-rails) introduced how you can integrated AWWS S3 to Application. 

The other choice is that deploying the application to AWS, since AWS is enabled already. 


I encountered some technical issue here.

1. AWS settings problem: The article mentioned how to define environment and use in RoR application

{% highlight ruby %}
    Aws.config.update({
        region: 'us-east-1',
        credentials: Aws::Credentials.new(ENV['AWS_ACCESS_KEY_ID'], ENV['AWS_SECRET_ACCESS_KEY']),
        })

        S3_BUCKET = Aws::S3::Resource.new.bucket(ENV['S3_BUCKET'])
{% endhighlight %}

The benefit to use environment variable is that you can defineed settings by different enviroment. But the way it mentioned using .env is not feasible in Window environment.

1.a region of Aws: the example use us-east-1 as example, and when you create the S3 , the property show where the server located. Since I chose "Tokyo" as region, but correct settings is not "Tokyo".

"Tokyo" is Region Name rather than Region. go to [manual](http://docs.aws.amazon.com/general/latest/gr/rande.html) to check the mapping.

2. js loading problem: the article uses [fileupload plugin](http://blueimp.github.io/jQuery-File-Upload/), which includes at least 2 files. Loading order may be the problem, since fileupload.js depends on widget.js.

The article just adds a "z." front of fileupload.js,it's kind of tricky. But I still encountered the another error showing "fileupload is not a method". Finally I found it's due to conflict against jQeury.UI, since I loaded it after widget.

But actually jquery.ui can coexist with jquery.widget with correct order.





