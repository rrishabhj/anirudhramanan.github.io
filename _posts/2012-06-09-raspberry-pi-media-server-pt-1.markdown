---
layout: post
title: 'Raspberry Pi: Media Server Pt. 1'
date: '2012-06-09 00:00:00'
---

<p>Now that I&#8217;ve got my Pi working a bit more efficiently it&#8217;s time to actually start putting it to use: a media server. This post will detail the initial steps in getting your Raspberry Pi serving media files from an external drive via DLNA.<br />
<img class="mceWPmore mceItemNoResize" title="More..." src="http://i1.wp.com/www.senab.co.uk/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif" alt="" data-mce-src="http://i1.wp.com/www.senab.co.uk/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif" data-recalc-dims="1" /></p>

<h4>Mount External Hard Drive</h4>

<p>The first thing we need to do is get the external hard drive mounted and read-able. When you plug in your hard drive Debian will not do anything with it, so you have two options: mount it manually each time or setup Debian to automatically mount it for you. As I didn&#8217;t really want to ssh into the Pi every time I restart it, I opted for the latter.</p>

<p>I will only ever be using 1 external hard drive, which means that I can set up Debian to auto mount this hard drive on startup by adding a fstab entry. If you&#8217;re going to be using lots of different drives, I&#8217;d recommend you having a look at install &#8216;autofs&#8217; which will automatically mount any drives you plug in. See <a href="http://wiki.debian.org/AutoFs" target="_blank" data-mce-href="http://wiki.debian.org/AutoFs">here</a>&nbsp;on how to set it up. I could have done this for my setup, but autofs runs a daemon in the background and I&#8217;d rather not waste the RAM/CPU on it.</p>

<p>The first thing you need to do is plug in your external hard drive (obviously), now we need to find out what it&#8217;s UDID is. This fairly easy to do:</p>

<pre class="brush: bash; gutter: false">sudo blkid</pre>

<p>Which will return a list with all of the drives (partitions really) connected to the Pi. You should be able to easily recognise your hard drive as it will <strong>not</strong> be any of the items starting with /dev/mmcblk. Here&#8217;s an example of my drive:</p>

<pre class="brush: bash; gutter: false">/dev/sda1: LABEL="CHRIS HD" UUID="4956-13EB" TYPE="vfat"</pre>

<p>There are two important bits of information here, the UDID and partition type. With this information we can change the fstab file and add the hard drive to it, but first we need to create a directory for the hard drive to mount to. The following commands will create a directory, and then set the permissions so that we can read it without problems:</p>

<pre class="brush: bash; gutter: false">sudo mkdir -p /media/ExternalHd
sudo chmod 755 /media/ExternalHd</pre>

<p>Now we can edit the fstab file:</p>

<pre class="brush: bash; gutter: false">sudo nano /etc/fstab</pre>

<p>Then add the following to the bottom of the file (replacing the UDID with TYPE with whatever blkid returned, and the mount location with whatever you created above):</p>

<pre class="brush: bash; gutter: false">UUID=4956-13EB  /media/ExternalHd   vfat    defaults      0     0</pre>

<p>Now if you call &#8216;sudo mount -a&#8217; the hard drive should be mounted! Your drive will also be mounted every time the Pi boots.</p>

<h3>MiniDLNA</h3>

<p>The next step is to now install a media server. I have chosen&nbsp;<a href="http://sourceforge.net/projects/minidlna/" target="_blank" data-mce-href="http://sourceforge.net/projects/minidlna/">MiniDLNA</a>&nbsp;as it&#8217;s lightweight and works well with large libraries. Luckily MiniDLNA is available for Debian Wheezy!</p>

<h4>Install and Setup MiniDLNA</h4>

<p>First thing we need to do is install it:</p>

<pre class="brush: bash; gutter: false">sudo apt-get install minidlna</pre>

<p>The next step is to set it up to your needs by editing it&#8217;s configuration file:</p>

<pre class="brush: bash; gutter: false">sudo nano /etc/minidlna.conf</pre>

<p>I&#8217;m not going to write about every option here, but I&#8217;ll list some of the modifications I have made:</p>

<pre class="brush: bash; gutter: false"># Change the Media Dir&#039;s to point to the external hard drive, obviously change these to match your directory layout
media_dir=A,/media/ExternalHd/Media/Music
media_dir=V,/media/ExternalHd/Media/Video
media_dir=P,/media/ExternalHd/Media/Photos

# Change db_dir so that the database is saved across reboots
db_dir=/home/pi/.minidlna

# Uncomment log_dir for now in case we hit problems
log_dir=/var/log

# Inotify doesn&#039;t seem to work with this kernel
inotify=no</pre>

<p>Finally you can start MiniDLNA now (and also on boot) by running:</p>

<pre class="brush: bash; gutter: false"># Start MiniDLNA at boot
sudo update-rc.d minidlna defaults

# Start MiniDLNA now
sudo service minidlna start</pre>

<p>MiniDLNA will now begin indexing all of your media files. The more files you have the longer this takes, on my machine with 12000 media files it takes about 15 minutes. Luckily, as we changed the config to make MiniDLNA save it&#8217;s database to /home this only happens once.</p>

<p>If you&#8217;ve given MiniDLNA enough time to complete it&#8217;s indexing you&#8217;ll be ready to start serving media files.&nbsp;If you find that something isn&#8217;t working, you can check the log by running:</p>

<pre class="brush: bash; gutter: false">less /var/log/minidlna.log</pre>

<p>If everything is working well I&#8217;d recommend re-commenting out the log_dir variable in the MiniDLNA config, as it does write out a lot of log information which may affect performance.</p>

<h3>Next Steps</h3>

<p>There are a number of improvements we can make to this setup for performance and power-saving which I&#8217;ll detail in a later post. For now though, enjoy your new DLNA media server!</p>
