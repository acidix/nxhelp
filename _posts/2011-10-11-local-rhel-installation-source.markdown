---
comments: false
date: 2011-10-11 10:02:00
layout: post
slug: local-rhel-installation-source
title: Local RHEL Installation Source
wordpress_id: 64
tags:
- Linux
---

For RedHat System which are not connected to the internet, it's good to have a local dvd as an installation source. Here's how to do it:

First, mount the Installation DVD:

    
    mkdir /media/cdrom && mount /dev/cdrom /media/cdrom


Now, let's quickly create an entry in `/etc/yum.repos.d/` that points to your dvd

    
    cat >/etc/yum.repos.d/rhel_dvd.repo <<EOF
    [dvd]
    mediaid=$(head -n1 /media/cdrom/.discinfo)
    name=RedHat Enterprise Linux $(sed -n '2p' /media/cdrom/.discinfo | rev | cut -c -3 | rev) DVD
    baseurl=file:///media/cdrom/Server
    EOF


That's it! Now you can use the following command to install packages without contacting RedHat Network:

    
    yum install strace --noplugins


For further information, visit the [Official RedHat Article](https://access.redhat.com/kb/docs/DOC-9744).
