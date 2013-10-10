---
layout: post
title: "Executable Representation"
date: 2013-10-06 12:20
comments: true
categories: 
---

I find Urbit interesting for one simple reason: Nock is the most elegant expression of computation I have yet to encounter.

I am no kind of software engineer or computer scientist, at all. I am a professional developer, through a colorful maze of career turns. Curtis Yarvin is trolling when he claims to know no "PL Theory". I'm not. 

As an autodidact on most subjects (I can claim a major in chemistry and a minor in mathematics) my coverage is perforce spotty. McCarthy's thesis I get well enough; Alonzo Church and Haskell Curry make me nervous. 

You know who I do understand? Douglas Hofstadter. Came by that understanding the old fashioned way, by dropping out of highschool for a semester to read GEB:EGB and the Dragon Book, then dropping back in, then dropping acid. The order there is important. Get your diplomas, kids.

I also understand DNA, at least okay. I wrote a vector once, or truthfully, my professor kindly took the time to explain how vectors work in enough detail that writing the actual base pairs out was not entirely transcription. I've transfected them into plants, as well as wrapping them in tiny pellets of gold and blasting them into cells with high pressure helium, a technique that goes by the sobriquet "shotgun genetic engineering". Shotgun indeed. If any of that sounds exciting, may I recommend lathe work? It pays better.

So when I encounter Nock, what I see is Lisp, rewritten as a cellular automaton, capable of serving as a DNA for computational beasties. That somehow encodes the Tree of Life. 

Let's run with that and see where it takes us. 

##A Cellular Lisp Machine

Many people ask: what on Urth is Nock? Comparisons are made across many abstractions. All of them are interesting, and many are salient. To quote the current #urbit channel topic: "[jtauber](https://thoughtstreams.io/jtauber/nock/) oh man, the light just went off that [0 1], [1 b] and [2 b c] are just the I, K and S combinators". Indeed this may be the case, but I am in no position to judge it.

I luck out here because Nock is exactly on Urth a [cellular automaton](http://en.wikipedia.org/wiki/Cellular_automaton) and needn't be anything else, or make reference to anything else, to do what it does. A different von Neumann machine indeed! An Ulam/von Neumann machine.

I am quite convinced that Nock could not have been designed without reference to more-or-less every important formalism of computation in the literature. [Hangul](http://en.wikipedia.org/wiki/Hangul) could not have been designed without intimate knowledge of the [Hanzi](http://en.wikipedia.org/wiki/Hanzi), but knowing the latter is no help at all in using the former. 

Cellular automata are easy to understand if you've read GEB: Nock takes a string, applies rewrite rules in a definite order, and repeatedly reduces until it returns or loops forever. Looping forever can be trivial or not: detecting loops and deciding what to do about them is interpreter-specific. 

##The DNA of the Urb.

I happen to believe (on alternating Tuesdays) that DNA, as an abstraction, is something that just shakes out of reality when you get oily water to bubble-bath temperature for long enough. The exact base pairs, the resulting mappings, and the detailed chemistry are more than likely homebrewed for each planet. It would be nice to be wrong, but most likely the very basic proteins of other protein-based life would be allergens at best to our chemistry and vice versa. 

If you don't get me, consider that caffeine makes a perfectly good substitute for A in your DNA, and if you're a fan of the stuff like I am, your DNA contains a considerable amount of it. On some other planet, caffeine is the standard, and as a result, the alpha helix protein causes us to break out in terrible hives on skin contact. Adenosine-producing plants have a powerful narcotic effect on the native mammals, but their chlorophyll destroys our optic nerves. And so on. 

Indeed, we may imagine that early life on Earth had many nucleic acid gangs, locked in struggle, and that our chemistry was most toxic to theirs, and/or theirs least toxic to ours. Perhaps not; such losers have a way of going to ground and remaining in pockets. 

The Nock spec could serve as the base pairs of an entire kingdom of number, as DNA is the pillar of the kingdoms of life. That is unabashedly the goal. 

Yet number is not matter. There are critical differences. Here's counting in number: 0, 1, 2, ..., n or aleph depending on your taste. Here's part of counting in matter: 1, 2, ...., ~[92](http://en.wikipedia.org/wiki/Uranium). Important corrolary: 1.008, 4.002602, 6.94 map statistically to 1, 2, and 3 due to the fact that atoms are complicated. That is only the very beginning of it. 

The result is that molecules literally have aroma, flavor, texture, color. Numbers have none of those things without convention, and a lot of it. It is our task as programmers to show good taste in establishing these conventions.

##Executable Representation

Curtis Yarvin sells Nock as a VM, which I view as overstatement and undersell. Virtual machines do not merely calculate, they run. The only binary Nock interpreter is hinted and jet assisted: it may calculate Nock, but what it runs is Hoon. 

A cynic would say that what it runs is C. I say fie. When we are rewriting strings (these are mathy strings, not computery strings), recognizing a tedious reduction and supplying the result is proper cricket. 

What if you want to make something fast (call it Foon) and bypass Hoon? Well, you're kinda on your own as far as I can gather. The hinting mechanism is used, but is not documented as part of Nock. So write your own Nock interpreter that can run Foon, but it will only calculate Hoon. The HoonNock VM will be able to calculate Foon, but can only run Hoon.

Can this be mitigated? I think it can. One way would be to thoroughly document the hinting mechanism, allowing a compiler writer to target not just Nock, but the existing jets. Extra points for bug compatibility. 

##Not Nock

Nock is at 5K, though two cosmetic fixes have crept in at that level. I'd call that dodgy, except code temperatures this low have never been measured before. The thermometer cannot by definition be properly calibrated.

Ultimately, I feel Nock may be made to serve. Still, my brain runs at a few notches above room temperature, and it's interesting to think about how Nock could be changed.

Here's what I wouldn't touch a hair on:

```
A noun is an atom or a cell.  An atom is any natural number.  
A cell is any ordered pair of nouns.
```

A righteous man might say "Amen" here. I'll say right on. 

I would like to draw the Reader's attention to the second sentence. It contains the fateful words "natural number". The camel pokes her nose into the tent. 

So normally when we make formal systems that rewrite strings, we use symbols. See Lisp and GEB for the two cases that matter most here, and check out Game of Life again for extra credit: it's not just Turing complete, but demonstrably self-hosting. 

Oh hey, if you want to really impress me, design a Game of Life emulator in Game of Life and then demonstrate Godel incompleteness with it. 

Natural numbers, though. A fine choice that shows excellent taste. Not just any set of symbols! **The** set of symbols. Except it's not a [set](http://en.wikipedia.org/wiki/Abstract_algebra), is it. Indeed it is not: behold the face of the camel. She smells something she likes. It's warm in here. 

We are treated to a punky, fresh set of operators / rewrite rules that let us compute to our computer's content. Only basic arithmetic escapes cgy's benign and giving nature here. We are given increment as a warmup, and it would seem entire Dukedoms were awarded on the basis of decrement. I wonder if I could still score a cruiser for multiply. Do I get more points for O(n^4), or less?

My goodness this beast has a lot of neck. Here, let me move the hookah. In providing only increment I see the logic, but not the wisdom. Worse, I don't see the sense. It was magnaminous to provide the macros, though it may have been done merely to answer the calls of "you cannot possibly use that faster than Brainfuck" that would have followed such a skeletal machine. 

Thing is, when someone says 'natural numbers', they have promised a perfectly well-defined subset of operations, which Nock distains to provide. Hoon can do math; Nock just looks at you and says "i++;" a lot. 

Decrement is explained with much fanfare. Indeed most of my actual applicative understanding of Nock comes from that explanation. It's a simple case, because there's only one bad value to decrement, namely zero. It's also fairly obvious what to do: crash. 

Add, multiply, modulus, and remainder are entirely specified under the two words "natural number" and it is camelnine not to include them. Look, let me show this creature out: she'll start spitting or worse, and I like this rug. A modulus 0? Crash or fisticuffs, sir. 

Consider line 7. It plainly says "By add, we mean a restricted subset of addition, namely, addition by one". A little coda saying "oh yeah actually adding stuff in the general case of two arguments? 14" would not instantly bloat our automaton into JVM proportions. 

Subtract, though. I could see that going two ways for bad input. One would be to crash; it's the natural thing to do. Another option would be to return a cell with 1 at the head and the result of subtracting b from a rather than a from b at the foot. We might call it a negative number, or a useful error message. It's plausible is it not? Perhaps my Nock machine could jet assist this case and yours could absolutely plod. Clearly code allergy could be the only result. 

We do not want code allergy at the level of arithmetic. The only option given the Nock spec is to blindly follow the Hoon spec. I detect an abstraction leak.

Where on Urth could we put these extra operators? 

The back of the T shirt, of course. There should be plenty of room under the silkscreened all-seeing Eye.

##Call it Ax

Nock is unlikely to change. It's clear that Curtis Yarvin has thought this through, carefully, and simply come to a different conclusion from my own. I don't expect he will say "By Zod, your camel is completely persuasive" and go rewrite everything. 

If I had my own toy Kabbalistic computer, which I do not, it would be called Ax. Ax would be a cellular automaton that has the same preamble as Nock, and a similar structure. There are some good reasons for it to be operator-compatible with Nock, but not many, since it would hit the humble minus of Hoon and rip through each loop unless propped up with some kind of unholy JITing. 

So let's cut the umbilicus and call this automaton Ax. It has two lines of specification. The rest of this post will not add any. Sorry, yo.

Ax would have a longer spec than Nock. That's okay; Euclid is also longer than Nock, and it's pretty good. Making it shorter wouldn't make it better, though I'm sure there are some steps which we consider repetitive today. That's because it was written for doing geometry, not just for proving it. 

Ax would also lack an operator which Nock has, namely 10. Clearly, it may be reconstructed, as it's but a macro. It won't be a hint, though, because hints make me nervous. Jets don't, I emphatically agree with jet propulsion (though not of * for pity's sake), but hints. I believe I have a scheme for organizing Ax code so that hints aren't needed or even useful. This post is going to be a two parter.

I do have a question about 10 that is illustrative. It is said to expand thus: `*[a 10 [b c] d]   *[a 8 c 7 [0 3] d]`. The question is: if one were to open up some Hoon-compiled nock, and replace a 10 with the expansion, would the jet fire?

This seems like the sort of question which cannot be settled by reference to the Nock spec. While the Hoon source resolves it in a sense, that is not fully satisfactory either. 

I'm mulling over making 10 a `put` operator, that discards an atom or a calculation, with the semantics "something else might do something with this value in an implementation-dependent way". Such a tool is drenched in Midichlorians and could be used for great evil or great evil masquerading as good intentions. I'm not opposed necessarily, but I am skeptical of my own urges here.

That would be similar to allowing a calculation to slot on 0 and for any old thing to be in 0, even fairy dust. Gives me shivers just thinking about it.

##A Blizzard of Cores

Having a magic 0 address and the ability to mysteriously `put` data somewhere else isn't just provocative fashion sense. It's a loose mapping to the structure of OpenCL, with which we might Use the Cores. I want to Use the Cores, but if your code has pointers, I can't do that. 

We abandon any pretense of having a deterministic machine with such a move. `put` may be made well defined at a higher level, but a magic address is magic, and we are undone. That's okay. I was going to do that anyway. 

##Coloured Bits

Ax would differ from Nock in one key dimension, which I doubt can be resolved. Nock is [Colour blind](http://ansuz.sooke.bc.ca/entry/23), while Ax will be able to detect the Colour of your bits. Clearly to be automatically compliant with law across all juridictions and boundaries, a legal calculation must be able to detect, say, that the input belongs to Columbia Studios, the output belongs to a home theatre, and the combination is not allowed. 

Ahem. Colour at such a level is a Layer Four / Urbit concern which Nock nor Ax could possibly handle. There's a gem buried in that link: namely, that randomly generated numbers have a Colour which we must respect if we want to work with them. Which, y'know, we might.

One of the powerful ideas of Urbit seems is that, instead of Facebook and Google, there can be *my* facebook and *my* search engine. What Facebook and Google are supposed to do, respectively, is guess which content from my friends I'd like to see and guess what I'm looking for based on a search string. Modulo ads, they succeed reasonably well. Modulo ads, there's less and less left.

It would be nice if taking a guess was easy, cheap, and moronically simple. Simple like XORing an atom against an always-available pool of entropy and memoizing the result with the atom, thus preserving both the entropy and the computation in the same amount of space it would take to preserve the computation's inputs. That can of course be shimmied in at a higher level on a deterministic machine. Such a shim would be expensive. I want guesswork to be really, really cheap. 

So Ax has three Colours of bit: black, white, and red. Red bits are resolved when needed from a presumed infinite pool of high-quality entropy. It has an operator, `fuzz`, which opens an enormous can of worms. What happens when you XOR entropy across 27? 

Mu, is what. You can't XOR something that isn't binary. Nock deliberately and throughly rejects bits in favor of natural numbers, and drives the point home by describing them in decimal. Ax doesn't get this luxury. Longer spec. The rule is probably "take as many low bytes from the atom as specified by the byte width of the entropy call" or something similarly convoluted. 

`fuzz` should come before the macros, as it cannot be defined as one. There is no point in operator-level compatibility with Nock. It would be far easier to retarget Hoon at Ax, or more likely, virtualize the NockHoon VM on top of Ax directly. Like I said, two part post. 

Any function which has `fuzz` in it or which contains a `slot` on the 0 address is not deterministic. Otherwise, it is. The intention is to build some higher level conventions on top of that which allow for flexible communication with running Ax code (infinite loops are not all errors) and for building whatever sort of reasoning engine one might want. Assuming a limitless pool of entropy is a [safe bet](http://en.wikipedia.org/wiki/Comparison_of_hardware_random_number_generators) for future systems, and cheap to provide for current ones. 

We can let you cheat and use pseudorandom Pink bits, if you're willing to assume the consequences. Which for ordinary guesswork are presumably not dire. 

##Other Esoterica

Here's a question: if you took Nock, and permuted the operators, so that 4 is say 7 and so on, would it work?

Provisionally, my answer is "yes". That is, one could target that virtual machine and perform general calculations and nothing would be Wrong. I've talked this one over with [jtauber](https://github.com/jtauber/pynock/blob/master/nock.md), who really gets this Nock business.

Let's call the permuted Nock machine Conk. 

Second question: could you statically recompile Nock code into Conk code by permuting the operators in the same way and have it run on the new machine?

Clearly not. Nock can calculate a number and then use it as an operator, and this calculation would fail on Conk machine. Two increments on 0 can return 2, and then 2 can `*` on something; that doesn't work if `*` is 6.

Could one use some kind of reader table to change the meanings of operators when encountered, so that the permutation machine could be used, by swapping one layer for another, as a Nock machine? I believe so, I can't come up with a reason it wouldn't work. One needs a dispatch table, why not several? The hard part of running Nock on Conk is not dispatching the operators, it is sensibly firing jets when necessary. 

##On Beyond Hinting

This post will have a part two at some point. That's a discussion of a scheme for compressing Ax/Nock code in a way that should let us dispense with hinting altogether. It's half-baked and frozen, and it may take some time before I can pull it out and brown it. 

A better use of time is going to be finishing off a usable alpha of [Marmalade](http://mnemnion.github.io/blog/2013/08/03/a-tangled-web-we-weave/). Marmalade can be used directly to knock down the conceptual burden of writing and understanding the rest of the software. I consider an Ax/Nock machine running over OpenCL to be step three. 

Step two could be a fun blog post, but there's a rule of thumb: if you emit too much vapor, it's a safe bet some is coming out of your ass. Playing with Urbit is more rewarding. Or will be when ~doznec is back online...