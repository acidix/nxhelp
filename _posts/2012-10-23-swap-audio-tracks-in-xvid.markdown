---
published: false
comments: true
date: 2012-10-23 23:38:00
layout: post
slug: swap-audio-tracks-in-xvid-file
title: Swap audio tracks in Xvid file
tags:
- Scripting
---

Well, this is more or less personal. Still interesting. Needed to batch-swap audio track of some video files (stuff I ripped from DVDs to play in iTunes, and stupidly I selected the german audio channel to play first). When I couldn't find a tool doing that, I decided to just explore how it's done with ffmpeg. And here's the outcome:

{% highlight bash %}
#!/bin/bash

filelist=$@
OLDVAL=1

# list the available tracks
ffmpeg -i $1 2>&1 | grep Stream

printf "%s [%s]: " "Enter audio track to use: " "$OLDVAL"
read NEWVAL

for file in $filelist; do
	ffmpeg -i $file -map 0:0 -map 0:${NEWVAL} -vcodec copy swap_$file
done
{% endhighlight %}

