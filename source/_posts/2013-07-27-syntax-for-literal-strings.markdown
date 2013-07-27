---
layout: post
title: "Syntax for Literal Strings"
date: 2013-07-27 10:09
comments: true
categories: 
published: true
tags: Code
---
I find it somewhat astonishing that the languages with which I'm familiar still start and end strings with the same character. It is as though we used `|` for `{` and `}` in code blocks and relied on context to figure out if it was `begin` or `end`. 

Incidentally, it's quite possible to write a language this way, and an interesting exercise. `for | i = 0 ; i < 2 ; i++ || codeBlock |` should parse just fine. Heaven help you if you misplace anything. 

Check out [bracket delimiters](https://en.wikipedia.org/wiki/Delimiter#Bracket_delimiters) on the Wiki. Two of these things are not like the others. Those two are used preponderantly for strings. 

It's clear enough how it happened. A string has an obvious mapping to literary quotation: `"That's what she said!"`.  ASCII gives us exactly three options: `'`, `` ` ``, and `"`. [It turns out](http://c-programming.itags.org/q_c-programming-language_16297.html) that C was defined using 91 characters, and `` ` `` was not among them. 

Meta enough, I'm writing this in Markdown, and to type `` ` ``, I must type ``` `` ` `` ```. I will leave how I typed ``` `` ` `` ``` as an exercise for the reader. 

So C chose `"` for string syntax, and `'` for characters, and these decisions made sense, somewhere in the mists of time. C also initiated the proud tradition of string escaping, which wasn't invented to get around the delimiter problem, but which can be used for that purpose in a hacky way. String escaping is so you can say `\n` and get a newline, the incidental benefit is you can say `\"` and get a `"`, hence one may include any character in such a string. Two backslashes is of course `\\\\`. One gets used to it. 

Oh hey, just for fun, why not write a regex that will match such strings? Won't take you long, I promise. I'll be right here!

To the point. In typography, we don't do this. We start quotations with `“` or `‟` and end them with `”`. On the [Continent](http://en.wikipedia.org/wiki/%C2%AB), `«` and `»` are used, and this would be my preference as they are much easier to tell apart and don't have two choices for the opening delimiter. If you follow the link, It turns out they are used both `«this way»` and `»this way«` and even `»this way»` by Finns ([of course](http://en.wikipedia.org/wiki/Finnish_language)). We favor the first, because all other brackets in computer programming are inward facing `<{[(like so)]}>`.

What's the point? They aren't on standard keyboards in the US; while any worthwhile editor can get around this, there's a pain point there. Some people will argue a virtue in using ASCII for source code, and while those people [have a point](https://github.com/cgyarvin/urbit), the ship sailed a long time ago. We use Unicode, and it isn't going anywhere. 

The point is that, without proper left-right matched strings, you cannot literally quote your own source code within your source code. This is damaged, for any language that lets you evaluate at runtime (the interesting ones IOW). If we use `«` and `»`, we can use bog-standard reference counting to assure that any properly-balanced literal strings in the source code get quoted. Since in this imaginary syntax a bare `»` not balanced on the left with a `«` is a syntax error, any correct program can be embedded. 

If, for any reason, you need a bare `»`, why not use the ISO standard SHA-1 hash of the Unicode value of `»`? Why not indeed. It then becomes impossible to literally quote that one hash, which is officially the point where it is perverse to pursue the matter further. Concatenate for that one. 

To be clear, `"` for escaped strings is concise and well understood, and with enough convolutions one may write as one pleases. It's syntax such as `'''` for literal strings that grates against my sensibilities. 

Clojure has no syntax for literal, multi-line, unescaped strings. That's too bad; no one does syntax like Rich Hickey, and I suspect that the inadequacy of existing options plays a role here. He may not be willing to go off-keyboard, but I feel that the `«` and `»` syntax has a lot to offer. Certainly Europeans would be pleased. 