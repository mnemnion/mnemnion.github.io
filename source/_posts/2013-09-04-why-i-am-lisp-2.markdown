---
layout: post
title: "Why I Am Lisp 2"
date: 2013-09-04 09:44
comments: true
published: true
categories:
---

First things first: Lisp 1 and Lisp 2 are the worst sort of jargon, because they mean exactly nothing without explanation. Renaming them is even worse.

I've done this kind of bad abstracting before, in public even. Be careful when naming things. Nouns are sticky.

For those who don't speak Boston Yiddish, Lisp 1 means that symbols and functions share a namespace. In Lisp 2, they do not. Simple as that.

Of the Big Four, Clojure and Scheme are Lisp 1. Common Lisp and Emacs Lisp are Lisp 2. Yes, elisp is at least as important as the other three.

Having worked at this point with three of the four, Scheme excepted, I prefer Lisp 2. One simple reason: a function is linguistically a verb. If we don't make a separate namespace for 'variable' uses of symbols, we can't have separate nouns and verbs. This is somewhat uncomfortable in practice.

Ultimately, I feel that this problem arises from a peculiarity of English: The standard way to verb a noun, in the third person (which is what is used for the imperative), involves no change whatsoever to the noun. If we have a list, we list it. There are hundreds of valid verbs which are letter-identical to a corresponding noun, and it is increasingly easy to create more. Thanks Joss Whedon! No, seriously, thanks, it fits English like a glove.

Contrast with a language like Spanish, where there are quite few verbs which cannot be letter distinguished from a noun. It's a shame computer language design is so heavily influenced by English. A Spanish programming language would be a pleasure to work with, and I can only begin to imagine the sophistication possible in an Arabic or Hebrew based programming language that makes clever use of the Semitic roots and mutations.

Meanwhile, back on this planet, we benefit from separate noun and verb slots in a symbol. `funcall` is a small price to pay.
