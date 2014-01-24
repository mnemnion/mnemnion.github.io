---
layout: post
title: "Literal and Figurative Languages"
date: 2014-01-24 12:12
comments: true
categories: 
---

Axiom: there are no clean separations between programming languages.

Corrolary: if one can be found, someone will design a language specifically to blur the resulting boundary.

The terms high and low level languages are frequently bandied about. If your word of code stretches from Python to C, these are comfortable categories, with room for interesting bikeshed debates on the margins. 

At one time, this almost made sense. Languages were either properly interpreted from strings, or compiled into genuine code. Even in those hoary days there were vigorous communities, notably Lisp, Smalltalk and Forth, where these distinctions were blurred or simply did not apply. 

This is no longer even vaguely how things work. On the one hand, most interpreted languages are compiled to a bytecode, and many are further optimized from there. LuaJIT in particular can demonstrate superior performance over compiled languages in certain circumstances, in particular cases where the input to a functional cascade can rapidly change character. JIT compilers can keep up with this, while a static compiler by definition cannot optimize for data the program it's compiling hasn't seen yet. 

One the other side, which I feel is less known, we have OpenCL. This is a decidedly low-level dialect of C, in which pointers are not allowed and 'functions' are syntax to allow inlining of code. This is compiled and optimized into multiple dialects (CPU and any available GPUs), on the fly, from strings. A running OpenCL program makes some of the fastest machine code possible from strings in the runtime environment.

Clearly this distinction has completely collapsed. The more interesting division is between literal and figurative languages.

## What This is Not

The distinction is not between static and dynamic, nor is it about performance or degree of interactivity. I can happily find orthogonal examples of all of these in both the literal and figurative family.

The distinction is this: a literal language abstracts the hardware, while a figurative language abstracts a problem domain. 

Assembly is of course literal, along with Forth and C. Haskell is highly figurative, while the Lisps tend to blur the line in interesting ways: within only Scheme, Racket has a layer that is more hardware oriented, while Chicken delegates this aspect to C. SBCL can dig down as deep as you want, but Lisps in general are figurative in emphasis: invisible garbage collection is a figurative pattern and a pillar of Lisp programming. Despite this, the garbage collector itself has occasionally been written in Lisp. I confess I have trouble understand how this is done; low level Lisp is almost a different language. 

Hoon is an interesting case. We think of Nock as a sort of Hermetic seal; above the Nock layer, Hoon is straighforwardly literal. This is not uncommon; Retro, a Forth dialect, targets a very simple virtual machine that is quite literal on the target architectures. Lua is more figurative, but it's a brisk afternoon's work to discover how the virtual machine works and how to target it from C, Lua's home environment. 

This intersects with compactness in some ways but not others. Assembly, our most literal, is as complex as the architecture, which has gotten truly gnarly on our flagship systems. 

I'm doubling down on Forth because it combines a low mental surface area, a literal machine model, interactivity, and introspection. The 'word' is a strangely powerful abstraction. If you want (and I do) a colorful, interactive environment for exploratory systems programming on existing chips, Forth is the natural place to start. If only it had a nice, modern type system...