+++
date = "2009-12-04T17:31:00-04:00"
title = "Whats wrong with javascript prototype?"
+++

<strong>** Edit **</strong>You can see my current approach to these frustrations here: <a href="https://github.com/dskinner/js.js">https://github.com/dskinner/js.js</a>

O thats easy, ok lets write up a base object we'll want to inherit from (note, I would not actually write a this.get_name to retrieve a this.name, duh, its all in the spirit of an easy read)

<pre>function A(name, age) {
  this.name = name;
  this.age = age;
  this.get_name = function() { return this.name; }
  this.get_age = function() { return this.age; }
}</pre>

Good, ready? oh shit, wait a sec, according to MDC we were being naive. Now the only way to call the get_name and get_age functions is with an instance of A. Well crap, that doesn't help inheritance much, prototypal or not. Alright so lets do this right.

<pre>function A(name, age) {
  this.age = age;
  this.name = name;
}
A.prototype = {
  get_age: function() { return this.age; },
  get_name: function() { return this.name; }
}</pre>

Yeay, yippie for us, now lets extend A with B

<pre>function B() {};
B.prototype = new A;</pre>

Yeay, yippie, now lets make C with an extra param and function and extend A.

<pre>function C(name, age, title, money) {
  this.title = title;
  this.money = money;
};
C.prototype = new A;
C.prototype = {
  get_title: function() { return this.title; },
  get_money: function() { return this.money; }
}</pre>

Good? alright lets.. o crap! it doesn't work. Right, right, thats right, I totally erased the A prototype by using the prototype = {} syntax, ok so that syntax is no good for working with inheritance unless its the top level thing, but crap, i dont wanna think about that. Whatever, lets just fix C for now though readability will be a little funky, hey .. i know! lets just say if i use the prototype = {}; syntax, thats a way to differentiate it as a top level parent! yeah! thats great justification *cough*not*cough*

anyway

<pre>function C(name, age, title, money) {
  this.title = title;
  this.money = money;
};
C.prototype = new A;
C.prototype.get_title = function() { return this.title; }
C.prototype.get_money = function() { return this.money; }</pre>

ok, lets fire this bad boy up and test C.. Where the hell is my name? and age!? son of a.., I see, so i need to manually call the constructor of the same damn thing that I C.prototype='d

alright, its cool, lets fix it, I can dig it. Lets add that A.call

<pre>function C(name, age, title, money) {
  A.call(this, name, age);
  this.title = title;
  this.money = money;
};
C.prototype = new A;
C.prototype.get_title = function() { return this.title; }
C.prototype.get_money = function() { return this.money; }</pre>

Sweet buttery buttons! It works! wait a sec.. why is this code running slower then before? Yeah yeah, theres creating the name and age now that wasn't before but its more... its .call() ! Wth?! why is this causing it to go slower? Alright alright, its cool, lets just... migrate these object property inits out of the contructor and into a prototype function that will handle the needs of all that inherit it since its mostly the same.

Ah hell, im hungry, maybe another day..
