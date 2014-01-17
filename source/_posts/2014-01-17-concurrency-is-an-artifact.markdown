---
layout: post
title: "Concurrency Is An Artifact"
date: 2014-01-17 11:59
comments: true
categories: 
---

Your concurrency problems are a direct consequence of your software architecture. 

Concurrency is a consequence of carrying a solution forward without remembering what it's trying to solve.

Preemptive multitasking is a weird thing to do: you interrupt the computer, with its permission, do something else, then it goes back to what it's doing. There's all sorts of complex negotiations; the good news is, it works, usually. 

This is a solution to a hard problem: how to do a bunch of things with only one CPU.

So I'm guess you have 4 CPUs, which makes the architecture fundamentally asisine. 

All your concurrency problems stem from this. Do hypervisors have concurrency problems? Wouldn't know; bet they don't, because they sensibly allocate processors and memory. 

If you took a systems architect from a more sensible time, the 1970s say, and gave them a decent AMD chip, they would first go for a walk with a beer and a tab of acid to consider the implications. They would proceed to write something entirely different from what you have.

It would be entirely based on the idea that they have 4, let's say, supercomputers of unfathomable power, hooked up to more core memory than they can understand, attached to store counted in units they have never seen before, except in jest.

Would it emulate Unix? Would it create a master control program that emulates multiple Unices?

No. This is not what it would, at all.

It used to be standard practice to rearchitect systems in light of new capabilities. This stopped because of two autistic people with different fixations: Bill Gates and Richard Stallman. My esteem for both men is considerable, in light of the former's philanthropy and the latter's stubborn and correct fixation on the privacy and freedom implications of open source systems. 

They share a fatal obsession: backward compatibility. Stallman, in particular, has an almost detestable belief that software should be written once and for all. Imagine an artist with the same attitude to painting, or the novel; we would deride them as the martinet that RMS often presents.

There are other, better options: keep the old beasts running, rewrite for new systems, or wait around for awhile until speed gains mean you can emulate. Unix has been under emulation for quite some time.

We have two tricks: speed gains, and writing fresh software. And we appear to be fresh out of bubblegum.