---
comments: true
date: 2013-01-24 5:25:PM
layout: post
slug: automatically-set-hostname-from-dhcp
title: Automatically set hostname from DHCP in Debian using isc-dhcp-client
tags:
- Mac
---

One major drawback of isc-dhcp-client in Debian (in this case, Debian 6) is that the option to automatically update the hostname (like dhcpcd) is missing. I came a cross a [post](http://www.debian-administration.org/articles/447) on debian-administration.org that discusses the problem together with a quite well documented approach on how to fix it. One problem is that, with a decently configured DNS server, the 'host' command returns the hostname with a dot at the end.

I updated the script to that it just takes the actual hostname to set it, and by sending a correct domain-name option you will end up with a decent setup ('hostname' is the actual, short hostname, 'hostname --fqdn' returns the hostname with the domain). Here's how:

{% highlight bash %}
#!/bin/sh
# Filename: /etc/dhcp3/dhclient-exit-hooks.d/hostname
# Purpose: Used by dhclient-script to set the hostname of the system
# to match the DNS information for the host as provided by
# DHCP.
# Depends: dhcp3-client (should be in the base install)
# hostname (for hostname, again, should be in the base)
# bind9-host (for host)
# coreutils (for cut and echo)
#
if [ "$reason" != BOUND ] && [ "$reason" != RENEW ] \
 && [ "$reason" != REBIND ] && [ "$reason" != REBOOT ]
then
  return
fi

echo dhclient-exit-hooks.d/hostname: Dynamic IP address = $new_ip_address
hostname=$(host $new_ip_address | cut -d ' ' -f 5 | cut -d '.' -f 1)

echo $hostname > /etc/hostname
hostname $hostname

echo dhclient-exit-hooks.d/hostname: Dynamic Hostname = $hostname
# And that _should_ just about do it...
{% endhighlight %}

Thank you!
