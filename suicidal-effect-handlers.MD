The programming language world is constantly evolving - and so is the research on this field, always following an inclination to the mathematical and theoretical part of it. The latest published papers on the ACM are almost always related to type theory - HoTT, formal verification and so on. One of these aspects is algebraic effect handlers, which define ways to handle an effect whenever it occurs - and when/how *you* want to - in your program.

## The idea 

The idea for algebraic effect handlers is simple: whenever some computation occurs, write down a handler to handle it in a specific way. By computation, I mean any side effect of your program: non-determinism, input/output, mutable state, and so on. Basically, any functions of yours that doesn't simply compute a new value using only the direct inputs and returns it is causing a side effect. You see - any function that is not pure!

If you are familiar to CLU or something newer, such as Java, you've already been through something like it - exceptions, for instance, can be seen as *computational effects* of a program[1]. 

But languages and the idea has evolved. You can now define new effects for your programs - you can declare what is an effect and how it can be handled. For instance, one can declare that the calling of a function *is* an algebraic effect, and handle that effect in any way it is needed.

It is a simple concept to use - the idea of handling when and how you want a side effect is pretty pleasant and usefull. Of course, as many *compsci* concepts, the math (and theoretical) part of it is not that easy to explain. 

If one understand the idea behind monads - no, not *just monoids in the category of endofunctors* - and why they can be used to handle side effects[4], then it will be easy to see why algebraic effect handlers can be used to achieve similar goals (and also why they can be even better for that matter[1]). And if you don't, well, [it is never too late to learn something new :-)](https://conilas.github.io/purity-and-the-real-world/). 

**Note**: I'm not talking about monads as a whole, just the idea of using them to handle side effects - or computations, if you will - in the program.

But if you're reading this, you probably don't really get monads. Neither why they are used for that purpose. To be honest, you don't need to understand them, as this article's idea is to show you how to use effect handlers and why they can be usefull. Let's begin, shall we?

## What we need

We are going to need a running installation of Koka-lang, a language which contains effect handlers (and a lot more, indeed; check the language out in https://koka-lang.github.io/koka/doc/kokaspec.html#sec-an-overview-of-koka). That should be enough to get you going.

I won't waste your time by going through the syntax; if you are already familiarized with the C-syntax, you should be good to go. If you aren't, then maybe you are a beginner which is here only for curiosity. Keep on reading and you will probably understand what is going on. Otherwise, if you are not used to the C-syntax because you've been working with decent functional languages, you are probably smarter than the writer and should GTFO here and go read them papers, lol.

## What we are going to do

And now, for the idea. I'll show how to handle a call of a function which, whenever called, will print a way for you to kill yourself.

In order to start with it, open up a new file with the extension ```.kk``` (the koka extension) and let's start by writing our effect.

```koka
effect suicidal{
  fun print_me(s:string) : ()
}
```

Okay, so here we are doing the following: creating an effect called suicidal which define which function belongs to it - in this case, the ```print_me``` function. 

This means that the print_me function will need a handler to handle such effect - oh! so we now need to declare our pretty handler. But, as I told you before, we need this handler to call a function that will output a way for you to kill yourself - randomically - everytime the effect happens. So let's first write down that function.

```koka
fun randomSuicideWay() : <console, ndet> (){
  val x = random-int()

  if (x < 3000000000000000){
    return println("Kill youself: by putting 5 years of your life programming in Java")
  }

  if (x > 3000000000000000 && x < 5000000000000000) {
    return println("Kill youself: with a spoon")
  }

  if (x > 7000000000000000 && x < 9000000000000000) {
    return println("Kill youself: with your cellphone so far up in your butt you can't even breath")
  }

  println("Kill youself: with another knife :-)")
}
```

> Please ignore the shitty code. It is only here because this is also a shitty tutorial.

Okay, so this is new. Our function has the signature ```fun randomSuicideWay() : <console, ndet> ()```, which means:

* The function name is randomSuicideWay, which receive no args (duh);
* *The function has two side effects: ```console``` and ```ndet```*
* The function returns the type unit (or, if you will, void);

The side effects annotated are where I'm trying to get. This actually means that the function that will handle our side effect (```suicidal```) also have two side effects - one for the console, which means it will perform IO, and one for ndet, which is short for non-determinism that happens when we call the random-int function. 

This means we could also have handlers for those. And this also means that we have to tell the compiler we are going to have those side effects in our code (except those two are a special case that you can read in the book) - at the end, we need to handle all side effects we wish to, meaning we can control all the cases in order to avoid side-effect functions! That's the beauty of it.

Maybe even after that you didn't understand what I mean, because I suck at explaining stuff. But let's keep going. Now that we have our function to be executed, we must create our handler to actually perform it!

```koka
val suicidal_handler = handler {
  return   x -> [x]
  print_me(s:string) -> {
    println(s)
    resume(randomWay())
  }
}
```

Okay, so let's see if we are complete now. We created out effect, named ```suicidal```, which wrapts the function named ```print_me```. We've also created the function to print a random way to die and a handler to call it out. 

Okay, so... let's try to trigger our function now:

```koka
fun extensive_calculation(x1:int, x2:int) : <console, suicidal> (){
  print_me("hey, guys! Our value is: ")
  println(x1+x2)
}

fun main() {
  suicidal_handler {
    extensive_calculation(5, 10)
    extensive_calculation(10, 4)
    extensive_calculation(20, 30)
  }
}
```

So, as you see, we are performing some extensive calculations with x1 and x2. We then print the result of our calculation with a happy message, but we also warn the compiler that this has the suicidal effect, and that we are handling it with the suicidal handler.

Running the code, we get...

```koka
hey, guys! Our value is: 
Kill youself: with your cellphone so far up in your butt you can't even breath
15
hey, guys! Our value is: 
Kill youself: by putting 5 years of your life programming in Java
14
hey, guys! Our value is: 
Kill youself: with your cellphone so far up in your butt you can't even breath
50
```

Which way will you choose? :-)

**NOTE**: The whole code can be found at https://github.com/conilas/kill-yourself-with-effects/tree/master

## References

[1] - http://math.andrej.com/wp-content/uploads/2012/03/eff.pdf

[2] - http://delivery.acm.org/10.1145/3280000/3276481/oopsla18main-p148-p.pdf?ip=200.169.33.63&id=3276481&acc=OAkey=4D4702B0C3E38B35%2E4D4702B0C3E38B35%2E4D4702B0C3E38B35%2E6D218144511F3437&__acm__=1557515854_f146e87c7d305676676fe7a09e471f7f

[3] - https://arxiv.org/pdf/1312.1399.pdf

[4] - The awkward squad - https://www.microsoft.com/en-us/research/wp-content/uploads/2016/07/mark.pdf
