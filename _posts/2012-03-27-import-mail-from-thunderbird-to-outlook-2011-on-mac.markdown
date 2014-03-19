---
comments: true
date: 2012-03-27 13:22:20
layout: post
slug: import-mail-from-thunderbird-to-outlook-2011-on-mac
title: Import Mail from Thunderbird to Outlook 2011 on Mac
wordpress_id: 243
tags:
- Mac
tags:
- mac
- mbox
- outlook
- thunderbird
---

I recently got a Mac from my employer, and I'd like to try using Outlook 2011 for Mac. That, however, requires all my emails from Thunderbird to be transferred to Outlook 2011. After barely clinging onto sanity trying to migrate, I finally found the following article which brought me (at least halfway) there:

[http://www.frontendeveloper.com/thunderbird_outlook/](http://www.frontendeveloper.com/thunderbird_outlook/)

However, the App mentioned to change the creator- and type-codes is only running on Rosetta (which of course isn't available on Lion). I exchanged FileType to the (not nearly as comfortable, but at least working) app called [Quick Change](http://www.macupdate.com/app/mac/17300/quick-change). You will have to use the following codes:

* Creator: ttxt
* Type: TEXT

After that, and, renaming the files to mbx, I was able to import my mails into Outlook 2011.
