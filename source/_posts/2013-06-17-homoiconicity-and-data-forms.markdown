---
layout: post
title: "Homoiconicity and Data Forms"
date: 2013-06-17 14:52
comments: true
categories:
published: true
tags: Code, Clojure
---
###Representation of Data in Structured Programs.

Today we're going to discover a programming language. We're going to start by contemplating the idea of code as data. 

LISP, and by the all-caps I mean the original flavours, had two fundamental forms: atoms, and lists. As Lisp grew up, the lists became able to represent any sort of data type, but at the expense of a certain homoiconicity. 

That's a controversial assertion, but hear me out. A list in a Lisp is a bunch of cons cells, it's a specific data structure used by default to do pretty much anything. Since the first position (first second third) has a function or a macro, you can fake, say a hash, by saying something like (hash a a-prime b b-prime) but here's the problem: that's not homoiconic to your data anymore. Not in a way that accords with modern expectations. 

Let's talk about JSON. Now, JSON is homoiconic to your data. `{}`? Object. `[]`? List. `""`? String. `(1-9)(digits*)`? Number. And so on. 

<!-- more -->

What makes this homoiconic, and Lisp less so? Strictly, it's that by the time you reach the data, you know what type of data it is. Before you get to the value of a string, you see the `"`, before you get to an object, you see `{`, and so on. In paren-only Lisps, you see `(`, think "list", then see a "function", discover that it's actually "hash", and reparse the whole thing as a new data type. This has a cost that adds up over time. Also, the data type is closed exactly like a list, which it isn't, so finding the close character in a sea of  parentheses is genuinely hard -- though in Lisp, this matters less in practice than one might think.

CL heads will staunchly and indignantly deny all this, and they're probably right for them: there are reader macros, paren bashing is a totally valid way to close structured data, and so on. But it's just not how we'd do it now. So let's start with this JSON business and think through how we'd make a language from it.  

JSON is extracted from JavaScript, of course, so we could just add JavaScript back and call it a day. That's not the point of this exercise, the point of this exercise is to make a JSONian language from JSON. JSON is JavaScripty, but JavaScript is not JSONian. 

We'll add bare words back first. JSON supports only quoted strings, because JS uses bare symbols for all variables including functions, and JSON isn't supposed to be able to pass you executable code. But JSONian is all about executable code, so let's start by putting them back.

But put them where? Inside curly braces? Right now, `{ "foo" : "bar" }`is what we're doing with curly braces. In JS that's a special form, the more normal use of curlies is `{ statement; statement; statement; }`. Do we want to allow that? 

From the JSONian perspective, this would be confusing syntactic overloading. Also, parentheses are not used yet. So we'll do the Lispy thing, and use them for `(function arg arg arg)`. It's simple, and that's a virtue in a data representation format. Also, unlike Lisp, it will be much less overloaded, because we have `[list, list, list]` for lists. But let's call them vectors now, since we now have Lispy lists and don't want to get confused. 

Let's also wave a magic wand and get rid of some strictures we don't need. No commas between list elements, no colon between key : value pairs. They aren't strictly necessary, as list elements are already separated by whitespace and objects won't compile if they have an odd number of elements. Let's be nice and let you add commas wherever you want, if it helps you keep track of something. Our language will ignore them.

Furthermore, let's get rid of the silly strictures on numbers and just say that, to a first approximation, they behave like actual mathematical numbers. It might be nice to add imaginary/complex, but let's stay grounded: integers, rationals and reals. 

Are we done? We could be; this would be a fine language. But let's refine. One place we can improve: symbols need to resolve to something else or it's an error, while strings are opaque to the language, that is, the contents of a string is not meaningful to the compiler itself and we don't want to change that. A compiler won't know that "foo" isn't "bar" unless you compare them explicitly. Since we have a convenient colon left over from trimming the fat off our objects (let's just call them maps, since they are), we can define `:foo` and `:bar` as keywords, which always equal themselves. They're useful in our maps: `{:a a :b b}` could let us do something like `(:a {:a a :b b})`, where our keyword acts like a function and retrieves the `:a` value from the map. `("a" {:a a :b b})` should be an error, because it makes no sense to "literal string" something. 

I could keep being coy, but that's not the point: this is Clojure, and this is why I think it's phenomenal and am convinced that Clojure is the branch from which all future Lisps of importance and duration will grow. 

Being homoiconic to your primary data types is important, I dare say crucial. For LISP, that was atoms and lists, and for its descendants that are not Clojure, this fundamental duality is expressed in the syntax. 

For FORTH, that is atoms and words. FORTH is also good stuff, but pg isn't out there telling you to learn it. Maybe he should, we would get a lot of very reliable and hackable embedded systems in the bargain. 

But the verdict is in: certain data types are just fundamental and we like having syntax that reflects this. Consider `$$$ foo bar baz bux $$$`, where `$$$` is our separator so that we don't get any hints. 

Is that a list, such that adding some `qux` will give `$$$ qux foo bar baz bux $$$` ? Is it a vector, such that adding `qux` gives `$$$ foo bar baz bux qux $$$`?
Perhaps a map, where adding qux wouldn't make sense, but adding `$$$ qux quux $$$` would give `$$$ foo bar, baz bux, qux quux $$$`? Or is it a set, where adding qux would give `$$$ foo qux baz bux bar $$$` (as an example order) but adding another foo would do nothing?

In J. Random Lisp, this is easy: `(vec foo bar)` `(map foo bar)` `(set foo bar)`. Or wait, does map make a map or map a function over some values? Maybe it's hash, or wait, does hash create a SHA? Arse, where's my documentation? I think set dynamically binds argument one to argument two.... 

In Clojure, this is `(list foo)` `[vector foo]` `{:map foo}` `#{set foo}`. There are parenthetical forms of all of them, if necessary. 

Note that these are not type categories. If you need that kind of thing, there's Haskell. These are *form* categories. There are only so many ways to use linear order to represent data, and Clojure's set of those is, as far as I can determine, exhaustive. Since linear order is all we have as long as we're making our programs out of strings, we now have the right amount of expressive power, for my taste. 

