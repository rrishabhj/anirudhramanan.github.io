---
layout: post
title: Pull-to-Refresh v1.2
date: '2012-01-26 00:00:00'
---

Hey everyone,

I&#8217;ve just tagged v1.2 of the Pull-to-Refresh library on GitHub. You may have noticed there was no v1.1 post, well that was because 1.1 was a small bugfix release. Version 1.2 though is a much bigger release.<!--more-->

Here&#8217;s the changelog:

*   New (better) way of handling Touch Events
*   Add new way for PullToRefreshListView to work. Allows the user to scroll while it&#8217;s refreshing.
*   Fix Pull from Bottom happening when the view wasn&#8217;t completely at the bottom
*   Allow outside code to update the Widget to be refreshing (see `setRefreshing()`)
*   Fix Padding being doubled when added via XML
*   New Translations

<div>
  Thanks to <a href="https://twitter.com/#!/robUx4">Steve Lhomme</a>, who provided quite a lot of code for this release.
</div>

Thanks,

You can grab it on GitHub: <https://github.com/chrisbanes/Android-PullToRefresh>