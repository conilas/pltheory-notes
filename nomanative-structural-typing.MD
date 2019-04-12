This posts idea is to enlighten the concepts of nominative vs structural typing, give some examples, explain the idea and throw in a third idea.

In order to explain what any of those concepts are, we need to understand how a type system works. As explained in [1], type systems must, in general, answer some questions about the relations between types. Examples of those relations are:

* *Subtype-of*: The ```subtype-of``` relation is generally expressed as ```A<:B```, where A and B are both types belonging to the type system. The idea is to express that a type A is a subtype of B, i.e meaning A can be used when some action expects the B type. 

* *Equal-type*: The ```equal-type``` relation is expressed in the form ```A=B```. In this case, A and B are also types that belong to the type-system, but the relation says that they are equivalent: the type A can be interchangeably used when some actions expects type B and vice-versa.

So now that we have established that these rules must be answered when two types A and B are provided, the question that this piece tries to answer is ```how``` to do it. How do we know a type A is a subtype of B and how do we know the type A is equivalent to the type B?

There are main roads that will answer that. One of them is ```nominal typing``` and the other is ```strcutural typing```. 

## Nominative (sub)typing

The idea behind nominal typing is to express the answer of the questions by checking the ```name``` of each type. This is the most simple way to do so, and does not necessarily requires some fancy algorithm to type check. 

> C++, Java, C, Fortran and Crystal, for example, all follow this approach - which is why most people are already used to it. 

Let's pretend ```type A``` is a name (which means I'd be able to do something like ```let x = type A```) and the same goes for type B ```let y = type B```. As for type A and type B, I'm going to say they simply point to the same value: they are both strings. So, the code that represents all this follows:

```
let type A = String
let type B = String

let x = type A
let y = type B
```
[Code 1]

Imagine we have a function defined like ```let fn (arg is type A) => print arg```. In this simple case, it is easy to see that we could do something like ```fn x```: the ```fn``` function expects something of type A, and we do know that ```Γ ⊢ x:type A```! 

Now, what if we tried ```fn y```? In a **nominative typed** programming language, the type checker would throw an error, saying it expects something of type A, but received a type B. This means that even though they have the same structure (by structure in this case I mean they are both String), they cannot be used interchangeably (read: ```A=B``` is a false statement). 

This looks a bit hard to swallow, but most mainstream languages follow this idea. As for subtyping, we have the exact same idea: if we do not extend it by name, we are not the same type. I'll throw an example with Java, because nominal typing can  be hard on it.

```java

interface Named {
  public String getName()
}

class Animal {
  
  final String name;
  
  public String getName() {
    return this.name;
  }
  
  //getters, setters and constructors
}

class Main {
  
  public void printName(Named val) {
    System.out.println(val);
  }

  public static void main(String ...args){
    //...
  }
}
```

Is the ```Animal``` named? Because, if you are used to Java, you do know that we cannot call the ```printName``` function passing Animal because we did not state it implements the named interface. One way you could do it is to extend the Animal class and say that extension implements the named interface, but the point is: the compiler is only checked for the **name** of the type, not for its **structure**!

## Structural (sub)typing

After understanding that a nominative type system expects the programmer to name the type relations, it is clear to see that a structural (sub)typing system allows one to use two values that have the same structure without harm. This means that [Code 1] would totally be valid with ```fn y```, because they both have the exact same structure. 

I think todays canonical example could be Golang ```structural subtyping```. The following code is valid in the language:

```go
//... boilerplate package & imports

type  Named  interface{
    GetName() string
}

type Animal struct {
  name string
}

type Person struct {
  name string
}

func (a Animal) GetName() string {
  return a.name
}

func (a Person) GetName() string {
  return a.name
}

func printName(a Named) {
  fmt.Printf(a.GetName())
}

func main() {
  printName(Animal{name: "doggy doo"})
  printName(Person{name: "human doo"})
}
```

The ```Animal``` and ```Person``` structs did not say they implemented the ```Named``` interface. Because of the fact that they implement the function ```GetName```, they are already inferred to be of that type. 

## The best of both worlds

We have some examples of programming languages that implement both of the ideas. The general idea is to create different keywords for each of those aproaches.

* Modula-3: The modula-3 lang uses the ```BRANDED``` keyword in order to define a type that will be nominaly typed only - i.e other types don't have structural equivalence with it, and vice-versa.

* Ponylang: The ponylang approach is for subtyping rules. Pony's type system present the two types of subtyping - whenever an ```interface``` is created, every class that follows the contract described by it is considered a subtype of it (structural subtyping). When you instead define a ```trait```, the subtyping is explicit: the class must say it is of that type (using the keyword ```is```). 

## In short

* You can implement type and subtype equivalence explicitly - that's the nominative way;
* You can implement it in an implicit way - that's the structural way;
* You can have both of them - but how to implement that is up to you.

# References

[1] - http://lucacardelli.name/papers/typesystems.pdf
