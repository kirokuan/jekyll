---
layout: post
title: "Remove the header of the reports in Jenkins"
date: 2016-10-25 00:00:00
tags: Jenkins report
description: remove the headers in Jenkins report to make it readable
---

Recently I used some report generator to generate the reports in Jenkins. Some of them tends to use the library in CDN or external network. 

I think it's reasonable to use the external library, since jenkins keeps the reports for every build, it's kind of waste that it always keep something never changed.

According to [official doc](https://wiki.jenkins-ci.org/display/JENKINS/Configuring+Content+Security+Policy), use

    System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")

and inputing the command in Jenkins console makes it work. 