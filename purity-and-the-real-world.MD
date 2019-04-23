The idea of this small article is to bring out the idea of why do functional languages use monads to model impure functions. In this specific case, I'll be writing about the IO case, but the general idea can be applied outside. 

## In short

* Functional programming is based on pure functions - that is: no mutable state; only opearting on the received args performing transformations
* Impure functions operate mutating some state in a world that they should not even be aware of
* Impure functions can be transformed to pure by using the World as an argument, in which the function would receive the current world, transform it based on some other arguments and return the new World so it can be passed along
* As we need to have the same arguments from function to function - the world - and wait for each command to happen (an imperative way of thinking), we may model it as a monad

## Purity

Okay, so we start with the following: the real-world programmer, the one working in a CRUD API using a not-so-much RESTFull architecture, does not (in general) know what is a pure function. In fact, he doesn't *have* to: imperative programming languages (such as Java, C++, Ruby or so) abstract all this concept and leave only the business part for the developer. The idea behind this is that people need to focus on results, not details. 

I could add a statement saying that I do not agree with this because devs should undestand the world and what is happening around it and blá blá, but I will go on though another approach. What I think is wrong with that is: by not learning purity and some related concepts when learning to code, developers get used to programming in imperative languages and find themselves in some short pants when trying to learn functional programming. That being said, I do not think one needs to implement monads and handle purity everywhere - just as I do not think programmers need to write their own assembly code to achieve performance, they simply need to understand the idea.

## Simple definition

A pure function is just like a mathematical function. 

### ... and its explanation

The idea of a pure function is to not modify external state (non-local variable, IO streams and such) and to return the same value given the same input arguments. In fact, languages such as [Skip](http://skiplang.com/) use this for increasing performance - they store the result of the function from some given args and will not need to compute it again. 

Simple as that: a pure function will receive args, operate on them and then return a result. Say, adding, subtracting and so on are pure functions - they take the input and compute something from them. Not only numbers: a function that concatenates two strings is a pure function!

This all sounds beautifull - so we should always work with pure functions!

## :NO:

No, no we shouldn't. Actually, I'm not gonna be that hard-nerd (and maybe the reason is that I'm not that smart) that says working with pure functions is the greatest deal ever and we should only use them. That is a *really hard* idea - and not all of us are genius such as Wadler or S. P. Jones, lol!

The thing is: we need impurity. Real world and systems programming *is* about impurity. Programming is based on it: we need to change the current world and create a new one - say, add a line to a file. Maybe send a message to our moms across the web. If we do not have that, we would only be able to make some mathematical calculations! 

But the thing is: whenever we are able to, we should use pure functions. The reason is predictability and readability: we know what is happening in every part of the program if we read it carefully. 

## IO (those print statements).

So, let's start by thinkng of IO as an impure function. 

What I mean by that is: you know that ```System.out.println``` in your Java code, or maybe ```Console.WriteLine``` in your C# code? Maybe even the [unsafe printf function](https://github.com/conilas/pltheory-notes/blob/master/depedent-types.MD) from C. Those functions perform IO operations: they print some value on the screen, mutating the external world. 

So, there appears to be nothing we could do to run away from this disaster. The only possiblity is to cry in your bath, knowing that no one loves you. The problem is: we are mutating the external world, but how, if it is not ours (we do not receive it) to mutate? 

## What if

Think now that we have defined the world. The world is a value represented like ```word: Word```, and we can pass it along to every function in the world (pun intended... I guess).

So, the function that was defined as: 

```java
void println(String ... args)
```

Is now:

```java
public World println(Word world, String ... arg)
```

Which means that we will receive the current world as an argument, transform is somehow and then return that transformed value so we can use it for more operations! 

This means we had some function messing around with things that did not belong to it, mutating the world. Now it receives the current world and does not mutate it; instead, it creates a new world from the old one, and that new world is a world where ~~we do not want to die and are not miserable cucks~~ an IO was done!

## One explanation of haskell: IO monads

So we know have a world being passed around and recreated from function to function. Instead of passing the whole world, for instance, we could wrap them into something that only performs IO, and we could then use some functions to perform enumerous operations only related to IO. 

That's one way to explain haskells idea: we have an IO of a type that wraps the whole world and can only perform IO operations (such as ```getChar``` or ```putChar```). Here's the definition:

```hs
type IO a = World -> (a, World)
```

Therefore, the ```IO a``` type just wraps an input of type World and an output of type (a, World) so we don't have to write out that we're passing around the whole world. Cool idea, huh?

From that point on, we can pipe many operations of IO and in the end lift the result. This means we took an impure function, wrapped it up into a monad and got a pure function that has the whole world to operate on, but only exposes the IO parts! 

For example, this is an ```echo``` function: it reads a character and then prints it on the screen. The example also defines the ```getChar``` and ```putChar``` types, so we can understand the whole idea:

```hs
getChar :: IO Char
putChar :: Char -> IO ()

echo :: IO ()
echo = getChar >>= putChar
```

I will not extend myself in monads because there is already another article about it (what? you didn't see? so go check it out! =D). But the idea is rather simple: think of it like a container that has your value, and you can map that value applying some function (that also receives the same type) and lift it. That's basically it.. or not (go read the article!). 

## References

[1] https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/mark.pdf

[2] http://skiplang.com/

[3] https://github.com/conilas/pltheory-notes/blob/master/depedent-types.MD
