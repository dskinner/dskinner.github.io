+++
date = "2015-01-10T03:09:00-04:00"
title = "Writing Boring Code"
+++

I have bad news. Using Go as a shared library on Android 5.0 is simply not a good idea. In fact, it's a horrible idea. Android 5.0 introduced a new runtime called ART. This replaces JIT compilation (Just-In-Time compilation).

If you don't know, a JIT analyzes runtime heuristics and modifies the compiled program to run faster. JIT is one of the reasons you might see javascript outperforming C code on the DNA regex benchmark that's been floating around for years.

ART actually precompiles dalvik bytecode to native code and runs that native code in its runtime. As a refresher, when you write java code for android, the java code gets compiled to java bytecode and then gets transpiled to dalvik bytecode to run on the dalvik virtual machine (so no one has to pay oracle any cash).

Tangent, there's a new experimental compiler for android studio (based on intellij) that compiles java 7.x source directly to dalvik bytecode.

But, the ART runtime is much like the Go runtime, and the two fight over things. For the most part, it works and works well. But, when you begin trying to load native code such as PublisherAdView for ads that load in chromium webview that loads native code, bad things seem to happen.

It is essentially a no-go. For pure Go projects as is the initial target, this is a non-issue. But, for integrating Go into a normal Java app, this is a huge blocker. I've given up on using Go for normal apps.

I've actually stopped using Scala as well. I like Scala, and programming in a functional language (i.e. Lisp like) has really taught me a few things I appreciate greatly. There just hasn't been much uptake from the other android developer on our team. He's much older than me, in his 60's. He doesn't have anything bad to say, he just doesn't pursue it or have much interest.

I really can't blame him. It's not just Scala, it's anything that becomes too involved that's not well documented. He dropped an email about fixing some SQL entries in a 2 year old word game I wrote but mentioned he wasn't sure how to build the project.

The issue was two-fold. First, there's five different versions of the app; google, google pro, amazon, amazon pro, nook. Second, two years ago Google didn't have tools to help with this kind of stuff. As of maybe two months ago, they finally have stable tools to help with this.

I wrote a Makefile that would build all the apps and collect all the apk files into a single directory. If you're running OSX or Linux, as most developers on our team are, then it's trivial to build. If your running Windows, you're shit-outta-luck.

Another tangent, I found a make.exe command for windows a year back for a browser extension plugin I wrote that worked pretty well, based on the work done by the git-for-windows team.

Still, me giving up scala is actually more like paying respects to Go. One thing I've really come to embrace with Go is being boring. Boring works. Boring is readable by people other than me. Boring is quickly buildable by people other than me.

One thing I've been considering is the golang/mobile repo added a Dockerfile. It's really neat, boots up ubuntu, installs build tools, android sdk, go, gradle (used in android studio to build projects), and builds your source code for you. What's neat is now Docker is available for windows. Even bootstrapping android studio and android sdk with necessary requirements can be a pain. But saying "did you install docker.exe? great just run this file" is really boring and works. This works whether you're a developer or not.

Hell, even if I'm not writing go code, it works. It works for developers, it works for build servers, whatever.

So I've gone back to Java, I'm embracing the boring. I'm also typing "public static void" a lot.

I like Go like I like functional programming. It's taught me many things and it's certainly worth review.
