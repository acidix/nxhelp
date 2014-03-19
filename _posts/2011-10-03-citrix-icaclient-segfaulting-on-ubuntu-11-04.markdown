---
comments: true
date: 2011-10-03 15:27:32
layout: post
slug: citrix-icaclient-segfaulting-on-ubuntu-11-04
title: Citrix ICAClient dumping core on Ubuntu 11.04
wordpress_id: 29
tags:
- Linux
---

After switching to Ubuntu 11.04 from Fedora 15 I noticed that I wasn't able to connect to our internal Citrix environment using ICAClient. Having followed the usual documentation (listed below) I was presented with the following:



    
    *** glibc detected *** /usr/lib/ICAClient/wfica: free(): invalid next size (fast): 0x09c6e8b0 ***
          ======= Backtrace: =========
          ...



    
    The process is actually core-dumping in libproxy.so:



    
    Program terminated with signal 11, Segmentation fault.
          #0  0xf70d8f72 in nbc_connect () from /usr/lib/ICAClient/libproxy.so
          (gdb) bt
          #0  0xf70d8f72 in nbc_connect () from /usr/lib/ICAClient/libproxy.so
          #1  0xf70cd4d6 in NullConnect () from /usr/lib/ICAClient/libproxy.so
          #2  0xf70ce6cd in ProxyConnect () from /usr/lib/ICAClient/libproxy.so
          #3  0xf70ce8b1 in PROXYconnect () from /usr/lib/ICAClient/libproxy.so
          #4  0xf70d8764 in IPSTACKconnect () from /usr/lib/ICAClient/libproxy.so
          #5  0x080c3757 in ?? ()
          #6  0x080c2518 in ?? ()
          #7  0xf70c094a in ?? () from /usr/lib/ICAClient/PDCRYPT1.DLL
          #8  0x080c060a in WdSetInformation ()
          #9  0x080ffd8a in wdConnect ()
          #10 0x08100f9b in NCSbind ()
          #11 0x08077d09 in OldMain ()
          #12 0x08137f4a in PlatAppMain ()
          #13 0x080feda6 in AppMain ()
          #14 0x08137eda in main ()




However, Library dependencies on the libproxy.so looked fine:



    
    $ ldd /usr/lib/ICAClient/libproxy.so
          linux-gate.so.1 =>  (0xf7732000)
          libc.so.6 => /lib32/libc.so.6 (0xf7594000)
          /lib/ld-linux.so.2 (0xf7733000)




In the strace output I could see it was trying to parse a proxy configuration - having already unset all proxy variables (http_proxy, https_proxy, ftp_proxy) I was wondering where he was trying to get the proxy configuration from. The following line was the important one:



    
    [pid 15818] open("/home/i064850/.mozilla/firefox/cuqn88q3.default/prefs.js", O_RDONLY) = 5




It was actually trying to get my proxy configuration from my Firefox Preferences! While one could argue about this decision, it seems to cause my issue - because after moving the .mozilla folder, wfica was finally able to connect!




In the end I found out that this is an issue if you use **Automatic Proxy Configuration** in Firefox together with the Citrix ICA Client. Once you either deactivate or manually specify the proxy setting, it magically works.




I still have to find out where to submit a bug report for ICAClient - probably nowhere ...




[Edit] Bug report [posted](http://forums.citrix.com/message.jspa?messageID=1585465#1585465)




References: [The Ubuntu ICAClient HowTo](https://help.ubuntu.com/community/CitrixICAClientHowTo)
