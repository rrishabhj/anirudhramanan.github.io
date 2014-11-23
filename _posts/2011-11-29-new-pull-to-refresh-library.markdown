---
layout: post
title: New Pull-to-Refresh Library
date: '2011-11-29 00:00:00'
---

Hi everyone,

Over the past week or so I&#8217;ve been working on a new implementation of the Pull-to-Refresh widget for Android, as part of improvements for [FriendCaster][1]. The new library is based off Johan Nilsson&#8217;s [library][2], but has changed a lot since then.

The way that the previous library achieves Pull-To-Refresh is via ListView HeaderViews which has certain drawbacks:

*   Hard to show/hide the HeaderView when you don&#8217;t have enough items in your list
*   Have to use ScrollListener, which is slow.
*   Only works with ListView&#8217;s.

<div>
  Our new library is different because it&#8217;s actually a LinearLayout wrapper around an <a href="http://developer.android.com/reference/android/widget/AdapterView.html">AdapterView</a>, which hides/shows the Refreshing View. The end result has several improvements over the previous library:
</div>

<div>
  <ul>
    <li>
      Can be used with all AdapterViews (ListView, GridView, etc, etc).
    </li>
    <li>
      Animated Scrolling for all relevant Android OS versions (tested on v1.6+).
    </li>
    <li>
      Works properly when you don&#8217;t have many items in your AdapterView (no more &#8216;Tap to Refresh&#8217;).
    </li>
    <li>
      Works just as well as the Official Twitter App&#8217;s Pull-to-Refresh in my opinion.
    </li>
  </ul>
  
  <div>
    You can find the new Library here: <a href="https://github.com/chrisbanes/Android-PullToRefresh">https://github.com/chrisbanes/Android-PullToRefresh</a>
  </div>
</div>

 [1]: http://www.friendcasterapp.com/
 [2]: https://github.com/johannilsson/android-pulltorefresh