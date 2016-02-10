+++
date = "2009-12-05T14:17:00-04:00"
title = "A New Way to Prototype with Javascript"
+++

Ive been doing alot of reading. One place that I keep happening upon is Crockford's javascript pages. I dont reallly know who that is, but I've occasionally read he's some sort of javascript legend. Well, I was looking over one of his pages, describing instantiating new objects, specifically:
<a href="http://javascript.crockford.com/prototypal.html">http://javascript.crockford.com/prototypal.html</a>

He talked about prototypal behavior, how it should work, and his simple function for doing so. Effectively, creating a blank function definition and then setting its prototype to the passed in object. I suddenly realized something. Ive been going about it all wrong with this class() business. Furthermore, what I had in place was his function but on steroids. Instead of initializing a blank function, I have what i was considering a sort of meta object that links to magic methods. And instead of setting the prototype to the object passed in, I set it to the all the objects passed in. So I took this thought and made some minor changes to the code, including being able to pass in objects, not just functions. Now I can write stuff like this,

<pre>x = {get: function() { return this.url; }};

function y() {
    this.get_url = function() {
        return x.get.call(this);
    }
};

var super_object = prototype(y, x);</pre>

and now super_object has the methods of y and x, where the order of the arguments decides precedence of inheritance. So I can create a new instance by

<pre>var a = new super_object({url: '/test/this'});
a.get_url() // returns '/test/this'
a.get() // returns '/test/this'</pre>

There some behind the scenes action here. What I did was abstract out the meta object with the intention of overriding it. But on some more thought, I think I will simply make it explicit, this way one could define any number of meta objects with their own magic methods, or if it so fits, simply pass in a blank function() {}, following Crockford's lead. So it would look more like

<pre>var super_object = prototype(y, x, meta); // or whatever you call your meta, or
var super_object = prototype(y, x, function(){}); // for no magic methods or special constructors</pre>

Speaking on speed, Its important to note that there are no call's or apply's ever, though thats not to say one couldn't write it into a custom meta object. Point being, it runs fast, just as fast as typing it all out manually. It doesn't strive to be "classical" in any way. It simply focuses on custom constructors for multiple objects and a simple way to combine multiple objects, given order of precedence of arguments. It could be just as easily used in conjunction with explicit prototypal declarations. Seems like a win-win for keeping it simple.

<strong>*** Edit ***</strong> I forgot to mention, in the above example, x is an object that super_object inherited, but say you override x's method and then needed to call it? well its a good bit shorter since its already an object, simply

x.get.call(this)

No need for specifying the prototype. I think this could easily become a simple design paradigm I might follow, Ill need more experience to judge properly though
