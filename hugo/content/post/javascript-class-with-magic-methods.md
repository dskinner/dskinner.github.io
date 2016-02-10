+++
date = "2009-12-04T12:07:00-04:00"
title = "A Javascript Class with magic methods"
+++

Hey, ok so this is what i have so far, totally preliminary

<pre>function class() {
    var that = function() {
        this.__init__(arguments[0]);
    };
    that.prototype = new object;

    for (var x=arguments.length-1; x&gt;=0; --x) {
        var m = new arguments[x];
        for (var i in m) { that.prototype[i] = m[i]; }
    }
    this[arguments[0].name] = that;
}

function object() {
    this.__init__ = function(kwargs) {
        for (var k in kwargs) {
            this[k] = kwargs[k];
        }
    }
}</pre>

Short, right?

Then I can write something like this,

<pre>class(A, object)
function A() {
    this.get_name = function() {
        return this.name;
    }
}

class(B, A)
function B() {
    this.get_age = function() {
        return this.age;
    }
}

class(C, object)
function C() {
    this.__init__ = function() {
        this.name = "OVERRIDE";
    }
}


var a = new A({name: "Daniel", age: "24"});
var b = new B({name: "David", age: "25"});
var c = new C({name: "John", age: "26"});</pre>

effectively just sticking a little header over normal javascript functions, and everything works as one would expect.

a.name // returns Daniel
a.age // returns 24
b.get_age() // returns 25
b.get_name() // returns David
c.name // returns OVERRIDE

And to boot, it executes at the same speed as writing it the "native" way. Here's what i have for "native" (im a noob so correct any errors)

<pre>function object() {}
object.prototype.init = function(kwargs) {
    for (var k in kwargs) {
        this[k] = kwargs[k];
    }
}

function A(kwargs) {
    this.init(kwargs);
}
A.prototype = new object;
A.prototype.get_name = function() {
    return this.name;
}

function B(kwargs) {
    this.init(kwargs);
}
B.prototype = new A;
B.prototype.get_age = function() {
    return this.age;
}

function C() {
    this.name = "OVERRIDE";
}</pre>

I ran a test importing each implementation, respectively, and got similar results in execution speed and memory size. I created 100,000 thousand objects of each A, B, C and each method occupied 78mb according to top, and each method consistently ran between 2100-2300 ms with variance that occasionally hit 3000 ms. Ultimately its not surprising as all the class function i wrote does is auto write how you would do it natively. What Im surprised about is theres no extra cruft when the javascript runtime compiler handles it. I never intended this to be useful, it was all part of an experiment delving into javascript scope and messing with constructors so i could evaluate the use of a library like prototype.js or mootools.

But hell, so far this little bit of code is turning out to be fairly useful. I imagine if i write more magic methods, the memory size will increase by a small amount. I half expected to see a difference in memory since the C is much more stripped down in "native" version vs the version with __init__ cruft from object function.

This has all been using spidermonkey-bin (smjs) so now im curious to see how other javascript implementations handle the details, as from the get-go i expected a huge increase in memory (not that I know anything about anything) from functions existing in the constructor and then being linked to a prototype, and all those "new" instances called in class. But it all seems negligible, in spidermonkey anyway. This could be a totally different story in IE, lol

for reference, heres my lame-o profile code (i know, i know, but it was enough to find all sorts of issues when exploring javascript scope and constructors)

<pre>var date1 = new Date(); 
var milliseconds1 = date1.getTime(); 

load('custom.js'); // point this to which script to test
var l = [];
for (var j = 0; j &lt; 100000; ++j) {
    l.push(new A({name: &quot;Daniel&quot;, age: &quot;24&quot;}));
    l.push(new B({name: &quot;David&quot;, age: &quot;25&quot;}));
    l.push(new C({name: &quot;John&quot;, age: &quot;26&quot;}));
}

var date2 = new Date(); 
var milliseconds2 = date2.getTime(); 

var difference = milliseconds2 - milliseconds1;
print(l.length)
print(difference)</pre>

**EDIT** Also, function object needs a class(object) so you can call its magic methods, so in C

```
this.__init__ = function(kwargs) { object.prototype.__init__.call(this, kwargs) }
```

Of which, im a little confused, b/c I originally expected to not work. Anytime a Class(X) is called, its constructor gets replaced, so another Class(X) later on will be referring to that replaced class which i thought would cause some kind of error, or so i would think. So deep inheritance might cause some bad mojo with the amount of memory or hell if i know. I haven't looked into that yet

**EDIT 2** Also, im not sure how much of a "class" this is really, if it turns out useful i may find a different name, maybe just call it "prototype" so like
```
prototype(A, object)
function A() {};
var a = new A({name: "daniel"});
```
