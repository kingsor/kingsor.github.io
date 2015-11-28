---
layout: post
title: "Android ListView, show a message if the list is empty"
description: "This is the common way to show a message when the listview is empty."
category: Developer
tags: [developer,android,listview,empty]
---

This solution was found on [StackOverflow](http://stackoverflow.com) as an answer to this question: [Android ListView,show a message if arrayList of arrayAdapter is empty](http://stackoverflow.com/a/20329154).

The common way is setting an empty textview (@android:id/empty) after the listview:

**layout/activity_main.xml**
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >

<ListView
    android:id="@android:id/list"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:dividerHeight="1dp" >
</ListView>

<TextView
    android:id="@android:id/empty"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/empty_list" />

</LinearLayout>
{% endhighlight %}

And then set set the empty view to the listview:

**MainActivity.java**
{% highlight java %}
ListView lv = (ListView)findViewById(android.R.id.list);
TextView emptyText = (TextView)findViewById(android.R.id.empty);
lv.setEmptyView(emptyText);
{% endhighlight %}

This should show items in the list whenever they are, and show the textview when not.
