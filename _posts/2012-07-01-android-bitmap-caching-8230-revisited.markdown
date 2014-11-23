---
layout: post
title: Android Bitmap Caching… revisited
date: '2012-07-01 00:00:00'
---

<p>You may have read my <a href="http://www.senab.co.uk/2011/12/28/image-caching/" target="_blank">last post</a> on my techniques in Friendcaster for caching Bitmaps. In that post I argued against using LruCache and instead advocated the use of a thin cache based on SoftReferences.<!--more--> The main reason I argued against LruCache was because of this:</p>

<blockquote>
  <p>There is a nice callback in the cache called entryRemoved which gives you the item after it’s removed, and you would think here is the perfect time to call Bitmap.recycle(), but do <strong>not</strong> do this. The reason is that there is no guarantee that the Bitmap isn’t being referenced by a View, and you can’t recycle a Bitmap being used.</p>
</blockquote>

<p>For a project I&#8217;m working on at the moment, I&#8217;m spending a lot of time making the UI as &#8216;<a href="http://www.youtube.com/watch?v=Q8m9sHdyXnE" target="_blank">buttery</a>&#8216; as possible and one way I&#8217;m trying to achieve that is with a more concrete caching system, so I decided to revisit LruCache to see if I could fix the problems.</p>

<h4>Android-BitmapMemoryCache</h4>

<p>What I come up with is actually <strong>very</strong> simple.</p>

<ul>
<li>First I created a wrapper class for Bitmap which keeps track of whether it&#8217;s Bitmap is displayed at a particular time. It also keeps track of whether it is referenced by a cache. If the wrapper is not being displayed or cached, then it calls Bitmap.recycle().</li>
<li>Then I wrote a ImageView which uses this wrapper object, and sets the &#8216;being displayed&#8217; flag appropriately.</li>
<li>Finally I extended LruCache to create BitmapLruCache, which sets the &#8216;being cached&#8217; flag in put() and entryRemoved().</li>
</ul>

<p>I could now explain how it works, but it&#8217;s probably better if I just point you to the code:<br />
<a target="_blank"      class="btn    " href="#"><span>Android-BitmapMemoryCache</span></a></p>

<h4>Problems</h4>

<p><del>There is a small problem with this method: if entryRemoved() is called on a wrapper object and the bitmap is being displayed, then we can&#8217;t do anything with it (it&#8217;s just de-referenced). That means that the bitmap will not be explicitly recycled by the cache. If anyone can think of a way to fix this let me know.</del><br />
This problem has now been fixed (thanks to <a href="https://plus.google.com/u/0/106557483623231970995" target="_blank">Jesse Wilson</a>), and the post updated where necessary.</p>
