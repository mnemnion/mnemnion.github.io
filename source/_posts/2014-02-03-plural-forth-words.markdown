---
layout: post
title: "Plural Forth Words"
date: 2014-02-03 08:16
comments: true
categories: 
---

This is a simple pattern that points to Forth's expressiveness and philosophy.

Most words do one thing, and do it well. Sometimes you want to do that thing several times. 

In Forth, this is very easy and natural to express. For example, the word `key` will block until a key is available, and return it. Sometimes, at the REPL in particular, you want to do this multiple times. So we define this word: `: keys 0 do key loop ;`, and say `3 keys`. This simply calls `key` 3 times. 

We may approach this level of terseness in other languages, of course. In Lisp we may have `(key)` and `(key 3)`, Lua would offer `key()` and `key(3)`. Forth words can't natively know how much of the stack belongs to them, so variadic functions are harder to write. It is impossible to beat the terseness and clarity of `key` `3 keys` in use, and the second definition is transparent. 

Forth itself uses this custom. `cell` gives the number of bytes in a cell, while `1 cells` multiplies 1 by that number, giving the same value. In general, a plural word **must** be proceeded by the number of repetitions. 

The syntax of Forth is brutally simple, allowing for a rich semantics. The most important decision: whitespace is (almost) always important. This was made when Fortran was popular; in original Fortran, whitespace is literally never important, meaning `foo bar baz` and `foobarbaz` are always the same program. 

Consequently, any printable character within a word is fair game. Schemers and friends are accustomed to boolean functions that look like `test?`, which I'm sure was heady fun after decades of `test-p`. Forthwriters do this as well. 

High-level Forth ends up looking like this:

```
: clickloop
	begin 
		event
		event-respond
		1 .left?
	until
	;
``` 

Anyone can read this. `1 .left?` is not entirely obvious. The `.` suggests it's printing something, and the `?` suggests that it's testing something. If it just said `left?` most Forth programmers would conclude that it's setting the test that `until` checks. This is actually handled by `event-respond`, `.left?` is a debug function that prints the stack if `event-respond` leaves values behind. 

All of this is customary, and should probably remain so. Parsing within a Forth word breaks some important contracts, notably the dictionary. I've been mulling over a modular Forth dialect that parses within a word for exactly one reason, access to words defined in another module that are overloaded. So if you already have an `event` word, you can say `event.:book` and get the `event` word from some other book in your search chain. Since the effect of a vocabulary word is to leave its token on the stack, `event .: book` could just be the word `.:` checking the `event` wordlist for the word `book`. The compressed form is possibly Forthright, in that the effect is to either interpret or compile a single word. If we used `event .: book` we would expect 3 words to be compiled, though it is quite possible to have 3 compile-time words (or more) produce a single compiled token, such as `: example [ 2 3 + ] literal ;` compiles one word, the literal value `5`. 

Adding parsing to Forth words is pure Sith and should be done with great care if at all. Retro supports strings with the simple form ` "string"`, which is superficially cleaner than the Forth ` ." string"`, where the spacemark is important. I actually prefer the latter, which enforces a Forth convention that spaces always trail. Anyone who has dealt with two different assumptions about where spaces belong can feel me on this one, I hope. Similarly, cr/nl is always prefix. These are good conventions. If you need leading whitespace in a string frequently, first: consider that you might actually need trailing whitespace and second, write a word for it. It's an interesting challenge, try it! 

There's a more subtle problem. The Forth interpreter does this: `word? number? if succeed else fail then`. Retro has to try `string? word? number?`; I don't like anything that breaks the power to redefine a word. Even the absurdity `: 1 2 ;` is a consequent with value; consider a special vocabulary containing `: 10 [char] A ;`. Or `: 10 10 select.window`, which keeps the number `10` around for just long enough to make the definition. 

This imaginary modular forth would try `word? module-word? number?`, meaning a redefinition of `word.:book` would block `word` in the `book` module. The compiler should complain about any word that contains `.:`, and be loud about if that word actually blocks a module word. This is better than the user complaining because they need the ability to make a word containing `.:` for whatever reason, and the compiler won't let them. 