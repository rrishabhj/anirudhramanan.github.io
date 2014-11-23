---
layout: post
title: f+ v2.0.3 (and explanation of recent bugs)
date: '2010-10-16 00:00:00'
---

<p>Right,</p>

<p>First off, I want to apologise for the bugs in v2.0.x of this release. It shouldn&#8217;t have been released like this, and it should have been tested more (with different people). I&#8217;ve been working all afternoon on this and I <em>think</em> I&#8217;ve found the problem.</p>

<!--more-->

<p>f+ v.2.0.3 has just been released onto the Market. After reading through lots of log (provided by Michael Bernstein, thanks!), testing Facebook responses I think I&#8217;ve found what the error is.</p>

<p>In summary, Facebook just isn&#8217;t responding with anything for perfectly good Graph API calls to their server from certain users. Instead of a nice JSON response, f+ just gets the line &#8216;false&#8217;. Obviously this isn&#8217;t correct, therefore I&#8217;ve gone through all of the requests f+ makes to make sure they&#8217;re correct, and I&#8217;m pretty sure they are. Therefore the only thing I can think of left is that the permissions f+ requests when you login to Facebook aren&#8217;t quite enough. Therefore v2.0.3 will make you re-login on open to request extra permissions.</p>

<p>The F/C (Force Close) you&#8217;ve all been seeing is actually an F/C in the failsafe code in the Post/Comment view. It was put there as a failsafe if a non-valid response was found, it basicallylaunches the Touch website. This F/C is fixed in v2.0.3.</p>

<p>In summary, I&#8217;m still not sure whether the extra permissions will work, only you testing it can make me sure. If it doesn&#8217;t work, it looks like Facebook isn&#8217;t liking the requests f+ is making for certain users and I have no idea why. Fingers crossed then!</p>

<p>Chris</p>
