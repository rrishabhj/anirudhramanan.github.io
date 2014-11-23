---
layout: post
title: Flow v3.4 (with Screenshots)
date: '2011-03-11 00:00:00'
---

Hey everyone,

Flow v3.4 is what I&#8217;ve been working on for the past couple of days. It includes quite a few new features which I personally really like:

*   Zoomable Images. For devices with FroYo and above, you&#8217;ve got multi-touch zoom and double tap. For those on Eclair and below, you&#8217;ve just got the double tap. I&#8217;ve removed the swipe to move to the next/previous photo too, there&#8217;s now two icons at the bottom of the screen.
*   New Post Status/Link Screen! The same screen is now used for normally posting & Sharing Links & Text (via the Browser/Twitter/etc). Sharing text also now works properly. This new screen also has the added feature of being able to select the Privacy for each post. New Launcher Shortcut is available too.
*   Notifications for Friend Requests, Event Invites and Group Invites!
*   The Newsfeed is now backed up with a database cache! This is important for two reasons, firstly when you first open the Newsfeed you&#8217;ll get the previously downloaded feed items, which is better than looking at a blank screen while it downloads the new posts. Secondly it paves the way for a bigger better widget.
*   Performance increase. I&#8217;ve removed some innocent looking code that, through analysis, showed that it wasted about 1-2 seconds EACH call to Facebook. This means that Flow will be 1-2 seconds quicker when downloading information from Facebook. I&#8217;ve tested the speed compared to the official app, and while Flow is still slower (~3 secs vs ~2 secs to get the Newsfeed over WiFi), it&#8217;s no longer painfully slow.
*   Possible fix for Thumbnails not loading and constantly showing as Loading
*   I&#8217;m 99% sure I&#8217;ve also fixed the problems people have been having with Flow not refreshing in the background.
*   Profile Screen  has had the top bar removed. It didn&#8217;t do anything anyway therefore I&#8217;ve removed it to save screen space.
*   Other small UI improvements here and there.

<!-- see gallery_shortcode() in wp-includes/media.php -->

<div id='gallery-5' class='gallery galleryid-473 gallery-columns-4 gallery-size-thumbnail'>
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://i0.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/device.png' title='device' rel="lightbox[473]"><img src="http://i1.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/device.png?resize=150%2C150" class="attachment-thumbnail" alt="device" data-recalc-dims="1" /></a>
    </dt>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/photo.png' title='photo' rel="lightbox[473]"><img src="http://i0.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/photo.png?resize=150%2C150" class="attachment-thumbnail" alt="photo" data-recalc-dims="1" /></a>
    </dt>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://i1.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/postscreen.png' title='postscreen' rel="lightbox[473]"><img src="http://i1.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/postscreen.png?resize=150%2C150" class="attachment-thumbnail" alt="postscreen" data-recalc-dims="1" /></a>
    </dt>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon'>
      <a href='http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/postscreenprivacy.png' title='postscreenprivacy' rel="lightbox[473]"><img src="http://i0.wp.com/www.senab.co.uk/wp-content/uploads/2011/03/postscreenprivacy.png?resize=150%2C150" class="attachment-thumbnail" alt="postscreenprivacy" data-recalc-dims="1" /></a>
    </dt>
  </dl>
  
  <br style="clear: both" /> <br style='clear: both;' />
</div>