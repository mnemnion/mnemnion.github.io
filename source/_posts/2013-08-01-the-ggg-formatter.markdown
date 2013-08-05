---
layout: post
title: "The GGG Formatter"
date: 2013-08-01 22:57
comments: true
published: false
categories: 
---

The GGG formatter is a key component of the GGG system. Any compliant implementation *must* offer a formatter. There is no expectation that they be the same program in any meaningful sense. It's fine to specify someone else's formatter and provide a distinct implementation, as long as the particular formatter is specified. `ggg fmt` is the expected command line invocation. 

A GGG formatter *may* accept Unicodes, but it is very much encouraged to make Latin-1 encoding a conceptually distinct step from formatting in this case. `iconv`, found on Unices the world over, does a fine job and there is no reason not to use it. 

Formatted GGG is the only kind that should be published or shared. Obviously a repository or the like will have the unformatted GGG in it, but this is not the point. GGG is designed to be bytecode fast to parse, which is not true of unformatted GGG.

Since file suffixes remain important, let's talk about that convention. `*.ggg` refers to unformatted GGG, and things being what they are, will often be in a Unicode or conceivably in straight ASCII. Latin-1 is encouraged if it's easy/pleasant for one to edit with it. 

Given the superset nature of GGG, and the role of the formatter in providing implied types, every formatted GGG can be categorized by the degree of power needed to parse it. Thus, the suffixes for formatted GGG are `*.lgg`,`*.rgg`, `*.dgg`, `*.cgg`, and `*.fgg`. They stand for literal, regular, deterministic, context-free, and funcional. If there are collisions, add an extra g, G. 

The formatter does three basic things. It validates the grammar, formats the code according to precise conventions, and performs unification to determine and mark up rule levels. 

The first step should require no explanation. We are neck-deep in a wall of text about validating grammars, are we not?

The second step is formatting in the more usual sense. Input GGG is fairly relaxed about whitespacing: it is perforce mandatory in some places, and forbidden in others, and optional in yet more places. Input GGG will also be fully specified, just harder to parse, while still being very easy. 

A simple example: We allow `:`, `=`, and `:=` as assignments. `::=` may also be supported in future, as there's no real reason not to have it other than that I think it's fugly. A GGG formatter will replace all of these with `=`. 

Given the spec, there's actually no reason to forbid `::====:=:=::====` as an assignment, either, other than the aforementioned fugliness. Please stick with the three provided. 

Another example: rules are terminated with `;`, but one needn't provide it. The assignment operator to the right of the rule name definition allows for disambiguation. The formatter inserts a semicolon so that an implementation which only reads formatted GGG needn't allow for this step. 

The goal of formatted GGG is to allow a parser built on bitmasking, counters, a single, switched context with no memory, and a single token of lookahead. This can be made human readable without much difficulty, but will be too strict to write comfortably. 

While we may not succeed at this within function rules, we should strive mightily for it. Schemish syntax is, by design, pretty good at this. Lisp heads like fast readers too. 

##Unification

The third step in formatting, conceptually, is unification. Rules are accented to mark their type. In older schemes like EBNF, the type of a rule is indicated via case: `parse rule`, `Regular Rule`, `LITERAL VALUE`. Note that BNF, with its roots in natural linguistics, allows spaces in rule names and uses `,` to indicate concatenation. 

We have five types of rule, not three, which would be convoluted and ugly to provide using case alone. This is the rationale for using Latin-1 encoding, though I feel we make good use of the additional punctuation and symbols also provided. 

Using accent (these are more like tone marks) to disambiguate words is superb, because this is what accent marks were invented for. Latin the language has extremely regular vowels, just like its descendents Spanish and Italian. It also has five of them, you will not be surprised to know. English? Fourteen or more. And we don't use accents. Insane.

In order to accent rules by type, the formatter must be able to determine which type a rule is. This is, in our case, fairly easy to do, but is not a trivial matter of string comprehension. 

In the formatting pass, `ggg fmt` allows any accent marks on any rule. It then performs inference on all the rules. Literals are easy: they must have exactly one rValue, and it must be in a literal format. 

Regulars have three requirements: they must contain certain characters (one of `|~#,*?!±§` must be present), they may not contain certain characters (an example being `/`), and the rules they refer to must themselves be regular or literal. If regular, there must be no recursion, that is, no chain of rules calling rules may ever invoke a rule already called. 

That's unification in an nutshell: checking the regular rules to make sure they are a DAG, and marking accordingly. 

The others are easier, actually, because recursion is allowed and it is up to the implementation to try and realize the grammar in a useful period of time without encountering infinity on the road. 

The precise mechanism of markup is simple. Here's an English word: "Schwa". You can tell it's kind of not from around these parts, because we often call it "Shwa" since that's how we say it. Anyway, there are four consonants before you finally reach a vowel. The previous sentence has vowels at 1,3, 1, 2, 2, 2, 2, 2, 2, 1, and 2 within each word. That's typical. 

So we have this rule: when accenting a rule name, we allow up to four consonants and tag the first vowel we encounter. If we get to a fifth consonant without finding a vowel, we prepend one of ``´`^¨``, just like if we fail to find a vowel.

That's mandatory: no matter what markup accents you supply, there will be a disambiguating character in the first five characters of the name. The rule below is short circuited if there are other accent marks provided: the formatter instead replaces them if necessary with their correct equivalents. 

Presuming we're doing clean accenting, rather than correcting accents provided by the user, then each time we hit a dash or underscore, we place another accent mark on the first vowel we encounter, if we happen to find one. A period marks the boundary between a rule and a subrule.

The formatter must be conservative, and assign the lowest rule level possible. Sometimes that isn't what you want, and there's a trick to force precendence. Each middle level has a unique alternation operator, `|`, `/`, or `¦`. Literals are strictly defined by format, and functions always contain `[` as the beginning of the rule and `]` as the end. There is a rule which always fails, `¬`, which is the NOT sign. It is the antonym of `@`, the rule which succeeds. Therefore, you can make a CFG rule out of a literal by saying `^CFG = 0b01 ¦ ¬ ;`. This rule either matches 0b01 or fails. This rule is *semantically* identical to `literal = 0b01 ;` but may well be implemented differently. If that's what you want, that's how to get it. 

Here's what this gets us: on the human side, a readable format wherein one may instantly recognize the nature of a rule found anywhere within a grammar. That's non-optional, these grammars must be readable because their meaning is often fairly frozen before any non-wetware implementation can be usefully provided. Also, they are meant to be part of an answer to the question "What on Earth is this data", which is ultimately being asked by humans with computer assistance. 

On the computer side, exactly two bytes after a semicolon (to skip \n, which is there but needn't be looked at), you have a new rule definition. Five characters in, you are guaranteed to know what kind of rule you're parsing. We can then jump ahead, look for whitespace, throw the symbol into a hash that doesn't care if it's seen it before (but keeps track of lValue vs rValue), skip the `=` and space that we know are there, and we're at our next meaningful symbol. Building an intermediate representation in this manner can be quite dirty and fast and hacked within an inch of life, because the format is aggressively deterministic and self-describing. 

There is a standard way of compressing text, the [LZW algorithm](http://www.cs.duke.edu/csed/curious/compression/lzw.html), which is practically a feature of the Universe at this point, certainly the Unixverse. An implementation may certainly read LZW compressed GGG directly, to great advantage. At the least we may feel comfortable sending it around. Late in the freezing process, we may even standardize a dictionary containing the reserved function rules, in code points within Latin-1 that have been forbidden for general use. 

##Implementing the Formatter

GGG is being bootstrapped as we speak. The only code I've written so far is a peculiar, funky-flavored mix of regular expressions in a weird string-escaped syntax within JSON that is fit to a further schema. That's how you do syntax highlighting in Sublime Text, and it is a fsked, janky kludge indeed. It's almost like we need opinionated editors that work with formatted data, but we'd need a really good way to describe those formats, would we not?

If that sounds like your dayjob, I feel bad for you son; I got 99 problems but config ain't one. 

The next step is to write a grammar for GGG itself. This will be written in Instaparse, for a few good reasons. Instaparse uses GLL on [Java-flavored strings](http://stackoverflow.com/questions/3240498/why-does-the-java-ecosystem-use-different-character-encodings-throughout-their-s), which is completely terrifying but means whatever Unicode one is typing in, in practice. 

GLL is our reference algorithm for GGG. I know and like Clojure, and have done very minor hacking on Instaparse, in the form of a graph visualizer and [aacc](http://addme.com), which is best interpreted as a groping toward the functional language needed by GGG. The syntax of GGG, particularly the input flavor, is inspired by Instaparse. It stands to reason that an Instaparse format grammar of GGG will be fairly close to the final GGG form. 

The grammar will be used with Instaparse to write a formatter. Because this is a bootstrap, and because I get to break my own rules, this will input and output in whatever janky Unicode you're stuck with. I will then pipe it to a program that makes it into Latin-1, then change it back and validate the original output against the twice-transformed output. We save the Latin-1 version for software and keep the Unicode around for Markdown and editing purposes. 

Clojure is a good enough Lisp idiom for this purpose, in that it may be made easy to read if one likes. The source code will be chatty, and I trust Schemers and CL heads will be able to translate fairly readily. 

There will be complete grammars of both input GGG and formatted GGG, in GGG, probably before there is anything which can run GGG. This is all for the best. It is possible that LGG through CGG will be done before FGG is even attempted. Certainly, the context within FGG is entirely different from the context outside of it and the grammar rules will be scoped and namespaced accordingly. 

I pinky swear not to publish an implementation of GGG that uses Unicode anywhere in the tool chain. The source of the reference implementation will conceptually be ASCII, regardless of actual encoding: existing languages make this easy by bare necessity. That's also why there are ASCII equivalents of everything one must actually type in: I would prefer not to hear complaints about how the format is hard to edit on a remote TTY while hanging off the deck of an oil rig. 

I gave thought to [Lush](http://lush.sourceforge.net/), and got it working over on my Linux VM. It has an excellent story around integrating C and Lisp, and a collection of operations over binary data which is quite on point. Unfortunately, it uses ELF format for object linking, leaving Macs and Machs out in the cold. It's not the Mac part, it's that I want to target Lisp and C, not a funky flavor of Lisp and C *and* ELF. That's one dependency too many. All dialects of Lisp are intrinsically funky, but [Embeddable Common Lisp](http://ecls.sourceforge.net/) is an ANSI compliant Lisp written in ANSI C which is object-code agnostic. Sounds about right.

The implementation will come with its own formatter, the output of which may be unit tested against the bootstrapped Clojurian version. 










