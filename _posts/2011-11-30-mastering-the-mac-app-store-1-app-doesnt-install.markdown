---
comments: true
date: 2011-11-30 09:09:27
layout: post
slug: mastering-the-mac-app-store-1-app-doesnt-install
title: 'Mastering the Mac App Store #1: App doesn''t install'
wordpress_id: 217
tags:
- Mac
tags:
- appstore
- mac
- vico
---

I had another coming together with the *nice* Mac Appstore. I really have mixed feelings about this thing. While I do like the idea of having a central place to manage my apps, it suffers from major problems …

* It's too slow.
* It's incomplete.
* It causes problems

Especially the third aspect bit me yesterday. I had the Trial version of [Vico][VicoApp] installed on my Mac (the editor I was looking for, by the way …) and wanted to upgrade to the paid version. That said, the most logical  approach for me was to just go to the Mac Appstore and buy the damn thing!

Having done that, I got suspicious right after I bought it. The Appstore told me that [Vico][VicoApp] was already installed and if I wanted to redownload it. I responded "Yes, redownload". That did nothing - opening [Vico][VicoApp] still showed me my Trial period was over.

The next try was to remove *Vico.app* from my *Applications* directory, clearing the Trash and try again. This is where things got really annoying … I was able to select "Install" from the Mac Appstore, it showed Launchpad with the nice [Vico][VicoApp] Icon, but after the *Installing* step was finished, the icon just disappeared ...

Looking at the logs revealed at least the outcome of the problem ...

    
    
        installd: PackageKit: ----- Begin install -----
        installd: PackageKit: Not installing MAS receipt for skipped 
                bundle at Applications/Vico.app
        installd: Installed "Vico" ()
        installd: PackageKit: ----- End install -----
    


OK, so it *skipped* the bundle. But why? Alright, diving deeper I had a look at the 'installd' process while running the installation:

    
    
        # First find the PID
        ps -ef | grep installd
        #
        # Now trace it
        sudo dtruss -p <pid>
        # 
        # Now run the installation again
    


Right, a lot of tracing information. However, 2 files that I was suspicious about:

    
    
        open("/var/db/receipts/se.bzero.Vico.plist\0", 0x601, 0x1
             = 5 0
        open("/var/db/receipts/se.bzero.Vico.bom\0", 0x601, 0x1B6)
             = 5 0
    


Alright, let's clear them (they were also present in /tmp):

    
    
        sudo rm /var/db/receipts/se.bzero.Vico.plist 
        sudo rm /var/db/receipts/se.bzero.Vico.bom
    	sudo rm /tmp/se.bzero.Vico.plist 
        sudo rm /tmp/se.bzero.Vico.bom
    


But not enough, the install still showed the same problem. And even more annoyingly, every installation attempt created the files again ... what I had to do was to restart 'installd' and the Mac Appstore Management Process 'storeagent':

    
    
        sudo killall installd storeagent
    


Now, relaunching the Mac Appstore I could finally install [Vico][VicoApp] without any further issues …



Given that this was really annoying - it's also what I love about MacOS. You may have a partly malfunctioning Mac Appstore, an App Developer that may have not payed attention to what files to leave where - but in the end - you get it working. Thanks Apple! And hey - please make the Appstore more of a Mac Experience ... soon.



[VicoApp]: http://www.vicoapp.com/ "Vico App"
