---
layout: post
title: f-plus v3.0 Public Beta 2
date: '2011-01-15 00:00:00'
---

Hi everyone,

Here&#8217;s beta 2 which I&#8217;ve been working on over the last couple of days. On top of the features in [Beta 1][1], it includes:

*   Automatically hides &#8220;Post Status&#8221; Textbox when not at the top of the NewsFeed/Wall list.
*   Add View Profile link to ActionBar for Posts & Comments.
*   Now downloads all comments & likes (previously it would cut off at 25). Just keep in mind that the more comments there are, the longer it takes to download.
*   New Photos tab under Profiles which shows tagged photos.
*   New Event Attendee List, RSVP and clickable Location for Events. (Under Info when looking at an Event).
*   Other fixes here and there.

Beta 2: [DOWNLOAD][2]

Let me know how you get on, this will be the last beta before release.

* * *

On a separate note, I have noticed that the parsing of data from Facebook is a bit slow. This is due to the JSON parser in Android being very slow. I can improve it by moving to a different JSON parser but this would make the apk download around 700k larger.

<a id="pd_a_4392220"></a> <div class="PDS_Poll" id="PDI_container4392220" data-settings="{&quot;url&quot;:&quot;http://static.polldaddy.com/p/4392220.js&quot;}" style="display:inline-block;">
</div>

<div id="PD_superContainer">
</div>

<noscript>
  <a href="http://polldaddy.com/poll/4392220">Take Our Poll</a>
</noscript>

 [1]: http://christopherbanes.wordpress.com/2011/01/10/f-plus-v3-0-public-beta/
 [2]: http://dl.dropbox.com/u/6359346/f-plus-free-v3-b2.apk