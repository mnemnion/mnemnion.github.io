---
layout: post
title: "Those Dadgum Adders"
date: 2014-01-12 15:05
comments: true
categories: 
---

I read [a description of Forth](http://prog21.dadgum.com/33.html) years ago, that stuck with me.

Now that I'm writing some, let's try this problem out. The premise: given three arrays of three integers, add x and y, putting the result in z. In C, this is easy by design. 

Oh, and array overflows have probably cost us a trillion dollars by now. Kudos!

## Forth is factoring

First, no custom data types, but lets actually define and fill two 'arrays'.

```text
variable foo[] 2 cells allot
variable bar[] 2 cells allot
variable baz[] 2 cells allot

99 99 99 foo[] ! foo[] cell + ! foo[] 2 cells + !
2 4 8    bar[] ! bar[] cell + ! bar[] 2 cells + !
16 32 64 baz[] ! baz[] cell + ! baz[] 2 cells + !
```

This is wordy. Wordier than it needs to be. But also easy to read. Notably, we do not juggle the stack; it's somewhat bad form while interpreting. 

Clearly if we need a lot of arrays, we'll define something to handle them. Which kind of array do you need?

Today, like the problem, we need a C array: stupid, unbounded, and untyped. 

Now we need a word to add the values of three addresses together.

```text
: add-advance  \ ( a1 a2 a3 -- a1+ a2+ a3+ )
	\ "add a2 and a3, store value in a1. Advance all three addresses."
	dup>r -rot dup>r -rot dup>r -rot \ ( a1 a2 a3 -|- a1 a2 a3 )
	@ swap @ +                          \ ( a1 a2+a3 -|- a1 a2 a3 )
	swap !                              \ ( nil -|- a1 a2 a3      )
	r> r> r>					         \ ( a1 a2 a3    --        )
	cell + rot cell + rot cell + rot    \ ( a1+ a2+ a3+ --        )
	; 
```

Line by line, shall we?

`dup >r -rot dup >r -rot dup >r -rot` puts a copy of every address on the return stack for later. I'd call this `punt` if I planned to do it frequently. 

`@ swap @ +` takes the values from the two address and adds them together, while `swap !` saves the result to `a1`. 

`r> r> r>` is the anti-punt, basically. We could do the `cell +` call inline with it, but I feel this way is clearer: the `cell + rot cell + rot cell + rot` adds one cell width to each address, advancing it. 

So far so good. Let's make a word that creates a word that adds an array of n. 

Wait what?

```text
: n-adder \ ( create: n -> nil does>: 'add-advance' )
	create , 
	does>
	@ 0 do add-advance loop
	drop 2drop ;
3 n-adder 3-adder 
```

Looks like we solved the problem for, not array[3], but array[n], for any n. Of course, we have to make a new word each time. `here`, `allot` and `:noname` can be combined to the same thing in anonymous ways. This is strictly speaking over general; we add an entire extra line of code, and one additional word, in the pursuit of perfect generality. 

`:noname create , does> @ ( ... ) ; 3 swap execute 3-adder` would make our adder without providing further capability.  

Just as clearly, we could make a word that generates an array of n, and stashes n as the first value of that array. Then we could make a bounds checked version. It could even add two arrays of unequal length into an array equal or larger than the both, though we would need to define each behavior fairly carefully. 

Or we could make a creator word that takes any other word that acts on the values of three addresses, and applies that word to an array of arbitrary length. There's plenty of room in a 64 bit offset for a type system for the resulting array; we could end up with a `+` (pre|post|in) fixing (two|three) arrays of arbitrary type and length, if that was useful. Depends on the language you want.

The reason Forth doesn't provide the C array structure is because it doesn't presume to make that choice for you. Simple as that. 

### Footnote

Astute Forth heads might find this one weird: `\ ( a1 a2 a3 -|- a1 a2 a3 )`. That `-|-` is an annotation I call the mirror: the return stack runs backwards from the data stack: `a b c -- '>r >r' -> a -|- b c`.

It's easy to read, and clear. I use `--` to show that a stack annotation is completed and `->` to show a stack change, normally in a colon definition. It's so regular a computer could almost do it. 