The idea for this small article is to bring the idea of dependent type theory to the table. I will not extend the piece in the math of it, because [1] covers it way better than I could.

# The idea

The idea for dependent type theory is to use types as a high order construct. As for high order, I mean that you will be able to use the return value of an expression as a Type. This looks like a simple idea and may also seem that is has no utility - but this is because people are not used to it. 

I'll take the same example as the language Cayenne[2]. In the paper, they use the idea of ```printf```, a common function used in C. Let's take the method signature for it:

```int printf(const char*, ... );```

Okay, so we now know that it is a variadic function, and that it may receive parameters of different types. The thing is: the types must be based on the first argument, the format. I will not extend myself on that function, but the reader has to know that when the format is in the form ```%d```, it expects a number. 

Let's say I want to do this:

```printf("Hello World %d", "hey");```

I'm passing a ```char*``` argument when the function was expecting a number. This represents undefined behavior. But the whole point is: the compiler is not typechecking my argument! It will accept it and output some unexpected value. If I've stated that I would pass a number by using the ```%d``` (pun intended), then the compiler shouldn't let the application pass.

### But what if there was a way to overcome this?

And there is. In Cayenne's paper[2], they show you how to write a printf function that would typecheck. I'll try to express the idea in a syntax that the reader is probably used to (the language used is #{LANG_NAME}). 

```js
1. let printf_type be the Fn of Universe (fmt is String) => {
2.  match fmt with 
3.    "%d" => Int
4.    "%s" => String
5.  end
6. }
7.
8. let printf be the Fn of Unit (fmt is String, arg is z) where z is printf_type(fmt) => {
9.  ... //the implementation of a syscall to print to the screen
10. }
11. 
12. printf("%d", "hey"); //will not be able to compile
```

Beautifull, huh? 

The whole idea is that the second argument of printf is now the ```Universe``` returned by the type expression printf_type passing the argument ```fmt``` received!

This means that the expression in line 12 is going to output an error on the compilation phase. No runtime errors, no undefined behavior: you get what you've expressed because of your types!

This article was just to bring up the idea of dependent types and talk about why they are a good deal in order to achieve type safety.

*Note: There is a much cleaner and more complex example in the work [2]. This example was just to bring the idea for the reader used to some C-family syntax. If one likes the idea of dependent type, I'd recomend that and [3].*

# The type of all types

Inspired by The Little Typer[3], we represent the type of a type as the ```Universe```. This is the same approach as in #{LANG_NAME}. So when we write a type expression, defined as an expression that returns a type in the current context, we write down that the type of return will be a ```Universe```. 

Haskell use Kinds [4] and the ```*``` representing the type of types. There's also the idea of Higher order type constructors (as for the ```Maybe``` monad, for instance) that construct a ```Kind``` from another ```Kind```. Taken from their example, we can clearly see the idea that relates to the article:

```haskell
Maybe :: * -> *
Bool :: *
Int :: *
```

# Conclusion

The whole idea of dependent types is to achieve type safety. In fact, every type theory when applied to the field of programming languages is chasing that same goal: bring type safety and remove undefined behavior (or the likes). This basis for this type theory was introduced by Per Martin-Löf[1] and called *Intuitionistic type theory*. Worth checking it out!

# References

[1] http://www.cse.chalmers.se/~peterd/papers/MartinL%C3%B6f1984.pdf

[2] http://www.kframework.org/images/5/5e/Cayenne.pdf

[3] Friedman, Daniel P. and Christiansen, David Thrane. 2018: The Little Typer. MIT Press Ltd

[4] https://wiki.haskell.org/Kind
