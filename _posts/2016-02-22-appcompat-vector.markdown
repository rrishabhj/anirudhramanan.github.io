---
layout: post
title: AppCompat v23.2 — Age of the vectors
date: '2016-02-25'
cover_image: '/content/images/2016/rulers.jpg'
---
As you may have seen on the Support Lib 23.2.0 blog post, we now have compatible vector drawable implementations in the support libraries: VectorDrawableCompat and Animated VectorDrawableCompat.

Those are implemented as standalone pieces of functionality. As we know that developers want to use them from resources, we've added support for vector drawables directly into AppCompat.

There are various reasons for this integration, including:

* Allows developers to easily use `<vector>` drawables on all devices running Android 2.1 and above.
* Allows AppCompat to use vector drawables itself. This itself has shaved off around 70KB from AppCompat's AAR (~9%). That doesn't sound like a lot, but the saving is compounded for all apps which use AppCompat, on all devices. The savings quickly add up here for both storage and transfer.

## First things first

VectorDrawableCompat relies on some functionality in aapt which tells it to keep around any recently added attribute IDs which `<vector>` uses, so that they can be referenced pre-v21. This is turned on via an aapt flag (mentioned below).

Without this flag enabled, you will see the following error (or similar) when running your app on devices running KitKat or lower:

    Caused by: android.content.res.Resources$NotFoundException:
        File res/drawable-v19/abc_ic_ab_back_material.xml from drawable resource ID ...
    at android.content.res.Resources.loadDrawable(Resources.java:2097)
    at android.content.res.Resources.getDrawable(Resources.java:700)
    ...

### Enabling the flag

I'm guessing that most of you will be using Gradle so let's quickly walk through that.

If you're using v2.0 or above of the Gradle plugin, we have a handy shortcut:

{% highlight groovy %}
android {
    defaultConfig { 
        vectorDrawables.useSupportLibrary = true  
    }
}
{% endhighlight %}

If you have not updated yet, and are using v1.5.0 or below of the Gradle plugin, you need to add the following to your app's build.gradle:

{% highlight groovy %}
android {
    defaultConfig {
    // Stops the Gradle plugin's automatic rasterization of vectors
        generatedDensities = []  
    }
        
    // Flag to tell aapt to keep the attribute ids around
    aaptOptions {
        additionalParameters "--no-version-vectors"  
    }
}
{% endhighlight %}

## How do I use my own Vector assets in my app?

There are a few things to note before we start. When using AppCompat, VectorDrawableCompat is only used on API 20 and below. This means that you'll be using the framework's VectorDrawable class when running on API 21 and above. This is slightly different to the create() APIs on VectorDrawableCompat which still uses the framework implementation, but via a proxy object.

Right, so you want to use a vector asset in your app and your minSdkVersion is < 21. Great! First check the asset works on a API 21+ device, just as a sanity check.

### OK, so how do I use it?

There are two ways to use vector resources in AppCompat which work back to API 7+:

### AppCompatImageView

So you probably know that AppCompat 'injects' its own widgets in place of many framework widgets. This allows it to perform tinting and other backporty things. We've added support for VectorDrawableCompat here too with the new `app:srcCompat` attribute. You can safely use srcCompat for non-vector assets too.

Here's an example vector asset which we're actually using in AppCompat:

**res/drawable/ic_search.xml**

{% highlight xml %}
<vector xmlns:android="..."
    android:width="24dp"
    android:height="24dp"
    android:viewportWidth="24.0"
    android:viewportHeight="24.0"
    android:tint="?attr/colorControlNormal">
    
    <path android:pathData="..."
        android:fillColor="@android:color/white" />
            
</vector>
{% endhighlight %}

Using this drawable, an example ImageView declaration would be:

{% highlight xml %}
<ImageView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    app:srcCompat="@drawable/ic_search" />
{% endhighlight %}

You can also set it at run-time:

{% highlight java %}
ImageView iv = (ImageView) findViewById(...);
iv.setImageResource(R.drawable.ic_search);
{% endhighlight %}

The same attribute and calls work for ImageButton too.

### The 'magic' way

> Disclaimer: this may or may not work for you. AppCompat uses this extensively internally, but it relies on the just the right environment to actually work.

AppCompat can intercept _some_ drawable inflation from the framework. What this enables is you can use all of the standard attributes for a vector asset and everything may just work.

So let me tell you what may work:

DrawableContainers which reference other drawables resources which contain only a vector resource. For example, a StateListDrawable which references other files which contain a vector.

**res/drawable/state_list_icon.xml**

{% highlight xml %}
<selector xmlns:android="...">
    <item android:state_checked="true" android:drawable="@drawable/checked_icon" />
    <item android:drawable="@drawable/icon" />
</selector>
{% endhighlight %}

**res/drawable/checked_icon.xml**

{% highlight xml %}
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="32dp"
    android:viewportWidth="32"
    android:height="32dp"
    android:viewportHeight="32">
    
    ...
    
</vector>
{% endhighlight %}

You can then use the state list drawable in most places:

As a TextView's compound drawable:

{% highlight xml %}
<TextView android:drawableLeft="@drawable/state_list_icon">
{% endhighlight %}

As a RadioButton's button:

{% highlight xml %}
<RadioButton android:button="@drawable/state_list_icon">
{% endhighlight %}

As an ImageView's src:

{% highlight xml %}
<ImageView android:src="@drawable/state_list_icon">
{% endhighlight %}

You don't have to use StateListDrawable. It also works with InsetDrawable, LayerDrawable, LevelListDrawable and RotateDrawable containers. The only rule is that the vector needs to be in a separate file.

## What about Animated Vectors?

So far we've only talked about 'static' vector drawables. So let's talk about animated vectors. They work too in much the same way, but they're only available on API v11+. If you try to load an `<animated-vector>` on devices running API 10 or below then the system will return `null` and nothing will be displayed.

There are also some limitations to what kind of things animated vectors can do when running on platforms < API 21. The following are the things which do not work currently on those platforms:

* Path Morphing (PathType evaluator). This is used for morphing one path into another path.
* Path Interpolation. This is used to defined a flexible interpolator (represented as a path) instead of the system defined ones like LinearInterpolator.
* Move along path. This is rarely used. The geometry object can move around, along an arbitrary path.

In summary, the Animator declaration has to be valid and functional based on the platform your app will be running on.

## How does the 'magic way' even work?

You can skip this bit if you're not interested in how this is implemented, or have a weak stomach. 😷

There is no way currently within the Android platform to use custom drawable implementations from resources. So the following does not work:

**res/drawable/my_awesome_drawable.xml**

{% highlight xml %}
<my.package.superawesomedrawable xmlns:app="..."
    app:customAttr1="32dp"
    app:customAttr2="32dp">
    
    ...
    
</my.package.superawesomedrawable>
{% endhighlight %}

To re-iterate: the previous code **DOES NOT** work currently.

So you might be asking how we got custom drawables to work in AppCompat? Well there's a little known hack in the Android drawable system on API 19 and below which has been kept safe for a rainy day.

First though, a little background _(I should really do a talk on this, note to self)_. When you set attributes on a resource, that manifests itself as a [TypedArray][1] instance to the implementing View, Drawable, etc, via a call to obtainStyledAttributes(). That call chain is safely locked up, there's no way to hook into it (believe me, I've tried). TypedArray is final so we can't extend from it.

The hack I mentioned earlier (actually a bug fixed in API 21) is the fact that certain DrawableContainer implementations grab their child drawables from a call to Resources.getDrawable() instead of directly from the TypedArray.

So for example, here an excerpt from [InsetDrawable][2] in kitkat-release:

{% highlight java %}
Resources r = ...;
    
int drawableRes = a.getResourceId(android.R.styleable.InsetDrawable_drawable, 0);
    
if (drawableRes != 0) {  
   dr = r.getDrawable(drawableRes);  
}
{% endhighlight %}

Fortunately, Resources **is** something that we can hook it, but it's a bit clumsy to actually implement.

First we had to create a Resources wrapper which overrides the getDrawable() call. For the purpose of this blog post we'll call it ResourcesWrapper. Its getDrawable() first checks AppCompat's own internal drawable manager for any custom drawable handling. If AppCompat doesn't want to handle it, it just lets the framework handle it.

So we have the Resources wrapper, but we now need a way for Views to use it. We do this via a ContextWrapper which returns one of our ResourcesWrapper instances from getResources(). Then we need to make all of the Views use our new Context. We do that via two ways:

1. Every AppCompat widget (AppCompatImageView, etc) wraps it's provided Context in the constructor.
2. AppCompat's internal view inflater provides one of these ContextWrappers to every inflated view.

Fun right? As I said, this is all implementation detail so don't worry if you don't understand it. I know that some of you like to know this kind of stuff though.

---

_Cover photo: [rulers](https://flic.kr/p/mKQ2JR) by Dean Hochman_

[1]: https://developer.android.com/reference/android/content/res/TypedArray.html
[2]: https://android.googlesource.com/platform/frameworks/base/+/refs/heads/kitkat-release/graphics/java/android/graphics/drawable/InsetDrawable.java#87
