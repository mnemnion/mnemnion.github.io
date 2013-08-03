---
layout: post
title: "The Semantic Tower"
date: 2013-07-30 13:01
comments: true
categories: 
published: false
---

We do different things with data, and there are in practice a finite number of ways to treat it. 

We may be able to define a pyramid of semantic behaviors surrounding data, with relatively clear separation of purpose. I intend to try.

The inspiration is the Numeric Tower common to Lisp languages. These take a single representation of number (binary) and use it to progressively allow for whole numbers, integers, rationals, reals, and often complex and higher order numbers as well. 

We need to take a single representation of number and use it to progressively allow for everything. Piece of cake, we do it all the time. 

##Layer One: The Literal (Atziluth)

The base layer is the literal: numbers considered as numbers. Conceptually, it's cleanest to think of these as whole numbers, from zero to as large as we need. The underlying representation must be binary, which provides clarity only to the computer at the expense of human readers. 

Any data at all can and must be represented literally, as a single large number. Per [Nock](https://github.com/cgyarvin/urbit/blob/master/doc/book/1-nock.markdown) we refer to these as atoms. 

A few notes about atoms: for a human to do anything with them, they must be mapped to sensory data in some fashion, and fed to a machine capable of physically creating those sensoria. A monitor or speaker, for example. Computers have no such limitation between them.

For a computer to do anything with them requires at least two and probably all three of our remaining layers. 

It is relatively easy to map any atom to another atom of a fixed size, in a way that in practice allows us to compare the fixed-size atoms and determine the equality of the original atoms. This is what a cryptographic hash function does. We needn't be particularly paranoid about hash collision and the attacks it makes possible, since we don't use hashing to validate data, only to ask for it. Hash + checksum should keep serving our purposes even if the hashing algorithm becomes meaningfully compromised: revealing information is always a more forgiving task than concealing it. 

Less straightforward is hashing two atoms `a` and `b` such that, by 'adding' `hash(a) + hash(b)`, one gets the same hash as `hash(a + b)`. Adding is in quotes because this must be non-Abelian, since concatenating `a + b` does not equal concatenating `b + a`. This lets us compose and decompose hashes of hashes, equivalent to atoms of atoms. 

It can be done, but appears to provide weaker guarantees against collision and is simply the wrong algorithm for passwords, the other main use for a hash. The reason it sucks for passwords is clear: even if it's a good hash, once one has hashed, say, a bunch of likely trigraphs, one can rapidly generate six-character passwords by 'adding' the generated hashes. 

Even for identity, the two approaches are stronger together than apart. If we can provide 'pretty good' collision resistance in the composition, and require any atom to carry its identity hash, we can resolve collisions and injections against the identity hash. The aggregate hash will come up again at our top layer. 

This is the main thing one can do with literal atoms: test them for equality in two ways: `cat (a,b) != ab` and `cat(a,b) = ab`. 

We call this literal layer Layer One. We also call it Atziluth, but not yet in polite company.

It is not Layer Zero, because that distinction belongs to physical reality, or some abstract mathematical Ground of Being. I consider these the same being, but take your pick; either way, you may agree our numbers need somewhere to live. 

##Layer Two: The Formal (Briah)

The literal layer is absolute, and spectacularly useless. All we have are whole numbers. We can write them down, do arithmetic etc, and eventually reinvent the whole of algebra if we'd like. Any of that would be enacting the higher layers on our own personal computer, the neural system. To bake these notions into silicon, we need to do analogous things, digitally. 

The first form we need is a form for our atoms themselves. There are options, which we perforce embrace. Representing whole values in binary hardware may be cautiously judged a solved problem. 

There is a bit of a strange loop between Layer Two and Layer Three: forms are useless without activities, which must be written in order for a form to exist on physical hardware. Separating the concerns of Layer Two and Layer Three is seldom done, never cleanly, and is necessary to support a Layer Four. 

Perhaps not "never cleanly". IP seems to do a fine job. Layer Two is unquestionably lower than Three, since in order to have an execution semantics one needs a specification of a format, and for that, we may only use language and its mathematical subset. Nothing else is available, nor can anything else do the job, since humans, ultimately, create implementations. 

We may want computers to create implementations, down the road. It would be best to have really, really good specification languages for that task. 

Most of our formats revolve around a couple of assumptions. One is that storage is expensive and memory is precious. Another is that data is generic, that there is no point in encoding identity in a basic way, and consequently that data can be expected to be redundant and expensive to compare. The former is no longer true, and kicks out the only weak leg the latter had to stand on. 

We can, and should, dive right down this rabbit hole. [Content Addressable Memory](http://en.wikipedia.org/wiki/Content-addressable_memory) can be expected to be a standard part of at least the server components of the Arcitecture. Given that a perfect world needs exactly three copies of any arbitrarily large data set, given that we can decompose to be efficient with small differences, and given that we can create and destroy more copies at leisure, storage is indeed cheap, and memory is largely cached storage. 

Forms in Araka are defined quite precisely as the set of all data which may pass through a particular formula. A formula is a function to validate forms; I trust these are not murky waters. That's the strange loop in action: in order to be general about our forms, we must be quite exacting about our execution semantics. 

##Layer Three: The Executive (Yetzirah)

Clearly, after settling on a way to represent whole numbers, our next task is to define semantics by which we may perform computation. Let's pretend we're using [Nock](https://github.com/cgyarvin/urbit/blob/master/doc/book/1-nock.markdown). It's quite good enough; he keeps losing me when he gets to [Hoon](https://github.com/cgyarvin/urbit/blob/master/doc/book/2-syntax.markdown) or I might not even be writing this. 

This is not entirely Martian exegesis, however. Or at least, I don't believe so. It's hard to tell sometimes. 

Everything thus far is recognizably Martian, however, including the next step: empowered with a way to write down computations, we can define functions which will pass/fail particular atoms as types of a given form. 

Hoon is a fascinating way to do this: it's the wackiest macro system imaginable, basically, hacking the equivalence between ASCII and bytes to within an inch of utter ruin. 

This is cool stuff, ASCII has swagger and C heads have long enjoyed punning around with it. It's Unicode that ruins the party: for an untyped blob of anything-goes, ASCII (along with its compatible, more sophisticate cousin Latin-1) should be carved into our DNA as the go-to choice. 

##And back to Layer Two

Strange loops are scary, being as they require our active participation at present. It is tempting to allow only one such loop, which appears to be the philosophy behind Hoon. The problem as I see it is that is the wrong such loop to make canonical.

Linguists are very comfortable at Level Two, since Level Three is done by speaking and listening, rather than by software in the usual sense. They write, publish, and peer review intricate grammars, all without the apparent need to have a specified execution semantics for the grammar. 

Over in Computerland, we do the same thing. It is our only recourse when our execution semantics fail us, for whatever reason. A binary encoded blob with finite meanings for each fully-specified bit field will still have a specification saying "only these four values are allowed, and they're supposed to mean this", or we will roundly curse our ancestors on that project. 

Sometimes that specification is only code. This is usually considered a bad thing, and I'm not buying Hoon as a dodge around that roadblock. 

More Nock: the precision of Nock is the precision of mathematics, and cannot be made more regular. It can be specified as grammar production rules, without losing that precision. An ASCII format for specifying grammars could be frozen to zero Kelvin and be expected to work, given that Panini's format from 2500 years ago could specify C right now. 

Therefore, we specify a Really Good Grammar format. [Instaparse](https://github.com/Engelberg/instaparse) is a pointer to what's possible, though entirely unsuitable as it parses Strings(tm). 

Hoon is not a Really Good Grammar because I, as a linguist, cannot look at it and say "yes, that is an Excellent Grammar" nor do I expect it will magically grow these wings. 

Let me stress a point: grammars do not have to be executed, it is perfectly possible and expected that humans will frequently step through them ourselves, complete with getting things wrong and trying various chunks with software support. 

A Really Good Grammar format would have the following, each a proper superset of the last: a literal recognizer, a regular pattern recognizer (tokenizer), a PEG format, a CFG format, and a format for specifying a rule as a function which passes or fails. Nock can be specified in the first two or three rings, depending on where recursion is allowed. GLL is a straightforward algorithm with much potential for parallel execution which can give us all the functionality required. RGG format will not require it or any other implementation in particular: it will require that ambigous grammars be capable of returning all parse options on command, treating any successful parse as a pass. 

The first implementation should be written in something fast, dirty, and easy to read. That suggests a Scheme such as Racket: we want a contagious and small binary that can be aimed at many targets. That's not Clojure, though it may be Common Lisp. I don't care. I'd like to see it in Hoon and Haskell too; many implementations reduce the surface area of error like nothing else.

We deliberately allow only literals, regulars, and PEGs for the bottom layer. This compels disambiguity and guarantees halting, two properties which are desirable for low-level specificaiton.  

The astute reader will note that [Really Good Grammar Format](/blog/2013/07/30/generalized-generative-grammar/) will be ultimately specified in Really Good Grammar Format. Nock cannot lick its own lollipop in quite so outrageous a fashion: we can get away with this because we are much, much better at grammar than we are at doing weird arithmetic. 

Computers are equally capable of both, because it's all weird arithmetic to them. 

A note on encoding: I strongly believe that one byte is exactly enough to specify a standard of such importance as RGG. 127 code points is kind of light, though. Latin-1 is exactly as well defined as ASCII, in practice well supported: Emacs, Sublime/TM, Vim have no problem editing Latin-1, and I consider that conclusive. 

Therefore, GGG format will be *specified* in Latin-1. It may be *implemented* in Unicode, and perhaps even should be: the code point mapping is a trivial one time cost, and any engine worth having should take Latin-1 or any of the Unicodes with equal tranquility. 



