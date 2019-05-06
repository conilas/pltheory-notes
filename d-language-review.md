So as for today (6 of may / 2019), the new GCC version is being released. It will have enhancements to the FORTRAN compiler, implement the C2X and C++2A language specs and the D language. To be honest, I had already checked out D only to find one or two things interesting; but with this new support I've read the entire language spec and found some more interesting stuff. And that's what this is about (wish I could talk about a GCC x LLVM war or C++ concepts coming - oorray! - but nah, not today).

## Some history

Well, what's the deal with letter-named languages? I mean, we all know that historically we had CPL, then BCPL which led to B, C and P(ascal). But all those languages were following the BCPL name - why do people still working that kind of name? -- not talking here about E, which is a decent language and the ML-Family F# and F* because they follow a different path.

But oh - this one does have something to do with C++. To give a short something: did you know that around 1980 the first C++ was called C with classes and was mostly syntatic sugar that compiled to C? And even in 1982~84, when Stroustrop (C++ father) came with the spec (and C84, the first actual compiler), the most known compilers were still compiling to C? [2]

Actually, the core designer of D, Walter Bright, owned a company that was responsible for one of the first C++ direct compiler - and actually the first comercial C++ compiler for windows [1], named Zortech C++ -- *cries in free software language*. To be fair, this is such an important deal that Stroustrop cites him in the *history of c++* paper.

So yeah, now you see where this is going. Walter Bright started developing D around 1999, which is when the version 0.00 of the language was released [3]. He (probably) felt like he could do a different work from C++ - possibly using automatic memory management and some other stuff -- so now, let's take a look at the features that comes to attention.

## On memory

One of the great differences from D to C++ (and C, if you will) is that D has both manual memory management and an automatic one, and it does recommend the use of the garbage collection because us programmers are dumb. The garbage collection of D is not a generational one, but as far as I could find it, a simple mark and sweep one. Don't worry - I'll make another post about GC in the future. For now, you only have to understand that this is not the best possible algorithm for a GC. If you want some more information about D's GC, read [4] and [5].

## On program correctness

D was heavily inspired by Eiffel[6], which, around the 80's, was the first language to bring the idea of design by contracts. Design by contracts is a way to enforce program correcteness heavily related to Hoare's logic & triples with {pre}code{post}. This is a great deal when one wishes to achieve what is wanted and IMO can also be seen as a form of AOP -- sorta.

A simple dBc program in D could be written as:

```D
int fun(int a, int b)
in (a > 0)
in (b >= 0, "b cannot be negative!")
out (r; r > 0, "return must be positive")
{
    int r = a/b;
    return r;
}


void main()
{
    writeln("hey d");
    fun(10,2);
    fun(-1,2); //assertion error
}
```

Which, as you see: makes sure that A and B are greater than 0 and that the return is also greater than 0. This, for example, is a way to ensure that the division is safe - i.e it is not dividing by 0 (and it is also a positive division in this case). 

D also has invariants to control state, such as:

```D
class Date
{
    int day;
    int hour;

    this(int d, int h)
    {
        day = d;
        hour = h;
    }

    invariant
    {
        assert(1 <= day && day <= 31);
        assert(0 <= hour && hour < 24, "hour out of bounds");
    }
}
```

Which makes *sure*, in any state update, that the hour & day are in a specified range, else it will throw an error. Cool, huh?


> Well, it is a fact that D's contracts are a runtime assertion and this is a little different than dependent/refined typed languages, which are mostly checked at compile time by constraint solving and whatnot. But this may be another post's concer - for now, have fun with the D :-)

## On parametric polymorphism (generics) & metaprogramming

talk about how D supports bounded template programming - even better than cpp
talk about template metaprogramming

## On concurrency/paralelism

talk about Actor model, std.concurrency and std.paralelism stuff.

## On error handling

talk about the exception models and stuff

## Other stuff

operator overloading, maybe talk about some of the OO model w/ interfaces, template mixins as a block of code, maybe union types, default arguments, property functions, the stringof property that does not evaluate the expression, Parameter Storage Classes (lazy bla bla), postfix calls (syntatic sugar to call {type}.fn when having fn(type)), unit tests inside classes, Alias This, maybe say that having an ABI is good.

## Funny stuff

Voldemort types :-)

## What's bad in the D (pun intended lol)

Talk about bad names for stuff - mixins? traits? all wrong! 
It has the null type and lacks an optional monad for the default mechanism to avoid problem with null pointers

## What this was not about

not wasting time on basic stuff (inheritance, it has a simple type system and bla bla);
not wasting time on environment (the DUB and blabla)

## References

[1] - http://www.softwarepreservation.org/projects/c_plus_plus/

[2] - Stroustrup, Bjarne. "A history of C++: 1979--1991." History of programming languages---II. ACM, 1996. - http://www.stroustrup.com/hopl2.pdf

[3] - https://wiki.dlang.org/Language_History_and_Future

[4] - https://olshansky.me/gc/runtime/dlang/2017/06/14/inside-d-gc.html

[5] - https://dlang.org/spec/garbage.html

[6] - https://www.eiffel.com/resources/faqs/eiffel-language/

[7] - Bracha, Gilad, and William Cook. "Mixin-based inheritance." ACM Sigplan Notices 25.10 (1990): 303-311. - http://www.bracha.org/oopsla90.pdf

[8] - https://link.springer.com/chapter/10.1007/978-3-540-45070-2_12
