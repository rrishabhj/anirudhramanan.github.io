---
layout: post
title: Android-BitmapCache v2.1
date: '2013-01-24 00:00:00'
---

<p>Android-BitmapCache has version 2.1! Hopefully by now you should have heard about Android-BitmapCache (if you haven&#8217;t, have a look at this post <a title="Android-BitmapCache… the rebirth." href="http://www.senab.co.uk/2012/12/18/android-bitmapcache-the-rebirth/" target="_blank">here</a>).<!--more--></p>

<p>Since starting development on the library, I&#8217;ve been aware that the requirement of making the usage of the library as seamless and easy as possible. The main problem is the need for a custom ImageView class. Unfortunately, there is no reliable way to be notified of when a Drawable is being used, so the need for <a href="https://github.com/chrisbanes/Android-BitmapCache/blob/master/library/src/uk/co/senab/bitmapcache/CacheableImageView.java" target="_blank">CacheableImageView</a> remains.</p>

<p>So for this release I&#8217;ve been concentrating on other parts of the library:</p>

<ul>
<li>All reference counting function has been moved to the new <a href="https://github.com/chrisbanes/Android-BitmapCache/blob/master/library/src/uk/co/senab/bitmapcache/CacheableBitmapDrawable.java" target="_blank">CacheableBitmapDrawable</a>. This means that you have direct access to all of BitmapDrawable&#8217;s function, while staying within the confines of caching.</li>
<li>Force access to the disk cache to be completed on a worker/background thread.</li>
<li>Update to v1.3.1 of DiskLruCache.</li>
<li>New version of put() and get() which allow you to specify <a href="https://developer.android.com/reference/android/graphics/BitmapFactory.Options.html" target="_blank">BitmapFactory.Options</a> decode options.</li>
<li>New contains*() methods.</li>
<li>Fix race condition which means that a Bitmap is recycled too early.</li>
<li>CacheableBitmapWrapper still exists but is simply a deprecated &#8216;dumb&#8217; sub-class of CacheableBitmapDrawable to aid migration.</li>
</ul>

<p>You can get it from GitHub:</p>
