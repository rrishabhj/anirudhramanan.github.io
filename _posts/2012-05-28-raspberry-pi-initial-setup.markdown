---
layout: post
title: 'Raspberry Pi: Initial Setup'
date: '2012-05-28 00:00:00'
---

After receiving my Raspberry Pi this weekend, I&#8217;ve finally had some time to have a play with it. My end goal for the little device is to convert it into a NAS serving media files across my home. For now though I&#8217;m just trying to optimise the Debian image Raspberry Pi distribute.  
<!--more-->

### Enable SSH

First off, I enabled SSH Server so I could connect to it wirelessly.

<pre class="brush: bash; gutter: false">sudo /boot/boot_enable_ssh.rc</pre>

### Optimise fstab

There is a swap partition by default in the image, but it&#8217;s not used. This is good as SD Cards as other flash media have limited write counts on block, so having a swap writing to your SD Card would just destroy it sooner or later. For now, lets optimise the root fs mount options:

<pre class="brush: bash; gutter: false">sudo nano /etc/fstab</pre>

Change it to the following:

<pre class="brush: bash; gutter: true">proc            /proc         proc    defaults                        0    0
/dev/mmcblk0p1  /boot         vfat    defaults                        0    0
/dev/mmcblk0p2  /             ext4    defaults,noatime,nodiratime     0    0</pre>

The options we&#8217;ve added to the root fs mount (noatime,nodiratime) basically reduce the amount of filesystem accesses for last accessed times.

### Remove Unnecessary Packages

Most of this section is taken from Gordon&#8217;s post [here][1].

<pre class="brush: bash; gutter: false"># NFS. I don&#039;t need this as I&#039;ll use sftp/rsync
sudo apt-get purge portmap

# NTFS and FUSE. My external HD is FAT32 so no need for this.
sudo apt-get purge fuse-utils
sudo apt-get purge libfuse2
sudo apt-get purge libntfs10

# GDM. Not really sure why this is installed as the recommend
# way to start X is to call &#039;startx&#039;.
sudo apt-get autoremove gdm

# Finally call autoremove to remove all the unneeded dependencies
sudo apt-get autoremove</pre>

### Decrease Video Memory

By default the Debian distro is set to use 64MB of the 256MB of RAM for the GPU. As I don&#8217;t really need the video output I&#8217;ve reduced this to 32MB by using one of the pre-built elfs (the number in the file name is the amount of system RAM available):

<pre class="brush: bash; gutter: false">sudo cp /boot/arm224_start.elf /boot/start.elf</pre>

You can see what RAM/GPU variations are available by doing this command:

<pre class="brush: bash; gutter: false">ls /boot/arm*.elf</pre>

That will do for now, my next post will be about getting the Pi ready for serving Media files!

 [1]: https://projects.drogon.net/raspberry-pi/initial-setup2/