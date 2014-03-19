---
comments: true
date: 2012-03-20 10:19:32
layout: post
slug: prevent-file-deletion-on-linux
title: Prevent file deletion on Linux
wordpress_id: 240
tags:
- Linux
---

Note to self, before I google it another 25 times ... Here's how to prevent a file to be deleted using extended attributes on Linux:

{% highlight bash %}
    chattr +i <file>
{% endhighlight %}

to check if it worked:

{% highlight bash %}
    lsattr <file>
{% endhighlight %}
    
example:
    
{% highlight bash %}
    $> sudo chattr +i testfile
    $> lsattr testfile
    ----i------------e- testfile
{% endhighlight %}


