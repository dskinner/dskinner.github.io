+++
date = "2012-11-17T02:18:00-04:00"
title = "Sublime Text 2 and multiple cursors"
+++

Two days ago I started using sublime text 2 for projects. I'm coming from a long sprint of VIM usage and I have to say that sublime is pretty awesome. In fact, I'd describe sublime as VIM + awesome. Unfortunately sublime isn't free and not to be a glory taker, but I hope to produce something on par for free in the future.

One thing that really wow'd me into at least trying sublime was the proposition of multiple cursors. With that being said, this wouldn't be my first foray into the lust for nonsensical features. But, only after a day of usage I can honestly say multiple cursors in a text editor is just a plain win.

Primarily, I've used VIM in the past for all but the most heavily-dependent-on-IDE tasks (namely, java). I love VIM. Just the other day I opened a 6.4GB mysql dump to make some minor changes by hand before passing it on to an AWK script for conversion to postgres (yeay 16GB of system memory), but sublime is, well, simply sublime.

Refactoring tools in an IDE typically consist of being scope aware and allowing you to rename method variables and class members. One thing no refactoring tool can touch though is editing multiple values at the same time. As I worked my way on the very first day through sublime on a python project, I found myself wanting to change a value set on five variables. Instead of a default value of `None`, I wanted it to (effectively) read `kwargs.get('', None)`, and I thought, "ok! let's try multiple cursors!".

I moved into position and slammed `ctrl+d` five times and there they were, five cursors ready to alter the default value of five members.

Afterwards, I reflected on the wonder of how practical multiple cursors really were. "Is this just some cheap skate refactoring tool?". No. It's much more powerful. I think that's one of the many reasons I'm a sublime convert now and hope to see this idea spread through free tools in the future.

Thanks for reading, try sublime, hell buy sublime, and feel the preposterous range for which it defines.
