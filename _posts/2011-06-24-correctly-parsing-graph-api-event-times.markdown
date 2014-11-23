---
layout: post
title: Correctly Parsing Graph API Event Times
date: '2011-06-24 00:00:00'
---

One of things that really really annoys me about the Facebook [Graph API][1] is the way that they handle Events times. If you read the [docs][2] you&#8217;ll see the description:

> Start Time: A JSON string containing an ISO-8601 formatted date/time or a UNIX timestamp; if it contains a time zone (not recommended), it will be converted to Pacific time before being stored and displayed

Nice and cryptic, and an absolute pain to work with if you have a device which isn&#8217;t in the Pacific time zone. The time that is returned also doesn&#8217;t have a sense of Time Zone, when you create an Event Facebook takes the date and time, and then converts that exact time into Pacific time. For example, if you create a Event starting at 9pm on 1st July 2011 GMT, Facebook will store 9pm 1st July 2011 Pacific Time. They do not do any conversion for time zones.

Therefore if you have a global app, you&#8217;ll need to convert it to the user&#8217;s TimeZone:

#### Receiving Time&#8217;s in Epoch Format

While this isn&#8217;t necessary, it does make parsing quicker and easier. When requesting objects through the Graph, all times will be returned in ISO-8601 format (e.g. `2011-07-02T05:00:00+0000`) which is a string, and therefore slow to parse. Facebook recently added a feature where date/times will be returned in Unix timestamp format, simply add the `date_format=U` query parameter to end of your Graph call. For example:

<pre>https://graph.facebook.com/me/events?date_format=U&access_token=.....</pre>

This will return time/dates in Unix timestamp format (such as 1309572000).

#### Converting Time to User&#8217;s Time Zone

Now that we have the time in a format we can actually use, it&#8217;s time to convert it to the User&#8217;s current TimeZone. There are a couple of ways to do this, but the simplest is this:

<pre class="brush: java; gutter: true">public static long formatTimeForEvent(long pacificTime) {
	return (pacificTime + TimeZone.getTimeZone("America/Los_Angeles").getOffset(pacificTime))
		- TimeZone.getDefault().getOffset(pacificTime);
}</pre>

What this does is, convert the Pacific Time (`America/Los_Angeles`) time to GMT, and then into the User&#8217;s Time Zone via `getDefault()`.

Easy when you know how eh? This took me about a week to work out.

 [1]: https://developers.facebook.com/docs/reference/api/
 [2]: https://developers.facebook.com/docs/reference/api/event/