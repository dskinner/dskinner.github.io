+++
date = "2009-12-05T08:18:00-04:00"
title = "Javascript's not all bad"
+++

I gotta admit, through all the frustration and experimentation ive gone through with the language recently, its not so bad. At first I thought, "expressive? no way" b/c it seems like when you go to be expressive, you crop up with irreparable errors that eventually force you into one "expression". Frustration follows each step, as is learning for me, yet I carry on.

Now Im feeling the chains loosened. Not so much do I feel tied to a particular paradigm when writing out bits of javascript, particularly, paradigms that Ive brought over from projects Ive worked on in python. Im looking at new ways to do things in javascript, in particular I was pretty happy to see getters and setters in javascript 1.5, Ive always enjoyed them for a couple reasons.

<strong>Getters and Setters provide a consistent API</strong>
Alot of times, we find ourselves writing utilities and libraries of utilities to perform particular actions. When using these libraries (or someone else's for that matter), its important to have a consistent API so one can think about the task at hand, not the details of the API. If you have a series of attributes on an object, but some of them might be dependent on others, getters and setters might prove useful for consistency in accessing object properties. If you have the following properties, parent, children, x, y, w, h, batch, visible, well keeping in mind to call get_parent() or batch() or whatever while calling .w and .visible for others really blows. All I can say is that I hope theres auto generated documentation to keep at hand, which will be a pain if theres long time periods in between using the library.

<strong>Getters and Setters provide a mechanism for error checking</strong>
Another use I've found for getters and setters is error checking. It provides a centralized point to assert the value is something valid before getting or setting. So say I have

widget.x = input

Theres alot of places I could check input, but if Im getting input and setting x from different scenarios that crop up, being able to check it with a setter on x would consolidate alot of code.

Anyway, having access to getters and setters in javascript will be useful to say the least. Embracing the functional style of javascript looks like a win-win situation. Unlike others, I have no gripes with calling object.prototype.method.call(this) but even that is probably unnecessary in alot of situations, trying to fit a bull with a shoehorn.
