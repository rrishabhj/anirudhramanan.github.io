---
layout: post
title: 'Snippet: DateUtils'
date: '2013-01-15 00:00:00'
---

<p>This is the first in a new series of posts I&#8217;m going to regularly publish (weekly-ish) of code snippets which could save you a lot of #AndroidDev time. The first of which is about one of my favourite classes in Android: <a href="http://developer.android.com/reference/android/text/format/DateUtils.html" target="_blank">DateUtils</a>.</p>

<!--more-->

<p>It&#8217;s Javadoc keeps things nice and short:</p>

<blockquote>
  <p>This class contains various date-related utilities for creating text for things like elapsed time and date ranges, strings for days of the week and months, and AM/PM text etc.</p>
</blockquote>

<p>It contains a lot of static methods to help you format date, times, time deltas into a human-readable String. The best bit though is that it handles all of the localisation for you!</p>

<h2>Example Use</h2>

<p>So I&#8217;ll give you an example of where you might use this class. Say you have a Twitter timeline and want to display how long ago a tweet was sent. You could write the code yourself, but you&#8217;d quickly end up with a long method which is difficult to test. So instead, use <code>DateUtils.getRelativeTimeSpanString(...)</code>. At the time of writing there five versions of the method, but the one I tend to use is:</p>

<pre>CharSequence getRelativeTimeSpanString (long time, long now, long minResolution)</pre>

<p>So how do you use the method? Well it&#8217;s pretty simple:</p>

<ul>
<li><code>time</code> &#8211; The time of the Tweet (epoch format).</li>
<li><code>now</code> &#8211; System.currentTimeMillis()</li>
<li><code>minResolution</code> &#8211; This depends how accurate you granular you want the String to be. I tend to use <a href="https://developer.android.com/reference/android/text/format/DateUtils.html#SECOND_IN_MILLIS" target="_blank">SECOND_IN_MILLIS</a> so the smallest unit of time will be seconds.</li>
</ul>

<p>Which returns something like this (in English):</p>

<pre>2 seconds ago</pre>

<p>Or this in German:</p>

<pre>Vor 33 Sekunden</pre>

<h2>Bottom Line</h2>

<p>If you&#8217;re doing any kind of date or time formatting, make sure you check to see if it is already implemented in DateUtils. It will save you a lot of time!</p>
