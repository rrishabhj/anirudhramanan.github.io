---
layout: post
title: FriendCaster v4.0
date: '2011-10-27 00:00:00'
---

Hey everyone,

This is a quick post to explain why you might be seeing a lot of FCs in the FriendCaster v4.0.

In terms of code, v4.0 is about 70% different to v3.x, and this is due to the fact that we&#8217;ve now moved to having both the Tablet and Phone UIs in the same code. Previously we had separate code bases for all versions, which was a nightmare for several reasons:

*   Every time I made a code change, I have to manually add it ALL code bases.
*   Our Tablet version only worked on Honeycomb Tabs
*   We were using the Android Market&#8217;s new [Multi-APK][1] functionality (which is good), but it&#8217;s not available on all Markets (Amazon for one).

<div>
  This is why we decided to merge everything together, and create one APK to distribute which will work for all devices (from Donut 1.6 up) across all screen sizes, and the result is FriendCaster v4.0. By doing this it gives us the following benefits:
</div>

<div>
  <ul>
    <li>
      Can run on all devices.
    </li>
    <li>
      We&#8217;re now using <a href="http://developer.android.com/guide/topics/fundamentals/fragments.html">Fragments</a> across everything. This is way Google want people to do Android development from now on.
    </li>
    <li>
      Will run on Ice Cream Sandwich without any changes, utilising all it&#8217;s new UI.
    </li>
    <li>
      We can now distribute a Tab version on Amazon, without creating lots of separate listings.
    </li>
    <li>
      Will make development easier, which should speed up bug fixes, etc.
    </li>
  </ul>
  
  <h3>
    Bugs
  </h3>
  
  <p>
    As you can imagine with a major update such as this, bugs can creep in. We had several rounds of beta testing, which overall went well and showed lots of bugs.  Unfortunately we didn&#8217;t catch them all though, and you may be seeing a few Force Closes in v4.0.
  </p>
  
  <p>
    Testing on Android is difficult, there are hundreds of devices using different version of Android, etc and you can never test them all. Our job with FriendCaster is even more difficult due to the fact that we rely on Facebook, who often change their API responses without telling anyone, and <strong>everyone&#8217;s</strong> Facebook data is different.
  </p>
  
  <h3>
    Summary
  </h3>
  
  <p>
    In summary, please bear with us as we fix these bugs. We&#8217;ll get a bug fix release out soon.
  </p>
</div>

 [1]: http://android-developers.blogspot.com/2011/07/multiple-apk-support-in-android-market.html