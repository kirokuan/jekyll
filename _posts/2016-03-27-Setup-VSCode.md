---
layout: post
title: "Setup VSCode for typescript project"
date: 2016-03-27 11:04:06
tags: VScode Typescript
description: Setup VSCode for typescript project
---

Recently, we started to convert old js projects to tyescript for better maintainence. Default behavior of VS2013 is convert ts to js in the same folder upon file saved. It's not a good choice.

we try to use VSCode to make it easier to build and test.

For every project, VSCode allows the user to configure how to build and test (or say that it never defines how you build your project), Press F1 and find "task" and there is one called "Tasks: Configure Task Runner", and select it.

VSCode will generate a .vscode folder and there is tasks.json.

If all you need is build command,jsut use
{% highlight json %}
{
	"version": "0.1.0",

    "command":"tsc",
	"isShellCommand": false,
	"showOutput": "always",
     "args":[".","-p","--removeComments"]
}
{% endhighlight json %}
use `Ctrl`{: .key}+`Shift`{:.key}+`B`{:.key} can trigger the build,


if you need you project can be tested upon the hotkey , can use the config like
{% highlight json %}
{
	"version": "0.1.0",
    "command":"cmd",
	"isShellCommand": false,

	"showOutput": "always",

     "args":["/C"],
    "tasks": [
        {
             //CHILDREN WITH COMMAND ;)
            "taskName": "Build",
            "suppressTaskName": true,
            "isBuildCommand": true,
            "args": ["tsc -p . -removeComments"]
        },
        {
            "taskName": "test",
            "suppressTaskName": true,
            "isTestCommand": true,
          
            "args": ["chutzpah.console.exe D:\\testScript\\Nike-wl\\Nike-wl\\NikeWLUnitTest\\"]
        }
    ]
}  
{% endhighlight json %}

once `Ctrl`{: .key}+`Shift`{:.key}+`T`{:.key} clicked, the task with  "isTestCommand": true will be triggered.