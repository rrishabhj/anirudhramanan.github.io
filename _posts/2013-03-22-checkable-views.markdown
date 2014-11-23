---
layout: post
title: Checkable Views
date: '2013-03-22 00:00:00'
---

<p>After a month of not posting anything on here due to being busy, I&#8217;ve finally found a bit of time to write a new Snippet post. This post is all about <a href="https://developer.android.com/reference/android/widget/Checkable.html">Checkable</a>, and how to properly implement the interface so that your views display as you expect them to.<!--more--></p>

<p><img src="http://i0.wp.com/www.senab.co.uk/wp-content/uploads/2013/03/title.png?fit=750%2C150" alt="title" class="alignnone size-full wp-image-2134" data-recalc-dims="1" /></p>

<h3>What&#8217;s Checkable?</h3>

<p>Checkable is an interface which can be implemented when your View needs to change state based on a boolean switch. The framework contains a few views which implement this interface: <a href="https://developer.android.com/reference/android/widget/CheckedTextView.html">CheckedTextView</a> and <a href="https://developer.android.com/reference/android/widget/CompoundButton.html">CompoundButton</a> being the main two. There are also a number of descendent views which you&#8217;ve probably used: Checkbox, RadioButton, Switch and ToggleButton.</p>

<p>Those Views are great and be sufficient in 80% of cases, but what about when your needs are a little more complex? Well you need to implement it yourself!</p>

<h3>CheckableLinearLayout</h3>

<p>CheckableLinearLayout (full class code is at the end of the post) is a class which can be used in replacement for LinearLayout, and provides support for the Checkable interface. There are many implementations of this class on the web, but most omit the functionality that triggers the background drawable to change state:</p>

```
public void setChecked(boolean b) {
    // cut down version of method
    // ...

    refreshDrawableState();

    if (mOnCheckedChangeListener != null) {
        mOnCheckedChangeListener.onCheckedChanged(this, mChecked);
    }
}

// MAOR CODE

public int[] onCreateDrawableState(int extraSpace) {
    final int[] drawableState = super.onCreateDrawableState(extraSpace + 1);
    if (isChecked()) {
        mergeDrawableStates(drawableState, CHECKED_STATE_SET);
    }
    return drawableState;
}
```

<p>As you can see, I also added a OnCheckedChangedListener to allow you to easily hook into the check state changes.</p>

<h3>What about if I don&#8217;t want to use LinearLayout?</h3>

<p>Good question! The easy answer is that you can easily change the parent class it extends from to any View descendent! Just remember to change the class name too.</p>

<h3>Propogate child state changes</h3>

<p>Say you have a layout, with a CheckableLinearLayout as a parent, and a Checkbox as a child view. This is common scenario when you&#8217;re using ListView&#8217;s <code>CHOICE_MODE_MULTIPLE</code> mode. If the Checkbox is clicked then it&#8217;s checked state will toggle as expected, but you probably also want the parent&#8217;s state to toggle too. You could do this manually using a listener, but it&#8217;s much easier to just set ViewGroup&#8217;s <a href="https://developer.android.com/reference/android/view/ViewGroup.html#attr_android:addStatesFromChildren">addStatesFromChildren</a> property.</p>

<p>By setting this to true, the ViewGroup will copy it&#8217;s children&#8217;s state (for drawing purposes). Below is an example of how to use it in your layouts, but you can also just call <a href="https://developer.android.com/reference/android/view/ViewGroup.html#setAddStatesFromChildren(boolean)">setAddStatesFromChildren(boolean)</a></p>

```
<?xml version="1.0" encoding="utf-8">
<com.example.checkableviews.CheckableLinearLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="?android:attr/listPreferredItemHeight"
  android:addStatesFromChildren="true"         
  android:background="@drawable/checked_background">

    <CheckBox android:layout_height="wrap_content"
              android:layout_width="wrap_content" />

</com.example.checkableviews.CheckableLinearLayout>
```

<h3>Full Class Code</h3>

```
package com.example.checkableviews;

/*******************************************************************************
 * Copyright 2013 Chris Banes.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.    
 *******************************************************************************/

import android.content.Context;
import android.util.AttributeSet;
import android.util.Log;
import android.view.View;
import android.widget.Checkable;
import android.widget.LinearLayout;

public class CheckableLinearLayout extends LinearLayout implements Checkable {

    /**
     * Interface definition for a callback to be invoked when the checked state of this View is
     * changed.
     */
    public static interface OnCheckedChangeListener {

        /**
         * Called when the checked state of a compound button has changed.
         *
         * @param checkableView The view whose state has changed.
         * @param isChecked     The new checked state of checkableView.
         */
        void onCheckedChanged(View checkableView, boolean isChecked);
    }

    private static final int[] CHECKED_STATE_SET = {android.R.attr.state_checked};

    private boolean mChecked = false;

    private OnCheckedChangeListener mOnCheckedChangeListener;

    public CheckableLinearLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public boolean isChecked() {
        return mChecked;
    }

    public void setChecked(boolean b) {
        if (b != mChecked) {
            mChecked = b;
            refreshDrawableState();

            if (mOnCheckedChangeListener != null) {
                mOnCheckedChangeListener.onCheckedChanged(this, mChecked);
            }
        }
    }

    public void toggle() {
        setChecked(!mChecked);
    }

    @Override
    public int[] onCreateDrawableState(int extraSpace) {
        final int[] drawableState = super.onCreateDrawableState(extraSpace + 1);
        if (isChecked()) {
            mergeDrawableStates(drawableState, CHECKED_STATE_SET);
        }
        return drawableState;
    }

    /**
     * Register a callback to be invoked when the checked state of this view changes.
     *
     * @param listener the callback to call on checked state change
     */
    public void setOnCheckedChangeListener(OnCheckedChangeListener listener) {
        mOnCheckedChangeListener = listener;
    }

}
```
