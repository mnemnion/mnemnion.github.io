---
layout: post
title: "Progressive Refinement in GLL"
date: 2013-08-04 14:24
comments: true
published: true
categories: 
---
    
The [GLL Algorithm](http://dotat.at/tmp/gll.pdf) is one of those core concepts that can change how we do computation. It's phenomenally powerful. I believe we're just starting to see what it's capable of.

I plan to implement GLL, as soon as some of the support work is done. It's going to be a somewhat leisurely task. I'm taking a lot of notes. Currently, I'm using [Instaparse](https://github.com/Engelberg/instaparse) to play around with the algorithm.

It's fantastic stuff, much more flexible as an idiom than, say, ANTLR. It is also much easier to write yourself into an exponential corner, and I find myself abusing regular expressions somewhat rather than torturing the grammar in other ways. 

I think this is a consequence of GLL's strengths, which allow it to parse out well over data that is already in a nested structure. Using it for a shallower kind of pattern matching kills performance even in parallel execution, because at any point in matching a long string of related structures, it could encounter a context that would kill the whole chain of inquiry. If there is any possibility of ambiguous matching, this explodes even faster. 

An example of this kind of use is parsing a bunch of sentences and paragraphs into text, which is words and whitespace. Except if you encounter a special character you have to switch context completely; this last requirement makes most natural ways of writing the grammar fail. 

An alternative would be progressive refinement. In Instaparse, the latest definition of a rule is used, and all earlier rules are ignored. My proposal, which I intend to use in my own work, is that multiple definitions of the same rule are tried sequentially against the data, after a successful parse.

This would damage the arbitrarily-ordered nature of Instaparse grammars, in a sense. Instaparse still has to decide what to do with multiple definitions, and currently uses the last one provided. 

This would just formalize something one can already do with Instaparse, which is to parse a string, flatten the area of interest back into a string, and re-parse that string into a new rule. Substitute your new parse for the old one (this is very easy), and you've done it.

Automating that into the grammar would allow one to grab large globs of identical data, switching context when necessary, then parse that data into shape in smaller pieces that don't have to worry about context boundaries. 

It's a straightforward adaptation, being properly speaking a composition of the same function, `insta/parse grammar`, over a subset of the data returned by the first call. 

##Automated profiling

One neat thing about grammars is that you can run them backwards to generate compliant gibberish. As you'd expect, given the vast difference in magnitude between the set of valid input of a given length and the set of all input of that same length, it is quick to use Markov type rules to generate babble for even an intricate, ambigous grammar that would blow up trying to validate the output.

In fact, that's the point. One may envision, and I may even write, a tool that uses pseudorandom walks to generate longer and longer streams of valid gibberish, and try them against the grammar, looking for suspicious jumps in speed or number of possible parses. Even a naive tool, running on a remote server, would generate useful information while developing a parser. One may envision the tool dialing in where parses go ambigous, generating input accordingly, and alerting the user, or doing the same for square and cubic factors that show up.

If the validity of the babble is questionable, then one has identified permission within the grammer that one may wish to eliminate. It has potential.