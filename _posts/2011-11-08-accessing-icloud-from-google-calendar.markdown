---
comments: true
date: 2011-11-08 21:46:00
layout: post
slug: accessing-icloud-from-google-calendar
title: Accessing iCloud from Google Calendar
tags:
- Featured
- iCloud
- Mac
tags:
- calendar
- icloud
- ics
---

Accessing my iCloud Calendar from within Google Calendar was a major issue since my move to iCloud. Having said that, it's not even my own "Backup" setup on Google Apps that bothers me that    much, it's more about sharing my calendar with other family members that are not using iCloud.

### Getting Access - the easy part ###
Getting access itself is, despite one small caveat, the easy task. You'll have to publicly share your calendar. WIthout public sharing only iCloud users will be able to access your calendar. Despite the fact that you'll share your calendar with the world - that's a minor pitfall to take.

### Getting Access - the problem ... ###
However, if you now try to access your calendar from Google (replacing `webcal://` by `http://`), you'll be presented with the following error:

<img style="display: block; margin-left: auto; margin-right: auto;" title="Screen Shot 2011-11-08 at 21.06.19.png" src="http://www.nxhelp.com/wp-content/uploads/2011/11/Screen-Shot-2011-11-08-at-21.06.19.png" border="0" alt="Screen    Shot 2011 11 08 at 21 06 19" width="459" height="111" />

The iCloud servers seem to prevent access from Google - probably mainly to avoid the Google servers crawling all public calendars, but with the drawback of keeping us from accessing our calendar. After a [post in the iCloud]{https://discussions.apple.com/message/16617386#16617386} forums I don't have much confidence this will be fixed soon … 

### Solution 1 - the PHP script ###
So, I thought there must be an easier way than relying on Apple to fix the problem. Actually, there were 2 solutions that came to my mind:

- Apache Reverse Proxy
- A PHP script

Saying that I know that the majority of people doesn't have an own (V-)Server to run an Apache instance they can actually configure. However, there are plenty of (free) providers that allow you to run PHP scripts.
The following script gets the calendar (via the public URL) from iCloud and presents it in a way that Google Calendar will be able to access it:

{% highlight php %}
<?php
// Set error level reporting level
error_reporting(E_ERROR);

// create a new curl resource and set options
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL, "https://<your iCloud feed>");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_BINARYTRANSFER, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);

// grab URL, and return output
$output = curl_exec($ch);

// Send the header based on the response from the server
header('Content-type: text/calendar');
header("Cache-Control: no-cache, must-revalidate"); 

// HTTP/1.1
header("Expires: Sat, 26 Jul 1997 05:00:00 GMT"); 
// Date in the past

// Send the curl output
if (empty($output))    
    echo var_dump($output);
    else
        echo $output;
    
    // close curl resource
    curl_close($ch);
    
?>
{% endhighlight %}

### Solution 2 - mod_proxy ###
The even easier solution (as long as you've got your own configurable Apache instance) is using mod_proxy to use a simple HTTP Reverse Proxy to get the feed from the Apple Servers and redirect it. One pitfall though is that the iCloud access strictly requires https access, so we'll have to configure apache for ssl as well. This example is based on a RedHat 5.4 system, so adapt accordingly ...
     
First, install apache2 and mod_ssl
{% highlight bash %}
yum install httpd mod_ssl
{% endhighlight %}

Next, we'll configure the SSL Engine:
{% highlight bash %}
vi /etc/httpd/conf.d/ssl.conf

...
<VirtualHost _default_:1443>
SSLProxyEngine on
SSLOptions -StrictRequire
...
{% endhighlight %}

Having configured https and enabling the SSL Proxy option, we can now go ahead an get the actual ReverseProxy working:

{% highlight bash %}
vi /etc/httpd/conf/httpd.conf
{% endhighlight %}

Add the following lines to the end of the file:
{% highlight bash %}
ProxyRequests   off
SSLProxyEngine  on
    
<Proxy *>
   Order deny,allow
   Allow from all
</Proxy>
    
# Set TCP/IP network buffer size for better throughput (bytes)
ProxyReceiveBufferSize 4096
    
ProxyPass /ical/ https://p03-calendarws.icloud.com/
ProxyPassReverse /ical/ https://p03-calendarws.icloud.com/
{% endhighlight %}

Now, start the apache process:

{% highlight bash %}
/etc/init.d/httpd start
{% endhighlight %}

You can now access your calendar by replacing the `webcal://p03-calendarws.icloud.com/` part of your public URL by `http(s)://yourserver/ical/`, making is something like `http://yourserver/ical/ca/subscribe/1/pY7JHjsAi` ...
