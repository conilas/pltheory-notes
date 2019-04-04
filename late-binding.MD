Okay, so it seems like late-binding is a big deal. Late binding was firstly introduced in 1969 (http://homepages.cs.ncl.ac.uk/brian.randell/NATO/nato1969.PDF) when talking *shit* about Lisp.

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
    return GreetOthers();
  }
  
  public static void Main(String ...args){
    Boolean which = <...> //some user input
    
    getIt(which).greet(); //How would the compiler store this in a table?
  }
}
```

This code example represents the idea of dynamic dispatch/some way of late binding in Java. You can see that the compiler does not have enough information to know which method will be called from which of the classes. It means that the value will be looked up at *runtime* -- the interpreter will have to check it!

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
