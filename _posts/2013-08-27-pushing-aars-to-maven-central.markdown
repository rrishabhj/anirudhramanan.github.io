---
layout: post
title: Pushing AARs to Maven Central
date: '2013-08-27 11:05:00'
---

<p>Over the past few weeks I've been updating <a href="http://github.com/chrisbanes/ActionBar-PullToRefresh">ActionBar-PullToRefresh</a> for the release of v0.7, but have been a bit blocked on publishing the library as an Android Archive (aar) to <a href="http://search.maven.org/">Maven Central</a>. It was the number one issue/request that I received, and while I had a working local Gradle build I could not find an easy way to publish the results.<br />
<!-- more --></p>

<p>There a few solutions out there but nothing really definitive. The main solutions I found were:</p>

<ol>
<li><a href="http://www.gradle.org/docs/current/userguide/artifact_management.html">upload task</a>. The original Gradle-provided method for uploading archives, provided as part of the 'maven' plugin. This seems to have been superseded by number 2.</li>
<li><a href="http://www.gradle.org/docs/current/userguide/publishing_maven.html">maven-publish</a>. The new way to upload artifacts, currently being incubated. While this plugin looked great it seems to only support <code>java</code> and <code>web</code> projects. It also requires you to modify the <code>pom.xml</code> as XML rather than a object which means no value checking.</li>
<li><a href="http://github.com/bmuschko/gradle-nexus-plugin">gradle-nexus-plugin</a> is a third-party open source plugin which is designed to work on top of solution number 1, but does a lot of work for you. Unfortunately it only seems to work with <code>java</code> projects.</li>
<li><a href="http://github.com/koush/mvn-repo">mvn-repo</a> a specialized script provided by <a href="http://plus.google.com/u/0/110558071969009568835/">+Koushik Dutta</a> which again works on top of solution 1 but specialised for Android. Unfortunately it doesn't have <code>snapshot</code> support and requires a GitHub API key which I didn't want to setup.</li>
</ol>

<p>So as you can see, it's not exactly easy.</p>

<h2>Noob and proud</h2>

<p>Before I go any further I must say that I am a Groovy and Gradle n00b so my solution below may not be optimized.</p>

<h2>My solution</h2>

<p>As we had a long weekend in the UK this week, I decided to spend a bit of time creating a easy to use solution for myself.</p>

<p>The first thing I decided was that I was going to re-use <code>mvn-repo</code> and customize it to my needs. The great thing about <code>mvn-repo</code> is that it is basically a Gradle script, which gets called from your project's <code>build.gradle</code> to say: 'hey, upload me!'. This means you can be selective on what projects get uploaded and you can re-use as much boilerplate as possible.</p>

<p>The downsides to <code>mvn-repo</code> were that it drags in a lot of information from GitHub to include in the <code>pom.xml</code>. While this is great for some projects, I wanted much more control of goes into the <code>pom.xml</code>. After reading some of the Gradle <a href="http://www.gradle.org/docs/current/userguide/userguide.html">User Guide</a> I stumbled upon the use of properties files which allow you to seperate out any configuration values from your scripts. A eureka moment quickly followed when I realised I could combine a re-usable 'upload' script with project specific property files, hence keeping my <code>build.gradle</code> scripts as clean as possible.</p>

<h3>The result...</h3>

<p>The end result is <a href="http://github.com/chrisbanes/ActionBar-PullToRefresh/blob/master/maven_push.gradle"><code>maven_push.gradle</code></a> which is a modified version of <code>mvn-repo</code>:</p>

<ul>
<li>Automatically selects which repository to upload to (snapshot/release).</li>
<li>Imports <code>pom.xml</code> values from the project's <code>gradle.properties</code> (<a href="https://github.com/chrisbanes/ActionBar-PullToRefresh/blob/master/library/gradle.properties">example</a>).</li>
</ul>

<p>It is also exteremely easy to use, it just requires the following line to be added at the end of your <code>build.gradle</code> (<a href="https://github.com/chrisbanes/ActionBar-PullToRefresh/blob/master/library/build.gradle">example</a>):</p>

<pre><code>apply from: '../maven_push.gradle'
</code></pre>

<h3>Extras</h3>

<p>I have used the properties files for a number of other things in the project:</p>

<ul>
<li>The version number and name is now controlled in one place: the root <a href="https://github.com/chrisbanes/ActionBar-PullToRefresh/blob/master/gradle.properties"><code>gradle.properties</code></a>. These version values are applied throughout the project, including the built sample APKs.</li>
<li>Each application project has a private <code>signing.properties</code> file which controls which keystore to use to sign the APK (<a href="https://gist.github.com/chrisbanes/6353187">example</a>).</li>
</ul>
