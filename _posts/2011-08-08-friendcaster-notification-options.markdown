---
layout: post
title: FriendCaster Notification Options
date: '2011-08-08 00:00:00'
---

With v3.11 of FriendCaster will come some changed notification options, and this post has been created to help you set them up. Previously we just had Periodic and Gmail alerts, and they worked well, as you may well know though, Gmail decided to [block 3rd party access][1] so FriendCaster&#8217;s Gmail alert no longer worked.

Over the past week I&#8217;ve been working on getting alternatives working for FriendCaster, and v3.11 is the first release of these.

## Periodic

Periodic is the most simple to set up, and is simple in that it simply checks Facebook every *x* minutes for any new notifications. The Free version is limited to a minimum check of every 15 minutes, Pro can go down to every minute.

***Pros:** Simple to set-up. Good battery impact if you use a low frequency (every hour+).*  
***Cons:** Not instant, therefore you&#8217;ll have to wait for notifications. High battery impact if you use a high frequency (< 15 mins).*

## C2DM (Push)

The first thing to realise is that having Push Notifications does **not** mean instant notifications. It just means that there is no polling happening on the device. Facebook provide no way of being notified when you have a notification, therefore we have a server that polls on your behalf at a specified time. This time can not be changed by the user and is currently set to 5 minutes, meaning that FriendCaster may be up to 5 minutes later in receiving the notification.

## Gmail

Gmail notifications are back and working 100%! If you haven&#8217;t set-up Gmail notifications before, please read the &#8216;Facebook Email Set-up&#8217; section below to set-up Facebook to send you emails when you get a notification. From there:

1.  Choose which Gmail account you wish you to use under &#8216;Gmail Account&#8217; (even if there&#8217;s only one, make sure you select it!)
2.  Enter your Gmail account password in. This is needed so that FriendCaster can mark any Facebook emails as read (or move to trash if you have FriendCaster Pro).
3.  If you have the Pro version, you&#8217;ll also get a couple of more options: Delete Facebook email will move the Notifications to Trash, and Periodic Check is pretty self-explanatory.

<div>
  Please note, I recommend that you create a separate Gmail account for Facebook emails. There are two reasons for this:
</div>

<div>
  <ul>
    <li>
      FriendCaster is notified <em>EVERY</em> time your unread email list changes, but it&#8217;s not told what has changed. So every time you read an email, get a new email, etc,  FC will be notified and connects to Gmail to see what has changed. If you imagine the amount of times that can happen, it&#8217;s a lot of wasted resources. If you use a separate account just for Facebook notifications though, FC will only connect when it needs to.
    </li>
    <li>
      You can also control whether you receive Gmail notifications for that account. That means that you can disable notifications for that Gmail account (in the Gmail app) and just leave FC to notify you.
    </li>
    <li>
      Make sure you update your Email Preferences at Facebook <a href="https://www.facebook.com/settings?tab=account&section=email&t">here</a>.
    </li>
  </ul>
</div>

***Pros:** Requires set-up. Good battery impact. Push Notifications.*  
***Cons:** If you receive a **lot** of notifications, you may find it to have a high battery impact.*

## K-9 Mail

In case you don&#8217;t know what K-9 is, it&#8217;s a very good open-source email client for Android ([here][2]). One of the reason it&#8217;s very good is that it has good support for Push Email (IMAP Idle) which is obviously needed. Set-up for K-9 and FriendCaster is pretty automatic, just <a href="https://github.com/k9mail/k-9/wiki/Manual" target="_blank">install and set-up K-9</a> and then select the K-9 option under &#8216;Refresh Type&#8217;.

Please note that K-9 doesn&#8217;t offer support for marking emails as read, therefore FriendCaster delete&#8217;s them instead.

***Pros:** Good for those who use K-9 already. Push Notifications if you use a email account that supports IMAP Idle (e.g. Gmail).*  
***Cons:**** Means switching Email client if you don&#8217;t already use K-9.** *

### Facebook Email Set-up

Using K-9 notifications means that the application will intercept any emails you receive from Facebook, and then refresh itself and thus notify you of any notifications. In simple terms, it gives you Push Notifications. To setup this up, there is a one time setup you need to do with Facebook:

1.  Make sure Facebook is set up to send emails to the email address you have set up above, if it’s not then this will not work for you.
2.  Open this link on Facebook. It will open up your Email Notification preferences: <a href="http://www.facebook.com/editaccount.php?notifications" target="_blank">HERE</a>.
3.  You need to tick the boxes so that you get emails from Facebook about notifications. You can see the minimum needed for FriendCaster: <a href="http://178.239.161.90/~senabcou/wp-content/uploads/2011/03/gmail_setup.png" target="_blank" rel="lightbox[880]">HERE</a>.
4.  If you&#8217;re using Gmail and have created a filter in Gmail to automatically label any Facebook emails (which I recommend), make sure you do NOT set the option to “<label for=":ao">Skip the Inbox</label> (Archive it)” or “Mark as read”.

That’s it, you should now start getting Facebook notifications!

 [1]: http://www.senab.co.uk/2011/07/29/android-gmail-app-block-3rd-party-tools/
 [2]: https://market.android.com/details?id=com.fsck.k9