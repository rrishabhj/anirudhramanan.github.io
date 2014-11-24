---
layout: post
title: SystemUiHelper
date: '2014-08-29 11:35:58'
---

Here's something Roman Nurik and I wrote to help with dealing with Android System UI visibility across the different API levels.

It allows you to provide a level specifying how the System UI's visibility should be changed: `LEVEL_LOW_PROFILE`, `LEVEL_HIDE_STATUS_BAR`, `LEVEL_LEAN_BACK` or `LEVEL_IMMERSIVE`. Each one is pretty self explanatory.

#### To use the helper:

{% highlight java %}
SystemUiHelper helper = new SystemUiHelper(
        activity,
        LEVEL_LEAN_BACK,   // Choose from one of the levels
        additionalFlags);  // There are additional flags, usually this will be 0

// Show the system ui
helper.show()

// Hide the system ui
helper.hide()

// Delay the hide for 2 seconds
helper.delayHide(2000);
{% endhighlight %}

You can find the code at this gist:
https://gist.github.com/chrisbanes/73de18faffca571f7292