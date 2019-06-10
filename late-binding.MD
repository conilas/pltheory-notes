Okay, so it seems like late-binding is a big deal. Late binding was firstly introduced in 1969[4] when talking *shit* about Lisp.

The idea is to keep all the functions to be binded as late as possible. Let's say I've defined a method that calls ```z()```, but Z was not yet defined anywhere. The compiler should not complain: I may define this method at runtime, which means it cannot try to bind the value Z at compile time because it is really not yet defined. 

# Dynamic dispatch

And the same goes for objects on functions that have [ad-hoc polymorphism](polymorphism.MD) (aka Overloading) and [subtype polymorphism](polymorphism.MD#subtype). The idea is what C++ and C# use as ```virtual```. Let's say I have some specializations of an object and then I call a function that returns one of those specializations, like such:

```Java
abstract class Test{
  public abstract void greet();
}

class GreetSelf extends Test {
  public void greet() { System.out.println("Greeting test!"); }
}

class GreetOthers extends Test {
  public void greet() { System.out.println("Greeting other!"); }
}

class Main { 
  public static Test getIt(boolean which){
    if (which) return new GreetSelf();
    return new GreetOthers();
  }
  
  public static void main(String ...args){
    Boolean which = <...>; //some user input
    
    Test t = getIt(which);

    t.greet(); //How would the compiler store this in a table?
  }
}
```

This code example represents the idea of dynamic dispatch/some way of late binding in Java. You can see that the compiler does not have enough information to know which method will be called from which of the classes. It means that the value will be looked up at *runtime* -- the interpreter/runtime will have to check it!

# Multiple vs Single dispatch

There's an addon here. Dynamic dispatch can be described as above, but in Java and C# we have what is called Single Dynamic Dispatch, or just Single Dispatch (the term dispatch is most commonly used when talking about the definition of methods dynamically, so we can just say it like that). 

The idea of Single Dispatch is to base the dynamicity only on one argument: the object who will receive the message. If you go back and look at the example, it is easy to see that the only part that has to be checked at runtime is the caller object - the arguments passed through that message do not vary. What if we had that same polymorphic idea, but also having to define the arguments types at runtime? 

Let's extend our example. We will now have overloaded methods defined in our objects, and this polymorphism will still be based on the types we are working with (GreetSelf, GreeOther and Greet).

```java
abstract class Test{
  public abstract void greet();
  public abstract void greet(Test self);
  public abstract void greet(GreetSelf self);
  public abstract void greet(GreetOthers self);
}

class GreetSelf extends Test {
  public void greet() { System.out.println("Greeting self !"); }

  public void greet(Test self) { System.out.println("Greeting self with test..."); }
  public void greet(GreetSelf self) { System.out.println("Greeting self with self "); }
  public void greet(GreetOthers others) { System.out.println("Greeting self with others"); }
}

class GreetOthers extends Test {
  public void greet() { System.out.println("Greeting others !"); }

    public void greet(Test self) { System.out.println("Greeting other with test..."); }
  public void greet(GreetSelf self) { System.out.println("Greeting other with self "); }
  public void greet(GreetOthers others) { System.out.println("Greeting other with others"); }
}

class Main { 
  public static Test getIt(boolean which){
    if (which) return new GreetSelf();
    return new GreetOthers();
  }
  
  public static void main(String ...args){
    Boolean which = false; 
    
    Test t = getIt(which);

    t.greet(); //How would the compiler store this in a table?
    t.greet(t); 
  }
}
```

**Note**: The user input was removed in this case so that it easier to see which instance we have.

And our output:

```
Greeting others !
Greeting other with test... 
```

*Can you see the magnitude of this?*. We have called both times methods from ```GreetOthers``` just as expected. But in the second call, although the compiler *knew* the type of the argument - which we can see because it called the correct implementation of the methods-, it was not able to pick the right implementation of the function when that dependend on the arguments. In this case, this is mostly due the fact that Java implements ad-hoc polymorphism (overloading, in Javish lango) at compile-time, which doesn't *at all* feels like late binding!

Real dynamic languages are actually able to do that. CLOS implements multimethods, as does Dylan. 

If you want, you can read on the idea of multiple dispatch and the relation of it with late binding in [2]. Funny thing is: it led to the formalization of covariance vs contravariance issue in object oriented languages[3], which is the subject of another post in this blog!

# Real late binding

And this is a dummy example for Java. Let's say now that we are playing with javascript:

---

**DISCLAIMER**: This is not good code. It is a simple example of late binding. DO NOT TRY THIS AT WORK!

***

```javascript
const double = (a) => a * 2

const redefine = (bool) => {
  if (bool && !Array.prototype.doubleAll)
    Array.prototype.doubleAll = function() {
      return this.map(double)
    }
  }
}

const main = (array) => { 
  const shouldRedefine = array.length > 0
  
  redefine(shouldRedefine)
  
  console.log(array.doubleAll()); //what will happen?
}
```

How would the compiler know what will happen? I'm extending the array prototype just in case the array is bigger than 0! This means calling ```main([])``` will not extend the prototype - it will not define a method for the object at runtime - which will make the interpreter throw the error ```array.doubleAll is not a function```, but calling it like ```main([1,2])``` would output ```[2,4]```. 

This means that I've altered the shape of the object at runtime. A dummy compiler would never know that - it would look if the function is defined in the array object, see that it is not ant then throw an error. Imagine using reflection in Java to create a method for an object and calling it directly. Java's compiler will NOT be able to understand it and won't produce the bytecode. 

This show the real difference of dynamic dispatch in java fashion, that tries to emulate some late binding but in a different way, and a real late binding in another language.

# Some history

The funny thing is - Alan kay, [who coined the term OOP](http://wiki.c2.com/?HeInventedTheTerm), said himself that OOP was about "messaging, local retention, and protection and hiding of state-process, and **extreme late-binding of all things.** It can be done in Smalltalk and in LISP"[1]. 

You can make your own conclusions from that.

# References

[1] - www.purl.org/stefan_ram/pub/doc_kay_oop_en

[2] - https://dl.acm.org/citation.cfm?id=141537

[3] - https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.115.5992

[4] - http://homepages.cs.ncl.ac.uk/brian.randell/NATO/nato1969.PDF
