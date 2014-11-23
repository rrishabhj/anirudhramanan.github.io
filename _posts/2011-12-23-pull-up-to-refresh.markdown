---
layout: post
title: Pull Up to Refresh!
date: '2011-12-23 00:00:00'
---

Hi everyone,

I&#8217;ve just added a new mode to the [Pull-to-Refresh library][1] which allows you to set the View to Pull **Up** to Refresh (from the bottom) instead of the standard Pull Down from the top.  
<!--more-->

  
It&#8217;s pretty simple to use, just add another tag in the XML declaration:

<pre class="brush: xml; gutter: true">&lt;com.handmark.pulltorefresh.library.PullToRefreshListView
  xmlns:ptr="http://schemas.android.com/apk/res/YOUR_APP_PACKAGE_NAME"
    android:id="@+id/pull_refresh_list"
    android:layout_height="fill_parent"
    android:layout_width="fill_parent"
    ptr:mode="pullUpFromBottom" /&gt;</pre>

As you can see there&#8217;s a new mode attribute which was two possible values:

*   pullDownFromTop (default)
*   pullUpFromBottom

And that&#8217;s all there is to it!

<span class='embed-youtube' style='text-align:center; display: block;'></span>

 [1]: http://www.senab.co.uk/2011/12/13/pull-to-refresh-view/