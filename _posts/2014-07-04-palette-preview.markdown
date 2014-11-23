---
layout: post
title: Palette preview
date: '2014-07-04 11:21:36'
---

Tried the Android L preview SDK? You may have noticed a new support library called Palette.

Palette allows you to extract colors from images for use in your UI (sound familiar?).

Anyway I have made a mistake and not pushed any javadoc for it. So here is my quick overview until we push something official.

_Please be aware though that Palette's API is changing for the full L release (not much though)._

### Generating a palette
The first step is to generate a Palette instance from a `Bitmap`. We have four related ways to do this:

```
// Synchronous methods.
// --------------------------------
// These should be used when you have access to the underlying image loading thread.
// Picasso allows this through a Transformation. For other libraries, YMMV.

// Uses the default palette size (16).
Palette p = Palette.generate(bitmap);

// Allows you to specify the maximum palette size, in this case 24.
Palette p = Palette.generate(bitmap, 24);


// Asynchronous methods
// --------------------------------
// This is the quick and easy integration path. Internally uses an AsyncTask so 
// this may not be optimal (since you're dipping in and out of threads)

// Uses the default palette size (16).
Palette.generateAsync(bitmap, new Palette.PaletteAsyncListener() {
    @Override
    public void onGenerated(Palette palette) {
       // Here's your generated palette
    }
});

// Allows you to specify the maximum palette size, in this case 24.
Palette.generateAsync(bitmap, 24, new Palette.PaletteAsyncListener() {
    @Override
    public void onGenerated(Palette palette) {
       // Here's your generated palette
    }
});
```

### Using the palette

When a palette is generated, it tries to pick six colors which match certain criteria:

* Vibrant. `palette.getVibrantColor()`
* Vibrant dark. `palette.getDarkVibrantColor()`
* Vibrant light. `palette.getLightVibrantColor()`
* Muted. `palette.getMutedColor()`
* Muted dark. `palette.getDarkMutedColor()`
* Muted light. `palette.getLightMutedColor()`

Which one you choose depends on your use case. Vibrant and Dark Vibrant are the ones that developers will use mostly though.

So to use a generated palette item:

```
PaletteItem item = palette.getVibrantColor();
if (item != null) {
    view.setBackgroundColor(item.getRgb());
}
```

Please note that if Palette can not find a color which matches the criteria then it will return null. This is why there is a null check above.

#### Palette size

You may have seen above that you can specify the palette size. The higher the number, the longer it takes to generate a palette. The lower the number, the less colors we have to choose from. The best number to use depends on the image type:

* Contact images/avatars: optimal values are 24-32
* Landscapes: optimal values are 8-16

The default value is 16 which is good compromise and works well in most situations.

### Banesy Top Tip
![](/content/images/2014/Jul/gif_150x221_64e5de.gif)

You should be caching Palette instances as they are non-trivial to generate. On my Nexus 5 a Palette typically takes 20-30ms to generate.

### Done.
So there you go. If you have any specific questions on Palette, add a comment below.