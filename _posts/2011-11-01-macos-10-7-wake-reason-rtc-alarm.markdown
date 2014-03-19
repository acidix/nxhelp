---
comments: false
date: 2011-11-01 09:08:54
layout: post
slug: macos-10-7-wake-reason-rtc-alarm
title: 'MacOS 10.7: Wake reason RTC (Alarm)'
wordpress_id: 108
tags:
- Mac
---

Ever since I upgraded to MacOS 10.7 (or OS 10.7 as you'd call it nowadays) I noticed my machine periodically waking up from sleep without turning on the Monitor - and going back where it came from. While it's not a big deal, it is kind of annoying, so I wanted to get rid of it. A quick search for "wake reason" in Console revealed at least parts of the issue:



    
    01/11/2011 06:14:13.000 kernel: Wake reason: RTC (Alarm)




After a quick search I found the Knowledge Base Article that describes exactly my problem: [About Wake on Demand](http://support.apple.com/kb/HT3774?viewlocale=en_GB)




The reason for this odd behaviour is the Bonjour Sleep Proxy that is used to periodically advertise services of the machine to the network. While I had this turned off in 10.6, is was set to on automatically when I upgraded to 10.7. To shorten the article, the solution is to disable "Wake on Network Access" in the Energy Saving options.
