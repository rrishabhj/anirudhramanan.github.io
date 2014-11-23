---
layout: post
title: New Website
date: '2013-06-17 12:04:00'
---

<p>So welcome to the new website (and new domain), powered by <a href="http://octopress.org/">Octopress</a> and hosted on Amazon <a href="http://aws.amazon.com/s3/">S3</a>.
<!-- more --></p>

<p>I've been looking for a excuse to play with S3 for a while, and here was my chance. Octopress does have a bit of a learning curve, there is a bit of Ruby magic to setup before you can post anything.</p>

<p>Some things to come:</p>

<ul>
<li>Make it look a bit more personal.</li>
<li>Styling improvements, it's basically just a tweaked version of the default Octopress theme.</li>
<li>Add Google+ comments.</li>
<li>Try <a href="http://aws.amazon.com/cloudfront/">CloudFront</a>. As this is hosted on S3, pushing to the CDN is really easy. Not sure it's worth it though at the moment.</li>
</ul>

<p>The one big downside to this setup so far is the lack of ability of remote editing. Obviously I could setup a Ruby environment on every machine I have, but that's not very practical. So I plan on creating a Dropbox + NAS solution which will means that I just have to drop a markdown file into Dropbox and my NAS will automatically pick it up, regenerate the site and publish it to S3. More to come on that in a later post.</p>
