---
layout: post
title: Dangers of using WeakReference/SoftReference in Hash-based collections
date: '2011-12-20 00:00:00'
---

Hey everyone,

Just a quick post about something I saw this morning. In FriendCaster we use a HashMap collection of SoftReference&#8217;d Bitmap&#8217;s as an in-memory cache for images. This works fairly well as it means that we can get access to the bitmaps from memory (if they&#8217;re still in there). While looking through the source for [Reference][1] (super class of [WeakReference][2] and [SoftReference][3]) I saw that it does **not **override equals or hashcode.  
<!--more-->

This means that trying to use the Reference class for hashing/equals will not work as expected, and will not compare against it&#8217;s referenced object. Please note, this only matters if you&#8217;re using the Reference class for the hash, using a HashMap<String,SoftReference<?>> is fine as it&#8217;s the String being hashed (as the key). If you&#8217;re using a HashSet though, or call equals on the Reference, it does affect you.

Anyway, here&#8217;s a quick class I just knocked up to fix this. It basically just delegate the equals and hashCode methods to it&#8217;s reference object:

<pre class="brush: java; gutter: true">public class WeakEqualReference&lt;T&gt; extends WeakReference&lt;T&gt; {

	public WeakEqualReference(T r) {
		super(r);
	}

	@SuppressWarnings("unchecked")
	@Override
	public boolean equals(Object other) {

		boolean returnValue = super.equals(other);

		// If we&#039;re not equal, then check equality using referenced objects
		if (!returnValue && (other instanceof WeakEqualReference&lt;?&gt;)) {
			T value = this.get();
			if (null != value) {
				T otherValue = ((WeakEqualReference&lt;T&gt;) other).get();

				// The delegate equals should handle otherValue == null
				returnValue = value.equals(otherValue);
			}
		}

		return returnValue;
	}

	@Override
	public int hashCode() {
		T value = this.get();
		return value != null ? value.hashCode() : super.hashCode();
	}
}</pre>

 [1]: http://developer.android.com/reference/java/lang/ref/Reference.html
 [2]: http://developer.android.com/reference/java/lang/ref/WeakReference.html
 [3]: http://developer.android.com/reference/java/lang/ref/SoftReference.html