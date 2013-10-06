---
layout: post
title: "The Hundred Year Editor"
date: 2013-09-18 12:15
comments: true
published: false
categories:
---
If we know one thing about life a hundred years from now, it's that we'll all be programming our flying cars in [Arc](http://paulgraham.com/hundred.html). Today's question is, what editor will we, or our descendents, be using to write this mythic language?

Here's a prediction: professional corresponding to today's programmers will be working off a physical keyboard. It will be recognizable as a descendent of today's keyboards, and 90% or more of typists working with English or another European language will have asdf under their left fingers. Some things simply do not change quickly, if at all.

That's a guess, of course. It's possible that CAD or gaming will spawn some sort of chunky widgets for each hand that can be frobbed in ways which are so efficient as to obviate the keyboard + pointer scheme. I doubt it, simply because I'm routinely hitting two to five key combinations to trigger events in Emacs, and that's with 87 keys on my deck.

Keyboards for professionals, and likely for everyone, will have screens on each key. That's a gimme. This is a part of why I predict they'll continue: a chord device will always be opaque, whereas the always arbitrary complexity of input into a computer can be documented with icons on the keys of an open-plan keyboard.

In fact, I predict that the difference between casual and advanced users of computers will be whether or not they own and use such a keyboard. Tablets are self-evidently good enough for mere interaction, anything that isn't primarily a) writing text or b) issuing commands.

Enough groundwork: we're imagining a future where programming is still done with keyboards and screens. What else?

##Stringly Typed

First prediction: the hundred-year editor will not be based on any existing codebase, because it won't be a text editor. It will be an Editor, that is, a tool for editing information on a computer. Just as we are converging on a Browser as a tool for consuming and interacting with information, it stands to reason we will also converge on an Editor as a tool for editing and producing information of all sorts.

Second prediction: it will look more like Emacs than any other existing tool. Vim is an evolutionary dead end. It has found an admirable local maximum around efficient editing of strings, which is what a *text* editor does.

Emacs suffers from the same limitation. But it is real suffering; the nature of Emacs is to expand to fill all use cases, and the inability to work properly with anything but strings is constantly being kludged on with `image-mode` and the like. Vim does not suffer from this lack in the slightest: it is a text editor, period, and extensions from that base are largely envious and detract from the elegance of the tool.

Corrolary: the hundred year editor is written in Arc, and is a natural extension of having the hundred-year language. Clearly this language is not a collection of strings either; that would be perverse. The natural end point of REPLs, incremental compilation, etc. is a situation where text is an interface to object code, generated and manipulated on the fly.

The key to the whole shebang is an operating environment in which hashes serve the purpose of pointers. It is sad that we are still dealing with namespacing in our programming environments. To get rid of it requires some real foundational architecture, the kind of corrosive development like Unix or IP which will undermine any environment which tries to host or emulate it.

Plan 9 took the Unix premise, that everything is a file and either a string or a blob, and brought it to the logical conclusion. The next paradigm is that data is referenced by the hash of its value and typed through a function which may validate or reject it as being a member of some category. Any strings associated with these operations are for user convenience.

The reason this is corrosive like Unix is this: in Unixland data is `file://some.handle/that/is/long-and-arbitrary.in-nature/`. When it hits our world, it is hashed into something definite and stays that way. We may surely keep track of the various strings associated with a given lolcat, but it is so much metadata. If we embed said lolcat, it is embedded as a hash reference, like anything else.

The key liberating concept is not "everything is a hash", it is the idea of a universal textual interface to binary/arbitrary data. In Unixland, especially in Webville, absolutely everything is a combination of strings and the programs who love them. Smalltalk failed because everything was in essence a program, and if you weren't Smalltalk, good luck talking to it. Strings aren't like that. Strings are so friendly that for a first approximation they are all that our existing programs understand.

The difference in approach is intentionally subtle from a user perspective. We see something that looks very much like a string anytime we examine data. We produce most executable data by inputting strings, and if we send the program to someone else, they will also see it as a string. It is the computer which is no longer burdened with this formality, unless the information embodied by the strings themselves is useful to it.

This post is generated from a text file with a longish name. On my computer, that longish name, along with a reference to the area of the SSD that stores the data, is actually how the value of the text file is retrieved and manipulated. In our hundred-year editor, each 'save' of the text file is a distinct hash, and there is another unique hash that is arbitrarily selected to represent the identity. "file names", "URIs" etc are so many pseudonyms. Metadata and a few functions more advanced than mere hashing provide additional context, with the entire system living and breathing on the ability to make a literal hash of any arbirary data and relate it to other such references.
