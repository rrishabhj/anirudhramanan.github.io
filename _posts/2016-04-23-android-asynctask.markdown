---
layout: post
title: Android Async Task
date: '2016-04-24'
cover_image: '/content/images/2016/asynctask.jpg'
---
[AsyncTask](http://developer.android.com/intl/ko/reference/android/os/AsyncTask.html) is basically an abstract class provided by Android , which allows you to run long/heavy task in the background thread, without interrupting the main UI Thread.

### When to use ?

Async Task are used when you need to run some heavy task in the background. For eg: If you want to download some files from the server, you will be using Async Task for the same. NOTE : For this scenario, [Volley](http://developer.android.com/intl/ko/training/volley/index.html) will be a better option (will discuss this in more detail in the next tutorial)

Now, as soon as it gets the result, it will update the UI Thread without interrupting the Main Thread.

### How to use ?

* Create a class for AsyncTask, lets name is FileTask
{% highlight groovy %}
private class FileTask extends AsyncTask<URL, Integer, Void> {

    @Override
    protected Long doInBackground(URL... url) {
       //code to download file
    }

    @Override
    protected void onProgressUpdate(Integer... progress) {
        
    }

    @Override
    protected void onPostExecute(Void result) {
        //update the UI here,
    }
}
{% endhighlight %}

* Now, on button click just execute that ansyc task that we created.
{% highlight groovy %}
new FileTask().execute("url_to_download");
{% endhighlight %}

### Methods

* ####onPreExecute
This method gets called before the doInBackground method gets called. It is where you can display a progress bar stating the download has been started.

* ####doInBackground
Heavy or long performing operations should be called here. So whenever you call the execute method(with parameters) in the UI Thread, doInBackground method gets called internally with those parameters.

* ####onProgressUpdate
This method is invoked by calling publishProgress anytime from doInBackground. Basically used to display the progress.

* ####onPostExecute
This method gets invoked when the doInBackground finishes execution. Here, you will dismiss any kind of progress, or display any success/failure message in the UI Thread.

This was a very basic information on how and when to use AsnycTask in Android Apps.

### Can we cancel the AsyncTask ?

Yes. A task can be cancelled at any point of time by invoking ````cancel(boolean)````. Invoking this method will cause subsequent calls to isCancelled() to return true. After invoking this method, onCancelled, instead of onPostExecute will be invoked after doInBackground returns. 

### Recommendation

* [Volley](http://developer.android.com/intl/ko/training/volley/index.html). It is an HTTP library that makes networking for Android apps easier and faster. I will be discussing on why to use volley in my upcoming posts.
