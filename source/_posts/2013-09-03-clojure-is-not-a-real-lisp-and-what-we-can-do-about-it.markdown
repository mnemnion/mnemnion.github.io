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

In Elisp, a list is a collection of data types called cons cells. A list like ` '(1 2 3) ` has four cells in it. The last cell is is an empty list, aka `nil`. So what happens if you have just two cons cells and no empty list?

You have an improper list, is what. Also called an association list. They get used kind of like key-value pairs, except Elisp and the other Lisps have maps too.

Clojure has what's called a Map Element. It's a key and a value. Maps are built up out of map elements. Lists are not; they are conceptually distinct. This shows clearly in Clojure's treatment of the empty list as well. 

An association list has a definite order, like any list. This is a natural consequence of the data type. Clojure provides the *syntax* of this type through sorted maps. The implementation details are entirely different. 

What happens if the last pointer in a list points back to the first element? Well, you have an infinite circular list, and if you try and read to the end of it, you'll hang. Clojure provides `cycle` as a function to turn any sequence into a potentially infinite circular structure. There's a lot of this kind of indirect correspondence involved in making Clojure what it is today. 