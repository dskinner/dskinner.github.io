+++
date = "2009-12-04T08:14:00-04:00"
title = "Baffling Results from my Javascript Class(-ishness)"
+++

** Edit, regarding the following, I suddenly realized what the problem was, .call is slow and was used in the "native" approach. Nevertheless Ive had positive result following through on multiple inheritence and magic methods. refer here, <a href="http://wp.me/piHZk-14">http://wp.me/piHZk-14</a> **

Ok, So recently I've taken an interest in javascript. By interest I mean taking it more seriously as a language. One of the first things I wanted to do was to see if I should adopt a framework like mootools to allow for classical inheritance type of stuff or if I should develop using the prototypal javascript inheritance. This lead me to really dig in deep to javascript scope and all of its nuances, especially considering prototype.

Along the way, pecking away at my smjs console (aptitude install spidermonkey-bin), I eventually wrote this function class() {} as I was trying to see what I could get away with in poking at the scope of functions and their prototypes. I was particularly annoyed with a seperation between the constructor and that which was prototyped and the foresight required, which Im going to lack since Im new to the game. Anyway, heres the function,

<pre>function class() {
    var that = arguments[0];
    for (var x=arguments.length-1; x&gt;=0; --x) {
        m = new arguments[x];
        for (var i in m) { that.prototype[i] = m[i]; }
    }
}</pre>

effectively, this allowed me to write my prototypes in the constructor as well as extend functions. It was all in the name of learning and I wasn't considering it practical. So basically I wrote stuff like

<pre>function object() {
    this.init = function(kwargs) {
        for (var k in kwargs) {
            this[k] = kwargs[k];
        }
    }
}

class(A, object)
function A(kwargs) {
    this.init(kwargs);
}

var a = new A({name: "Daniel", age: "24"});</pre>

I was also playing around with object constructors (unsuccessfully) curious as to if i could implement magic methods that could be inherited and run automatically, but yeah, that went no where so i was all but about to abandon this whole excursion when I decided, before I do, I wonder how much more memory my function class() {} uses and how much slower it is from doing it the standard way. By standard, I mean what I basically learned from perusing the net and from MDC javascript 1.5 Engineering Model Example. Heres what I have for the "standard" way

<pre>function object() {}
object.prototype.init = function(kwargs) {
    for (var k in kwargs) {
        this[k] = kwargs[k];
    }
}

function A(kwargs) {
    object.prototype.init.call(this, kwargs);
}

var a = new A({name: "Daniel", age: "24"});</pre>

Now my profiling isn't very scientific I suppose, I used top and timed the execution from within javascript, but the results are consistent. What I basically did was this

<pre>var date1 = new Date(); 
var milliseconds1 = date1.getTime(); 

load('test2.js');
var l = [];
for (var j = 0; j &lt; 5000; ++j) {
    l.push(new A({name: &quot;Daniel&quot;, age: &quot;24&quot;}));
}

var date2 = new Date(); 
var milliseconds2 = date2.getTime(); 

var difference = milliseconds2 - milliseconds1;
print(l.length)
print(difference)</pre>

where load('test2.js') was the "standard" way and load('test4.js') in a seperate file was my way. The first thing that caught me off guard was that memory consumption was the exact same. I was half expecting my method to take more memory b/c the function definations existed in two places, but I guess the javascript runtime compiler doesn't cause this to happen, so yippie freaggin do da. Now what left me baffled was that my way was consistently faster then the standard way. Here are the time results, running 10 in a row

* all times are in milliseconds
=== Standard ===
54
58
49
55
55
53
53
49
53
52

=== My Way ===
42
41
42
45
45
42
42
48
48
48

The numbers were close, so then i decided to increase the number of objects created to perhaps provide a more significant and visible difference. So I increased the number of objects from 5,000 to 500,000.

=== My Way ===
5716
4257
4229
4331

=== Standard Way ===
7601
4866
4913
5564

Its as if the javascript compiler runs faster instantiating an object property and linking it to a prototype then it does when just instantiating a prototype property. And it doesn't require any extra headway in memory to do it.

If theres any javascript ninja's that can explain whats going on, thatd be simply awesome. Speaking of which, im gonna go find mailing list now...
