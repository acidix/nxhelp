---
published: false
comments: true
date: 2012-10-23 23:32:00
layout: post
slug: opening-iterm-at-current-finder-location
title: Opening iTerm at current Finder location
tags:
- Mac
---

I often download files, may it be from the web or from my e-mail program of choice (at work: sadly Outlook, at home Mail) and need to do stuff with it in the console.

To speed up this process, I've written this small AppleScript. The ideas far outpace my abilities with AppleScript, so for now it only works in Finder (although it has stuff in it to prepare it to work universally, it currently doesn't). I thought I'd share it anyway, so here it comes:

{% highlight applescript %}
-- get active window
tell application "System Events"
	-- get the name of the frontmost application
	set theApp to (name of the first process whose frontmost is true) as text
end tell

-- debug
--tell application "Finder" to display dialog theApp


tell application theApp
	-- check if we have documents or, basically, Finder
	set documentCount to count documents
	-- if we have documents, select the currently open one
	if documentCount > 0 then
		-- fetch the document name
		set theDocument to document 1
		set theFile to path of theDocument
		-- chop the file name to cd to the directory
		set tid to AppleScript's text item delimiters
		set AppleScript's text item delimiters to "/"
		set chunks to text items of theFile
		if last item of chunks is "" then set chunks to reverse of (rest of (reverse of chunks))
		set theFilePath to quoted form of (reverse of (rest of (reverse of chunks)) as text)
		set AppleScript's text item delimiters to tid
	else
		using terms from application "Finder"
			-- no documents open. Here comes Finder ...
			set theFilePath to POSIX path of (target of window 1 as alias)
		end using terms from
	end if
end tell


-- Now open in iTerm2
tell application "iTerm"
	activate
	set theTerm to (make new terminal)
	tell theTerm
		-- set size 	
		set number of columns to 132
		set number of rows to 50
		-- launch a default shell in a new tab in the same terminal 
		launch session "Default Session"
		tell the last session
			-- cd to the directory
			write text "cd " & theFilePath
		end tell
	end tell
end tell
{% endhighlight %}
