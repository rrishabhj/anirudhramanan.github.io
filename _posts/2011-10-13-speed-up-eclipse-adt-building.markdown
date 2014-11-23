---
layout: post
title: Speed up Eclipse + ADT building
date: '2011-10-13 00:00:00'
---

<a href="http://i1.wp.com/www.senab.co.uk/wp-content/uploads/2011/10/Screen-Shot-2011-10-13-at-14.23.38.jpg" rel="lightbox[907]"><img class="alignright size-medium wp-image-908" title="Eclipse Speed Up Option" src="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2011/10/Screen-Shot-2011-10-13-at-14.23.38-300x218.jpg?fit=300%2C218" alt="" data-recalc-dims="1" /></a>Just a quick post,

Currently whenever you save a file, Eclipse will rebuild all your app&#8217;s resources. When you&#8217;re working on an app like FriendCaster, which has a complex set of resources, it can take around 10 seconds each time I press the save button!

Anyway, after digging through the Eclipse Preferences I found this option which is disabled by default: *&#8220;Stop packaging and dexing until export or launch&#8221;. *I&#8217;ve now had this enabled for about a day and it&#8217;s definitely  quicker when saving. There are sometimes problems when I&#8217;m editing resources (layouts, drawables, etc)with the changes not reflected in the generated R.java, but a Project Clean sorts it out.

Give it a try!