---
layout: post
title: Notification Checking via Gmail Label
date: '2010-12-23 00:00:00'
---

A lot of people have asked for F-Plus to check a specified label rather than the Inbox because the Facebook emails are making your Inboxes cluttered. I completely understand this and have wondered the same thing myself. Unfortunately after spending a night or so messing with the Gmail application, there is no way of doing this nicely.

Now we&#8217;re going to get a bit technical here so don&#8217;t worry if you don&#8217;t understand the next paragraph. There are two ways of detecting when Gmail has new email: a BroadcastReciever and a ContentObserver.

*   BroadcastReceiver: At the moment F-Plus uses BroadcastReceiver&#8217;s as they are by far the best way of detecting, basically F-Plus registers to receive Gmail&#8217;s intent and Android automatically starts my receiver and it checks the Gmail database for messages. This is great because it works all the time, theres no long-running Services that get destroyed. This is limited though as the Gmail app only send Intents out when there&#8217;s new Inbox messages.
*   ContentObserver: These are similar to BroadcastReceiver expect you register them to a ContentResolver (think of it as a fancy database). These are more powerful as F-Plus could be notified on every change in the database (including labels, etc). The (major) downside is that you need a long-running Service to hold the ContentObserver, and these can be destroyed at any time by the Android framework (the longer a Service has been alive, the more likely it is to be destroyed).

Therefore I could write a Service with a ContentObserver and you&#8217;d get label notifications. *But* the service would eventually get killed and I&#8217;d get lots of reviews and people emailing me telling me the app doesn&#8217;t work as you&#8217;re not getting notifications.

**In short: checking via a label is not something I&#8217;m going to add anytime soon. My advice is to get the Paid version of F-Plus and uses it&#8217;s Automatic Archive feature to help de-clutter your Inbox.**