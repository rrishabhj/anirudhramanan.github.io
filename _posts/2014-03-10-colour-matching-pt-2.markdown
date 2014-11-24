---
layout: post
title: Colour Matching - Part 2
date: '2014-03-10 15:48:00'
---

_This post is the second part in a two part series. You can find the first post [here](http://chris.banes.me/2014/02/18/colour-matching/)._

---

Carrying on from the previous post, we now have the palette of colours which make up our chosen image. The palette can be retrieved by calling `getQuantizedColors()` on our `MedianCutQuantizer`. We could just sort this array based on the count and job done, use the colours in descending order. Unfortunately it turns out that most images use a lot of two colours: white and black (or close shades), hardly the colours to make your app stand out. This means that we need to think a bit more about which colours to pick out.

For my app I set out wanting the following sub-palette:

 * A main accent colour, ideally colourful.
 * A secondary accent colour, which is different to the primary.
 * A tertiary accent colour, which has enough contrast from both the primary & secondary.
 * A main text colour which has enough contrast from the main accent colour to be readable.
 * A secondary text colour which is either black/white. Which depends on the brightness of the main accent colour so that it's readable.

This post is all about how to select these colours.

## Accent Colours

For my requirements above, I decided to sort them using an average (mean) of the following characteristics:

 * Colorfulness
 * Count (population)
 
### Colourfulness

This turned out to pretty easy actually. First I converted the colour's RGB value to HSV using the built-in Android method [Color.RGBToHSV()](https://developer.android.com/reference/android/graphics/Color.html#RGBToHSV(int, int, int, float[])). If you do not know about HSV then you can read up on it [here](http://en.wikipedia.org/wiki/HSL_and_HSV).

In simple terms, it's a cylindrical representation of the RGB colour space which represents colour via three axis: **H**ue, **S**aturation and **V**alue (brightness).

<caption>
  <img src="/content/images/2014/3/HSV_color_solid_cylinder_alpha_lowgamma.png" />
  <figcaption>
      The HSV colour space. Taken from [Wikipedia](http://en.wikipedia.org/wiki/File:HSV_color_solid_cylinder_alpha_lowgamma.png)
  </figcaption>
</caption>

To calculate colourfulness I used a simple product of the saturation and value. To my eyes the most colourful values are when both of these values are high.

```
public float[] getHsv() {
    float[] hsv = new float[3];
    Color.RGBToHSV(r, g, b, hsv);
    return hsv;
}

public float calculateColorfulness() {
    float[] hsv = getHsv();
    return hsv[1] * hsv[2];
}
```

This resulted in a colorfulness value in the range 0.0 - 1.0.

### Count

Remember that each colour has a count attached to it? We can use that to determine how populous that colour is in the palette, making sure that it is in the range 0.0 - 1.0.

Say we have the following very simple palette:

```
|  Color   |  Count  |
----------------------
|  White   |   200   |
|  Purple  |   175   |
|  Black   |   150   |
|  Red     |   125   |
|  Orange  |   100   |
|  Blue    |    50   |
----------------------
|  Total   |   800   |
```

We could calculate the colour's percentage as it’s ratio within the whole picture. In the example above there are 800 pixels so using purple as an example, the colour's percentage would `175 / 800 = ~0.22`. Unfortunately this generated values which are too small, a colour would only be reach 1.0 if it was the only colour in the picture.

Instead we can calculate the ratio based on the most populous colour within the palette. Using the example palette above, white is the most populous colour so purple’s percentage is `175 / 200 = 0.87`. Much more indicative of a colour's relative population.

### Final value

Using these values we can combine them into one final value. A simple mean works OK but it turns out that monochrome colours feature highly because they tend to be highly populous. To combat this we can weight the characteristic(s) we desire so that they feature with greater importance, in this case we boost colourfulness:

```
static float weightedAverage(float... values) {
    assert values.length % 2 == 0;
    
    float sum = 0;
    float sumWeight = 0;

    for (int i = 0; i < values.length; i += 2) {
        float value = values[i];
        float weight = values[i + 1];
        sum += (value * weight);
        sumWeight += weight;
    }
    return sum / sumWeight;
}

static float calculateColorWeight(ColorNode node, final float maxCount) {
    return weightedAverage(
            // Colorfulness has a weight of 2
            ColorUtils.calculateColorfulness(node), 2f,
            // Count has a weight of 1
            (node.getCount() / maxCount), 1f
    );
}
```

The final thing was to sort the colors using this calculated weight, in descending order. We now have our weighted palette.

## Primary Accent Colour

This is simple, use the first colour from our sorted weighted palette.

## Secondary Accent Colour

From my requirements above, this needs to be different from the primary accent chosen above, ideally with a sufficiently different hue. Again we will use the **HSV** value, finding the first colour which has a sufficiently different hue value.

```
// Hue is calculate in degrees (0-360)
private static final int SECONDARY_MIN_DIFF_HUE_PRIMARY = 120;

...

final float primaryHue = primary.getHsv()[0];

// Find the first color which has sufficient different hue from the primary
for (ColorNode candidate : mWeightedPalette) {
    final float candidateHue = candidate.getHsv()[0];

    // Calculate the difference in hue, if it's over the threshold return it
    if (Math.abs(primaryHue - candidateHue) >= SECONDARY_MIN_DIFF_HUE_PRIMARY) {
        return candidate;
    }
}

// If we get here, just return the second weighted color
return mWeightedPalette[1];
```

## Tertiary Accent Colour

This is very similar to the secondary accent colour above, but instead of checking the hue we check the difference in contrast in both of it's preceding colours (primary & secondary).

```
// Contrast values are in the range 0-255.
private static final int TERTIARY_MIN_CONTRAST_PRIMARY = 20;
private static final int TERTIARY_MIN_CONTRAST_SECONDARY = 90;

...

// Find the first color which has sufficient contrast from both the primary & secondary
for (ColorNode color : mWeightedPalette) {
    if (ColorUtils.calculateContrast(color, primary)
                >= TERTIARY_MIN_CONTRAST_PRIMARY
            && ColorUtils.calculateContrast(color, secondary)
                >= TERTIARY_MIN_CONTRAST_SECONDARY) {
        return color.getRgb();
    }
}

// We couldn't find a colour. In that case use the primary colour, modifying it's
// brightness by 45%
return ColorUtils.changeBrightness(secondary.getRgb(), 0.45f);
```

But wait, where has this `calculateContrast()` method come from? This one actually took me a while to work out how to get right and mainly comes from this great article on [color contrast](http://optional.is/required/2011/01/12/maximum-color-contrast).

In the end I converted each RGB colour into the YIQ colour space, only caring about the **Y** (luma) value. You can then compare two colour's luma to get the difference in brightness and apply a threshold.

```
/**
 * @return difference in luma. Possible values are 0 (no difference) to 
 *         255 (max difference).
 */
private static final int calculateContrast(int rgbColor1, int rgbColor2) {
    return Math.abs(calculateYiqLuma(rgbColor1) - calculateYiqLuma(rgbColor2));
}

/**
 * @return luma value. Values are in the range 0-255.
 */
public static final int calculateYiqLuma(int color) {
    return (299 * Color.red(color) + 587 * Color.green(color) + 114 * Color.blue(color)) / 1000;
}
```

## Finally, the code

Remember in the last post when I said that I would include some code? Well here you go:
https://gist.github.com/chrisbanes/ba8e7b9ec0e40f6949c6

It probably won't run as it is, you'll need to make modifications to include it in your app. This is in on purpose to make you think about how to integrate the functionality.

All of guts are there though, you'll just need to think about the integration.

Have fun!
