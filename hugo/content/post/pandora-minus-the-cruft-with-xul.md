+++
date = "2008-11-06T11:00:00-05:00"
title = "Pandora minus the cruft with XUL"
+++

I made this a while back then found out a couple months ago that pandora has a ?cmd=mini getvar that shows a smaller player. So I updated this xul package I made. Im no expert or even novice in xul. Very basic stuff is all I've cared to do, but i figured this was a great way to use the service minus the cruft and get it out of my browser. Google Chrome's save as application is great too if your on windows.

Pandora via XUL can be downloaded from this link: <a href="http://dasacc22.googlepages.com/pandora.tar.gz">http://dasacc22.googlepages.com/pandora.tar.gz</a>

This should work on any platform with xulrunner. You can run it from linux by

$&gt; xulrunner application.ini

in the folder, similarly on other platforms. Ive also created a sh shortcut in the folder that runs

$&gt; nohup xulrunner application.ini &gt; /dev/null 2&gt;&amp;1 &amp;

to background the service. When starting from the shortcut, click "run" to start it. I have adobe flash complain on startup saying it prevented something dangerious from happening, i guess b/c the flash object is embedded in XUL?? And that it shutdown the offending application. Just click ok and it runs just fine. No need to click settings like it prompts you to (it doesn't seem to launch settings anyway).

If the ?cmd=mini ever dissappears you can just update the xul package by opening chrome/content/main.xul and replacing the embed object with the one from the site.
