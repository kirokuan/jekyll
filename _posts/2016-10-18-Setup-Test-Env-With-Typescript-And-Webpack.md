---
layout: post
title: "Setup Test Environment with Typescript and Webpack"
date: 2016-10-18 00:00:00
tags: Typescript Javascript QUnit Webpack chutzpah requirejs karma tests
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
    /*"Compile": {
        "Extensions": [ ".ts" ],
        "ExtensionsWithNoOutput": [ ".d.ts" ],
         "Paths": [
            { "OutputPath": "dist" } 
        ]

    },*/
    "References": [
       { "Path":"require.js" }
    ],
    "EnableCodeCoverage": "true",
    "Tests": [
        { "Path":"dist/test" }
    ]
}

{% endhighlight %}


"dist" folder is the destination of compiled files. Before running the tests,"tsc" should be executed to compile the scripts, since the chutzpah is no longer to support to compile the script, so compiled part seemed redundant. 
Since tsc compiled the script into module compatible with requirejs, including the require.js makes the tests works.

Actually, the solution **is nothing to do with Webpack**. It compiled all single ".ts" into ".js" and just run test on ".js".

While running the tests with Chutzpah, it created some temporary files and running it with Phantomjs.

The another way to run test combining with webpack is using Karma. There is a [example](https://github.com/sethmcl/typescript-webpack-karma-mocha) to run karma with typescript/webpack/mocha.

Although I followed the settings and made 1 , but eventually it failed. Karma is more coomplicated to setup. In brief, it used preprocessor to compiled typescript and pack them.

But some package(like karma-webpack) let the tests shared the same settings with that of Typescript and Webpack, and karma also allows the tests executed on different browsers, which Chutzpah can't do.

and Karma also support to use other reporting package. 

===Conclusion===

Karma is compatible with its various packages including browsers, report, test framework, but is more complicated to setup.

Chutzpah is setup much easilier and support development in window environment very well, like:it also support to run the test in Visual Studio. But it lacks the flexibility of configuraion.


