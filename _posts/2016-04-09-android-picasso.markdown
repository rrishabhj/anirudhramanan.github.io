---
layout: post
title: Using Picasso for Image Loading
date: '2016-04-09'
cover_image: '/content/images/2016/picasso.jpg'
---
[Picasso](http://square.github.io/picasso/) is a android library used for loading and caching images. It was created by [Square](http://square.github.io/). Picasso is a complete library, from making connection to the server for image and loading them in the app, thus reducing the amount of code to be written for the same.

This is a small tutorial on how to integrate Picasso into your app to ease the process of image loading and caching.

### Gradle Dependency

Just a single line of code to your dependency block in build.gradle, and you are done
{% highlight groovy %}
    compile 'com.squareup.picasso:picasso:2.5.2'
{% endhighlight %}

### Demo Implementation

Start by creating an android project, with an ImageView in your layout file. Add few lines of code to your java file to load images in the imageview using Picasso.

{% highlight groovy %}
    Picasso.with(context).load("your_image_url").into(imageView);
{% endhighlight %}

#### Caching Images

Many issues such as image caching in memory / disk are all handled by Picasso. You can also provide your own logic of caching images for your app.

So, if you do not want your picasso request to be cached into memory, ````MemoryPolicy.NO_CACHE```` will do it for you.
{% highlight groovy %}
    Picasso  
        .with(context)
        .load("your_image_url")
        .memoryPolicy(MemoryPolicy.NO_CACHE)
        .into(imageView);
{% endhighlight %}

#### Transforming Images

Picasso also helps in transforming images to better fit into your layout.

To reduce the size of a image, we can use ````resize(width,height)```` for the same.
{% highlight groovy %}
    Picasso.with(context)
        .load("your_image_url")
        .resize(50, 50)
        .centerCrop()
        .into(imageView)
{% endhighlight %}

#### Placeholder and Error Images

Picasso provides an optional place holder image that will be displayed until your image is loaded, and error image is displayed when there is any error (network error etc) while loading the image.

{% highlight groovy %}
    Picasso.with(context)
        .load("your_image_url")
        .placeholder(R.drawable.placeholder)
        .error(R.drawable.error)
        .into(imageView);
{% endhighlight %}

This is a very useful library for loading and caching images with out any external implementation.

### Development Purpose

For developement purpose ie debug build, you can enable ````setIndicatorsEnabled(true)```` which displays a small color band on the top left of image view.

* Red color : Image is loaded from the network
* Blue color : Image is loaded from the disk
* Green color : Image is loaded from the memory. 

Make sure you disable this option in the release build.

Easy isn't it ? Just few lines of code ,and you are done. Now, no need to write complex code for caching images. Picasso provides it all.

