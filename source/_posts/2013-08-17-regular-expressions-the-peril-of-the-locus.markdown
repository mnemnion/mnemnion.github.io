---
layout: post
title: "Regular Expressions: The Peril of the Locus"
date: 2013-08-17 18:43
comments: true
published: false
categories: 
---

Regular expressions are one of those facts of a programmer's life. I write a lot of them, in fun and in anger. I always have a reference loaded up. 

The problem amounts to the fact that the regex is using a symbol set to recognize the same symbol set. This means some characters have to be 'special'. 

We get this from Perl, where regular expressions typically live inside of `/` and `/`. Being Perl, there's More Than One Way to delimit them, but this is canonical. Regex's have a long proud history that I'm glossing over here: pretend they burst forth from Larry Wall's bloated forehead.

In Perl, regexes are quirky, but very, very useful. They were Perl's raison d'etre in the 90s, and they exist in all its spiritual descendents, with slight modifications. The problem is *almost* not the regex itself. It's those frakkin strings.

Larry's regular engine uses a backslash to mark special characters. He borrowed this from the C string, which exists with small variations in all of C's decendants. So... what happens if you want to pass a regular expression as a string?

It becomes exceedingly ugly and hard to follow for many common cases, is what happens. 

Next question: What does a regular expression to recognize strings look like?

Glad you asked. It looks like this: `("[^"\\]*(?:\\.[^"\\]*)*")`, and that's as a bare literal. Wrap it in a string, and it becomes a real monster. I stole this off the Internet, by the way, and merely hope that it works. I think the stringy version looks like this `"(\"[^\"\\\\]*(?:\\\\.[^\"\\\\]*)*")"` but I haven't tried to make it work. Good luck. 

Next question: What does a regular expression to recognize a regular expression look like? Bonus points if it recognizes string-escaped regular expressions as well. It's okay to write two.

If you do some Stack searches on this topic, you will find some genuine beasts. Monsters of the deep. It can apparently be done, which is impressive indeed. This is what a strange loop looks like, when you have a completely ad-hoc higher-level system of representation that lacks composability and elegance. 

This is where Unix goes off the rails. Everything as text is brilliant right until it's not and then you're in a world of hurt for awhile. That it breaks down around **textual recognition** is a sign that something is deeply off.

