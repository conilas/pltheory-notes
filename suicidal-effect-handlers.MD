The programming language world is constantly evolving - and so is the research on this field, always following an inclination to the mathematical and theoretical part of it. The latest published papers on the ACM are almost always related to type theory - HoTT, formal verification and so on. One of these aspects is algebraic effect handlers, which define ways to handle an effect whenever it occurs - and when *you* want to - in your program.

## The idea 

The idea for algebraic effect handlers is simple: whenever I found an effect of some type, I want to handle it on my way. If you are familiar to CLU or something newer, such as Java, you've already been through something like it - exceptions, for instance, can be seen as *computational effects* of a program[1]. 

I could, and I wished to, go on about effects - maybe talk about monads and side effects and why are effects can be combined more smoothly than monads do [1], maybe talk about the program structure that follows with *effect signatures*, *effectfull functions* and the *effect handlers*[2].  But the papers listed here already give a great overview onto those ideas. Instead, I want to show an example of how they work - and, user-wise, how *easy* they can be in simple cases.

## What we need

We are going to need a running installation of Koka-lang, a language which contains effect handlers (and a lot more, indeed; check the language out in https://koka-lang.github.io/koka/doc/kokaspec.html#sec-an-overview-of-koka). That should be enough to get you going.

> SELF NOTE: this should not be a list

I won't waste your time by going through the syntax; if you are already familiarized with the C-syntax, you should be good to go. If you aren't, then:

* You might only program in decent functional languages, which means you probably know more than I do. So, please, GTFO here;
* You might be a beginner which is here only for curiosity. Keep on reading and you will probably understand what is going on;

## What we are going to do

And now, for the idea. I'll show how to handle an IO of a function which, whenever called, will print a way for you to kill yourself.

In order to start with it, open up a new file with the extension ```.kk``` (the koka extension) and let's start by writing our effect.

```koka
effect suicidal{
  fun print_me(s:string) : ()
}
```

Okay, so here we are doing the following: creating an effect called suicidal which define which function belongs to it - in this case, ```print_me``` function. 

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

> SELF NOTE: expand more stuff here

Okay, so this is new. Our function has the signature ```fun randomSuicideWay() : <console, ndet> ()```, which means:

* The function name is randomSuicideWay, which receive no args (duh);
* *The function has two side effects: ```console``` and ```ndet```*
* The function returns the type unit (or, if you want to call it, void);

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
