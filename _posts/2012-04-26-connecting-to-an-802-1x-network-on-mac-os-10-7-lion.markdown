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


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.06.25-300x185.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.06.25.png)


  4. Select _Configuration Profiles_ from the pane on the lefthand side


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.07.16.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.07.16.png)


  5. Click on _New_


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.08.05.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.08.05.png)


  6. Enter some required values in the _General_ section


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.10.10-300x167.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.10.10.png)


  7. Select the _Credentials_ section


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.11.18-300x137.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.11.18.png)


  8. Click on _Configure_ and select your network certificate (I used a base64 encoded certificate)


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.14.27-300x173.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.14.27.png)


  9. Now go back to the _Wi-Fi_ section (never mind, Wi-Fi works for both wireless and cable connection)


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.15.37-300x137.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.15.37.png)


  10. Now create a new Configuration. For a cable connection, specify any name as SSID, for a wireless connection - obviously - specify the _correct_ SSID


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.19.45-300x185.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.19.45.png)


  11. Now select the protocols you need to use. In my case it was TTLS and PEAP with MSCHAPv2 authentication


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.21.06-300x203.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.21.06.png)


  12. Move on to the _Authentication_ tab and put in your username and password (if needed). In case you need to present a certificate, go back to step 8 and import your _private key_ for this certificate. You should be able to select it in the dropdown underneath the _Password_ field.


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.22.11-300x266.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.22.11.png)


  13. Last but not least navigate to the _Trust_ tab and activate the checkbox next to your network certificate you have to trust.


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.23.52-300x229.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.23.52.png)


  14. After you've done all that, you click on _Export_


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.24.56.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.24.56.png)


  15. In the _Export Configuration Profile_ dialog you select _None_ as security and proceed by clicking _Export ..._


[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.25.35-300x219.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.25.35.png)


  16. In the next dialog you just save the file somewhere on your harddisk



  17. To import the configuration profile, just double-click the file from finder.






Whenever you connect a network cable, a dialog should pop up asking you for the configuration profile to use. Just select the name you specified earlier and click _OK_. You can check the status of your 802.1X connection from the _Network Preferences_, allowing you to Connect, Disconnect and view some stats:
[![](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.34.13-300x266.png)](http://www.nxhelp.com/wp-content/uploads/2012/04/Screen-Shot-2012-04-26-at-10.34.13.png)

Hope it's helpful for anyone :)
