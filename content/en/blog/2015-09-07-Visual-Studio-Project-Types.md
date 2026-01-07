---
date: "2015-09-07"
summary: 'Everything started opening a Visual Studio 2010 Solution and getting
  the following error: ''The project type is not supported by this installation.'''
categories:
- developer
tags:
- visual studio
- project type
- guid
title: Visual Studio Project Type GUIDs
---

Everything started opening a Visual Studio 2010 Solution and getting the following error: 'The project type is not supported by this installation.'

So I looked for the `<ProjectTypeGuids>` in the `csproj` file. Then I looked for the GUIDs I found in order to understand what type of project was missing.

I found a very usefull [List of Visual Studio Project Type GUIDs](http://www.codeproject.com/Reference/720512/List-of-Visual-Studio-Project-Type-GUIDs) on [CodeProject](http://www.codeproject.com/) by [Yvan Rodrigues](http://www.codeproject.com/Members/Yvan-Rodrigues).

By the way, the C# project I was trying to open was an [ASP.NET MVC 4](http://www.asp.net/mvc/mvc4) project type. I downloaded and installed the right package and then I was able to open my C# project without errors.
