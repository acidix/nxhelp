---
comments: true
date: 2012-05-23 09:49:50
layout: post
slug: x11-forwarding-to-a-mac
title: X11 Forwarding to a Mac
wordpress_id: 302
tags:
- Linux
- Mac
---

I just tried to use X11 Forwarding from a RedHat 6 box to my Mac failing miserably. 

You have to make sure to have the `xorg-x11-xauth.x86_64` package installed on the RedHat box in order for MacOS to start the tunnel for X11 forwarding - otherwise it won't work ... 

See the logs:

{% highlight bash %}
debug1: Requesting X11 forwarding with authentication spoofing.
...
debug1: Remote: No xauth program; cannot forward with spoofing.
{% endhighlight %}    

After that X11 forwarding works like a charm :)
