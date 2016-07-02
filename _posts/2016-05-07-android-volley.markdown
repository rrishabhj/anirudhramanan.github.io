---
layout: post
title: Volley for Network Calls - Part 1
date: '2016-05-07'
---
[![GOOGLE IO 2013](https://img.youtube.com/vi/yhv8l9F44qo/0.jpg)](https://www.youtube.com/watch?v=yhv8l9F44qo)

Watch the Google IO 2013 video on [Volley](http://developer.android.com/training/volley/request.html).

[Volley](http://developer.android.com/training/volley/request.html) is a networking library, introduced in Google IO 2013 which is basically used to make network calls. Volley is extremely useful in case such as accessing JSON API, loading images using ImageLoader etc.

In the previous post, I gave an introduction on why to use volley in place of AsyncTask/HttpURLConnection for making network calls. 

NOTE : Volley should be used only to query network server, not to download any bigger file such as mp3 files etc.

### How to Install ?

Add a single line of dependency to your gradle file:

{% highlight groovy %}
compile 'com.android.volley:volley:1.0.0'
{% endhighlight %}

And, Make sure to add INTERNET permission to the manifest file, since we are accessing internet for quering data. 

{% highlight groovy %}
<uses-permission android:name="android.permission.INTERNET" />
{% endhighlight %}

### How to Use ?

* Initialize the Volley Request Queue in the Application class. Add a getter for the request queue. 

{% highlight groovy %}
private RequestQueue requestQueue;

public RequestQueue getRequestQueue() {
    if (requestQueue == null) {
        requestQueue = Volley.newRequestQueue(this);
    }
    return requestQueue;
}
{% endhighlight %}

Using this method, you will be able to get the request queue for making network calls.

* Now, lets make a network call to get a fake JSON data. We will use JsonArrayRequest provided by volley to get JSON Array data.

{% highlight groovy %}

//url for getting JSON data
String url = "http://jsonplaceholder.typicode.com/posts";

//creating a json array request for making a call
JsonArrayRequest jsonArrayRequest = new JsonArrayRequest(url, new Response.Listener<JSONArray>() {
    @Override
    public void onResponse(JSONArray response) {
        Toast.makeText(MainActivity.this, response.toString(), Toast.LENGTH_LONG).show();
    }
}, new Response.ErrorListener() {
    @Override
    public void onErrorResponse(VolleyError error) {

    }
});

//get the request queue from your Application class (you will need to typecast it to your Custom Application), and the json request to the queue.
((DemoApplication) getApplication()).getRequestQueue().add(jsonArrayRequest);

{% endhighlight %}

This is how we fetch the JSON Data from the server, this remains similar incase of JSONObjectRequest(to get json object) and StringRequest (to get string format data from the server).

### Features ?

* #### Handling Request
There are 3 types of request which volley supports : 1. StringRequest 2. ImageRequest and 3. JsonRequest. These are mainly used request in common scenario and volley can be very useful if we have such use cases.

* #### Cancelling Request
Volley also provides the support to cancel all the requests that are in the queue. RequestQueue provides a method named `cancelAll()` which will be used for the same.

* #### Setting Priority
You can easily set the priority of any request to HIGH , NORMAL and LOW depending upon the request.

* #### Retry Policy
Volley provides a default retry policy using which the request will retry itself in case it fails. You can extend the DefaultRetryPolicy and can have your own implementation.

This was a very basic explanation on why to use volley instead of HttpURLConnection for making network calls and how to use it. In the next part I will have a more deep discussion on volley library, and how to load images using volley.

Stay Tuned!
