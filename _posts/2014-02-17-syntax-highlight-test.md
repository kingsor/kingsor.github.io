---
layout: post
title: "Syntax Highlight Test"
description: "This is a test post for watching how it looks like syntax highlight ..."
tags: [developer,test,syntax highlight]
---

Let's start with a light theme for our app:

**values/styles.xml**
{% highlight xml %}
<style name="AppTheme" parent="Theme.AppCompat.Light">
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
    <item name="android:textColorPrimary">@color/textColorPrimary</item>
    <item name="android:textColorSecondary">@color/textColorSecondary</item>
    <item name="android:textColorPrimaryInverse">@color/textColorPrimaryInverse</item>
    <item name="android:textColorSecondaryInverse">@color/textColorSecondaryInverse</item>
    <!-- some other theme configurations for actionbar, overflow menu etc. -->
    ...
</style>
{% endhighlight %}


To enable consistent color, and make our views and texts ready for multiple themes, it's best that we specify their colors as color resource reference, e.g. `android:textColor="@color/textColorPrimary"`, or via style, e.g. `textEmptyStyle` style below, using only a limited set of colors from your chosen color palette.

**values/styles.xml**
{% highlight xml %}
<style name="textEmptyStyle">
    <item name="android:textColor">@color/textColorSecondary</item>
    <item name="android:textSize">@dimen/abc_text_size_headline_material</item>
    ...
</style>
{% endhighlight %}

Within the same JS file add the following code beneath the controller:
{% highlight javascript %}
// Service
app.factory('calculateService', function(){
  return {
    calculate: function(number){
      return number * 10
    }
 }
});
{% endhighlight %}

Under helper package, create a java class named Movie.java and add below code. This is a model class required to create movie objects to provide data to the List View.

**Movie.java**
{% highlight java %}
package info.androidhive.swiperefresh.helper;
 
/**
 * Created by Ravi on 13/05/15.
 */
public class Movie {
    public int id;
    public String title;
 
    public Movie() {
    }
 
    public Movie(int id, String title) {
        this.title = title;
        this.id = id;
    }
}
{% endhighlight %}
