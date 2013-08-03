Hello, World!---
layout: post
title: "Introduction to the Architecture"
date: 2013-07-27 12:20
comments: true
published: false
categories: Code, Arc
---
##Notes on a Sensible Environment for Computation. 

The goal is, superficially, a simple one. I wish to be able to turn on my computer, and reset it to one year ago. I can then move around inside that space and do new things, like fetch information from one year ago, rerun programs exactly as they ran then, and the like. 

That's not the whole goal. That's a taste of the goal. It suffices to start the conversation.

My name for this project is the Arc. [Paul Graham](http://www.paulgraham.com/arc.html) will just have to get used to that; the concept is not his, and the word is too ancient and noble to languish as an experimental dalliance. To differentiate, we may call it Araka, and I will use the two interchangeably. There is no difference, any more than LISP and Lisp are two canonically different things. Vowels simply aren't that important unless they begin a word. Nor does the distinction between c and k matter, when they are sounded the same. 

This is another taste of the goal. The Arc is Architecture, not Environment. 

Hi. I'm your host, mnemnion. My day job is to make heterogenous computing environments of multi-core CPU and GPU architectures do computation as fast as possible. When I say [John von Neumann](http://en.wikipedia.org/wiki/Von_Neumann_architecture) is fired, I mean it.

For another taste of the goal, I present [LoperOS](http://www.loper-os.org/?p=284). Loper would appear to be hard at work on his own version of the goal. I eagerly await. 

The Seven Laws of Sane Personal Computing are [Asimovian](http://en.wikipedia.org/wiki/Three_Laws_of_Robotics) in scope and breadth. Certain clauses, and the soul of Rule IV, are sociopolitical in nature: Show me a readable program and a compiler for it, and I will show you complete gibberish that does the same thing. This should not keep us from doing our level best, and is no excuse at all for failing at the purely technical points. 

Another player in the game is [C. Guy Yarvin](https://github.com/cgyarvin/urbit), whose Urbit is a credible attempt at the whole shebang that you can actually go and marvel at right now. It's written in Martian and there is no dictionary, the Rosetta is in C and hard going, and [no, I am not exaggerating](http://moronlab.blogspot.com/). The goal appears to be to write the damn thing rather than, I dunno, blogging about it. Laudable; I eagerly await. 

What with the aforementioned day job, when I write code on my own time, [I go light](https://github.com/mnemnion/emojure). Whimsical, even. What I'm going for with these posts is an exploration of the [patterns](http://en.wikipedia.org/wiki/The_Nature_of_Order) that will eventually make up the Architecture. 

An aside: if you haven't read *The Nature of Order* you've missed what Alexander has to offer. *A Pattern Language*, while a brilliant gem, is not the Magnum Opus. 

##Arc and Arcism

The Arc is part of a larger project, the arcist tendency. The Free Software Movement, which is the only technocratic political movement of any significance thus far, is thoroughly rooted in International Socialism. I say that with all affection, and hope RMS would agree, though he may prefer to say "progressive". I'm all about [emic](http://en.wikipedia.org/wiki/Emic_and_etic) vocabulary; progressive it is. 

The Arc is in this same sense a vehicle for arcism. There is no sense in which the Arc can succeed on its own, as will become clear enough as we explore. As arcism does not rely on controlling the State (though flourishing under such conditions), a modest success at forming Arcist society will suffice for modest success in the Architecture. Modest success is sufficient to the task at hand. 

As it happens, I'm making both the Arc and arcism up as I go along. I'll do what I can to make them separate posts. 

To illustrate the difference of approach, we may consider licenses. An arcist considers contracts which cannot be vigorously enforced to be suspect, akin to a pact with a demon, and a weak-willed one at that. The GPL, with its sticky viral influence and lax enforcement in practice, is a source of great amusement.

The arcist recognizes several states code can be in: unavailable, for example, or encrypted with a key that is believed secure at present, or in the public domain. We favor the public domain for anything interesting: it is the nature of software to be most broadly useful if the source code is readily available. 

We may also suggest an old-fashioned way to get paid for all this open-source code: guild up and shake the rest of society down for our share of the loot. We are open to other suggestions that are sensible and reality-respecting. 

There are... very few servants of the Emperor at present. The Arc is made of purest vapor. Let us proceed to condense, and see what may trickle into our flask.

##Rationale

Two questions worth asking: Why do this Great Work of reimplementing All the Things? Granted that it's worth doing, what am I bringing to the table? 

I stole Arc because naming things is the Second Hard Problem, and arc- solves it. The Arc runs on the Arcitecture, a physical platform that runs ArcOS. Information travels across the Network (yep, same Network) and is cooled and stored via the Arcive protocol. The system language is Araka, or Arc if you prefer. 

This intensive focus on the sensory is characteristic of the entire project. Arc, as pronounced by Americans, is a hard word for much of the rest of the world. `a(r|l)əkə` can be broadly realized by essentially everyone. 

I am a representation nerd. Ask me about the connection between astrology and abstract algebra sometime. No, really, ask me this as often as possible, because I'm not good enough at explaining it yet to write it down. 

This drive led me to develop Phon, a writing system, featural in nature, with the same scope as the International Phonetic Alphabet. Phon deserves its own separate series of blog posts, which really should be published before this one. You can write it in all four canonical directions with minimal confusion. That took some doing; the manual runs 180 pages at present, in trade paper format. It should take maybe two weeks to teach a literate ten year old to write her own language, in her preferred direction. 

Phon lies at the intersection of linguistics, representation theory, abstract algebra, Hermetics, and Tolkien studies. The astute reader may notice that I have a knack for naming things. Cache validation is less my style, and I have been known to be off by one, from time to time. A youth misspent with Pascal will do that to ya. 

One may at least say that I'm coming at the project from a distinctive angle. I believe that reaching the goal begins with representation, and am confident in saying that I'm the only person on the planet, right now, who has read the books I consider critical.<sup>[[citation needed]]()</sup>

Meanwhile, I haven't read all of *your* critical books, dear Reader. Suggestions are welcome. 

##Representation

It's all zeros and ones, kids. Zeros and ones. Everything else is metaphor.

Our metaphor is dominated by typography. You could cast the entirety of Unicode in lead, bring it back 300 years, and induce quivering orgasm in the printers of the day. It would be quite the heavy box. 

Code is textual for a reason, tamper with that at your peril. But serious face, folks: with 64 bits of Unicode, largely barren, what earthly sense does it make to represent **executable code** with the same characters you kiss your mother with?

Nope, we keep reusing those bottom bytes like a bunch of monkeys. UTF-16 is still out there, Emoji is a mess of shims and image embeds, and this is not the worst of it. The code points mean only one thing: a name, and a shape that hopefully can be recognized as the same across fonts that goes with the name. 💩, PILE OF POO, Unicode: U+1F4A9 (U+D83D U+DCA9), UTF-8: F0 9F 92 A9.

Do you see the Poo, dear Reader? Try Firefox.

We can only burn the fields and start anew. Somewhere in a dusty corner of our Representation Format, we shall find the Unicodes, along with the all-important canonical translation matrices which allow archaeologists to sensibly render the fragments of the past and make them actually useful again. 

MIME is a bit closer to the point, while missing the target entirely. MIME is, from our perspective, just a bunch of bullshit tacked on to the beginning of a file that may or may not help you figure out what the fuck to do with it. That it works well enough in practice is a miracle of discipline. 

A useful header would say "the following content is of type Foo. Its identity may be hashed as `Q222a27db79ac39dd6ba2fc1901d6b69c`, and the content may be validated for type using a function whose hash is `222a27db79ac39dd6ba2fc1901d6b69c`". 

Entropy being what it is, white noise can be encoded into any data-only format. We cannot expect to assure that an mp3 contains music, nor that a Unicode string can be meaningfully displayed at all, there being many code points that are set aside for private use. 

We can validate that Javascript is Javascript, however, by loading it into an interpreter without errors. How much confidence this gains you varies by language. 

This is going backwards, however. JSON can be validated for type and could be usefully transmitted with a header as described. It would be a modest improvement. It's still Unicode, which means any content it provides is buried in some implementation somewhere and subject to rot. The fact that a human could read it and partially decode it is cold comfort. 

Part of the goal is to allow archaeologists a fighting chance at decoding 500 year old partially degraded thumb drives. We have, maybe, one shot at this, before Unicode takes on the strength of DNA. 

##Why we care

There are only two formats, text and binary. Everything else is tacked on. Here's a number: 42. It's represented as Unicode, and if I represented it as, say, an integer, it couldn't coexist in this Markdown file. I would have to put it somewhere else or do something hacky like Base64 encode it. 

If it's binary, it's either implementing some spec or it's homebrew. MIME may help you figure out the former, or it may not. There is nothing even vaguely resembling consistency or regularity anywhere in binary land, and the relationship between the textual world and the blobosphere is uneasy. 

Where text is concerned, ASCII at least had the virtue of being somewhat narrowly defined in terms of (mostly) glyphs with distinct shapes that most humans can tell apart. Unicode offers no such promise. 

At some point, we're going to be hashing everything and chunking it out onto the network. Data has to represent the same way, in part and in aggregate, for this to work. The only possible shape is a sort tree, so that's what we'll end up using. 

##Section

What we need is a single format that can resolve all conceivable data and code. That's not as whacky as it sounds: we have hundreds of equally bad formats for doing this already. Most of them are ASCII or Unicode, of which my opinion is clear. This is computation we're talking about; it's all zeros and ones, kids. 

Here are some of the patterns which constrain this format:

* Data types are defined in a single fashion, that doesn't differentiate in principle between standard and extended types. 

* Data is composable, a single logical file may contain an arbitrary combination of types, and the operating environment can be relied upon to do sensible things with those compositions. 

* New types are, in general, old types with new constraints on their composition. 

* All data is sortable, such that if the order is arbitrary for a grouping of data, it is encoded the same way each time. This is critical for deduplication of content in the Arcive. 

To contrast this approach with the state of the art: a website contains a logical structure in the form of files and directories, a server that can present a second, logically distinct superstructure of the same format through URLs, and finally pages, presented to the user, which fetch down this content in yet another logical order provided by the formatting of the HTML et al. 

In Arc, we present a hash to the network, and chunks of the content arrive until we have enough to display it. The data structure contains text, images, logic, and whatever else we want it to have, including placeholders with hashes that can fetch down yet more content, video shall we say.

We would like to not have to include an entire video in order for the value of the video to be a part of the identity of the container (say the page it's embedded in). We also don't want a page to have a different identity depending on whether the content was fetched or physically included. Clearly, our metaformat is nothing but a concatenation of hashes which is itself hashed to provide the identity. 

This means once you create a lolcat, it is one and the same lolcat from the perspective of every person who views it and every context in which it is embedded. They are all served from the same hash; as many extra copies are kept around on the Arcive as are convenient, and no more. The minimum is three.  

Also, if you copy and paste the Gettysburg Address from a website, you should end up with the same Gettysburg Address, not a different one. This is transclusion and we want to have it. We can have it, if we give up on the idea that information should have a physical canonical source on the network from which we retrieve it. 

Right now, transclusion is called hotlinking, and is bad form. Jesus wept. It seems like a small matter when it's lolcats, but when it's multi-GiB scientific data sets, it makes a difference. Right now, a URL citation says "well, we found something here once that was what we'd like you to look at. Good luck!". 

What is needed is a name for data, not an address. A citation should say "this is the identity of the data in question. If you find something with this identity, it is that to which we refer". An address, if provided, simply says "the computer at this location can probably find the data in question".

##Representation and Resilience 

Choosing the wrong level of representation can have severe consequences on resilience. Sound encoding is a good example. A sensible format for sound would treat encoding as entirely separate from what a track *is*. Not only that, it wouldn't rely on promises, such as a header that says "this file definitely isn't Never Gonna Give You up". It would say something like "regardless of encoding, you'll find the following sonic fingerprint if you analyze the track with this piece of logic". The logic would do what our ears do, basically. Two files containing the same song in different encodings would have the same sonic fingerprint header.

This doesn't have to be pass / fail, either, that's merely the simplest implementation. For bitmapped data, a mild Photoshopping could still provide say 90% confidence that the file is 'the same image'. For anything with a canonical sensory form, this kind of validation works, and there are plenty of proprietary solutions built on this premise: Google Reverse Image Search, Shazaam, and Soundhound, to name three. 

Google also does a good job of detecting what natural language a passage is written in. It's the same trick. The equivalent test for a passage of text would involve OCR on the bitmap that the environment generates to display the text. 

The technique is surprisingly general and works for any static representation of data which is ultimately presented to the senses. It cannot tell you if two games written in Python and Clojure are the same game, any more than it could tell you if a movie and a book have the same plot. It could tell you if an audio book and a text file have the same text, however, or take a good guess at it. 

While hashes provide a guarantee of exactitude, this kind of fingerprinting can provide a guide to similarity. 

There is another kind of exactitude beloved of computers, that of type. Hashing is for identity, for type, we need some other approach. 

##More Unicode Bashing

There are few topics more muddled in our field than what a type 'is'. I mean something relatively simple: that there exists a function which can take a certain input and validate that it may be treated as of a certain type. For instance, we may define a degenerate type, Blob, the validation function for which simply returns true.

This is also the validation function for ASCII, and hence in principle for Unicode. In practice, Unicode is such a beast that one can often reject arbitrary data as unlikely to contain it, given the number of barren code points. 

Anyone from the DOS era remembers loading binaries into a text editor, and the resulting beeping and screen vomit. It is possible to interpret any data at all as ASCII, though this is unwise. Off on [mars](http://moronlabs.com), cgy is embracing this peculiarly arithmetical relationship between ASCII and the low numbers to do voodoo. I see the temptation: it's here, it's queer, and we're used to it. 

My beef with Unicode begins with its basic paucity. The logic is that of English and Latin writing, falling apart as soon as one reaches French (e, é etc. have a dictionary order which is not preserved) and becoming wholly ruined somewhere beyond the European mind. Ge'ez, for an example, has a relationship between the characters that isn't preserved by writing them all out, while the Hanzi are made of radicals, damnitall, they are in no sense some named flat collection of code points and it's a crying shame to treat them that way. Check out a Chinese input form for how the logic of Hanzi should be handled. There are options. Better options!

It gets ridiculous, and dangerous, with a practice that is universal and placidly accepted: users are encouraged to input text using the same encoding as the interpreted language running the logic governing the local environment. The mind boggles. There has to be a better way. 

In my own little backwater, I am also peeved that Unicode's "one code point per glyph" policy ends up overloading most of the glyphs used by the IPA to represent sound. If I type e, you have no idea what I meant, and Unicode can't help you. It sucks the root: I can use visual tricks to inform you, the reader, of my intention, but the computer gets left out. 

I could go on. I suspect I shall. For now, suffice to say that it is to our advantage to design types which can be validated, and if we can do so rapidly, so much the better. 

##Functional Validation.

The idea couldn't be simpler. You have a chunk of data and some reason to believe it might be, say, music. You run it through a function, which may examine it but not alter it, and the function does things. It returns a verdict, true or false. 

The function can and should be anything. It is to our advantage if the heavy lifting is done by a grammar. Grammars can be ambiguous, which is a thorn for interpretation but can be a great boon for validation. They are also declarative, serving to isolate at least some of the semantics of execution from the constraints expected.

This is about more than running differently on different architectures. A grammar may be used exactly, in a linear fashion, or it may be used statistically, to provide a degree of confidence that a file is more-or-less of a certain type. This is useful when files are damaged, missing, or when we must make an initial guess as to type because we have no clues. 











