---
layout: post
title: Pull-to-Refresh v1.0
date: '2012-01-05 00:00:00'
---

Hey everyone,

I&#8217;ve just tagged v1.0 of the Pull-to-Refresh on the Github [repository][1]. This is the first stable release of the library and it&#8217;s now something I feel is ready for public consumption.<!--more--> In case you don&#8217;t know about it, here&#8217;s a list of the features in the library:

*   Supports both Pulling Down from the top, Pulling Up from the bottom, and Both at the same time!
*   Has implementations for all the AbsListView classes: ListView, GridView and ExpandableListView.
*   Allows Pull-to-Refresh even when the Empty View is displayed.
*   Animation Scrolling.
*   Has an integrated End of List listener.
*   Works with Android 1.6+

That being said I have a few things planned for future releases, the main being to make the library more generic! At the moment it&#8217;s very much tied to the [AbsListView][2] classes, but it doesn&#8217;t have to be. I want to do a small bit of refactoring so that it can work with any type of View, including WebViews. You can see what stuff I have lined up on the [Issue Tracker][3].

&nbsp;

 [1]: https://github.com/chrisbanes/Android-PullToRefresh
 [2]: http://developer.android.com/reference/android/widget/AbsListView.html
 [3]: https://github.com/chrisbanes/Android-PullToRefresh/issues?labels=Feature+Request&sort=created&direction=desc&state=open&page=1