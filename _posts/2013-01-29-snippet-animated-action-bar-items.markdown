---
layout: post
title: 'Snippet: Animated Action Bar Items'
date: '2013-01-29 00:00:00'
---

<p>This snippet is taken from some of the code I&#8217;ve recently released in <a href="http://www.senab.co.uk/2013/01/19/photup-is-now-open-source/" title="photup is now open source.">photup</a>, and shows you how to make an Action Bar item more noticeable, allowing you to lead the user onto the correct path.</p>

<!--more-->

<p><img class="alignnone size-full wp-image-2010" alt="actionbar" src="http://i1.wp.com/www.senab.co.uk/wp-content/uploads/2013/01/actionbar.jpg?fit=780%2C150" data-recalc-dims="1" /></p>

<p>You can see what I mean here:</p>

<p><span class='embed-youtube' style='text-align:center; display: block;'></span></p>

<p>So why did I add this in photup? Well, one of the problems that I had with the selection screen was that there were many many clickable items on it:</p>

<ul>
<li>Each item in the Grid is clickable twice, one for selection (the tick), and the other to &#8216;open&#8217; the image (clicking the image).</li>
<li>Filter spinner at the bottom</li>
<li>Navigation Tabs.</li>
<li>Action Bar items, with overflow.</li>
</ul>

<p>For me, the Action Bar item got lost in the melee of 29 clickable items on the screen, which meant that I had to find a way &#8216;promoting&#8217; the Action Bar item, making it obvious to the user what the next step is (or isn&#8217;t). I could have simplified the UI, adding more layers of interaction, but this would have resulted in the user having to click more to achieve an action. So instead, I decided on trying to make the Action Bar item stand out more more visually.</p>

<p>I&#8217;m not saying this technique is needed for all apps, in fact it should be needed on very few.</p>

<h2>How to do it</h2>

<p>The first thing you need to do is create the layout for the item. You can pretty much add anything here, but I wanted to mimic an standard Action Bar item as much as possible:</p>

<pre>&lt;uk.co.senab.photup.views.UploadActionBarView
    xmlns:android="http://schemas.android.com/apk/res/android"

    &lt;!-- If you use ActionBarSherlock: --&gt;
    <strong>style="?attr/actionButtonStyle"</strong>
    &lt;!-- If you're only targeting ICS (possible HC) above: --&gt;
    <strong>style="?android:attr/actionButtonStyle"</strong>

    android:layout_width="wrap_content"
    android:layout_height="match_parent"&gt;

    &lt;View
        android:id="@+id/v_action_upload_bg"
        android:layout_width="0px"
        android:layout_height="0px"
        android:layout_alignBottom="@+id/iv_action_upload"
        android:layout_alignLeft="@+id/iv_action_upload"
        android:layout_alignRight="@+id/iv_action_upload"
        android:layout_alignTop="@+id/iv_action_upload"
        android:background="@color/pressed_photup"
        android:visibility="gone" /&gt;

    &lt;ImageView
        android:id="@+id/iv_action_upload"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:src="@drawable/ic_action_upload"
        android:paddingLeft="12dp"
        android:paddingRight="12dp" /&gt;

&lt;/uk.co.senab.photup.views.UploadActionBarView&gt;</pre>

<p>You can find the source for UploadActionBarView on <a href="https://github.com/chrisbanes/photup/blob/master/client/src/uk/co/senab/photup/views/UploadActionBarView.java" target="_blank">Github</a>. It&#8217;s a simple class that just animates the background View on demand, and exposes the functionality via two simple methods: <code>animateBackground()</code> and <code>stopAnimatingBackground()</code>.</p>

<h3>Hooking it up</h3>

<p>Now we have the layouts, we just need to hook it up to the Action Bar. It&#8217;s as simple as adding the <code>android:actionLayout</code> attribute to the menu xml entry, referencing your new layout,.</p>

<pre>&lt;menu xmlns:android="http://schemas.android.com/apk/res/android" &gt;

    &lt;item
        android:id="@+id/menu_upload"
        <strong>android:actionLayout="@layout/layout_action_upload"</strong>
        android:showAsAction="ifRoom"
        android:title="@string/upload_title"/&gt;

&lt;/menu&gt;</pre>

<p>Then to start the animation, you&#8217;ll need the following in your Activity:</p>

<pre>// Keep Reference to Action Layout
private UploadActionBarView mUploadActionView;

@Override
public boolean onCreateOptionsMenu(Menu menu) {    
    getSupportMenuInflater().inflate(R.menu.your_menu_file, menu);

    // Get MenuItem, and it's Action View
    MenuItem item = menu.findItem(R.id.menu_upload);
    mUploadActionView = (UploadActionBarView) item.getActionView();

    // onOptionsItemSelected will NOT be called for a custom View,
    // so set a OnClickListener and handle it ourselves.
    mUploadActionView.setOnClickListener(this);    
}

public void onClick(View v) {
    if (v == mUploadActionView) {
        // Action Bar item has been clicked, do something...
    }
}

// When you later want to animate the background, or stop the animate, just call:
if (null != mUploadActionView) {
    // To start the animation
    mUploadActionView.animateBackground();

   // Or to stop it
   mUploadActionView.stopAnimatingBackground();
}</pre>
