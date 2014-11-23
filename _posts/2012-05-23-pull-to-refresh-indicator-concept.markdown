---
layout: post
title: Pull-to-Refresh Indicator Concept
date: '2012-05-23 00:00:00'
---

Lots of evangelist Android developers dislike the Pull-to-Refresh pattern on Android. For instance Cyril Mottier wrote a whole [blog post][1] detailing his views and it&#8217;s well worth a read. The trouble with his arguments (in my opinion) is that he&#8217;s far too &#8216;pure&#8217; in his views on using patterns from other platforms. For me, getting something which works well and people can use is more important than keeping to Google&#8217;s [Pure Android][2] guidelines.<!--more-->

*Just to clarify, Cyril is obviously a very clever guy and you&#8217;d be a fool not to value his views. I just don&#8217;t agree with him on this one point <img src='http://i2.wp.com/www.senab.co.uk/wp-includes/images/smilies/icon_wink.gif' alt=';)' class='wp-smiley' data-recalc-dims="1" /> *

The main argument against using Pull-to-Refresh on Android is that it&#8217;s not discoverable as we don&#8217;t have an overscroll bounce like iOS does. I totally agree with this point so today I&#8217;ve quickly added something in to my Pull-to-Refresh library to help make it more discoverable, a small indicator graphic at the top/bottom that shows when a pull is available:

<a href="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2012/05/indicator_screen.png" rel="lightbox[1206]"><img class="aligncenter size-medium wp-image-1209" title="indicator_screen" src="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2012/05/indicator_screen-460x310.png?fit=300%2C202" alt="" data-recalc-dims="1" /></a>

Now it&#8217;s obviously not as elegant as iOS&#8217; bounce but it **does** work and isn&#8217;t too intrusive. At the moment I&#8217;m testing it but the code has been optimised and is usable as-is (it&#8217;s in the [dev][3] branch if you want the code). FYI, the graphic is something I knocked up quickly so will probably change when I get some time to improve it.

<span style="text-decoration: line-through;">If you&#8217;d like to test this, I&#8217;ve built a new version of the sample app for download here: <a href="http://bit.ly/Lou5lv">http://bit.ly/Lou5lv</a> <em>(the GridView sample has the Andy custom drawable).</em></span>  
**EDIT: This has now been released into v1.3.0. See here: <https://github.com/chrisbanes/Android-PullToRefresh>**

Let me know what you think!

 [1]: http://android.cyrilmottier.com/?p=598
 [2]: http://developer.android.com/design/patterns/pure-android.html
 [3]: https://github.com/chrisbanes/Android-PullToRefresh/tree/dev