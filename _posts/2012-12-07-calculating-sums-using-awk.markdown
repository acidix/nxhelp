---
comments: true
date: 2012-12-07 5:25:PM
layout: post
slug: calculating-sums-using-awk
title: Calculating sums using awk
tags:
- Mac
---

Before I google that another time (I am pretty sure we're standing a chance that google will explode in that case), here's how you calculate sums using awk:

{% highlight bash %}
cat something | awk '{ SUM += $5} END { print SUM }'
{% endhighlight %}

Thank you!