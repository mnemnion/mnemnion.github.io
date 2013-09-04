---
layout: post
title: "On the Implementation of GGG"
date: 2013-08-01 13:14
comments: true
categories: 
published: false
---

[GGG](/blog/2013/07/30/generalized-generative-grammar/) is intended to be human-readable, and also to be ripped through rapidly by a computer. What an implementation does with that input, in terms of representation and the details of execution, is its own business. Different environments and tasks favor different forms and functions. 

The first step is to standardize the grammar, and for that I'll be using Sublime Text's syntax highlighting tool. It is a blunt tool, which is good, because formatted GGG must be parsable with nothing but regular expressions and counting. I hope to extend that requirement to a functional GGG. It should be practical to port the regular expressions to Pygments, allowing for blog syntax highlighting of GGG. 

I'm starting with the language side and working down to actual implementation, which will happen in layers or rings.

GGG is a whole reference grammar language, but it has proper subsets. LGG, literal GG, is a funky way to specify a literal value. RGG is a regular language for describing regular stuff, DGG is deterministically recursive, CGG is fully declarative, and FGG = GGG contains our restricted functional language. 

CGG is itself powerful and useful, and is where my focus lies. Sometimes I'll call it GGG and then refer to FGG or functional GGG. Sorry about that. 

The rings are actually composable, rather than merely an onion. More on that later. 

I intend to write the reference implementation in a chatty, commented fashion, using code that is clear rather than fast per se. It will mostly be turned loose on small data sets anyway, even torture cases can be made acceptably short. I am promised O(n^3) behavior from worst case GLL, and can live with that. 

Incidentally, having a functional notation around is a great way to bring down worst-case performance any time it actually comes up. It's mostly cooked up by grammar nerds, since given the tools available, who would choose worst-case performance?

The second implementation, ideally in the same language while being in C, will aim at being small, fast, and dirty. It will take seriously the pass/fail premise and simply explode on bad data, maybe tossing you an error, maybe not. It's going to be a piece of system software that can seriously sit in front of your Internet going "Yep" and "Nope" without blowing up your battery or CPU. 

Both need to be distributable as object binaries which start up fast. The former, and ideally the latter, should also be available within a decent REPL interactive environment. 

I'm favoring [Lush](http://lush.sourceforge.net/). Lisp is a lovely language to write parsers in, and I'm familiar enough with the idiom. Lush promises to make it easy to drop down to C where needed, and to virtualize memory over large stored data sets, and other stuff that leads me to think it's the right choice. I hope the user community proves friendly, I'm brand new here. 

The two implementations will unit test each other. It's the only sensible thing. One will take its time, preserve all bits of importance, interact with the user, attempt to recover gracefully, and provide useful error messages. The other will die, or not.

They will both produce the same result, which will be two return values: either false, or true with a compact binary encoding of a tree map to offsets in the parsed binary data. This is a data format that is likely [older than writing](http://en.wikipedia.org/wiki/Caral), certainly old enough that we will pick, rather than invent, a structure for it. 

Both implementations will be single threaded, for clarity on the one hand, and for use on limited systems on the other. The fast implementation should be 32 bit system safe. 

Many implementations may be expected/hoped for. A language like Go, with a good garbage and concurrency story, would make a fine GLL candidate, and Instaparse in Clojure could be hacked, or followed, in providing a GGG implementation for the JVM. These could be faster than the reference C implementation for grammars that use a lot of parallelizable tasks, given sufficient cores. 

An important implementation, which I'm in no hurry to write, will be a GGG implementation of GGG. Fully functional GGG will have function rules, into which one may drop arbitrary data before parsing begins. It will be possible, and absolutely necessary, to write a GGG program which has a top function rule into which you drop a GGG program. The function will then validate any provided atom against that GGG program. This is a crucial part of [freezing](/blog/2013/08/01/celsius-versioning/) the code, which is not yet even steam, but the merest mental plasma. 

##Constraints on Functional GGG

There are two parts to functional GGG: a language to specify functions, and an execution semantics that unambigously translates that language into an executable representation. [Nock](https://github.com/mnemnion/urbit/blob/master/doc/book/1-nock.markdown) is the best executable representation I'm aware of. I don't necessarily buy it as the best executable representation which is possible.

I thoroughly agree with the noun = (atom ¦ cell) construction and with the sufficiency of the minimal operators. However, if we're to do math, let's just include the rest of the basic operators. Someone, somewhere, with a pencil and paper, will thank us for this. 

Decrement can fail as well as succeed, as we have no concept of negative numbers and no need for one. A failure produces the atom one, a success a cell with the atom zero as the head and the value as the tail. That's a lot like our rules, which return a 1 if they fail and a 0 plus a cursor increment if they succeed. 

Hoon is right out as the language to specify functions. GGG has to be readable, period. I'm still not sure if Hoon is Brainfuck reloaded or the second coming of Djikstra, either way, it isn't extended extended Backus-Naur for binary, and GGG is. 

It's also far too general. I think. It's hard to tell with Martian, which is more than half the problem to date. Hoon shares a goal with FGG, which is to translate cleanly into the executable representation. Hoon has many other goals, none of which seem directly relevant. I may change my mind. 

Our syntax will be Schemish, for the usual reasons, using `[` and `]` because `(` and `)` are for ordinary precedence and each rule level has a distinctive character (or a few) that defines it by its presence. It will be heavy on the macros, since it exists only to be statically rewritten into Nock, which I'm just going to call ER for Executable Representation. Since it may well not be a Nock, but rather a Nockoff. 

The first, obvious thing our functions must be able to do is invoke another rule, and branch on the result. They must be able to move the cursor forward as often as they'd like, and must also be unable to move it backwards at any point. This is a soft guarantee of halting. 

Invoking rules is safe if the grammar contains no infinite loops. That's actually a statement about grammars, not GGG specifically. If it does have loops, a compliant GGG implementation should detect them; this is not an absolute requirement, and may be done heuristically. Static analysis of grammar can prove the impossibility of such recursion, but cannot detect all actual cases of it (that is, it being possible, it may not be provable from static analysis that it can be created with a specific input). Most recursion, even pathological recursion designed as attack, may be detected in practice. A compliant GGG implementation must be able to hang itself on bad input, but is certainly encouraged to refuse. Many GGGs will not have to encounter potentially hostile grammars and may be faster accordingly. 

It is worth adding that any declaration-heavy computation is more or less in danger of Billion Laughs / fork bomb style attack. Every system has its attack vector, and these are at least well understood, if not by me. 

Functions may iterate, but only downwards to zero with integers, and comprehensions over finite data (map et al) are the first class choice. One core function will be a selector over the digest, returning any data which meets the query requirements. It might look something like `[$ rule-1 rule-2 rule-3.rule-4.rule5]`. Functions may not add to the digest except by returning true and moving the cursor to the right. 

Note that in selector syntax, rule-1 will return a single literal data value which is an aggregate of every literal match for rule-1 which has so far been digested. rule-1.rule-2.rule-3 represents recursion in matching: that is, all rule-3 matches, produced from a rule-2 context which is in a rule-1 context. It does not represent a namespaced rule, as it does elsewhere; `$` is a special form. 

There is exactly one sort of side effect allowed: a print function, which emits strings along some predictable channel. This is purely for debugging and comprehension and an implementation is welcome to ignore any such functions, which needn't be specially marked. Errors being an implementation matter, the print function cannot in principle be used for error reporting,

Functions may call themselves, if they have moved their internal cursor forward. To put this precisely, a function calling itself implicitly compares the internal cursor (which is magic) with the cursor of the calling rule, and if the former is greater than the latter, it calls itself. Otherwise, it returns in whatever state it's in, success being another magic variable that is born false and may be set true.

Functions may call other functions, but it is exactly like calling other rules: the function gets the digest and the cursor, and returns false, or true and a new cursor. To simulate calling a generic function, and mostly for readability, there is a macro expansion mechanism. It's worth reiterating that functions may call each other in a way that is indirectly recursive, as long as a) the calling-cursor is moving forward between calls or b) the condition that creates the call is testing some value that is provably decreasing over the cycles.

This does enable a limited form of data sharing, since we can call a function on a literal value. That function must return success and a cursor or failure, but the cursor may be discarded. This means one may pass a function a literal value that it then interns. It can then fail, allow one to pass information that the function would not validate, or succeed, allowing one to pass information which the function would validate. I believe this enables arbitrary data passing, with the understanding that there is no way to share variables, which are always equivalent to whatever literal value they happen to be holding. There is no concept of a passable reference.

These will be somewhat weird macros, because they will be typed. One will be able to provide arguments to macros, indeed there is little point in having them otherwise. Those arguments will be provided in the form of rules at definition time and literal data at run time. The data must pass the type rule in order to be used. You may use `@`, the rule that always succeeds, if you wish.  

A great deal of what a parser must actually do can be done with selectors and rule-like functions. For the rest, there is math, so long as the calculation you specify is bounded. If you're in truly deep waters trying to specify a function, ask yourself: are you trying to do something to the data that isn't parsing or validation? If so, you are using the wrong tool in the chain.

Success does not cause a function to exit: it is quite possible for a function to achieve success, and wish to keep parsing for even more success. Functions exit when they run out of code, and return `success` and `cursor`.

When functions exit, they are saved/closed over whatever internal state they're in, and the next invocation uses that function. The definition of a function may provide arguments, and if the implementation does not fill those arguments, the function must fail. The arguments are specified as rules which the binary data provided by the implementation must adhere to. An invocation of a function by a grammar cannot pass data into an argument. Arguments are applied once, when the grammar is loaded, and may not be changed during parsing. 

Note that, as is common in functional idioms, a function may perform an effectively infinite calculation as long as a moving bound is is provided. A function may, for example calculate ten more primes every time it is called, an operation with no known upper bound. 

This is a useful idiom for validating a data format: provide a calculated infinite series that has good amount of variation, and encode it in an insignificant bit. This is a handy way to validate data which is otherwise high-density and correspondingly permissive in form. 

New variables within a function are created by assignment, unless they already exist. Arguments are provided as variables, as well as certain magic, like calling-cursor, cursor, and digest. They have the same namespace as the function they are in, but are scoped to the function alone. Variables are mutable, and are kept around until the parse exits. 

I am considering requiring variables to be in single quote marks, which intentionally get no use in the existing standard. `'foo'`, just like that, shows it isn't a rule and serves to namespace variables separate from functions. I think it also does a good job of separating variables from literals, which are immutable and assigned outside of function rules. `foo` would be some literal value defined elsewhere which the function is not enabled to redefine. 

Though mutable, variables may contain only literal values. These may be found in other variables within the function, retrieved off a cursor, or found within the digest via a selector or back cursor. They may conceptually contain any literal value, no matter the size. No method to allocate or free variables is implied or provided, but if you set a variable that had a large value to `nil`, the implementation will have no reason to keep the old value around. 

Variables will not actually contain aggregate data, at least conceptually. The single atom in a variable may be typed by validating it with a rule, and syntax will be provided for doing this and for naming the offset ranges within the atom created by the rule matching.

A word about these cursors: a function can create them whenever it would like, they are merely offsets to locations within the atom being parsed. They are nothing but integers, so storing and retrieving them won't do anything surprising. The magic cursors are `calling-cursor`, which is a literal value (not a variable) which corresponds to the cursor position when the function was called, and `cursor`, which is the value returned when the function exits successfully. `cursor` may be equal to calling-cursor, or greater, but never less. The increment-only function cursor may be named `¢`, perhaps.

Functions may import namespaces, and call the rules and functions within them. 
	
There is no equivalent of eval, as we do intend to halt if at all possible. FGG will be capable of implementing GLL, and hence GGG, given the sternly mathematical and terminating nature of that task. At least, it cannot be considered done if we haven't done so. Without the debugger on, it is incapable of "Hello World", and it always will be. 

There are a few good reasons to nevertheless implement GGG using this restricted language. One is freezing: a language is only as frozen as the language of the reference implementation, and C will freeze right around when Hell does. It may be fossilized, but this is nowhere near good enough. 

If this were the only reason, Hoon would suffice, as it's well on the way to inky black. 

##A Note About Binary Math

That's binary math as in binary and unary operators, not binary the representation format.

FGG will have an enclosed prefix syntax, because of the usual advantages. However, at a guess, 90%-95% of math heads will be unhappy trying to read a purely prefix-enclosed piece of math. Since doing math, particularly simple arithmetic, is a large part of functional validation, some sugar is provided. 

Rules at the first position are treated as functions: they read forward from the `cursor` and succeed accordingly. A block composed entirely of rules is threading the cursor between those rules. A function is just another rule, remember, and cannot have arguments. A macro will have arguments, in the sense described earlier. 

If a literal value is at the first position, or a variable (which is, recall, a reference to a literal value), then *if* the next token is a built-in function with an arity > 2, the built-in or macro is swapped in place with the literal. Since the math operators are built in functions, that means [2 + 3] will turn into [+ 2 3]. Be careful with this; [2 + 3 4 5] will turn into [+ 2 3 4 5] but [2 + 3 + 4 + 5] is a syntax error. There is no notion of operator precedence or association.

Personally I find textual math easiest to read if all operators are unary or binary and precedence is neglected in favor of parentheses. Easy to read is what we want here; Lisps' extreme regularity is to support metaprogramming, which we do not do with GGG. 

##The Use of Representative Functions

Function rules are written directly into grammars, when necessary, and serve two purposes: to specify the function in a human-readable manner, and to exactly translate that function into an executable representation which is fairly fast, though favoring clarity and generality. A GGG formatter *must* be able to translate FGG into an ER, and also *must* embed this as metatdata after the semicolon terminating the rule. This is one of the most important functions of the formatter, along with type unification. 

No implementation details are provided, other than the required form of the parse tree representation and compliant validation of data. This includes how the functions are executed. Interpret the FGG directly, use the ER, compile to native, substitute a less clear but faster algorithm, the choice is yours. The second one is most reliable, because it specifies the interpretive semantics in pure math. It may not be the fastest.

You really want to make sure your function does the right thing, though. If it does the wrong thing, that's bad. The only worse thing is if your actual code does the right thing and the ER/FGG indicates the wrong thing by accident. Nevertheless, the readable implementation of something like a cryptography function is often much slower than, say, the piece of silicon that actually runs it. These things are cautiously allowed. 

You may be gathering this, but a major use case for these functions is to take encrypted data, and a key, decrypt, and validate the decrypt for type. There's going to be a lot of that going around, I hear. 

##Global Namespacing

GGG is a standard, which will not be required to be attached to the Internet to function. Therefore, global namespacing will be a social problem, not a technical one.

The solution to this problem will be to reserve all globals shorter than eight characters for namespaces which are standard or definitive. Make your top level rule names descriptive and all will be well. A typical grammar never calls the top rule itself, so this is not too much to ask. 

...That's about it. Digital feudalism by benevolent dictatorship, basically.


