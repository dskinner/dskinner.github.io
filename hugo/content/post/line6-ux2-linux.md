+++
date = "2010-02-24T11:06:00-04:00"
title = "Line 6 UX2 and Linux"
+++

I originally wrote a bad/sad review about the Line 6 UX2 not working under linux as can be seen here:
<a href="http://line6.com/community/thread/17663">http://line6.com/community/thread/17663</a>

I was looking to sell it recently when I decided to give it another look over and try out the drivers done for the PodXT devices by someone as can be found here:
<a href="http://www.tanzband-scream.at/line6/">http://www.tanzband-scream.at/line6/</a>

The 0.8.1 release failed to compile so i checked out the trunk of the development version

```
svn co https://line6linux.svn.sourceforge.net/svnroot/line6linux
cd line6linux/driver/trunk
make
sudo make install
```

To my surprise, it worked fairly ok. My line 6 ux2 device lit up and seemed ready to go. One of the first things I noticed was that there was a constant buzzing out of the right channel monitor. This was alleviated by simply plugging my guitar in. Next, I read through the docs to get some general info. I went ahead and fired up alsamixer, increased pcm output and zero'd out the monitor. Note: the docs say to set pcm output to zero as they can be much louder then the guitar monitor. I had no trouble with this when i increased output to 75, volume was normal.

So then I fired up jack and played around with a couple settings. To my surprise I was able to get latency down to 1.5 ms according to ardour (2.87 according to qjackctl)! Either way, this was much lower then I ever managed with my edirol which normally clocked in around 15ms and was littered with xruns that were very audible in the recording. Normally around 22ms I could achieve good sound with no xruns. After a short recording session, I noticed qjackctl had logged numerous xruns but I never heard a thing. Once during the recording, ardour disconnected from jack but this could be totally unrelated.

This is fine news indeed and prompts me to want to hold on to this device. The bad news is I have been unable to get the microphone inputs working. I haven't had time to look into it fully but hopefully after exploring the line6linux docs some more, I can have some success with getting this working.

So all in all, this is great. What I would really like to see is Line6 spend a few resources on this project or doing something themselves. But that aside, hardware wise (i guess), this little bugger rocks!
