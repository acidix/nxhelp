---
comments: true
date: 2013-02-04 5:25:PM
layout: post
slug: match-vmware-disk-to-linux-device
title: Match VMware disk to linux device
tags:
- Mac
---

Today I had to find out which disks on Linux match up with the virtual disks on VMWare in order to remove some of them. This being quite a sensitive task, I spent some time figuring out how to do it consistently. Documenting it here for the afterworld:

{% highlight bash %}
$ lsscsi | grep VMware | while read scsi ig1 ig2 ig3 ig4 ig5 dev; do echo $dev - SCSI \($(echo ${scsi:1}|cut -d\: -f1)\:$(echo $scsi|cut -d\: -f3)\); done
{% endhighlight %}

Combining that with `pvs` to find emtpy physical volumes in Linux LVM gives you a sweet list of devices you can safely disconnect (after vgreduce and pvremove of course ...)

{% highlight bash %}
$ pvs | sed 1d | while read dev vg ig1 ig2 size free; do if [ "_$size" == "_$free" ]; then echo -e "$dev \t SCSI ($(lsscsi | grep $dev | cut -d":" -f1,3 | tr -d "["))"; fi; done
{% endhighlight %}

