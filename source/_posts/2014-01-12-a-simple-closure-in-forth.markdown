---
layout: post
title: "A Simple Closure in Forth"
date: 2014-01-12 12:28
comments: true
categories: 
--- 
 
Forth is what you make out of it. If you want a structure or language artifact, and you understand what it does, you may create it.

Let's write a simple closure in Forth.

##Simple Closure 
 
```
\ Rollhex

: offset-hexpr \ ( offset n -- new-offset )
	 tuck                            \ ( n offset n -- )
	 hex 0 do                        \ ( n -- `hex`    )
	 	dup i + 16 mod               \ ( n n+i%16 --   )
	 	dup 15 <> if                 \ ( n n2     --   )
	 		0 <# # #> type           \ ( n -- "n2"     )
	 	else \ red F
	 		0 <# .#! # .#r #> type 	 \ ( n -- "n2"     )
	 	then
	 loop decimal					 \ ( n -- `decimal` ) 
	 + 16 mod						 \ ( new-offset --  )
	 ;

: hexer  \ ( C: nil -> nil D: nil -> nil "hex" )
	 	create \ ( nil -> nil )
	 		0 ,
	 	does>  \ ( nil -> nil )
	 	dup >r @ swap offset-hexpr r> ! ;

hexer rollhex 
```
  
 A call such as `2 rollhex` will produce `01` as output. `2 rollhex` again produces `23` and so on. We highlight each `F` in red. 

 This is a small utility, kind of like a measuring tape for a terminal. You can repeatedly fire it at a rectangle of text, and get a quick count for how many characters you've printed. There's no need for a reset word, you can call ` 0 ' rollhex ! ` or whatever new value you want the closure to have. Literally, this puts 0 on the stack, puts the address of rollhex on the stack, then stores 0 to the address, just like we did when we `create`d it.

 Could you embed this in some kind of "object"? Certainly, and you do so in the same fashion: by rolling the behavior you want, directly. If it gets moderately complex, you use access words. And so on. 

 This is dramatically over-commented Forth. Almost every comment is wrapped in an outer comment. The result looks weirdly like some kind of annotative type system operating in parallel with the Forth. In fact, it is, this is how Forth programmers keep track of programs. In their head, with a yellow legal notepad nearby. 

 You'll note the famous 'stack juggling'. Stack juggling is to Forth as the parenthesis is to Lisp. The issue with Forth is not on the left side of the source code; the issue is that the right side exists, at present, only in our minds. 