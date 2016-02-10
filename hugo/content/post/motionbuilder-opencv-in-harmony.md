+++
date = "2008-11-14T23:43:00-05:00"
title = "Motionbuilder from AutoDesk and OpenCV in Harmony"
+++

For anyone ever curious, recently I have delved into the world of motion capture, image recognition and the likes for the past week. The task, build a headset with a mounted usb camera that tracks the eye and moves it accordingly to a model in motionbuilder. Just to get this out there, I know nothing about 3D art, modelling or anything of the likes, including motionbuilder. This would be the first time I've ever even used the software.

That aside, I asked someone for a model with a moveable eye and what i got was basically a head with the eyes fixed to a null point. when the point moves, the eyes move. So next was to get the data I had been receiving from opencv to motionbuilder. Just to clarify, currently I am simply using a default haarcascade for face detection so as to track something with motion on the screen. Im taking the center point of the face and using it as proof of concept to move the null. Books are expected Monday so i can delve into tracking the pupil.

The thing about motionbuilder though is its python implementation is hooorrible. As in the worst of the worst. If i were to say its barely usable, i might be hitting the head on the nail for some, but it still feels like a bit of an overstatement. That said, its still great that it has python support so kudos for someone atleast trying to implement it. Im sure its a daunting task for the type of project.

Let me briefly describe the limitations for any that might be unfamiliar. For one, the python version used is 2.4.1 and it comes with pyfbsdk library and nothing else. Considering the lack of documentation for the python module, i would typically resort to something like,

<pre>
import inspect
for x in inspect.getmembers(FBSystem().Scene):
    print x
</pre>

but as i said no libraries. So the first thing i did was go to the python site and download 2.4.1. Its not actually listed there but just click on whatever the latest 2.4.x is and then change the revision number to 2.4.1 in your address bar. Download, install, then copy over from your c:\python24\Lib directory all the .py's to your program\ files\\autodesk\\python\\lib folder. Now you can you do some basic stuff like inspect.getmembers. Secondly, the python console in motionbuilder is horrid. You can type in no more than one line at a time. Syntax error's have crashed the console. I cant up-arrow to previous commands. The output is limited to whatever the last command was that you ran, aaaaand the text of the output isn't selectable. So that means no copy and paste of all the methods of whatever after you inspect.getmembers... aaargh! I still have the screenshot on my desktop somewhere ...

But fear not! b/c there is telnet. From the python console from within motionbuilder, there is a tab that lets you enable telnet. So you can open up a telnet client and go to addy 127.0.0.1 port 4242 and hopefully you should be presented with a python console. I say hopefully b/c i didn't have the best of luck the first few times b/c of motionbuilder (and/or my own) quirkiness.

And finally, the real bugger in it all is that if you write a python script that takes some time to run, all of motionbuilder locks until the script is finished. So this means no script thats running in the background waiting to receive data. Instead, this data needs to be sent via the telnet link to motionbuilder. I found some great resources, but mainly ill list this particular one,

<a href="http://chrisevans3d.com/tutorials/mbui.htm">http://chrisevans3d.com/tutorials/mbui.htm</a>

He's got some great sample code for integrating a seperate wxpython script into motionbuilder and breaks down a number of things as well. Unfortunately, his code for making use of telnetlib from within a seperate python instance to issue commands to motionbuilder didn't work out so hot, which is precisely what prompted me to write about this. The code he had listed seemed a bit cryptic with these read_until's with params of 0.1 and 0.01, and i didn't see anything of the sort mentioned in python docs (barely looking of course) so I wrote my class for doing this and saved it in a mbpipe.py and it reads as follows

```
import telnetlib

class MBPipeline:
    def __init__(self, host="127.0.0.1", port="4242"):
        self.tn = telnetlib.Telnet(host, port)
        self.tn.read_until('&gt;&gt;&gt; ')

    def call(self, command):
        self.tn.write(command + '\n')
        r = self.tn.read_until('&gt;&gt;&gt; ')[:-6]
        try:
            return eval(r)
        except:
            return str(r)
```

Now from my script where i am doing opencv stuff (or simply from your console) I can do a
<pre>from mbpipe import MBPipeline
mb = MBPipeline()
mb.call('FBSystem().Scene.Components[216].PropertyList.Find("Lcl Translation")')</pre>
and what it returns is the actual tuple from motionbuilder. In any case where the string from the telnet session can be eval'd, you'll receive the object. Otherwise just the string.

Just thought I'd share. :D

I may comment later on my experiences with opencv, which so far have been great. QueryFrame, haarcascade, convert to image and push over to pyglet and render to screen my live video (note, opencv has its own windowing and controls which most will probably find useful).
