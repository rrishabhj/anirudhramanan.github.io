---
layout: post
title: Android-BitmapCache… the rebirth.
date: '2012-12-18 00:00:00'
---

<p>A while ago I wrote Android-BitmapMemoryCache, a cache based off of LruCache which would cache Android Bitmap objects in memory in a clever way. You can get the full details from this earlier <a href="http://www.senab.co.uk/2012/07/01/android-bitmap-caching-revisited/" title="Android Bitmap Caching… revisited">post</a>, but the bottom line is that it knew when a Bitmap was no longer being used, and so would recycle it.<!--more--></p>

<p>I left it like that the past 5 months and I knew it worked. I was aware though, that developers were looking for something a little more complete, so for the past few days I&#8217;ve been refactoring Android-BitmapMemoryCache into Android-BitmapCache.</p>

<h4>Android-BitmapCache</h4>

<p>So I introduce Android-BitmapCache, a new multi-level cache for Bitmaps. Currently it has two levels: memory and disk, which are exposed as a single cache for you to use as you wish.</p>

<p>The <em>memory</em> level is pretty much the same as before, nothing much new there. The new <em>disk</em> level is a cache using Jake Wharton&#8217;s <a href="https://github.com/JakeWharton/DiskLruCache">DiskLruCache</a>, which means you get all of the benefits of that library: solid implementation and bounded disk space usage.</p>

<h5>Creation</h5>

<p>It&#8217;s pretty simple to use cache. Just created a <em>BitmapLruCache</em> object and store it somewhere you can access, either the Application object or a static variable:</p>

<pre class="brush: java; gutter: true">BitmapLruCache mCache;

// This can be anywhere your app has Read/Write access
File diskCacheLocation = getExternalFilesDir(null);

// Create Builder object
BitmapLruCache.Builder builder = new BitmapLruCache.Builder();

// Enable Memory Cache, and set max size using heap size
builder.setMemoryCacheEnabled(true).setMemoryCacheMaxSizeUsingHeapSize();

// Enable Disk Cache, and set location
builder.setDiskCacheEnabled(true).setDiskCacheLocation(diskCacheLocation);

// Finally get the build BitmapLruCache
mCache = builder.build();</pre>

<h5>Usage</h5>

<p>Once your cache has been setup and is accessible, you can start using it! You should use the bundled <a href="https://github.com/chrisbanes/Android-BitmapCache/blob/master/library/src/uk/co/senab/bitmapcache/CacheableImageView.java">CacheableImageView</a> wherever possible, as the memory cache has a close relationship with it.</p>

<p>To retrieve a cached value from the given Url, clients can call:</p>

<pre>get(String)</pre>

<p>This will check all available caches for the value. There are also the <code>getFromDiskCache(String)</code> and <code>getFromMemoryCache(String)</code> which allow more granular access.</p>

<p>There are a number of update methods:</p>

<pre>put(String, InputStream) and &lt;code&gt;put(String, InputStream, boolean)&lt;/code&gt;</pre>

<p>are the preferred versions of the method, as they allow 1:1 caching to disk of the original content.</p>

<pre>put(String, Bitmap) and put(String, Bitmap, boolean)</pre>

<p>Should only be used if you can&#8217;t get access to the original InputStream.</p>

<p>You can easily have a look at the <a href="https://github.com/chrisbanes/Android-BitmapCache/tree/master/sample/src/uk/co/senab/bitmapcache/samples">sample code</a>.</p>

<h4>Future&#8230;</h4>

<p>I have previously said that this library would only be a simple caching library, and that I would not add any image fetching to it. Well to make the library easier to use for a novice I&#8217;ve going to add this in v2.1. It will not be a hugely complex implementation, but enough to get someone &#8216;going&#8217;.</p>
