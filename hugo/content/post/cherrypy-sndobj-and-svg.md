+++
date = "2008-08-10T22:55:00-05:00"
title = "CherryPy, SndObj, and SVG"
+++

Ok so sometime back when i first started tinkering with pysndobj, i was toying with some ideas for a user interface. Primarily i come from a web background and i decided to toy with the idea of a web frontend to a sndobj thread(s) that would write to an output stream that multiple people could connect to to work collaboratively together semi-realtime. Yeah, it sounds a bit amibitious but anyway I decided to take the time to toy around with SVG as well, which ive never worked with, to see what I might come up with.

Ultimately, I got this, [cpsndobj](http://dasacc22.googlepages.com/cpsndobj.tar.gz).

This is the source code to my simple cherrypy/sndobj/svg demo. Basically I have an svg knob that i made that you can manipulate by clicking and dragging the mouse down on it. Unfortunately with a browser you cannot lock the mouse down in place so it can seem a bit odd if you reach the edge of your screen. But anyways, tar -xzf this bad boy and ./python cpsndobj.py and you will find a local webserver at http://127.0.0.1:8080/. Of course you need to sudo easy_install-2.5 cherrypy if you wanna use it. The rest is javascript(jquery if i remember correctly(been a while since i looked(i think this is why i like python, avoiding all these tags(woot!)))). And I believe I have it set to connect to a jack server so you'll want to change it to SndRTIO if you want otherwise or start your jack first. After you get it up and running, visit the local address /on to turn on the modulating frequency the visit the root of the site to display the svg knob. Now click and drag it. and youll see it do its thing. Things to note, if your using internet explorer, you'll need to install the adobe svg viewer (though ive not tested it in IE). Here in firefox land, we apparently like things to run slow so if youd like to experience a smooth svg knob .. experience, then get opera installed and notice the difference.

Uh but yet, i dont use opera past playing with an svg knob and playing flash movies that dont lock up my browser in linux.

One might say, ultimately i basically have a tinker toy thats ok (woot!).

--Edit: I suddenly realized the fallacy of my "i like python" statement above, python still has paranthesis.. duh..
