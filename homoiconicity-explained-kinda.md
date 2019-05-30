I've said it before and I'll say it again, as many times as necessary: the world of *compsci* is filled with holes. You look at one definition that you find interesting, dig deeper and find out that the term is ill-defined. You search in books, read the 60's papers and see that people do not use the same definition for the same term. Then it hits you - so how in the name of the holy Unicorn will I talk about this concept when not even the academia shows consensus on it? 

Well, this article is about another ill-defined term, and it probably wont clear your mind. Yet again. But I hope it gets you where I am so that we can at least share this doubt. Let's talk *homoiconicity*.

## Some actual definition

Let's begin with the actual definition - or, at least, the widespread one. If you ask around "what is the meaning of homoiconicity in the context of programming languages", some people will laugh at you because that is not applied to webdev and say you are out of date and should go study node, react or some other JS-trending stuff. But as for the other developers, they'd tell you that being homoiconic means **repesenting code as data (and vice versa)**.

This concept looks simple at first - but it gets tricky when you think about what data is and what code is. Because with that definition, we could say that JS, for having an ```eval``` function, does represent code as string and data as string, so you can manipulate both the same way and call the evaluation function. 

But when you get an example of what an homoiconic language is, it is possible to see what is the **actual** idea behind it. And to do that, I will use the canonical example of homoiconicity: Lisp. 

You don't really have to know LISP to understand this, although it will probably help, even if you've just played around in your emacs for a while. Well, LISP is a language based on S-expressions (all those parenthesis), and S-expressions represents stuff as lists. So, if we have something like an add operation ```(+ 6 9)``` we have a list containing a plus sign (operation) followed by two numbers. Put that in your mind.

Of course, when you type that into any lisp interactive session, it will be evaluated. But you can also store that list as data in another variable using ```quote```. For instance, one could do ```(define x '(+ 6 9))``` and now x is an actual list, not yet evaluated, containing an operation and two numbers - now it is pure **data**.

That type of data can actually be evaluated. Type ```(eval x)``` and check the magic. 

Well - you might say - but how is that different from javascripts eval?

And here is the part I'm hoping I can elucidate. The big difference here is that, in Lisp, we eval data - represented as list - as code - which is also lists of stuff. In javascript, we are using strings to represent code, and the code is based on statements, primitive types, loops, and so on, and so forth. The thing we evaluate in javascript is not something considered as first class by the language - it is just some plain-old string. 

And to prove that, I'm going to change X's code directly. Instead of adding 6 and 9, x will now multiply those two numbers - giving, of course, a different result. As x is just a list, we should be able to directly modify one of that list's element. For some basic lisp background, to work with lists we use ```car``` and ```cdr``` - just like we use ```head``` and ```tail```, respectively, in the ML familly. In the Scheme dialect of lisp, we could do ```(set-car! x '*)``` to change the head of x. 

Now, if you try to print x, you'd probably have ```(* 6 9)```, which is exactly what we would expect. You can evaluate that, and, as expected, you'd get ```54```. This means that we can dynamically change code easily in lisp because the representation of data and code is the same - and so is the manipulation of it. This is the big difference from JS, which would only let you manipulate a code in string format using ```split```, ```splice```, ```replace``` and stuff like that - unless you're working with data parsing, you wouldn't have to work with that stuff.

## Historically speaking

Started with TRAC in the sixties, then talk about Lisp and how HeCoinedTheTerm (talk about how Kay spoke about the term).

## Meta-circular evaluator & macros

## Well, this is unfortunate...

Show the C2 Wiki articles (all them) and show why is it hard. Also show http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.453.7185&rep=rep1&type=pdf because I really liked it.

http://wiki.c2.com/?HomoiconicExampleInManyProgrammingLanguages
http://wiki.c2.com/?HomoiconicExampleInJava
http://wiki.c2.com/?HomoiconicLanguages
http://wiki.c2.com/?HomoiconicFaq

## Some examples

Show the IO example, show the Lisp example, show the TCL example and try to find some 

## Some not-so-much-examples

Show the string + eval idea and explain why it does show some weak typing of homoiconicity because it is not treating it as first class, just some random string representation

(
## Yet again we come to this point

If we talk about weak/strong homoiconic languages, show that we came to yet another ill definition, hopping from one hole to the other.
)

## References
