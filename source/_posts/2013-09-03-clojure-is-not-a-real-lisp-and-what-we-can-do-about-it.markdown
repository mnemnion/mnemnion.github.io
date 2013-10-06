---
layout: post
title: "Clojure Is Not A Real Lisp (&amp; What We Can Do About It)"
date: 2013-09-03 12:17
comments: true
published: false
categories:
---

Clojure is my favorite language to write in. Period. Keep that in mind, I beg.

In learning Clojure, I have encountered more than a few old-school Lisp hackers who dismiss Clojure as simply not a real Lisp. To be honest, I assumed they were poor losers. Poor losers abound in software, and always will; I remember being cornered once by a wild-eyed proponent of [Oberon](http://en.wikipedia.org/wiki/Oberon_programming_language), in the early Naughties.

Lisp hackers are notoriously poor losers. As I travel further down the road, I'm starting to understand why.

##A Real Lisp

Clojure has the *syntax* of a real Lisp. In some ways, this is better than being a real Lisp. In fact, I could make a cogent argument that Clojure's particular gift is abstracting the syntactic advantages of Lisp away from the details of implementation. Witness `recur`. Recur says, "this recursion is one which must be optimized into a loop", while all other recursion lacks this requirement. There's a movement in Clojure to add tail-call optimization to the language, when the JVM supports it.

If this is good enough for you, Clojure is a great language. Hell, it's a great language anyway. But check it: Java and Javascript are C like in a syntactic sense, and you can even compile arbitrary C to Javascript via Enscripten. Does that make the resulting code C code? That's a philosophical question, unless you have an application where it isn't, in which case the answer is simply "no".

Under the hood, Clojure is a bunch of objects. Java objects, Javascript objects, whatever. There's a Clojure that compiles to Scheme, then C, then runs. Scheme is being used to simulate the object system here.

Clojure could no doubt be written in C++. Writing it in C would take faking an object system; either brace tongue would require a garbage collector. No big deal, those are always written in a systems language, for obvious reasons.

To program Clojure, and also to get going on a Common Lisp project, I've adopted Emacs. I didn't want to, but I'm getting settled in. Mostly by reprogramming the old beast extensively.

This has lead me to emacs lisp, which is a Real Lisp. It's one of the realest, in a way: it shares an ancestor with Common Lisp but never needed to go through the abstraction process that produced the latter. The documentation, which is excellent, also goes to great lengths to show off the bits under the hood.

##Implementation Matters (Except When It Doesn't)

Elisp has a basic data type called an association list. You will find this, somewhere, in all real Lisps. Clojure doesn't have one. Let's look at why.

In Elisp, a list is a collection of data types called cons cells. This an exact thing: it is a collection of two pointers. That is precisely all a cons cell is. Normally, the first pointer points to a value, and the second pointer points to the head of the next cons cell in the list.

A list like ` '(1 2 3) ` has four cells in it. The last cell is is an empty cell
, aka `nil`. That means the last cons cell has no value in either pointer.

So what if you take a cons cell, and instead of one pointer to a value and one pointer to another cons cell, you took that second pointer and pointed it at another value?

You have an improper list, is what. Also called a pair, and written like this: `(1 . 2)`. They get used somewhat like key-value pairs, except Elisp and the other Lisps have maps too.

What we often see is a list where each value pointer points to a pair. This is called an association list.

Clojure has what's called a Map Element. It's a key and a value. Maps are built up out of map elements. Lists are not; they are conceptually distinct. This shows clearly in Clojure's treatment of the empty list as well. An empty list in Clojure is a List object which happens not to have any elements at the moment. It is not equal to `nil`, and now you know why.

An association list has a definite order, like any list. This is a natural consequence of the data type. Clojure provides the *syntax* of this type through sorted maps. The implementation details are entirely different.

What happens if the last pointer in a list points back to the first element? Well, you have an infinite circular list, and if you try and read to the end of it, you'll hang. Clojure provides `cycle` as a function to turn any sequence into a potentially infinite circular structure. There's a lot of this kind of indirect correspondence involved in making Clojure what it is today.

##A Maze Of All Different, Tiny, Twisty Passages

Now it's time to explain the provocative term Real Lisp. The antonym of real here isn't "fake", it is "emulated". Distinguishing between programming languages is a hard problem, and ultimately the distinctions are somewhat surreal.

Real languages may be run in emulation, and one may built hardware to make a virtual machine into a real machine. The distinction is one of environment, not nature.

Clojure manages, impressively, to closely emulate the semantics of a running Lisp environment within an emulated machine, the JVM. It's even reasonably fast, considering.

In general, however, Lisps are real. Especially the older ones target hardware in a well-defined way, providing only primitive data types, much like C. All the `higher level` abstractions are ways of manipulating these primary data types.

C concentrates on the higher-level abstractions of doing mathematics. The C operator precedence heirarchy is a work of terrifying beauty. Even in a very powerful grammar, it is a headache to write.

Lisps focus on the higher level abstractions of pointer manipulation. If you know how a lisp works, and with Elip in particular this is made very clear, you know more or less what the machine is doing when you run a program.

I'm focusing on Elisp because Common Lisp provides a much heavier collection of abstractions. It's more like a C++. Elisp is the only surviving descendent of the old school Lisp Machine lisps. It has a tiny C core that emulates a Lisp Machine, and the rest is done with magic.

We already mentioned cons cells and their reuse as pairs. This would be a dirty trick in any language but a Lisp, where it is placidly accepted. As you might imagine, cons cells are used to implement queues, stacks, graphs (cyclic or otherwise), and all the other things you can do with a struct that has two pointers.

Ever written any of those in C? It gets annoying.

The value field of a cons cell can and often does point to literal data, where the environment keeps track of type through various magic tricks. Just as often, it points to a symbol. What's a symbol? If you guessed an array of pointers, you are exactly correct. In a Lisp 2, such as Emacs, there are four pointers. A Lisp 1 would have three.

These pointers...are pointers. The first one will always have the data pointed to interpreted as a literal string, and the environment will only create one symbol per literal string. Emacs has a single global namespace. It's kind of cray, actually, but it makes sense in context. No one would ever rewrite the program like this, even with a great deal of sympathy to the underlying architecture. Which I have.

The remaining three pointers point the variable value, the function value, and the property list. That is, `foo` can be defined to point to...anything, or it can be defined to point to (again, effectively anything) in a function context, also, there's a slot you can fill with, you guessed it, anything. That has the 'properties' of the symbol, which is metadata, kind of, in practice. It's a box where the programmer can put arbitrary information, and heaven help you if you interfere with something the environment is using.

What this means is that symbols have totally bewildering behavior unless you know that they are four pointers being trifled with in various ways. Then it all makes sense, even if that doesn't solve the problem you're having.

Elisp, notoriously, has dynamic scoping of variables. Lexical scoping is more common and widely considered a better idea. Emacs has been hacked to provide it as an option, but this is janky and slow and you should never, ever turn it on.

The difference is usually explained in terms of the semantics, and for good reason. The semantics of dynamic scope are hard to explain and hard to reason about, while lexical scoping is easy. In lexical scoping, when you assign a function name, it applies within the code block you've created. So if you set `bar` in the `foo` function to be `42`, and the `foo` function calls `baz` which has `bar` set to `"beautiful"`, everything works the way the folks who wrote the `foo` and `baz` functions intended it to.

Dynamic scope, in contrast, is easy to explain in terms of the implementation. When you use e.g. `let` to bind a variable in Elisp, the symbol (remember there is only one) is pointed at a new value. So if you set `bar` in `foo` and then call `baz`, `bar` is worth precisely what `foo` says it is. Unless `baz` does a rebind, in which case the whole thing unwinds.

Combine this with a global namespace and you have a program with no concept of privacy at all. Written by RMS! The irony isn't so thick once you get to know the gent.

This is fantastic for punching holes in things, in the hands of a skilled operator. Let's say you're perusing some compiled source in Emacs, trying to figure out why the function doesn't do what you want. You realize that it's calling `ancient-set-foo` while all your code uses `new-set-foo` off el-get. You code up a wrapper, `let` `ancient-set-foo` to `new-set-foo` and call the function. Bam, you've changed behavior. Possibly in unexpected ways! It's a heady feeling.

If you're really feeling your oats, you can even globally re-bind `new-set-foo` and watch what happens. This might even result in improved behavior everywhere. More likely you are completely hosed if you haven't done a thorough perusal. Always remember, Emacs plays Calvinball, and if you don't touch all the bases you don't score the point.

I firmly believe that both namespacing and lexical scope provide benefits that outweigh the tradeoffs. They make it a bit harder to understand what the computer is actually doing, but lexical closures are one of my favorite abstractions.

I'd like a language that provides the benefits of both, because punching holes in code is awesome. One could add a `punch` special form that works like `let` but shamelessly tampers with the variable scoping of functions called within the `punch` environment. Or rather, `punch` works like `let` in Emacs, not `let` in Common Lisp. We'd use `let` for sensible things instead.

I suspect Common Lisp allows this, and I just don't understand how.

Combining the advantages of namespacing with a single global symbol table can also be done, I think. It's a topic for another time.
