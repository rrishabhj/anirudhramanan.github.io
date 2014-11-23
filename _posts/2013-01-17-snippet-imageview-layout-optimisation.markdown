---
layout: post
title: 'Snippet: ImageView Layout Optimisation'
date: '2013-01-17 00:00:00'
---

<p>Welcome to the second instalment of my new Android Snippet series. This one is a fairly short one as it points out an optimisation which was added in Android v4.0, but which can be easily backported to your applications for use in previous versions of Android.<!--more--></p>

<h2>Scenario</h2>

<p>Imagine you have a 3-column GridView, each item is properly recycled in the Adapter, and contains an ImageView filling the View. You load each ImageView with a Bitmap downloaded from the web, in a background thread, which is cached in memory and to disk, and displayed using <a href="https://developer.android.com/reference/android/widget/ImageView.html#setImageBitmap(android.graphics.Bitmap)" target="_blank">setImageBitmap()</a> or <a href="https://developer.android.com/reference/android/widget/ImageView.html#setImageDrawable(android.graphics.drawable.Drawable)" target="_blank">setImageDrawable()</a>. This isn&#8217;t an unlikely scenario as it&#8217;s employed by nearly all social and media apps (think Google+&#8217;s Album View).</p>

<p>Previous to 4.0, ImageView would do the following when you call <code>setImageDrawable()</code>:</p>

<ol>
<li>Set the new Drawable (and keep a reference to it).</li>
<li>Request a layout pass.</li>
<li>Invalidate itself to display the new contents.</li>
</ol>

<p>Item 1 and 3 are obviously necessary for an ImageView (otherwise you won&#8217;t see the new Drawable), item 2 is not always needed though. The layout pass is used to position the Drawable within the ImageView (as well as other things), but is only needed if it&#8217;s contents have changed size.</p>

<p>Imagine that the images you are displaying are all of equal size, which is pretty likely when you think that you are most likely going to be displaying logical groups of images (e.g. contact photos, MediaStore thumbnails, etc), this means that after you&#8217;ve layed out the first Drawable, you can re-use the same positioning for any subsequent Drawable with the same size.</p>

<h2>Result</h2>

<p>The result? Scrolling performance will be improved, <a href="http://i.imgur.com/S0zl2.gif" target="_blank" rel="lightbox[1831]">#win</a>.</p>

<h2>Code</h2>

<p>This code simply backports the optimisation from 4.0 for apps running on older devices. Just use this ImageView class instead of the framework version in your app.</p>

<pre>import android.content.Context;
import android.graphics.drawable.Drawable;
import android.os.Build.VERSION;
import android.os.Build.VERSION_CODES;
import android.util.AttributeSet;
import android.widget.ImageView;

public class OptimisedImageView extends ImageView {

    private boolean mIgnoreNextRequestLayout = false;

    public OptimisedImageView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    public void setImageDrawable(final Drawable newDrawable) {
        if (VERSION.SDK_INT &lt; VERSION_CODES.ICE_CREAM_SANDWICH) {

            // The currently set Drawable
            final Drawable oldDrawable = getDrawable();

            if (null != oldDrawable && oldDrawable != newDrawable) {
                final int oldWidth = oldDrawable.getIntrinsicWidth();
                final int oldHeight = oldDrawable.getIntrinsicHeight();

                /**
                 * Ignore the next requestLayout call if the new Drawable is the
                 * same size as the currently displayed one.
                 * */
                mIgnoreNextRequestLayout = oldHeight == newDrawable.getIntrinsicHeight()
                        && oldWidth == newDrawable.getIntrinsicWidth();
            }
        }

        // Finally, call up to super
        super.setImageDrawable(newDrawable);
    }

    @Override
    public void requestLayout() {
        if (!mIgnoreNextRequestLayout) {
            super.requestLayout();
        }

        // Reset Flag so that the requestLayout() will work again
        mIgnoreNextRequestLayout = false;
    }

}</pre>

<h2>TL;DR</h2>

<p>If you&#8217;re using an ImageView in your AdapterView, and the images are all of equal size, considering using the code above.</p>

<p>****** Or any of the other setImage* methods.*</p>

<div class="bean-note">
  A previous version of this post stated that the change came in v4.2, I was wrong. It in fact came in v4.0 with Ice Cream Sandwich (it may have actually come in Honeycomb but I can&#8217;t find the source to check). Thanks to <a href="https://plus.google.com/118417777153109946393/" target="_blank">Cyril Mottier</a> for pointing this out.
</div>
