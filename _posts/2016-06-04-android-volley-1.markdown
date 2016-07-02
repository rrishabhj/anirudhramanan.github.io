---
layout: post
title: Volley for Network Calls - Part 2
date: '2016-06-04'
---
[![GOOGLE IO 2013](https://img.youtube.com/vi/yhv8l9F44qo/0.jpg)](https://www.youtube.com/watch?v=yhv8l9F44qo)

Watch the Google IO 2013 video on [Volley](http://developer.android.com/training/volley/request.html).

Hello All. Hope you are doing well.

In the previous post ie (Volley for Network Calls - Part 1) I gave a brief intro on what volley is and its usage. In this tutorial, we will learn more advanced features that volley provides eg: how to load images using volley.

NOTE : Those who haven't gone through the part-1, make sure you go through the part-1 for the basic understanding of volley.

### How to Load Images using Volley ?

* Initialize the following in the Application class.

First, initialize the Volley Request Queue

    {% highlight groovy %}
    private RequestQueue requestQueue;

    public RequestQueue getRequestQueue() {
      if (requestQueue == null) {
          requestQueue = Volley.newRequestQueue(this);
      }
      return requestQueue;
    }
    {% endhighlight %}

Second, initialize the Volley ImageLoader

    {% highlight groovy %}
    private ImageLoader imageLoader;

    public ImageLoader getImageLoader() {
        if(imageLoader == null) {
            imageLoader = new ImageLoader(requestQueue,
                new ImageLoader.ImageCache() {
                    private final LruCache<String, Bitmap>
                        cache = new LruCache<String, Bitmap>(20);

                    @Override
                    public Bitmap getBitmap(String url) {
                        return cache.get(url);
                    }

                    @Override
                    public void putBitmap(String url, Bitmap bitmap) {
                        cache.put(url, bitmap);
                    }
                });
        }
        return imageLoader;
    }
    {% endhighlight %}

* In the layout file, add a NetworkImageView provided by volley. 

{% highlight groovy %}
<com.android.volley.toolbox.NetworkImageView
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/imageView"/>
{% endhighlight %}

* In the java file, add a method to load the image

{% highlight groovy %}
private void loadImage(){
    //url of the image you want to load
    String url = editTextUrl.getText().toString();
    if(url.equals("")) {            
        return;
    }
 
    imageLoader = ((DemoApplication) getApplication()).getImageLoader();
    imageLoader.get(url, ImageLoader.getImageListener(imageView,
            R.drawable.image, android.R.drawable.ic_dialog_alert));
    imageView.setImageUrl(url, imageLoader);
}
{% endhighlight %}

That's it. You are done. 

### What is LRU Cache ?

The volley provides a standard implemenation of the Cache that you can use. For using ImageLoader, we must provide a LRU Bitmap Cache. Here's the sample implementation given in the Offical Android Site that we can use.

{% highlight groovy %}
public class LruBitmapCache extends LruCache<String, Bitmap>
        implements ImageCache {

    public LruBitmapCache(int maxSize) {
        super(maxSize);
    }

    public LruBitmapCache(Context ctx) {
        this(getCacheSize(ctx));
    }

    @Override
    protected int sizeOf(String key, Bitmap value) {
        return value.getRowBytes() * value.getHeight();
    }

    @Override
    public Bitmap getBitmap(String url) {
        return get(url);
    }

    @Override
    public void putBitmap(String url, Bitmap bitmap) {
        put(url, bitmap);
    }

    // Returns a cache size equal to approximately three screens worth of images.
    public static int getCacheSize(Context ctx) {
        final DisplayMetrics displayMetrics = ctx.getResources().
                getDisplayMetrics();
        final int screenWidth = displayMetrics.widthPixels;
        final int screenHeight = displayMetrics.heightPixels;
        // 4 bytes per pixel
        final int screenBytes = screenWidth * screenHeight * 4;

        return screenBytes * 3;
    }
}
{% endhighlight %}

You can modify the LRUCache based upon your app requirements.

### [Official Link](https://developer.android.com/training/volley/request.html)

This was all about Volley. So, you now know why volley is such a very powerful library for making network calls, and makes it so easy with few lines of code. 

Feel free to ask any query regarding volley or its usage.

Contact Address : anirudhramanan23@gmail.com

Thank you.
