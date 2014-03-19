---
comments: true
date: 2011-11-16 18:54:48
layout: post
slug: timemachine-backup-on-a-shared-network-location
title: TimeMachine Backup on a Shared Network location
wordpress_id: 133
tags:
- Mac
tags:
- backup
- mac
- timemachine
---

Having my iMac setup for TimeMachine I wanted to do the same for the new Macbook Air. While I don't want to run around with TimeMachine Backup drive all the time (used to do that for my last MacBook, *bad* decision) I wanted to setup TimeMachine Backup to my iMac on the same external HDD.

Here's the link that helped me doing that: [Ease Time Machine networked backups][TMLINK]


[TMLINK]: http://hints.macworld.com/article.php?story=20071110145136486 (Ease Time Machine networked backups)

The workflow:

##### On the iMac

1. Connect the TimeMachine Drive
2. Under *System Preferences*, *Sharing* share the drive

##### On the Macbook Air

1. Connect to the shared drive
2. Setup TimeMachine to use the drive
3. Start a backup
4. When "*Preparing backup …*" turns into the actual backup, *Cancel*
5. Mount the newly created `sparsebundle` file by double-clicking
6. Open *DiskUtility* and select the `sparsebundle` file
7. Select the *Partition* tab
8. Shrink the size of your Time Machine destination as needed
9. Click Apply


Alright, now the Macbook Air is backing up to the Disk via the `sparseimage` file and over the network. Now the tricky part begins. Getting the iMac to backup to a `sparseimage` file locally won't work. You have to follow these steps:

##### On the Macbook Air

1. Connect the external drive to the Macbook Air
2. In *System Preferences* select *Sharing*
3. Turn on *File Sharing* and select the external drive to be shared

##### On the iMac

1. Connect to the newly created share and navigate to the Time Machine drive
2. Open Time Machine preferences and select the Time Machine Share
3. Let Time Machine create another `sparsebundle` file for the iMac
4. Once that's done cancel the backup again and mount the bundle to shrink it (see top)

##### On the Macbook Air

1. Turn off File Sharing to disconnect the drive
2. Connect the drive to the iMac again

##### On the iMac

1. Open Time Machine Preferences
2. Choose Backup Disk and select the local disk
3. Happy Time Machine'ing! The `sparsebundle` on the disk will be used even though it's attached locally!
