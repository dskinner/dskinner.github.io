+++
date = "2008-08-08T21:39:00-05:00"
title = "SndObj, Jack, and Inputs"
+++

Ok so I have been tinkering with pysndobj on and off for a while. One thing I have been wanting to do is get it setup where i have a thread using SndJackIO and doing a line in from my edirol ua-25 usb soundcard with my guitar. I couldn’t find any documentation on how to do it with SndJackIO for a while, though there was plenty for SndRTIO. But then i noticed a reference to core audio on mac and doing inputs. Effectively, there is a single SndJackIO for both input and output. So when instantiating for such just do a

```
outp = sndobj.SndJackIO("MyName")
inp = outp
```

Anyway, here it is in all its glory

```
import sndobj

jack = sndobj.SndJackIO('test5')
inp = jack

snd = sndobj.SndIn(inp, 1)
cmb = sndobj.Comb(0.001, 0.001, snd)

jack.SetOutput(1, cmb)

thread = sndobj.SndThread()
thread.AddObj(snd)
thread.AddObj(cmb)
thread.AddObj(inp, sndobj.SNDIO_IN)
thread.AddObj(jack, sndobj.SNDIO_OUT)

thread.ProcOn()
```

--Edit: Above, cmb is a filter that is not needed. You can simply SetOutput to snd instead of cmb and bypass that.
