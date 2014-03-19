---
comments: false
date: 2011-11-22 10:15:50
layout: post
slug: thunderbird-throwing-sec_error_unknown_issuer
title: Thunderbird throwing 'sec_error_unknown_issuer'
wordpress_id: 179
tags:
- Linux
---

This morning Thunderbird greeted me with the following Error:


     > [server address:443] uses an invalid security certificate. The certificate is not trusted because the issuer certificate is unknown.
     > (Error code: sec_error_unknown_issuer)

As you see from the port it's not the IMAP nor the LDAP interface of our internal Exchange Server that's being accessed, it's the [Exchange Data Provider for Lightning][xchg] I [use][xchglnx].

To trust the certificate (given you know that you want to trust that server) click "Display Certificate" and navigate to the *Advanced* tab to *Export Certificate*. You then go to your Thunderbird Settings, *Advanced*, *Security*. In the *Servers* Tab you import the newly exported certificate. Click on it and select *Edit Trust* to select *Trust this server certificate*.

Should be sorted! It would really be too easy to place a "Trust certificate" button next to "Cancel" button ...


[xchg]:http://gitorious.org/lightning-exchange-provider/pages/Home
[xchglnx]:http://www.nxhelp.com/2011/10/my-perfect-exchange-on-linux-setup/
