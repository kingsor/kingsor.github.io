---
layout: post
title: "Android Studio 1.0"
description: "Android Studio 1.0 has been released. Android Studio is the official Integrated Development Environment (IDE) from the Android team."
tags:
- developer
- manager
- android
- mobile
- ide
---

<blockquote class="embedly-card" data-card-key="839d2b95a8314a328b67ec0cdd25c12c" data-card-type="article-full"><h4><a href="http://android-developers.blogspot.it/2014/12/android-studio-10.html">Android Studio 1.0</a></h4><p>Today we are excited to introduce Android Studio 1.0. Android Studio is the official Integrated Development Environment (IDE) from the Android team. It is built on the popular IntelliJ IDEA (Community Edition) Java IDE. We first released a preview of Android Studio at I/O last year. We value the on-going feedback from you, thanks!</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>

You can find features and requirements of [Android Studio](http://developer.android.com/sdk/index.html) on [Android Developers](http://developer.android.com/index.html) site.

With the official release of Android Studio, Eclipse with ADT seems no more supported.

> If you have been using Eclipse with ADT, be aware that Android Studio is now the official IDE for Android, so you should migrate to Android Studio in order to continue to receive all the latest IDE updates. For help moving projects, see Migrating to Android Studio.

After installing the new release, you may read Migrating Gradle Projects to version 1.0.0 in particular for the error:

{% highlight bash %}
method not found: 'runProguard()'
{% endhighlight %}

``` bash
method not found: 'runProguard()'
```

that has to be replaced with `minifyEnabled` in your *build.gradle* files.