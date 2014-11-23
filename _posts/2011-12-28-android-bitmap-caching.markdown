---
layout: post
title: Android Bitmap Caching
date: '2011-12-28 00:00:00'
---

Hey everyone,

This blog post has come about through a conversation between me and [Ander Webbs][1] on Twitter, about the usage of Reference and LruCache classes for Bitmap caching in Android. Here I&#8217;ll explain how FriendCaster caches it&#8217;s Bitmaps, and more importantly manages it&#8217;s memory so it doesn&#8217;t cause any [OOM][2]&#8216;s.  
<!--more-->

#### FriendCaster

As you can imagine, FriendCaster uses a LOT of bitmaps, and most of those are fetched from the web via HTTP. Profile Pictures, Photos, Post Photos, all of various sizes therefore we have to make sure we manage our cache properly. If you look on StackOverflow and search for &#8216;android bitmap imageview&#8217; you&#8217;ll find lots of threads about this very subject.

FriendCaster uses a two-level cache for Bitmaps:

*   First we have an in-memory cache. This will be discussed in depth below.
*   Then we have a &#8216;hard&#8217; cache on the device&#8217;s External Memory. This is pretty simple and basically just saves the images as files in a pre-defined directory, the filename is created from the URL of the image (we use a MD5 hash of the URL) so that we can fetch it back later.

#### In-memory Cache

First of all, our In-memory Cache is fairly &#8216;thin&#8217;, and by that I mean that I do not expect it to actually cache Bitmap&#8217;s much. The reason for this is because, as mentioned above, we have to keep very tight control on our memory usage. You might ask, &#8220;Why even have the cache?&#8221; Well the reason is simple, we use the cache more as a directory of Bitmap&#8217;s currently being used in the app, rather than an actual cache. For that reason, we use the tried-and-tested cache of a HashMap, with a String key, and SoftReference&#8217;d Bitmap.

There are many people on the web that say that a Collection of SoftReferences is not a cache, and I 100% agree with them. The important thing to know about Android&#8217;s VM  is that it frees SoftReferences much earlier than standard Java VMs, and for good reason, it&#8217;s usually running on a device where each app has a heap size of only 16-64MB (depending on the device). SoftReferences fit our purposes though, as they give us a nice way of finding out whether the VM has already GC&#8217;d a Bitmap or not. If it has, then we simply remove the SoftReference from the Map, if not then the ImageView gets a concrete reference to the Bitmap meaning it can&#8217;t be GC&#8217;d.

#### Android&#8217;s Bitmap

If you&#8217;ve worked with Android, you&#8217;ll almost certainly know the pitfalls of using Android&#8217;s Bitmap class. Pre-Honeycomb, the actual bitmap data is kept in native memory, but still counted against your heap size. Post-Honeycomb, the bitmap data is kept on VM heap. In my experience, this hasn&#8217;t made any improvement; in fact, on my Galaxy Nexus it actually takes longer to free Bitmaps (resulting in OOMs) than on my SGS II (Gingerbread).

So what does this mean? Well, we have to use Bitmap&#8217;s [recycle][3] method to force the Bitmap to free itself. Sounds good? Well there are a few caveats to this:

*   The first being that if you recycle an Bitmap that is currently being used by an ImageView (or any other View), you&#8217;ll get Exception galore.
*   The second, you&#8217;ll need to make sure the In-memory is Recycled-Bitmap aware. Fortunately, this is fairly easy through Bitmap&#8217;s <a href="http://developer.android.com/reference/android/graphics/Bitmap.html#isRecycled()" target="_blank">isRecycled()</a> method.

So when can you call recycle()? I hear you ask. Well in FriendCaster we have a very simple way of doing this: each Fragment keeps a reference of all the ImageView it holds. We does this with a HashSet of WeakEqualReference(see <a href="http://www.senab.co.uk/2011/12/20/dangers-of-using-weakreferencesoftreference-in-hash-based-collections/" target="_blank">here</a> for an explanation of WeakEqualReference), and every time we download an Bitmap from the Web, we add the ImageView to the HashSet. Then in the Fragment&#8217;s onDestroy method, we iterate through the HashSet and recycle each ImageView&#8217;s bitmap.

There is a small caveat to this when using ImageViews in a ListAdapter. If we&#8217;re using an ListAdapter, then the ImageView can actually download multiple Bitmaps in the lifetime of the Fragment (because the view gets recycled), therefore FriendCaster manually recycles the Bitmap whenever we hit a recycled View.

#### Alternative

A dedicated cache class was added in Honeycomb (and later added to the Compatability Package) called [LruCache][4]. On paper, this class sounds like it should help with this problem. For our purposes though, it just makes matters worse. I&#8217;m not going to go into the specifics of LruCache, you can read up on them yourself, but the main advantage is that it manages it&#8217;s cache with concrete references and removes items based on it&#8217;s items perceived size. There is a nice callback in the cache called [entryRemoved][5] which gives you the item after it&#8217;s removed, and you would think here is the perfect time to call Bitmap.recycle(), but do **not** do this. The reason is that there is no guarantee that the Bitmap isn&#8217;t being referenced by a View, and as mentioned earlier, you can&#8217;t recycle a Bitmap currently being used. Ander argued on Twitter that you can avoid this by setting the LruCache to a large enough size, but when we&#8217;re talking about Bitmaps the cache would have to be very large indeed, leading to OOMs.

#### Conclusion

We may actually come back to LruCache at a later date, but for now they do not fit our use-case. As we have to recycle all of our Bitmaps when a Fragment gets destroyed (due to memory constraints) we do not need a &#8216;proper&#8217; cache because the Bitmap will never live longer than the lifetime of the Fragment.

 [1]: http://jbthemes.com/anderweb/
 [2]: http://developer.android.com/reference/java/lang/OutOfMemoryError.html
 [3]: http://developer.android.com/reference/android/graphics/Bitmap.html#recycle()
 [4]: http://developer.android.com/reference/android/util/LruCache.html
 [5]: http://developer.android.com/reference/android/util/LruCache.html#entryRemoved(boolean, K, V, V)