---
comments: true
date: 2012-02-22 16:39:17
layout: post
slug: finding-only-parent-pids
title: Finding only parent PIDs
wordpress_id: 234
tags:
- Categories
---

Having a process that spawns children of itself it can be hard to only find the parents ... this command is pretty weird looking, but actually works. You can achieve the same much easier if you want to use 'ps -ef | grep ...', but I wanted to stick with pgrep, so:

    
    
    pgrep boe_crprocd.bin | egrep -v "$(pgrep boe_crprocd.bin -P "$(pgrep boe_crprocd.bin -d ",")" -d "|")"
    
