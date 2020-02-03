# Syntax feature - Universal Call Syntax (UCS) or a.b(d)
One can argue that this is a somewhat normal sequence of events in PL theory: a feature that gets implemented again and again because it feels useful, or because it was inherited from some other language; but has hitherto not been named. After a number of reincidences of this event, some entity - be it a person, a group, a ghost or maybe a cool guy that has nothing else in his life -  decides to study an abstraction of it and may then be presented with the choice of a name. But hey - this might even be extended to science in general, right? ...well, that's not the main topic of this useless dissertation.

And why naming is important, you ask? Well, you see - for us, mere mortals far from the academia hurricane that is the implementation of a real language, it is plain to see that naming is what makes research possible. And apart from this pretensious bull, it is what makes it easy to compare two languages feature-wise. In the crazy scenario where there would be no naming for specific features, how'd you fight against that prick on facebook that keeps telling you that Python has more useful 'things' than Java? How'd you tell it that python has ABC (*pun intended*) [1] and Java does not?

The main topic is about this one feature that followed this path through a long generation of languages. A feature that has no place in diabetic's plates as it is purely sugar, but nevertheless deserves some attention for going through so many eras without receiving proper attention. What I'm talking about here is U(F)CS, or Universal (Function) Call Syntax. 

## What is it

Well, glad you keep your interest. UFCS is the sugar with power to transform a function with arity `n > 2` into a postfixed call with arity `n-1`.  That's pretty much it.

I know, I know, there's no need for examples as this explanation could be understood by a baby with serious injury on the brain, but let me enlighten your pretensious little world. Say we have the following definition: 
```
fn hey = (String a, Int b) returns Int => {...}
```
With this sugar-honey, we have the power to transform this
```
hey("hello",5)
```
Into this
```
"hello".hey(5)
```
Voi-lá! 

## Why is it good
Well, okay, I know this may sound like nothing, but a lot of benefits come from it. 
If you think about *what is known today as* Object Orientation, most mainstream languages already use this postfix call notation when referring to a method defined in the object - be it defined in the class as in Java or C++ or even defined in the prototype as in Javascript. 

[1] - ABC is the language that was designed by Guido van Rossum before Python; also the one to inspire it. Talking 'bout features, that is a language that had list comprehensions before it was cool!
