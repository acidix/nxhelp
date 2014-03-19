---
comments: false
date: 2011-10-21 14:26:10
layout: post
slug: my-perfect-exchange-on-linux-setup
title: (My) Perfect Exchange on Linux setup
wordpress_id: 90
tags:
- Featured
- Linux
---

Given the fact that most companies still use Microsoft Exchange as their preferred E-Mail and Collaboration solution, we (running Linux and/or MacOS) see ourselves confronted with the same problem: _How to get E-Mails and Calendar entries with the least possible pain._

I want to share my current, well working, setup with you.

**Choosing a client**

I've chosen Mozilla Thunderbird as my preferred client on the Linux desktop for E-Mail and Calendar.

**E-Mail**

I went for the usual setup: IMAP and SMTP directly to the Exchange server. The only (at least known to me) area where this damn thing is actually standards compliant.

**Calendaring**

I use the [Exchange Data Provider for Lightning](http://gitorious.org/lightning-exchange-provider/pages/Home) for calendaring. Works like a charm. After using DavMail with all the ups and downs this is the most stable solution I've used so far on Linux. Can only recommend it.

**Calendar syncing**

This goes for Google Calendar and/or iCloud. Syncing is a major factor for me as I want to have at least read-only access on my work calendar while I'm on the road.

Also, I went for an extremely straight-forward setup. Using the [Automatic Export Plugin](http://www.sunbird-kalender.de/extension/autoexport/de/index.html) for Thunderbird I just let it put a copy of my calendar exported to ICS into my Public folder in Dropbox. Using the "Public URL" feature I then subscribe to the calendar in read-only mode from Google Calendar and iCloud.

That's it! The easiest to configure and maintain setup I ever had. Man am I happy :)
