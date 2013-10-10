---
layout: post
title: "The Urbit Has Landed"
date: 2013-10-05 10:41
comments: true
categories: 
---

At long last, [Urbit](http://urbit.org) has landed. The Internet is bewildered, flummoxed, and intrigued. 

I encountered Urbit (and [Moldbug](http://unqualified-reservations.blogspot.com)) when the original [Moron Lab](http://moronlab.blogspot.com/) posts dropped on HN. Unless it was through [LoperOS](http://loper-os.org), who incidentally might want to get his tail moving. Either way, I wasn't programming computers then, and I am now, and that's not a coincidence. 

See this one time, I had a vision. I'm sure my [dimethyltryptamine](http://en.wikipedia.org/wiki/Dimethyltryptamine) levels were elevated, since you can't have visions (or dream) without that being the case. It would be impossible to explain, and has proven very difficult to draw. But what it told me is that the Kabbalah, specifically the [Tree of Life](http://en.wikipedia.org/wiki/Sefer_Yetzirah), holds the key to a Kabbalistic Computer. 

So reading the Nock spec was a revelation, because you can map it to the Tree of Life. I haven't been able to suss out whether this was cgy's intention, he's cagey as a mockingbird. He's also a fellow Yid whose passion for the classics exceeds mine by a full order: it's improbable in the extreme that this is coincidence. 

This whole post will be steeped in Kabbalah. You think Hoon semantics are weird, watch out. At least I'm not making this stuff up. 

##A Grove of Pomegranates

Kabbalah, and Hermeticism in general, is not religion. Nor is it science. To call it philosophy is merely to acknowledge that it is too old and crufty to be more specifically typed. 

It has a non-trivial relationship to actual mathematics. By far the best modern work of Hermetics is Christopher Alexander's ["Nature of Order"](http://en.wikipedia.org/wiki/The_Nature_of_Order), though I haven't a clue if Mr. Alexander would consider it to be such a thing. 

I mean non-trivial exactly: Hermetics cannot be reduced to mathematics, nor vice versa. Where there is no mapping, one or several await discovery. 

Hermetics means "that which is proper to [Hermes](http://en.wikipedia.org/wiki/Hermes)", which should not be misunderstood as a religious sentiment. We might call engineering Vulcanics by the same trick (I consider interchangeable use of Latin and Greek to be strength, not sin); the Rod of Asklepios means medicine and implies no belief whatsoever in Olympian deity. 

You will find no Wikipedia article on Hermetics, though [Hermeticism](http://en.wikipedia.org/wiki/Hermeticism) is covered. We pick our isms carefully over at Unit of Analogy, and aren't signing up for this one. There is considerable sympathy. 

The Pardesh ha'Rimonim, the grove of pomegranates, is a common image in Kabbalah. Which means "the received". One will find a QBLH desk at every hotel in Israel. 

The Tree of Life (in orthodoxy) is an arrangement of the numbers 1 through 10. Each of these Sephirah (the name for number treated in this fashion) is fractal, containing an entire Tree of Life within it. Furthermore, 1 is 10, and there are four levels which repeat between the Ultimate and the Real. They are called Atziluth, Briah, Yetzirah and Assiah. The manifest is not even Assiah, it is merely our picture of Assiah.

Gibberish? No, [Jargon](http://en.wikipedia.org/wiki/Yiddish_words_used_in_English). You will find the identical scheme in Plato and in Tantric thought. That's likely to be cribbing, not parallel invention. From whom? Good question.

I will be using jargon, and worse, translating it on the fly into a mapping I completely made up. Can't be helped.

As a statement about ontology, let's set it aside for today. According to the [elves](http://en.wikipedia.org/wiki/Terence_McKenna), it's a diagram of the network layers. How do we go from the conceptual (we have an urge to calculate) to the realized (here is a machine which does so)? 

Any which way we want to, of course! What Would [Ari](http://en.wikipedia.org/wiki/Isaac_Luria) Do? 

###Lolwhut

Look, you already have a spaceship written in the language of horse-headed beasts which eat Yahoos. 

I'm taking it for granted that you can handle a little colorful metaphor. 

##Layer Zero

The zero layer is of course physics. Let's try to stick with things that work on that substrate.

##Layer One

This is Atziluth. It is our calculation represented literally, as a calculation. In a word, Nock. 

Nock is a work of praeternatural brilliance. I will save all critique for a later post. Let's pretend it's perfect, as indeed it is. 

Let's note, though, that to get Layer One running on Layer Zero (our chip) requires a bit of cheating from Layer Three. Can this be avoided? Perhaps.

##Layer Two

Layer Two is missing from the Urbit stack, or rather, it is conflated with Layer Three. I see how that happened. It's tempting. It may even be fixable from within the existing structure. 

Let me break down how, in ol' QBLH, something might travel down this ladder. There's some primal Thirst that is the same thirst no matter what experiences it, anywhere in Universe. So goes the premise. That's Atziluth. Briah is where an I forms an urge that is a personal thirst. Yetzirah is where this coalesces into an action, in Assiah the action is actually taken, and then the individual experiences a theatrical performance of the act of drinking water, assembled by his or her neural cortex. 

Is that last point at all unsettling to you, dear Reader? I should hope not. It is on the firmest of ground, however shaky the rest may be.

That's the metaphor for our network stack. The Briatic layer is specification of form. In perfection it would be purely declarative, and the form it should take is ancient and not open to debate: it is, should be, ultimately must be, a grammar.

Not a powerful set of gonads. A grammar. [GLL](https://github.com/epsil/gll) is totally a thing and for the first time we can make performant grammars that are as expressive as [Pāṇini](http://en.wikipedia.org/wiki/P%C4%81%E1%B9%87ini). 

Here's some Hoon: 

```
             %r
            =+  yug=(yell q.p.lot)
            =>  ^+(. .(rex ?~(f.yug rex ['.' (s-co f.yug)])))
            :-  '~'
            ?:  &(=(0 d.yug) =(0 m.yug) =(0 h.yug) =(0 s.yug))
              ['.' 's' '0' rex]
            =>  ^+(. ?:(=(0 s.yug) . .(rex ['.' 's' (a-co s.yug)])))
            =>  ^+(. ?:(=(0 m.yug) . .(rex ['.' 'm' (a-co m.yug)])))
            =>  ^+(. ?:(=(0 h.yug) . .(rex ['.' 'h' (a-co h.yug)])))
            =>  ^+(. ?:(=(0 d.yug) . .(rex ['.' 'd' (a-co d.yug)])))
            +.rex
          ==
```

This, I am told, specifies the syntax of a floating point number. Or some of it does.

This is from the JSON spec:

```
number
    int
    int frac
    int exp
    int frac exp 
int
    digit
    digit1-9 digits
    - digit
    - digit1-9 digits 
frac
    . digits
exp
    e digits
digits
    digit
    digit digits
e
    e
    e+
    e-
    E
    E+
    E-
```    

The former is executable, the latter is admirably clear. These advantages can be combined profitably. 

This is not even a critique of Hoon the language, because we haven't gotten to Level Three at all. This is an assertion that Hoon is poorly suited to specify any data format which may be expected to be used by anything but the Urbiverse. I consider that a deficit.

Thing is, I'm pretty sure those gonads can be whipped into a nice powerful GLL for parsing binary data. Hoon is not leading the pack as a choice for the first implementation; that's a simple matter of documentation, namely the lack of it. 

##Level Three

Level Three is the Executive layer, wherein we get to specify what we want our machine to do. Generally that's a programming language of some sort. 

Here the difference in approach becomes clear. From cgy's perspective, Urbit's Level Three is written in Hoon. From my perspective, Urbit's L3 is written in Hoon, C, Nock and Markdown. 

That is because humans execute, not machines. Machines don't rush to the wall and plug in when they're low on juice, at least not yet. For a calculation to happen, a person (I can only introduce you to human persons, but let's not be prejudiced) must know that they want to calculate, and must know how to do it.

I defy you to do anything at all with only Hoon. Without any reference to Urbit's Markdown files, or rather, their conveniently compiled HTML derivatives. Hell, I can't do anything with Hoon yet, even with existing documentation. That can only improve and is no criticism at all at this stage of the project. 

###Houyhnhnm

I could like Hoon. I want to like Hoon. I cannot seriously credit the idea of One Language to Rule Them All. If I could, it would not look even vaguely like either Perl or APL. No offense to the Admiral.

If I could credit the idea of the UrTongue, it would clearly need to be a format capable of usefully embedding any existing or contemplated programming languages, [cleanly and usefully](https://help.github.com/articles/github-flavored-markdown). 

I would strongly recommend to anyone considering designing a new language at the present time: The sequence ```` \n``` ```` is utterly reserved and I will come down on you like a ton of tiny Internet bricks if you tamper with that convention. I suspect the present dominance of github is sufficient motive to keep it real. 

This leads to a couple important questions: Will Hoon, presuming decent tutorials and documentation, prove a pleasant systems language? More specifically and urgently, will it be pleasurable to write parsers and compilers in? 

I am sold on one aspect of Hoon: as the Urbit core and bootstrap sequence. Why? Because it's there, brah. 

I think cgy gets this. The deal with Unix is clear: you can have any language you want, as long as it's C. I hope the same bargain between Urbit and Hoon will prove to be the upgrade we all want. 

Again, having a nice tight Layer Two spec would make this all the more likely. 

##Level Four

You can't execute without an environment, which is fundamentally about data in aggregate. That's Urbit, which is fantastic. The surface area is a set of rules on strings that produces a "directory" and you should just [read the docs](http://www.urbit.org/2013/08/22/Chapter-1-arvo.html) because they're pretty good. It's URL safe, which is nifty. 

I am cheerfully unclear on how any of that operates under the hood. I have notions of how it should work, but no way to contrast that to how it does work. It appears to work, in that pre-alpha-software way. I'd wager the problems we're seeing right now aren't design-level. 

This is the Assiah layer, which is the world you actually wander around in when you go get your drink of water. If anyone is still keeping track. 

###Okay. That was arcane. Your point?

Here's a helpful table:

<table border="1" bordercolor="#00000" style="background-color:" width="100%" cellpadding="3" cellspacing="3">
	<tr>
		<td><em>Kabbalah</em></td>
		<td><em>Urbit</em></td>
		<td><em>Arc</em></td>
	
	</tr>
	<tr>
		<td>Atziluth</td>
		<td>Nock</td>
		<td>AX</td>
	</tr>
	<tr>
		<td>Briah</td>
		<td>Gonads</td>
		<td>GGF</td>
	</tr>
	<tr>
		<td>Yezirah</td>
		<td>Hoon</td>
		<td>Marmalade</td>
	</tr>
	<tr>
		<td>Assiah</td>
		<td>Urbit</td>
		<td>ArcOS/Arcive</td>
	</tr>
	<tr>
		<td> 　 </td>
		<td>  　</td>
		<td>  　</td>
	</tr>
</table>
  

###Huh.

I'm sure that made everything much clearer!

The names on the right are referents without value in the present. Unless you count a bunch of Markdown and a partially specified grammar description language. From my modest perspective, that's code, since I can compile it; it does nothing but inform, and even in that capacity is not ready for public consumption. 

The Arc doesn't exist and needn't be written if Urbit will serve. By the very nature of the name, it's a huge, ballsy target. It's utterly vaporous, though I hope to release the first tool in the chain before the end of the year. That would be [Marmalade](http://mnemnion.github.io/blog/2013/08/03/a-tangled-web-we-weave/), the literate Markdown dialect. The first metacircular compiler of Marmalade is in Clojure, but the lovely thing about [Git Flavored Markdown](https://help.github.com/articles/github-flavored-markdown) is that one may embed any number of languages in it. Indeed that is rather the point. 

In following posts I'll go over the cake, layer by layer, with less attention to Urbit and more to a hand-rolled, idiosyncratic take on the same domain. I'll remind the Reader that there's no substitute for working code, which cgy haz and I haz not. 

In the meantime, Urbit is here, utterly fascinating, and on the verge of working. Come check out #urbit on freenode, and join in the madness. 


