
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

## Arguing in favor
Well, okay, I know this may sound like nothing, but a lot of benefits come from it. 
If you think about *what is known today as* Object Orientation, most mainstream languages already use this postfix call notation when referring to a method defined in the object - be it defined in the class as in Java or C++ or even defined in the prototype as in Javascript. It is also common to have property accessing using the same syntax, so languages like F# or even Rust that have Records, Structs or anything like it share the same behavior. The name for this is dot notation, pretty well known in almost any language. The function should therefore act on the first argument using the second parameter, and well, here's an example:
```
fn cut = (String word, Int from) => word[from:]
```
What we really are doing here is some operation on the word, so we are indeed acting directly on it. It would make a whole lotta sense then to call `"word".cut(2)` instead of `cut("word", 2)`, right?
And yes, some may argue that the definition of the function could be in the record scope, in the class file or even directly in the prototype. Obviously this is not always possible - C# has extension methods whilst Java lack them; STD libs are already defined in most languages, so we cannot therefore change i.e the `Array.prototype` from JavaScript directly. 
The last argument I can think of is expressiveness power. You see, lots of languages lack the power of a pipe construct (such as Bash with `|` or Elixir with `|>`), which is very useful for you to make chain of operations piping down the result of the latest call. And when this is a fact and the language has no intention of changing it, chaining down the operations with a dot operator gives the programmer this form of flow, which makes the chaining feel natural. For those of you who have ever programmed in Smalltalk or Dart, tell me how great it is  to use the method cascading construct - altough it is not the exact same idea as you are reusing the referenced object and not the result, the expressiveness it gives you is close; that is something really good to give to the programmer!
## ... and against
When I started writing this, I thought my feelings for this feature were mostly good. I could not however explain why, but it just felt right. But while doing so, I realized there are a lot of arguments against it, and here are some I could think about.
When you talk about this - or about any syntax sugar, really - it makes you realize that you are leaving an opening. In this particular case, an opening for the programmer to choose which way to call the functions. To leave this power in the programmer's hands is to let a door open for a code mess - some programmers may be used to the old form of declaring values and passing them to the next operation and some other may use this other form, and they may mix in a project that has no style guide whatsoever. And we all know how good it is to keep a shared pattern on projects so it feels easier to read - one may get confused if this pattern is used once or twice in the middle of the whole f(x,y) code base. Obviously, this is a bigger discussion: should we then not give any options to the programmer in this terms? would we not be taking creativity out of the game, then? This is a discussion for another time; for now, just be warned that this might be used as an argument against - or even in favor as used by Bjarne Stroustrup [here](https://isocpp.org/blog/2016/02/a-bit-of-background-for-the-unified-call-proposal) - of the feature.

Talk about how having two call syntaxes may be bad because it does not provide uniformity among codebases, talk about how it looks as if we are mutating state in the examples, how it is harder for intellisense or people to find the definition of the method as they don't have to only scan the definition file (but this is the same for extension methods in Kotlin or C#), maybe about how curry is made harder in this terms.


[1] - ABC is the language that was designed by Guido van Rossum before Python; also the one to inspire it. Talking 'bout features, that is a language that had list comprehensions before it was cool!
