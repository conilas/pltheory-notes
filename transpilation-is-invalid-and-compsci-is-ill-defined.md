I've been reading a lot in different places the word "transpilation". People are using the word to define a process in which they take one *common* language and translate it to another one. Note, however, that the term *common* is absolutely vague, and that is what this article is about.

## Quick definition (Glossary)

I'll use the term language in the following article when refering to the language itself (spec) or to it's reference implementation. It should be easy to distinguish them.

## Transpilation in *forums* language

People are used to the term *compilation*: that is what they learn at school or maybe after some reading over the web. It is a simple term used by many to define the process of generating a binary application. Back in the day, languages like C, FORTRAN, Algol 60 and so on followed only that approach: taking the source code and spitting out a binary application to be ran on some architecture/SO. (*there are exceptions - lisp[1], for once, was interpreted before it was cool. but I'm talking about industry standarts; mainstream languages*).

But things have changed, and modern languages have more paths to follow - the language may be interpreted, it may follow some idea of tiered-compilation with JIT and it may even be translated to another language. This is the approach that, for example, TypeScript[2] and Nim[3] took (for different reasons, but this may be another articles concern :-)).

And here comes the definition: people say that TS and Nim *transpile* to other languages - that is, they take the source code in one language and output it in another one. 

## The actual definition for compilers

The word compilation, as stated earlier, defines the process of taking some code and outputing it to some *other* code. There is no distinction between which language one may have as input and which may be the output. One may recompile ASM code - having ASM as input and ASM as output in order to try and perform some optimizations. 

### ... and why is transpilation ill-defined

This it the problem with those definitions: they are the freaking same! You are not transpiling from TS to JS. You are not transpiling from Java to Bytecode, neither. You are *compiling* it.  Both terms point to the same process - [input code in #lang] - magic - [translated code], There is not difference that holds at all.

And the problem, as always, is toxic people. The guys that want to know more than you - so they correct you when they say "this high level language is transpiling to this other high level language. You silly cunt*. And yeah, I'm looking at you, reddit and stackoverflow - that's the place where those guys hide, tha's their cave. The term transpilation is defined just so they can show that they know more than you because, for some reason, they *need* that. 

Sure, some people may argue that if you output human-readable code, then you are transpiling. That looks like a definition that holds, right? Except it doesn't. At all. Human-readable is yet another term that has no good definition: what about esolangs, such as Iota[4] or brainfuck[5] or any other turing tarpit[6]? What about code-golfing[7] languages? 

Okay, so another argument I've found is that is must be *human-readable* and have some application. Yet again, that does not hold. Is APL[8] even human-readable? 

## Another ill-defined term example

Well, that is not a problem that exists only in compiler/programming language areas. That problem is a big hole in compsci. 

As stated in the last section, some people use the term "high level" language to define compilation. And here it goes - high level language is another ill-defined term! So people are basing their bad argument defending some ill-defined term in some other ill-defined term - a recursiveness without TCO of ill-definition!

That term was usually defined as "a language that does not care about the underlying architecture". Let's be a little scientific here. The first definition I could find was Hoare's paper, in which he states:

> A high level programming language, such as Algol, FORTRAN, or COBOL, is usually intended to be implemented on a  variety of computers of differing size, configuration, and design. 

There it is - a language in which you would not have to care about details of the architecture you're running on. And check it out: Hoare says that Fortran, Algol an COBOL are *all* high level languages. And so is C, for instance. And it it a common misconception that, because those languages are old or offer some type of access to memory, they must be low level - because people say that Python, JS and so on are high level.

And I'll try to not be pedant here. There are new definitions for high level languages, and people are pointing that towards *modern* languages, with modern concepts - still a vague definition, I know. That shows the problem - the term is changing and maybe the definition also is. 

But that means we do not have a solid definition for that term, and therefore can't use it as a basis for other arguments. 

## Conclusions

Computer science, when you dig deeper, is kind of a mess. Some school-of-thought will use a term to define the same thing that others chose another term to represent. And some of them will use the same term to define different things (such as high level languages). 

This is something we can run away from - stop trying to coin terms that cannot be well-defined, or else it will cause problems for future research. The term compilation defines a process, and the term transpilation will define the *same* process with one or other constraint - so why create that other term when you can specify even more your compilation process?

Let this be a cry for help - stop trying to use terms in order to look smart. Use the actual definition and maybe do a bit research on the term definition to understand where it came from and why you are using it. It is not hard to avoid problems in the future if we can avoid being dicks - sorry for the harsh words. Here, have a panda: 🐼.

## References

[1] McCarthy, John. "Recursive functions of symbolic expressions and their computation by machine." (1959). - http://www-formal.stanford.edu/jmc/recursive.pdf

[2] https://github.com/Microsoft/TypeScript/wiki/Architectural-Overview

[3] https://nim-lang.org/

[4] https://esolangs.org/wiki/iota

[5] https://esolangs.org/wiki/brainfuck

[6] https://esolangs.org/wiki/Turing_tarpit and http://wiki.c2.com/?TuringTarpit

[7] https://code-golf.io/

[8] Iverson, Kenneth E. "A programming language." Proceedings of the May 1-3, 1962, spring joint computer conference. ACM, 1962. - https://dl.acm.org/citation.cfm?id=1460872

[9] Hoare, C. A. R. (1969). An axiomatic basis for computer programming. Communications of the ACM, 12(10), 576–580. doi:10.1145/363235.363259 - https://dl.acm.org/citation.cfm?id=363259
