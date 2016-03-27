---
layout: post
title: "Make Unit Test with App_Code in WSP"
date: 2016-03-11 11:04:06
tags: C# App_Code Unit-Test
description: make unit test for App_code
---

Recently, I need to make a unit-test for a WSP(Web Site Project), since converting it to a WAP(Web Application Project) may be troublesome.

Eventually, WSP can be compiled to dll, so we can test the App_code.dll.

{% highlight xml linenos %}
{% raw %}

  <PropertyGroup>
    <PreBuildEvent>C:\Windows\Microsoft.NET\Framework\v4.0.30319\aspnet_compiler.exe -p $(SolutionDir)WebSiteName -v / -f -u $(SolutionDir)PrecompiledWeb</PreBuildEvent>
  </PropertyGroup>

{% endraw %}
{% endhighlight xml %}

WebSiteName is the name of website project, and output folder is called PrecompiledWeb. So in the test project, only we need is that we add reference to PrecompiledWeb/bin/App_code.dll,and it's required to add all other its dependancy.

Whenever the test project builds, it needs to precompile whole website, so it became slower. This is the way I can run test with test with VS2013, but I can't make it run with Resharper somehow,all the test is labled as "inconclusive".


The other way is add before build in test solution file

{% highlight xml linenos %}
{% raw %}
  <Import Project="$(ProjectDir)\Website.targets" />
  <Target Name="BeforeBuild" DependsOnTargets="CompileWebsite">
  </Target>
{% endraw %}
{% endhighlight xml %}

and add website.targets in the root of test solution.


{% highlight xml linenos %}
{% raw %}

<?xml version="1.0" encoding="utf-8"?>
<!--
    Target that compiles Website's App_Code to be used for testing
  -->
<Project DefaultTargets="CompileWebsite" ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ItemGroup>
    <AppCodeFiles Include="$(WebsitePath)\$(WebsiteName)\App_Code\**\*.*" />
  </ItemGroup>
  <Target Name="CompileWebsite" Inputs="@(AppCodeFiles)" Outputs="$(ProjectDir)\PrecompiledWeb\bin\App_Code.dll">
    <AspNetCompiler VirtualPath="$(WebsiteName)" PhysicalPath="$(WebsitePath)\$(WebsiteName)" TargetPath="$(ProjectDir)\PrecompiledWeb" Force="true" Debug="true" />
  </Target>
  <Target Name="CleanWebsite">
    <RemoveDir Directories="$(WebsitePath)\$(WebsiteName)\PrecompiledWeb" />
  </Target>  
</Project>

{% endraw %}
{% endhighlight xml %}


