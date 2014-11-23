---
layout: post
title: IntelliJ Copyright Profile for Android
date: '2013-06-01 00:00:00'
---

<p>One of things that I keep finding myself doing is creating copyright profiles in IntelliJ for my open source projects. If I move to a new machine, I have to do it all over again. So I thought I&#8217;d write a quick blog post, more for my reference than anything but hopefully you&#8217;ll find it helpful too.<!--more--></p>

<h2>Scope</h2>

<p>The first thing you need to do is create a &#8216;Scope&#8217;. This is basically a filter of files within your project which the copyright profile should update. For Android projects I the following filter, which updates only files in my src and res directories:</p>

<pre><code>file:src//*||file:res//*
</code></pre>

<p>You also need to make sure that you check the &#8216;Share Scope&#8217; button at the bottom is checked.<br />
<a href="http://i1.wp.com/www.senab.co.uk/wp-content/uploads/2013/06/scope1.png" rel="lightbox"><img class="alignnone size-medium wp-image-2335" alt="scope" src="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2013/06/scope1-300x213.png?fit=300%2C213" data-recalc-dims="1" /></a></p>

<h2>Copyright Profile</h2>

<p>Now we need to create the actual copyright profile. Expand the &#8216;Copyright&#8217; menu item and click on the &#8216;Copyright Profiles&#8217;, then add a new Profile. For instance I have an Apache v2.0 profile as all of my open source code is licensed that way.<br />
<a href="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2013/06/copyright_profile.png" rel="lightbox"><img class="alignnone size-medium wp-image-2339" alt="copyright_profile" src="http://i2.wp.com/www.senab.co.uk/wp-content/uploads/2013/06/copyright_profile-300x209.png?fit=300%2C209" data-recalc-dims="1" /></a></p>

<h2>Link Scope &amp; Profile</h2>

<p>Now you need to go back to main Copyright page and link the scope with the profile. Just make sure that you keep the &#8220;Default project copyright&#8221; as &#8220;No copyright&#8221;. If I set that then IntelliJ seemed to use the &#8216;all&#8217; scope rather than my &#8216;src+res&#8217; scope when updating the copyright.<br />
<a href="http://i0.wp.com/www.senab.co.uk/wp-content/uploads/2013/06/copyright_profile_2.png" rel="lightbox"><img class="alignnone size-medium wp-image-2338" alt="copyright_profile_2" src="http://i0.wp.com/www.senab.co.uk/wp-content/uploads/2013/06/copyright_profile_2-300x207.png?fit=300%2C207" data-recalc-dims="1" /></a></p>
