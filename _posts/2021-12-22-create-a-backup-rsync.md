---
layout: post
title:  "How to create a backup using rsync"
date:   2021-12-22
tags: linux, cli
---

Sometimes I want to create a backup of my data on another server. 
Or, maybe, copy some data from one place to another efficiently.
The easiest thing to do in those cases is often to use a standard command `rsync`.

This is a short note on how to create a backup using only `rsync` command.


<!--more-->


### Prerequisites

* Data you want to sync
* A server you want to sync to
* Free space on that server

### The basic command I use

{% highlight bash %}
rsync -av --partial --progress --delete --exclude "Library"  ~/ username@address:/path/to/directory/
{% endhighlight %}

### Explanation

`rsync` is the tool we are using

`-av` makes it recursive (not only) and adds verbosity (more information) to the output

`--partial` keeps the partial file in the case of interruption during the transfer

`--delete` deletes extraneous files from the receiving side. This is a very important option for doing a backup.
It means that if the file is not present locally (e.g., on your laptop) but is present on the server 
(maybe from previous backup), that file will be deleted

`--exclude` excludes files from the transfer. In macOS "Library" contains things like preferences and some
cashes, so I don't want to keep it in the backup

`~/` is the path to home directory I want to make a backup of

`username@address:/path/to/directory/` is the path on your server where you want to sync your data to


### See more

For more information on `rsync`, type in your terminal

{% highlight bash %}
man rsync
{% endhighlight %}
