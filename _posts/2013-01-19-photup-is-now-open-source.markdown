---
layout: post
title: photup is now open source.
date: '2013-01-19 00:00:00'
---

Early last year I bought myself an iPhone, to see what iOS is really like. By that I mean to see what the quality of iOS apps are compared to Android&#8217;s. One of the apps that I really liked during that time was Facebook&#8217;s Camera app, which was the inspiration for photup. Another reason I created photup was that it had been a while since I created something, at the time I was working on agency apps and I wanted to keep myself &#8216;fresh&#8217;. <!--more-->

<a href="https://github.com/chrisbanes/photup" target="_blank"><img class="alignnone size-full wp-image-1901" alt="photup-opensource" src="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2013/01/photup-opensource.png?fit=750%2C250" data-recalc-dims="1" /></a>

Fast forward six months to now, and I&#8217;m happy with the results. Over <span class="highlight">80,000</span> downloads with an average rating of <span class="highlight">4.4</span>, without being mentioned, featured or anything like that. With my personal development time now becoming more limited, I need to start cutting projects, and the first of these is photup.

So photup is now open source, released under the Apache v2 licence. So ye, happy hacking.

## Code Quality

There a few &#8216;gems&#8217; in the source. I&#8217;ve listed a few here:

*   <span style="line-height: 13px;">Photos Tag which scale and pan with the photo (see <a href="https://github.com/chrisbanes/photup/blob/master/client/src/uk/co/senab/photup/views/PhotoTagItemLayout.java" target="_blank">PhotoTagItemLayout.java</a>)</span>
*   The uploading Service is solid (see <a href="https://github.com/chrisbanes/photup/tree/master/client/src/uk/co/senab/photup/service" target="_blank">here</a>).
*   The Uploads Action Bar item, which is displayed on large screens (see <a title="UploadsActionBarView" href="https://github.com/chrisbanes/photup/blob/master/client/src/uk/co/senab/photup/views/UploadsActionBarView.java" target="_blank">UploadsActionBarView.java</a>).

In any application, there are always bits of the code which you would want to improve. Here are the ones in photup:

*   Database operations. It was added after everything else, and was &#8216;hacked&#8217; in.
*   Use of AsyncTasks for most network operations (other than uploading). This was just because I was being lazy.
*   Lack of documentation or comments in the code. Again, me being lazy.