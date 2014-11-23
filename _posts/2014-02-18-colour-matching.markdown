---
layout: post
title: Colour Matching
date: '2014-02-18 10:14:12'
---

<p>Android 4.4 introduced more subtle use of colour throughout the OS and provided new guidance on using <a href="http://developer.android.com/design/style/branding.html#color">colour</a> in your applications to provide better branding (see this <a href="http://www.youtube.com/watch?v=6QHkv-bSlds&amp;list=PLWz5rJ2EKKc8j2B95zGMb8muZvrIy-wcF&amp;feature=share&amp;index=3&amp;t=3m25s">episode</a> of ADiA for more discussion). This means that the onus is now on you as a designer/developer to inject some colour into your app to make it stand out.<!--more--></p>

> Use your brand color for accent by overriding the Android framework's default blue in UI elements. <cite>Android Design site</cite>

<p>A simple way to do this is by add a custom background to your Action Bar, but I wanted something a bit more dynamic for an hobby app I'm currently writing. One of the best examples of adapting to colour is iTunes which picks out the colour scheme from an album's artwork, and then colours the tracklist popout to suit.</p>

![iTunes album view](https://s3.amazonaws.com/chrisbanesme/color-matching-itunes.png)

<p>So I set about implementing this on Android.</p>

<h2>Basic theory</h2>

<p>After searching around on the web I found quite a few open source implementations in other languages. The best implementation I found was a Javascript library called <a href="http://lokeshdhakar.com/projects/color-thief">Color Thief</a> which taught me the basics of this technique which I've tweaked to my needs.</p>

<h2>Quantize</h2>

<p>The first step is the quantization of your source image. In layman terms, this mean decreasing the number of colours used in the image. If you like animated GIFs then you will have seen the effect as GIFs can only have a 8-bit palette, therefore can only use up to 256 discrete colours per frame.</p>

<p>For our purpose, I needed to reduce the palette down even further to a small amount of dominant colours. From there we can take the generated palette and work out the important colours. More on that later.</p>

<p>For now though I had to decide which quantization algorithm to use. Color Thief uses a modified version of Median Cut Quantization (MCQ) ingeniously called Modified Median Cut Quantization (MMCQ). You can find more information on MMCQ <a href="http://www.leptonica.com/color-quantization.html">here</a>. Other popular quantization techniques include <a href="http://members.ozemail.com.au/~dekker/NEUQUANT.HTML">NeuQuant</a> and <a href="http://www.cubic.org/docs/octree.htm">OctTree</a></p>

<p>I found a Java MCQ implementation on <a href="https://github.com/biometrics/imagingbook/blob/master/src/color/MedianCutQuantizer.java">GitHub</a> from the book <a href="http://books.google.co.uk/books?id=s5CBZLBakawC">Principles of Digital Image Processing</a> which was easy to adapt for Android.</p>

<p>I decided to go with MCQ as it has a few nice characteristics for my purposes:</p>

<ul>
<li>It's quick. Much quicker than NeuQuant, and much much quicker than OctTree. This is obviously important when we're running on a handheld device.</li>
<li>Internally it's uses a histogram, therefore each colour (cube) has a count attached to it. This is handy later on for sorting.</li>
</ul>

<p>MCQ does not produce the best quality images though, but as I only need the colour palette and don't display the quantized image; that's fine.</p>

<h3>Results</h3>

<p>So here are the results, using Color Thief's example images. Remember that MCQ uses a histogram? Well we can sort the palette based on how often the colour is used, and it turns out that a simple sorted list of the palette seems to work OK. This can definitely be improved upon though.</p>

<figure>
  <img src="https://s3.amazonaws.com/chrisbanesme/color-matching-result-1.jpg" />
  <figcaption>
    Result 1
  </figcaption>
</figure>

<figure>
  <img src="https://s3.amazonaws.com/chrisbanesme/color-matching-result-2.jpg" />
  <figcaption>
    Result 2
  </figcaption>
</figure>

<figure>
  <img src="https://s3.amazonaws.com/chrisbanesme/color-matching-result-3.jpg" />
  <figcaption>
    Result 3
  </figcaption>
</figure>

<p>The results are slightly different from what Color Thief produces:</p>

<ol>
<li>Mine wins as it picks out the blue as the dominant colour.</li>
<li>Color Thief wins as it picks out the blue, silver and green as the top colours.</li>
<li>Color Thief wins as it does not pick out varying shades of dark grey. Something to improve upon.</li>
</ol>

<p>Part 2 of this post will be talk about how to better pick out the dominant colours from the palette, and will be published next week.</p>

<h3>Example</h3>

<p>Here's the technique being used in my hobby app. It generates the colour palette from a movie's poster, and then updates the rating bar to match.</p>

<iframe width="100%" height="400" src="//www.youtube.com/embed/eBkHg4ZU_zc?rel=0" frameborder="0" allowfullscreen></iframe>

**EDIT**: You find the second part of this post [here](http://chris.banes.me/2014/03/10/colour-matching-pt-2/). 
