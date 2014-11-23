---
layout: post
title: Android Gmail App Block 3rd Party Tools
date: '2011-07-29 00:00:00'
---

Hey everyone,

An update to the Gmail app went out today (v2.3.5) which blocks 3rd party apps from using it&#8217;s DB (ContentResolver).

The problem is to do with it&#8217;s manifest:

<pre class="brush: xml; gutter: true">&lt;permission android:label="@string/read_perm_label" android:name="com.google.android.gm.permission.READ_GMAIL" android:protectionLevel="signature" android:permissionGroup="android.permission-group.MESSAGES" android:description="@string/read_perm_desc" /&gt;
 &lt;permission android:label="@string/write_perm_label" android:name="com.google.android.gm.permission.WRITE_GMAIL" android:protectionLevel="signature" android:permissionGroup="android.permission-group.MESSAGES" android:description="@string/write_perm_desc" /&gt;</pre>

Basically, previously an app just had to declare that they use the READ_GMAIL permission in it&#8217;s Manifest and it would all work. Now though, they&#8217;ve added the `android:protectionLevel="signature"` which means that only the Gmail app itself can access it&#8217;s DB (as that&#8217;s the only thing which will have the right signature).

Which means that all the Gmail notifiers, widgets, etc in the Market will not work. Ever. The same can be said for FriendCaster&#8217;s Gmail notifications feature.

Thanks Google&#8230;.