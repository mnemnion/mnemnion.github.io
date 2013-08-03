---
layout: post
title: "Celsius Versioning"
date: 2013-08-01 09:25
comments: true
published: false
categories: 
---
Celsius Versioning is a development of [Kelvin Versioning](), yet another Urbit innovation. You may notice that I crib fairly heavily from cgy. It comes from working on the same thing, with different strategies: I favor infiltration, Urbit favors Martian. 

It's unlikely I'll have usable versions of my toolchain before there's a Martian-English dictionary. I've downloaded vere and I've run it; it does something, which is more than I can say of GGG or ent. They aren't nothing, constraints have been applied and much Markdown generated, but procedurally there's bupkis. 

Kelvin versioning is Martian to the core and conceptually as clean as liquid gas. Pick a number, count down per revision, reach zero (or within a few degrees), STOP CODING. Other computations may now treat your code as a feature of the universe, rather than as an inherently untrustworthy, larval creature. 

This is meant to replace the current technique of fossilization, wherein standards are published, or not, and respected, or not, until the sheer weight of surrounding code makes further changes impossible. You then have a rather funky mineral. But it is a mineral, and we may build cathedrals with it, with a comfortable margin of safety. 

Well and good! You know a tight metaphor when you can safely torture it within an inch of organ failure. Kelvin versioning has no concept of an alpha, beta, and certainly no concept of an n.0 endless release cycle. That is the whole point. 

Celsius versioning is based, like Celsius itself, on water. It is correspondingly less stern. The notion is that you finagle the code down to zero and switch to Kelvin versioning. Conventionally, you then have 273 changes (defined below) left to your codebase. Perhaps physics works differently on your planet and 0˚C = 512K. No matter; it is polite to call your shot early. Code being what it is, I could make a good case for 256 as a conventional Kelvin number; as someone with a background in the Smelly Science, I'd rather not make my professors blanch. Let's take our rather arbirary extra 17 changes and consider them the release runway. 

Freezing is a big moment, though. Leeway is allowed; the whole purpose here is signal strength.

Conventionally, code should be assigned a number over 100˚ before a project begins. Above 100˚ the project is vapor, and will expand to fill all constraints. 

Feel free to heat the code up when you find more stuff that still needs adding. Anyone who builds solid components with gas deserves their fate. The full name of a release always includes the ISO form of the release day, so the temperature is meant as a signal of how close the code is to reaching the first phase change. One might graph it over time, like the weather. 

When you cool down to 100˚, your code is expected to condense, and fill the volume provided. You now have 100 steps left, and should always count down. Liquids go straight into the fridge, it would be embarassing, and a bad signal to send, to heat up code in the 0-100 C range. If you don't have a firm surface area, you're not at 100˚. Feel free to change as much as you wish with each step, keeping in mind that if you're not changing less and less as you descend, you'll never hit Kelvin, which is a Bad Thing, generally. 

At 0˚ C = 273 K, your code is as done as you can make it. Errors are all that remain. If you have done your job well, the last 150 or so degrees towards 0 K will be awarded by acclaim, without further changes to the code. 

By that definition, you have a well-defined source syntax and executable representation, and the format of this will *not* change further. Ice is a solid, which can only be chipped off and deformed under great pressure. Therefore, each degree downward from 273K is a single change to a leaf node in the parse tree. I trust this is sufficiently precise for our good company. 

This is a different Kelvin from cgy's original. He says one change per degree, and by change he means something like a commit that compiles. I call that a Celsius commit. A Kelvin degree is one operational transformation, exactly. 

GGG and ent will use Celsius versioning, beginning life as high temperature vaporware. As a rule, small code can be cooled quickly, having less volume from which to expel entropy. A degree is meant as a signal of doneness, not as an amount of change left to make in an absolute sense. That's what the post-freeze exists for. So a ten line standard will still start above 100˚ during the commentary and peer review stage, but may get knocked to 50˚ in a single move, bumped down by tens to 0˚, four or five typos later get voted to 256K, and stay there for, conceivable with no further changes other than adiabatic cooling. 

