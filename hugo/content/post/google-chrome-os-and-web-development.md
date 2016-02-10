+++
date = "2008-09-13T11:54:00-05:00"
title = "Google Chrome, OS', and Web Development"
+++

Ok so everyone, their mama, and her pet hamster have written an article about google chrome. Ive also read an interesting article that some-what theorizes on a google os based on chrome in the distant future. Only interesting b/c of what I have been planning to do.

First off, I read about people complaining over initial memory consumption of Google Chrome. I dont know why but I feel a need to state my opinion on the matter. What I care about most is a responsive system and chrome does that, short and simple. If i were to liken chrome to something id say starting an instance of google chrome is like typing

```
$> ls
```

at a command prompt. Its freaggin fast. Period. Running chrome nay interferes with most anything else i do. That is using it on a core 2 duo laptop with 2 gigs of ram and a amd64 desktop with 1 gig of ram. Just a quick statement of facts. Anyway, im not really interested in the distant future, what i AM interested in is the possibility of now. Honestly with very few modifications i would use chrome as a full shell replacement, on both linux and windows. Here's my wish list,

    * A better file manager
    * the ability to launch local programs from the address bar

Ok so thats all i have for the most part. Frequently used programs, I could simply bookmark. Launching them from the addy bar would be very similar to what Ive always enjoyed doing on linux using the likes of katapult in the past and gnome-do nowadays (which does alot more interesting stuff). Ubiquity is a mozilla project that has a similarly driven concept of a type-into pop console to accomplish tasks (a little more complex than just launching a program). To accomplish this my local computer would need to be indexed or the standard entries for programs installed would need to be, but id prefer the first so i can search my computer for a file

```
local:[search-term]
```

anyone? I think alot of this couldn't really be accomplished without patching some code (mostly the use of the addy bar).

As for the file manager, the builtin file manager is of the likes of firefox. just a point and click and launch scheme. An actual robust file system could be written as a local webapp. Chrome is like a staging ground for writing a new breed of applications as I see it, I actually just wrote something today to display an index of my movie collection and made a shortcut with chrome, works beautifully. Yes it runs in firefox and anything else for that matter, but would i ever use it in such? likely not b/c when i want to watch a movie, i want to click an icon and i WANT it NOW. I dont have all day to wait for something to start up. Well I do, but its a real buzz-killer.

Ok enough ranting. I am looking at chrome as a means to develop desktop centric applications (one of which is a music app based on the likes of the sndobj library that would allow multiple people to mix at the same time) and I think the best first place to start is to write a file manager and be able to bookmark applications i use and also index my local drive so i can search it. the urls wont be the prettiest with accessing 127. but it will do for now. Then on X startup or on windows startup, chrome is launched instead of gnome or kde or fluxbox or my beloved openbox which has always come to save my day in one way or another, or explorer.exe

Those are my ideas, I will be starting some of them soon and developing in python/cherrypy (unless theres due cause for something else) and then looking into investigating how to create a windows service. If anyones interested in collaborating, feel free to contact me.
