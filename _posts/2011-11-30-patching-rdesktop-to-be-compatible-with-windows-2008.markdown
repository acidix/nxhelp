---
comments: true
date: 2011-11-30 11:23:42
layout: post
slug: patching-rdesktop-to-be-compatible-with-windows-2008
title: Patching rdesktop to be compatible with Windows 2008
tags:
- Coding
- Linux
---
Lots of people may know the famous rdesktop bug that keeps you from transferring files from mapped drives on your local machine. The error shown is:

{% highlight bash %}
Path is not accessible. You might not have permission to use this 
network resource. Contact the administrator of this server to find 
out if your have access permissions.

Attempt to access an invalid address.
{% endhighlight %}

While there is a patch for rdesktop since 2009, it doesn't seem to make it into the major distributions. At least not into Ubuntu (11.04 as I'm speaking).

So, I'm patching it myself ... Here are the steps:

#### Install all build dependencies ####
{% highlight bash %}
apt-get build-dep rdesktop
{% endhighlight %}
#### Get the source package ####
{% highlight bash %}
mkdir /var/tmp/rdesktop-build
cd /var/tmp/rdesktop-build
apt-get source rdesktop
{% endhighlight %}
#### Patch it ####
{% highlight bash %}
sudo patch -p0 </home/user/Downloads/fix-2022945.patch
{% endhighlight %}
#### Build the package ####
{% highlight bash %}
cd rdesktop-1.6.0/
sudo dpkg-buildpackage -rfakeroot -b
{% endhighlight %}
#### Install the package ####
{% highlight bash %}
cd ..
sudo dpkg -i rdesktop_1.6.0-3ubuntu4.1_amd64.deb
{% endhighlight %}

Done. Now rdesktop is working with Windows Server 2008.
