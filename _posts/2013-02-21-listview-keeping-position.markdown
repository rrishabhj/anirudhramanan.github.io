---
layout: post
title: ListView - Keeping Position
date: '2013-02-21 00:00:00'
---

<p>Last week <a href="https://plus.google.com/107942105369460253496" target="_blank">Joaquim Verges</a> asked me a question about ListView. I&#8217;ve finally got around to writing up this solution and post.<!--more--></p>

<h2>The Problem</h2>

<p>To paraphrase Joaquim, the problem is:</p>

<blockquote>
  <p>I have a ListView populated from a CursorLoader. When I swap to a cursor with new items on top, I use <a href="https://developer.android.com/reference/android/widget/ListView.html#setSelectionFromTop(int, int)" target="_blank">setSelectionFromTop()</a> to keep the ListView at the same visible position. Unfortunately the ListView changes position and flicks in between the calls.</p>
</blockquote>

<h2>The Solution</h2>

<p>The solution is to selectively block ListView laying out it&#8217;s children. The flicker mentioned above is ListView laying out it&#8217;s children to display the new items at the old position. If we stop the children layout pass, we stop the visible change in position.</p>

<pre>public class BlockingListView extends ListView {
 
    private boolean mBlockLayoutChildren;
 
    public BlockingListView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
 
    public void setBlockLayoutChildren(boolean block) {
        mBlockLayoutChildren = block;
    }
 
    @Override
    protected void layoutChildren() {
        if (!mBlockLayoutChildren) {
            super.layoutChildren();
        }
    }
}
</pre>

<p>You can then use it as below:</p>

<pre>int firstVisPos = mListView.getFirstVisiblePosition();
View firstVisView = mListView.getChildAt(0);
int top = firstVisView != null ? firstVisView.getTop() : 0;

// Block children layout for now 
mListView.setBlockLayoutChildren(true);

// Number of items added before the first visible item 
int itemsAddedBeforeFirstVisible = ...;

// Change the cursor, or call notifyDataSetChanged() if not using a Cursor
mAdapter.swapCursor(...);

// Let ListView start laying out children again 
mListView.setBlockLayoutChildren(false);

// Call setSelectionFromTop to change the ListView position
mListView.setSelectionFromTop(firstVisPos + itemsAddedBeforeFirstVisible, top);
</pre>

<p>And that&#8217;s it!</p>
