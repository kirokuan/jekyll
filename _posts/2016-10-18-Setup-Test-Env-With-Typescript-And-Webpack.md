---
layout: post
title: "Setup Test Environment with Typescript and Webpack"
date: 2016-10-18 00:00:00
tags: Typescript Javascript QUnit Webpack chutzpah requirejs karma
description: Try to setup environment for typescript which is packed with webpack
---

I recently worked on typescript and packed them with webpack. It's not difficult, since there is some existing tutorial.

And when I turned into setting up the environment of the tests, it got much difficult. There is no framework designed for typescript.

Most of framework aimed at javascript, so the workaround I previously applied is to test the building version rather than original script.

But recently, I also started to use webpack and "import"/"export" to manage my modules more systematically. And Typescript compiles "import"/"export" into requirejs style.

I googled and found a framework called "Chutzpah", and it supports to test Typescript. 


{% highlight json %}
 
{
    "Framework": "qunit",
    "TestHarnessReferenceMode": "AMD",
    "TestHarnessLocationMode": "SettingsFileAdjacent",
    "TypeScriptModuleKind": "AMD",
    "TestFileTimeout": "20000",
    "Compile": {
        "Extensions": [ ".ts" ],
        "ExtensionsWithNoOutput": [ ".d.ts" ],
         "Paths": [
            { "OutputPath": "dist" } 
        ]

    },
    "References": [
       { "Path":"require.js" }
    ],
    "EnableCodeCoverage": "true",
    "Tests": [
        { "Path":"dist/test" }
    ]
}

{% endhighlight %}
