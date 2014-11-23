---
layout: post
title: Performance Issues – Facebook Bug
date: '2011-02-18 00:00:00'
---

I hear a lot of people emailing me telling me that Flow is slow and is always loading. I decided to investigate this, and found this to be true. After some poking around I found it to be because of a Facebook bug. Basically Flow requests that data from Facebook to be sent compressed, thereby minimizing the amount of data transferred. All of Facebook&#8217;s old API&#8217;s support this, but their new API (Graph API) doesn&#8217;t, which means that all data is sent uncompressed. Flow was changed to use the new API as the others are getting &#8220;deprecated&#8221;.

*In effect, to download the first 15 posts in your Newsfeed at present is about 15KB. Over GPRS that takes at least 4 seconds.*  
*When compressed, the size is about 4KB, which would take about 1-1.5 seconds. **That&#8217;s a quarter of the time.***

I&#8217;ve raised a bug with Facebook therefore when (if?) they decide to fix this, you&#8217;ll all suddenly notice a big performance increase. You can help by &#8220;voting&#8221; up the bug&#8217;s importance: <http://bugs.developers.facebook.net/show_bug.cgi?id=15292>