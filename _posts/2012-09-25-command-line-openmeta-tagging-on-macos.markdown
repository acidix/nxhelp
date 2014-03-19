---
comments: true
date: 2012-09-25 11:15:49
layout: post
slug: command-line-openmeta-tagging-on-macos
title: Command-Line openmeta tagging on MacOS
wordpress_id: 307
tags:
- Mac
---

I love tags. I want to use them more (I am a lazy ass), because when I use them I find myself finding the stuff I look for way easier. I use [Tags](http://www.caseapps.com/tags/) and [DefaultFolderX](http://www.stclairsoft.com/DefaultFolderX/) mainly for tagging, but when I am on the commandline (like most of the times) I always find it annoying to either use Tags or something else to add/remove/list tags.

That's why I wrote this little script to help me tagging my files/folders. It needs [openmeta](http://code.google.com/p/openmeta/) and should be placed somewhere in your `$PATH` for easier access (obviously):

{% highlight bash %}
#!/usr/bin/env bash

#
# Frontend script for the openmeta commandline utility
#

function usage {
	echo "Usage:"
	echo "$0 [file|directory]"
	echo
	echo "Specify one tag per line to add a tag, prepend a '-' to remove a tag"
	exit 0
}

timestamp=$(date +%s)
dir=$1
qdir=$PWD/$1

openmeta -t -p $qdir | rev | cut -d" " -f 2- | rev >/var/tmp/tag_$timestamp 2>/dev/null
if [ $? -ne 133 ]; then
	curtags=$(cat /var/tmp/tag_$timestamp)
	echo "Current tags: $curtags"
	echo
fi

# Exit if we have nothing to tag
[ ! -d $qdir -a ! -f $qdir ] && usage

while read -p "Tag: " line
do
    [ -z "${line:-}" ] && break
    if [ "${line:0:1}" == "-" ]; then
	    rmtags="$rmtags ${line:1}"
    else
	    newtags="$newtags $line"
    fi
done

echo
if [ "_$rmtags" == "_" ]; then
	openmeta -a ${newtags} -p $qdir | rev | cut -d" " -f 2- | rev
else
	for rmtag in $rmtags; do
		curtags=$(echo $curtags | sed "s/$rmtag//g")
	done
	openmeta -s ${curtags} ${newtags} -p $qdir | rev | cut -d" " -f 2- | rev
fi

rm -f /var/tmp/tag_$timestamp &>/dev/null
{% endhighlight %}
