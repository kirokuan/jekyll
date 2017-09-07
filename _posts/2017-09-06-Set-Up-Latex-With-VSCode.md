---
layout: post
title: "Set Up Latex Project with VSCode"
date: 2017-09-06 00:00:00
tags: VSCode Latex Windows
description: 
---

Recently, I worked with Latex for reporting. And It's a little troublesome to setup Latex environment in Windows. 

I downloaded and installed Miktex, whiich will download all dependant package the project required.  

And open the folder which my Latex project located, and press `F1` to configure the build script.


{% highlight json %}
{
    "version": "2.0.0",
    "tasks": [
        {
            "taskName": "build",
            "type": "shell",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "command": "pdflatex.exe --interaction=errorstopmode --synctex=-1 \"thesis.tex\" ; .\\thesis.pdf",
            "problemMatcher": []
        },
        {
            "taskName": "push",
            "type": "shell",
            "command": "git push",
            "problemMatcher": []
        }
    ]
}
{% endhighlight %}

The build script just calls pdflatex.exe, whcih is installed by Miktex. My entry file is thesis.tex, then after build finished, make the pdf open automatically. And there is another task called "push", which can push the project to git.

There are some extension to provide the preview function from pdflatex, like LaTex Preview...etc. I also install "Latex Language Support".