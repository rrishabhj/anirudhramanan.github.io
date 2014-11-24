---
layout: post
title: Measuring Text
date: '2014-03-27 15:08:04'
---

If you start manually drawing things to Android's [Canvas](https://developer.android.com/reference/android/graphics/Canvas.html), you will probably start to draw text as well. 

When doing so you need to know where to position the text when you draw, and to do that you will need to measure the text before drawing it, to compute the starting x/y values.

---

In an app recently I needed to draw some text centered both vertically and horizontally on the Canvas. So I started off with the following code:

{% highlight java %}
Paint mTextPaint = new Paint();
mTextPaint.setTextAlign(Paint.Align.CENTER); // Center the text

// Later when you draw...
canvas.drawText(mText, // Text to display
        mBounds.centerX(), // Center X of canvas bounds
        mBounds.centerY(), // Center Y of canvas bounds
        mTextPaint
);
{% endhighlight %}

I didn't expect this to work first-time and it didn't, producing the following:

![](/content/images/2014/3/1.png)

## Measuring text

Next I tried to position the text, calculating the text's height/width and modifying the drawing X and Y values appropriately:

{% highlight java %}
int mTextWidth, mTextHeight; // Our calculated text bounds
Paint mTextPaint = new Paint();

// Now lets calculate the size of the text
Rect textBounds = new Rect();
mTextPaint.getTextBounds(mText, 0, mText.length(), textBounds);
mTextWidth = textBounds.width();
mTextHeight = textBounds.height();

// Later when you draw...
canvas.drawText(mText, // Text to display
        mBounds.centerX() - (mTextWidth / 2f),
        mBounds.centerY() + (mTextHeight / 2f),
        mTextPaint
);
{% endhighlight %}

This time we're getting much closer, but as you can see that it's not quite centered correctly. 

![](/content/images/2014/3/2.png)

To make sure that I wasn't seeing things, I added an extra call to draw a rectangle behind the text, with the exact bounds calculated with [`Paint.getTextBounds()`](https://developer.android.com/reference/android/graphics/Paint.html#getTextBounds(java.lang.String, int, int, android.graphics.Rect)).

![](/content/images/2014/3/3.png)

As you can see, the text clearly draws outside of it's calculated bounds, both in height and width.

### Another text measuring method

It was at this point where I saw that Paint has another method for calculating text width: [`Paint.measureText()`](https://developer.android.com/reference/android/graphics/Paint.html#measureText(java.lang.String))

This method only calculates the width and not the height, so I next tried combining the two methods:

{% highlight java %}
int mTextWidth, mTextHeight; // Our calculated text bounds
Paint mTextPaint = new Paint();

// Now lets calculate the size of the text
Rect textBounds = new Rect();
mTextPaint.getTextBounds(mText, 0, mText.length(), textBounds);
mTextWidth = mTextPaint.measureText(mText); // Use measureText to calculate width
mTextHeight = textBounds.height(); // Use height from getTextBounds()

// Later when you draw...
canvas.drawText(mText, // Text to display
        mBounds.centerX() - (mTextWidth / 2f),
        mBounds.centerY() + (mTextHeight / 2f),
        mTextPaint
);
{% endhighlight %}

Which resulted in almost perfectly centered text! Phew.
![](/content/images/2014/3/4.png)