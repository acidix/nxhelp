---
comments: false
date: 2011-11-21 11:39:18
layout: post
slug: find-base-dn-for-windows-ad-user
title: Find Base-DN for Windows AD User
wordpress_id: 161
tags:
- Active Directory
- Windows
---

To find out the Bind-DN for your Windows Active Directory User just execute the following from any logged-in Windows Box:

    
    
        dsquery user dc=example,dc=com -name user1*
    


Where *example.com* is your domain and *user1* is your username. Useful if you need a correct Bind DN to connect to the AD LDAP interface or similar.

Also, a good resource can be found at the following [Symantec Website][sym]

[sym]:http://www.symantec.com/business/support/index?page=content&id;=HOWTO41996
