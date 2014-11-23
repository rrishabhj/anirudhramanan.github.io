---
layout: post
title: 'Raspberry Pi: Overclocking'
date: '2012-05-29 00:00:00'
---

The Raspberry Pi is pretty easy to overclock due to an inbuilt config file. This morning I decided to see how far I could push my little Pi.  
<!--more-->

You can see the full list of options for the config file [here][1]. Just to make clear, overclocking shouldn&#8217;t cause any damage to your Pi (meaning your warranty is fine), overvolting on the other hand **probably will** damage it over time. If you use the overvolting options then a one-time-programmable bit is set, and your warranty is **void**. You have been warned.

#### Benchmark

From here we can start setting some clock values. To start I created a very crude calculation benchmark, calculating PI to 2000 decimal places ([thanks to this post][2]):

<pre class="brush: bash; gutter: false">time echo "scale=2000;4*a(1)" | bc -l</pre>

Which took **21.727** seconds on my stock Raspberry Pi.

#### Overclocking

The config file location is on the /boot partition if you&#8217;re using the Debian distro, but it&#8217;s not there by default. Instead we have to create it:

<pre class="brush: bash; gutter: false">sudo nano /boot/config.txt</pre>

There are three main areas we can overclock: CPU, RAM and GPU. As I&#8217;m not too interested in the GPU, I&#8217;m going to concentrate on how far I can push the CPU and RAM.

The default CPU frequency for the Pi is 700MHz, so my first try was to increase that to 800MHz by putting the following in the config.txt and rebooting:

<pre class="brush: bash; gutter: false">arm_freq=800</pre>

This produced a benchmark result of **19.023** seconds. A nice increase of ~12%, so far so good! Next I wanted to increase the SDRAM frequency from it&#8217;s stock 400MHz to 500MHz:

<pre class="brush: bash; gutter: false">arm_freq=800
sdram_freq=500</pre>

This produced a benchmark result of **18.981** seconds. Not much of an increase, which I kind of expected considering that my benchmark is more likely to be CPU bound than memory bound. I imagine for my media serving purposes that the RAM frequency increase will be beneficial though.

My final change is to increase the CPU Frequency again to 900MHz, which seems to be the limit for chips without overvolting:

<pre class="brush: bash; gutter: false">arm_freq=900
sdram_freq=500</pre>

This produced a benchmark result of **16.924** seconds. Another increase of ~11%!

#### Stability

That&#8217;s as far as I want to push my Pi for now, a ~25% increase in performance is not bad for a hour&#8217;s work! The test will be whether this is stable to run constantly.

 [1]: http://elinux.org/RPi_config.txt
 [2]: http://alien.slackbook.org/blog/calculating-pi/