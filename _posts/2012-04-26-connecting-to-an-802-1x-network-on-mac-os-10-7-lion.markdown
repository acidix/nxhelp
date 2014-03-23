---
comments: false
date: 2012-04-26 09:44:57
layout: post
slug: connecting-to-an-802-1x-network-on-mac-os-10-7-lion
title: Connecting to an 802.1X Network on Mac OS 10.7 Lion
wordpress_id: 274
tags:
- Lion
- Mac
---

I recently faced the challenge (yes, it's a _challenge_) to connect to an 802.1X secured network from Mac OS 10.7. While normally the people responsible should provide you with a configuration profile for your Mac, that's actually not very often the case ...

After trying to connect, installing certificates into my Keychain, I ended up with the following thread on the Apple Support Community:

[https://discussions.apple.com/message/16164097#16164097](https://discussions.apple.com/message/16164097#16164097)

The answer of DrVenture pretty much explains the procedure, but I'd like to outline it here as well:





  1. Download iPCU (iPhone Configuration Utility) from [here](http://support.apple.com/kb/DL1465)
  2. Open iPCU from Applications / Utilities or via Finder
  3. You screen will look like this:
  4. Select _Configuration Profiles_ from the pane on the lefthand side
  5. Click on _New_
  6. Enter some required values in the _General_ section
  7. Select the _Credentials_ section
  8. Click on _Configure_ and select your network certificate (I used a base64 encoded certificate)
  9. Now go back to the _Wi-Fi_ section (never mind, Wi-Fi works for both wireless and cable connection)
  10. Now create a new Configuration. For a cable connection, specify any name as SSID, for a wireless connection - obviously - specify the _correct_ SSID
  11. Now select the protocols you need to use. In my case it was TTLS and PEAP with MSCHAPv2 authentication
  12. Move on to the _Authentication_ tab and put in your username and password (if needed). In case you need to present a certificate, go back to step 8 and import your _private key_ for this certificate. You should be able to select it in the dropdown underneath the _Password_ field.
  13. Last but not least navigate to the _Trust_ tab and activate the checkbox next to your network certificate you have to trust.
  14. After you've done all that, you click on _Export_
  15. In the _Export Configuration Profile_ dialog you select _None_ as security and proceed by clicking _Export ..._
  16. In the next dialog you just save the file somewhere on your harddisk
  17. To import the configuration profile, just double-click the file from finder.

Whenever you connect a network cable, a dialog should pop up asking you for the configuration profile to use. Just select the name you specified earlier and click _OK_. You can check the status of your 802.1X connection from the _Network Preferences_, allowing you to Connect, Disconnect and view some stats.

Hope it's helpful for anyone :)
