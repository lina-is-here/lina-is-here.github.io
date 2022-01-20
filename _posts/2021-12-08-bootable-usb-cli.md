---
layout: post
title:  "How to prepare a bootable USB stick with Ubuntu on macOS using CLI"
date:   2021-12-08
tags: [ubuntu, linux, cli, macOS]
---

Recently I wanted to install fresh Ubuntu LTS on my other laptop. Official documentation has a [tutorial][ubuntu-official-tutorial] how to prepare a bootable USB stick on macOS, however in the second step it requires installation of another application. 

Here's how to do it without installing Etcher, using only standard tools.
1. Format USB drive with the standard application `Disk Utility`.

<!--more-->

2\. Find out the name of your disk (USB drive)

{% highlight bash %}
$ diskutil list
{% endhighlight %}

3\. You need to unmount this disk (but do not eject!). In my case the name of the disk is `disk2`, be careful here

{% highlight bash %}
$ diskutil unmountDisk /dev/disk2
{% endhighlight %}

4\. Navigate to the directory where you have your ISO image. Now write this to the USB stick. Be extra careful here and check the name of the disk that corresponds to your USB stick.

{% highlight bash %}
$ sudo dd if=ubuntu-20.04.3-desktop-amd64.iso of=/dev/disk2 bs=1m
{% endhighlight %}

If you get "Operation not permitted" the likely cause is lack of permissions for the Terminal application you use (e.g., iTerm2). To fix this:

1. Go to `System Preferences`
2. Select `Security & Privacy`
3. Go to `Privacy` tab
4. Find `Full Disk Access`
5. Click on the lock and enter your password to make changes
6. Click the `+` button to add you Terminal application here

That's it. Of course, `dd` can be used for any other image, not just Ubuntu.

[ubuntu-official-tutorial]: https://ubuntu.com/tutorials/create-a-usb-stick-on-macos
