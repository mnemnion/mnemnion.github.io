---
layout: post
title: "State of Forth, 2014"
date: 2014-01-08 13:45
comments: true
categories: 
---

I had a [very interesting conversation](http://www.loper-os.org/?p=1390#comments) with Loper recently about, well, superficially it was about Urbit. 

It came at an interesting time, as I was rewriting [Ax](http://mnemnion.github.io/ax/spec.html) in Forth. That's a done deal, but I've temporarily lost interest. As an aside, the Ax inner loop calls for roughly ten basic Forth words, the kind that are normally one to five machine instructions long. That is...not insolubly slow, even on hardware that's somewhat hostile to the level of indirection involved in a noun.

Forth itself has seized and held my attention. 

I remain convinced that Urbit, or something like it, must be built and must be built now. I hope for efficiency's sake that [Tlon](http://tlon.io) will be the ones to do it. 

Similarly, the things [Loper](http://www.loper-os.org/?p=1427) is up to right now are totally right on, and frankly, the sooner the world gets Loper designed hardware the better off we'll be. 

And then I started learning the Way of Forth. People have the wrong idea about Forth, because they think it's a language, when it's actually a programmable program. Now, every language runs on a programmable program, but Forth **is** a programmable program, and therein lies the difference. 

##Tame the Beast

Forth is in danger of becoming a lost art. Few of the younger generation are learning the way. It survives in a surprising number of contexts, far away from the mainstream of open-source, Unix and network-oriented software. Most Forth users are using commercial Forth environments that they paid for. 

This is easy to explain: Forth, the language, has almost nothing to offer. Viewed as a language, it's more than just primitive, it's barbaric. 

Then you go to write something like an assembler, and realize that Forth is dramatically the best language for that job. Seriously, [check this out](http://krue.net/avr/). This is the best assembler you've ever seen. Go find a C assembler if you don't believe me. 

The version of this that runs on the microcontroller, in a [native Forth environment](http://krue.net/avrforth/), lets you interactively design assembler words and test them, on the microcontroller itself. Y'know, no big deal, I bet you do that all the time.

It's been said there are more Forth implementations than serious systems written in Forth. This is actually untrue to the verge of slander, but Forth is meant to be hand-rolled to the task. It is a [problem oriented language](http://www.colorforth.com/POL.htm). By the way, if you haven't read POL, I insist. It is a fundamental work of computer science; Chuck was a student of McCarthy at the dawn of Lisp.

The way of Forth is to take the target architecture and tame it into a consistent environment for getting things done. The stack abstraction may always be applied, and normally at a cost similar to a subroutine-call environment. 

I have my notions of what's driven Forth into its present, moribund state. It's mostly ANS, which abstracts the machine away in the name of portability. This renders Forth a weird concatenative abstract language, taking away its numerous advantages. The C standard is far more literal and close to the hardware than the Forth standard, and this causes the latter `language to suffer immensely. 

It remains the best machine tool in the arsenal. If the job is taming a wild chip, Forth is your friend. It's the best way to punch down, period, and with some support, I can picture it punching up with the best of them.

How to write a post-C environment using existing high-calibre ARMs and possibly Intels? Forth alone will not suffice. But if we stick with [AVR](http://en.wikipedia.org/wiki/Atmel_AVR) for a bit, then gun for modest 32 bit ARM systems, we may discover that the big iron is obsolete. It's also quite possibly dangerous: there may be Balrogs built into the big chips, and certainly, the board architecture we're using is not nearly paranoid enough. 

##Interactive Enlightenment

I don't dislike writing C. I learned it when my brain was fairly plastic, so despite the rust, I can get 'er done. I have always despised debugging it. GCC is pain, mostly concealed with make, though make is also pain when you have to write it. GDB is pain multiplied by pain. I cut my teeth on Turbo Pascal, and this shit *hurts*. The only saving grace of Java is that stepping though that garbage is of necessity well-tooled. C programmers are masochists, to the last one; they not only invented Hungarian, they frequently use it. 

C engages in premature optimization, which is known to be the root of all evil. It won anyway, because Forth provides no way to hand off any programmer discipline to the compiler, and efforts to add them have failed spectacularly. I have a notion, but code speaks and I have a lot to learn. 

There's also the fact that C is Algolic. Algol was designed so the pseudo-code programmers developed ad-hoc could be used to write real programs. It's no wonder that a language driven (that's human language, specifically Western European) design became effective and popular. It's taken decades to learn that the pain of compiling Algol puts an enormous burden on systems programming; indeed, this lesson has yet to catch on.

Nonetheless, let me state a law: If a system does not provide an Algol, it will fail. Dialect matters less than you think it does, but there are no downsides in using something sensible with a decent user base. Lua, say. Lua is a nice Algol. 

I plan to write a nice, Unix hosted system that provides a roomy, enhanced window into the running environment of a second computer. This treats Unix like the fancy terminal it is, giving us the all-important ability to code and use the Internet at the same time. 

The only Forth that is even skeletally complete, from a Unix perspective, is gforth. That's a GNU tool, making the code pure contagious poison that can never be used for anything. Unfortunately pforth, the public domain version, is missing certain essential ANSI term handling words, so I'm using gforth for now. 

The program, provisionally called Forge, may be thought of as a cockpit for piloting a Forth system. Self-introspective, yes, but built primarily to operate an umbilical Forth environment on a remote platform. We may use this comfortable tool to make compact, excellent microcontrollers which can talk to each other and host systems; the wisdom gained may be applied to writing a better host Forth, and then we're in business.

Forge is intended for interactive systems programming. Building out an operating system requires a lot of scaffolding, and there are self-reference problems with trying to host that scaffolding on the system you're assembling. 

I mean enlightenment in the sense of shining a light. Compiled C code is dank, obscure, and our current architectures further convolute the actual execution of code. There have been benefits, but the tradeoff isn't worth it: what you cannot see, you cannot understand without great strain. Show me a tool for interactive physical inspection of the hardware state. Show me how to write it in C. If you know the language, you know the problem: all the information you need is simply gone, and there's no good way to get it back. 

Astute coders can do terrifying things with stack machine optimization. LUAJit and HotSpot are works of pure magic. The fact that this has not been effectively combined with Forth in an open-source tool is part accident, and part missing type information. 

Oddly enough, Forth programmers provide type information, in the form of comments. All good code has them. Nothing in the computer reads these annotations or does anything useful with them. We'll get back to this thought, but not today.  

###The Way

Forth is a Daoist, immediate, personal approach to the computer. I have some problems, that are conceptually large. I intend to write and rewrite those problems until I have some good languages for solving them. 

This post was going to be a survey of the Forth ecosystem; perhaps later. Instead, I'll end with meditation.

```
    If the System provides garbage collection,
    the Language cannot write one.
    If the System provides syntax,
    the Language must use it.
    If the System provides types, 
    the Language shall have them.
    Doing less,
    Presuming nothing,
    Staying empty,
    The Way of Forth
    Provides the Language,
    Enables the User,
    To Build the System
    In Accordance with the Way.
```

Back to the metal. More later.