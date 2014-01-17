---
layout: post
title: "There is no Garbage"
date: 2014-01-17 13:02
comments: true
categories: 
---

Garbage collection is another solution to a problem we don't have.

Here's my system: 4 cores, 8 GiB ram, 256 GiB flash store, 2 TiB off-deck backup. 64 GiB on the phone, 128 GiB on the tablet; I'm a packrat by nature. 

I have garbage collection why, again? Because no one has written software around this unbelievably roomy environment.

###"But... what if I have an arbitrary amount of data to process?"

What, like in a file?

####Sure...

Couldn't you allocate some memory for the file and copy it over?

####That's slow with a huge file.

No, **reading** it is slow with a huge file. Allocating it is free.

You could also, maybe, make the files a little smaller? The user never has to know, though it's polite to tell her if she asks.

####What if I run out?

What, of the huge chunks of imaginary allocated memory? Close a "file" by forgetting it ever happened.

####What if I changed something in the file's contents that I wanted to keep?

And you didn't make a copy to the store immediately? Are you five?

####I didn't know if it was a good change or not

...you're five. I said "copy", not "destructive update". 

####Okay hotshot, what if it's arbitrary data from a socket? 

Me, I'd be writing it to the store, hoss. Then, it's a file. Allocate more than you can store or process and start using it. 

Take notes. Store them. Perhaps even ask the other computer first how much data it's sending. This is legitimately not always an answerable question, and sensors provide numbers too. 

####What about huge transients from my enormous calculations?

...you were dynamically allocating huge transients in enormous calculations?

I can hear that server farm humming from here. You. Had. One. Job.

####But I just want an array, and I don't know how much to put in it, and I won't need it for long, and...

No, you are wrong about all of that. You want a proper functional shared memory data structure, to which two things are done:

They are created and

They are durably written to main store.

Unless they're, uh, huge transients. Y'know, like the fake Intel computer asm.js makes for you. Dynamically, wink wink, say no more. "wait, that's.. that's an array of...". That's no moon, friend. 


####Look, I have really complex, relational data relationships, that I have to query and then get results of variable size back.

That sounds like you want some kind of "base" for "data". Perhaps someone has written one, and it can handle your requirements.

Which sound kind of special. Perhaps you want a dedicated chunk of hardware? It could 'serve' you this data. Now, you have arbitrary data from a socket to allocate. 

There are two kinds of bases for data: those in which there is a type of request which always, without exception, returns the same data, every time, and those which are enormously useless and must be scrapped in favor of something you can use.

This is also true if you call your "data base" a "web site", as it happens.

If you have the former, you: request data be delivered in fixed chunks, process it, remember the *request*, save the *result*, and anything else may be immediately and comfortably forgotten.

#### ...lisp

I can't help you there, at the moment.

#### You are significantly underestimating the complexity of a modern operating system.

I have enough room in RAM to load a DVD and watch the entire thing.

#### You cannot eliminate garbage without abandoning Unix.

Happily, Unix now comes on Flash USB drives, with costs similar to a cheap night out wherever the hacker is found in native habitat. 

Fusing this to the motherboard is not a hard problem.